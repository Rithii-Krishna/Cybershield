# CyberShield

Password strength analyser with an encrypted local vault. Static single-page app:
no build step, no dependencies, no backend, and **no external requests of any kind** —
including no web fonts, which is why the wordmark is built from CSS and SVG rather than
a downloaded typeface.

## Screens

- **Splash** — the wordmark resolves character by character out of scrambling glyphs while
  the shield and padlock draw themselves. Skippable with any key or click; `SPLASH_MS`
  controls the length.
- **Passcode gate** — optional. Set one under Vault → App passcode and it is asked for on
  open. It is a lock on the front door, not authentication: there is no server. It does not
  protect the vault, which has its own encryption.
- **Analyse** — segment rail, crack times, findings, reuse meter, measured improvement table,
  generator.
- **History** — this session only, masked by default, held in memory.
- **Vault** — black and gold. Switching to it re-themes the whole page, not one panel.

## Palette

Brand violet `#8B6BFF` on a charcoal-indigo ground. Strength runs
`#FF4D5E → #FF8F3D → #E3C13F → #5FD97A → #3FD9C0`. The vault overrides the theme through
`body.vault-mode` to `#08080A` with gold `#D9B451`.

## What is stored where

- **Analysis** — nothing stored, no requests made.
- **History** — memory only, gone on reload.
- **Vault** — `localStorage`, encrypted: random AES-256 data key, wrapped by a key derived
  from your master password (PBKDF2-HMAC-SHA256, 600,000 iterations) and optionally by a
  WebAuthn PRF output for fingerprint unlock. The master password is never stored.
- **App passcode** — `localStorage`, stored as an AES-GCM verifier under PBKDF2 (200,000
  iterations). The passcode itself is never stored.

Requires https: browsers only expose `crypto.subtle` in a secure context, and the app
refuses to run the vault without it rather than storing anything unencrypted.

## Editing

`SPLASH_MS`, `WORDS` / `EXTRA`, `BANDS` / `KNOTS`, `SCENARIOS`, `KDF_ITER`.
