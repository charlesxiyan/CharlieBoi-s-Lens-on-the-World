# CharlieBoi's Lens on the World

A personal technical knowledge base and blog, built with **MkDocs Material**.
Markdown source, LaTeX mathematics via MathJax, Python/C++ highlighting, full-text
search, light/dark themes, and responsive navigation are configured.

## Preview locally

Use Python 3.9 or newer (Python 3.12 recommended). From the repository root:

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.lock
python -m mkdocs serve --dev-addr 127.0.0.1:8000
```

Open **http://127.0.0.1:8000/**. Edits to Markdown and configuration reload the
preview automatically. Stop with Ctrl+C. On Windows, activate with
`.venv\Scripts\activate` instead.

The first install requires internet. Math rendering loads a pinned MathJax script
from jsDelivr, so equations require internet in the browser. Fonts are system
fonts; search and syntax highlighting are built into the generated site.

## Build and validate

```sh
python -m mkdocs build --strict
```

Generated HTML goes into `site/` (ignored by Git). CI runs the same strict build
on main pushes and pull requests. Main pushes deploy to GitHub Pages; pull
requests only build.

`requirements.txt` records supported direct dependency ranges;
`requirements.lock` records the exact versions used for the verified build.
To intentionally update, install from `requirements.txt` in a clean environment,
run the strict build, then regenerate the lock with `python -m pip freeze`.

## Project map

```text
mkdocs.yml                Site settings, navigation, and extensions
requirements.txt          Direct Python dependency ranges
requirements.lock         Resolved Python dependencies
AGENTS.md                 Instructions for coding agents
.github/workflows/        Build-only continuous integration
planning/decap-cms.md      Future browser editing integration
docs/
  index.md                Homepage
  about.md                About page
  leetcode/               194 Python write-ups: Day 1–55 and 6 bonus modules
  quant-finance/          Interview study plan, day modules, and project showcase
  notes/                  Python, C++, and mathematics references
  blog/index.md           Life journal listing
  blog/posts/             Dated Markdown posts
  javascripts/mathjax.js  LaTeX rendering configuration
  stylesheets/extra.css   Small visual customizations
```

## Write a note or post

Add notes under `docs/notes/` and register them in `mkdocs.yml` under `nav`.
Use relative Markdown links, fenced code blocks labeled `python` or `cpp`, and
`$...$` for inline math or `$$...$$` for display equations.

Add posts under `docs/blog/posts/`, for example `2026-10-27-a-new-idea.md`:

```markdown
---
date: 2026-10-27
categories:
  - Learning
slug: a-new-idea
---

# A new idea

A short introduction.

<!-- more -->

The rest of the post.
```

Blog archives and categories are generated automatically. Avoid future dates
unless you intend the blog plugin to treat the entry as a future post.

## GitHub Pages deployment

The intended custom domain is `https://charlieboi.com/`. The workflow reads the
current Pages base URL and passes it through `SITE_URL`, so links and canonical
URLs work on the initial GitHub Pages project URL and after domain binding.
Local builds default to the intended custom domain.

Pushes to `main` build and deploy `site/` through GitHub Actions. Pull requests
build without deploying. Repository Pages must use GitHub Actions as its source.
The deploy job uses the `github-pages` environment with Pages and OIDC permissions.

Domain binding and DNS are separate setup steps. No CMS authentication is enabled.

## Browser editing later

See [the Decap CMS integration plan](planning/decap-cms.md) for authentication,
collections, and custom-domain media paths. Decap is not active in this starter.

## References

- [MkDocs Material](https://squidfunk.github.io/mkdocs-material/)
- [Blog setup](https://squidfunk.github.io/mkdocs-material/setup/setting-up-a-blog/)
- [Math support](https://squidfunk.github.io/mkdocs-material/reference/math/)
- [Decap GitHub backend](https://decapcms.org/docs/github-backend/)

## Collaborating and editor credits

Invite contributors through GitHub Settings → Collaborators → Add people.
Contributors should use their own GitHub accounts. For shared editing, create a
branch and a pull request, check the build, then merge into `main` to publish.
GitHub's web editor can edit Markdown without a local Python installation.

Question pages credit the confirmed content editor directly below the title:

```markdown
*Edited by Sophia*
```

Use `Charles`, `Sophia`, or `Charles and Sophia` as appropriate, and update the
day overview's editor column too. This credit records content contributions and
is independent of the account that imports or commits the file.
