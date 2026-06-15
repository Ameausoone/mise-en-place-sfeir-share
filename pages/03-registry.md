---
layout: section
---

# 📦 Registry

---
transition: fade-out
---

# Registry — 800+ outils disponibles

### Explorer & installer

```bash {1|2-3|4-5|all}
# Lister tous les outils disponibles
mise registry

# Rechercher un outil
mise registry | grep -i terraform

# Installer plusieurs outils d'un coup
mise use node terraform kubectl helm awscli gh
```

<v-click>

### Outils populaires et leurs backends

| Outil | Backend |
|-------|---------|
| `node`, `python`, `go`, `ruby` | core / asdf |
| `terraform`, `kubectl`, `helm` | asdf / aqua |
| `gh`, `jq`, `ripgrep`, `fd`    | aqua / ubi  |

</v-click>

---
transition: fade-out
---

# Les backends

mise installe les outils via **plusieurs sources** selon leur nature :

<v-clicks>

```toml {1-3}
[tools]
# asdf — compatible 800+ plugins
node = "24"
```

```toml {1-3}
# npm / pipx — packages isolés
"npm:prettier"   = "latest"
"pipx:ansible"   = "latest"
```

```toml {1-4}
# cargo — compilé depuis crates.io
"cargo:ripgrep" = "14"

# ubi — binaire GitHub Releases (universel)
"ubi:sharkdp/bat" = "latest"
```

</v-clicks>

---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Lockfile — `mise.lock`

Figer les versions exactes pour reproductibilité :

```toml
# mise.lock — généré automatiquement
# ✅ À commiter dans git !

[tools.node]
version  = "24.0.0"
checksum = "sha256:abc123…"

[tools.terraform]
version  = "1.7.5"
checksum = "sha256:ghi789…"
```

```bash
# Activer le lockfile
mise settings set lockfile true
```

::right::

<v-click>

# Reproductibilité garantie

```bash
# Machine dev
mise install   # → node@24.0.0

# Machine CI
mise install   # → node@24.0.0 ✓
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
