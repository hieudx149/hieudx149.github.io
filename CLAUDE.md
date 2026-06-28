# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A personal website/portfolio + blog for Hieu Duong, built with [Quarto](https://quarto.org) and published to GitHub Pages. The `quarto-authoring` skill is available and should be used when writing or editing `.qmd` content.

## Build & preview

```bash
quarto preview          # live-reload dev server
quarto render           # render whole site into docs/
```

The CV is a separate LaTeX artifact (see [cv/README.md](cv/README.md)):

```bash
cd cv && latexmk -xelatex -interaction=nonstopmode -halt-on-error Hieu_Duong_CV.tex
```

**Publishing the CV PDF.** The `cv/` LaTeX sources/output are gitignored (local-only), so the compiled PDF must be copied into the project to be served. The published copy lives at `files/Hieu_Duong_CV.pdf` (committed) and is declared under `project.resources` in [_quarto.yml](_quarto.yml) so `quarto render` copies it into `docs/files/`. The navbar **CV** icon and the About page's CV link both point at `files/Hieu_Duong_CV.pdf`. When the CV changes: rebuild it in `cv/`, copy `cv/portfolio.pdf` → `files/Hieu_Duong_CV.pdf`, then re-render and commit.

## Publishing to GitHub Pages

This site uses the **"Render to `docs/`"** method ([Quarto docs](https://quarto.org/docs/publishing/github-pages.html)): the rendered output is committed and GitHub Pages serves it from the `docs/` folder of `main`. To deploy:

```bash
quarto render          # regenerate docs/ from the .qmd sources
git add docs
git commit -m "Publish site"
git push
```

GitHub Pages is configured (repo Settings → Pages) to publish from the `docs/` directory of the `main` branch; the push above triggers redeploy. Do not use `quarto publish gh-pages` here — that method pushes to a separate `gh-pages` branch and would conflict with this `docs/`-committed setup.

## Architecture & key conventions

- **Output goes to `docs/`, not `_site/`.** This is set by `output-dir: docs` in [_quarto.yml](_quarto.yml) so GitHub Pages can serve from the `docs/` folder of `main`. The generated `docs/` directory **is committed** — re-render and commit it whenever source `.qmd` files change, or the published site will be stale. `.nojekyll` (root and `docs/`) disables Jekyll processing.
- **Source vs. generated.** Edit the `.qmd` files (`index.qmd`, `posts.qmd`, `vi_posts.qmd`, `posts/*/index.qmd`) and `_quarto.yml`/`styles.css`. Everything under `docs/` is generated — never hand-edit it. (`index.qmd` is the About landing page; its contact links — CV, GitHub, LinkedIn, Email — are configured via the `about.links` YAML block.)
- **Site config** (navbar, theme `cosmo`, social links, page list) lives entirely in [_quarto.yml](_quarto.yml).
- **Blog posts** are folders under `posts/` each containing `index.qmd` plus images. [posts/_metadata.yml](posts/_metadata.yml) applies `freeze: true` and `toc: true` to all posts — `freeze` means computational output is cached and not re-executed on render unless the source changes. `posts.qmd`/`vi_posts.qmd` are currently placeholders ("Coming soon"); the Quarto listing config is present but commented out.
- **Ignored by git** (`.gitignore`): `/.quarto/` (render cache), `_site`, `**/*.quarto_ipynb`, and `/cv/` (LaTeX sources/output are local-only).
