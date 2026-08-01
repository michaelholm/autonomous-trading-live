# Change & Incident History

This is the live-money graduation of the paper-trading experiment at
`michaelholm/autonomous-trading`. That repo's `HISTORY.md` has the full
record of everything learned before this account existed — three-plus
weeks of rule iteration, several distinct platform bugs, and one honest
performance review that led directly to the design of this account. This
file starts fresh from launch and only covers what happens here.

## Launch

- 2026-08-01: Account owner decided to graduate from the paper account to
  a small live-money account funded by a group of friends, each
  contributing $200-300 and explicitly accepting the possibility of total
  loss. Discussed and flagged before building anything: the two-week
  paper-trading window originally proposed doesn't provide statistically
  meaningful evidence either way on whether the strategy has real edge
  (the paper account's own 7-day track record was already -2.75% vs. a
  flat SPY, and a backtest of the mechanical rules alone underperformed
  SPY by ~5 points over a full year) — the account owner and friends
  chose to proceed anyway, treating the live account explicitly as an
  experiment rather than an investment expected to profit, sized so that
  a total loss is fully acceptable to everyone involved.
- Deliberately built as a **separate repo, separate GitHub credentials,
  separate Alpaca account and API keys, and (pending) a separate CCR
  environment** from the paper account — full isolation, motivated
  directly by the paper account's own leaked-API-key incident
  (`michaelholm/autonomous-trading`, commit `224cbc8`). A compromise of
  the paper account's secrets should never be able to touch this one, or
  vice versa.
- Re-derived several rules for the ~67x smaller account size ($1,500 vs.
  $100,000) rather than copying the paper account's numbers verbatim —
  see the "What's different from the paper account, and why" section in
  `SCHEDULE.md` for the specifics (position/sector cap, position count
  cap, dust floor) and the reasoning. One correction made during this
  process worth recording: an initial proposal to lower the Position
  Count Cap to ~8 (paired with raising the single-position cap to 15%)
  was checked against the Cash Utilization 10%-cash target and found
  arithmetically inconsistent — 8 positions at a 15% cap can't reach 90%
  invested. Recalculated and settled on a count cap of 10, the minimum
  consistent with the other two numbers plus headroom.
- Also corrected a claim made earlier in the same conversation: the
  paper account's Cash Utilization section was already fractional-sizing
  new-position buys before this rewrite (the "not new discretionary
  buys" scoping in that account's own history refers to *which*
  candidate to buy, a judgment call fractional sizing doesn't touch —
  not *whether* a qualifying buy gets sized fractionally, which it
  already does). No fix was needed there; only the dust-floor dollar
  amount needed rescaling for this account's size.
- Added a **Capital Preservation Halt** with no paper-account
  equivalent: a hard, permanent trading halt if equity falls to 50% of
  starting capital ($750 of $1,500), requiring an explicit account-owner
  note in the journal to resume. Purpose: distinguish "wiped out by a
  bug" (unacceptable, worth a hard stop) from "lost money through
  legitimate trading" (accepted risk, already priced in by everyone
  contributing) — the existing Portfolio Drawdown Circuit Breaker
  auto-resumes after a cooldown and wasn't designed to catch a runaway
  failure mode specifically.
- **Pending before triggers can be created**: a dedicated CCR
  environment with this account's own `ALPACA_API_KEY`/
  `ALPACA_SECRET_KEY` (live, not paper) needs to exist. Once it does,
  all seven triggers should be created via `create_trigger` from a
  directly-driven session (not the Routines UI) so they're agent-owned
  from the start — the paper account's `HISTORY.md` documents at length
  why UI-created triggers on that account became a recurring source of
  `"http_api"`-lockout and content-fidelity incidents; starting
  agent-owned avoids that failure class entirely, assuming
  `create_trigger`'s repo-binding actually attaches correctly (verify
  `session_context.sources` is non-null in a fresh `list_triggers` pull
  before enabling any of them — this has failed before on the paper
  account, for reasons never fully diagnosed).
