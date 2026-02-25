---
name: baekjoon-solver-helper
description: "Handle Baekjoon problem links by deciding mode from user intent. Use when the user shares a Baekjoon URL and wants either a full algorithm solution or a failing test case (counterexample). If intent is explicit, execute directly. Ask only when intent is ambiguous. In solution mode, provide a rigorous structured explanation and implement Kotlin code in C:/Users/sangb/intellij/Algorithm/src/Answer.kt."
---

# Baekjoon Solver Helper

## Workflow

1. Receive a Baekjoon problem URL from the user.
2. Determine mode from user intent:
   - If user intent is explicit (`solution`/`solve` or `counterexample`), do not ask again.
   - Ask one clarification question only when the request is ambiguous.
3. If mode is `counterexample`:
   - Analyze common wrong approaches for that problem.
   - Provide one strong counterexample input.
   - Include why it breaks incorrect logic.
4. If mode is `solution`:
   - Explain the solution with the required structure below.
   - Then write the final Kotlin solution to `C:/Users/sangb/intellij/Algorithm/src/Answer.kt`.
   - Ensure the file is complete and directly runnable in Baekjoon.

## Required Output For `solution`

Always present the explanation before code, in this order:

1. Problem understanding: summarize objective in 1-2 lines.
2. Key observations: state the critical properties that make the approach work.
3. Optimization formulation: define what is minimized/maximized and how the objective changes.
4. Algorithm choice and steps: explain the selected method as concrete steps.
5. Correctness reasoning: short but explicit proof-style reasoning.
6. Complexity: time and space complexity.
7. Algorithms used: list the paradigms/data structures used in this problem (for example: Greedy, Sorting, Prefix Sum, Binary Search, Graph Traversal, DP).
8. Final Kotlin code: write/update `C:/Users/sangb/intellij/Algorithm/src/Answer.kt`.

## Required `Answer.kt` Comment

- In `solution` mode, copy the same explanation content into a Kotlin block comment in `src/Answer.kt`.
- Use `/** ... */` style and keep the same section order as the chat explanation.
- Place this `/** ... */` comment immediately above the `main` function.
- Write the `/** ... */` explanation comment in Korean.

## Kotlin Code Layout Rules

- `fun main()` must be the first declaration in the file.
- Do not declare classes, objects, or helper functions before `main`.
- Put helper classes/functions (for example `FastScanner`) below `main`.

## Guardrails

- If user intent is explicit, do not ask mode again.
- Ask mode only when the request is ambiguous.
- Use only one mode per request unless the user asks for both.
- For `counterexample`, do not provide vague hints only; provide concrete input.
- For `solution`, do not output code only; include the full structured explanation.
- For `solution`, include `Algorithms used` explicitly.
- For `solution`, update `src/Answer.kt` instead of only pasting code in chat.
- For `solution`, include the same explanation in a `/** ... */` comment immediately above `main`.
- For `solution`, `/** ... */` explanation comments must be written in Korean.
- In Kotlin output, `main` must appear first in the file; no class/function declarations are allowed above it.
- Prefer a reasoning-strong model when runtime model routing supports per-skill model selection.