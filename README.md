# Weaveworks GitHub Action

Enforce cloud security and best practices at the build and deployment time.

## Supported Kustomizations
- [x] [Helm](https://helm.sh)
- [x] [Kustomize](https://kustomize.io)


## Usage

| Parameter                     | Description                                     | Required  |
| ----------------------------- | ----------------------------------------------- | --------- |
| `path`                        | Path to resources kustomization                 |    Yes    |
| `helm-values-file`            | Path to resources helm values file              |    No     |
| `policies-path`               | Path to policies kustomization                  |    Yes    |
| `policies-helm-values-file`   | Path to policies helm values file               |    No     |
| `remediate`                   | Automatically remediate resources if possible   |    No     |
| `sarif-file`                  | Export result in sarif format                   |    No     |


```yaml
- uses: weaveworks/weave-action@v1
  with:
    path: <path to resources kustomization directory>
    policies-path: <path to policies kustomization directory>
```

## Examples

### Using Remote Policies Repository
```yaml
- run: git clone https://github.com/<owner>/<repository> -b <branch>
- uses: weaveworks/weave-action@v1
  with:
    path: helm/
    policies-path: <repository>/<path to policies kustomization directory>
```

### Enable Auto Remediation
```yaml
- uses: weaveworks/weave-action@v1
  with:
    path: helm/
    policies-path: policies
    remediate: true # enable auto remediation
```


### Enable Github Code Scanning
```yaml
- uses: weaveworks/weave-action@v1
  with:
    path: helm/
    policies-path: policies
    sarif-file: results.sarif # export result into SARIF format

# upload results to github
- uses: github/codeql-action/upload-sarif@v2
  if: always()
  with:
    sarif_file: results.sarif
```
