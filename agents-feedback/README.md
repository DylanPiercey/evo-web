# Agents Feedback

Actionable observations from agent sessions that were **out of scope for the task that surfaced them**. If something is in scope, fix it instead. Do not expand a task's diff to fix issues recorded here.

## When to add an entry

While working on any task, record anything a future contributor should act on:

- a suspected bug or behavior regression you couldn't pursue → `bugs.md`
- duplication, dead code, inconsistency, refactor opportunities → `cleanup.md`
- code, conventions, or docs that were confusing, and what would clarify them → `unclear.md`
- friction in builds, tests, tooling, skills, or repo workflows → `dx.md`

## Rules

1. **Search the category file first.** If an entry already covers it, don't duplicate; append a corroborating sentence only if you have new information.
2. **Be self-contained.** Include enough detail (paths, symbols, reasoning) that someone can act without re-discovering your analysis. Never reference "my earlier analysis" or conversation context.
3. **Append to the end** of the category file.
4. Entries are **removed when resolved** (delete, don't mark done; git history is the archive).
5. Verify claims before recording. A guess is not feedback.

## Entry format

```md
## <one-line imperative summary>

`<primary/file/path.ts:line>` | 2026-07-10 | impact:<low|med|high> | effort:<low|med|high>

<2–6 sentences: the problem, why it matters, and a concrete suggested direction.
Additional file paths inline as needed.>
```
