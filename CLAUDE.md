# mise-en-place-sfeir-share

Présentation Slidev (French) sur [mise](https://mise.jdx.dev/), l'outil de gestion d'environnement de développement.

## Stack

- **[Slidev](https://sli.dev/)** — framework de slides en Markdown/Vue
- **pnpm** — gestionnaire de paquets
- **mise** — gestion des versions de runtimes (voir `.mise.toml`)

## Commandes essentielles

```bash
# Installer les dépendances
pnpm install

# Démarrer le serveur de dev (hot reload)
pnpm dev        # → http://localhost:3030

# Build statique
pnpm build

# Export PDF
pnpm export
```

## Structure

```
slides.md              # Entrée principale — référence les pages/
pages/
  01-introduction.md   # Le problème & présentation de mise
  02-installation.md   # Installation & activation shell
  03-registry.md       # Registry & backends & lockfile
  04-tools.md          # Commandes tools (install, use, list…)
  05-tasks.md          # Tasks (toml, fichiers, watch, args)
  06-hooks.md          # Hooks (enter/leave, pre/post, watch_files)
  07-env.md            # Variables d'env, migration, perf, bonnes pratiques
  08-fnox.md           # fnox — secrets chiffrés
  09-recap.md          # Récap & slide de fin
  10-platform-engineering.md  # Équipe, cross-repo, monorepo, CI/CD
components/            # Composants Vue custom pour les slides
snippets/              # Extraits de code réutilisables
```

## Conventions slides

- Chaque fichier `pages/*.md` peut contenir plusieurs slides séparées par `---`
- Utiliser `<v-clicks>` pour révéler les listes progressivement
- Utiliser `{1|2|3|all}` dans les blocs de code pour la mise en évidence ligne par ligne
- Layout `two-cols` pour les comparaisons côte à côte
- Layout `section` pour les slides de transition entre chapitres
- `transition: fade-out` ou `slide-up` pour les animations entre slides
