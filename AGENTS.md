# Repository guide

## Purpose

Personal knowledge base and blog for CharlieBoi, built with MkDocs Material.

## Layout

- `mkdocs.yml`: site configuration and explicit navigation.
- `docs/leetcode/`: Algorithm section (retain these URLs); Python solutions in `day-N/` and `bonus-*/` modules; keep
  same-problem variants in their original modules. Assets are in `assets/`.
- `docs/quant-finance/`: landing page for `study-plan/` and `projects/`.
  The interview plan links to the existing `day-N/` modules (keep their URLs);
  each has an overview and four `question-N.md` pages. Register new days,
  questions, and projects under the corresponding `nav` section.
- `docs/notes/`: supporting technical references.
- `docs/blog/posts/`: dated posts for the Life section, with YAML front matter.
- `docs/javascripts/` and `docs/stylesheets/`: small presentation enhancements.
- `planning/`: future integrations; not published.
- `site/` and `.venv/`: generated, ignored; never commit.

## Working conventions

- Credit question editors below the title with `*Edited by Charles*` or
  `*Edited by Sophia*`, using confirmed contributions rather than the Git uploader.
  If both contributed, use `*Edited by Charles and Sophia*`.
- Algorithm article order: editor credit and compact difficulty/topic line;
  collapsed `??? question "Problem"`; collapsed `??? note "Explanation, examples & other approaches"`;
  any folded study records or original drafts; then `## Python solution` with
  the complete main code visible. Put core ideas, derivations, hand simulations,
  implementation details, complexity, and takeaways inside Explanation rather
  than expanding them by default. Keep every explanatory dropdown before code.
- For LeetCode problems, link the official statement inside Problem; for custom
  exercises, include the supplied statement, examples, and constraints directly.
  Topic notes should describe their scope instead of inventing a problem.
  Preserve source images, relative links, original study records, and alternate
  solutions. Sudoku Solver (37) uses Charles's supplied complete solution.
  Do not claim that a syntax check verifies algorithm correctness.
- For Charles-edited Quant pages, follow `planning/charles-question-style.md`.
  Keep new draft solutions outside `docs/` until Charles approves publication.
- Keep content in Markdown. Preserve the author's voice and do not invent personal facts.
- Add new notes to `nav` in `mkdocs.yml`. Blog posts are discovered automatically.
- Use fenced `python` and `cpp` code blocks and `$...$` / `$$...$$` for math.
- Blog posts need a `date` (YYYY-MM-DD), title, and `<!-- more -->` excerpt break.
- Prefer relative internal Markdown links so project-subpath hosting works.
- Keep dependencies in `requirements.txt`; update `requirements.lock` after resolving changes.
- After content or configuration edits, run `.venv/bin/python -m mkdocs build --strict`.
- For preview, run `.venv/bin/python -m mkdocs serve --dev-addr 127.0.0.1:8000`.

## Deployment boundary

GitHub Pages deployment is authorized and runs on pushes to `main` through
GitHub Actions. Pull requests build without deploying. The intended custom domain
is `https://charlieboi.com/`; CI uses the current Pages URL through `SITE_URL`
until domain setup is complete. Do not modify DNS or enable CMS credentials
without explicit user instructions for those changes.
