---
layout: section
---

# 📋 Tasks

---
transition: fade-out
---

# Tasks — Définir dans `.mise.toml`

```toml {1-3|5-8|10-12|14-17|all}
[tasks.install]
run         = "npm install"
description = "Installer les dépendances"

[tasks.dev]
run         = "npm run dev"
description = "Démarrer le serveur de développement"
depends     = ["install"]

[tasks.lint]
run         = ["eslint src/", "prettier --check src/"]
description = "Vérifier le style du code"

[tasks.build]
run         = "npm run build"
description = "Build de production"
depends     = ["lint", "test"]
```

---
transition: fade-out
---

# Tasks — Exécuter

```bash {1-2|4-5|7-8|10-11|all}
# Lister toutes les tâches
mise tasks

# Lancer une tâche
mise run dev

# Passer des arguments
mise run test -- --watch

# Exécution parallèle
mise run --jobs 4 lint test
```

<v-click>

```bash
# Voir ce qui serait exécuté (dry-run)
mise run --dry-run build
```

</v-click>

---
transition: fade-out
---

# Tasks — Variables & dépendances

```toml {1-6|8-10|all}
[tasks.deploy]
run = """
  echo "Deploying to $ENVIRONMENT"
  docker build -t myapp:$VERSION .
"""
env     = { ENVIRONMENT = "production" }

# Graphe de dépendances : lint + test en parallèle,
# puis build si les deux réussissent
depends = ["test", "build"]
```

<v-click>

```bash
mise run deploy
# ↳ [lint] ✓   [test] ✓   (parallèle)
# ↳ [build] ✓
# ↳ [deploy] 🚀
```

</v-click>

---
transition: fade-out
---

# Tasks — Tâches fichiers

Des **scripts exécutables** dans `.mise/tasks/`, dans n'importe quel langage :

```
.mise/
└── tasks/
    ├── dev          ← mise run dev
    ├── test         ← mise run test
    └── db/
        ├── migrate  ← mise run db:migrate
        └── seed     ← mise run db:seed
```

<v-click>

```bash
#!/usr/bin/env bash
# .mise/tasks/dev
#MISE description="Démarrer le serveur de dev"
#MISE depends=["install"]

set -euo pipefail
npm run dev
```

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Tasks — Script Python

N'importe quel langage est supporté :

```python
#!/usr/bin/env python3
# .mise/tasks/generate-report
#MISE description="Générer le rapport mensuel"

import json, datetime

data = {"date": str(datetime.date.today())}
print(json.dumps(data, indent=2))
```

```bash
# Rendre exécutable puis lancer :
chmod +x .mise/tasks/generate-report
mise run generate-report
```

::right::

<v-click>

### Avantages des tâches fichiers

- Coloration syntaxique dans l'éditeur
- Testables indépendamment
- Versionnables dans git
- N'importe quel langage

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Tasks — `mise watch`

Relancer automatiquement à chaque modification :

```bash
# Relancer à chaque changement
mise watch -t test

# Surveiller des fichiers spécifiques
mise watch -t build -- src/**/*.ts
```

```toml
[tasks.test]
run     = "npm test"
sources = ["src/**/*.ts", "tests/**/*.ts"]
outputs = ["coverage/"]
```

::right::

<v-click>

# Arguments & `usage`

```toml
[tasks.deploy]
#USAGE flag "-e --env <env>" help="Target env"
#USAGE flag "--dry-run"      help="Simulate only"
run = """
  echo "Deploy to: $usage_env"
  [ "$usage_dry_run" = "true" ] \
    && echo "(dry-run)" || true
"""
```

```bash
mise run deploy --env staging
mise run deploy --env prod --dry-run
mise run deploy --help  # aide intégrée
```

</v-click>
