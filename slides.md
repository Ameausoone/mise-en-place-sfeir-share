---
theme: seriph
background: https://cover.sli.dev
title: "Mise en place — Gérer votre environnement de développement"
info: |
  ## Mise en place
  Une présentation sur [mise](https://mise.jdx.dev/), l'outil polyvalent pour gérer
  les versions de runtimes, les variables d'environnement et les tâches de développement.
class: text-center
drawings:
  persist: false
transition: slide-left
comark: true
---

# Mise en place

### Gérer votre environnement de développement

<div class="mt-4 text-lg opacity-80">
  Runtimes · Variables d'environnement · Tâches
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
des versions de runtimes, des variables d'environnement et des tâches de projet.
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

<div class="grid grid-cols-3 gap-6 mt-8">
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">🔧</div>
    <div class="font-bold">Runtimes</div>
    <div class="text-sm opacity-70">Node, Python, Go, Ruby, Java…</div>
  </div>
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">🌿</div>
    <div class="font-bold">Environnements</div>
    <div class="text-sm opacity-70">Variables d'env par répertoire</div>
  </div>
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">📋</div>
    <div class="font-bold">Tâches</div>
    <div class="text-sm opacity-70">Scripts et automatisations</div>
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
```

</div>
</div>

<div v-click class="mt-6">

### Vérification

```bash
mise --version
# mise 2024.x.x
```

</div>

<!--
L'installation est rapide et l'activation dans le shell permet à mise d'intercepter
les commandes et de charger automatiquement le bon environnement selon le répertoire.
-->

---
layout: two-cols
layoutClass: gap-8
---

# Gestion des Runtimes

<div class="mt-4">

### Installer une version

```bash
# Installer Node.js 20
mise install node@20

# Installer la dernière version de Python
mise install python@latest

# Installer plusieurs outils d'un coup
mise install node@20 python@3.12 go@1.22
```

### Utiliser une version

```bash
# Définir globalement
mise use --global node@20

# Définir pour le projet courant
mise use node@20

# Utiliser une version temporaire
mise exec node@18 -- node --version
```

</div>

::right::

<div class="mt-4">

### Lister les outils disponibles

```bash
# Lister les plugins disponibles
mise registry

# Lister les versions installées
mise list

# Lister les versions disponibles
mise ls-remote node
```

### Fichier de configuration

```toml
# .mise.toml (à la racine du projet)
[tools]
node = "20"
python = "3.12"
go = "1.22"
```

</div>

---

# Le fichier `.mise.toml`

<div class="grid grid-cols-2 gap-6">

<div>

### Configuration complète

```toml
# .mise.toml
[tools]
node = "20.11.0"
python = "3.12"
terraform = "1.7.0"
awscli = "latest"

[env]
NODE_ENV = "development"
DATABASE_URL = "postgres://localhost/mydb"
API_BASE_URL = "http://localhost:3000"

[tasks.dev]
run = "npm run dev"
description = "Démarrer le serveur de développement"

[tasks.test]
run = "npm test"
description = "Lancer les tests"

[tasks.lint]
run = "npm run lint"
description = "Vérifier le style du code"
```

</div>

<div>

<div v-click>

### Héritage et surcharge

```
monorepo/
├── .mise.toml        # Config globale
├── backend/
│   └── .mise.toml    # Surcharge pour le backend
└── frontend/
    └── .mise.toml    # Surcharge pour le frontend
```

</div>

<div v-click class="mt-4">

### Mise en place automatique

```bash
cd mon-projet/
# mise charge automatiquement :
# - les outils définis dans .mise.toml
# - les variables d'environnement
# - active les bonnes versions
```

</div>

</div>
</div>

---

# Variables d'environnement

<div class="grid grid-cols-2 gap-6">

<div>

### Dans `.mise.toml`

```toml
[env]
# Variables statiques
NODE_ENV = "development"
PORT = "3000"

# Référencer d'autres variables
DATABASE_URL = "postgres://{{env.DB_USER}}:{{env.DB_PASS}}@localhost/mydb"

# Valeur depuis un fichier
_.file = ".env"

# Valeur depuis une commande
API_KEY = { value = "{{exec('vault kv get -field=key secret/api')}}" }
```

</div>

<div>

### Fichiers `.env`

```toml
# .mise.toml
[env]
_.file = ".env"
_.file = [".env", ".env.local"]
```

<div v-click class="mt-4">

### Path management

```toml
[env]
# Ajouter au PATH
_.path = ["./bin", "./node_modules/.bin"]

# Exemple pratique
GOPATH = "{{env.HOME}}/go"
_.path = "{{env.GOPATH}}/bin"
```

</div>

</div>
</div>

---

# Gestion des Tâches

<div class="grid grid-cols-2 gap-6">

<div>

### Définition dans `.mise.toml`

```toml
[tasks.build]
run = "npm run build"
description = "Compiler le projet"

[tasks.test]
run = "npm test -- --coverage"
description = "Lancer les tests avec couverture"

[tasks.deploy]
run = """
  npm run build
  docker build -t myapp .
  docker push myapp
"""
description = "Builder et déployer"
depends = ["test"]
```

</div>

<div>

### Utilisation

```bash
# Lister les tâches disponibles
mise tasks
# ou
mise run --list

# Exécuter une tâche
mise run build
mise run test
mise run deploy

# Exécuter en parallèle
mise run --parallel build test
```

<div v-click class="mt-4">

### Tâches comme scripts

```
.mise/tasks/
├── build       # Script shell exécutable
├── test
└── deploy
```

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

### Structure de projet recommandée

```
mon-projet/
├── .mise.toml          # Outils + env + tâches
├── .mise.local.toml    # Config locale (gitignored)
├── .gitignore
└── src/
```

### `.gitignore`

```gitignore
# Fichier de config locale mise (secrets)
.mise.local.toml
.env
.env.local
```

::right::

### Partager la configuration

```toml
# .mise.toml (commité dans git)
[tools]
node = "20"
python = "3.12"

[env]
NODE_ENV = "development"
# Ne pas mettre de secrets ici !

[tasks.setup]
run = """
  npm install
  pip install -r requirements.txt
"""
description = "Installer les dépendances"
```

<div v-click class="mt-4 p-3 rounded bg-blue-500/10 border border-blue-500/30">
  💡 <code>.mise.toml</code> remplace <code>.nvmrc</code>, <code>.python-version</code>,
  <code>.tool-versions</code> et <code>Makefile</code> en un seul fichier.
</div>

---

# Fonctionnalités avancées

<div class="grid grid-cols-2 gap-6">

<div>

### Plugins personnalisés

```bash
# Ajouter un plugin custom
mise plugin add my-tool https://github.com/user/mise-my-tool

# Utiliser un plugin asdf existant
mise plugin add scala https://github.com/asdf-community/asdf-scala
```

### Hooks

```toml
# .mise.toml
[hooks]
enter = "echo 'Bienvenue dans {{config.project_root}}'"
leave = "echo 'Au revoir !'"
```

</div>

<div>

### Intégration CI/CD

```yaml
# .github/workflows/ci.yml
- name: Setup mise
  uses: jdx/mise-action@v2

- name: Install tools
  run: mise install

- name: Run tests
  run: mise run test
```

<div v-click class="mt-4">

### Trust

```bash
# Approuver un .mise.toml
mise trust

# Approuver automatiquement (CI)
MISE_YES=1 mise install
```

</div>

</div>
</div>

---
layout: center
class: text-center
---

# Récapitulatif

<div class="grid grid-cols-2 gap-6 mt-6 text-left max-w-3xl mx-auto">

<div v-click>

### ✅ Ce que mise fait

- Gère les versions de runtimes (Node, Python, Go…)
- Charge les variables d'environnement par projet
- Exécute des tâches de développement
- Compatible avec l'écosystème asdf
- Ultra-rapide (Rust 🦀)

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

# 4. Installer les outils
mise install
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

<div class="mt-8 grid grid-cols-3 gap-4 max-w-xl mx-auto text-sm">
  <a href="https://mise.jdx.dev/getting-started.html" target="_blank" class="p-3 rounded border border-primary/30 hover:border-primary/60 transition-colors">
    📖 Getting Started
  </a>
  <a href="https://mise.jdx.dev/configuration.html" target="_blank" class="p-3 rounded border border-primary/30 hover:border-primary/60 transition-colors">
    ⚙️ Configuration
  </a>
  <a href="https://mise.jdx.dev/tasks/" target="_blank" class="p-3 rounded border border-primary/30 hover:border-primary/60 transition-colors">
    📋 Tasks
  </a>
</div>

<PoweredBySlidev mt-10 />
