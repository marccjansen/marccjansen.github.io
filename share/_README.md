# /share — one-off static pages

Self-contained HTML you want to hand someone a link to. Same trick as
`/murdoku`: any file **without YAML front matter** is copied verbatim by
Jekyll, so a page can use whatever `<style>`/`<script>` it likes without
Jekyll or the site theme touching it.

Live at `https://marccjansen.com/share/<folder>/`.

## Add one

```sh
git checkout develop && git pull
git checkout -b DEV/share-<slug>

mkdir -p share/<slug>
cp /path/to/page.html share/<slug>/index.html   # must be named index.html
# drop any images/css/js next to it and reference them relatively

git add share/<slug> && git commit -m "Share: <slug>"
git push -u origin DEV/share-<slug>
gh pr create --base develop --fill
```

Merging to `develop` publishes it — GitHub Pages builds this repo from
`develop` at `/`. Give it a minute or two.

## Naming

`<kind>_<place>_<yyyy-mm-dd>` — e.g. `trip_inverness-edinburgh_2026-10-09`.
Lowercase, hyphens inside a part, underscores between parts. ISO dates so
they sort. The folder name *is* the URL, so renaming it breaks links you
already sent.

## Rules

- **`index.html`**, always — `share/x/page.html` would need `/share/x/page.html`
  in the URL and gets no trailing-slash form.
- **No front matter.** A leading `---` block makes Jekyll render the file as a
  template, and `{{ }}` / `{% %}` inside it will be eaten as Liquid.
- **Relative asset paths** (`./map.png`, not `/map.png`) so the folder stays
  self-contained and previewable by opening the file directly.
- **Unlisted, not private.** `robots.txt` has `Disallow: /share/` and the
  `sitemap: false` default in `_config.yml` keeps these out of `sitemap.xml`
  (verify with `bundle exec jekyll build && grep share _site/sitemap.xml`).
  Anyone with the link can still read it — don't put anything sensitive here.

## Check before pushing

```sh
bundle exec jekyll build
open _site/share/<slug>/index.html
```
