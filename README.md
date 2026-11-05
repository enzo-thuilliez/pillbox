# pillbox

> AI-assisted staff scheduling for pharmacies. A CP-SAT solver builds the schedule; an LLM
> copilot turns "Sam is sick until Thursday" into feasible options you pick with one click.

🚧 **Work in progress.** Rewrite of a production prototype used daily by an independent
pharmacy in France. See [ROADMAP.md](ROADMAP.md).

## Why

A pharmacy can never be left without a pharmacist, never with a single person on site, and
every employee must hit their exact contract hours on a two-week A/B cycle. Re-planning by hand
when someone calls in sick takes an hour. pillbox does it in seconds and never breaks a hard
rule.

## How it works

1. Describe what happened, in plain French.
2. The copilot asks only for what is missing (how long? half day?).
3. The solver produces 2–3 schedules with different trade-offs (fewest changes, balanced hours,
   no overtime).
4. You pick one; it is applied, and can be undone.

The LLM never writes a schedule. It extracts structured events and explains results. Every
schedule comes from the solver ([ADR-003](DECISIONS.md)).

## Stack

| Layer   | Tech                                         | Hosting (free tier)     |
| ------- | -------------------------------------------- | ----------------------- |
| Web     | React, TypeScript, Vite                      | Cloudflare Pages        |
| API     | Cloudflare Worker, D1 (SQLite)               | Cloudflare Workers      |
| LLM     | Workers AI (open-weight models), AI Gateway  | Cloudflare              |
| Solver  | Python, FastAPI, Google OR-Tools CP-SAT      | Hugging Face Space      |
| Auth    | Cloudflare Access                            | Cloudflare Zero Trust   |

## Privacy by design

Real staff data never enters this repository, the LLM or the solver. Names stay in D1 behind
Cloudflare Access; everything else sees pseudonymous IDs. Details in
[DECISIONS.md](DECISIONS.md) (ADR-005 to ADR-008).

## License

[MIT](LICENSE)
