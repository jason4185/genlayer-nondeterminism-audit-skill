# GenLayer Nondeterminism Audit Skill

GenLayer Nondeterminism Audit Skill is an agent skill for reviewing GenLayer Intelligent Contracts for unsafe nondeterministic state impact and missing validation/equivalence protection.

## Purpose

Use this repo as a lightweight skill package for AI coding agents that audit nondeterministic state-impact safety in GenLayer Intelligent Contracts.

## What It Checks

- executable AI, web, API, or rendered-page nondeterminism
- whether nondeterministic output can affect stored contract state
- validation/equivalence protection
- Certified, Conditional, or Rejected classification

## Repository Contents

```text
genlayer-nondeterminism-audit-skill/
├── README.md
├── SKILL.md
└── references/
    ├── audit-schema.md
    ├── examples.md
    └── verification-standard.md
```

## Intended Use

Use this skill for the GenLayer Intelligent Contract audit workflow:

```text
intent -> nondeterminism -> state impact -> validation/equivalence -> classification
```

It is not a general smart contract vulnerability scanner, gas optimizer, or full business logic prover.

## Using This Skill With AI Agents

This repository is designed to be used as an agent skill package. Developers can make the skill available to an AI coding agent, then ask the agent to review GenLayer Intelligent Contract code for nondeterministic state-impact safety.

A typical workflow is:

1. Add this skill folder to the agent's skills directory or make the repository available in the agent's workspace.
2. Open a GenLayer Intelligent Contract project in the agent environment.
3. Ask the agent to use the GenLayer Nondeterminism Audit Skill.
4. Provide the contract code or point the agent to the relevant contract file.
5. Review the returned JSON audit result and recommendations.

Example prompts:

```text
Use the GenLayer Nondeterminism Audit Skill to audit this contract for unsafe nondeterministic state impact.
```

```text
Review this GenLayer Intelligent Contract for AI/web/API/rendered data usage, state impact, and validation/equivalence protection.
```

```text
Classify this contract as certified, conditional, or rejected using the GenLayer Nondeterminism Audit Skill.
```

The skill is intended to help agents produce consistent, evidence-based audit results using the schema in `SKILL.md` and the references in the `references/` directory.

## Agent Output

When the skill is used, the agent should return a structured audit result with:

- classification
- intent
- nondeterminism
- state impact
- validation/equivalence assessment
- recommendations

The output should identify executable code evidence and explain whether the validation mechanism protects the exact nondeterministic state-changing path.

## Public Verification Standard

This skill references a public verification standard for GenLayer nondeterministic state-impact safety:

https://soothmark-verification-standard.netlify.app/standard.json

## Expected Output

Audit results use a JSON object with classification, intent, nondeterminism, state impact, validation, and recommendations fields. See `SKILL.md` and `references/audit-schema.md` for the exact schema.

## Limitations

The skill reviews nondeterministic state-impact safety only. It does not prove full business logic correctness, deploy contracts, or assess unrelated security properties.
