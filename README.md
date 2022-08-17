# Weaveworks GitHub Action

Enforce cloud security and best practices at the build and deployment time.

## Supported Kustomizations
- [x] [Helm](https://helm.sh)
- [x] [Kustomize](https://kustomize.io)


## Usage

| Parameter                     | Description                                                           | Required  | Default |
| ----------------------------- | --------------------------------------------------------------------- | --------- | ------- |
| `path`                        | Path to Kustomization, Helm Chart or plain Kubernetes files           |    Yes    |    -    |
| `helm-values-file`            | Path to resources helm values file                                    |    No     |    -    |
| `policies-path`               | Path to policies Kustomization, Helm Chart or plain Kubernetes files  |    Yes    |    -    |
| `policies-helm-values-file`   | Path to policies helm values file                                     |    No     |    -    |
| `remediate`                   | Automatically remediate resources if possible                         |    No     |  false  |
| `sarif-file`                  | Export result in sarif format                                         |    No     |    -    |


```yaml
name: Weaveworks
on:
  push:
    branches: [ master ]
jobs:
  main:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: weaveworks/weave-action@v1
        with:
          path: entities-to-scan
          policies-path: policies
          remediate: true # enable auto remediation
          sarif-file: results.sarif ## export result in SARF format to send it to github code scanning

      # send results to github code scanning
      - uses: github/codeql-action/upload-sarif@v2
        if: always()
        with:
          sarif_file: results.sari
```

## Examples

### Using Remote Policies Repository
```yaml
- run: git clone https://github.com/<owner>/<repository> -b <branch>
- uses: weaveworks/weave-action@v1
  with:
    path: helm/
    helm-values-file: helm/dev-values.yaml
    policies-path: <repository>/<path to policies kustomization directory>
```

### Enable Auto Remediation
```yaml
- uses: weaveworks/weave-action@v1
  with:
    path: helm/
    helm-values-file: helm/dev-values.yaml
    policies-path: policies
    remediate: true # enable auto remediation
```


### Enable Github Code Scanning
```yaml
- uses: weaveworks/weave-action@v1
  with:
    path: helm/
    helm-values-file: helm/dev-values.yaml
    policies-path: policies
    sarif-file: results.sarif # export result into SARIF format

# upload results to github
- uses: github/codeql-action/upload-sarif@v2
  if: always()
  with:
    sarif_file: results.sarif
```
