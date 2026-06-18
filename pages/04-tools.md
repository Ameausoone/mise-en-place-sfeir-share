---
layout: section
---

# 🔧 Tools

---
transition: fade-out
---

# Tools — Installer & utiliser

```bash {1-3|5-6|8-9|11-12|14-15|all}
# Dans sfeir-conf/
mise install   # installe node@24, python@3.12, terraform@1.9
mise install node@24 python@3.12 terraform@1.9

# Définir pour le projet courant (.mise.toml)
mise use node@24

# Définir globalement (~/.config/mise/config.toml)
mise use --global node@24

# Exécuter avec une version précise
mise exec node@18 -- node --version

# Lancer un shell isolé
mise shell node@18
```

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
node       = "24"       # frontend Vite
python     = "3.12"     # backend FastAPI
terraform  = "1.9"      # infra AWS
gh         = "latest"   # GitHub CLI
```

</v-click>

<v-click>

### Ajouter un plugin custom

```bash
mise plugin add scala \
  https://github.com/asdf-community/asdf-scala
```

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: slide-up
---

# Onboarding `sfeir-conf` en 5 minutes

### Avant mise

```bash
# README de 50 lignes...
# "Installe nvm, puis Node 18.x (pas 20 !)"
# "Active le virtualenv Python"
# "Installe Terraform 1.7.5 (pas 1.8 !)"
# "Copie .env.example et remplis les valeurs"
# "Lance npm install, puis pip install..."
# Durée réelle : 2-3h, souvent cassé
```

::right::

<v-click>

### Avec mise

```bash
git clone git@github.com:sfeir/sfeir-conf
cd sfeir-conf

mise trust
mise install
mise run setup
```

</v-click>

<v-click>

> ✅ **5 minutes**. Même sur une machine neuve. Le `.mise.toml` commité est la documentation vivante de l'environnement.

</v-click>
