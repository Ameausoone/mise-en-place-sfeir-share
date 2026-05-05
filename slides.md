---
theme: seriph
background: https://cover.sli.dev
title: "Mise en place — Gérer votre environnement de développement"
info: |
  ## Mise en place
  Une présentation sur [mise](https://mise.jdx.dev/), l'outil polyvalent pour gérer
  les versions de runtimes, les variables d'environnement, les tâches, les hooks
  et les secrets avec [fnox](https://fnox.jdx.dev/).
class: text-center
drawings:
  persist: false
transition: slide-left
comark: true
---

# Mise en place

### Gérer votre environnement de développement

<div class="mt-4 text-lg opacity-80">
  Tools · Registry · Tasks · Hooks · fnox
</div>

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Appuyez sur Espace <carbon:arrow-right />
</div>

<div class="abs-br m-6 text-xl">
  <a href="https://mise.jdx.dev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
mise (prononcé "meez") est un outil en ligne de commande qui unifie la gestion
des versions de runtimes, des variables d'environnement, des tâches et des hooks de projet.
-->

---
transition: fade-out
---

# Le problème

<div class="grid grid-cols-3 gap-6 mt-8">

<div v-click class="p-4 rounded-lg border border-red-400/30 bg-red-400/5">
  <div class="text-2xl mb-2">😤</div>
  <div class="font-bold mb-2">Versions multiples</div>
  <div class="text-sm opacity-75">
    Node 18 pour le projet A, Node 20 pour le projet B…<br/>
    nvm, n, fnm, volta… lequel choisir ?
  </div>
</div>

<div v-click class="p-4 rounded-lg border border-yellow-400/30 bg-yellow-400/5">
  <div class="text-2xl mb-2">🔑</div>
  <div class="font-bold mb-2">Variables d'environnement</div>
  <div class="text-sm opacity-75">
    .env, direnv, des secrets différents par projet…
    comment les partager sans les committer ?
  </div>
</div>

<div v-click class="p-4 rounded-lg border border-blue-400/30 bg-blue-400/5">
  <div class="text-2xl mb-2">⚙️</div>
  <div class="font-bold mb-2">Tâches répétitives</div>
  <div class="text-sm opacity-75">
    Makefile, scripts shell, Taskfile…
    chaque projet a ses propres conventions.
  </div>
</div>

</div>

<div v-click class="mt-10 text-center text-xl">
  → <strong>mise</strong> résout tout ça avec un seul outil 🎉
</div>

---
layout: center
class: text-center
---

# Qu'est-ce que mise ?

<div class="mt-6 text-lg opacity-80 max-w-2xl mx-auto">

**mise** est un gestionnaire d'environnement de développement polyglotte.

Il remplace **asdf**, **nvm**, **pyenv**, **rbenv**, **direnv** et bien d'autres
avec un seul outil rapide, écrit en Rust 🦀.

</div>

<div class="grid grid-cols-4 gap-4 mt-8">
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">🔧</div>
    <div class="font-bold">Tools</div>
    <div class="text-sm opacity-70">Node, Python, Go, Ruby…</div>
  </div>
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">🌿</div>
    <div class="font-bold">Env</div>
    <div class="text-sm opacity-70">Variables par répertoire</div>
  </div>
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">📋</div>
    <div class="font-bold">Tasks</div>
    <div class="text-sm opacity-70">Scripts & automatisations</div>
  </div>
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">🪝</div>
    <div class="font-bold">Hooks</div>
    <div class="text-sm opacity-70">Réagir aux événements</div>
  </div>
</div>

<div class="grid grid-cols-2 gap-4 mt-4 max-w-sm mx-auto">
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">📦</div>
    <div class="font-bold">Registry</div>
    <div class="text-sm opacity-70">800+ outils disponibles</div>
  </div>
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">🔐</div>
    <div class="font-bold">fnox</div>
    <div class="text-sm opacity-70">Secrets chiffrés en git</div>
  </div>
</div>

<div v-click class="mt-8">
  <a href="https://mise.jdx.dev" target="_blank" class="text-primary">mise.jdx.dev</a>
</div>

---

# Installation

<div class="grid grid-cols-2 gap-6">

<div>

### Linux / macOS

```bash
# Via curl (recommandé)
curl https://mise.run | sh

# Via Homebrew
brew install mise

# Via cargo
cargo install mise

# Via winget (Windows)
winget install jdx.mise
```

</div>

<div>

### Activation dans votre shell

```bash
# Bash (~/.bashrc)
eval "$(mise activate bash)"

# Zsh (~/.zshrc)
eval "$(mise activate zsh)"

# Fish (~/.config/fish/config.fish)
mise activate fish | source

# Nushell (~/.config/nushell/config.nu)
mise activate nu | save -f ~/.config/nushell/mise.nu
```

</div>
</div>

<div v-click class="mt-4">

### `mise doctor` — diagnostics d'installation

```bash
mise doctor
# ✓ mise version: 2024.x.x
# ✓ shell: bash
# ✗ nvm detected — can conflict with mise, disable it
# ✓ ~/.local/share/mise/shims is in PATH
```

</div>

<!--
L'installation est rapide et l'activation dans le shell permet à mise de charger
automatiquement le bon environnement selon le répertoire courant.
`mise doctor` est la première commande à lancer en cas de problème.
-->

---
layout: section
---

# 📦 Registry

---
layout: two-cols
layoutClass: gap-8
---

# Registry — 800+ outils disponibles

<div class="mt-2">

### Explorer le registre

```bash
# Lister tous les outils
mise registry

# Rechercher
mise registry | grep -i terraform
mise registry | grep -i java

# Afficher les backends disponibles
mise registry --backend asdf
mise registry --backend aqua
```

### Quelques outils populaires

```bash
mise use node terraform kubectl \
          helm awscli gh jq ripgrep
```

| Outil | Backend |
|-------|---------|
| `node`, `python`, `go`, `ruby` | core / asdf |
| `terraform`, `kubectl`, `helm` | asdf / aqua |
| `gh`, `jq`, `ripgrep`, `fd`    | aqua / ubi |

</div>

::right::

<div class="mt-2">

### Les backends

mise peut installer des outils via **plusieurs sources** :

```toml
[tools]
# asdf (par défaut, compatible 800+ plugins)
node = "20"
terraform = "1.7"

# npm — package npm global
"npm:prettier" = "latest"
"npm:typescript" = "5"

# cargo — compile depuis crates.io
"cargo:ripgrep" = "14"

# pipx — package Python isolé
"pipx:ansible" = "latest"

# ubi — binaire GitHub Releases (universel)
"ubi:cli/cli" = "latest"        # gh CLI
"ubi:sharkdp/bat" = "latest"    # bat

# go — binaire Go
"go:mvdan.cc/gofumpt" = "latest"
```

</div>

---

# Registry — Lockfile

<div class="grid grid-cols-2 gap-6">

<div>

### `mise.lock` — figer les versions

```toml
# mise.lock — généré automatiquement
# À commiter dans git !

[tools.node]
version  = "20.11.0"
checksum = "sha256:abc123…"

[tools.python]
version  = "3.12.3"
checksum = "sha256:def456…"

[tools.terraform]
version  = "1.7.5"
checksum = "sha256:ghi789…"
```

```bash
# Activer le lockfile
mise settings set lockfile true

# Installer exactement les versions lockées
mise install
```

</div>

<div v-click>

### Reproductibilité garantie

```bash
# Machine A (dev)
mise install  # installe node@20.11.0

# Machine B (CI)
mise install  # installe node@20.11.0 ✓

# Mettre à jour le lockfile
mise upgrade        # met à jour toutes les versions
mise upgrade node   # met à jour seulement node
```

### Intégration CI avec `jdx/mise-action`

```yaml
- uses: jdx/mise-action@v2
  # lit automatiquement .mise.toml + mise.lock
  # installe exactement les bonnes versions
```

<div class="mt-3 p-3 rounded bg-blue-500/10 border border-blue-500/30 text-sm">
  💡 Comme <code>package-lock.json</code> mais pour
  <strong>tous vos runtimes et outils CLI</strong>.
</div>

</div>

</div>

---
layout: section
---

# 🔧 Tools

---
layout: two-cols
layoutClass: gap-8
---

# Tools — Commandes essentielles

<div class="mt-2">

### Installer & utiliser

```bash
# Installer une version
mise install node@20
mise install python@latest
mise install node@20 python@3.12 go@1.22

# Définir pour le projet courant (.mise.toml)
mise use node@20

# Définir globalement (~/.config/mise/config.toml)
mise use --global node@20

# Exécuter une commande avec une version précise
mise exec node@18 -- node --version

# Lancer un shell avec les outils chargés
mise shell node@18
```

### Lister

```bash
mise list           # versions installées
mise ls-remote node # versions disponibles
mise outdated       # outils à mettre à jour
mise upgrade        # tout mettre à jour
```

</div>

::right::

<div class="mt-2">

### Registre & plugins

```bash
# Explorer les outils disponibles
mise registry

# Rechercher un outil
mise registry | grep terraform

# Ajouter un plugin asdf existant
mise plugin add scala \
  https://github.com/asdf-community/asdf-scala

# Ajouter un plugin custom
mise plugin add my-tool \
  https://github.com/user/mise-my-tool
```

### Dans `.mise.toml`

```toml
[tools]
node       = "20"          # version majeure
python     = "3.12.3"      # version exacte
go         = "latest"      # dernière stable
terraform  = "1.7"
awscli     = "2"
jq         = "latest"
```

</div>

---
layout: section
---

# 📋 Tasks

---
layout: two-cols
layoutClass: gap-8
---

# Tasks — Dans `.mise.toml`

<div class="mt-2">

### Définir des tâches

```toml
[tasks.install]
run         = "npm install"
description = "Installer les dépendances"

[tasks.dev]
run         = "npm run dev"
description = "Démarrer le serveur de développement"
depends     = ["install"]

[tasks.test]
run         = "npm test -- --coverage"
description = "Tests avec couverture de code"

[tasks.lint]
run         = ["eslint src/", "prettier --check src/"]
description = "Vérifier le style du code"

[tasks.build]
run         = "npm run build"
description = "Build de production"
depends     = ["lint", "test"]
```

</div>

::right::

<div class="mt-2">

### Exécuter

```bash
# Lister toutes les tâches
mise tasks
mise run --list

# Lancer une tâche
mise run dev
mise run test

# Passer des arguments
mise run test -- --watch

# Exécution parallèle
mise run --jobs 4 lint test

# Voir ce qui serait exécuté
mise run --dry-run build
```

### Variables dans les tâches

```toml
[tasks.deploy]
run = """
  echo "Deploying to $ENVIRONMENT"
  docker build -t myapp:$VERSION .
"""
env   = { ENVIRONMENT = "production" }
depends = ["test", "build"]
```

</div>

---

# Tasks — Tâches fichiers

<div class="grid grid-cols-2 gap-6">

<div>

### Scripts dans `.mise/tasks/`

Des **scripts shell exécutables** directement dans le repo,
sans les écrire dans le `.mise.toml` :

```
.mise/
└── tasks/
    ├── dev          ← mise run dev
    ├── test         ← mise run test
    ├── build        ← mise run build
    └── db/
        ├── migrate  ← mise run db:migrate
        └── seed     ← mise run db:seed
```

```bash
#!/usr/bin/env bash
# .mise/tasks/dev
#MISE description="Démarrer le serveur de dev"
#MISE depends=["install"]

set -euo pipefail
npm run dev
```

</div>

<div v-click>

### Avantages des tâches fichiers

- Écrites dans n'importe quel langage (`bash`, `python`, `node`…)
- Coloration syntaxique dans l'éditeur
- Testables et versionnables indépendamment

```python
#!/usr/bin/env python3
# .mise/tasks/generate-report
#MISE description="Générer le rapport mensuel"

import json, datetime

data = {"date": str(datetime.date.today())}
print(json.dumps(data, indent=2))
```

```bash
# Rendre exécutable
chmod +x .mise/tasks/generate-report

# Puis simplement :
mise run generate-report
```

</div>

</div>

---

# Tasks — Options avancées

<div class="grid grid-cols-2 gap-6">

<div>

### `mise watch` — Rechargement automatique

```bash
# Relancer la tâche à chaque modification
mise watch -t test

# Surveiller des fichiers spécifiques
mise watch -t build -- src/**/*.ts
```

```toml
# Configurer dans .mise.toml
[tasks.test]
run    = "npm test"
sources = ["src/**/*.ts", "tests/**/*.ts"]
outputs = ["coverage/"]
```

### Dépendances & graphe

```toml
[tasks.ci]
# Lance lint et test en parallèle,
# puis build si les deux réussissent
depends = ["lint", "test"]
run     = "npm run build"
```

</div>

<div v-click>

### Arguments & `usage`

```toml
[tasks.deploy]
#USAGE flag "-e --env <env>" help="Target environment"
#USAGE flag "--dry-run"      help="Simulate only"
run = """
  echo "Deploy to: $usage_env"
  if [ "$usage_dry_run" = "true" ]; then
    echo "(dry-run, nothing deployed)"
  fi
"""
```

```bash
mise run deploy --env staging
mise run deploy --env prod --dry-run
```

### `mise run` — aide intégrée

```bash
mise run --help          # aide globale
mise run deploy --help   # aide de la tâche
```

</div>

</div>

---
layout: section
---

# 🪝 Hooks

---

# Hooks — Réagir aux événements

<div class="grid grid-cols-2 gap-6">

<div>

### Hooks de répertoire

Déclenchés automatiquement quand vous **entrez ou quittez** un répertoire :

```toml
# .mise.toml
[hooks]
enter = "echo '👋 Bienvenue dans {{config.project_root}}'"
leave = "echo '👋 À bientôt !'"
```

### Cas d'usage concrets

```toml
[hooks]
# Vérifier que les dépendances sont à jour
enter = """
  if [ package.json -nt node_modules ]; then
    echo "⚠️  Dépendances obsolètes, lancez : mise run install"
  fi
"""

# Nettoyer les processus au départ
leave = "pkill -f 'npm run dev' || true"
```

</div>

<div v-click>

### Hooks de tâches — `pre` / `post`

```toml
[tasks.deploy]
run  = "./scripts/deploy.sh"

[hooks]
# Avant chaque tâche
pre_task  = """
  echo "🚀 Démarrage : $MISE_TASK_NAME"
  date
"""

# Après chaque tâche (succès ou échec)
post_task = """
  echo "✅ Terminé  : $MISE_TASK_NAME"
  echo "   Exit code : $MISE_TASK_EXIT_CODE"
"""
```

### Variables disponibles dans les hooks

| Variable | Valeur |
|---|---|
| `MISE_TASK_NAME` | Nom de la tâche |
| `MISE_TASK_EXIT_CODE` | Code de sortie |
| `config.project_root` | Racine du projet |

</div>

</div>

---

# Hooks — `watch_files`

<div class="grid grid-cols-2 gap-6">

<div>

### Surveiller des fichiers

`watch_files` déclenche une tâche automatiquement dès qu'un fichier change :

```toml
# .mise.toml

[tasks.codegen]
run         = "npm run generate"
description = "Regénérer le code depuis le schéma"

[tasks.sync-deps]
run         = "npm install"
description = "Synchroniser les dépendances"

[watch_files]
# Regénérer si le schéma GraphQL change
"schema.graphql" = "codegen"

# Réinstaller si package.json change
"package.json"   = "sync-deps"
```

</div>

<div v-click>

### Activer la surveillance

```bash
# Démarrer le watcher
mise watch

# Tous les watch_files sont surveillés
# Les tâches se déclenchent automatiquement
# ↓ Modifiez schema.graphql…
# ✓ codegen relancé automatiquement !
```

### Combinaison hooks + watch

```toml
[watch_files]
".env.example" = "check-env"

[tasks.check-env]
run = """
  echo "⚠️  .env.example a changé !"
  echo "Vérifiez votre .env local."
"""
```

<div class="mt-3 p-3 rounded bg-green-500/10 border border-green-500/30 text-sm">
  ✅ Fini les <em>"n'oublie pas de relancer X après avoir modifié Y"</em>
</div>

</div>

</div>

---
layout: section
---

# 🌍 Environnement & Configuration

---

# Variables d'environnement

<div class="grid grid-cols-2 gap-6">

<div>

### Dans `.mise.toml`

```toml
[env]
# Variables statiques
NODE_ENV = "development"
PORT     = "3000"

# Charger un fichier .env
_.file = ".env"
# Ou plusieurs
_.file = [".env", ".env.local"]

# Ajouter au PATH
_.path = ["./bin", "./node_modules/.bin"]
```

### Templates

```toml
[env]
GOPATH = "{{env.HOME}}/go"
_.path = "{{env.GOPATH}}/bin"

DATABASE_URL = "postgres://{{env.DB_USER}}@localhost/mydb"
```

</div>

<div>

### Config locale (secrets)

```toml
# .mise.local.toml  ← dans .gitignore !
[env]
DB_USER   = "alice"
DB_PASS   = "s3cr3t"
API_KEY   = "sk-..."
```

### Héritage entre répertoires

```
monorepo/
├── .mise.toml          ← Node 20, env communs
├── backend/
│   └── .mise.toml      ← Python 3.12, PORT=8000
└── frontend/
    └── .mise.toml      ← PORT=3000
```

<div class="mt-3 p-3 rounded bg-blue-500/10 border border-blue-500/30 text-sm">
  💡 Chaque sous-répertoire hérite et peut surcharger
  la configuration du parent.
</div>

</div>
</div>

---
layout: center
---

# Migration depuis d'autres outils

<div class="grid grid-cols-3 gap-4 mt-6">

<div v-click class="p-4 rounded-lg border border-primary/30 text-center">
  <div class="text-2xl font-bold mb-2">asdf</div>
  <div class="text-sm opacity-70 mb-3">Migration transparente</div>

```bash
# mise lit .tool-versions
# Compatible avec les plugins asdf
mise install
```

</div>

<div v-click class="p-4 rounded-lg border border-primary/30 text-center">
  <div class="text-2xl font-bold mb-2">nvm / fnm</div>
  <div class="text-sm opacity-70 mb-3">Remplacer Node manager</div>

```bash
# Importer depuis .nvmrc
mise use node@$(cat .nvmrc)
# Lire .nvmrc automatiquement
```

</div>

<div v-click class="p-4 rounded-lg border border-primary/30 text-center">
  <div class="text-2xl font-bold mb-2">direnv</div>
  <div class="text-sm opacity-70 mb-3">Variables d'env</div>

```bash
# mise gère les .envrc
# ou utilise .mise.toml [env]
# Compatible direnv
```

</div>

</div>

<div v-click class="mt-6 text-center">

```bash
# Migration depuis .tool-versions (asdf)
mise install  # lit automatiquement .tool-versions
```

</div>

---

# Pourquoi mise est rapide ? 🦀

<div class="grid grid-cols-2 gap-6 mt-4">

<div>

### Écrit en Rust

- Démarrage quasi-instantané
- Pas de dépendance à Ruby/Python pour le core
- Parallélisation des installations

### Comparaison des performances

| Outil | Temps de démarrage shell |
|-------|------------------------|
| asdf  | ~200ms                 |
| mise  | ~4ms                   |
| nvm   | ~100ms                 |

</div>

<div v-click>

### Shims vs PATH

```
asdf utilise des "shims" (wrappers)
→ chaque commande passe par un script intermédiaire

mise modifie directement le PATH
→ exécution directe, sans overhead
```

<div class="mt-4 p-3 rounded bg-green-500/10 border border-green-500/30">
  ✅ 50x plus rapide qu'asdf au démarrage du shell
</div>

</div>

</div>

---
layout: two-cols
layoutClass: gap-8
---

# Bonnes pratiques

### Structure recommandée

```
mon-projet/
├── .mise.toml          # Outils + env + tâches
├── .mise.local.toml    # Config locale (gitignored)
├── mise.lock           # Lockfile (commité)
├── .mise/
│   └── tasks/          # Tâches fichiers
│       ├── dev
│       ├── test
│       └── build
└── .gitignore
```

### `.gitignore`

```gitignore
# Secrets locaux mise
.mise.local.toml
.env
.env.local
```

::right::

### CI/CD avec `jdx/mise-action`

```yaml
# .github/workflows/ci.yml
- name: Setup mise
  uses: jdx/mise-action@v2

- name: Install tools
  run: mise install

- name: Run CI
  run: mise run ci
  env:
    MISE_YES: "1"   # Approuver automatiquement
```

<div v-click class="mt-4 p-3 rounded bg-blue-500/10 border border-blue-500/30 text-sm">
  💡 <code>.mise.toml</code> remplace <code>.nvmrc</code>,
  <code>.tool-versions</code>, <code>Makefile</code> et <code>.envrc</code>
  en un <strong>seul fichier</strong>.
</div>

---
layout: center
class: text-center
---

# Récapitulatif

<div class="grid grid-cols-2 gap-6 mt-4 text-left max-w-3xl mx-auto">

<div v-click>

### ✅ Ce que mise fait

- 🔧 **Tools** — gère Node, Python, Go, Terraform…
- 🌿 **Env** — variables d'environnement par projet
- 📋 **Tasks** — scripts reproductibles avec dépendances
- 🪝 **Hooks** — réagir aux événements du projet
- ⚡ **Rapide** — écrit en Rust, ~4ms de démarrage

</div>

<div v-click>

### 🚀 Pour démarrer

```bash
# 1. Installer mise
curl https://mise.run | sh

# 2. Activer dans votre shell
echo 'eval "$(mise activate bash)"' >> ~/.bashrc

# 3. Créer un .mise.toml
mise use node@20 python@3.12

# 4. Tout installer
mise install

# 5. Lancer une tâche
mise run dev
```

</div>

</div>

---
layout: center
class: text-center
---

# Merci ! 🙏

<div class="mt-6 text-lg opacity-80">

Documentation officielle : [mise.jdx.dev](https://mise.jdx.dev)

GitHub : [github.com/jdx/mise](https://github.com/jdx/mise)

</div>

<div class="mt-8 grid grid-cols-4 gap-4 max-w-2xl mx-auto text-sm">
  <a href="https://mise.jdx.dev/getting-started.html" target="_blank" class="p-3 rounded border border-primary/30 hover:border-primary/60 transition-colors">
    📖 Getting Started
  </a>
  <a href="https://mise.jdx.dev/tasks/" target="_blank" class="p-3 rounded border border-primary/30 hover:border-primary/60 transition-colors">
    📋 Tasks
  </a>
  <a href="https://mise.jdx.dev/hooks.html" target="_blank" class="p-3 rounded border border-primary/30 hover:border-primary/60 transition-colors">
    🪝 Hooks
  </a>
  <a href="https://mise.jdx.dev/registry.html" target="_blank" class="p-3 rounded border border-primary/30 hover:border-primary/60 transition-colors">
    🔧 Registry
  </a>
</div>

<PoweredBySlidev mt-10 />
