# AGENTS.md

Use a lightweight, verifiable coding workflow. More specific project rules override this file.

## 1. Language

- Respond in Chinese by default unless the user requests another language.
- Write project documentation primarily in Chinese.
- Keep code identifiers, commands, paths, and established technical terms in their conventional form.

## 2. Think before acting

- Define the goal, constraints, impact, and verification before making changes.
- Inspect enough context to explain the intended change before editing.
- When a requirement or technical choice cannot be determined confidently and would materially change the result, ask a focused question, include a recommendation, and wait for the user's decision.
- For ordinary changes, explain how the code will change and how it will be verified, then wait for explicit confirmation.
- For large changes, create one plan document as described below, then wait for explicit confirmation.
- Require confirmation after presenting the current proposal or plan. The original implementation request is not confirmation of material the user has not seen.
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

Before coding a large change, create `docs/plans/YYYY-MM-DD-主题-plan.md` using the template in `docs/plans/`. Include the goal and scope, current state and constraints, chosen solution, impact surface, phased steps, verification and acceptance, risks and rollback, and an execution record.

Present the plan for explicit confirmation before implementation. Do not create separate design and implementation documents. If the plan or implementation reveals a material unresolved choice, pause, ask the user, update the plan, and obtain confirmation again.

## 5. Implement first, then test in one batch

- Add or update unit tests for every new or changed testable behavior.
- Finish all code for the current task before writing or updating tests.
- Do not run tests after every class, function, file, or individual test.
- Run only the test files, test cases, or filters directly related to the current change and its affected behavior.
- In a monorepo, test only affected packages or workspaces unless a narrower selection is available.
- Never run the complete project test suite unless the user explicitly requests it.
- If the relevant test scope cannot be determined confidently, ask the user before expanding it.
- Finish with applicable type checks, lint, and build.
- During failure repair, rerun only the related focused tests.
- Never delete tests, weaken assertions, disable relevant tests, or change a correct expectation merely to make tests pass.
- Do not use TDD unless the user explicitly requests test-first development.

Documentation, comments, formatting, static assets, and behavior-neutral configuration changes do not require new unit tests, but still require applicable checks. Report the exact tests run, why they were selected, and any related behavior not verified. When related tests cannot run, explain why and state the unverified scope.

## 6. Complete and report

- Compare the result with the confirmed change proposal or plan document.
- Report actual changes, test commands, real results, and remaining risks.
- For large changes, update the plan only at meaningful phase boundaries or when material findings change it.
- State anything not verified. Do not claim completion based on assumption.
