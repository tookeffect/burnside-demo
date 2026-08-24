# TookEffect × Burnside Project — Integration Surface

This repository is a **public, documentation-only integration reference** for exploring a small post-action evidence gate between Burnside Project / Factory Floor and TookEffect.

It intentionally contains **no TookEffect production source code, provider credentials, signing secrets, internal verification logic, or private infrastructure details**.

## What TookEffect does

TookEffect treats an external action as an intended effect with an expected authoritative state.

The integration boundary is deliberately simple:

1. A workflow provides the intended effect and the expected state.
2. TookEffect independently reads the authoritative external provider rather than trusting the executor's own success message.
3. TookEffect returns one normalized verdict:
   - `APPLIED`
   - `NOT_APPLIED`
   - `AMBIGUOUS`
4. The result is accompanied by structured Evidence and a verifiable Receipt.

If the external state cannot be established strongly enough, TookEffect remains `AMBIGUOUS` rather than inferring success.

## Why this fits Factory Floor

A possible boundary is:

`Factory Floor governance → agent/action → TookEffect independent read-back → verdict + Evidence + Receipt`

Factory Floor can remain responsible for specification, implementation, audit, quality gates, and approval. TookEffect can sit after the action as a separate observer of the resulting external state.

## First controlled experiment

The proposed first experiment is intentionally narrow:

- one disposable GitHub repository;
- one controlled pull request;
- one exact expected head SHA;
- independent GitHub read-back;
- one normalized verdict;
- one Evidence/Receipt artifact returned to the calling workflow.

See [`docs/integration-contract.md`](docs/integration-contract.md) for the public integration contract and [`examples/`](examples/) for sanitized examples.

## Public product surface

TookEffect: https://tookeffect.com/

## Security boundary

This repository documents **what an integrating system can provide and receive**, not how TookEffect implements verification internally.

See [`SECURITY.md`](SECURITY.md).
