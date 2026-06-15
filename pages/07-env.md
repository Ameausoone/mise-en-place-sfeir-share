---
layout: section
---

# 🌍 Environnement & Configuration

---
transition: fade-out
---

# Variables d'environnement

### Dans `.mise.toml`

```toml {1-4|6-7|9-10|all}
[env]
# Variables statiques
NODE_ENV = "development"
PORT     = "3000"

# Charger un fichier .env
_.file = [".env", ".env.local"]

# Ajouter au PATH
_.path = ["./bin", "./node_modules/.bin"]
```

<v-click>

### Templates — composer des valeurs dynamiques

```toml
[env]
GOPATH       = "{{env.HOME}}/go"
_.path       = "{{env.GOPATH}}/bin"
DATABASE_URL = "postgres://{{env.DB_USER}}@localhost/mydb"
```

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Config locale & secrets

```toml
# .mise.local.toml  ← dans .gitignore !
[env]
DB_USER   = "alice"
DB_PASS   = "s3cr3t"
API_KEY   = "sk-..."
```

<v-click>

### Héritage entre répertoires

```text
monorepo/
├── .mise.toml          ← Node 24, env communs
├── backend/
│   └── .mise.toml      ← Python 3.12, PORT=8000
└── frontend/
    └── .mise.toml      ← PORT=3000
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
mon-projet/
├── .mise.toml          # Outils + env + tâches
├── .mise.local.toml    # Config locale (gitignored)
├── mise.lock           # Lockfile (commité ✓)
└── .mise/
    └── tasks/
        ├── dev
        ├── test
        └── build
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
