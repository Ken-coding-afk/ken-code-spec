---
name: ken-code-spec
description: Use for Ken-style confirmation-gated coding workflows and cross-tool project setup for Codex, Claude Code, and Cursor. Trigger when initializing or refreshing agent entrypoints, implementing features, fixing bugs, or refactoring. For ordinary changes, explain how the code will change and how it will be verified, then wait for explicit confirmation before editing. For large changes involving cross-module work, architecture, public APIs, databases, auth, deployment, migration, or major refactoring, resolve material uncertainties with the user, create one plan document under docs/plans/, and wait for explicit confirmation before implementation. Ask focused questions with a recommendation whenever requirements or technical choices cannot be determined confidently. Run only tests related to the current change; never run the complete test suite unless the user explicitly requests it.
---

# Ken Code Spec

Use one lightweight, confirmation-gated skill for project setup and code execution. Keep ordinary tasks in chat. For large changes, use one confirmed plan document instead of separate design and implementation documents. Do not introduce subagents, worktrees, TDD, or multi-stage review unless the user explicitly asks.

## Core rules

- Respond in Chinese by default and write project documentation primarily in Chinese.
- Inspect enough context before proposing a change, but do not edit implementation files before the required confirmation.
- Choose the smallest solution that satisfies the current request. Do not add speculative capabilities.
- Exercise independent judgment instead of defaulting to agreement. When the user's proposed approach is materially flawed, risky, internally inconsistent, or misaligned with their stated goal, challenge it respectfully, explain the concern, and recommend a better alternative. Do not object merely because another valid approach exists.
- Change only what is directly relevant. Preserve existing project style and user-owned rules.
- Let explicit user instructions override this workflow.
- Treat the proposal or plan and implementation as separate stages for every code change.
- Treat only an explicit instruction such as “确认”“开始”“执行”“直接改” as authorization to implement the latest proposal or plan. A new requirement, clarification, or answer to a question is not implementation approval.
- Require that authorization after the proposal or plan has been presented. Do not treat the original implementation request as confirmation of a proposal or plan the user has not seen.

Skip the confirmation gate for pure explanations and simple lookups. Read-only inspection is allowed before confirmation. For a large change, creating or updating its plan document is also allowed before implementation confirmation.

## Resolve uncertainty first

Do not guess when an unresolved requirement, solution choice, or implementation choice could materially change behavior, scope, compatibility, risk, or user experience.

1. Inspect enough current context to understand the existing behavior and constraints.
2. Ask only the focused questions needed for the user to decide. Explain the impact of the options and include a recommended choice when possible.
3. Wait for the user's decision before finalizing the ordinary-change proposal or large-change plan.
4. After the user decides, incorporate the decision and still wait for explicit implementation confirmation.
5. If a material uncertainty appears during implementation, pause, ask the user, update the proposal or plan, and obtain confirmation again before continuing.

Do not ask about details that can be established safely from the repository or existing project conventions. Do not present unresolved material choices as assumptions inside a proposal or plan.

## Select a workflow

### Initialize or refresh a project

Run this workflow only when the user asks to initialize, complete, or synchronize the collaboration baseline:

1. Inspect `AGENTS.md`, `CLAUDE.md`, `.cursor/`, `README.md`, and `docs/` at the project root.
2. Copy missing files from `assets/project-template/` without rewriting them. Preserve relative paths.
3. Add missing content only. Never overwrite an existing file without explicit user authorization.
4. Update `docs/README.md` after adding or renaming documentation.
5. Keep placeholders short and never invent project facts.

Do not initialize the whole project merely because the user requested an ordinary code change.

### Implement, fix, or refactor code

Classify the task as an ordinary or large change before proceeding.

## Classify large changes

Treat the task as a large change when any condition applies:

- It spans three or more logical modules or services.
- It changes architecture, core data flow, or module boundaries.
- It changes a public API, database schema, authentication, authorization, build, or deployment behavior.
- It requires migration, compatibility, staged rollout, or rollback planning.
- It is a large refactor that can change existing behavior.
- The user explicitly requests a design document or implementation plan.

