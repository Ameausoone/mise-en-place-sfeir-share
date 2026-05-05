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

**fnox** est le compagnon de mise pour gérer vos secrets.
Il stocke les secrets de **deux façons** :

<v-clicks>

- 🔐 **Chiffrés dans git** — via `age`, AWS KMS, Azure KMS, GCP KMS → commitables en toute sécurité

- ☁️ **Dans le cloud** — AWS Secrets Manager, Vault, 1Password, Bitwarden, Infisical, GCP Secret Manager…

- 💻 **En local** — OS Keychain, KeePass, pass (GPG)

</v-clicks>

::right::

### Pourquoi fnox ?

<v-clicks>

- 🤝 **Team-friendly** — secrets chiffrés dans git, tout le monde peut déchiffrer
- 🌍 **Multi-environnements** — dev chiffré, prod via AWS SM
- 🔄 **Shell integration** — auto-chargement au `cd`
- 🔒 **Pas de vendor lock-in** — changer de provider sans modifier le code

</v-clicks>

<v-click>

### Installation

```bash
# Via mise (recommandé)
mise use -g fnox

# Via cargo
cargo install fnox
```

</v-click>

---
layout: two-cols
layoutClass: gap-8
---

# fnox — Configuration & utilisation

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

::right::

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

<v-click>

### Shell integration (auto-load)

```bash
# Bash (~/.bashrc)
eval "$(fnox activate bash)"

# Zsh (~/.zshrc)
eval "$(fnox activate zsh)"

# Fish (~/.config/fish/config.fish)
fnox activate fish | source
```

> ✅ Les secrets se chargent automatiquement au `cd`, comme mise charge les runtimes.

</v-click>
