---
layout: section
---

# 🔧 Tools

---
transition: fade-out
---

# Tools — Installer & utiliser

```bash {1-3|5-6|8-9|11-12|all}
# Installer des versions
mise install node@24
mise install node@24 python@3.12 go@1.22

# Définir pour le projet courant (.mise.toml)
mise use node@24

# Définir globalement (~/.config/mise/config.toml)
mise use --global node@24

# Exécuter avec une version précise
mise exec node@18 -- node --version
```

<v-click>

```bash
# Lancer un shell isolé
mise shell node@18
```

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Tools — Gérer les versions

### Lister & surveiller

```bash
mise list           # versions installées
mise ls-remote node # versions disponibles
mise outdated       # outils à mettre à jour
mise upgrade        # tout mettre à jour
```

::right::

<v-click>

### Dans `.mise.toml`

```toml
[tools]
node       = "24"       # version majeure
python     = "3.12.3"   # version exacte
go         = "latest"   # dernière stable
terraform  = "1.7"
jq         = "latest"
```

</v-click>

<v-click>

### Ajouter un plugin custom

```bash
mise plugin add scala \
  https://github.com/asdf-community/asdf-scala
```

</v-click>
