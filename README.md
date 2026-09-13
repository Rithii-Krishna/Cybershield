# CyberShield

Password strength analyser with an encrypted local vault. Static single-page app:
no build step, no dependencies, no backend. `index.html` is the whole application.

## Deploy

Push to GitHub and import into Vercel (Framework Preset: **Other**, build command and
output directory empty), or run `vercel --prod` from this folder.

## Features

- **Analyse** — pattern decomposition with a segment rail showing why a password fails
- **Reuse meter** — flags exact reuse and near-copies ("Summer2024!" vs "Summer2025!")
  against your vault and this session. No breach-corpus lookup: that needs a network call.
- **Make this one stronger** — three measured rewrites of the password you typed
- **Generate** — passphrases, or characters with per-class control (a–z, A–Z, 0–9, symbols)
- **History** — session only, masked
- **Vault** — encrypted local storage, optional fingerprint unlock

## Features

- **Analyse** — pattern decomposition with a per-character segment rail, crack times
  under four attacker models, and plain-language findings.
- **Reuse meter** — checks the typed password against your vault (when unlocked) and this
  session's history, catching exact matches, same-base rewrites (`…41` → `…99`) and
  near-twins within two edits. Also flags passwords on the global most-used list.
- **What each change would buy you** — every suggested fix is run back through the
  analyser and reported as a measured bit delta, so patching and replacing can be compared
  honestly.
- **Generator** — passphrases, or characters with a–z / A–Z / 0–9 / Symbols toggles. One
  character from each ticked set is guaranteed, placed by an unbiased Fisher-Yates shuffle.
- **Band reactions** — the input ring takes the colour of the verdict, and a one-shot
  animation fires when the band *changes* (a shake and red flash on Trivial, a settle on
  Strong, a double pulse on Excellent). It does not re-fire while you keep typing inside
  the same band, and is disabled entirely under `prefers-reduced-motion`.
- **Save prompt** — after you pause, offers to keep the password. Yes carries it into the
  vault form; No dismisses and does not ask again for that password.
- **Vault** — black and gold, visually separate from the rest of the app. See below.

## What is stored where

- **Analysis** — nothing is stored. No network requests are made at all.
- **History** — in memory only, for the current tab. Gone on reload.
- **Vault** — the only thing written to disk, in `localStorage`, encrypted:
  - random 256-bit data key (DEK) encrypts the contents with AES-256-GCM
  - the DEK is wrapped by a key derived from your master password
    (PBKDF2-HMAC-SHA256, 600,000 iterations, random 16-byte salt)
  - optionally wrapped a second time by a key derived from a WebAuthn PRF output,
    which is what makes fingerprint unlock real rather than cosmetic
  - the master password is never stored; a wrong one fails GCM authentication

Requires https. Browsers only expose `crypto.subtle` in a secure context, and the app
refuses to run the vault without it rather than storing anything unencrypted.

## Editing

- `SPLASH_MS` — how long the opening animation lasts (default 4600 ms)
- `WORDS` / `EXTRA` — dictionary for matching and passphrase generation
- `BANDS` / `KNOTS` — strength thresholds and the 0–100 score curve
- `SCENARIOS` — assumed attacker guess rates
- `KDF_ITER` — PBKDF2 iteration count
