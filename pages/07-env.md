---
layout: section
---

# 🌍 Environnement & Configuration

---
transition: fade-out
---

# Variables d'environnement

### Dans `.mise.toml`

```toml {1-5|6-8|9-11|all}
# sfeir-conf/.mise.toml
[env]
NODE_ENV    = "development"
API_URL     = "http://localhost:8000"
AWS_REGION  = "eu-west-1"

# Charger un fichier .env
_.file = ".env"

# Ajouter au PATH
_.path = ["./frontend/node_modules/.bin", "./backend/.venv/bin"]
```

<v-click>

### Templates — composer des valeurs dynamiques

```toml
[env]
DATABASE_URL = "postgres://{{env.DB_USER}}@localhost/sfeir_conf"
```

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Config locale & secrets

```toml
# sfeir-conf/.mise.local.toml  ← dans .gitignore !
[env]
DB_USER      = "alice"
DATABASE_URL = "postgres://alice:s3cr3t@localhost/sfeir_conf"
STRIPE_KEY   = "sk_test_..."
```

<v-click>

### Héritage entre répertoires

```text
sfeir-conf/
├── .mise.toml          ← NODE_ENV, API_URL, AWS_REGION
├── frontend/
│   └── .mise.toml      ← PORT=3000, VITE_API_URL=http://localhost:8000
└── backend/
    └── .mise.toml      ← PORT=8000, DATABASE_URL template
```

</v-click>

::right::

<v-click>

> 💡 Chaque sous-répertoire **hérite** et peut **surcharger** la configuration du parent.

</v-click>

<v-click>

### `.gitignore` recommandé

```bash
.mise.local.toml
.env
.env.local
```

</v-click>

---
layout: center
transition: fade-out
---

# Migration depuis d'autres outils

<v-clicks>

- **asdf** — `mise install` lit `.tool-versions` directement, compatible avec tous les plugins asdf

- **nvm / fnm** — mise lit `.nvmrc` automatiquement  
  `mise use node@$(cat .nvmrc)` pour importer

- **direnv** — mise gère `[env]` dans `.mise.toml`, compatible direnv

</v-clicks>

<v-click>

```bash
# Migration asdf → mise : zéro configuration
mise install  # lit automatiquement .tool-versions
```

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Pourquoi mise est rapide ? 🦀

### Écrit en Rust

<v-clicks>

- Démarrage quasi-instantané (~4ms)
- Pas de dépendance à Ruby/Python
- Installations parallélisées

</v-clicks>

<v-click>

### Shims vs PATH

| Approche | Mécanisme |
|----------|-----------|
| **asdf** | Shims (wrappers) — overhead à chaque commande |
| **mise** | Modifie directement le PATH — exécution directe |

</v-click>

::right::

<v-click>

### Comparaison

| Outil | Démarrage shell |
|-------|----------------|
| nvm   | ~100ms         |
| asdf  | ~200ms         |
| **mise**  | **~4ms** ⚡  |

> ✅ **50x plus rapide** qu'asdf

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Bonnes pratiques

### Structure recommandée

```text
sfeir-conf/
├── .mise.toml          # Outils + env + tâches
├── .mise.local.toml    # Config locale (gitignored)
├── mise.lock           # Lockfile (commité ✓)
└── .mise/
    └── tasks/
        ├── dev
        ├── test
        └── deploy
```

::right::

<v-click>

### CI/CD avec `jdx/mise-action`

```yaml
# .github/workflows/ci.yml
- name: Setup mise
  uses: jdx/mise-action@v2

- name: Run CI
  run: mise run ci
  env:
    MISE_YES: "1"
```

</v-click>

<v-click>

> 💡 `.mise.toml` remplace `.nvmrc`, `.tool-versions`,  
> `Makefile` et `.envrc` en **un seul fichier**.

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Monorepo `sfeir-conf` — Structure

```text
sfeir-conf/
├── .mise.toml          <- outils & env communs
├── mise.lock           <- lockfile commité
├── fnox.toml           <- secrets chiffrés
├── .mise/tasks/
│   ├── dev             <- pipeline global
│   └── deploy          <- déploiement orchestré
├── frontend/
│   └── .mise.toml      <- PORT=3000
├── backend/
│   └── .mise.toml      <- PORT=8000
└── infra/
    └── .mise.toml      <- terraform, AWS_WORKSPACE=dev
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
transition: slide-up
---

# Monorepo `sfeir-conf` — Héritage d'environnement

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
$ echo $NODE_ENV    -> development   (hérité)
$ echo $AWS_REGION  -> eu-west-1     (hérité)
$ echo $PORT        -> 8000          (local)
```

> 💡 Héritage automatique des `.mise.toml` parents jusqu'à la racine.

</v-click>
