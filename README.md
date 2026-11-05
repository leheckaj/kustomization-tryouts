# Helm + Kustomize environment-specific values

The local Helm chart lives in `base/charts/mychart`. The common values are in `base/values.yaml`.
Each environment defines its own `helmCharts` entry and adds a values file with `additionalValuesFiles`.
This is intentional: Kustomize overlays cannot patch the `helmCharts` generator configuration inherited from a base.

Build:

```sh
kustomize build overlays/prod --enable-helm --load-restrictor LoadRestrictionsNone
```

Requirements: Kustomize with Helm support, and Helm installed on PATH.
For Argo CD, configure `kustomize.buildOptions: --enable-helm` in argocd-cm and ensure Helm is available in repo-server.

Expected result: dev uses one replica and nginx:1.27-alpine; prod uses three replicas and a LoadBalancer Service.
