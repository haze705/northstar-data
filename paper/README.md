# NorthStar Paper Desk — ledger (v1, PAPER ONLY)
No broker orders are ever placed. Capital: swing 25,000 CAD · scalp 10,000 CAD · crypto 5,000 USDC.

Files
- `ledger.json` — per book: capital, cash, equity, realized, unrealized, status, killSwitch, windowR, rLog, source(asOf/gate).
- `positions.json` — `{positions:[{id,book,symbol,venue,fx,entry,qty,qty0,stop,initStop,mark,rNow,halfTaken,score,why,openedAt}]}`
- `fills.jsonl` — append-only `{ts,book,symbol,side,qty,px,reason,r,id}`
- `daily.jsonl` — EOD snapshot per book (`./paper_run.sh --eod`)

Rules (equities: swing=screen-data.js, scalp=scalp-data.js)
- Entry: bucket `action` or state PRIME (or inventory with state BREAKOUT and all checks pass); gate must be OPEN; max 2 names/book; risk 1% book equity; qty = risk / (last−stop); fill at `last`.
- Exit: last ≤ stop → out at stop. +1R → stop to breakeven (unless rotating). +2R → sell half, trail rest 1R below last. ≥1R position may be rotated out only when a higher-score PRIME needs the slot.
- Kill-switch −3R: equities per Whitehorse session; crypto rolling 24h.
- USD names in CAD books use fixed USDCAD (env `NS_USDCAD`, default 1.37).
- Crypto: skipped while crypto-data.js is the stub (`status: waiting_on_generator`).
