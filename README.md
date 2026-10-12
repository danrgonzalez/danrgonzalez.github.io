# danrgonzalez.github.io

Source for my personal website, built with [Quarto](https://quarto.org) and published to GitHub Pages.

## Edit

Content is plain Markdown:

| Page | File |
|---|---|
| About (home) | `index.qmd` |
| Experience | `experience.qmd` |
| Projects | `projects.qmd` lists every file in `projects/` (add a new `.md` there to add a project; `order:` sets its position) |
| Research | `research.qmd` |

Site-wide settings (navigation, links, theme) are in `_quarto.yml`; styling is in `styles/`.

## Preview

```bash
quarto preview
```

## Publish

Pushing to `main` triggers `.github/workflows/publish.yml`, which renders the site with Quarto 1.10.18 and deploys it to GitHub Pages.

`project.render` in `_quarto.yml` controls which pages are built into the site. Every committed file is public, so `.gitignore` is an allowlist: only the site source above can be committed, and anything else in this folder stays local.
