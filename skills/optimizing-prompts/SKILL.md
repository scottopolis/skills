---
name: optimizing-prompts
description: Improves rough prompts into clear, effective instructions while preserving their intent. Use when asked to optimize, rewrite, refine, or improve a prompt for an AI agent or language model.
license: MIT
---

# Optimizing Prompts

Turn the user's draft into a prompt that is specific, actionable, and easy for an AI agent to follow.

## Process

1. Identify the intended outcome, relevant context, constraints, and desired output.
2. Remove ambiguity, repetition, and unnecessary wording.
3. Make implicit requirements explicit without inventing new requirements.
4. Organize complex instructions into a logical order.
5. Preserve the user's meaning, tone, and non-negotiable constraints.
6. Ask a focused clarifying question only when missing information would materially change the prompt. Otherwise, make the smallest reasonable assumption and state it briefly.

## Output

Return:

1. **Optimized prompt** — ready to copy and use.
2. **What changed** — a short list of meaningful improvements.
3. **Assumptions** — only when assumptions were necessary.

Do not answer or execute the optimized prompt unless the user explicitly asks.