Do not classify by file count alone. Keep a clearly bounded local change in the ordinary workflow.

## Ordinary change workflow

1. Read only the context needed to define success criteria and the test scope.
2. Resolve material uncertainties with the user as defined above.
3. Present a concise change proposal in chat covering the intended result, affected files or modules, how the code will change, and how it will be verified.
4. Wait for explicit confirmation. If the user changes the request, revise the proposal and confirm again.
5. After confirmation, finish all code changes for the current task.
6. Add or update unit tests in one batch for every new or changed testable behavior.
7. Run only the test files, test cases, filters, or affected package/workspace tests directly related to the current change, followed by applicable type checks, lint, and build.
8. Report the actual changes, commands, results, and anything not verified.

Do not run tests after every class, function, file, or test. During failure repair, rerun only the related focused tests. Do not run the complete test suite unless the user explicitly requests it.

## Large change workflow

1. Analyze the current state, impact, constraints, and viable options.
2. Resolve all material uncertainties with the user as defined above.
3. Create `docs/plans/YYYY-MM-DD-主题-plan.md` from `assets/project-template/docs/plans/_plan-template.md` or the project's existing equivalent. Keep the initial plan concise, normally 1–3 pages.
4. Include the goal and scope, current state and constraints, chosen solution, impact surface, phased implementation checklist, verification and acceptance, risks and rollback, and an execution record.
5. Present the plan path and a concise summary, then wait for explicit confirmation. If the user changes the request, update the document and confirm again.
6. After confirmation, implement according to the plan. Update it only when a phase completes or material findings require a change.
7. If implementation uncovers a materially different architecture, behavior, risk, or scope, pause, resolve the decision with the user, revise the plan, and obtain confirmation again.
8. Add or update unit tests in one batch after implementation is complete.
9. Run only the tests related to the changed behaviors and affected modules or dependency boundaries, plus applicable checks. Record the selected test scope, real results, and remaining gaps in the same plan document.
10. Report the completed plan items, verification results, deviations, and remaining risks.

Do not create separate design and implementation documents. Create additional ADR, migration, or rollout documents only when the user explicitly requests them or an existing project rule requires them.

## Testing policy

- Require new or updated unit tests for every new or changed testable behavior.
- Use test-last by default: finish the task implementation, then write tests and run them together.
- Do not use TDD unless the user explicitly requests test-first development.
- Select the narrowest test files, test names, or runner filters that cover the current change and its directly affected behavior.
- In a monorepo, run tests only for affected packages or workspaces unless a narrower selection is available.
- Never run the complete project test suite unless the user explicitly requests it.
- If the relevant test scope cannot be determined confidently, ask the user before expanding it.
- Never delete tests, weaken assertions, disable relevant tests, or change a correct expectation merely to obtain a passing result.
- Exempt documentation, comments, formatting, static assets, and configuration changes that do not affect behavior from new unit tests; still run applicable checks.
- Report the exact tests run, why they were selected, their results, and any related behavior not verified.
- When related tests cannot run, explain why, run the closest relevant verification, and state the unverified scope.

## Template layout

Use the project template in `assets/project-template/`:

```text
assets/project-template/
├── AGENTS.md
├── CLAUDE.md
├── README.md
├── .cursor/
│   ├── mcp.json
│   └── rules/
│       ├── codegraph-first.mdc
│       └── karpathy-guidelines.mdc
└── docs/
    ├── README.md
    ├── spec-overview.md
    ├── spec-requirements.md
    ├── spec-design.md
    ├── spec-glossary.md
    ├── plans/
    │   ├── README.md
    │   └── _plan-template.md
    ├── decisions/README.md
    └── changes/README.md
```

## Completion criteria

- Keep changes aligned with the request and avoid unrelated refactors.
- Cover behavioral changes with unit tests or a clear, valid exemption.
- Run only change-related tests in one consolidated phase and report their selection, scope, and traceable results.
- For an ordinary change, obtain confirmation of the chat proposal before editing.
- For a large change, resolve material uncertainties, create one plan document, and obtain confirmation before implementation.
- During setup, add only missing files and preserve all user-owned rules.
