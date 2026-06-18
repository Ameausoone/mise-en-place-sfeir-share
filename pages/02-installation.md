---
transition: fade-out
---

# Installation

<v-clicks>

- **Linux / macOS** (recommandé)
  ```bash
  curl https://mise.run | sh
  ```

- **Homebrew** (macOS)
  ```bash
  brew install mise
  ```

- **Windows**
  ```bash
  winget install jdx.mise
  ```

- **Rust / cargo**
  ```bash
  cargo install mise
  ```

</v-clicks>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Activation dans votre shell

```bash {1-2|4-5|7-8|all}
# Bash (~/.bashrc)
eval "$(mise activate bash)"

# Zsh (~/.zshrc)
eval "$(mise activate zsh)"

# Fish (~/.config/fish/config.fish)
mise activate fish | source
```

<v-click>

> 💡 Après activation, mise charge automatiquement le bon environnement selon le répertoire.

</v-click>

::right::

<v-click>

# `mise doctor`

Première commande à lancer en cas de problème :

```bash
mise doctor
```

</v-click>

<v-click>

```shell
✓ mise version: 2024.x.x
✓ shell: bash
✗ nvm detected — can conflict with mise
✓ ~/.local/share/mise/shims is in PATH
```

</v-click>

---
transition: fade-out
---

# `mise trust` — Modèle de sécurité

La première fois que vous `cd` dans un projet avec `.mise.toml` :

<v-click>

```shell
cd ~/projects/sfeir-conf

⚠️  mise a trouvé un fichier .mise.toml non approuvé
    /home/alice/projects/sfeir-conf/.mise.toml
    Voulez-vous lui faire confiance ? [y/N]
```

</v-click>

<v-click>

```bash
# Approuver explicitement
mise trust

# Approuver tous les projets d'un répertoire
mise trust ~/projects/

# Révoquer la confiance
mise untrust
```

</v-click>

<v-click>

> 🔒 Pourquoi ? Un `.mise.toml` peut exécuter des commandes (hooks, tasks). Faire confiance à un repo = accepter que ses scripts s'exécutent automatiquement.

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: slide-up
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
~/.config/mise/config.toml   <- config globale
      |  hérite
~/projects/sfeir-conf/
├── .mise.toml       <- node@24, python@3.12, terraform@1.9
├── frontend/
│   └── .mise.toml   <- PORT=3000
└── backend/
    └── .mise.toml   <- PORT=8000
```

</v-click>

<v-click>

> 💡 Sur `sfeir-conf`, chaque développeur embarque ses outils globaux (`gh`, `jq`) et le repo surcharge avec les versions exactes du projet.

</v-click>
