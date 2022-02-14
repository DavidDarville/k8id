# Updating argocd-helm-charts applications

## Test update - with current values files

- generate current helm yaml output

```bash
  helm dep up ./<application-folder-name>
  helm template ./<application-folder-name> --values ./<application-folder-name>/values.yaml --values /path/to/cluster-specific-values.yaml > before.yaml

```

- Update the umbrella and Upstream chart versions in `Chart.yaml`
  - set this chart (the umbrealla chart) to same as AppVersion in upstream chart
  - Update dependency chart version to match the one you want to update to (see link to upstream source in `Chart.yaml`)

- generate helm yaml output for new version

```bash
  helm dep up ./<application-folder-name>
  helm template ./<application-folder-name> --values ./<application-folder-name>/values.yaml --values /path/to/cluster-specific-values.yaml > after.yaml
```

- push changes to MR

- Then when merged, it should be out of sync. Sync it on ArgoCD

- Confirm the version has been updated

- updating charts.yaml for the 2 versions and helm dep up will update Chart.lock

- if it doesn't automatiically, CI will fail and it will then inform you what exactly should be included in the
  `Chart.lock` file - for it to succed.

**While running ```helm dep up <repo>``` - if you encounter The error below during the update on helm chart version:**

```bash
  Error: cannot get a valid version for repositories metrics-server. Try changing the version constraint in Chart.yaml
```

- add the repo to your local helm

```bash
  helm add <repo> <repo url>
```

- check for available versions

```bash
  helm search repo <repo>
```

- update the versions to delete outdated charts