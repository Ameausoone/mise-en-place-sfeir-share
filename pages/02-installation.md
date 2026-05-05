---
layout: two-cols
layoutClass: gap-8
---

# Installation

### Linux / macOS

```bash
# Via curl (recommandé)
curl https://mise.run | sh

# Via Homebrew
brew install mise

# Via cargo
cargo install mise

# Via winget (Windows)
winget install jdx.mise
```

::right::

### Activation dans votre shell

```bash
# Bash (~/.bashrc)
eval "$(mise activate bash)"

# Zsh (~/.zshrc)
eval "$(mise activate zsh)"

# Fish (~/.config/fish/config.fish)
mise activate fish | source

# Nushell (~/.config/nushell/config.nu)
mise activate nu | save -f ~/.config/nushell/mise.nu
```

<v-click>

### `mise doctor` — diagnostics

```bash
mise doctor
# ✓ mise version: 2024.x.x
# ✓ shell: bash
# ✗ nvm detected — can conflict with mise, disable it
# ✓ ~/.local/share/mise/shims is in PATH
```

</v-click>

<!--
L'installation est rapide et l'activation dans le shell permet à mise de charger
automatiquement le bon environnement selon le répertoire courant.
`mise doctor` est la première commande à lancer en cas de problème.
-->
