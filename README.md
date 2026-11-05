# Helm + Kustomize environment-specific values

The local Helm chart lives in `base/charts/mychart`. The common values are in `base/values.yaml`.
Each environment defines its own `helmCharts` entry and adds a values file with `additionalValuesFiles`.
This is intentional: Kustomize overlays cannot patch the `helmCharts` generator configuration inherited from a base.

## Build

From this project's root:

```sh
kustomize build overlays/prod --enable-helm --load-restrictor LoadRestrictionsNone
```
For Argo CD, configure `kustomize.buildOptions: --enable-helm` in argocd-cm and ensure Helm is available in repo-server.

`LoadRestrictionsNone` may be needed for the main chart path outside the overlay,
depending on the Kustomize version. For Argo CD, configure the equivalent build
options and ensure the repo-server has Helm installed.


## Folders

- **dev**: Basic Kustomize overlay extending the base configuration.
- **prod**: Helm chart with environment-specific values and Kustomize patches.
- **prod2**: Multiple Helm charts, including an additional chart for custom resources, with environment-specific values and Kustomize patches.