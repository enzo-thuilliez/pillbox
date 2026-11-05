# Decisions

Short architecture decision records. Add new entries at the bottom; never rewrite accepted
ones, supersede them instead.

## ADR-001: Monorepo, npm workspaces for TypeScript, uv for Python

- **Status:** accepted (2026-11-05)
- **Context:** Three deployables (web, API, solver) share one domain model and must evolve
  together. Two languages are involved.
- **Decision:** One repo. TypeScript packages under `apps/` and `packages/` as npm workspaces.
  The Python solver under `services/solver/`, managed with `uv`.
- **Consequences:** One PR can change the schema, the API and the solver together. CI runs
  both toolchains.

## ADR-002: The solver stays Python and is a stateless HTTP service

- **Status:** accepted (2026-11-05)
- **Context:** The prototype's CP-SAT model is validated: with no change in input, it
  reproduces the pharmacy's real schedule exactly. OR-Tools has no first-class JS binding that
  runs in a Worker.
- **Decision:** Port the model as is to a FastAPI service on a free Hugging Face Space. It
  receives a full problem (pseudonymous employees, rules, events) and returns schedules. It
  stores nothing.
- **Consequences:** Cold starts on the free tier; the UI must show progress. The API owns all
  state. A golden test guards the "same input, same schedule" property.

## ADR-003: The LLM is an interface, never the optimizer

- **Status:** accepted (2026-11-05)
- **Context:** LLMs produce plausible but infeasible schedules and cannot guarantee hard
  constraints (pharmacist always present, contract hours).
- **Decision:** The LLM only (1) classifies a request into a typed event and asks for missing
  fields, and (2) summarizes solver results in plain French. Every schedule comes from the
  solver, run once per objective to produce 2–3 options.
- **Consequences:** Hard constraints cannot be lifted from the chat. LLM quality is measured on
  extraction (evals), not on schedule quality.

## ADR-004: One schema package is the source of truth

- **Status:** accepted (2026-11-05)
- **Context:** Event types (absence, peak, swap…) and their required fields drive the copilot's
  questions, the API validation and the solver input.
- **Decision:** Define them with Zod in `packages/schema`. Export JSON Schema for the LLM tool
  definitions and for the solver's Pydantic models (checked in CI for drift).
- **Consequences:** Adding an event type is one schema change plus a solver handler.

## ADR-005: Cloudflare D1 for persistence, EU jurisdiction

- **Status:** accepted (2026-11-05)
- **Context:** The prototype wrote JSON files to disk. Pages and HF Space have no persistent
  disk, and the pharmacist must find her data from any device.
- **Decision:** D1, created with EU jurisdiction. The Worker is the only reader and writer.
- **Consequences:** Migrations live in `apps/api/migrations`. A JSON export/backup is offered
  in the UI.

## ADR-006: Cloudflare Access for authentication

- **Status:** accepted (2026-11-05)
- **Context:** A handful of known users, staff data, no budget, no wish to maintain auth code.
- **Decision:** Put Cloudflare Access (email one-time code) in front of the web app and API.
  The API also verifies the Access JWT.
- **Consequences:** No login code in the app. The public demo is a separate deployment with
  fictitious data and no Access.

## ADR-007: Pseudonymize before any call that leaves the API

- **Status:** accepted (2026-11-05)
- **Context:** Free LLM tiers and the public HF Space give no confidentiality guarantee.
- **Decision:** The API replaces employee names with stable IDs (`E1`, `E2`…) before calling the
  LLM or the solver, and maps them back on return. Free-text notes are never sent.
- **Consequences:** The LLM resolves names through a tool that maps "Sam" to `E3` inside the
  API; it never sees the roster.

## ADR-008: No real data in the repository

- **Status:** accepted (2026-11-05)
- **Context:** The repo is public. The prototype's history contains real names and the
  pharmacy's identity, so it is not reused.
- **Decision:** Fresh history. Fixtures and the golden test use fictitious names. CI runs
  gitleaks and a denylist check (real names, pharmacy name) whose list is stored as a GitHub
  secret, not in the repo.
- **Consequences:** The denylist must be kept up to date when staff changes.

## ADR-009: Free tiers only

- **Status:** accepted (2026-11-05)
- **Context:** Built for a small independent pharmacy; running cost must be zero.
- **Decision:** Cloudflare Pages, Workers, D1, Workers AI, AI Gateway, Access; Hugging Face
  CPU Space. No dependency that requires a paid plan.
- **Consequences:** Model choice is limited to Workers AI's catalog and its daily free
  allocation. Usage is low (one user, a few requests a day).

## ADR-010: English codebase, French product

- **Status:** accepted (2026-11-05)
- **Decision:** Code, docs, issues and commits in English. UI, prompts and LLM replies in
  French. Domain terms keep their French name where they have no exact equivalent (documented
  in the glossary in AGENTS.md).
