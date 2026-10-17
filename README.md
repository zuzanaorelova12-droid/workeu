# WorkEU proxy

Server-side job aggregation for WorkEU. API keys stay on the server; the browser only talks to `/api`.

## Run
```
cp .env.example .env     # fill in keys for the sources you want
npm test && npm start    # http://localhost:8787  (serves the app + /api)
```
`public/index.html` is the WorkEU frontend; the server injects `window.WORKEU_API="/api"` so live jobs switch on automatically. Deploy anywhere that runs Node 18+ (Render, Fly.io, Railway, a VPS, Docker). Put HTTPS in front and set `TRUST_PROXY=true` behind a proxy.

## API
- `GET /api/health` – which sources are enabled
- `GET /api/jobs?q=&city=&country=&page=` – normalized, deduplicated jobs + per-source status + attribution
- `GET /api/jobs/:id` – detail (e.g. `ba:10001-123-S`); BA fetches the full description lazily

## Sources and rules
- **Bundesagentur für Arbeit** (disabled in WorkEU; leave `BA_API_KEY` empty): BA offers no official public API. This uses the endpoint its own job portal uses, with the public client id documented by the bundesAPI community. It can change or be restricted without notice. Before commercial launch, check BA terms and ask BA about an official partner interface.
- **Adzuna**: needs free API keys; display "Jobs by Adzuna" and link to the original listing (the response includes `attribution`).
- **Arbeitnow**: free API; verify current terms; country is left unknown because the listing doesn't state it.
- **StepStone / Indeed**: no scraping. Add a source in `SRC` only once you have a licensed feed/contract.
- Every job keeps `source` + `sourceUrl`. Unknown fields stay `null` (the app shows "Not specified"). Adzuna's *predicted* salaries are dropped on purpose.
- Privacy: no user data is sent to job sources; only the search terms. Add a privacy-policy entry for the third-party sources.
