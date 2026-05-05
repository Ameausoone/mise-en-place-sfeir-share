---
layout: section
---

# 🌍 Environnement & Configuration

---
layout: two-cols
layoutClass: gap-8
---

# Variables d'environnement

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

::right::

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
├── .mise.toml          ← Node 24, env communs
├── backend/
│   └── .mise.toml      ← Python 3.12, PORT=8000
└── frontend/
    └── .mise.toml      ← PORT=3000
```

> 💡 Chaque sous-répertoire hérite et peut surcharger la configuration du parent.

---
layout: center
---

# Migration depuis d'autres outils

<v-clicks>

- **asdf** — Migration transparente : `mise install` lit `.tool-versions`, compatible avec tous les plugins asdf

- **nvm / fnm** — `mise use node@$(cat .nvmrc)` importe le `.nvmrc` ; mise le lit aussi automatiquement

- **direnv** — mise gère les `.envrc` ou utilise `[env]` dans `.mise.toml`, compatible direnv

</v-clicks>

```bash
# Migration depuis .tool-versions (asdf)
mise install  # lit automatiquement .tool-versions
``` {v-click}

---
layout: two-cols
layoutClass: gap-8
---

# Pourquoi mise est rapide ? 🦀

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

::right::

<v-click>

### Shims vs PATH

```
asdf utilise des "shims" (wrappers)
→ chaque commande passe par un script intermédiaire

mise modifie directement le PATH
→ exécution directe, sans overhead
```

> ✅ 50x plus rapide qu'asdf au démarrage du shell

</v-click>

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

```bash
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

<v-click>

> 💡 `.mise.toml` remplace `.nvmrc`, `.tool-versions`, `Makefile` et `.envrc` en un **seul fichier**.

</v-click>
