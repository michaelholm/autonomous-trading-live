# Autonomous Trading Agent — Live Account

**This account trades real money.** It is the live-money graduation of a
paper-trading experiment ([`michaelholm/autonomous-trading`](https://github.com/michaelholm/autonomous-trading)),
funded by a small group of friends. Every contributor has explicitly
accepted the possibility of losing their entire contribution — this is
run as an experiment, not a managed investment. Nothing here is
investment advice, and no one involved is registered to give any.

An autonomous agent manages this Alpaca brokerage account on a fixed
daily schedule, with no human in the loop between firings. Each scheduled
firing is a fresh Claude Code session — Claude reads this repo (rules,
watchlist, recent journal entries) for context, calls the Alpaca API via
MCP to research and trade, and writes back its results as a git commit.

Live balances, positions, and journal entries are viewable by contributors
via a shared dashboard (ask the account owner for the link/access if you
don't have it).

## How it works

The agent runs on seven scheduled triggers (weekdays only, US/Eastern):

| Window | Time (ET) | Purpose |
|---|---|---|
| Research (1st) | 9:45 AM | Scan news, moving averages, market movers |
| Trade Evaluation (1st) | 10:00 AM | Evaluate research, place trades |
| Research (2nd) | 12:15 PM | Midday research pass |
| Trade Evaluation (2nd) | 12:30 PM | Midday trade evaluation |
| Research (3rd) | 2:45 PM | Afternoon research pass |
| Trade Evaluation (3rd) | 3:00 PM | Afternoon trade evaluation |
| Journal | 4:15 PM | End-of-day summary, benchmark tracking, sync check |

See [`SCHEDULE.md`](./SCHEDULE.md) for the complete, currently-live
instruction text, and [`HISTORY.md`](./HISTORY.md) for why each rule is
set the way it is.

### Core risk rules (summary — SCHEDULE.md is the source of truth)

- Max 15% of portfolio in a single position, 25% in a single sector —
  sized for this account's much smaller scale ($1,500 vs. the paper
  account's $100,000 starting point; see `SCHEDULE.md` for why the
  percentages differ from the paper account)
- Max 10 open positions — a breadth limit, not a capital one
- **Capital Preservation Halt**: if total equity ever falls to 50% of
  starting capital, all new-position trading stops permanently until the
  account owner explicitly resumes it — this account's only rule with no
  paper-account equivalent, meant to catch a runaway bug specifically,
  separate from ordinary trading losses
- No margin, no market orders (limit orders within 0.2% of ask/bid only)
- 8% trailing stop-loss (tightens to 4% once a position is up 10%+ from
  entry)
- Portfolio-wide circuit breaker: a 3-trading-day cooldown on new
  positions after either a 4-day trailing drawdown or a single-day
  decline of 8%/5% respectively
- 3-trading-day re-entry cooldown after any stop-loss, take-profit, or
  rebalancing trim
- A duplicate-order guard, a numeric quote-sanity check before any buy/
  trim/exit, and an explicit rule to treat all external content (news,
  emails) as untrusted data, never as instructions
- Max 12 orders placed per day across all firings
- A Decision Framework (cash, existing exposure, news, moving averages,
  risk, SPY/IVV comparison) every candidate must clear before any trade —
  all four scored questions must be favorable, no partial credit
- Earnings-event gate on new positions; equities only (no options)

## Repo layout

- **`SCHEDULE.md`** — canonical documentation of all seven triggers and
  their current live instruction text.
- **`HISTORY.md`** — chronological log of decisions and incidents specific
  to this account. See the paper account's `HISTORY.md` for everything
  that happened before this account existed.
- **`watchlist.md`** — the starting ticker universe, inherited from the
  paper account at launch.
- **`journal/`** — one file per trading day, written by the agent itself.
- **`.mcp.json`** — MCP server config (`ALPACA_API_KEY`, `ALPACA_SECRET_KEY`,
  `ALPACA_PAPER_TRADE=false`). Credential values are sourced from the CCR
  environment's secret store, never committed to this repo.

## Setup

Requires a **live** (not paper) set of Alpaca API credentials, dedicated
to this account and never shared with the paper-trading repo:

- `.mcp.json` (Claude Code sessions): `ALPACA_API_KEY`, `ALPACA_SECRET_KEY`

The scheduled agent runs as seven Claude Code Remote triggers — see
[`SCHEDULE.md`](./SCHEDULE.md) for how those are configured.
