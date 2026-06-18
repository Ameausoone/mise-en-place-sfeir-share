---
layout: section
---

# 📦 Registry

---
transition: fade-out
---

# Registry — 800+ outils disponibles

### Explorer & installer

```bash {1-2|4-5|7-8|all}
# Lister tous les outils disponibles
mise registry

# Rechercher un outil
mise registry | grep -i terraform

# Installer plusieurs outils d'un coup
mise use node terraform kubectl helm awscli gh
```

<v-click>

### Outils utilisés dans sfeir-conf

| Outil | Backend |
|-------|---------|
| `node`, `python`, `terraform` | core / asdf |
| `gh`, `jq`, `ruff`            | aqua / ubi  |

</v-click>

---
transition: fade-out
---

# Les backends

mise installe les outils via **plusieurs sources** selon leur nature :

<v-clicks>

```toml
[tools]
# asdf — compatible 800+ plugins
node = "24"
```

```toml
[tools]
# npm / pipx — packages isolés
"npm:prettier"   = "latest"
"pipx:ansible"   = "latest"
```

```toml
[tools]
# cargo — compilé depuis crates.io
"cargo:ripgrep" = "14"

# go — binaire Go
"go:mvdan.cc/gofumpt" = "latest"
```

</v-clicks>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Backends GitHub & GitLab

Installer **n'importe quel binaire** depuis GitHub ou GitLab Releases :

```toml
[tools]
# GitHub Releases (public)
"github:cli/cli"         = "latest"
"github:sharkdp/bat"     = "latest"
"github:BurntSushi/ripgrep" = "14"

# GitLab Releases (public)
"gitlab:inkscape/inkscape" = "latest"
```

<v-click>

### GitHub Enterprise

```toml
[tools]
"github:myorg/internal-tool" = { version = "latest", api_url = "https://github.mycompany.com/api/v3" }
```

```bash
export GITHUB_TOKEN="ghp_..."   # ou MISE_GITHUB_TOKEN
```

</v-click>

::right::

<v-click>

### GitLab self-hosted

```toml
[tools]
"gitlab:myorg/mytool" = { version = "latest", api_url = "https://gitlab.mycompany.com/api/v4" }
```

```bash
export MISE_GITLAB_TOKEN="glpat-..."
# ou MISE_GITLAB_ENTERPRISE_TOKEN pour self-hosted
```

</v-click>

<v-click>

### Forgejo (Gitea)

```toml
[tools]
"forgejo:myorg/mytool" = { version = "latest", api_url = "https://forgejo.mycompany.com/api/v1" }
```

</v-click>

---
transition: fade-out
---

# Backends pour registres privés

<v-clicks>

### S3 / stockage compatible (MinIO, DigitalOcean Spaces…)

```toml
[tools]
"s3:my-company-tools/mytool" = { version = "latest", url = "s3://my-bucket/tools/mytool-{{version}}-linux-amd64.tar.gz", endpoint = "https://s3.mycompany.com", region = "eu-west-1" }
```

Auth via les credentials AWS standard (`AWS_ACCESS_KEY_ID`, `~/.aws/credentials`, IAM role)

### HTTP avec authentification

```toml
[tools]
"http:mytool" = { version = "1.0.0", url = "https://artifacts.mycompany.com/mytool-{{version}}.tar.gz" }
```

```bash
export MISE_HTTP_AUTH_mytool="Bearer my-token"
```

</v-clicks>

<v-click>

> 🏢 **Use case entreprise** : distribuer des outils internes sans passer par npm/Artifactory — juste un binaire dans S3 ou GitHub Enterprise.

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Lockfile — `mise.lock`

Figer les versions exactes pour reproductibilité :

```toml
# sfeir-conf/mise.lock
[tools.node]
version  = "24.2.0"
checksum = "sha256:abc123…"

[tools.python]
version  = "3.12.3"
checksum = "sha256:def456…"

[tools.terraform]
version  = "1.9.5"
checksum = "sha256:ghi789…"
```

```bash
# Activer le lockfile
mise settings set lockfile true
```

::right::

<v-click>

# Reproductibilité garantie

```text
Machine dev :
$ mise install   → node@24.2.0

Machine CI :
$ mise install   → node@24.2.0 ✓
```

</v-click>

<v-click>

```bash
# Mettre à jour
mise upgrade        # toutes les versions
mise upgrade node   # seulement node
```

</v-click>

<v-click>

### CI avec `jdx/mise-action`

```yaml
- uses: jdx/mise-action@v2
  # lit .mise.toml + mise.lock automatiquement
```

> 💡 Comme `package-lock.json` mais pour **tous vos runtimes**.

</v-click>

---
layout: two-cols
layoutClass: gap-8
transition: slide-up
---

# CI/CD — `sfeir-conf`

```yaml
# sfeir-conf/.github/workflows/ci.yml
jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: jdx/mise-action@v2
        # lit automatiquement .mise.toml + mise.lock

      - run: mise install

      - run: mise run ci
        env:
          MISE_YES: "1"
```

::right::

<v-click>

### Convention cross-repo

```toml
# sfeir-conf — tâches standard exposées
[tasks.ci]
depends = ["lint", "test", "build"]

[tasks.lint]
run         = ["eslint src/", "ruff check app/"]
description = "Vérifier le code"

[tasks.test]
run         = ["npm test", "pytest"]
description = "Lancer les tests"

[tasks.build]
run         = "docker build -t sfeir-conf ."
description = "Builder l'artefact"
```

> 💡 `mise run ci` — interface uniforme quel que soit le repo ou le langage.

</v-click>
