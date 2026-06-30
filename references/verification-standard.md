# Verification Standard Reference

Public standard URL: https://soothmark-verification-standard.netlify.app/standard.json

## Purpose

This public verification standard supports GenLayer nondeterministic state-impact safety review. It helps auditors decide whether AI outputs, web data, API responses, or rendered page content can safely affect stored contract state.

## Scope

Included:

- GenLayer Intelligent Contracts
- executable nondeterministic AI outputs
- web data
- API responses
- rendered page content
- state-changing usage of nondeterministic data
- validation/equivalence protection

Excluded:

- general smart contract vulnerability scanning
- gas optimization
- generic Solidity or EVM bug detection
- unrelated security review

## Classifications

- `certified`: no executable nondeterministic state-changing path exists, or the path is protected by suitable validation/equivalence.
- `conditional`: validation exists, but assumptions, boundaries, validator strength, or equivalence conditions are weak or unclear.
- `rejected`: nondeterministic output can affect stored state without meaningful validation/equivalence.

## Validation Mechanisms

- `strict_eq`: strict deterministic equality or field-level equivalence where exact matching is appropriate.
- `run_nondet_unsafe`: GenLayer leader/validator execution for custom validation of nondeterministic output before state impact.
- `prompt_comparative`: a comparative prompt checks output, evidence, or sources against a clear expected condition.
- `prompt_non_comparative`: a prompt asks for judgment without a clear comparison target.
- `none`: no meaningful validation/equivalence mechanism is present.

## Important Rules

- `response_format` is not validation.
- Executable `gl.nondet.exec_prompt`, `gl.nondet.web.get`, `gl.nondet.web.request`, or `gl.nondet.web.render` means nondeterminism is present.
- `state_impact.present` is true only when executable nondeterministic output can affect saved contract state.
- If nondeterministic output is wrapped by `gl.vm.run_nondet_unsafe`, `validation.mechanism` should usually be `run_nondet_unsafe`.
- A strong/substantive validator can be certified.
- A weak validator that only checks shape, schema, classification, allowed values, malformed output, or score ranges should be conditional with `validation.properly_used = false`.
- Weak validation is conditional, not rejected, when a real validation wrapper exists but the validator is shallow.
- If nondeterministic output can affect stored state and there is no meaningful validator, or the validator is unrelated to the state-changing path, classify as rejected.
- Do not recommend adding `run_nondet_unsafe` if it already wraps the result; recommend strengthening the validator instead.

## Operational Guidance

Audit only this path:

```text
intent -> nondeterminism -> state impact -> validation/equivalence -> classification
```

Use the full submitted contract code as the source of truth. Count only executable contract logic. Ignore GenLayer method names inside strings, comments, prompts, schemas, detector lists, documentation, recommendations, and JSON examples.

Validation must fit the exact state transition being protected. Strong validators substantively compare the leader result against deterministic invariants, trusted evidence, an independently derived equivalent result, or another clear condition that protects the stored state update.
