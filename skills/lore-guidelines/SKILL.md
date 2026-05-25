---
name: lore-guidelines
description: Behavioral guidelines to reduce common LLM coding mistakes. Apply when writing, reviewing, or refactoring code. Covers thinking before coding, simplicity, surgical changes, and goal-driven execution.
---

# Lore Guidelines

Coding discipline to reduce common LLM mistakes, combining Karpathy's observations with proven engineering principles.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks (typo fixes, obvious one-liners), use judgment.

---

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State assumptions explicitly. If uncertain, ask — don't pick an interpretation silently and run with it.
- If multiple valid interpretations exist, present them. Don't choose silently.
- If a simpler approach exists, say so and push back.
- If something is genuinely unclear, stop. Name what's confusing. Ask.

---

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- Three similar lines is better than a premature abstraction.
- If you wrote 200 lines and 50 would do, rewrite it.

Ask: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

---

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.

When your changes create orphans:
- Remove imports, variables, and functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

Test: every changed line should trace directly to the user's request.

---

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform imperative tasks into verifiable goals:

| Instead of... | Transform to... |
|---|---|
| "Fix the bug" | "Write a test that reproduces it, then make it pass" |
| "Add validation" | "Write tests for invalid inputs, then make them pass" |
| "Refactor X" | "Ensure tests pass before and after" |

For multi-step tasks, state a brief plan with explicit verification:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria allow independent looping to completion. Weak criteria ("make it work") require constant back-and-forth.
