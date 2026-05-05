---
layout: center
class: text-center
---

# Récapitulatif

<div class="grid grid-cols-2 gap-6 mt-4 text-left max-w-3xl mx-auto">

<div v-click>

### ✅ Ce que mise (+ fnox) fait

- 🔧 **Tools** — gère Node, Python, Go, Terraform…
- 📦 **Registry** — 800+ outils, multiples backends
- 🌿 **Env** — variables d'environnement par projet
- 📋 **Tasks** — scripts reproductibles avec dépendances
- 🪝 **Hooks** — réagir aux événements du projet
- 🔐 **fnox** — secrets chiffrés dans git ou cloud
- ⚡ **Rapide** — écrit en Rust, ~4ms de démarrage

</div>

<div v-click>

### 🚀 Pour démarrer

```bash
# 1. Installer mise
curl https://mise.run | sh

# 2. Activer dans votre shell
echo 'eval "$(mise activate bash)"' >> ~/.bashrc

# 3. Créer un .mise.toml
mise use node@20 python@3.12

# 4. Tout installer
mise install

# 5. Lancer une tâche
mise run dev
```

</div>

</div>

---
layout: center
class: text-center
---

# Merci ! 🙏

<div class="mt-6 text-lg opacity-80">

Documentation officielle : [mise.jdx.dev](https://mise.jdx.dev) · [fnox.jdx.dev](https://fnox.jdx.dev)

GitHub : [github.com/jdx/mise](https://github.com/jdx/mise) · [github.com/jdx/fnox](https://github.com/jdx/fnox)

</div>

<div class="mt-8 grid grid-cols-4 gap-4 max-w-2xl mx-auto text-sm">
  <a href="https://mise.jdx.dev/getting-started.html" target="_blank" class="p-3 rounded border border-primary/30 hover:border-primary/60 transition-colors">
    📖 Getting Started
  </a>
  <a href="https://mise.jdx.dev/tasks/" target="_blank" class="p-3 rounded border border-primary/30 hover:border-primary/60 transition-colors">
    📋 Tasks
  </a>
  <a href="https://mise.jdx.dev/hooks.html" target="_blank" class="p-3 rounded border border-primary/30 hover:border-primary/60 transition-colors">
    🪝 Hooks
  </a>
  <a href="https://fnox.jdx.dev" target="_blank" class="p-3 rounded border border-primary/30 hover:border-primary/60 transition-colors">
    🔐 fnox
  </a>
</div>

<PoweredBySlidev mt-10 />
