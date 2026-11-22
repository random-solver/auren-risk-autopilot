# AUREN Risk Autopilot

External web dashboard and standalone CLI agent built for the **AUREN Risk API** (`POST /risk`, `POST /liquidity`, `POST /wallet`).

- **Live Deployed Dashboard:** https://random-solver.github.io/auren-risk-autopilot/
- **Production Backend Used:** https://auren-api-8yrq.onrender.com

## What It Does
1. Queries `POST /risk` and `POST /liquidity` to audit concentration, volatility, max drawdown, sector concentration, correlated pairs, and 30-day ADV exit times.
2. Computes a minimum-turnover de-escalated rebalancing allocation capping single-name weights (`<= 20%`) and adding uncorrelated multi-asset ballast (`GLD`, `TLT`, `SPY`, `JPM`, `COST`, `XOM`).
3. Re-submits the proposed allocation to `POST /risk` and `POST /liquidity` to verify the exact before-vs-after Risk Score drop.

## Live Verified Output (`https://auren-api-8yrq.onrender.com`)
```text
=== AUREN Risk Autopilot (https://auren-api-8yrq.onrender.com) ===
Before: Score=65 (High) | Vol=24.8% | MDD=-31.7% | EffAssets=3.33 | MaxExit=0.000d
  Triggered Rules: ['Largest holding is 40% (over 30%)', 'Behaves like only 3.3 assets', 'Worst past fall was -32% (deeper than -30%)', '90% sits in one sector']
After : Score=0 (Low) | Vol=14.6% | MDD=-19.5% | EffAssets=9.26 | MaxExit=0.000d
  Triggered Rules: []
Delta : -65 risk pts | -10.2% vol | 50.0% one-way turnover
```

## Quickstart (CLI)
```bash
python auren_autopilot.py --api https://auren-api-8yrq.onrender.com --json-out audit_sample.json
```
