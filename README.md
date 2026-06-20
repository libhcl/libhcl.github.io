<p align="center"><img src="https://raw.githubusercontent.com/libhcl/brand/main/social/libhcl.png" alt="libhcl/libhcl.github.io" width="720"></p>

# libhcl.github.io

The documentation site for the **libhcl** organization, built with
[Hugo](https://gohugo.io) and served from GitHub Pages at
<https://libhcl.github.io>.

It covers the org's two libraries:

- [**c-hcl**](https://github.com/libhcl/c-hcl) — the declarative HCL subset.
- [**c-hcl2**](https://github.com/libhcl/c-hcl2) — a from-scratch HCL2.

## Layout

- `content/` — Markdown pages (`_index.md` is the home).
- `layouts/` — a minimal, dependency-free theme (no external Hugo theme/module).
- `static/css/` — the stylesheet.
- `.github/workflows/pages.yml` — builds with Hugo and deploys to Pages.

## Theme & hero banner

- A light/dark toggle (☀/☾) sits in the header; the choice is saved in
  `localStorage` and otherwise follows the OS (`prefers-color-scheme`).
- The home page shows an animated "coding" hero (pure CSS, no asset). To use a
  real video instead, drop an mp4 under `static/media/hero.mp4`, then set
  `params.heroVideo = "media/hero.mp4"` (and optional `heroPoster`) in
  `hugo.toml` — it plays on top of the animated backdrop.

## Develop

```sh
hugo server -D     # live preview at http://localhost:1313
hugo --minify      # build into ./public
```

## Publish

1. Create the repo `libhcl/libhcl.github.io` on GitHub (org-level Pages site).
2. Push this directory to its default branch.
3. In the repo settings, set **Pages → Build and deployment → Source** to
   **GitHub Actions**. The workflow then builds and deploys on every push.

BSD-3-Clause.
