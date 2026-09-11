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

## Production math v6

`abyssinia-97-hit20-cap100-cash100k-v6` retains the captured symbol paytable,
ten paylines, and evaluated bonus transformations. It uses custom outcome
weights, not the original provider's reel probabilities. Every complete paid
round, including respins, either king choice, and treasury, pays at most the
lower of **100× stake or 100,000 ETB**. The bet range remains **4–4,000 ETB**.

The target is exactly **97% theoretical RTP**, **20% positive-payout rounds**,
and **80% zero payouts**. Positive payouts include returns below the stake.
Weights depend only on the stake; no player identity, balance, history, or
recent GGR is used. Already accepted v5 wagers retain their stored awards.

### Base weights

These apply before cent-rounding and cash-cap compensation. For standard bet
options up to 1,000 ETB they are the final probabilities. Both king choices
complete the same total award with different bonus sequences.

| Award | Tickets per million | Probability |
| --- | ---: | ---: |
| 0× | 800,000 | 80% |
| 0.5× | 6,000 | 0.6% |
| 0.7× | 5,000 | 0.5% |
| 1× | 4,000 | 0.4% |
| 1.5× | 10,000 | 1% |
| 2× | 65,000 | 6.5% |
| 2.5× | 10,000 | 1% |
| 3× | 14,000 | 1.4% |
| 5× | 50,000 | 5% |
| 7.5× | 5,000 | 0.5% |
| 10× | 24,500 | 2.45% |
| 15× | 2,000 | 0.2% |
| 20× | 1,500 | 0.15% |
| 30× | 1,000 | 0.1% |
| 50× | 1,500 | 0.15% |
| 100× | 500 | 0.05% |

The former 200× tickets now pay 100×. Moving 500 tickets from 10× to 50×
restores the lost return: 50× frequency rises from 0.10% to **0.15%**, while
15× stays at **0.20%**. Base tickets still total 1,000,000 with 9,700,000
payout tenths and 200,000 positive awards.

### Cash-cap compensation

For each cent stake, calculate the expectation after applying both caps and
rounding the complete award to cents. Any deficit against 97% is restored by
a fixed rational chance to promote a 5× award into the nominal 50× tier.
Small cent-rounding excesses use the reverse shift. Both tiers are positive,
so the 20% hit rate is unchanged. At 2,000 ETB the nominal 50× frequency is
0.205556%; at 4,000 ETB it is 0.55%. At 4,000 ETB that tier pays 100,000 ETB
(25×), because the cash ceiling takes precedence.

Counters use the remaining round allowance, and bonus play ends when the cap
is reached. The saved round ceiling, wallet credit, history, and displayed
counters agree. A retry cannot create another credit or change the award.

## Validation

`TestExactCappedRTPForEveryStake` exhausts all 399,601 cent stakes from 4 through
4,000 ETB with integer identities, proving the capped expectation is exactly
97% and the positive hit probability remains 20%. Fake-wallet tests cover cap
boundaries, both king branches, a partial last award, retries, history, and
preservation of previously accepted v5 awards. PostgreSQL recovery runs in CI
against a disposable database. No real-money wager is used for verification.

One million complete plans per stake (seed 970050), independently checking
both king branches against evaluated lines and bonuses:

| Stake ETB | Observed RTP | Positive hits | 95% RTP interval |
| --- | ---: | ---: | --- |
| 4 | 97.3314% | 20.0843% | 96.5983–98.0646% |
| 1000 | 97.3314% | 20.0843% | 96.5983–98.0646% |
| 2000 | 96.9905% | 19.9834% | 96.3151–97.6659% |
| 4000 | 96.8559% | 19.9720% | 96.2731–97.4386% |

Reports are collected in [math-report-v6.json](math-report-v6.json). The same seed intentionally gives identical
results at 4 and 1,000 ETB because neither stake needs compensation. Simulation
results fluctuate; exact expectation comes from the enumeration proof.
[rules-audit-v6.json](rules-audit-v6.json) separately checks one million nominal plans for symbol
and feature eligibility. The v5 audit and simulation files remain historical.
These are internal engineering checks, not certification of original math.

## Asset provenance

The v7 theme uses carved Ethiopian king portraits, Ge'ez numerals, and imperial
stone lions. The underlying captured game client, unchanged special symbols,
effects, and audio originate from the Spribe/Bambuk demo. The theming does not
claim authorship of those retained assets.

`release.json` records the prepared release's targets and source revision.
