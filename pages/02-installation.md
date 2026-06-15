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

```
✓ mise version: 2024.x.x
✓ shell: bash
✗ nvm detected — can conflict with mise
✓ ~/.local/share/mise/shims is in PATH
```

</v-click>
