---
layout: section
---

# 🔐 fnox

### Fort Knox pour vos secrets

---
transition: fade-out
---

# fnox — Où sont stockés les secrets ?

**fnox** est le compagnon de mise pour gérer vos secrets.

<v-clicks>

- 🔐 **Chiffrés dans git** — via `age`, AWS KMS, Azure KMS, GCP KMS  
  → commitables en toute sécurité

- ☁️ **Dans le cloud** — AWS Secrets Manager, Vault, 1Password,  
  Bitwarden, Infisical, GCP Secret Manager…

- 💻 **En local** — OS Keychain, KeePass, pass (GPG)

</v-clicks>

---
transition: fade-out
---

# Pourquoi fnox ?

<v-clicks>

- 🤝 **Team-friendly** — secrets chiffrés dans git, toute l'équipe peut déchiffrer

- 🌍 **Multi-environnements** — dev chiffré avec `age`, prod via AWS Secrets Manager

- 🔄 **Shell integration** — auto-chargement au `cd`, comme mise pour les runtimes

- 🔒 **Pas de vendor lock-in** — changer de provider sans modifier le code

</v-clicks>

<v-click>

### Installation

```bash
mise use -g fnox   # via mise (recommandé)
```

</v-click>

---
transition: fade-out
---

# fnox — Configuration

```toml {1-3|5-8|10-14|all}
# fnox.toml
[providers]
age = { type = "age", recipients = ["age1..."] }

[secrets]
# Chiffré dans git ✅
DATABASE_URL = { provider = "age", value = "YWdl..." }
API_KEY      = { default = "dev-key-12345" }

# Profil production → AWS Secrets Manager
[profiles.production.providers]
aws = { type = "aws-sm", region = "eu-west-1", prefix = "myapp/" }

[profiles.production.secrets]
DATABASE_URL = { provider = "aws", value = "database-url" }
```

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# fnox — Commandes essentielles

```bash
# Initialiser dans le projet
fnox init

# Gérer les secrets
fnox set DATABASE_URL "postgres://localhost/mydb"
fnox get DATABASE_URL

# Lancer avec les secrets chargés
fnox exec -- npm start
fnox exec --profile production -- ./deploy.sh
```

::right::

<v-click>

# Shell integration

Les secrets se chargent automatiquement au `cd` :

```bash
# Bash (~/.bashrc)
eval "$(fnox activate bash)"

# Zsh (~/.zshrc)
eval "$(fnox activate zsh)"

# Fish
fnox activate fish | source
```

</v-click>

<v-click>

> ✅ Comme mise charge les runtimes, fnox charge vos secrets — **sans rien faire**.

</v-click>
