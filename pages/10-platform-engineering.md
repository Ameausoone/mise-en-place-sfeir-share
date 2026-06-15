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

- L'équipe sfeir-conf compte 5 développeurs, avec 3 versions de Node et 2 de Python…
- Les scripts `Makefile` ne tournent pas partout
- Les variables d'environnement divergent entre les membres
- Le onboarding d'un nouveau développeur prend des heures

</v-clicks>

---
transition: fade-out
---

# Solution : `.mise.toml` commité dans git

```toml {1-5|7-9|11-14|all}
# sfeir-conf/.mise.toml — commité dans git ✅
[tools]
node      = "24"
python    = "3.12"
terraform = "1.9"

[env]
NODE_ENV    = "development"
AWS_REGION  = "eu-west-1"

[tasks.setup]
run         = ["npm install", "pip install -r requirements.txt"]
description = "Installer toutes les dépendances"
depends     = []
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
~/projects/sfeir-conf/
├── .mise.toml       ← node@24, python@3.12, terraform@1.9
├── frontend/
│   └── .mise.toml   ← PORT=3000
└── backend/
    └── .mise.toml   ← PORT=8000
```

</v-click>

<v-click>

```text
$ cd ~/projects/sfeir-conf/frontend
$ node --version        → 24.x  (local)

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
# sfeir-conf/.github/workflows/ci.yml
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
# sfeir-conf — tâches standard exposées
[tasks.ci]
depends = ["lint", "test", "build"]

[tasks.lint]
run         = ["eslint src/", "ruff check app/"]
description = "Vérifier le code"

[tasks.test]
run         = ["npm test", "pytest"]
description = "Lancer les tests"

[tasks.build]
run         = "docker build -t sfeir-conf ."
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
sfeir-conf/
├── .mise.toml          ← outils & env communs
├── mise.lock           ← lockfile commité ✅
├── fnox.toml           ← secrets chiffrés
├── .mise/tasks/
│   ├── dev             ← pipeline global
│   └── deploy          ← déploiement orchestré
├── frontend/
│   └── .mise.toml      ← PORT=3000
├── backend/
│   └── .mise.toml      ← PORT=8000
└── infra/
    └── .mise.toml      ← terraform, AWS_WORKSPACE=dev
```

::right::

<v-click>

### Orchestration des packages

```toml
# sfeir-conf/.mise.toml
[tasks.ci]
depends = ["ci:frontend", "ci:backend", "ci:infra"]

[tasks."ci:frontend"]
dir = "frontend"
run = "mise run ci"

[tasks."ci:backend"]
dir = "backend"
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
# sfeir-conf/.mise.toml (racine)
[env]
NODE_ENV    = "development"
AWS_REGION  = "eu-west-1"
API_URL     = "http://localhost:8000"
```

<v-click>

```toml
# sfeir-conf/backend/.mise.toml
[env]
PORT = "8000"
# NODE_ENV et AWS_REGION hérités de la racine
```

</v-click>

<v-click>

```text
Dans sfeir-conf/backend/ :
$ echo $NODE_ENV    → development   (hérité)
$ echo $AWS_REGION  → eu-west-1     (hérité)
$ echo $PORT        → 8000          (local)
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
