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
layout: center
class: text-center
transition: fade-out
---

# Un seul fichier pour tout

```toml
# .mise.toml — à commiter dans git
[tools]
node      = "24"        # ← versions des runtimes
python    = "3.12"
terraform = "1.9"

[env]
NODE_ENV  = "development"   # ← variables d'environnement
_.file    = ".env"

[tasks.dev]
run       = "npm run dev"   # ← scripts & automatisations
depends   = ["install"]

[hooks]
enter     = "mise run check-deps"  # ← réactions aux événements

[watch_files]
"schema.graphql" = "codegen"  # ← surveillance de fichiers
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
