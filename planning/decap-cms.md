# Future browser editing with Decap CMS

Decap is not enabled yet. Markdown files and YAML front matter are kept in plain
text so a future CMS can edit the same source without changing the generator.

Before enabling it:

1. Select a GitHub OAuth authentication service compatible with Decap's GitHub backend.
   GitHub Pages serves static files and cannot host that authentication service.
2. Confirm the repository's publishing branch and OAuth callback origin.
3. Add an admin HTML entry and config under `docs/admin/`, with its script version
   pinned. Avoid placing OAuth client secrets in static files.
4. Define a folder collection for `docs/blog/posts` with date, title, slug,
   categories, and body fields. Define notes and homepage as separate collections.
5. Use `docs/assets/uploads` as `media_folder`. Set `public_folder` to
   `/assets/uploads` for `https://charlieboi.com/`. Verify images in the CMS
   preview and site.
6. Confirm new posts include an excerpt marker and that new notes enter the
   explicit MkDocs navigation (or add an automated navigation policy).
7. Test authentication, branch/PR permissions, Markdown math, and media paths
   before allowing publication. CMS preview styling is separate from MkDocs.

Keep OAuth credentials out of Git. This guide deliberately installs no active
admin endpoint and enables no authentication or publishing flow.
