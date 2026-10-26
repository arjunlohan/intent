# Worked examples

Eleven requests run through the loop in [SKILL.md](SKILL.md). Each shows what the context settles, the brief that results, and the move. Most of them end in zero or one question; that is the point.

## 1. "CI is red on `test_invoice_totals`. Fix it."

- **Context:** the failure log points at a rounding change in `money.py` from yesterday's commit.
- **Brief:** Goal: green CI without changing invoice math anywhere else. Done: the full suite passes. Effort: small. Limits: fix the regression; no refactor.
- **Move:** no questions. A failing check carries its own intent. Read the log, fix the cause, run the suite.
- **Say, after:** "Fixed the rounding in `money.py:42` (half-even rounding came in with `a1b2c3d`). Full suite passes, 212 tests."

## 2. "Add a `--json` flag to the `report` command."

- **Context:** `report` prints a table with five columns; no other command emits JSON yet.
- **Brief:** Goal: machine-readable output. Done: `report --json` emits valid JSON that a test parses. Effort: small. Limits: the table output stays unchanged.
- **Move:** assume and go. The output shape is cheap to change later.
- **Say:** "Assuming `--json` prints a list of objects keyed by the table's five column names. Easy to change if you need another shape."

## 3. "Make the onboarding flow less clunky."

- **Context:** five screens; screens 2 and 4 both ask for the company name; no progress indicator; the Next button sits below the fold on small phones.
- **Brief:** Goal: fewer drop-offs. Done: no field asked twice, progress visible, primary action on screen at 375px wide. Effort: moderate. Limits: no new steps.
- **Move:** fix the concrete problems (cheap, reversible), state the reading, and offer the taste question as options instead of asking it.
- **Say:** "I read 'clunky' as friction, so I removed the duplicate company-name field, added a step indicator, and pinned Next above the fold. If you meant a visual redesign, I'll mock three directions on one page for you to react to."

## 4. "Switch our background jobs from cron to a queue."

- **Context:** 14 cron jobs; two of them (`charge_renewals`, `send_digest`) are not safe to run twice; Redis is already in the stack.
- **Brief:** Goal: retries and visibility for background work. Done: every job runs through the queue, the cron entries are gone, and a dry run shows each job firing once. Effort: large. Limits: billing must not double-charge.
- **Move:** ask. Each gap is costly: the queue choice is an architecture decision, and a mistake on the two unsafe jobs costs money.
- **Ask:**
  1. "Queue: RQ on the Redis we already run, or a managed queue such as SQS? Default: RQ, so there is no new infrastructure."
  2. "`charge_renewals` and `send_digest` aren't safe to run twice, and queues retry. Default: make them idempotent before they move."
  3. "Cutover: run cron and the queue side by side for a week, or switch in one deploy? Default: side by side, with the queue in shadow mode."

## 5. "Announce the outage postmortem to customers."

- **Context:** `incidents/2026-10-14.md` has the timeline and root cause; past announcements went to the status page plus an email to affected accounts.
- **Brief:** Goal: affected customers trust the fix. Done: a draft that a support lead can approve without edits. Effort: moderate. Limits: nothing gets posted or sent without a yes.
- **Move:** draft first (cheap), then ask about the parts that reach people (costly).
- **Ask:** "Draft below. Should it go to the status page and the 312 affected accounts, or to all customers? Default: affected accounts plus the status page, which matches the last two incidents. I'll send nothing until you say go."

## 6. "Add an index on `orders.created_at` to speed up the dashboard."

- **Context:** the query plan already uses an index on `created_at`; 80% of the request time goes to serializing 40,000 rows to JSON.
- **Brief:** the premise ("the query is slow") does not hold, so the goal behind the request (a fast dashboard) takes over.
- **Move:** say so first, recommend the fix for the real cause, and ask before changing approach. The user named a specific solution, and pagination changes what the dashboard's callers receive. Had the real fix been local and invisible to callers (an N+1 query, say), the cost test says make it, report it, and then ask whether they still want the solution they named.
- **Say:** "The index exists and the query takes 30 ms; the other 1.2 s is serializing 40,000 rows. Paginating to 100 rows gets the page under 200 ms. Should I do that instead of the index?"

## 7. "Get the Android build under 6 minutes. Keep going until it's done."

- **Context:** builds take 14 minutes on CI; build caching is off; the runners are the default size.
- **Brief:** Goal, Done, and Effort all arrived in the request; that is what a well-specified prompt looks like. Only one Limit is open.
- **Move:** one question, then a long run. Write the brief to the task file, measure after every change, and suggest a goal loop with the check `p50 build time < 6 min over 5 runs`.
- **Ask:** "May I move to larger CI runners? That costs money. Default: no, code and build config only; I'll ask again if we stall above 6 minutes."

## 8. "Summarize this vendor contract."

- **Context:** the contract names your company as Customer; 40 pages; an auto-renewal clause on page 31.
- **Brief:** Goal: decide whether to sign or renegotiate. Done: one page a busy reader acts on. Effort: moderate. Limits: none.
- **Move:** assume from context. A summary is cheap to redo, and the header settles whose side to read it from.
- **Say:** "Read from your side as Customer: money, dates, renewal, termination, liability, and anything unusual, on one page. The 60-day auto-renewal on page 31 is the item to act on."

## 9. Mid-task: renaming `customer_id` to `account_id`

- **Context, discovered halfway:** the billing team's service queries `customer_id` in 14 raw SQL strings, outside this repo's ownership.
- **Move:** stop and ask. The scope just grew into someone else's system.
- **Say:** "Billing's service reads `customer_id` in 14 raw queries. Options: (a) keep a `customer_id` view so they're unaffected, (b) open a PR against their repo, (c) pause. Default: (a), because it ships today and breaks nobody."

## 10. "We need a logo for the newsletter."

- **Context:** no brand guide; the newsletter covers developer tooling.
- **Move:** taste, so show instead of asking. "What style do you like?" produces vague answers; four concrete options produce a clear one.
- **Say:** "Four directions on one page: a wordmark, a monogram, an abstract mark, and a playful mascot. Pick one, or point at the parts you like, and I'll refine it."

## 11. "Build us a customer portal."

- **Context:** an existing web app with auth; no spec; the request names no features.
- **Brief:** Goal, Done, and Limits are all open, and every one of them is costly to guess.
- **Move:** offer an interview aimed at a written spec, instead of building a guess. Once the user accepts, ask the questions whose answers change the architecture first and keep going until the spec is complete, then write it as a file for the user to correct. Build from the spec, ideally in a fresh session.
- **Ask, first round:**
  1. "Who logs in: your customers' admins only, or every one of their users? Default: admins only, which keeps permissions simple."
  2. "What must they do on day one: view invoices, manage seats, file tickets? Default: invoices and seats."
  3. "Same app and login as today, or a separate subdomain? Default: same app, so there is one auth system."
