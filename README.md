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

## Public Verification Standard

This skill references a public verification standard for GenLayer nondeterministic state-impact safety:

https://soothmark-verification-standard.netlify.app/standard.json

## Expected Output

Audit results use a JSON object with classification, intent, nondeterminism, state impact, validation, and recommendations fields. See `SKILL.md` and `references/audit-schema.md` for the exact schema.

## Limitations

The skill reviews nondeterministic state-impact safety only. It does not prove full business logic correctness, deploy contracts, or assess unrelated security properties.
