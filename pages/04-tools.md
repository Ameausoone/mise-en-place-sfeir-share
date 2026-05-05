---
layout: section
---

# 🔧 Tools

---
layout: two-cols
layoutClass: gap-8
---

# Tools — Commandes essentielles

### Installer & utiliser

```bash
# Installer une version
mise install node@24
mise install python@latest
mise install node@24 python@3.12 go@1.22

# Définir pour le projet courant (.mise.toml)
mise use node@24

# Définir globalement (~/.config/mise/config.toml)
mise use --global node@24

# Exécuter une commande avec une version précise
mise exec node@18 -- node --version

# Lancer un shell avec les outils chargés
mise shell node@18
```

### Lister

```bash
mise list           # versions installées
mise ls-remote node # versions disponibles
mise outdated       # outils à mettre à jour
mise upgrade        # tout mettre à jour
```

::right::

### Registre & plugins

```bash
# Explorer les outils disponibles
mise registry

# Rechercher un outil
mise registry | grep terraform

# Ajouter un plugin asdf existant
mise plugin add scala \
  https://github.com/asdf-community/asdf-scala

# Ajouter un plugin custom
mise plugin add my-tool \
  https://github.com/user/mise-my-tool
```

### Dans `.mise.toml`

```toml
[tools]
node       = "24"          # version majeure
python     = "3.12.3"      # version exacte
go         = "latest"      # dernière stable
terraform  = "1.7"
awscli     = "2"
jq         = "latest"
```
