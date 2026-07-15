---
name: ken-code-spec
description: Use for Ken-style lightweight coding workflows and cross-tool project setup for Codex, Claude Code, and Cursor. Trigger when initializing or refreshing agent entrypoints, implementing features, fixing bugs, or refactoring. Keep ordinary changes lightweight with a brief plan, test-last unit tests, and one consolidated final verification. For ambiguous or large changes involving cross-module work, architecture, public APIs, databases, auth, deployment, migration, or major refactoring, discuss the proposed solution with the user and obtain explicit confirmation before writing design or implementation documents or starting large-scale coding.
---

# Ken Code Spec

Use one lightweight skill for project setup and code execution. Keep ordinary tasks short. Use formal documents only for large changes. Do not introduce subagents, worktrees, TDD, or multi-stage review unless the user explicitly asks.

## Core rules

- Respond in Chinese by default and write project documentation primarily in Chinese.
- Before taking action, give a brief plan of 3–5 items covering the goal, change scope, and verification.
- Choose the smallest solution that satisfies the current request. Do not add speculative capabilities.
- Change only what is directly relevant. Preserve existing project style and user-owned rules.
- Let explicit user instructions override this workflow.
- Treat solution discussion, document approval, and implementation as separate stages. Do not silently collapse them into one step.

Skip the plan format for pure explanations and simple lookups. After presenting the plan, wait for confirmation before deletion, overwrite, external communication, irreversible action, or a major technical tradeoff.

## Discuss before documenting

For every large change, ambiguous requirement, or consequential technical choice, discuss the solution in chat before creating or updating design and implementation documents.

1. Inspect enough current context to understand the existing behavior and constraints.
2. Present a concise proposal in chat covering:
   - the understood goal and non-goals;
   - the current-state findings that affect the solution;
   - the recommended approach and key tradeoffs;
   - alternatives only when they lead to a materially different result;
   - unresolved questions or assumptions that could change the design.
3. Ask focused questions only for decisions that materially affect the solution. Include a recommendation when possible.
4. Wait for explicit user confirmation of the proposed direction. A new requirement is not itself approval of a solution.
5. If the user changes the direction, revise the conversational proposal and confirm again.

Before confirmation, do not create or update plan documents and do not start large-scale coding. Do not present an internally generated document as a substitute for discussion. Skip this gate only when the user explicitly says to proceed without discussion or confirms that the solution has already been agreed.

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
2. Present a brief plan. Continue without another confirmation when intent is clear and risk is low.
3. If an unresolved choice would materially change behavior, use the discussion gate before coding.
4. Finish all code changes for the current task.
5. Add or update unit tests in one batch for every new or changed testable behavior.
6. Run affected-module tests, then the complete project test suite, followed by applicable type checks, lint, and build.
7. Report the actual changes, commands, results, and anything not verified.

Do not run tests after every class, function, file, or test. During failure repair, run focused tests as needed; after the fix, run the complete suite again.

## Large change workflow

1. Analyze the current state, impact, constraints, and viable options.
2. Complete the conversational solution discussion and obtain explicit confirmation as defined above.
3. Only after confirmation, follow an existing project documentation convention when available. Otherwise use:
   - `assets/project-template/docs/plans/_design-template.md`
   - `assets/project-template/docs/plans/_implementation-template.md`
4. Save the documents as:
   - `docs/plans/YYYY-MM-DD-主题-design.md`
   - `docs/plans/YYYY-MM-DD-主题-implementation.md`
5. Present both documents together for approval. Do not start large-scale coding before document approval.
6. Implement all code according to the implementation document. Update it only at phase boundaries, not after every file.
7. Add or update unit tests in one batch after implementation is complete.
8. Run module tests, the complete suite, and applicable checks together. Record real results in the implementation document.

Use the design document for why, the target design, and risks. Use the implementation document for how, execution order, and verification. Avoid duplicating the same content in both.

## Testing policy

- Require new or updated unit tests for every new or changed testable behavior.
- Use test-last by default: finish the task implementation, then write tests and run them together.
- Do not use TDD unless the user explicitly requests test-first development.
- Never delete tests, weaken assertions, skip tests, or change a correct expectation merely to obtain a passing result.
- Exempt documentation, comments, formatting, static assets, and configuration changes that do not affect behavior from new unit tests; still run applicable checks.
- When the complete suite cannot run or has clearly unreasonable cost, explain why, run the closest available verification, and state the unverified scope.

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
    │   ├── _design-template.md
    │   └── _implementation-template.md
    ├── decisions/README.md
    └── changes/README.md
```

## Completion criteria

- Keep changes aligned with the request and avoid unrelated refactors.
- Cover behavioral changes with unit tests or a clear, valid exemption.
- Run final verification in one consolidated phase and report traceable results.
- For a large change, obtain solution confirmation before writing documents, then obtain document approval before implementation.
- During setup, add only missing files and preserve all user-owned rules.
