# Gates of Abyssinia

Static production frontend for Gates of Abyssinia, prepared for the Le Catcher
catalog entry (`le-catcher`) and audience `kassino_le_catcher_etb_prod`.

## Deployment

Import this repository into Vercel using the **Other** framework preset and
the repository root as the output directory. No install or build step is
required. `vercel.json` contains the static files' routing, asset redirects,
and Go launcher rewrites. There are no Vercel functions in this repository.

The client is configured for:

- Frontend: `https://gates-of-abyssinia.vercel.app`
- Gameplay and session API: `https://api.orginals.io`
- Artwork and audio: the immutable Cloudflare asset URLs in `vercel.json`
- Launch: `/` or `/launch`, forwarding operator credentials to the Go launcher

The production Go adapter and instance registration must be deployed before
activating the frontend. Le Catcher's audience is confirmed; its JWT issuer
still needs verification from a fresh operator launch. Publishing this
repository does not register the instance or deploy the backend.

## Production math

The backend math version is `abyssinia-97-hit20-cap200-v4`:

| Setting | Value |
| --- | --- |
| Theoretical RTP | 97% |
| Positive-payout paid rounds | 20% |
| Zero-payout paid rounds | 80% |
| Maximum complete-round payout | 200× the bet |
| Bet range | ETB 4–4,000 |

The 10-million-round simulation returned 96.674717% RTP,
19.99183% positive payouts, and 80.00817% zero payouts. Its RTP 95%
interval was 96.277893–97.071541%; exact ticket enumeration confirms 97% RTP.
A positive payout includes a return below the stake; free spins and respins
are included in their originating paid round. See [math-report.json](math-report.json).

This repository contains the compiled client and routing only. Math, JWT
validation, live balance, and durable wallet settlement remain in the separate
Go backend. No operator token or editable artwork archive is included.

## Asset provenance

The v7 theme uses carved Ethiopian king portraits, Ge'ez numerals, and imperial
stone lions. The underlying captured game client, unchanged special symbols,
effects, and audio originate from the Spribe/Bambuk demo. The theming does not
claim authorship of those retained assets.

`release.json` records the prepared release's targets and source revision.
