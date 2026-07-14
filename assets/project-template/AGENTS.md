# AGENTS.md

Use a lightweight, verifiable coding workflow. More specific project rules override this file.

## 1. Language

- Respond in Chinese by default unless the user requests another language.
- Write project documentation primarily in Chinese.
- Keep code identifiers, commands, paths, and established technical terms in their conventional form.

## 2. Think before acting

- Define the goal, constraints, impact, and verification before making changes.
- For actionable work, first present a brief plan of 3–6 items.
- Surface competing interpretations instead of choosing silently. Ask when a missing fact changes the result.
- Wait for confirmation before deletion, overwrite, external communication, irreversible action, or a major tradeoff.
- Skip the plan format for pure explanations and simple lookups.

## 3. Keep changes simple and surgical

- Implement only what the user requested. Do not add speculative features or configurability.
- Avoid abstractions for one-off logic. Prefer fewer lines, fewer dependencies, and a smaller impact surface.
- Modify only files and code directly relevant to the task.
- Do not refactor adjacent modules, normalize unrelated formatting, or delete pre-existing dead code.
- Match existing naming, structure, and style. Remove only unused code introduced by the current change.

Every changed line must trace to the request, a required test, or a direct documentation update.

## 4. Distinguish ordinary and large changes

Treat a task as a large change when any condition applies:

- It spans three or more logical modules or services.
- It changes architecture, core data flow, or module boundaries.
- It changes a public API, database, authentication, authorization, build, or deployment behavior.
- It requires migration, compatibility, staged rollout, or rollback planning.
- It is a large refactor that can change existing behavior.
- The user explicitly requests a design document or implementation plan.

Before coding a large change, create:

- `docs/plans/YYYY-MM-DD-主题-design.md`
- `docs/plans/YYYY-MM-DD-主题-implementation.md`

Use the templates in `docs/plans/`. Present both documents together for approval and do not start large-scale coding before approval. Do not create formal planning documents for ordinary changes.

## 5. Implement first, then test in one batch

- Add or update unit tests for every new or changed testable behavior.
- Finish all code for the current task before writing or updating tests.
- Do not run tests after every class, function, file, or individual test.
- After code and tests are complete, run affected-module tests and then the complete project test suite.
- Finish with applicable type checks, lint, and build.
- During failure repair, run focused tests as needed; after the fix, run the complete suite again.
- Never delete tests, weaken assertions, skip tests, or change a correct expectation merely to make tests pass.
- Do not use TDD unless the user explicitly requests test-first development.

Documentation, comments, formatting, static assets, and behavior-neutral configuration changes do not require new unit tests, but still require applicable checks. When the complete suite cannot run, explain why and state the unverified scope.

## 6. Complete and report

- Compare the result with the brief plan or implementation document.
- Report actual changes, test commands, real results, and remaining risks.
- For large changes, update the implementation document only at phase boundaries, not after each file.
- State anything not verified. Do not claim completion based on assumption.
