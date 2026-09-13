# CyberShield

A password strength analyzer that explains *why* a password is weak, not just that it is.
Static single-page app: no build step, no dependencies, no backend. `index.html` is the
whole application.

## Deploy to Vercel

### Option A — CLI

```bash
npm i -g vercel
cd cybershield
vercel          # preview URL
vercel --prod   # production
```

Accept every default. When asked for the code directory, enter `./`.

### Option B — from GitHub

1. Push this folder to a GitHub repo.
2. Vercel dashboard → **Add New → Project** → import the repo.
3. Framework Preset: **Other**
4. Build Command and Output Directory: *leave empty*
5. **Deploy**

## vercel.json

Sets response headers only. The notable one:

    connect-src 'none'; form-action 'none'

The browser blocks any outbound request or form submission from this page. CyberShield
already makes none — this makes that enforced rather than promised.

## Editing

All code is in `index.html`:

- `WORDS` / `EXTRA` — dictionary used for matching and for generating passphrases
- `BANDS` / `KNOTS` — strength thresholds and the 0–100 score curve
- `SCENARIOS` — assumed attacker guess rates
