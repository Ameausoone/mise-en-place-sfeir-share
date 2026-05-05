---
layout: section
---

# 📋 Tasks

---
layout: two-cols
layoutClass: gap-8
---

# Tasks — Dans `.mise.toml`

### Définir des tâches

```toml
[tasks.install]
run         = "npm install"
description = "Installer les dépendances"

[tasks.dev]
run         = "npm run dev"
description = "Démarrer le serveur de développement"
depends     = ["install"]

[tasks.test]
run         = "npm test -- --coverage"
description = "Tests avec couverture de code"

[tasks.lint]
run         = ["eslint src/", "prettier --check src/"]
description = "Vérifier le style du code"

[tasks.build]
run         = "npm run build"
description = "Build de production"
depends     = ["lint", "test"]
```

::right::

### Exécuter

```bash
# Lister toutes les tâches
mise tasks
mise run --list

# Lancer une tâche
mise run dev
mise run test

# Passer des arguments
mise run test -- --watch

# Exécution parallèle
mise run --jobs 4 lint test

# Voir ce qui serait exécuté
mise run --dry-run build
```

### Variables dans les tâches

```toml
[tasks.deploy]
run = """
  echo "Deploying to $ENVIRONMENT"
  docker build -t myapp:$VERSION .
"""
env   = { ENVIRONMENT = "production" }
depends = ["test", "build"]
```

---
layout: two-cols
layoutClass: gap-8
---

# Tasks — Tâches fichiers

### Scripts dans `.mise/tasks/`

Des **scripts shell exécutables** directement dans le repo,
sans les écrire dans le `.mise.toml` :

```
.mise/
└── tasks/
    ├── dev          ← mise run dev
    ├── test         ← mise run test
    ├── build        ← mise run build
    └── db/
        ├── migrate  ← mise run db:migrate
        └── seed     ← mise run db:seed
```

```bash
#!/usr/bin/env bash
# .mise/tasks/dev
#MISE description="Démarrer le serveur de dev"
#MISE depends=["install"]

set -euo pipefail
npm run dev
```

::right::

<v-click>

### Avantages des tâches fichiers

- Écrites dans n'importe quel langage (`bash`, `python`, `node`…)
- Coloration syntaxique dans l'éditeur
- Testables et versionnables indépendamment

```python
#!/usr/bin/env python3
# .mise/tasks/generate-report
#MISE description="Générer le rapport mensuel"

import json, datetime

data = {"date": str(datetime.date.today())}
print(json.dumps(data, indent=2))
```

```bash
# Rendre exécutable
chmod +x .mise/tasks/generate-report

# Puis simplement :
mise run generate-report
```

</v-click>

---
layout: two-cols
layoutClass: gap-8
---

# Tasks — Options avancées

### `mise watch` — Rechargement automatique

```bash
# Relancer la tâche à chaque modification
mise watch -t test

# Surveiller des fichiers spécifiques
mise watch -t build -- src/**/*.ts
```

```toml
# Configurer dans .mise.toml
[tasks.test]
run     = "npm test"
sources = ["src/**/*.ts", "tests/**/*.ts"]
outputs = ["coverage/"]
```

### Dépendances & graphe

```toml
[tasks.ci]
# Lance lint et test en parallèle,
# puis build si les deux réussissent
depends = ["lint", "test"]
run     = "npm run build"
```

::right::

<v-click>

### Arguments & `usage`

```toml
[tasks.deploy]
#USAGE flag "-e --env <env>" help="Target environment"
#USAGE flag "--dry-run"      help="Simulate only"
run = """
  echo "Deploy to: $usage_env"
  if [ "$usage_dry_run" = "true" ]; then
    echo "(dry-run, nothing deployed)"
  fi
"""
```

```bash
mise run deploy --env staging
mise run deploy --env prod --dry-run
```

### `mise run` — aide intégrée

```bash
mise run --help          # aide globale
mise run deploy --help   # aide de la tâche
```

</v-click>
