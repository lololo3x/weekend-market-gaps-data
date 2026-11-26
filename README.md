# Weekend Market Gaps Dataset (CC-BY-4.0)

Open historical dataset of **indicative** weekend price moves and reopen gaps for gold, silver, oil, major equity indices, US stocks, and FX pairs derived from 24/7 perpetual futures on **Hyperliquid `xyz` (HIP-3)** markets.

- **Live Dashboard:** [weekendmarkets.site](https://weekendmarkets.site)
- **Methodology & Dataset Docs:** [weekendmarkets.site/methodology/#dataset](https://weekendmarkets.site/methodology/#dataset)
- **Editorial Disclosure & Neutrality Policy:** [weekendmarkets.site/disclosure/](https://weekendmarkets.site/disclosure/)

This repository mirrors the live API endpoints from `weekendmarkets.site` and is automatically synced daily at **01:00 UTC** via GitHub Actions.

---

## Direct Download URLs

### Live API (real-time server output)
- **CSV:** `https://weekendmarkets.site/api/history.csv`
- **JSON (v1):** `https://weekendmarkets.site/api/history.v1.json`

### Versioned GitHub Snapshot (daily sync)
- **CSV:** `https://raw.githubusercontent.com/lololo3x/weekend-market-gaps-data/main/data/history.csv`
- **JSON (v1):** `https://raw.githubusercontent.com/lololo3x/weekend-market-gaps-data/main/data/history.v1.json`

---

## Current Coverage

As of the initial snapshot synced on **2026-11-26** (updated automatically as new weekends close):

- **Total records:** 236 asset-weekend observations
- **Weekends covered:** 31 Friday-to-Monday windows (`2026-04-24` to `2026-11-20`)
- **Tracked assets (16):**
  - **Metals (3):** `gold`, `silver`, `copper`
  - **Energy (3):** `wti`, `brent`, `natgas`
  - **Equity Indices (4):** `spx`, `ndx`, `jp225`, `kr200`
  - **US Equities (3):** `aapl`, `nvda`, `tsla`
  - **Major FX (3):** `eurusd`, `gbpusd`, `usdjpy`

> Note: Coverage depth varies by asset depending on when each contract was added to tracking (for example, core series such as `wti`, `eurusd`, `gold`, `spx`, `silver`, `brent`, `ndx`, `usdjpy`, and `gbpusd` span 23–29 weekends, while recently added markets begin with `2026-11-20`).

---

## Files & Schema

| File | Format | Description |
|---|---|---|
| [`data/history.csv`](./data/history.csv) | CSV (UTF-8, comma-delimited) | Flat table with header row and one row per `(asset, weekend)` observation. |
| [`data/history.v1.json`](./data/history.v1.json) | JSON (schema `version: 1`) | Full structured payload containing top-level metadata (`version`, `generated_at`, `source`, `docs`, `fields`, `count`) and the `records` array. |

### Record Fields

Both `data/history.csv` columns and `data/history.v1.json` `records[]` objects share the exact same 9 fields:

| Field | Type | Nullable | Description |
|---|---|---|---|
| `asset` | `string` | No | Asset identifier (`gold`, `silver`, `wti`, `brent`, `ndx`, `spx`, `eurusd`, `usdjpy`, `gbpusd`, `nvda`, `tsla`, `aapl`, `natgas`, `copper`, `kr200`, `jp225`). |
| `weekend` | `string` | No | ISO-8601 Friday date (`YYYY-MM-DD`) identifying the weekend window. |
| `lastClose` | `number` | No | Perp price in the 1-hour candle ending at the official market close. |
| `preReopenPrice` | `number` | No | Perp price one hour before the official market reopen. |
| `reopenPrice` | `number` | No | Perp price at the official market reopen (1-hour candle open). |
| `cashOpen` | `number` | Yes | Perp price at Monday 9:30 a.m. ET (populated for equity indices and US equity markets: `spx`, `ndx`, `jp225`, `kr200`, `aapl`, `nvda`, `tsla`; `null` / empty for commodities and FX). |
| `weekendMovePct` | `number` | No | Percentage move during closure: `(preReopenPrice - lastClose) / lastClose * 100`. |
| `reopenGapPct` | `number` | No | Percentage gap at reopen: `(reopenPrice - lastClose) / lastClose * 100`. |
| `sameDirection` | `boolean` | No | Whether `weekendMovePct` and `reopenGapPct` have the same sign (`true` / `false`). |

---

## Data Source & Methodology

- **Venue / Market:** Hyperliquid `xyz` (HIP-3) 24/7 perpetual futures
- **Upstream Endpoint:** `POST https://api.hyperliquid.xyz/info` (`candleSnapshot`, `1h` candles)
- **Measurement Basis:** Perp-to-perp weekend records. No official Monday exchange open is stored in this dataset, and individual rows carry no per-row fetch timestamp.
- **Full Documentation:** See [weekendmarkets.site/methodology/#dataset](https://weekendmarkets.site/methodology/#dataset) for close/reopen schedule definitions, liquidity caveats, and known limitations.

---

## Disclaimer & Neutrality

- **Indicative prices only:** All values are derived from 24/7 Hyperliquid `xyz` (HIP-3) perpetual futures via public APIs. They are **not official exchange settlement prices** and may diverge from traditional futures, spot, or cash-market opens due to weekend liquidity, funding, or basis differences.
- **Not investment advice:** Nothing in this dataset or repository constitutes trading signals, financial advice, or an investment recommendation.
- **Free & neutral:** Weekend Market Prices is free to access with no sign-up required and currently carries **no affiliate, referral, or commission links** (see [Editorial Disclosure](https://weekendmarkets.site/disclosure/)).

---

## License & Citation

This dataset is released under the **Creative Commons Attribution 4.0 International (CC-BY-4.0)** license. See [`LICENSE`](./LICENSE) for the full legal text.

You are free to share and adapt the data for any purpose, including commercial and academic use, provided you give appropriate attribution:

### Plain-text Attribution
> Weekend Market Prices (2026). *Weekend Market Gaps Dataset (Hyperliquid xyz / HIP-3 24/7 Perps)*. Available at https://weekendmarkets.site and https://github.com/lololo3x/weekend-market-gaps-data (CC-BY-4.0).

### BibTeX
```bibtex
@dataset{weekendmarkets_gaps_2026,
  author       = {{Weekend Market Prices}},
  title        = {Weekend Market Gaps Dataset (Hyperliquid xyz / HIP-3 24/7 Perps)},
  year         = {2026},
  url          = {https://github.com/lololo3x/weekend-market-gaps-data},
  note         = {Live dashboard and API at https://weekendmarkets.site; licensed under CC-BY-4.0}
}
```
