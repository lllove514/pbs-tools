# Peanut Butter Sundays — map and assistant

Two things I built for Peanut Butter Sundays, a youth-founded 501(c)(3) nonprofit in Los Angeles
that I co-founded: a homelessness map and a chat assistant. This repository holds only those two,
pulled out of the organization's website so the code I wrote stands on its own. The full site is
at https://peanutbuttersundays.com.

Live demo: https://lllove514.github.io/pbs-tools/

## What is here

### `map/` — LA homelessness map
A choropleth of Los Angeles County's eight Service Planning Areas (SPAs), shaded by the number of
people experiencing homelessness. Leaflet on the front, one static GeoJSON of the county
boundaries, and the counts from the 2025 LAHSA Greater Los Angeles Homeless Count. No build step,
no backend. Open `map/index.html` and it runs.

### `chat/` and `worker/` — Jelly, the assistant
Jelly answers questions about the nonprofit: programs, how to donate, how to volunteer.

- `chat/jelly.js` and `chat/jelly.css` are the widget. Vanilla JavaScript, no framework, no build
  step. Conversation history stays in memory, nothing is written to the browser's storage.
- `worker/` is the backend, a small Cloudflare Worker. It holds the API key so the key never
  reaches the browser, checks the request origin, sanitizes input, and keeps the assistant to a
  fixed set of facts about the nonprofit so it does not wander.

`chat/demo.html` loads the widget on its own so you can try it.

## Run it

The map needs nothing:

```
python3 -m http.server 8000
```

Then open http://localhost:8000/map/ for the map, or http://localhost:8000/chat/demo.html for the
assistant. The assistant's replies need the backend running, see `worker/README.md` for the key
setup and `npx wrangler dev`. Point `JELLY_ENDPOINT` in `chat/jelly.js` at your Worker URL, and
add your site's origin to the allowlist in `worker/index.js`.

## Notes

The map data and method are documented in `map/README.md`. The backend setup and guardrails are in
`worker/README.md`.
