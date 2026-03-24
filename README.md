# Bel Essex (Valeo) — PV Monitoring Dashboard

Live solar PV monitoring dashboard for the Bel Essex (Valeo) site.

## Architecture

```
index.html                  ← Dashboard (GitHub Pages)
process_plant_data.py       ← Data processor (hourly via GitHub Actions)
scraper.py                  ← FusionSolar scraper (to be added)
data/
  raw_report.xlsx           ← Latest scraped report
  processed.json            ← Dashboard-ready JSON
  history.json              ← Rolling 30-day historical data
  alert_state.json          ← Telegram alert state
.github/workflows/
  scrape.yml                ← Automated hourly scrape + process
```

## Key Design Decisions

- **Purely data-driven thresholds** — no hardcoded `DAILY_EXPECTED_KWH`. All baselines (daily avg, min, max, percentile bands) are derived from the rolling 30-day history.
- **Bootstrap grace period** — alerts are suppressed until at least 7 days of data have been collected (`MIN_HISTORY_DAYS`).
- **Irradiation-scaled alerts** — expected generation is adjusted by today's actual irradiation vs the 30-day average, preventing false alerts on cloudy days.

## Setup

1. **Secrets** — add to GitHub repo → Settings → Secrets → Actions:
   - `FUSIONSOLAR_USER` / `FUSIONSOLAR_PASS`
   - `TELEGRAM_BOT_TOKEN` / `TELEGRAM_CHAT_ID` (optional)

2. **GitHub Pages** — enable from Settings → Pages → Source: `main` branch, root.

3. **Scraper** — add `scraper.py` for FusionSolar data extraction.

## Site Details

| Field | Value |
|-------|-------|
| Plant | Bel Essex (Valeo) |
| Location | -33.7837, 25.4210 |
| Timezone | SAST (UTC+2) |
| Data source | FusionSolar |
