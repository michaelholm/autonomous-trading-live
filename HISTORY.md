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
- **Environment created**: account owner created the `auto-trading-live`
  CCR environment. Real Alpaca live credentials don't exist yet (the live
  brokerage account itself hasn't been opened), so `ALPACA_API_KEY`/
  `ALPACA_SECRET_KEY` were set to placeholder values for the account owner
  to fill in once the account is funded.
- **Trigger creation**: attempted `create_trigger` first, agent-owned as
  planned below. It reproduced the paper account's known repo-binding bug
  exactly — the resulting trigger had no `sources` field at all — on the
  very first attempt, confirming this is a platform-level issue and not
  something specific to the paper account's history. The broken test
  trigger was deleted via `delete_trigger` before it could ever fire.
  Fell back to the same path the paper account ultimately relies on: all
  seven triggers were created by the account owner directly through the
  claude.ai Routines UI, using seven prepared prompt files sent via
  `SendUserFile`. This makes all seven `"http_api"`-owned rather than
  agent-owned, meaning future edits (content or schedule) require the UI
  again, not `update_trigger` — confirmed by testing `update_trigger`
  against one of them, which failed with the same ownership error the
  paper account's `HISTORY.md` documents for content edits, showing the
  restriction also covers cron-only edits.
- **Cron bug found and fixed**: all seven UI-created triggers initially had
  the wrong schedule — each fired exactly one hour later than intended
  (e.g. `45 14 * * *` instead of `45 13 * * 1-5`) and with no weekday
  restriction (fired every day, not just Mon-Fri). Presented the account
  owner a before/after cron table for all seven; they corrected each one
  manually in the Routines UI. Re-verified afterward: all seven now have
  the correct cron (weekdays only, correct ET time), byte-for-byte correct
  content, correct repo binding, and were enabled at that point.
- **Paused pending funding**: with real Alpaca credentials still not in
  place (the `auto-trading-live` CCR environment holds placeholder
  `ALPACA_API_KEY`/`ALPACA_SECRET_KEY` values), the account owner paused
  all seven triggers via the Routines UI rather than let them fire and
  error out on every window. Confirmed paused directly by the account
  owner — this state isn't visible to a directly-driven session via
  `list_triggers`, which doesn't expose an enabled/paused field for
  `http_api`-owned triggers, so this has to be taken on the account
  owner's word rather than independently re-verified. All seven will need
  to be manually re-enabled via the Routines UI once real credentials and
  capital are in place — this is not something a session can flip back on
  from here either.
- **Open item**: the Capital Preservation Halt section still contains the
  literal placeholder `[DATE — fill in once the account is funded]` in all
  seven live trigger prompts. Once real capital is deposited, this needs a
  real date — another round of Routines UI edits, verified byte-for-byte
  as always. The dashboard password the account owner shared directly in
  chat (for the contributor-facing balance/journal viewer) was flagged for
  rotation since chat transcripts persist; not yet confirmed rotated.

## SEC Filing Check

- 2026-08-01: ported the paper account's newly-added SEC Filing Check
  section (SEC EDGAR full-text filings — 8-K material events, Form 3/4/5
  insider transactions — as a free, no-API-key, primary-source complement
  to the existing news pull) to this account, adapted only by trimming the
  paper account's account-specific cross-references (its own recurring
  quote-issue tickers, its 10 req/sec framing) to match this file's more
  condensed style. Added the same three edits: the new section itself
  (Research Scope, before Watchlist Policy), a cross-reference from
  Decision Framework question 3, and an entry in the shared-sections list.
  Since all seven live triggers are `"http_api"`-owned (no `meta_mcp`
  triggers exist on this account — every one was created through the
  Routines UI), all seven needed a manual paste rather than the mixed
  `update_trigger`/manual-paste split used on the paper account.
  **Deployed same day**: sent all 7 corrected prompts via `SendUserFile`;
  account owner pasted each into the Routines UI. Re-pulled `list_triggers`
  and diffed all 7 contents byte-for-byte against the intended text — all
  7 matched exactly, each with a fresh `updated_at` timestamp confirming
  the paste took, and cron unchanged on all 7.
- 2026-08-02: account owner asked to build a test for the news APIs.
  Testing (against the paper account's development session) surfaced a
  real gap: `sec.gov`/`data.sec.gov` are blocked outright by that
  session's network egress policy (a 403 at the proxy level, confirmed
  three ways against a working control request). Unknown whether the
  actual scheduled trigger execution environment shares that restriction.
  Hardened the SEC Filing Check section on both accounts rather than
  wait and risk a silent failure: added a required distinct journal note,
  "SEC EDGAR unreachable this firing," for any outright request failure,
  so a blocked or failed call can never be silently misread as "no new
  filings" — the two are now required to be logged differently. Applied
  the same fix to this account's SCHEDULE.md and all 7 triggers. Since
  all seven are `"http_api"`-owned, all seven needed a manual paste via
  `SendUserFile`; re-pulled `list_triggers` afterward and diffed all 7
  contents byte-for-byte against the intended text — all 7 matched, with
  fresh `updated_at` timestamps confirming each paste took. See the paper
  account's `HISTORY.md` for the full reachability-testing detail.
- 2026-08-02 (later): account owner allowlisted `*.sec.gov` on the
  network egress policy. Reachability re-tested and confirmed end-to-end
  (not just host-level — the full ticker→CIK→filings flow, run against
  real watchlist/portfolio tickers with real SEC data returned). The SEC
  Filing Check should now work as designed on future firings, once this
  account's triggers are live and funded. The point-5 unreachable-request
  hardening stays in place regardless as a permanent safety net. Full
  detail in the paper account's `HISTORY.md`.
- 2026-08-02 (later still): a root-cause analysis on the paper account
  (its journal history, not this account's — this account hasn't traded
  yet) found that its recurring wide/stale-quote data issue was not the
  cause of its stop-loss losses, but did surface a real gap: bar-derived
  peak/trough values (used for "highest price reached since entry"
  tracking) had no sanity check, only live bid/ask quotes did. Ported
  the fix here even though this account isn't trading yet, since the
  same gap exists in this account's identical rule text and will apply
  the moment it starts: added a third point to Stop-Loss Confirmation
  cross-checking any new post-entry high/low against that day's
  volume-weighted average price (`vw`, already returned by
  get_stock_bars) using the same 2% threshold as the existing bid/ask
  check, using the day's close instead of the raw extreme when they
  diverge. Scoped to only affect newly-set peaks/troughs, never to
  retroactively loosen an already-active trailing stop. Full root-cause
  detail in the paper account's `HISTORY.md`.
