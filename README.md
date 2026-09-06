# Project X

A static site hosted on [GitHub Pages](https://pages.github.com/).

**Live site:** https://anjan-dutta.github.io/project-x/

## Layout

| Path | Purpose |
| --- | --- |
| `index.html` | The whole site: markup, styles, and content in one file |
| `.github/workflows/deploy-pages.yml` | Publishes `main` to Pages on every push |
| `.nojekyll` | Skips Jekyll processing so files beginning with `_` are served as-is |

## Working on it

There is no build step and no dependencies. Open `index.html` in a browser, or
serve the directory if you want a real origin:

```bash
python3 -m http.server 8000
```

Then visit http://localhost:8000.

## Deploying

Push to `main`. The workflow uploads the repository root as the Pages artifact
and deploys it, usually within a minute. Progress is visible under the repo's
**Actions** tab.

The first run also enables Pages itself (`actions/configure-pages` is set to
`enablement: true`), so there is no setting to flip by hand.

## Adding a build step later

If the site grows into something that compiles (Vite, Astro, Hugo), add the
build to the workflow before the upload step and point `path:` at the output
directory instead of `.`:

```yaml
      - run: npm ci && npm run build
      - uses: actions/upload-pages-artifact@v3
        with:
          path: dist
```

## Custom domain

Add it under **Settings → Pages → Custom domain**. GitHub commits a `CNAME`
file to the repo, and the workflow will keep serving it with the rest of the
site.
