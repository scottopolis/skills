---
name: optimizing-prompts
description: Implements and reviews system prompts for customer-facing agents and other LLM workflows. Use when writing, optimizing, auditing, or revising a prompt, including its role, behavior, workflow, tool rules, failure paths, examples, and eval plan.
license: MIT
---

# Optimizing Prompts

Implement the smallest prompt that gives the agent a clear job, trusted facts, safe actions, and a
plan for getting unstuck. Review existing prompts against the same standard.

Do not make a prompt longer merely to make it look complete. Add detail only when it defines a real
product requirement, resolves an important ambiguity, or fixes a measured failure.

## Start

1. Determine whether the user wants a new prompt, a revision, or a review.
2. Identify the exact model, agent harness, available tools, trusted data sources, and existing evals
   when they are available. Never assume one model's best prompt is best for another.
3. Identify the customer outcome, business boundaries, approvals, handoffs, and actions the agent
   must never perform on its own.
4. Ask one focused question only when missing information would materially change the result.
   Otherwise make the smallest reasonable assumption and label it.

## Prompt contract

Include only the parts the workflow needs:

1. **Job and scope** — Name the coherent outcome the agent owns and what falls outside it.
   - Good: `You help customers check order status and shipping.`
   - Avoid persona costumes such as `You are a world-famous support expert.` A role sets scope; it
     does not make the model smarter.
2. **Trusted sources** — State where current facts come from and forbid unsupported guesses.
   - `Use order_status for current order facts. Do not guess dates or account details.`
3. **Observable behavior** — Describe actions instead of broad adjectives.
   - Prefer `Answer directly. Ask one question at a time.` over `Be friendly and empathetic.`
   - State what brevity must preserve: the answer, an important warning, and the next step.
4. **Business workflow** — Specify required branches, prerequisites, approvals, stop rules, and
   handoffs. Do not prescribe obvious reasoning steps the model can infer.
5. **Tool policy** — State when to use each tool, required information, source-of-truth rules,
   retries, and failure behavior. Keep input/output/schema details in the tool description. State a
   rule once rather than repeating it in the prompt and tool description.
6. **Failure path** — Tell the agent what to say or do when information is missing, tools fail, or
   the request is out of scope.
7. **Real boundaries** — Pair an important prohibition with the desired alternative.
   - `Do not guess an arrival date. Use order_status. If it returns no date, say a current date is
     unavailable and offer shipping support.`
   - Treat the prompt as guidance, not enforcement. Authentication, authorization, privacy,
     payments, refunds, irreversible actions, and other hard controls belong in application code.
8. **Durable shared facts** — Keep only facts needed in most conversations. Retrieve changing,
   private, account-specific, location-specific, or detailed information just in time.
9. **Examples only when earned** — Add a small example for a product requirement or recurring,
   measured error that is hard to state clearly. Do not add examples by default. Warn the agent not
   to copy wording exactly when repetition would harm the customer experience.

## Keep it lean

- Write each instruction once.
- Remove repeated, obsolete, obvious, unrelated, stale, or conflicting text.
- Prefer a short principle at the right level over many brittle micro-rules.
- Do not confuse `short` with `underspecified`. A prompt is complete when the decisions that matter
  are clear.
- Use progressive or tool-based retrieval instead of loading every possible policy or document.
- Preserve the user's meaning and non-negotiable business constraints while editing.

## Review an existing prompt

Report concrete findings, highest impact first. For each finding:

1. quote the smallest relevant passage;
2. explain the likely behavior or risk;
3. provide exact replacement text; and
4. say how to test whether the change helped.

Check for:

- unclear or mixed job ownership;
- vague tone labels without observable behavior;
- missing trusted sources, tool rules, failure paths, approvals, or handoffs;
- negative-only instructions with no safe alternative;
- duplicated, conflicting, stale, or irrelevant context;
- examples that anchor behavior without fixing a known problem;
- changing facts that should come from a tool;
- security or permissions incorrectly entrusted to prompt text; and
- assumptions copied from a different model without evaluation.

Do not invent problems to fill a review. Say when a section is already clear and useful.

## Verify on the production model

Prompt quality is model- and task-dependent. Test the exact model snapshot and agent harness before
and after a change. Start with a lean prompt, change one rule or one example group at a time, and
keep the change only when held-out cases improve without unacceptable regressions.

Use representative cases for normal requests, missing or conflicting data, tool failure, long
conversations, angry customers, out-of-scope requests, prompt injection, sensitive actions, and
human handoff. Check the final answer and tool trace. Track task success, grounding, tool choice and
arguments, unauthorized actions, escalation, observable tone, repeated example wording, latency,
tokens, and cost.

## Output

For implementation or revision, return:

1. **Prompt** — ready to copy and use.
2. **Key decisions** — only choices that affect behavior.
3. **Test cases** — a short, targeted set for the changed behavior.
4. **Assumptions** — only when necessary.

For review, return:

1. **Findings** — ordered by impact, each with exact replacement text.
2. **Revised prompt** — when requested or when a full rewrite is the clearest fix.
3. **Test cases** — focused on the findings.

Keep explanations practical and concise. Do not answer or execute the prompt unless the user asks.
