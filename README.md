# Procurement Timing Engine

System design for a product that tells a factory **which raw material to order,
how much, and by what date**, from its own consumption history, its suppliers'
actual reliability, and what is happening in the world.

**Read it:** https://anjan-dutta.github.io/project-x/

## The premise

"When should I buy" is not a prediction. It is a decision that falls out of two
predictions plus a policy. Modelling the purchase date directly is the common
failure, because the date is not a property of the world, it is the output of
the policy you choose.

So the system forecasts **consumption** and **lead time** as distributions, then
lets inventory theory convert them into a reorder decision. Global events do not
enter as model features; they shift the parameters of those two distributions.

## The four layers

| | Layer | Approach |
| --- | --- | --- |
| L1 | Demand, as a distribution | BOM explosion inside the MRP horizon; Croston/TSB, ETS/ARIMA or quantile LightGBM beyond it, chosen by demand pattern |
| L2 | Lead time, with its variance | Per supplier-material distribution fitted from promised vs actual |
| L3 | The policy that produces a date | Reorder point and safety stock; Monte Carlo for A-class items |
| L4 | The event overlay | Signals scored into a fixed schema, then a hand-authored transmission map to parameter deltas |

## Repository layout

| Path | Purpose |
| --- | --- |
| `index.html` | The system design: the four layers, the decision math, the build sequence |
| `market-research.html` | Published evidence on supply chain risk for Indian manufacturers under Rs 100 crore, with sources |
| `demo.html` | Live speech-to-text demo, browser-only, no backend |
| `style.css` | Shared design tokens and components for both pages |
| `.nojekyll` | Skips Jekyll processing so files beginning with `_` are served as-is |

No build step and no dependencies. Open `index.html` in a browser, or serve the
directory if you want a real origin:

```bash
python3 -m http.server 8000
```

## Deploying

Push to `main`. Pages serves the repository root directly via
**Settings → Pages → Source: Deploy from a branch**, set to `main` at `/ (root)`.
Builds appear in the Actions tab as `pages build and deployment`.

## Demo

`demo.html` runs the first stage of the voice reporting path: the browser's
Web Speech API transcribing a spoken command onto the page. It needs no server,
which is why it works on a static Pages site.

Chrome or Edge required (Firefox does not implement `SpeechRecognition`), over
HTTPS or localhost. Note that Chrome sends audio to Google's servers for
recognition; it is not processed locally.

## Market research

`market-research.html` collects published evidence that the problem is real:
price volatility is the top procurement challenge for Indian MSMEs (55.7% of
27,000+ surveyed), the average invoice payment cycle runs 73 days against a
45-day statutory limit, and manufacturers held only 2-4 weeks of rare-earth
magnet cover during the 2025 export-control episode.

Every figure names its study and sample size. Read the provenance note at the
top of that page before quoting any of it.

## Status

Design only. Nothing here is implemented yet. Figures for holding cost and
stockout ratios are standard industry ranges, to be replaced with real customer
data once available.
