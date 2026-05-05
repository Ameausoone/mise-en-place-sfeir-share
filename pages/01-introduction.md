---
transition: fade-out
---

# Le problème

<div class="grid grid-cols-3 gap-6 mt-8">

<div v-click class="p-4 rounded-lg border border-red-400/30 bg-red-400/5">
  <div class="text-2xl mb-2">😤</div>
  <div class="font-bold mb-2">Versions multiples</div>
  <div class="text-sm opacity-75">
    Node 18 pour le projet A, Node 20 pour le projet B…<br/>
    nvm, n, fnm, volta… lequel choisir ?
  </div>
</div>

<div v-click class="p-4 rounded-lg border border-yellow-400/30 bg-yellow-400/5">
  <div class="text-2xl mb-2">🔑</div>
  <div class="font-bold mb-2">Variables d'environnement</div>
  <div class="text-sm opacity-75">
    .env, direnv, des secrets différents par projet…
    comment les partager sans les committer ?
  </div>
</div>

<div v-click class="p-4 rounded-lg border border-blue-400/30 bg-blue-400/5">
  <div class="text-2xl mb-2">⚙️</div>
  <div class="font-bold mb-2">Tâches répétitives</div>
  <div class="text-sm opacity-75">
    Makefile, scripts shell, Taskfile…
    chaque projet a ses propres conventions.
  </div>
</div>

</div>

<div v-click class="mt-10 text-center text-xl">
  → <strong>mise</strong> résout tout ça avec un seul outil 🎉
</div>

---
layout: center
class: text-center
---

# Qu'est-ce que mise ?

<div class="mt-6 text-lg opacity-80 max-w-2xl mx-auto">

**mise** est un gestionnaire d'environnement de développement polyglotte.

Il remplace **asdf**, **nvm**, **pyenv**, **rbenv**, **direnv** et bien d'autres
avec un seul outil rapide, écrit en Rust 🦀.

</div>

<div class="grid grid-cols-4 gap-4 mt-8">
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">🔧</div>
    <div class="font-bold">Tools</div>
    <div class="text-sm opacity-70">Node, Python, Go, Ruby…</div>
  </div>
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">🌿</div>
    <div class="font-bold">Env</div>
    <div class="text-sm opacity-70">Variables par répertoire</div>
  </div>
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">📋</div>
    <div class="font-bold">Tasks</div>
    <div class="text-sm opacity-70">Scripts & automatisations</div>
  </div>
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">🪝</div>
    <div class="font-bold">Hooks</div>
    <div class="text-sm opacity-70">Réagir aux événements</div>
  </div>
</div>

<div class="grid grid-cols-2 gap-4 mt-4 max-w-sm mx-auto">
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">📦</div>
    <div class="font-bold">Registry</div>
    <div class="text-sm opacity-70">800+ outils disponibles</div>
  </div>
  <div v-click class="p-3 rounded border border-primary/30">
    <div class="text-3xl mb-2">🔐</div>
    <div class="font-bold">fnox</div>
    <div class="text-sm opacity-70">Secrets chiffrés en git</div>
  </div>
</div>

<div v-click class="mt-6">
  <a href="https://mise.jdx.dev" target="_blank" class="text-primary">mise.jdx.dev</a>
</div>
