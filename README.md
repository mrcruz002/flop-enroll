# FLOP Enroll — devnet gateway key helper

A single-file website that gets anyone a FLOP devnet gateway key in about 2 minutes:
connect wallet → get 25 free FLOP from the faucet → sign one message → receive a
24-hour gateway key. No seed phrase, no sign-up, no server — the page talks only to
FLOP's own public API, and the key is stored only in the visitor's own browser.

## Open it

Double-click `index.html` in Chrome (or any Chromium browser) with Talisman,
SubWallet, Nova or Polkadot.js installed.

> If the wallet doesn't appear: in `chrome://extensions`, turn on **"Allow access to
> file URLs"** for the wallet, then reload. Or serve the folder over http instead:
> `python3 -m http.server` from this folder, then open `http://localhost:8000`.

## Host it

Any static host works — no backend needed. Drag the folder into Vercel, Netlify,
Railway (static), GitHub Pages, or IPFS. `index.html` is the whole site.

## What it does

1. **Connect wallet** — detects every installed Substrate wallet, lists accounts.
2. **Get test FLOP** — calls the public faucet (`POST faucet.flop.network/fund`),
   25 FLOP, once per 24h. Skippable if the wallet already holds FLOP.
3. **Sign & enrol** — fetches a fresh 5-minute challenge from
   `api.flop.network`, asks the wallet to sign it, submits the enrolment.
   Handles the daily enrolment cap (HTTP 503) and bad signatures (401) with plain
   explanations.
4. **Your key** — shows the key masked, with Show / Copy / Download, plus model,
   remaining quota and expiry. A "Check quota" button refreshes the allowance.
   The key is saved in the visitor's browser local storage only, per wallet
   address, with a "Forget this key" wipe.

## Design

Ash, black and white only. Dark theme by default, light theme on the toggle in the
header (choice saved in the browser). Space Grotesk / Inter / IBM Plex Mono.

## Notes

- Unofficial community tool — not affiliated with FLOP.
- Devnet FLOP is worthless and earns nothing; the flow is practice for testnet.
- The page never asks for a seed phrase and never sends the key anywhere except
  FLOP's own API (as a bearer token when checking quota).
- FLOP's enrolment cap sometimes fills before noon UTC; the page says so plainly
  and the retry is after midnight UTC (01:00 WAT).
