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

<div v-click class="mt-4 p-3 rounded bg-blue-500/10 border border-blue-500/30 text-sm">
  💡 <code>.mise.toml</code> remplace <code>.nvmrc</code>,
  <code>.tool-versions</code>, <code>Makefile</code> et <code>.envrc</code>
  en un <strong>seul fichier</strong>.
</div>
