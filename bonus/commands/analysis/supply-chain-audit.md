# /supply-chain-audit

Audite um projeto quanto a riscos de supply chain de software, incluindo vulnerabilidades de dependências, problemas em lockfiles, indicadores de pacotes maliciosos, cobertura de SBOM e preocupações de licenças.

## Propósito

Use este comando para realizar uma revisão de segurança de supply chain focada em uma base de código. Ele ajuda a identificar riscos relacionados a dependências em projetos JavaScript, Python, Go, Rust, Java, Ruby e baseados em container.

O comando revisa dependências diretas e transitivas, destaca os achados mais importantes primeiro e recomenda etapas de remediação exatas quando possível.

## Uso

```
/supply-chain-audit
/supply-chain-audit npm
/supply-chain-audit python
/supply-chain-audit docker
/supply-chain-audit ./services/api
/supply-chain-audit --report
/supply-chain-audit --sbom
/supply-chain-audit --licenses
```

## Implementação

Quando este comando for executado, Claude deve:

1. Detectar o ecossistema verificando package.json, requirements.txt, go.mod, Cargo.toml, pom.xml, Gemfile ou Dockerfile.

2. Inventariar todas as dependências: diretas, transitivas, dev, build-time e imagens base.

3. Avaliar risco de supply chain em:
   - CVEs conhecidas e advisories
   - Versões não fixadas ou flutuantes
   - Lockfiles ausentes ou desatualizados
   - Scripts de instalação que executam código
   - Typosquatting e nomenclatura suspeita
   - Vetores de dependency confusion
   - Workflow de SBOM ausente
   - Incompatibilidades de licença
   - Provenance de CI/CD fraca

4. Apresentar achados usando níveis de severidade:
   - CRÍTICO
   - ALTO
   - MÉDIO
   - BAIXO

5. Para cada achado incluir: o que foi detectado, por que importa, como verificar e o comando exato de remediação.

6. Adequar remediação ao ecossistema:
   - npm: npm audit, verificação de lockfile, scoping .npmrc, --save-exact
   - Python: pip-audit, cyclonedx-py, verificação de lockfile
   - Go: govulncheck, go mod verify
   - Rust: cargo audit, cargo deny
   - Java: dependency-check, Snyk, plugin OWASP
   - Ruby: bundler-audit
   - Docker: Syft, Grype, Trivy, pinning de digest

7. Encerrar com um plano de ação: Corrigir agora / Corrigir este sprint / Monitorar / Legal ter

## Exemplos

Usuário: /supply-chain-audit
Claude detecta o gerenciador de pacotes, verifica lockfiles, sinaliza versões flutuantes, escaneia hooks de instalação e produz um relatório classificado com comandos de remediação.

Usuário: /supply-chain-audit --sbom
Claude verifica se um SBOM existe e recomenda syft ou cdxgen para gerar um em formato CycloneDX ou SPDX, depois explica como anexá-lo aos artefatos de CI.

Usuário: /supply-chain-audit ./services/api
Claude limita a análise a esse diretório, detecta o gerenciador de pacotes local e produz achados apenas para esse serviço.