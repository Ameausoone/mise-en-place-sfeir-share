---
layout: section
---

# 🏗️ Platform Engineering

---
layout: two-cols
layoutClass: gap-8
---

# Mise en place en équipe

### Problème classique

> _"Ça marche sur ma machine"_ 🤷

<v-clicks>

- Chaque développeur a sa propre version de Node, Python, Terraform…
- Les scripts `Makefile` ne tournent pas partout
- Les variables d'environnement divergent entre les membres de l'équipe
- Le onboarding d'un nouveau développeur prend des heures

</v-clicks>

::right::

<v-click>

### Solution : `.mise.toml` commité

```toml
# .mise.toml — commité dans git
[tools]
node      = "24"
python    = "3.12"
terraform = "1.9"
kubectl   = "1.31"
helm      = "3.16"

[env]
NODE_ENV = "development"
TF_WORKSPACE = "dev"

[tasks.install-deps]
run         = "npm install"
description = "Installer les dépendances npm"

[tasks.setup]
depends     = ["install-deps"]
description = "Onboarding complet en une commande"
```

> ✅ Tout le monde utilise **exactement** les mêmes outils et versions.

</v-click>

---
layout: two-cols
layoutClass: gap-8
---

# Contexte Cross-Repo

### Config globale `~/.config/mise/config.toml`

```toml
# Outils disponibles partout sur la machine
[tools]
node     = "24"
python   = "3.12"
jq       = "latest"
gh       = "latest"
terraform = "1.9"

[env]
DOCKER_BUILDKIT = "1"
GOPRIVATE       = "github.com/myorg/*"

[tasks.doctor]
run         = "mise doctor && gh auth status"
description = "Vérifier l'environnement global"
```

::right::

### Surcharge par repo

```
~/.config/mise/config.toml    ← config globale (all repos)
      ↓  hérite
~/projects/
├── frontend/
│   └── .mise.toml            ← node@22 (override)
├── backend/
│   └── .mise.toml            ← python@3.11
└── infra/
    └── .mise.toml            ← terraform@1.8
```

<v-click>

```bash
# Dans ~/projects/frontend/
node --version   # → 22.x  (override local)

# Dans ~/projects/infra/
terraform --version  # → 1.8.x  (override local)

# Dans ~/projects/  (pas de .mise.toml)
node --version   # → 24.x  (config globale)
```

</v-click>

---
layout: two-cols
layoutClass: gap-8
---

# Contexte Cross-Repo — CI/CD

### Action GitHub partagée

```yaml
# .github/workflows/ci.yml  (dans chaque repo)
name: CI
on: [push, pull_request]

jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup mise
        uses: jdx/mise-action@v2
        # lit automatiquement .mise.toml

      - name: Install tools & deps
        run: mise install

      - name: Run CI pipeline
        run: mise run ci
        env:
          MISE_YES: "1"
```

::right::

<v-click>

### Tâches standardisées entre repos

```toml
# Convention : chaque repo expose ces tâches
[tasks.ci]
depends     = ["lint", "test", "build"]
description = "Pipeline CI complet"

[tasks.lint]
description = "Vérifier le code"
# ... implémentation spécifique au repo

[tasks.test]
description = "Lancer les tests"

[tasks.build]
description = "Builder l'artefact"
```

> 💡 Interface uniforme `mise run ci` quel que soit le repo ou le langage.

</v-click>

---
layout: two-cols
layoutClass: gap-8
---

# Contexte Monorepo

### Structure recommandée

```
monorepo/
├── .mise.toml              ← outils & env communs
├── mise.lock               ← lockfile commité
├── .mise/
│   └── tasks/
│       ├── ci              ← pipeline global
│       ├── lint-all        ← lint tous les packages
│       └── release         ← release orchestrée
├── packages/
│   ├── frontend/
│   │   └── .mise.toml      ← node@22, PORT=3000
│   ├── backend/
│   │   └── .mise.toml      ← python@3.12, PORT=8000
│   └── infra/
│       └── .mise.toml      ← terraform@1.9
└── .gitignore
```

::right::

### `.mise.toml` racine

```toml
# monorepo/.mise.toml
[tools]
node      = "24"
python    = "3.12"
terraform = "1.9"

[env]
CI_REGISTRY = "ghcr.io/myorg"

[tasks.ci]
depends = ["ci:frontend", "ci:backend", "ci:infra"]
description = "CI globale (tous les packages)"

[tasks."ci:frontend"]
dir = "packages/frontend"
run = "mise run ci"

[tasks."ci:backend"]
dir = "packages/backend"
run = "mise run ci"

[tasks."ci:infra"]
dir = "packages/infra"
run = "mise run ci"
```

---
layout: two-cols
layoutClass: gap-8
---

# Monorepo — Tâches avancées

### Exécution parallèle des sous-packages

```toml
# monorepo/.mise.toml
[tasks.build-all]
depends     = ["build:frontend", "build:backend"]
description = "Builder tous les packages en parallèle"

[tasks."build:frontend"]
dir         = "packages/frontend"
run         = "npm run build"
sources     = ["packages/frontend/src/**"]
outputs     = ["packages/frontend/dist/**"]

[tasks."build:backend"]
dir         = "packages/backend"
run         = "pip install -e . && python -m build"
sources     = ["packages/backend/src/**"]
outputs     = ["packages/backend/dist/**"]
```

```bash
# Lancer tous les builds en parallèle
mise run --jobs 4 build-all
```

::right::

<v-click>

### Héritage d'environnement

```toml
# monorepo/.mise.toml (racine)
[env]
LOG_LEVEL   = "info"
CI_REGISTRY = "ghcr.io/myorg"
```

```toml
# packages/backend/.mise.toml
[env]
LOG_LEVEL = "debug"    # surcharge locale
PORT      = "8000"
# CI_REGISTRY hérité du parent → "ghcr.io/myorg"
```

```bash
# Dans packages/backend/
echo $LOG_LEVEL    # → debug   (surcharge)
echo $CI_REGISTRY  # → ghcr.io/myorg (hérité)
echo $PORT         # → 8000    (local)
```

> 💡 Héritage automatique des `.mise.toml` parents jusqu'à la racine du repo.

</v-click>

---
layout: center
---

# Platform Engineering — Récap

<v-clicks>

- 🌍 **Config globale** `~/.config/mise/config.toml` — outils par défaut sur toute la machine
- 🔁 **Cross-repo** — convention `mise run ci/lint/test/build` uniforme dans tous les repos
- 🏗️ **Monorepo** — héritage de config + tâches orchestrant les sous-packages
- 🤝 **Onboarding** — `mise install && mise run setup` suffit pour démarrer
- ⚙️ **CI/CD** — `jdx/mise-action` lit `.mise.toml`, même comportement local et CI

</v-clicks>

<v-click>

> 🎯 mise est le **point d'entrée unique** de votre environnement de développement, quel que soit le contexte.

</v-click>
