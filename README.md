# Safe Refactor Engine

Language-agnostic refactoring skill for agentic workflows.

This repository packages a reusable skill that helps an agent plan and execute refactors without guessing user intent, without overfitting to a single language, and without breaking behavior casually.

## What It Does

`safe-refactor-engine` is designed for refactors such as:

- splitting large units
- extracting modules or helpers
- isolating side effects
- reducing coupling
- cleaning up structure without changing intended behavior
- planning larger architectural refactors in verifiable slices

The skill is opinionated about process:

- clarify user intent before acting when ambiguity is real
- do not ask the user technical questions that the agent can resolve alone
- preserve behavior unless the user explicitly asks for behavior changes
- use `KISS`, `DRY`, `YAGNI`, `SRP`, and separation of concerns as active refactor constraints

## Validation Roles

The skill centers on one principal workflow and a small set of validation roles:

- architecture
- contracts
- execution
- maintainability
- governance

These roles are usually applied as a checklist by one agent. Extra delegation is optional, not the default path.

## Repository Layout

This repo follows the `skills.sh`-style pattern where the installable skill lives under `skills/<skill-name>/`.

```text
safe-refactor-engine/
├── README.md
├── references/
│   ├── agents/
│   ├── patterns.md
│   ├── slicing.md
│   ├── verification.md
│   └── workflow.md
└── skills/
    └── safe-refactor-engine/
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```

## Key Behaviors

- The principal agent must clarify intent only when the request is materially ambiguous.
- Technical uncertainty about libraries, APIs, commands, or framework behavior should be resolved with local tooling, official docs, or MCPs before asking the user.
- Quick and Standard mode should work from the main `SKILL.md` alone; references are support material, not required startup context.
- The agent should estimate blast radius before editing. If the likely impact exceeds 5 files, it should escalate to a deeper workflow.
- Each slice should include a rollback point and immediate verification.
- In typed or compiled stacks, the agent should run the narrowest available lint, build, or type-check after each slice.
- Refactors should be executed in small slices with verification after each step.
- Reports should target terminal or chat, not Slack or dashboards.

## Main Skill File

The installable skill lives here:

- [`skills/safe-refactor-engine/SKILL.md`](./skills/safe-refactor-engine/SKILL.md)

The UI metadata lives here:

- [`skills/safe-refactor-engine/agents/openai.yaml`](./skills/safe-refactor-engine/agents/openai.yaml)

Optional deep references live here:

- [`references/workflow.md`](./references/workflow.md)
- [`references/slicing.md`](./references/slicing.md)
- [`references/verification.md`](./references/verification.md)
- [`references/patterns.md`](./references/patterns.md)

## Example Invocation

```text
Use $safe-refactor-engine to plan and apply a safe refactor for the code in scope.
```

Example requests:

- `Use $safe-refactor-engine to split this service into smaller modules without changing behavior.`
- `Use $safe-refactor-engine to reduce coupling in this subsystem and preserve public contracts.`
- `Use $safe-refactor-engine to refactor this file safely; ask me only if the desired outcome is unclear.`

## Status

The skill is valid structurally and packaged as a standalone repository ready to version and publish.
