<!-- ============================================================ -->
<!-- HEADER · animated banner + typing tagline                    -->
<!-- ============================================================ -->

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=12,2,6,20,30&height=200&section=header&text=Claude%20Skills%20Hub&fontSize=70&fontColor=ffffff&animation=fadeIn&fontAlignY=42" alt="Claude Skills Hub" width="100%" />

### The largest curated collection of Claude Code skills, agents, hooks &amp; MCPs

<a href="https://github.com/artubss/SKILLS-CLAUDE-CODE">
  <img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=20&duration=3000&pause=900&color=7C3AED&center=true&vCenter=true&width=820&lines=1%2C044%2B+production-ready+SKILL.md+files;Curated+across+26+categories+%2B+28+agent+packs;Plug+%26+play+with+Claude+Code%2C+Cursor%2C+Codex+%26+more;Auto-triggered+by+natural+language;Open+source.+Battle-tested.+Always+growing." alt="Typing SVG" />
</a>

<br/><br/>

<!-- Badges row · trust + discovery signals -->
<a href="https://github.com/artubss/SKILLS-CLAUDE-CODE/stargazers"><img alt="GitHub stars"   src="https://img.shields.io/github/stars/artubss/SKILLS-CLAUDE-CODE?style=for-the-badge&logo=github&color=FFD700&labelColor=0d1117"></a>
<a href="https://github.com/artubss/SKILLS-CLAUDE-CODE/network/members"><img alt="Forks"     src="https://img.shields.io/github/forks/artubss/SKILLS-CLAUDE-CODE?style=for-the-badge&logo=git&color=00D9FF&labelColor=0d1117"></a>
<a href="https://github.com/artubss/SKILLS-CLAUDE-CODE/issues"><img alt="Issues"           src="https://img.shields.io/github/issues/artubss/SKILLS-CLAUDE-CODE?style=for-the-badge&logo=github&color=FF6B6B&labelColor=0d1117"></a>
<a href="https://github.com/artubss/SKILLS-CLAUDE-CODE/pulls"><img alt="PRs welcome"        src="https://img.shields.io/badge/PRs-welcome-22C55E?style=for-the-badge&logo=git&logoColor=white&labelColor=0d1117"></a>
<a href="./LICENSE"><img alt="License: MIT"                                                 src="https://img.shields.io/badge/license-MIT-7C3AED?style=for-the-badge&logo=opensourceinitiative&logoColor=white&labelColor=0d1117"></a>
<br/>
<img alt="Skills"   src="https://img.shields.io/badge/SKILL.md_files-1%2C044%2B-7C3AED?style=flat-square&logo=anthropic&logoColor=white&labelColor=0d1117">
<img alt="Agents"   src="https://img.shields.io/badge/agents-400%2B-00D9FF?style=flat-square&logo=robotframework&logoColor=white&labelColor=0d1117">
<img alt="Commands" src="https://img.shields.io/badge/commands-341-22C55E?style=flat-square&logo=gnubash&logoColor=white&labelColor=0d1117">
<img alt="MCPs"     src="https://img.shields.io/badge/MCP_servers-13-FF6B6B?style=flat-square&logo=protonvpn&logoColor=white&labelColor=0d1117">
<img alt="Last update" src="https://img.shields.io/github/last-commit/artubss/SKILLS-CLAUDE-CODE?style=flat-square&logo=git&color=orange&labelColor=0d1117">

</div>

<!-- ============================================================ -->
<!-- LANGUAGE SELECTOR · keep markers for readme-i18n skill       -->
<!-- ============================================================ -->

<!-- LANGUAGE-SELECTOR-START -->
<p align="center">
  <strong>English</strong>
  &nbsp;·&nbsp;
  <a href="https://github.com/artubss/SKILLS-CLAUDE-CODE/issues/new?title=Translation+request&labels=i18n">Translate this README</a>
</p>
<!-- LANGUAGE-SELECTOR-END -->

