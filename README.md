# catalog-charts

An open collection of production-ready Helm charts - a maintained replacement for
Bitnami charts. Part of the [catalog](https://github.com/vojtechmares) project.

## Using the charts

Charts are published as **OCI artifacts** to `registry.mareshq.com`. There is no
`helm repo add` index; reference charts directly:

```sh
# install
helm install my-release oci://registry.mareshq.com/catalog/charts/<chart> --version <version>

# show values
helm show values oci://registry.mareshq.com/catalog/charts/<chart> --version <version>
```

As a dependency in another chart's `Chart.yaml`:

```yaml
dependencies:
  - name: <chart>
    version: <version>
    repository: oci://registry.mareshq.com/catalog/charts
```

Every chart is signed with [cosign](https://docs.sigstore.dev/) (keyless). See a
chart's README for the exact `cosign verify` command.

## Available charts

| Chart | Description |
| --- | --- |
| [`example`](./charts/example) | Minimal reference chart / template for new charts. |

More charts are added over time. Need one prioritized? Open an issue.

## Repository layout

```
charts/
  <chart>/
    Chart.yaml
    values.yaml
    templates/
    ci/               # values files used by CI install tests
    README.md
```

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md). In short: copy `charts/example`, adjust
it, open a PR. CI lints the chart and installs it on a kind cluster. Merging to
`main` releases any chart whose `version` was bumped.

## Premium

Hardened images and installation/upgrade support are part of the commercial
offering - see [`catalog-hardened-images`](https://github.com/vojtechmares/catalog-hardened-images).

## License

[Apache-2.0](./LICENSE).
