# Audit Schema Reference

## `classification`

One of `certified`, `conditional`, or `rejected`.

- Use `certified` when no unsafe nondeterministic state-changing path exists or the path is properly protected.
- Use `conditional` when validation exists but is weak, unclear, incomplete, or under-specified.
- Use `rejected` when nondeterministic output can affect state without meaningful validation/equivalence.

## `intent`

A string describing what the submitted contract is trying to do based on class name, storage fields, public methods, and main write flow. Describe the submitted contract’s actual purpose, not the auditor’s task.

## `nondeterminism.present`

A boolean. Use `true` only when executable contract logic uses nondeterministic AI, web, API, rendered content, or similar external/unpredictable input.

## `nondeterminism.evidence`

A list of strings naming or quoting executable code lines that show nondeterministic usage.

## `state_impact.present`

A boolean. Use `true` only when executable nondeterministic output can affect saved contract state.

## `state_impact.evidence`

A list of strings naming or quoting executable code lines that connect nondeterministic output to storage writes, counters, maps, or other persistent state.

## `validation.mechanism`

One of:

- `strict_eq`
- `run_nondet_unsafe`
- `prompt_comparative`
- `prompt_non_comparative`
- `none`

Use `run_nondet_unsafe` when nondeterministic output is wrapped by `gl.vm.run_nondet_unsafe`. Use `none` when `response_format` is the only claimed protection.

## `validation.properly_used`

A boolean. Use `true` only when the mechanism properly protects the nondeterministic state-changing path, or when no validation is required because no executable nondeterministic state impact exists.

Use `false` for weak, shape-only, schema-only, classification-only, allowed-value-only, score-range-only, unrelated, or incomplete validators.

## `validation.evidence`

A list of strings naming or quoting executable code lines that show the validation/equivalence mechanism.

## `validation.explanation`

A string explaining whether the mechanism fits the exact state-changing path.

## `recommendations`

A list of strings with the smallest useful fixes. Recommend strengthening existing validators when `run_nondet_unsafe` already wraps the result. Do not recommend adding another `run_nondet_unsafe` wrapper when one already exists.
