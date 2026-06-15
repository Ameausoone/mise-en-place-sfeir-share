---
layout: section
---

# 🪝 Hooks

---
transition: fade-out
---

# Hooks — Réagir aux événements

Déclenchés automatiquement quand vous **entrez ou quittez** un répertoire :

```toml {1-3|5-10|12-13|all}
# .mise.toml
[hooks]
enter = "echo '👋 Bienvenue dans {{config.project_root}}'"

# Vérifier que les dépendances sont à jour
enter = """
  if [ package.json -nt node_modules ]; then
    echo "⚠️  Dépendances obsolètes !"
    echo "    → lancez : mise run install"
  fi
"""

# Nettoyer les processus au départ
leave = "pkill -f 'npm run dev' || true"
```

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Hooks — pre / post tâche

```toml
[hooks]
pre_task  = """
  echo "🚀 Démarrage : $MISE_TASK_NAME"
  date
"""

post_task = """
  echo "✅ Terminé  : $MISE_TASK_NAME"
  echo "   Exit code : $MISE_TASK_EXIT_CODE"
"""
```

::right::

<v-click>

### Variables disponibles

| Variable | Valeur |
|---|---|
| `MISE_TASK_NAME` | Nom de la tâche |
| `MISE_TASK_EXIT_CODE` | Code de sortie |
| `config.project_root` | Racine du projet |

</v-click>

<v-click>

> 💡 Les hooks `pre_task` / `post_task` s'appliquent à **toutes** les tâches du projet.

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Hooks — `watch_files`

Déclenche une tâche automatiquement dès qu'un fichier change :

```toml
[tasks.codegen]
run = "npm run generate"

[tasks.sync-deps]
run = "npm install"

[watch_files]
"schema.graphql" = "codegen"
"package.json"   = "sync-deps"
```

```bash
# Démarrer le watcher
mise watch
# ↓ Modifiez schema.graphql…
# ✓ codegen relancé automatiquement !
```

::right::

<v-click>

### Combinaison hooks + watch

```toml
[watch_files]
".env.example" = "check-env"

[tasks.check-env]
run = """
  echo "⚠️  .env.example a changé !"
  echo "Vérifiez votre .env local."
"""
```

</v-click>

<v-click>

> ✅ Fini les _"n'oublie pas de relancer X après avoir modifié Y"_

</v-click>
