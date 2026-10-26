# Python Trading Simulator

**Solana automation, evaluation and risk controls**

A Python research platform for testing Solana trading workflows. It combines repeatable paper simulations, token discovery, safety filters, guarded execution, structured audit records, dashboards and Discord alerts.

> **Status:** Research and testing project. Live execution is limited to supervised, small-scale pilots. It is not an unattended production system, and no profitable strategy is claimed.

![System architecture](docs/crypto-sniper-architecture-v5.svg)

## What this project demonstrates

- **End-to-end systems work:** Move a workflow from paper simulation through dry runs and supervised live validation, with explicit safety boundaries.
- **Operational reliability:** Record events, monitor runs, investigate failures, use regression tests and maintain recovery procedures.
- **Evidence-driven improvement:** Analyse outcomes, classify failure reasons and check changes against fresh data instead of judging by isolated examples.

## How it works

1. A preset, seed or signal source provides candidates.
2. Discovery and scoring identify possible trades.
3. Token, liquidity, route and risk checks decide whether to proceed.
4. An execution adapter uses paper, dry-run or guarded live mode.
5. Results are recorded in SQLite, JSON, CSV or JSONL and reviewed through dashboards and alerts.

## Evaluation and quality checks

The public repository includes a regression suite covering simulation, execution adapters, safety limits, reporting and live-path controls. Run it before changing execution behaviour.

My local evaluation work has included **3,000+ recorded closed outcomes** and **6,000+ transaction rows**. I reviewed failure categories, segmented performance, compared configurations and tracked operational reliability separately from strategy results. These counts describe analysed records, not profitability; the full local run history is not published here.

The main improvement loop is: **observe → classify → diagnose → change → test → monitor**. For example, analysis pointed towards candidate selection quality rather than exit handling, leading to stricter selection and confirmation checks.

## Screenshots

### Strategy review

![Strategy review dashboard](docs/images/strategy-review-sample.png)

### Operations monitoring

![Operations dashboard](docs/images/live-ops-dashboard-crop.png)

### Validation records

![Validation bundle viewer](docs/images/validation-bundles-sample.png)

## Technology

| Area | Tools |
| --- | --- |
| Core | Python |
| Market integrations | DexScreener, Jupiter, Solana JSON-RPC |
| Storage and exports | SQLite, JSON, CSV, JSONL |
| Dashboards | HTML, CSS, JavaScript |
| Operations | PowerShell, Windows batch scripts, Discord webhooks |
| Tests | Pytest |

## Try the paper simulator

On Windows PowerShell:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m src.runner.paper_sim_runner --steps 100 --seed 7 --export-json-dir data\exports
```

Open the dashboard from the repository root:

```powershell
python -m http.server 8000
```

Visit `http://localhost:8000/frontend/index.html` and select an exported JSON, CSV or JSONL file.

Run regression tests:

```powershell
python run_regression_tests.py
```

Database-writing tests should not run in parallel on Windows because `data/sniper.db` may lock.

## Live-mode boundaries

- Use paper mode first. Supervised live pilots require an operator, small test sizes and strict limits.
- Paper results do not fully account for fees, slippage, latency, partial fills or live conditions.
- Keep wallets, private keys, `.env` files and Discord webhook URLs out of Git.
- Review [Live Readiness Notes](LIVE_READINESS_NOTES.md), [Known Limitations](KNOWN_LIMITATIONS.md) and the [V1 Release Checklist](V1_RELEASE_CHECKLIST.md) before experimenting with live features.

This repository documents technical research, operational testing and risk controls. It is not financial advice.
