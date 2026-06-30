---
name: GenLayer Nondeterminism Audit Skill
description: Audit GenLayer Intelligent Contracts for unsafe nondeterministic state impact and missing validation/equivalence protection.
---

# GenLayer Nondeterminism Audit Skill

## Purpose

Use this skill to audit GenLayer Intelligent Contracts for unsafe nondeterministic state impact and missing validation/equivalence protection.

This skill references a public verification standard for GenLayer nondeterministic state-impact safety:

https://soothmark-verification-standard.netlify.app/standard.json

## When To Use

Use this skill when reviewing GenLayer Intelligent Contract code for:

- executable AI, web, API, or rendered-page nondeterminism
- nondeterministic output that can affect stored contract state
- validation/equivalence protection
- `gl.vm.run_nondet_unsafe` validator quality
- `response_format` misuse as validation
- Certified, Conditional, or Rejected audit classification

## When Not To Use

Do not use this skill for:

- broad smart contract vulnerability scanning
- gas optimization
- generic Solidity or EVM bug detection
- general code style review
- proving business logic correctness outside the nondeterminism -> state impact -> validation/equivalence path

## Core Audit Workflow

1. Identify the submitted contract's intent.
2. Find executable nondeterministic calls.
3. Ignore GenLayer method names inside strings, comments, prompts, schemas, detector lists, docs, recommendations, and JSON examples.
4. Decide whether nondeterministic output can affect stored contract state.
5. Identify validation/equivalence mechanism.
6. Classify as certified, conditional, or rejected.
7. Return evidence using actual executable code lines.
8. Recommend the smallest useful fix.

## Classification Rules

- `certified`: no executable nondeterministic state-changing path exists, or the path is protected by a suitable validation/equivalence method.
- `conditional`: validation exists, but assumptions, boundaries, validator strength, or equivalence conditions are weak or unclear.
- `rejected`: nondeterministic output can affect stored state without meaningful validation/equivalence.

Weak validation is conditional, not certified. Nondeterministic state impact with no meaningful validation is rejected.

## GenLayer-Specific Rules

- `response_format` is not validation.
- Executable `gl.nondet.exec_prompt`, `gl.nondet.web.get`, `gl.nondet.web.request`, or `gl.nondet.web.render` means nondeterminism is present.
- `state_impact.present` is true only when executable nondeterministic output can affect saved contract state.
- If nondeterministic output is wrapped by `gl.vm.run_nondet_unsafe`, `validation.mechanism` should usually be `run_nondet_unsafe`.
- A strong/substantive validator can be certified.
- A weak validator that only checks shape, schema, classification, allowed values, malformed output, or score ranges should be conditional with `validation.properly_used = false`.
- No meaningful or unrelated validator for nondeterministic state impact should be rejected.
- Do not recommend adding `run_nondet_unsafe` if it already wraps the result; recommend strengthening the validator instead.

## Output Schema

Return audit results using exactly this schema:

```json
{
  "classification": "certified | conditional | rejected",
  "intent": "string",
  "nondeterminism": {
    "present": true,
    "evidence": ["string"]
  },
  "state_impact": {
    "present": true,
    "evidence": ["string"]
  },
  "validation": {
    "mechanism": "strict_eq | run_nondet_unsafe | prompt_comparative | prompt_non_comparative | none",
    "properly_used": true,
    "evidence": ["string"],
    "explanation": "string"
  },
  "recommendations": ["string"]
}
```

Boolean fields must be JSON booleans, not strings.

## Evidence Rules

- Quote or name executable code lines where possible.
- Do not treat method names inside strings, comments, schemas, prompts, docs, detector lists, recommendations, or JSON examples as executable usage.
- Identify the specific path from nondeterministic output to stored contract state.
- Identify the specific validation/equivalence mechanism and whether it protects that path.

## Recommendation Rules

- Recommend the smallest useful fix.
- If `run_nondet_unsafe` is absent and nondeterministic output affects state, recommend adding suitable validation/equivalence.
- If `run_nondet_unsafe` already wraps the result but the validator is weak, recommend strengthening the validator instead of adding another wrapper.
- If `response_format` is the only protection, recommend real validation/equivalence.

## References

Read these files when more detail is needed:

- `references/verification-standard.md` for the public standard summary.
- `references/audit-schema.md` for field meanings.
- `references/examples.md` for common classification examples.
