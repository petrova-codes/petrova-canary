# petrova-canary

**Disposable.** This repository exists to be written to by PETROVA write-verb
probes. It contains nothing of value by design.

If a probe corrupts it, the remedy is deletion and recreation — there is nothing
here to recover, and that is the point. Do not add anything you would miss, do
not point a domain at it, do not publish a package from it, and do not link to it
from anywhere that matters. The moment this repo acquires value it stops being a
canary.

## Why it exists

INV-1 permits an applied write against exactly one repository. Naming an existing
repo for that role failed once already: a 2026-08-12 ruling designated `pwplz` on
the strength of an absent `url:` field in `registry.yaml`, and `pwplz` turned out
to be a live product. See
[`docs/decisions/2026-08-12-purpose-built-canary-supersedes-pwplz.md`](https://github.com/petrova-codes/petrova)
and delta D-19 in the control plane.

## Rules

- Registered in `registry.yaml` with `role: canary` and this `url:`, so probes
  exercise the real registry → App-auth → write-verb path.
- Reachable by exactly one credential: a GitHub App installed here and nowhere
  else, with `contents: write`, `pull_requests: write`, `metadata: read` — and
  explicitly not `administration` or `workflows`.
- Branch protection on `main` requires a review the App cannot supply to itself,
  so the probe can open a PR and must not be able to merge it.
- A P3 deliverable is the **negative** test: attempt a push to a second
  repository under the same credential and record the refusal. Without it, a
  green probe proves only that some credential works.
