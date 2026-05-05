# AGENTS.md

This file defines how contributors (and AI agents) should write notes in this repository.

## Goal
- Keep notes easy to scan in 1-2 minutes.
- Make templates directly reusable in interviews, contests, and practice.
- Prioritize clarity over clever wording.

## Required note structure
Use this exact high-level order:
1. `# <Topic>`
2. `## One-sentence definition`
3. `## When to use it (use cases)`
4. `## Step-by-step explanation`
5. `## Visual intuition (ASCII)` (recommended when helpful)
6. `## Templates (Python)` (one or more)
7. `## Time & space complexity (Big O)`
8. `## Practice questions (concept check)`
9. `<details><summary>Answers</summary> ... </details>`
10. `## Practice questions (LeetCode/Codeforces)`
11. `## One thing that was confusing to me`
12. `## See also`

## Template writing standard (important)
Every template block should include these lines before code:
- `Use when: ...`
- `Input: ...`
- `Output: ...`

Inside code, use concise CP-style comments only:
- Invariant comments (what stays true)
- Complexity tags (`TC`, `SC`) where valuable
- Edge-case notes
- Avoid noisy line-by-line explanations

## Code quality rules
- Keep function names meaningful and problem-oriented.
- Handle obvious edge cases (`k <= 0`, empty input, invalid index/range).
- Prefer plug-and-play signatures.
- Keep examples language-consistent (Python by default unless topic requires otherwise).

## Practice questions policy
- Keep **concept-check** questions separate from platform problems.
- Include answers only for concept-check questions.
- Do not include full solutions for LeetCode/Codeforces question lists.

## Folder and naming rules
- Use topic folders at repo root (flat structure): `graph/`, `heap/`, `recursion/`, etc.
- Use lowercase and hyphen-separated filenames.
- For split topics, include an overview file plus focused files (example: linked list family).

## Consistency checklist (before PR)
- Section names follow repository style.
- Template blocks have `Use when / Input / Output`.
- Complexity is present and reasonable.
- Concept answers exist directly after concept questions.
- LeetCode/Codeforces list exists and has no answers.
- `README.md` is updated if a new topic folder/path was added.
