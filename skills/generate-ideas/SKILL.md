---
name: generate-ideas
description: Generate startup ideas from the user's interests or skills with Preuve's live-search idea generator. Use when the user asks for startup ideas, what to build, or ideas in a domain they know.
argument-hint: '[interests or skills]'
---

# Generate startup ideas with Preuve

The interests or skills to generate from:

$ARGUMENTS

Every idea comes from the Preuve `generate_ideas` tool, grounded in live search.

1. **Check the input.** It needs 3 to 500 characters. When it is empty, shorter or longer, ask the user to restate it and wait for the answer.
2. **Generate once.** Call `generate_ideas` with `interests`. Pass `budget`, `audience` or `industry` only when the user stated them. Leave `fit` out. Set `pro: true` only when the user asked for the Pro model.
   Work with this one response. Some accounts have a cap of 5 generations per rolling 24 hours, so a second call waits for the user to ask.
3. **Present every idea** with its name, pitch, why now, market saturation and audience.
   - When a pain point is locked, say pain points are unavailable on this account. The pain point text comes only from the tool.
   - When the response carries `fitHint`, relay it as the reason tailoring did not apply.
   - When `fellBack` is true, say a faster fallback model answered in place of the Pro model.
4. **Close** by naming the most promising idea and suggesting `/preuve:validate-idea` on it, which runs a starter scan.

## Refusals

A refusal or a `429 DAILY_LIMIT_REACHED` answer is relayed exactly as the tool returned it.
