# example

A minimal, installable reference chart. It deploys a single unprivileged nginx
pod and exists to:

1. exercise the CI (lint, install test, release + signing), and
2. serve as a starting point - copy this directory to create a new chart.

## Installing

Charts are published as OCI artifacts (no `helm repo add`):

```sh
helm install my-release oci://registry.mareshq.com/catalog/charts/example --version 0.1.0
```

## Verifying the signature

```sh
cosign verify \
  --certificate-identity-regexp '^https://github.com/vojtechmares/catalog-charts' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  registry.mareshq.com/catalog/charts/example:0.1.0
```

## Values

See [`values.yaml`](./values.yaml). Key options:

| Key | Default | Description |
| --- | --- | --- |
| `replicaCount` | `1` | Number of replicas. |
| `image.repository` | `nginxinc/nginx-unprivileged` | Image repository. |
| `image.tag` | `1.27.3-alpine` | Image tag. |
| `service.port` | `8080` | Service port. |
| `resources` | see values | CPU/memory requests and limits. |

## Releasing

Bump `version` in `Chart.yaml` and merge to `main`. CI packages the chart,
publishes it to the registry and signs it. Publishing is idempotent - an
existing `name:version` is never overwritten.
