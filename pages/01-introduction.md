---
transition: fade-out
---

# Le problème

<v-clicks>

- 😤 **Versions multiples** — Node 18 pour le projet A, Node 24 pour le projet B…
  nvm, n, fnm, volta… lequel choisir ?

- 🔑 **Variables d'environnement** — `.env`, direnv, secrets différents par projet…
  comment les partager sans les committer ?

- ⚙️ **Tâches répétitives** — Makefile, scripts shell, Taskfile…
  chaque projet a ses propres conventions.

</v-clicks>

<v-click>

<div class="mt-8 text-xl text-center font-bold text-green-400">
  → <strong>mise</strong> résout tout ça avec un seul outil 🎉
</div>

</v-click>

---
layout: center
class: text-center
transition: slide-up
---

# Qu'est-ce que mise ?

**mise** remplace **asdf**, **nvm**, **pyenv**, **rbenv**, **direnv** et bien d'autres  
avec un seul outil, écrit en Rust 🦀

---
transition: fade-out
---

# Un seul fichier pour tout

```text
# sfeir-conf/.mise.toml — à commiter dans git

[tools]                              ← versions des runtimes
node      = "24"
python    = "3.12"
terraform = "1.9"

[env]                                ← variables d'environnement
NODE_ENV  = "development"
API_URL   = "http://localhost:8000"

[tasks.dev]                          ← scripts & automatisations
run       = ["npm run dev", "uvicorn app.main:app --reload"]
depends   = ["setup"]

[hooks]                              ← réactions aux événements
enter     = "mise run check-deps"

[watch_files]                        ← surveillance de fichiers
"schema.graphql" = "codegen"
```

---
layout: two-cols
layoutClass: gap-16
transition: fade-out
---

## Ce que mise gère

<v-clicks>

- 🔧 **Tools** — Node, Python, Go, Ruby, Terraform…
- 🌿 **Env** — Variables d'environnement par répertoire
- 📋 **Tasks** — Scripts & automatisations reproductibles
- 🪝 **Hooks** — Réagir aux événements du projet

</v-clicks>

::right::

<v-click>

## Et en bonus

- 📦 **Registry** — 800+ outils, multiples backends
- 🔐 **fnox** — Secrets chiffrés dans git ou cloud

</v-click>

<v-click>

<div class="mt-8">

👉 [mise.jdx.dev](https://mise.jdx.dev)

</div>

</v-click>
