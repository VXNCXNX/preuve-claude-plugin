---
name: validate-idea
description: Validate a startup idea with Preuve's live analysis and report the scored verdict, competitors and risks. Use when the user asks whether an idea is worth building, whether a market exists, or who already does it.
argument-hint: '[idea: what it does and who it is for]'
---

# Validate an idea with Preuve

The idea to validate:

$ARGUMENTS

Every verdict, score, competitor, risk and number comes from the Preuve MCP tools. Leave `workspace` unset on every call, so the scan stays in the personal workspace. Client work and `get_agency` belong elsewhere.

1. **Check the idea.** It needs at least 40 characters, one or two sentences saying what it does and who it is for. When it is shorter or empty, ask the user for more and wait for the answer.
2. **Start the scan.** Call `start_analysis` with the idea, `scanType: "starter"`, and a fresh unique `clientRunId`, such as `validate-<timestamp>-<random>`.
   - Run a deep scan only when the user explicitly asks for one. Ask first for their stage and budget, then pass the values they chose. Stage is one of `idea`, `validation`, `mvp`, `launched`, `growth`, `scaling`. Budget is one of `bootstrap`, `10k`, `50k`, `100k`, `100kPlus`, `1mPlus`. Never guess either. A deep scan takes at least 10 minutes.
   - After a timeout or a lost reply, call again with the same `clientRunId`. That replays the original run instead of starting a second one.
   - `429 CONCURRENT_LIMIT_REACHED` is pacing. Wait, then retry with the same `clientRunId`.
3. **Poll.** Call `get_analysis` with the returned `id`. Wait at least `pollAfterSeconds` between calls. A starter scan takes about a minute. The step is done when `status` is `COMPLETED` and `readyForExport` is true.
   - When `status` is `COMPLETED` and `readyForExport` is false, call `enrich_analysis` with the id and no `modules`, then poll again.
   - When `status` is `FAILED`, tell the user and ask before starting a new run.
4. **Export.** Call `export_analysis` with the id and `verbosity: "summary"`.
5. **Report** from the export, in this order.
   - The verdict and score, and what drives them.
   - The closest competitors.
   - The biggest risk.
   - The report URL from `get_analysis`.

## Reading the export

- A starter scan returns some fields as null, with a top-level `locked` object and a note. Relay that note as written. Market size stays as the export left it, with no figures of your own.
- A deep report names the market its figures cover in `idea.details.market.sizedFor`. Quote it with any TAM, SAM or SOM.
- When `idea.details.idea.inferredContext` is present, list those assumptions Preuve made about the idea. The user can fold corrections into the idea text and run a new scan.

## Refusals

When a tool refuses, relay its text exactly as returned. It names the way forward for that report.
