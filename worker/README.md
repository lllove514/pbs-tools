# Jelly — chat backend (Cloudflare Worker)

Jelly is the guardrailed chat assistant for Peanut Butter Sundays. This Worker is the backend: it
receives chat messages from the static widget, calls the Anthropic (Claude) API, and returns a
reply. The API key lives only in the Worker's secrets, never in the browser.

The frontend widget is in `../chat/` (`jelly.js`, `jelly.css`).

## Files

- `wrangler.jsonc` — Worker config (name, entry point, compatibility date).
- `index.js` — the Worker. CORS, input sanitizing, guardrails, Claude call.
- `.dev.vars.example` — template for local secrets. Copy to `.dev.vars`.

## Prerequisites

- Node.js installed (so `npx` is available).
- An Anthropic API key from https://console.anthropic.com.

## Local development

1. Copy the secrets template and paste your key:

   ```sh
   cp .dev.vars.example .dev.vars
   # then edit .dev.vars and set ANTHROPIC_API_KEY=sk-ant-...
   ```

   `.dev.vars` is gitignored and is read automatically by `wrangler dev`.

2. Run the Worker locally:

   ```sh
   npx wrangler dev
   ```

   This serves the Worker at `http://localhost:8787`. To test the widget against it locally,
   temporarily point `JELLY_ENDPOINT` in `../chat/jelly.js` at that URL.

## Deploy

1. Log in to Cloudflare (one time per machine):

   ```sh
   npx wrangler login
   ```

2. Store the Anthropic key as a production secret (one time, or when it rotates):

   ```sh
   npx wrangler secret put ANTHROPIC_API_KEY
   ```

3. Deploy:

   ```sh
   npx wrangler deploy
   ```

After deploy, the live URL is printed and looks like:

```
https://jelly.<your-subdomain>.workers.dev
```

Paste that URL into `JELLY_ENDPOINT` at the top of `../chat/jelly.js` so the widget talks to the
deployed Worker.

## Notes

- Allowed origins are listed in `index.js` (`isAllowedOrigin`). Add any new domains there.
- The system prompt and `PBS_KNOWLEDGE` (mission plus FAQ) live in `index.js`. Search for `[TODO`
  and replace the placeholders with real PBS content.
