---
layout: section
---

# 🏗️ Platform Engineering

---
transition: fade-out
---

# Le problème en équipe

> _"Ça marche sur ma machine"_ 🤷

<v-clicks>

- Chaque développeur a sa propre version de Node, Python, Terraform…
- Les scripts `Makefile` ne tournent pas partout
- Les variables d'environnement divergent entre les membres
- Le onboarding d'un nouveau développeur prend des heures

</v-clicks>

---
transition: fade-out
---

# Solution : `.mise.toml` commité dans git

```toml {1-5|7-9|11-14|all}
# .mise.toml — commité dans git ✅
[tools]
node      = "24"
python    = "3.12"
terraform = "1.9"

[env]
NODE_ENV     = "development"
TF_WORKSPACE = "dev"

[tasks.setup]
depends     = ["install-deps"]
description = "Onboarding complet en une commande"
run         = "mise install && mise run install-deps"
```

<v-click>

> ✅ `mise install && mise run setup` — tout le monde démarre **exactement** pareil.

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Config globale

`~/.config/mise/config.toml` — outils disponibles partout :

```toml
[tools]
node      = "24"
python    = "3.12"
jq        = "latest"
gh        = "latest"

[env]
DOCKER_BUILDKIT = "1"
```

::right::

<v-click>

### Surcharge par repo

```text
~/.config/mise/config.toml   ← config globale
      ↓  hérite
~/projects/
├── frontend/
│   └── .mise.toml   ← node@22 (override)
├── backend/
│   └── .mise.toml   ← python@3.11
└── infra/
    └── .mise.toml   ← terraform@1.8
```

</v-click>

<v-click>

```text
$ cd ~/projects/frontend
$ node --version        → 22.x  (local)

$ cd ~/projects/        (pas de .mise.toml)
$ node --version        → 24.x  (global)
```

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# CI/CD — `jdx/mise-action`

```yaml
# .github/workflows/ci.yml
jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: jdx/mise-action@v2
        # lit automatiquement .mise.toml

      - run: mise install

      - run: mise run ci
        env:
          MISE_YES: "1"
```

::right::

<v-click>

### Convention cross-repo

```toml
# Chaque repo expose ces tâches standard
[tasks.ci]
depends = ["lint", "test", "build"]

[tasks.lint]
description = "Vérifier le code"

[tasks.test]
description = "Lancer les tests"

[tasks.build]
description = "Builder l'artefact"
```

> 💡 `mise run ci` — interface uniforme  
> quel que soit le repo ou le langage.

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Monorepo — Structure

```text
monorepo/
├── .mise.toml          ← outils & env communs
├── mise.lock           ← lockfile commité ✅
├── .mise/tasks/
│   ├── ci              ← pipeline global
│   └── release         ← release orchestrée
└── packages/
    ├── frontend/
    │   └── .mise.toml  ← node@22, PORT=3000
    ├── backend/
    │   └── .mise.toml  ← python@3.12, PORT=8000
    └── infra/
        └── .mise.toml  ← terraform@1.9
```

::right::

<v-click>

### Orchestration des packages

```toml
# monorepo/.mise.toml
[tasks.ci]
depends = ["ci:frontend", "ci:backend", "ci:infra"]

[tasks."ci:frontend"]
dir = "packages/frontend"
run = "mise run ci"

[tasks."ci:backend"]
dir = "packages/backend"
run = "mise run ci"
```

```bash
# Lance tous les CI en parallèle
mise run --jobs 4 ci
```

</v-click>

---
transition: fade-out
---

# Monorepo — Héritage d'environnement

```toml
# monorepo/.mise.toml (racine)
[env]
LOG_LEVEL   = "info"
CI_REGISTRY = "ghcr.io/myorg"
```

<v-click>

```toml
# packages/backend/.mise.toml
[env]
LOG_LEVEL = "debug"   # surcharge locale
PORT      = "8000"
# CI_REGISTRY hérité → "ghcr.io/myorg"
```

</v-click>

<v-click>

```text
Dans packages/backend/ :
$ echo $LOG_LEVEL    → debug          (surcharge)
$ echo $CI_REGISTRY  → ghcr.io/myorg  (hérité)
$ echo $PORT         → 8000           (local)
```

> 💡 Héritage automatique des `.mise.toml` parents jusqu'à la racine.

</v-click>

---
layout: center
transition: slide-up
---

# Platform Engineering — Récap

<v-clicks>

- 🌍 **Config globale** — outils par défaut sur toute la machine
- 🔁 **Cross-repo** — convention `mise run ci/lint/test/build` uniforme
- 🏗️ **Monorepo** — héritage de config + orchestration des sous-packages
- 🤝 **Onboarding** — `mise install && mise run setup` suffit
- ⚙️ **CI/CD** — `jdx/mise-action` : même comportement local et CI

</v-clicks>

<v-click>

> 🎯 mise est le **point d'entrée unique** de votre environnement, quel que soit le contexte.

</v-click>
