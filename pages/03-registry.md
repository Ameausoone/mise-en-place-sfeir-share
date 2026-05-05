---
layout: section
---

# 📦 Registry

---
layout: two-cols
layoutClass: gap-8
---

# Registry — 800+ outils disponibles

<div class="mt-2">

### Explorer le registre

```bash
# Lister tous les outils
mise registry

# Rechercher
mise registry | grep -i terraform
mise registry | grep -i java

# Afficher les backends disponibles
mise registry --backend asdf
mise registry --backend aqua
```

### Quelques outils populaires

```bash
mise use node terraform kubectl \
          helm awscli gh jq ripgrep
```

| Outil | Backend |
|-------|---------|
| `node`, `python`, `go`, `ruby` | core / asdf |
| `terraform`, `kubectl`, `helm` | asdf / aqua |
| `gh`, `jq`, `ripgrep`, `fd`    | aqua / ubi |

</div>

::right::

<div class="mt-2">

### Les backends

mise peut installer des outils via **plusieurs sources** :

```toml
[tools]
# asdf (par défaut, compatible 800+ plugins)
node = "20"
terraform = "1.7"

# npm — package npm global
"npm:prettier" = "latest"
"npm:typescript" = "5"

# cargo — compile depuis crates.io
"cargo:ripgrep" = "14"

# pipx — package Python isolé
"pipx:ansible" = "latest"

# ubi — binaire GitHub Releases (universel)
"ubi:cli/cli" = "latest"        # gh CLI
"ubi:sharkdp/bat" = "latest"    # bat

# go — binaire Go
"go:mvdan.cc/gofumpt" = "latest"
```

</div>

---

# Registry — Lockfile

<div class="grid grid-cols-2 gap-6">

<div>

### `mise.lock` — figer les versions

```toml
# mise.lock — généré automatiquement
# À commiter dans git !

[tools.node]
version  = "20.11.0"
checksum = "sha256:abc123…"

[tools.python]
version  = "3.12.3"
checksum = "sha256:def456…"

[tools.terraform]
version  = "1.7.5"
checksum = "sha256:ghi789…"
```

```bash
# Activer le lockfile
mise settings set lockfile true

# Installer exactement les versions lockées
mise install
```

</div>

<div v-click>

### Reproductibilité garantie

```bash
# Machine A (dev)
mise install  # installe node@20.11.0

# Machine B (CI)
mise install  # installe node@20.11.0 ✓

# Mettre à jour le lockfile
mise upgrade        # met à jour toutes les versions
mise upgrade node   # met à jour seulement node
```

### Intégration CI avec `jdx/mise-action`

```yaml
- uses: jdx/mise-action@v2
  # lit automatiquement .mise.toml + mise.lock
  # installe exactement les bonnes versions
```

<div class="mt-3 p-3 rounded bg-blue-500/10 border border-blue-500/30 text-sm">
  💡 Comme <code>package-lock.json</code> mais pour
  <strong>tous vos runtimes et outils CLI</strong>.
</div>

</div>

</div>
