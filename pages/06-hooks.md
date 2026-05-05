---
layout: section
---

# 🪝 Hooks

---
layout: two-cols
layoutClass: gap-8
---

# Hooks — Réagir aux événements

### Hooks de répertoire

Déclenchés automatiquement quand vous **entrez ou quittez** un répertoire :

```toml
# .mise.toml
[hooks]
enter = "echo '👋 Bienvenue dans {{config.project_root}}'"
leave = "echo '�� À bientôt !'"
```

### Cas d'usage concrets

```toml
[hooks]
# Vérifier que les dépendances sont à jour
enter = """
  if [ package.json -nt node_modules ]; then
    echo "⚠️  Dépendances obsolètes, lancez : mise run install"
  fi
"""

# Nettoyer les processus au départ
leave = "pkill -f 'npm run dev' || true"
```

::right::

<v-click>

### Hooks de tâches — `pre` / `post`

```toml
[tasks.deploy]
run  = "./scripts/deploy.sh"

[hooks]
# Avant chaque tâche
pre_task  = """
  echo "🚀 Démarrage : $MISE_TASK_NAME"
  date
"""

# Après chaque tâche (succès ou échec)
post_task = """
  echo "✅ Terminé  : $MISE_TASK_NAME"
  echo "   Exit code : $MISE_TASK_EXIT_CODE"
"""
```

### Variables disponibles dans les hooks

| Variable | Valeur |
|---|---|
| `MISE_TASK_NAME` | Nom de la tâche |
| `MISE_TASK_EXIT_CODE` | Code de sortie |
| `config.project_root` | Racine du projet |

</v-click>

---
layout: two-cols
layoutClass: gap-8
---

# Hooks — `watch_files`

### Surveiller des fichiers

`watch_files` déclenche une tâche automatiquement dès qu'un fichier change :

```toml
# .mise.toml

[tasks.codegen]
run         = "npm run generate"
description = "Regénérer le code depuis le schéma"

[tasks.sync-deps]
run         = "npm install"
description = "Synchroniser les dépendances"

[watch_files]
# Regénérer si le schéma GraphQL change
"schema.graphql" = "codegen"

# Réinstaller si package.json change
"package.json"   = "sync-deps"
```

::right::

<v-click>

### Activer la surveillance

```bash
# Démarrer le watcher
mise watch

# Tous les watch_files sont surveillés
# Les tâches se déclenchent automatiquement
# ↓ Modifiez schema.graphql…
# ✓ codegen relancé automatiquement !
```

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

> ✅ Fini les _"n'oublie pas de relancer X après avoir modifié Y"_

</v-click>
