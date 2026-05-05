# Copilot Instructions

## Project

This is a [Slidev](https://sli.dev/) presentation about [mise](https://mise.jdx.dev/) and [fnox](https://fnox.jdx.dev/).

## Structure

```
slides.md              # Entry point — frontmatter + cover + src: imports only
pages/
  01-introduction.md   # Le problème + Qu'est-ce que mise ?
  02-installation.md   # Installation + shell integration + mise doctor
  03-registry.md       # Registry section (backends, lockfile)
  04-tools.md          # Tools section (install/use/exec)
  05-tasks.md          # Tasks section (toml, file tasks, watch, usage)
  06-hooks.md          # Hooks section (enter/leave, pre/post task, watch_files)
  07-env.md            # Env section + migration + performance + best practices
  08-fnox.md           # fnox section (secrets, providers, shell integration)
  09-recap.md          # Récapitulatif + Merci
.mise.toml             # Tools (node) + tasks (install, dev, build, export)
```

## Development commands

```bash
mise run dev     # Start Slidev dev server
mise run build   # Build for production (outputs to dist/)
mise run export  # Export slides to PDF
```

The build uses `--base /mise-en-place-sfeir-share/` for GitHub Pages deployment.

## Content guidelines

- Language: **French** (slides are in French)
- Each section lives in its own `pages/NN-name.md` file
- `slides.md` must remain a minimal entry point — only frontmatter, cover slide, and `src:` imports
- Code blocks must use languages supported by Shiki (avoid `gitignore`, use `bash` instead)
- Prefer `v-click` for progressive disclosure on content slides
- Use `layout: section` slides as section dividers

## CI/CD

Slides are automatically deployed to GitHub Pages on every push to `main`.
The workflow is in `.github/workflows/deploy.yml`.
GitHub Pages must be configured to use **GitHub Actions** as the source (not a branch).
