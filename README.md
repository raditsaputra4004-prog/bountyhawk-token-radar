# bountyhawk-token-radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token Radar (model `radar-2`): the pre-token protocols in DefiLlama's snapshot, ranked by implied value, with a farm plan for each one.

The board covers all 40 watchlist protocols plus all 186 rankable pre-token protocols below the $50M floor — 226 rows. Implied value is `TVL x 0.20 x growth x maturity x chains`, where 0.20 is the median market-cap / TVL of the 20 live-token protocols in this snapshot with TVL over $50M, outside CEX and bridges (median 0.19, rounded to 0.20); growth is the seven-day TVL change clamped to [-60, 120]%; maturity rises with the time a protocol has run with no token; chain breadth adds up to 10% for protocols live on more chains. No protocol name is special-cased: one function scores every row. Live-token protocols are set aside with their market caps shown, never mixed into the ranking.

The dashboard is a static page (no network, no framework): a sortable, filterable board, a top-three spotlight, a detail view per protocol with the model's full breakdown, live-peer comparables and risks, and a farm plan with concrete steps, costs and times.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

`main.ts` reads the sealed snapshot, writes `board.json`, `data.js`, copies `dashboard/` and `screenshots/` to the output, and writes `dossier.json`. Node 22.18+ (24 works too), standard library only.

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
