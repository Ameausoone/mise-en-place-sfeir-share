---
layout: section
---

# 🔐 fnox

### Fort Knox pour vos secrets

---
layout: two-cols
layoutClass: gap-8
---

# fnox — Qu'est-ce que c'est ?

<div class="mt-2">

**fnox** est le compagnon de mise pour gérer vos secrets.
Il stocke les secrets de **deux façons** :

<div v-click class="mt-4 p-3 rounded border border-purple-400/30 bg-purple-400/5">
  <div class="font-bold mb-1">🔐 Chiffrés dans git</div>
  <div class="text-sm opacity-80">
    via <code>age</code>, AWS KMS, Azure KMS, GCP KMS<br/>
    → commitables en toute sécurité
  </div>
</div>

<div v-click class="mt-3 p-3 rounded border border-blue-400/30 bg-blue-400/5">
  <div class="font-bold mb-1">☁️ Dans le cloud</div>
  <div class="text-sm opacity-80">
    AWS Secrets Manager, Vault, 1Password,<br/>
    Bitwarden, Infisical, GCP Secret Manager…
  </div>
</div>

<div v-click class="mt-3 p-3 rounded border border-green-400/30 bg-green-400/5">
  <div class="font-bold mb-1">💻 En local</div>
  <div class="text-sm opacity-80">
    OS Keychain, KeePass, pass (GPG)
  </div>
</div>

</div>

::right::

<div class="mt-2">

### Pourquoi fnox ?

<div v-click>

- 🤝 **Team-friendly** — secrets chiffrés dans git, tout le monde peut déchiffrer
- 🌍 **Multi-environnements** — dev chiffré, prod via AWS SM
- 🔄 **Shell integration** — auto-chargement au `cd`
- 🔒 **Pas de vendor lock-in** — changer de provider sans modifier le code

</div>

<div v-click class="mt-6">

### Installation

```bash
# Via mise (recommandé)
mise use -g fnox

# Via cargo
cargo install fnox
```

</div>

</div>

---

# fnox — Configuration & utilisation

<div class="grid grid-cols-2 gap-6">

<div>

### `fnox.toml`

```toml
[providers]
# Chiffrement local avec age (clé SSH !)
age = { type = "age", recipients = ["age1..."] }

[secrets]
# Secret chiffré dans git (safe to commit ✓)
DATABASE_URL = { provider = "age", value = "YWdl..." }

# Valeur par défaut (plain, dev seulement)
API_KEY = { default = "dev-key-12345" }

# Profil production → AWS Secrets Manager
[profiles.production.providers]
aws = { type = "aws-sm", region = "eu-west-1", prefix = "myapp/" }

[profiles.production.secrets]
DATABASE_URL = { provider = "aws", value = "database-url" }
```

</div>

<div>

### Commandes essentielles

```bash
# Initialiser dans le projet
fnox init

# Gérer les secrets
fnox set DATABASE_URL "postgres://localhost/mydb"
fnox get DATABASE_URL

# Lancer une commande avec les secrets chargés
fnox exec -- npm start
fnox exec --profile production -- ./deploy.sh
```

<div v-click class="mt-4">

### Shell integration (auto-load)

```bash
# Bash (~/.bashrc)
eval "$(fnox activate bash)"

# Zsh (~/.zshrc)
eval "$(fnox activate zsh)"

# Fish (~/.config/fish/config.fish)
fnox activate fish | source
```

<div class="mt-2 p-3 rounded bg-purple-500/10 border border-purple-500/30 text-sm">
  ✅ Les secrets se chargent automatiquement au <code>cd</code>,
  comme mise charge les runtimes.
</div>

</div>

</div>

</div>
