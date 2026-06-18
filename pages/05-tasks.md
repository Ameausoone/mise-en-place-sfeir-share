---
layout: section
---

# 📋 Tasks

---
transition: fade-out
---

# Tasks — Définir dans `.mise.toml`

```toml {1-3|5-8|10-12|14-17|all}
[tasks.setup]
run         = ["npm install", "pip install -r requirements.txt"]
description = "Installer toutes les dépendances"

[tasks.dev]
run         = ["npm run dev", "uvicorn app.main:app --reload"]
description = "Démarrer frontend (Vite) + backend (FastAPI)"
depends     = ["setup"]

[tasks.lint]
run         = ["eslint src/", "ruff check app/"]
description = "Linter frontend (ESLint) + backend (Ruff)"

[tasks.build]
run         = "docker build -t sfeir-conf ."
description = "Build Docker"
depends     = ["lint", "test"]
```

---
transition: fade-out
---

# Tasks — Exécuter

```bash {1-2|4-5|7-8|10-11|13-14|all}
# Lister toutes les tâches
mise tasks

# Lancer une tâche
mise run dev

# Passer des arguments
mise run test -- --watch

# Exécution parallèle
mise run --jobs 4 lint test

# Voir ce qui serait exécuté (dry-run)
mise run --dry-run build
```

---
transition: fade-out
---

# Tasks — Variables & dépendances

```toml {1-4|6-9|11-13|all}
[tasks.test]
run         = ["npm test", "pytest"]
description = "Tests frontend (Jest) + backend (pytest)"

[tasks.deploy]
run         = "terraform apply"
description = "Déployer sur AWS"
depends     = ["build"]

# Graphe de dépendances : lint + test en parallèle,
# puis build, puis deploy
depends = ["lint", "test"]
```

<v-click>

```text
$ mise run deploy
  ↳ [lint] ✓   [test] ✓   (parallèle)
  ↳ [build] ✓
  ↳ [deploy] 🚀
```

</v-click>

---
transition: fade-out
---

# Tasks — Tâches fichiers

Des **scripts exécutables** dans `.mise/tasks/`, dans n'importe quel langage :

```text
sfeir-conf/.mise/tasks/
├── dev      ← mise run dev
├── test     ← mise run test
└── deploy   ← mise run deploy
```

<v-click>

```bash
#!/usr/bin/env bash
# sfeir-conf/.mise/tasks/dev
#MISE description="Frontend Vite + Backend FastAPI"
#MISE depends=["setup"]

set -euo pipefail
npm run dev &
uvicorn app.main:app --reload
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
run     = "pytest && npm test"
sources = ["backend/app/**/*.py", "frontend/src/**/*.ts"]
outputs = ["coverage/"]
```

::right::

<v-click>

# Arguments & `usage`

```toml
[tasks.deploy]
#USAGE flag "-e --env <env>" help="Target environment (dev/staging/prod)"
#USAGE flag "--dry-run"      help="Simulate deployment"
run = """
  echo "Deploying sfeir-conf to $usage_env"
  [ "$usage_dry_run" = "true" ] && echo "(dry-run)" || terraform apply
"""
```

```bash
mise run deploy --env staging
mise run deploy --env prod --dry-run
mise run deploy --help  # aide intégrée
```

</v-click>

---
layout: center
transition: slide-up
---

# Convention cross-équipe — `sfeir-conf`

Même interface dans **tous** les repos, quel que soit le langage :

<v-clicks>

```bash
mise run dev    # démarre le projet  (npm / uvicorn / go run...)
```

```bash
mise run test   # lance les tests    (jest / pytest / go test...)
```

```bash
mise run lint   # vérifie le code    (eslint / ruff / golangci...)
```

```bash
mise run ci     # pipeline complet   (lint + test + build)
```

</v-clicks>

<v-click>

> 🎯 Un développeur qui rejoint une nouvelle équipe sait **déjà** comment interagir avec le projet.

</v-click>