<p align="center">
  <a href="#-quick-start">Quick Start</a> ·
  <a href="#-whats-inside">What's Inside</a> ·
  <a href="#-repository-map">Repo Map</a> ·
  <a href="#-skills-by-category">Skills</a> ·
  <a href="#-agents-roster">Agents</a> ·
  <a href="#-bonus-arsenal">Bonus</a> ·
  <a href="#-how-to-use-with-claude-code">How to use</a> ·
  <a href="#-contributing">Contribute</a> ·
  <a href="#-connect">Connect</a>
</p>

---

## What is this?

**Claude Skills Hub** is a battle-tested, opinionated collection of **1,044+ Claude Code skills, 400+ subagents, 341 slash-commands, 13 MCP servers, 11 hook packs and 31 voice-cloning style guides** — organized into 26 themed folders so you can drop them into any Claude Code, Cursor, Codex or Anthropic-CLI project in seconds. Built for solo founders, indie hackers and senior teams who want **production-grade automations on day one**, not a weekend of glue code.

> **Why it matters.** Most "awesome-claude" lists link out to dozens of repos. This one keeps every artifact **inside the repo**, lockfile-tracked, categorized and trigger-phrase indexed — so the agent finds the right skill on its own.

---

## Quick Start

```bash
git clone https://github.com/artubss/SKILLS-CLAUDE-CODE.git
cd SKILLS-CLAUDE-CODE
```

**Use a skill with Claude Code (just talk to it):**

```text
You: "Refactor this React component and add a design review"
Claude → auto-triggers: skills/skills/development/react-best-practices
                       + plugins/frontend/skills/design-review
```

**Install the Jezweb plugin marketplace (10 plugins, 65 skills, official spec):**

```bash
/plugin marketplace add jezweb/claude-skills
/plugin install cloudflare@jezweb-skills
/plugin install frontend@jezweb-skills
/plugin install dev-tools@jezweb-skills
```

**Run a single plugin locally without installing:**

```bash
claude --plugin-dir ./claude-skills-main/claude-skills-main/plugins/cloudflare
```

**Search every SKILL.md in the repo from your shell:**

```bash
# PowerShell (Windows)
Get-ChildItem -Recurse -Filter "SKILL.md" | Select-String "stripe|paywall|billing"

# Bash / zsh
grep -RIli --include=SKILL.md -e "stripe" -e "paywall" -e "billing" .
```

**Re-sync skills from upstream sources tracked in `skills-lock.json`:**

```bash
# Inspect lockfile (sources + content hashes for every imported skill)
cat skills-lock.json | jq .
```

---

## What's Inside

| Bucket | Count | Path | What's there |
|--------|------:|------|--------------|
| **Skills** | **833** | [`skills/skills/`](./skills/skills) | Largest collection — 26 categories from `development` (216) to `scientific` (139) |
| **Curated Favorites** | **144 SKILL + 20 MD** | [`favorites/`](./favorites) | Hand-picked stack: ai/ml, backend, frontend, fullstack, devops, dominio, situacao templates |
| **Agents** | **400+** | [`agents/agents/`](./agents/agents) | 28 specialist teams: AI experts, expert-advisors (52), programming-languages (50), devops, security |
| **Marketplace Plugins** | **10 plugins / 65 skills** | [`claude-skills-main/claude-skills-main/`](./claude-skills-main/claude-skills-main) | Jezweb's MIT marketplace — Cloudflare, Shopify, WP, frontend, integrations, writing |
| **Slash-Commands** | **341** | [`bonus/commands/`](./bonus/commands) | 24 packs: deployment, git, performance, security, testing, project-management, … |
| **Hooks** | **11 packs** | [`bonus/hooks/`](./bonus/hooks) | Pre/post-tool, quality-gates, security, automation + `HOOK_PATTERNS_COMPRESSED.json` |
| **MCP Servers** | **13** | [`bonus/mcps/`](./bonus/mcps) | Browser automation, devtools, deepresearch, audio, web-data, productivity, marketing |
| **Sandbox Recipes** | **3** | [`bonus/sandbox/`](./bonus/sandbox) | Cloudflare, Docker, E2B isolation patterns |
| **Settings Presets** | **12** | [`bonus/settings/`](./bonus/settings) | API, auth, model, MCP, permissions, statusline, telemetry profiles |
| **Voice Clones** | **31** | [`clones/Clones/slim/`](./clones/Clones/slim) | Naval, Hormozi, MrBeast, Cialdini, Tim Urban, Gary Vee, Eugene Schwartz, Justin Welsh… |
| **Source Skills** | **2** | [`.agents/skills/`](./.agents/skills) | `github` + `readme-i18n` (the two skills used to write **this** README) |

