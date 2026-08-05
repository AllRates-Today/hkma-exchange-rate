# Hong Kong Monetary Authority Exchange Rate API client

Official **Hong Kong Monetary Authority** (Hong Kong) daily exchange rates in Node.js / TypeScript — ~19 currencies against the HKD, with history back to 1981. Zero dependencies, works in Node 18+, Bun, Deno, and edge runtimes (uses global `fetch`).

These are the *published central bank rates* required for tax filings, customs valuations, audits, and compliant invoicing — not moving market rates. Every response carries the bank's own publication date.

Powered by [AllRatesToday](https://allratestoday.com/central-bank-rates-api/hkma/). Get a free API key at [allratestoday.com/register](https://allratestoday.com/register) — 300 requests/month, no credit card.

## Install

```bash
npm install hkma-exchange-rate
```

## Quick start

```js
import { getRate, getLatestRates } from 'hkma-exchange-rate';

// One pair at the official Hong Kong Monetary Authority rate
const pair = await getRate('USD', 'HKD', { apiKey: 'art_live_...' });
console.log(pair.rate, pair.rate_date); // e.g. USD -> HKD on the bank's own date

// The bank's full published table
const table = await getLatestRates({ apiKey: 'art_live_...' });
console.log(table.rate_date, table.rates.length);
```

## Historical data (paid plans)

```js
import { getRatesForDate, getHistory } from 'hkma-exchange-rate';

// The official table for an invoice date — weekends/holidays return the
// most recent published date, flagged via published_on_requested_date.
const day = await getRatesForDate('2026-06-30', { apiKey: 'art_live_...' });

// Daily series for one pair
const series = await getHistory(
  { source: 'USD', target: 'HKD', from: '2026-01-01' },
  { apiKey: 'art_live_...' }
);
```

## Published vs derived rates

If Hong Kong Monetary Authority does not print a pair directly, the API resolves it from the bank's table (inverse, or a cross rate via HKD) and flags it `derived: true` with the `method` — so official and computed values are never confused.

## Notes

- Every request counts toward your AllRatesToday monthly quota. Rates change once per business day — cache a day's table locally and a small quota goes a long way.
- Latest rates are on every plan (including free); historical dates and time series need a [paid plan](https://allratestoday.com/pricing/).
- Full API reference: [allratestoday.com/docs#central-bank](https://allratestoday.com/docs/#central-bank) · All covered banks: [central bank rates API](https://allratestoday.com/central-bank-rates-api/)

## License

MIT
