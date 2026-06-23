# Tenuto · CPF Studio — CPF LIFE Projection

A single-file, client-facing web app that estimates a person's CPF balances at
retirement and their expected **CPF LIFE** monthly payouts across **every plan
(Standard / Basic / Escalating) and every payout start age (65–70)**.

- **Transient by design** — all inputs live in memory only. Nothing is stored,
  sent anywhere, or survives a refresh.
- **Self-contained** — one `index.html`, no build step, no backend. The only
  external request is the Newsreader web font.
- **On brand** — Tenuto's warm ivory canvas, single plum accent, and Newsreader
  serif voice, with ambient motion, count-up figures, and animated charts.

## Host it on GitHub Pages

1. Commit this folder to your repo.
2. Repo **Settings → Pages → Build and deployment**.
3. Source: **Deploy from a branch**, pick your branch, folder `/cpf-life-calculator`
   (or move `index.html` to the repo root and pick `/`).
4. Your page goes live at `https://<user>.github.io/<repo>/cpf-life-calculator/`.

> Tip: for a custom path, you can also just drop `index.html` into any static
> host (Netlify, Cloudflare Pages, S3). No configuration required.

## Customising

- **Call-to-action** — edit the `#ctaLink` `href` in `index.html` (search for
  `id="ctaLink"`) to point at your booking link, and change the button text.
- **Your name / branding** — update the masthead `brandname` and the footer.
- **Assumptions** — all financial parameters sit in the `CONSTANTS` block at the
  top of the `<script>` (wage ceiling, interest tiers, retirement sums, CPF LIFE
  reference tables). They're labelled and easy to bump when CPF updates figures.

## How the numbers work

Figures use **2026 CPF parameters**: age-based contribution & allocation rates,
the \$8,000 monthly wage ceiling, 2.5% OA / 4% SMRA interest with extra-interest
tiers, and retirement sums BRS \$110,200 · FRS \$220,400 · ERS \$440,800.

The model projects monthly contributions to age 55, forms the Retirement Account
(SA then OA, up to the cohort's projected Full Retirement Sum), then maps the RA
to payouts by **interpolating CPF Board's own published CPF LIFE Standard-Plan
tables** — so it reproduces CPF's official BRS/FRS/ERS payouts exactly at those
anchors. Basic (~95% of Standard), Escalating (~79% initial, +2%/yr), the female
adjustment (~7% lower), and deferral (~7%/yr past 65) are applied on top.

**This is an illustration, not advice.** Always confirm against the official
[CPF LIFE Estimator](https://www.cpf.gov.sg) before acting.