---

## Repository Map

```text
SKILLS-CLAUDE-CODE/
├── .agents/skills/                # Source-of-truth skills (github, readme-i18n)
├── agents/agents/                 # 28 categories · 400+ subagent definitions
│   ├── ai-specialists/            # prompt-engineer, llm-architect, ai-ethics-advisor…
│   ├── expert-advisors/           # 52 specialists for niche advisory work
│   ├── programming-languages/     # 50 language-specific senior engineers
│   ├── devops-infrastructure/     # 39 IaC, k8s, ci/cd, observability roles
│   ├── data-ai/                   # 40 data engineering + ML ops roles
│   └── …                          # security, finance, game-dev, podcast, mcp-dev-team…
├── skills/skills/                 # 833 SKILL.md across 26 themed buckets
│   ├── development/               # 216 — react, python, rust, terraform, playwright…
│   ├── scientific/                # 139 — clinical reports, research grants, LaTeX…
│   ├── ai-research/               # 129 — RAG, agents, evals, fine-tuning…
│   ├── business-marketing/        # 47 · creative-design/ 41 · productivity/ 44
│   ├── security/                  # 42 · enterprise-communication/ 33
│   ├── web-development/           # 29 · workflow-automation/ 22 (incl. n8n pack)
│   └── …                          # database, sentry, railway, pocketbase, sports…
├── favorites/                     # Curated short-list + tpl-* template skills
├── claude-skills-main/            # Jezweb marketplace · MIT · plugin spec
│   └── plugins/                   # cloudflare, frontend, dev-tools, design-assets,
│                                  # integrations, shopify, wordpress, social-media,
│                                  # writing, web-design
├── bonus/
│   ├── commands/                  # 24 packs · 341 slash-commands (.md)
│   ├── hooks/                     # 11 packs + HOOK_PATTERNS_COMPRESSED.json
│   ├── mcps/                      # 13 MCP servers (browser, devtools, audio, web…)
│   ├── sandbox/                   # cloudflare · docker · e2b
│   └── settings/                  # 12 ready-to-merge .claude/settings.json snippets
├── clones/Clones/slim/            # 31 high-signal voice/style guides (.md)
├── skills-lock.json               # Source + content-hash lockfile for every import
└── README.md                      # ← you are here
```

---

## Skills by Category

> 26 buckets · 833 SKILL.md · every entry is a folder with `SKILL.md` (and often `references/`, `scripts/`, `assets/`).

| Category | Count | Highlights |
|----------|------:|-----------|
| [`development`](./skills/skills/development) | **216** | react-best-practices · senior-architect · playwright · postgres-best-practices · stripe-integration · tdd-workflow · vercel-deploy · terraform-specialist |
| [`scientific`](./skills/skills/scientific) | **139** | clinical-reports · latex-posters · research-grants · scientific-schematics · treatment-plans |
| [`ai-research`](./skills/skills/ai-research) | **129** | RAG patterns · agent evals · fine-tuning recipes · LLM-ops |
| [`business-marketing`](./skills/skills/business-marketing) | **47** | brand · pricing · pitch · GTM · enterprise sales |
| [`productivity`](./skills/skills/productivity) | **44** | crafting-effective-readmes · humanizer · skill-judge · ship-learn-next |
| [`security`](./skills/skills/security) | **42** | OWASP audits · secrets scanning · IAM patterns |
| [`creative-design`](./skills/skills/creative-design) | **41** | banner-design · brand-identity · slide-systems |
| [`enterprise-communication`](./skills/skills/enterprise-communication) | **33** | feedback-mastery · session-handoff · daily-meeting-update |
| [`web-development`](./skills/skills/web-development) | **29** | shopify-development · next.js patterns · static site recipes |
| [`workflow-automation`](./skills/skills/workflow-automation) | **22** | n8n full pack (validation, patterns, expressions, code-js/py, MCP tools) |
| [`document-processing`](./skills/skills/document-processing) | **17** | OCR · markitdown · PDF → structured data |
| [`utilities`](./skills/skills/utilities) | **12** | domain-name-brainstormer · file utils |
| [`railway`](./skills/skills/railway) | **12** | one-click deploys, services, volumes |
| [`database`](./skills/skills/database) | **10** | schema design, migrations, query tuning |
| [`ai-maestro`](./skills/skills/ai-maestro) · [`sentry`](./skills/skills/sentry) · [`pocketbase`](./skills/skills/pocketbase) · [`web-data`](./skills/skills/web-data) | 6 each | Observability, BaaS & scraping |
| [`media`](./skills/skills/media) · [`video`](./skills/skills/video) · [`git`](./skills/skills/git) | 2–5 | FFmpeg, Remotion, git workflows |
| [`analytics`](./skills/skills/analytics) · [`design-to-code`](./skills/skills/design-to-code) · [`gmod-addon-maker`](./skills/skills/gmod-addon-maker) · [`marketing`](./skills/skills/marketing) · [`sports`](./skills/skills/sports) | 1 each | Edge & niche |

