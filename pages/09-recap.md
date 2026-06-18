---
layout: center
transition: fade-out
---

# Ce qu'on a construit sur `sfeir-conf`

<v-clicks>

- 🔧 **Tools** — Node 24, Python 3.12, Terraform 1.9 versionnés dans `.mise.toml` + `mise.lock`
- 📦 **Registry + lockfile** — reproductibilité garantie en local et en CI (`jdx/mise-action`)
- 📋 **Tasks** — `dev`, `test`, `lint`, `ci` : même interface dans tous les repos de l'équipe
- 🪝 **Hooks** — activation automatique de l'environnement à chaque `cd`
- 🌍 **Env + monorepo** — héritage de config racine → frontend / backend / infra
- 🔐 **fnox** — secrets chiffrés dans git, déchiffrés automatiquement au `cd`

</v-clicks>

<v-click>

> 🎯 mise est le **point d'entrée unique** de l'environnement `sfeir-conf`, du développeur jusqu'à la CI.

</v-click>

---
layout: center
transition: fade-out
---

# En 3 commandes

<v-clicks>

```bash
git clone git@github.com:sfeir/sfeir-conf
```

```bash
mise install
```

```bash
mise run dev
```

</v-clicks>

<v-click>

> ✅ C'est tout. Outils, environnement, secrets, tâches — tout est dans le repo.

</v-click>

---
layout: center
class: text-center
---

# Merci ! 🙏

Documentation officielle : [mise.jdx.dev](https://mise.jdx.dev) · [fnox.jdx.dev](https://fnox.jdx.dev)

GitHub : [github.com/jdx/mise](https://github.com/jdx/mise) · [github.com/jdx/fnox](https://github.com/jdx/fnox)

<v-clicks>

- 📖 [Getting Started](https://mise.jdx.dev/getting-started.html)
- 📋 [Tasks](https://mise.jdx.dev/tasks/)
- 🪝 [Hooks](https://mise.jdx.dev/hooks.html)
- 🔐 [fnox docs](https://fnox.jdx.dev)

</v-clicks>

<PoweredBySlidev mt-10 />
