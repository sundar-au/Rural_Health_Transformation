# RHT Rural Health Intelligence Platform

County-level Medicaid access analytics for the $50B Rural Health Transformation grant program.

## What this is

A fully static website containing:
- **Interactive analytics dashboard** — 9 states, 299 named counties, NOFO benchmarks, CAH viability
- **AI chatbot** — trained on RHT program details, NOFO guidelines, pipeline methodology, and all analytics output

## Files

| File | Purpose |
|------|---------|
| `index.html` | Site shell — header, iframe, chatbot widget |
| `dashboard.html` | Full analytics dashboard (127KB, all data embedded) |
| `worker.js` | Cloudflare Worker API proxy (hides API key for team use) |
| `wrangler.toml` | Cloudflare Worker config |
| `.github/workflows/deploy.yml` | Auto-deploy to GitHub Pages on push |

## Hosting (free)

### Step 1 — GitHub Pages

1. Create a new GitHub repo (public or private)
2. Push these files to the `main` branch
3. Go to Settings → Pages → Source: GitHub Actions
4. Push again — GitHub Actions will deploy automatically
5. Your site will be live at `https://YOUR-USERNAME.github.io/REPO-NAME`

### Step 2 — Cloudflare Worker (team deployment)

Deploy the API proxy so no user needs their own API key:

```bash
npm install -g wrangler
wrangler login
wrangler secret put ANTHROPIC_API_KEY   # paste your Anthropic key
wrangler deploy
```

Copy the Worker URL (e.g. `https://rht-api-proxy.YOUR-ACCOUNT.workers.dev`) and enter it in the site's proxy URL field.

### Step 3 — Custom domain (optional)

In Cloudflare → Pages (or GitHub Pages settings), add your custom domain. Cloudflare handles SSL automatically.

## Individual use (no Worker needed)

Open `index.html` in a browser and enter your Anthropic API key when prompted. The key is stored in `localStorage` only — never sent anywhere except directly to Anthropic.

## Chatbot knowledge base

The chatbot is trained on 19 structured knowledge chunks covering:

| Category | Chunks | Content |
|----------|--------|---------|
| RHT Program | 3 | $50B structure, award amounts, Medicaid cuts context |
| NOFO Scoring | 2 | F.1-F.9 factors with weights and our evidence mapping |
| Methodology | 4 | 3-stage pipeline, per-10K normalization, data caveats |
| National Results | 5 | 107,041 providers, desert rates, health burden, CAH |
| 9-State Results | 3 | 16,319 providers, critical counties, WF attrition |
| Benchmarks | 2 | 6 external validations, winning state corroboration |

To update the knowledge base, edit `knowledge_base.json` and rebuild `index.html` using `build_site.py`.

## Cost

| Component | Cost |
|-----------|------|
| GitHub Pages | Free |
| Cloudflare Worker | Free (100K requests/day) |
| Claude API | ~$0.003 per query (claude-sonnet-4-6) |

## Data

- Pipeline: Run 4, v1.1.2
- Source: HHS Medicaid Provider Spending Dataset (Feb 2026)
- Analysis window: Jan 2022 – Oct 2024
- States: AK, AZ, HI, IL, MI, NJ, NY, UT, WY