---

## Agents Roster

> 28 specialist teams in [`agents/agents/`](./agents/agents) — invoke them with `Task` / `@-mention` in Claude Code or Cursor.

| Pack | Count | What they do |
|------|------:|--------------|
| [`expert-advisors`](./agents/agents/expert-advisors) | **52** | Niche advisory roles (legal, pricing, GTM, product strategy) |
| [`programming-languages`](./agents/agents/programming-languages) | **50** | Senior engineers per language (Rust, Go, Swift, Elixir, Kotlin…) |
| [`data-ai`](./agents/agents/data-ai) | **40** | Data engineers, ML ops, vector search, eval harnesses |
| [`devops-infrastructure`](./agents/agents/devops-infrastructure) | **39** | k8s, Terraform, Pulumi, observability, SRE |
| [`development-tools`](./agents/agents/development-tools) | **34** | Linters, codegen, refactor bots, doc generators |
| [`business-marketing`](./agents/agents/business-marketing) · [`security`](./agents/agents/security) | 23 each | Growth/SEO/copy · AppSec/cloud-sec/red-team |
| [`development-team`](./agents/agents/development-team) · [`deep-research-team`](./agents/agents/deep-research-team) · [`web-tools`](./agents/agents/web-tools) | 16–17 | Full SDLC + multi-agent research pods |
| [`documentation`](./agents/agents/documentation) · [`database`](./agents/agents/database) · [`podcast-creator-team`](./agents/agents/podcast-creator-team) | 11 | Docs-as-code, DBA, podcast end-to-end |
| [`mcp-dev-team`](./agents/agents/mcp-dev-team) · [`ai-specialists`](./agents/agents/ai-specialists) · [`api-graphql`](./agents/agents/api-graphql) · [`ffmpeg-clip-team`](./agents/agents/ffmpeg-clip-team) | 8 each | Build MCPs · prompt eng · GraphQL · video edit |
| [`obsidian-ops-team`](./agents/agents/obsidian-ops-team) · [`ocr-extraction-team`](./agents/agents/ocr-extraction-team) | 7 | Knowledge graphs · doc parsing pipelines |
| [`finance`](./agents/agents/finance) · [`performance-testing`](./agents/agents/performance-testing) · [`game-development`](./agents/agents/game-development) · [`ui-analysis`](./agents/agents/ui-analysis) | 5 | Modeling · perf bench · gameplay · UX audits |
| [`blockchain-web3`](./agents/agents/blockchain-web3) · [`modernization`](./agents/agents/modernization) · [`git`](./agents/agents/git) · [`realtime`](./agents/agents/realtime) · [`accessibility`](./agents/agents/accessibility) | 1–4 | Smart contracts · legacy migration · git surgeon · WebRTC · a11y |

---

## Bonus Arsenal

### Slash-Commands · [`bonus/commands/`](./bonus/commands) · **341 commands** in 24 packs

`analysis` · `automation` · `azure` · `database` · `deployment` · `design` · `documentation` · `game-development` · `git` · `git-workflow` · `google-workspace` · `marketing` · `nextjs-vercel` · `orchestration` · `performance` · `project-management` · `security` · `setup` · `simulation` · `svelte` · `sync` · `team` · `testing` · `utilities`

