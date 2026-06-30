# Audit Examples

## 1. Deterministic Storage-Only Contract

Pattern:

```python
@gl.public.write
def set_value(self, value: str):
    self.value = value
```

Expected result:

- classification = certified
- nondeterminism.present = false
- state_impact.present = false
- validation.mechanism = none
- validation.properly_used = true

Reason: the contract stores user-provided or deterministic values only. No nondeterministic validation is required.

## 2. AI Output Stored With Only `response_format`

Pattern:

```python
result = gl.nondet.exec_prompt(prompt, response_format="json")
self.result = result["answer"]
```

Expected result:

- classification = rejected
- nondeterminism.present = true
- state_impact.present = true
- validation.mechanism = none
- validation.properly_used = false

Reason: `response_format` only structures model output. It does not validate equivalence or protect the state update.

## 3. `run_nondet_unsafe` With Shape/Classification-Only Validator

Pattern:

```python
def leader_fn():
    return gl.nondet.exec_prompt(prompt, response_format="json")

def validator_fn(result):
    return isinstance(result, dict) and result.get("classification") in ["certified", "conditional", "rejected"]

self.audit = gl.vm.run_nondet_unsafe(leader_fn, validator_fn)
```

Expected result:

- classification = conditional
- validation.mechanism = run_nondet_unsafe
- validation.properly_used = false

Reason: a validation wrapper exists, but the validator only checks shape or classification values. It does not substantively verify the result against the submitted contract or an equivalent source.

## 4. `run_nondet_unsafe` With Substantive Validator

Pattern:

```python
def leader_fn():
    return gl.nondet.web.get(price_url).body.decode("utf-8")

def validator_fn(leader_result):
    reference = gl.nondet.web.get(reference_url).body.decode("utf-8")
    return compare_normalized_price(leader_result, reference)

self.price = gl.vm.run_nondet_unsafe(leader_fn, validator_fn)
```

Expected result:

- classification = certified
- validation.mechanism = run_nondet_unsafe
- validation.properly_used = true

Reason: the validator substantively checks the leader result against an independently derived equivalent result before storage.
