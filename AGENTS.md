# Repository guide

## Purpose

Personal knowledge base and blog for CharlieBoi, built with MkDocs Material.

## Layout

- `mkdocs.yml`: site configuration and explicit navigation.
- `docs/leetcode/`: LeetCode explanations.
- `docs/quant-finance/`: Quant self-study plan; each `day-N/` has an overview
  and four separate `question-N.md` pages. Register new days and questions in `nav`.
- `docs/notes/`: supporting technical references.
- `docs/blog/posts/`: dated posts for the Life section, with YAML front matter.
- `docs/javascripts/` and `docs/stylesheets/`: small presentation enhancements.
- `planning/`: future integrations; not published.
- `site/` and `.venv/`: generated, ignored; never commit.

## Working conventions

- Keep content in Markdown. Preserve the author's voice and do not invent personal facts.
- Add new notes to `nav` in `mkdocs.yml`. Blog posts are discovered automatically.
- Use fenced `python` and `cpp` code blocks and `$...$` / `$$...$$` for math.
- Blog posts need a `date` (YYYY-MM-DD), title, and `<!-- more -->` excerpt break.
- Prefer relative internal Markdown links so project-subpath hosting works.
- Keep dependencies in `requirements.txt`; update `requirements.lock` after resolving changes.
- After content or configuration edits, run `.venv/bin/python -m mkdocs build --strict`.
- For preview, run `.venv/bin/python -m mkdocs serve --dev-addr 127.0.0.1:8000`.

## Deployment boundary

GitHub Pages is the intended future host, with `https://charlieboi.com/` as the
canonical site URL. Do not deploy, add an active deployment
workflow, change repository Pages settings, configure DNS, or enable CMS
credentials unless the user explicitly requests it. Current CI only builds.
