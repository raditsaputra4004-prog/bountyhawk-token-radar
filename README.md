# Token Radar — the best token launch radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/raditsaputra4004-prog/bountyhawk-token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/raditsaputra4004-prog/bountyhawk-token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Entry for make.fun [`token-radar-01`](https://make.fun/bounty/token-radar-01): a program that
ranks pre-token DeFi protocols by the implied value of the token they might one day issue, and
shows a reader how to farm each one. Referral code `2yq7mcmp` — https://make.fun/r/2yq7mcmp

## What it does

`main.ts` (Node 22, standard library only) reads the sealed snapshot of 758 protocols as of
2026-11-14, scores every protocol that records no token, and writes a ranked board of 226 rows:
the 40 protocols on the watchlist (at least $50M of TVL, no token on record) plus all 186
rankable pre-token protocols below that floor. Live-token protocols are set aside with their
market caps shown, never mixed into the ranking. The dashboard (`dashboard/`) renders the board
as a sortable, filterable table with a detail drawer per protocol, the live-token view, the model
in full, and the per-category live comparables the base multiple is calibrated from.

## The model — radar-2

Every row is scored by one function of the snapshot's own figures; no protocol name is
special-cased.

```
implied = round( tvl_usd x 0.20
  x (1 + clamp(change_7d, -60, 120) / 100 x 0.5)
  x (0.6 + min(age_days, 1095) / 1095 x 0.4)
  x (1 + min(chains - 1, 5) x 0.02) )
```

- **Base 0.20** — calibrated from the live market: the median market-cap-to-TVL ratio of 0.19
  across 20 live-token comparables, rounded to a round 0.20.
- **Growth** — the seven-day TVL change, clamped to −60%…+120%, worth half its weight.
- **Maturity** — how long the protocol has run with no token (listing date; 365 days when
  unlisted), from 0.60 for a newborn to 1.00 at three years.
- **Chains** — each chain past the first adds 2%, capped at five: distribution is a real moat.
- **Confidence** — a label for how much the snapshot says about a row's staying power: low
  when the protocol's listing date is unrecorded, so its age is a guess; high when its age is
  known and it runs on more than one chain; medium when its age is known but it runs on a single
  chain.

Top of the board: Steakhouse Financial ($478,399,750), Gauntlet ($284,923,287), Veda
($273,932,857).

## Farm plans

Each row carries six steps with a cost and a time: a fresh wallet, the category's own action
(supply for a lender, liquidity for a DEX, a bridge for a bridge), a weekly qualifying action
varied by what the category actually does, a hold of 30–60 days, claiming only from the
protocol's own published link, and a farm log. The dashboard builds the same six steps from the
same words, so the detail view and `board.json` agree step for step.

## Run it locally

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <kit>/input/snapshot.json
```

That writes `out/board.json`, `out/data.js`, `out/dashboard/`, `out/screenshots/` and
`out/dossier.json`. Open `out/dashboard/index.html` in a browser; the page needs no network.
Screenshots are taken at 1280×900: the board, a protocol's detail view
(`#p=steakhouse-financial`), and a filtered view (`#category=Lending`).

## License

MIT — see `LICENSE`.
