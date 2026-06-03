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
