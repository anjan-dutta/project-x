# Project X

A static site hosted on [GitHub Pages](https://pages.github.com/).

**Live site:** https://anjan-dutta.github.io/project-x/

## Layout

| Path | Purpose |
| --- | --- |
| `index.html` | The whole site: markup, styles, and content in one file |
| `.nojekyll` | Skips Jekyll processing so files beginning with `_` are served as-is |

## Working on it

There is no build step and no dependencies. Open `index.html` in a browser, or
serve the directory if you want a real origin:

```bash
python3 -m http.server 8000
```

Then visit http://localhost:8000.

## Deploying

Push to `main`. Pages is configured with **Settings → Pages → Source: Deploy
from a branch**, set to `main` at `/ (root)`, so GitHub serves the repository
contents directly. A push is usually live within a minute. Build progress shows
up under the repo's **Actions** tab as a `pages build and deployment` run.

## Adding a build step later

Serving from a branch only works while the site is committed as-is. Once it
compiles (Vite, Astro, Hugo), switch **Settings → Pages → Source** to **GitHub
Actions** and add a workflow that builds and uploads the output:

```yaml
permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
    steps:
      - uses: actions/checkout@v5
      - run: npm ci && npm run build
      - uses: actions/configure-pages@v5
      - uses: actions/upload-pages-artifact@v3
        with:
          path: dist
      - uses: actions/deploy-pages@v4
```

That route needs **Settings → Actions → General → Workflow permissions** set to
*Read and write permissions*; without it the token cannot publish to Pages.

## Custom domain

Add it under **Settings → Pages → Custom domain**. GitHub commits a `CNAME`
file to the repo and serves the site from that hostname.
