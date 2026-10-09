# AEGIS 2.0 for Vercel

**A Vercel-compatible web adaptation of the AEGIS 2.0 academic banking fraud investigation prototype.**

## What is included

- Your **original cinematic AEGIS website**, kept intact at `/portfolio.html` apart from the `09 Demo` menu item now linking to `/app`.
- A **Next.js investigation dashboard** at `/app`, visually based on the original Streamlit dashboard and containing all seven familiar navigation areas.
- A **real deterministic risk assessment API** at `/api/investigate`, ported from the *scoring logic* in the provided Python `risk_engine.py`: amount ratio, geographic deviation, new device/beneficiary, failed authentication, unusual hours, capped score, 30/70 thresholds.
- A basic **fail-safe input validation** gate. This port is **not a full replacement** for the original Python `case_schema.py` + `evidence_validator.py`.
- Local browser-only synthetic investigation queue, analyst notes, and exportable audit-style event history.
- Static recorded model-comparison outcomes, clearly labelled as a small historical benchmark, not new or live model executions.
- Tests confirming the original sample outcomes of **LOW/0, MEDIUM/40, HIGH/100**, plus input fail-safe and no real banking action.

## 🚨 Limitations you must tell your faculty

This deployment **does not run Python Streamlit, n8n, OpenRouter agents, authenticated n8n HITL callbacks, or the original persistent Python audit receiver**. Those cannot be made to work merely by adding static files to Vercel. The dashboard's reviewer decisions are **browser-only notes**, not the original backend's authoritative Human-in-the-Loop actions. Browser data does **not** sync across computers or persist reliably if local storage is cleared. This application **must not be described as the complete original integrated PoC**.

For demonstrating the real n8n pipeline, use the video of your original Mac-local working system. To create a fully live hosted platform, provision independently hosted n8n with durable storage, a secure callback/audit API and a shared database, then implement authenticated integration with this frontend.

## Upload to GitHub

1. Extract the ZIP you received. **Upload the files inside the `AEGIS_Vercel_Integrated` folder, not the ZIP itself.** Ensure `package.json`, `app/`, `lib/`, `public/` are at the repository root.
2. Create a new GitHub repository (for example `AEGIS-Vercel-Integrated`) and upload these files.
3. Go to https://vercel.com/new, import the GitHub repository, set the framework preset to **Next.js**, leave the root directory at `./`, and deploy. There is no custom build command required.
4. Open the deployed URL. The `/` page redirects to the cinematic site, and **09 Demo** opens `/app` on the *same* Vercel domain. No `localhost` links are required.

## Run locally (optional)

```bash
npm install
npm test
npm run dev
```

Open http://localhost:3000. Node.js 20+ recommended.

## GitHub safety

The provided GitHub-ready package **intentionally excludes** the full uploaded Mac project, `.env`, all secrets, sqlite3 databases, workflow callback tokens, operational logs, `.venv`, n8n credentials, and personal files. Do not upload your original 97 MB project ZIP directly to a public repository. The supplied `.gitignore` protects common local copies but cannot undo a leaked secret after a public commit.

## Reference scoring specification

Ported from the uploaded `risk_engine.py` `AEGIS-RISK-POLICY-2.0`, with the following point weights:

| Signal | Risk points |
|---|---:|
| Amount ≥10× historical average | 30 |
| Amount ≥5× | 25 |
| Amount ≥3× | 15 |
| Amount ≥1.5× | 5 |
| Different foreign location | 20 |
| Different domestic city | 15 |
| New device | 15 |
| New beneficiary | 15 |
| ≥3 failed authentication attempts | 20 |
| 1–2 failed attempts | 8 |
| Outside usual time window | 10 |

LOW: 0–29; MEDIUM: 30–69; HIGH: ≥70. If critical evidence is invalid, return UNKNOWN/fail-safe rather than LOW.

*AEGIS = AI analyses. Policy governs. Human decides.*
