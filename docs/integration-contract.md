# Public Integration Contract

This document describes the **observable integration boundary** only. It does not document TookEffect's internal verification implementation.

## Input

For a controlled GitHub merge verification, an integrating system can provide an exact intended effect such as:

```json
{
  "target_id": "<verified-target-id>",
  "owner": "example-org",
  "repo": "disposable-test-repo",
  "pull_number": 1,
  "expected_head_sha": "<exact-head-sha>",
  "expected_base": "main",
  "require_successful_checks": true,
  "external_context": {
    "pipeline_run_id": "<approved-run-id>",
    "evidence_bundle_id": "<approved-evidence-id>"
  }
}
```

The values above are placeholders. No live credentials or private identifiers are stored in this repository.

## Verification boundary

TookEffect does not treat the executor's own success output as proof of the resulting state.

At the public contract level, TookEffect:

- binds the request to the intended target and expected state;
- performs an independent authoritative provider read-back;
- compares the observed state with the intended postcondition;
- returns a fail-closed normalized verdict.

## Output

The calling workflow receives a result shaped around:

```json
{
  "effect_id": "<effect-id>",
  "verdict": "APPLIED | NOT_APPLIED | AMBIGUOUS",
  "evidence": {
    "expected": "<sanitized expected state>",
    "observed": "<sanitized observed state>"
  },
  "receipt": {
    "evidence_digest": "<digest>",
    "signature": "<verifiable receipt signature>"
  }
}
```

Exact production response fields may evolve behind a versioned contract. The important boundary for this experiment is that the calling system can associate the verdict and evidence with the exact effect it requested.

## Independence model

The actor making the change does not determine TookEffect's verdict.

The verification path can be separated from the actor by credentials/scopes and by authoritative provider read-back. If the required external state cannot be proven from the available authoritative observation surface, the result remains `AMBIGUOUS`.

## Proposed Factory Floor experiment

For the first test:

1. Factory Floor prepares/approves one exact GitHub PR action.
2. The handoff includes the exact repository, PR, expected head SHA, and optional pipeline/evidence references.
3. TookEffect verifies the resulting external GitHub state independently.
4. Factory Floor stores the returned verdict + Evidence/Receipt as a post-action evidence artifact.

The experiment does not require access to TookEffect's production source code or internal verification implementation.
