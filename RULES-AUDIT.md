# Gates of Abyssinia rules audit — v5

The paytable and ten left-to-right paylines match the captured game, but the
probabilities are custom. The server chooses a complete payout from fixed
tickets and then selects matching grids from a finite catalog. It does not
use the original provider's reel strips, probabilities, or bonus distribution.
Changing weights changes symbol frequencies; it does not authorize paying a
value unsupported by the displayed lines or bonuses.

## What the audit found and fixed

In the v4 baseline of one million rounds, all eight regular symbols appeared
and won. Purple free-spin wilds never appeared: 4,474 free-spin triggers used
red wilds instead. Another 5,701 red-wild respins had no random bonus.

V5 restores purple triggers, including accumulation across a respin. Winning
red wilds always give a bonus, with two or more activating level two. Wild Spot
now transforms every instance of the selected symbol type. Both levels of all
six bonuses are reachable and their payloads reproduce the evaluated grid.
X spots sum their multipliers rather than multiplying each other.

Free spins now consume exactly six or ten normal spins, with respins counted
separately. Menelik's branch applies Random Bonuses; Haile Selassie's does not.
Winning Bless wilds grant respins, the chosen king has the higher multiplier
offer until the ceiling, and payouts/protocol counters use the awarded tier.
Every tier from 2× through 50× was observed. Treasury collection includes every
winning chosen-king cell until the key is earned; a chest award requires that
key and the middle cell of reel three. The entire round remains capped at 200×.

## Symbol coverage

One million paid rounds, seed 97120510, alternating the selected king branch.
Presence counts a symbol once per round; winning cells count distinct cells
per step, including respins and the selected bonus. Frequencies need not be
equal because the symbol payout values and complete-award weights differ.

| Symbol | Rounds containing it | Winning cells |
| --- | ---: | ---: |
| Ge'ez 1 | 868,506 | 173,186 |
| Ge'ez 2 | 868,479 | 172,945 |
| Ge'ez 3 | 786,927 | 74,330 |
| Ge'ez 4 | 786,523 | 74,234 |
| Yohannes IV | 833,713 | 149,744 |
| Tewodros II | 833,564 | 150,258 |
| Haile Selassie | 854,377 | 97,018 |
| Menelik II | 854,706 | 98,161 |
| Empty wild | 762,331 | 146,521 |
| Red respin wild | 32,297 | 48,410 |
| Purple free-spin wild | 24,364 | 49,115 |
| Bless wild | 8,327 | 32,370 |
| Treasury chest | 7 | 0 |

All symbols are reachable. Bless wilds and chests are restricted to free spins.
The chest is a feature trigger, so its zero line-win count is correct.
The checked rule-violation map is empty. See `rules-audit.json`; the earlier
baseline is retained with the backend source for comparison.

## Native browser smoke check

The localhost demo fixture completed the king-selection transition and six-spin
Menelik branch, displayed increasing Bless multipliers, and showed the expected
200 ETB complete award from a 4 ETB bet (50×). This is a deterministic demo
check, not a live wallet transaction or exhaustive browser coverage.

## Limits of conformance

- The finite catalog and conditional feature selection do not reproduce every
  possible original reel layout or its frequency. This is not original RNG parity.
- Both king choices complete the same preselected total. Original higher/lower
  volatility claims were removed from the prepared production rules.
- Generated Bless awards use full groups of three matching energies. The engine
  does not recreate the original distribution of partial and interrupted bars.
- The requested 200× complete-round cap replaces the original larger maximum;
  a treasury award pays only the remaining amount under that cap.
- Browser verification and an authenticated production launch are separate from
  these mathematical checks. Production issuer binding remains unresolved.

## Reproduce in the Go backend

```sh
go test ./internal/games/gatesOfAbyssinia ./internal/spribeslots
go run ./cmd/abyssinia-audit -rounds 1000000 -seed 97120510
go run ./cmd/abyssinia-rtp -spins 10000000 -seed 970025
```

The v5 simulation returned 97.295578% RTP and 19.9983% positive payouts;
exact enumeration gives 97% theoretical RTP and 20% positive payouts.
No weight adjustment was made to force the sample to the expected value.
