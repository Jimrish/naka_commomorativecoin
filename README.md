# Naga Punk – Temple Coin Run

Browser endless runner with X-handle runners and a global top-10 leaderboard.
Plain HTML + Canvas, plus three small serverless functions in `api/`.

## Controls
- Desktop: Space / ↑ jump (twice = double jump), ↓ slide, P pause, M mute
- Touch: JUMP / SLIDE buttons, or swipe up / down

## Deploy on Vercel (with the global leaderboard)
1. Push this whole folder to a GitHub repository (`index.html`, `assets/`, `api/`, `package.json`, `vercel.json`).
2. On vercel.com: Add New → Project → import the repository → Framework Preset "Other" → Deploy.
3. In the project, open the **Storage** tab (or Marketplace) → add **Upstash Redis** (free plan) → connect it to this project.
   Vercel adds `KV_REST_API_URL` and `KV_REST_API_TOKEN` automatically.
4. Redeploy once (Deployments → ⋯ → Redeploy). The leaderboard now works.
5. Optional: add an environment variable `LB_SECRET` (any long random text) to sign run tokens with your own secret.

Without the database, the game still works. The leaderboard then shows only scores saved on that device.

## How scores are protected
- Each run gets a signed single-use token from `/api/start`.
- `/api/score` rejects scores that are impossible for the run time and coins, runs longer than real elapsed time,
  invalid handles, reused tokens, and more than 30 submissions per 10 minutes per IP.
- X handles are not verified. This stops casual cheating, not a determined attacker.

## Run locally
    python3 -m http.server 8000     # game + local-only leaderboard (the api/ folder needs `vercel dev`)

## Files
- `index.html` – the game
- `api/` – `start.js`, `score.js`, `leaderboard.js` (+ `_lib.js`)
- `assets/` – character, 10 coins, title art
- `play-offline.html` – single-file copy with images built in (local leaderboard only)
