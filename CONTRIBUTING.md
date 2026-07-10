# Contributing to catalog-charts

Thanks for helping build an open Bitnami replacement.

## Adding or changing a chart

1. **Start from the template.** Copy `charts/example` to `charts/<your-chart>`.
2. **Edit `Chart.yaml`.** Set `name`, `description`, `version` (chart version)
   and `appVersion` (packaged app version).
3. **Write templates and `values.yaml`.** Prefer secure defaults: run as
   non-root, drop capabilities, set resource requests/limits.
4. **Add CI test values.** Put one or more `ci/*-values.yaml` files in the chart
   directory. `ct install` runs the chart once per file on a kind cluster.
5. **Bump the version.** Any change to a chart requires a `version` bump
   (enforced by `ct` via `check-version-increment`).

## Local checks

```sh
# lint changed charts
ct lint --config ct.yaml --target-branch main

# render templates
helm template charts/<your-chart>

# install on a local kind cluster
kind create cluster
ct install --config ct.yaml --target-branch main
```

## How releases work

- Open a pull request. CI (`lint-test`) lints and install-tests changed charts.
- Merge to `main`. CI (`release`) packages each chart whose `version` is not yet
  in the registry, pushes it to `oci://registry.mareshq.com/catalog/charts/<name>`
  and signs it with cosign.
- Releases are **idempotent**: an existing `name:version` is never overwritten.
  To publish a change, bump the version.

## Commit messages

Use [Conventional Commits](https://www.conventionalcommits.org/), e.g.
`feat(example): add ingress support` or `fix(redis): correct probe port`.