### Hooks · [`bonus/hooks/`](./bonus/hooks) · **11 packs**

`automation` · `development-tools` · `git` · `git-workflow` · `monitoring` · `performance` · `post-tool` · `pre-tool` · `quality-gates` · `security` · `testing` · plus a pre-compiled [`HOOK_PATTERNS_COMPRESSED.json`](./bonus/hooks/HOOK_PATTERNS_COMPRESSED.json).

### MCP Servers · [`bonus/mcps/`](./bonus/mcps) · **13 servers**

`audio` · `browser_automation` · `database` · `deepgraph` · `deepresearch` · `devtools` · `filesystem` · `integration` · `marketing` · `productivity` · `research` · `web` · `web-data`

### Sandbox · [`bonus/sandbox/`](./bonus/sandbox)

Isolated execution recipes for `cloudflare`, `docker` and `e2b` — drop-in containers for running untrusted/agentic code.

### Settings Presets · [`bonus/settings/`](./bonus/settings) · **12 profiles**

`api` · `authentication` · `cleanup` · `environment` · `git` · `global` · `mcp` · `model` · `partnerships` · `permissions` · `statusline` · `telemetry`

### Voice Clones · [`clones/Clones/slim/`](./clones/Clones/slim) · **31 style guides**

Drop-in `.md` files that teach Claude to write in the voice of high-signal creators: **Naval Ravikant, Alex Hormozi, MrBeast, Tim Urban, Nassim Taleb, Robert Cialdini, Eugene Schwartz, Justin Welsh, Lara Acosta, Gary Vaynerchuk, Ann Handley, Donald Miller, Oren Klaff, Peter Thiel, Nicolas Cole, Sahil Bloom, Ryan Holiday, Jonah Berger, Nir Eyal, Dan Koe, Nancy Duarte, Robert McKee, Malcolm Gladwell, Derek Muller, Derral Eves, Paddy Galloway, Richard van der Blom, Jasmin Alic, Jenny Hoyos, Chris Walker, George Loewenstein.**

---

## How to use with Claude Code

### 1. Auto-trigger from natural language

Every `SKILL.md` ships with a `description:` frontmatter packed with trigger phrases. Claude Code, Cursor and the Anthropic CLI **load the right skill automatically** when your prompt matches.

```text
You: "Audit this PR for OWASP top-10 issues and write redlines."
Claude → loads: skills/skills/security/* + agents/agents/security/*
```

### 2. Manually attach a skill folder

In your project's `.claude/settings.json` (or `AGENTS.md`), point Claude at the folder:

```json
{
  "skills": {
    "search_paths": [
      "../SKILLS-CLAUDE-CODE/skills/skills",
      "../SKILLS-CLAUDE-CODE/favorites",
      "../SKILLS-CLAUDE-CODE/agents/agents"
    ]
  }
}
```

### 3. Cherry-pick a single skill into your repo

```bash
# Copy one skill into your project
cp -R SKILLS-CLAUDE-CODE/skills/skills/development/stripe-integration .claude/skills/

# Or symlink (POSIX)
ln -s "$(pwd)/SKILLS-CLAUDE-CODE/skills/skills/development/playwright" .claude/skills/playwright
```

### 4. Track upstream sources with the lockfile

[`skills-lock.json`](./skills-lock.json) records the **source repo, path and content hash** of every imported skill, so you can re-sync without losing local edits.

---

## Why this collection?

