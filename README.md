# Gates of Abyssinia

Static production frontend for Gates of Abyssinia, prepared for the Le Catcher
catalog entry (`le-catcher`) and audience `kassino_le_catcher_etb_prod`.

## Deployment

Import this repository into Vercel using the **Other** framework preset and
the repository root as the output directory. No install or build step is
required. `vercel.json` contains the static files' routing, asset redirects,
and Go launcher rewrites. There are no Vercel functions in this repository.

The client is configured for:

- Frontend: `https://le-catcher.coregames.io`
- Gameplay and session API: `https://api.orginals.io`
- Artwork and audio: the immutable Cloudflare asset URLs in `vercel.json`
- Launch: `/` or `/launch`, forwarding operator credentials to the Go launcher

The operator launch confirms issuer `kassino` and audience
`kassino_le_catcher_etb_prod`. Backend migration 033 registers that exact
binding. The frontend and production backend are deployed separately.

## Production math v5

The backend uses `abyssinia-97-hit20-cap200-v5`: **97% theoretical RTP,
20% positive-payout paid rounds, 80% zero payouts, a 200× complete-round cap,
and ETB 4–4,000 bets**. Positive payouts include returns below the stake.
Respins and free spins belong to their originating paid round.

The original symbol paytable and ten paylines are retained. Probabilities use
a custom weighted outcome catalog, not the original provider's reel strips.
Both king choices complete the same total award using different bonus sequences.

| Award | Probability per paid round |
| --- | ---: |
| 2× | 6.5% |
| 5× | 5% |
| 10× | 2.5% |
| 15× | 0.2% |
| 20× | 0.15% |
| 30× | 0.1% |
| 50× | 0.1% |
| 100× | 0.03% |
| 200× | 0.02% |

The 2×, 5× and 10× tiers account for 70% of positive rounds. The remaining
smaller tiers are 0.5×, 0.7×, 1×, 1.5×, 2.5×, 3× and 7.5×.
One million equally likely tickets pay exactly 9,700,000 payout tenths and
contain 200,000 positive awards. The weights do not adapt to a player's results.

The v5 simulation of 10 million rounds returned **97.295578% RTP** and
**19.9983% positive payouts**; see [math-report.json](math-report.json).
The sampled RTP 95% interval is 97.021336–97.569820%, narrowly excluding the
exact target. The weights were not retuned or the seed replaced to hide
sampling variation. Exact ticket enumeration confirms the configured 97%.
The older [10m](math-report-v4.json) and [100m](math-report-100m.json) reports
are historical **v4** results and do not validate the changed v5 distribution.

A separate [one-million-round audit](rules-audit.json) observed every symbol
and found no violations in its implemented rule checks. The six random bonuses
were independently tested at both levels; free-spin counters, Bless progression,
king collection and the round cap were checked. [Read the audit's scope and
remaining differences](RULES-AUDIT.md).

Math, JWT validation, balances and durable wallet settlement remain in the
separate Go backend. This repository contains only the compiled frontend,
routing and reports. Pushing it does not activate the production game.

## Asset provenance

The v7 theme uses carved Ethiopian king portraits, Ge'ez numerals, and imperial
stone lions. The underlying captured game client, unchanged special symbols,
effects, and audio originate from the Spribe/Bambuk demo. The theming does not
claim authorship of those retained assets.

`release.json` records the prepared release's targets and source revision.
