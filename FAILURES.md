# Failure modes, fixes, and results

Honest engineering notes for this project. Nothing here is invented for polish.

## What can go wrong

- **Upstream price API downtime or rate limits.** Impact: missed daily row. Mitigation: GitHub Actions can be re-run manually; historical CSV retains prior days.
- **Partial write (raw JSON ok, processed CSV/report not).** Impact: inconsistent artifacts. Mitigation: keep pipeline steps in `src/` small and rerunnable for the same date.
- **Actions schedule silently skipped (repo inactivity policies).** Impact: stale `reports/latest.md`. Mitigation: manual workflow_dispatch (README).
- **Schema change in the API response.** Impact: parser breakage. Mitigation: inspect `data/raw/YYYY-MM-DD.json` and fix parsers before appending history.

## What went wrong

**No recorded production incident in this repo yet.** Commit history is dominated by successful `daily fintech signal update` runs plus an MIT license commit. No issue tracker entries describing a failed collection day.

## How it was resolved

- Scheduled GitHub Actions workflow plus manual trigger documented in README.
- Outputs split into raw JSON, processed `history.csv`, and `reports/latest.md` for easy diffing when something looks off.

## Checked on 1 November 2026

- The pipeline ran cleanly from a fresh virtual environment. No tests exist yet.
- GBP/USD repeats at weekends because the ECB publishes working day rates only. This is expected, not a fault, and is now noted in the README.

## Results

- Successful demo: run the collector locally or via Actions; confirm new `data/raw/` file and updated `reports/latest.md` for BTC/ETH/GBP.
- No uptime percentage or API SLA figures are published in-repo.