| Factor | This repo |
|--------|-----------|
| **Coverage** | 1,044+ skills across 26 categories — the broadest open collection |
| **Production-ready** | Every skill produces tangible output (files, configs, deployable artifacts) |
| **Lockfile-tracked** | `skills-lock.json` pins source + hash for reproducibility |
| **Multi-tool** | Works with Claude Code, Cursor, Codex, Anthropic CLI, MCP-aware editors |
| **Curated, not collected** | `favorites/` short-list + categorical buckets, not a link dump |
| **Open spec** | Follows the official [Claude Code plugin spec](https://docs.claude.com/claude-code) |

---

## Contributing

PRs welcome — especially:

1. **New skills** — follow the [`SKILL_SHAPE.md`](./claude-skills-main/claude-skills-main/SKILL_SHAPE.md) authoring guide from Jezweb.
2. **Translations** — open an issue tagged [`i18n`](https://github.com/artubss/SKILLS-CLAUDE-CODE/issues/new?labels=i18n). The selector at the top of this file is wired with `<!-- LANGUAGE-SELECTOR-START/END -->` markers so future variants (`README.pt.md`, `README.es.md`, `README.zh.md`) drop in cleanly.
3. **Fixes / errata** — add `ERRATA.md` next to a `SKILL.md` instead of rewriting outdated content.
4. **Curated favorites** — submit your daily-driver skills with a one-line "why it's awesome".

```bash
git checkout -b skill/<short-name>
# add your SKILL.md inside the matching category folder
git commit -m "feat(skills): add <short-name>"
git push origin skill/<short-name>
# open a PR
```

---

## Recommended GitHub Settings

> Apply these in **Repo → Settings → General** and the **About** sidebar to maximise SEO + GEO (AI citation) signals.

| Field | Suggested value |
|-------|-----------------|
| **Repository name** | Current: `SKILLS-CLAUDE-CODE` — **strongly recommend renaming to `claude-skills-hub`** (lowercase + hyphens rank better on GitHub Search and Google; GitHub auto-redirects the old URL) |
| **Description (About)** | `The largest curated collection of 1,044+ Claude Code skills, agents, hooks, MCP servers and slash-commands. Production-ready, lockfile-tracked, organized by 26 categories.` |
| **Website** | `https://www.linkedin.com/in/artubs/` *(or your canonical site)* |
| **Topics (12)** | `claude-code` · `claude-skills` · `anthropic` · `awesome-list` · `agent-skills` · `mcp` · `mcp-servers` · `ai-agents` · `prompt-engineering` · `cursor` · `developer-tools` · `automation` |

---

## Credits

Curated and maintained by **[@artubss](https://github.com/artubss)**. This hub stands on the shoulders of incredible open-source maintainers tracked in [`skills-lock.json`](./skills-lock.json):

- [`jezweb/claude-skills`](https://github.com/jezweb/claude-skills) — 10-plugin marketplace (MIT) included as a vendored subtree under [`claude-skills-main/`](./claude-skills-main/claude-skills-main/).
- [`kostja94/marketing-skills`](https://github.com/kostja94/marketing-skills) — source of the `github` SEO skill.
- [`xixu-me/skills`](https://github.com/xixu-me/skills) — source of the `readme-i18n` skill.

If your skill is included and you'd like attribution adjusted, open an issue — credit will be applied immediately.

---

## License

[MIT](./LICENSE) — use, fork, remix, ship. Attribution appreciated.

---

## Connect

<p align="center">
  <a href="https://github.com/artubss">
    <img alt="GitHub" src="https://img.shields.io/badge/GitHub-%40artubss-181717?style=for-the-badge&logo=github&logoColor=white">
  </a>
  <a href="https://www.linkedin.com/in/artubs/">
    <img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-Arthur%20Klinos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white">
  </a>
  <a href="https://instagram.com/arthurklinos">
    <img alt="Instagram" src="https://img.shields.io/badge/Instagram-%40arthurklinos-E4405F?style=for-the-badge&logo=instagram&logoColor=white">
  </a>
</p>

<div align="center">

### Star History

<a href="https://www.star-history.com/#artubss/SKILLS-CLAUDE-CODE&Date">
  <img alt="Star History" src="https://api.star-history.com/svg?repos=artubss/SKILLS-CLAUDE-CODE&type=Date" width="720">
</a>

<br/><br/>

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=12,2,6,20,30&height=120&section=footer&text=Built%20for%20builders.%20Powered%20by%20Claude.&fontSize=18&fontColor=ffffff&animation=twinkling&fontAlignY=70" alt="footer" />

<sub>Last updated: <a href="https://github.com/artubss/SKILLS-CLAUDE-CODE/commits/main">see latest commits</a> · ⭐ a star helps it reach the next dev who needs it.</sub>

</div>