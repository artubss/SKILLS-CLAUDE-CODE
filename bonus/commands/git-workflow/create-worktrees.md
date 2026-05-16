# Comandos de Git Worktree

## Criar Worktrees para Todos os PRs Abertos

Este comando busca todos os pull requests abertos usando o GitHub CLI, depois cria um git worktree para a branch de cada PR no diretório `./tree/<BRANCH_NAME>`.

```bash
# Certifique-se de que o GitHub CLI está instalado e autenticado
gh auth status || (echo "Please run 'gh auth login' first" && exit 1)

# Criar o diretório tree se não existir
mkdir -p ./tree

# Listar todos os PRs abertos e criar worktrees para cada branch
gh pr list --json headRefName --jq '.[].headRefName' | while read branch; do
  # Lidar com nomes de branch com barras (como "feature/foo")
  branch_path="./tree/${branch}"
  
  # Para branches com barras, criar a estrutura de diretório
  if [[ "$branch" == */* ]]; then
    dir_path=$(dirname "$branch_path")
    mkdir -p "$dir_path"
  fi

  # Verificar se o worktree já existe
  if [ ! -d "$branch_path" ]; then
    echo "Creating worktree for $branch"
    git worktree add "$branch_path" "$branch"
  else
    echo "Worktree for $branch already exists"
  fi
done

# Exibir todos os worktrees criados
echo "\nWorktree list:"
git worktree list
```

### Exemplo de Saída

```
Creating worktree for fix-bug-123
HEAD is now at a1b2c3d Fix bug 123
Creating worktree for feature/new-feature
HEAD is now at e4f5g6h Add new feature
Worktree for documentation-update already exists

Worktree list:
/path/to/repo                      abc1234 [main]
/path/to/repo/tree/fix-bug-123     a1b2c3d [fix-bug-123]
/path/to/repo/tree/feature/new-feature e4f5g6h [feature/new-feature]
/path/to/repo/tree/documentation-update d5e6f7g [documentation-update]
```

### Limpar Worktrees Obsoletos (Opcional)

Você pode adicionar isso para remover worktrees obsoletos para branches que não existem mais:

```bash
# Obter branches atuais
current_branches=$(git branch -a | grep -v HEAD | grep -v main | sed 's/^[ *]*//' | sed 's|remotes/origin/||' | sort | uniq)

# Obter worktrees existentes (excluindo o worktree main)
worktree_paths=$(git worktree list | tail -n +2 | awk '{print $1}')

for path in $worktree_paths; do
  # Extrair o nome da branch do caminho
  branch_name=$(basename "$path")
  
  # Pular casos especiais
  if [[ "$branch_name" == "main" ]]; then
    continue
  fi
  
  # Verificar se a branch ainda existe
  if ! echo "$current_branches" | grep -q "^$branch_name$"; then
    echo "Removing stale worktree for deleted branch: $branch_name"
    git worktree remove --force "$path"
  fi
done
```

## Criar Nova Branch e Worktree

Este comando interativo cria uma nova branch git e configura um worktree para ela:

```bash
#!/bin/bash

# Certifique-se de que estamos em um repositório git
if ! git rev-parse --is-inside-work-tree > /dev/null 2>&1; then
  echo "Error: Not in a git repository"
  exit 1
fi

# Obter a raiz do repositório
repo_root=$(git rev-parse --show-toplevel)

# Solicitar o nome da branch
read -p "Enter new branch name: " branch_name

# Validar nome da branch (validação básica)
if [[ -z "$branch_name" ]]; then
  echo "Error: Branch name cannot be empty"
  exit 1
fi

if git show-ref --verify --quiet "refs/heads/$branch_name"; then
  echo "Warning: Branch '$branch_name' already exists"
  read -p "Do you want to use the existing branch? (y/n): " use_existing
  if [[ "$use_existing" != "y" ]]; then
    exit 1
  fi
fi

# Criar diretório da branch
branch_path="$repo_root/tree/$branch_name"

# Lidar com nomes de branch com barras (como "feature/foo")
if [[ "$branch_name" == */* ]]; then
  dir_path=$(dirname "$branch_path")
  mkdir -p "$dir_path"
fi

# Garantir que o diretório pai existe
mkdir -p "$(dirname "$branch_path")"

# Verificar se um worktree já existe
if [ -d "$branch_path" ]; then
  echo "Error: Worktree directory already exists: $branch_path"
  exit 1
fi

# Criar branch e worktree
if git show-ref --verify --quiet "refs/heads/$branch_name"; then
  # A branch existe, criar o worktree
  echo "Creating worktree for existing branch '$branch_name'..."
  git worktree add "$branch_path" "$branch_name"
else
  # Criar nova branch e worktree
  echo "Creating new branch '$branch_name' and worktree..."
  git worktree add -b "$branch_name" "$branch_path"
fi

echo "Success! New worktree created at: $branch_path"
echo "To start working on this branch, run: cd $branch_path"
```

### Exemplo de Uso

```
$ ./create-branch-worktree.sh
Enter new branch name: feature/user-authentication
Creating new branch 'feature/user-authentication' and worktree...
Preparing worktree (creating new branch 'feature/user-authentication')
HEAD is now at abc1234 Previous commit message
Success! New worktree created at: /path/to/repo/tree/feature/user-authentication
To start working on this branch, run: cd /path/to/repo/tree/feature/user-authentication
```

### Criar uma Nova Branch a partir de uma Base Diferente

Se você quiser iniciar sua branch a partir de uma base diferente (não do HEAD atual), você pode modificar o script:

```bash
read -p "Enter new branch name: " branch_name
read -p "Enter base branch/commit (default: HEAD): " base_commit
base_commit=${base_commit:-HEAD}

# Depois use a base especificada ao criar o worktree
git worktree add -b "$branch_name" "$branch_path" "$base_commit"
```

Isso permitirá que você especifique qualquer commit, tag ou nome de branch como ponto de partida para sua nova branch.