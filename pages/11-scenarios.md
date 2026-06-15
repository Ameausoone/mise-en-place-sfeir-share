---
layout: section
---

# 💡 Cas d'usage réels

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Cas 1 — Onboarding en 5 minutes

### Avant mise 😩

```bash
# README de 50 lignes…
# "Installe nvm, puis Node 18.x (pas 20 !)"
# "Active le virtualenv Python"
# "Installe Terraform 1.7.5 (pas 1.8 !)"
# "Copie .env.example → .env et remplis les valeurs"
# "Lance npm install, puis pip install…"
# Durée réelle : 2-3h, souvent cassé
```

::right::

<v-click>

### Avec mise 🚀

```bash
git clone git@github.com:sfeir/sfeir-conf
cd sfeir-conf

# mise trust (une seule fois)
mise trust

# Installe tout : Node 24, Python 3.12, Terraform 1.9…
mise install

# Configure et démarre
mise run setup
```

</v-click>

<v-click>

> ✅ **5 minutes**. Même sur une machine neuve.  
> Le `.mise.toml` commité est la documentation vivante de l'environnement.

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Cas 2 — Jongler entre projets

Le `cd` suffit — mise change tout automatiquement :

```text
$ cd ~/projects/sfeir-conf/frontend
$ node --version    → v24.x   (frontend/.mise.toml)

$ cd ~/projects/sfeir-conf/backend
$ python --version  → 3.12.x  (backend/.mise.toml)

$ cd ~/projects/sfeir-conf/infra
$ terraform --version  → 1.9.x
```

::right::

<v-click>

### Fini les oublis de `nvm use`

```bash
# Plus jamais ça :
nvm use 18
# Node.js v18.20.4 is not yet installed.
# nvm install 18
# ...
```

</v-click>

<v-click>

### Ni les conflits silencieux

```bash
# Plus jamais ça :
npm run build
# Error: requires Node >= 20
# (parce qu'on était sur le mauvais projet)
```

> ✅ mise active la bonne version **automatiquement** à chaque `cd`.

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Cas 3 — Secrets en équipe avec fnox

### Avant fnox 😬

<v-clicks>

- `DATABASE_URL` partagée par Slack, puis oublié de changer en prod
- `.env` committé par accident sur git
- Nouveau dev attend 2h que quelqu'un lui envoie les secrets
- Secrets différents entre dev / staging / prod → configuration manuelle

</v-clicks>

::right::

<v-click>

### Avec fnox ✅

```bash
# Alice chiffre les secrets dans sfeir-conf
fnox set DATABASE_URL "postgres://localhost/sfeir_conf"
fnox set STRIPE_KEY "sk_test_..."
git add fnox.toml && git commit -m "add secrets"
git push

# Bob clone le repo
git clone git@github.com:sfeir/sfeir-conf
cd sfeir-conf

# Bob déchiffre avec sa clé (déjà dans les recipients)
fnox exec -- uvicorn app.main:app
# DATABASE_URL et STRIPE_KEY sont disponibles ✓
```

</v-click>

<v-click>

> 🔐 Les secrets sont dans git, chiffrés, versionnés, auditables.  
> Pas de partage par messagerie, pas de `.env` oublié.

</v-click>

---
layout: center
transition: slide-up
---

# Cas 4 — Convention cross-équipe

Même interface dans **tous** les repos, quel que soit le langage :

<v-clicks>

```bash
mise run dev    # démarre le projet  (npm / uvicorn / go run…)
```

```bash
mise run test   # lance les tests    (jest / pytest / go test…)
```

```bash
mise run lint   # vérifie le code    (eslint / ruff / golangci…)
```

```bash
mise run ci     # pipeline complet   (lint + test + build)
```

</v-clicks>

<v-click>

> 🎯 Un développeur qui rejoint une nouvelle équipe sait **déjà** comment interagir avec le projet.

</v-click>
