# flux-example

```
###Infrastructure Deployment
apiVersion: source.toolkit.fluxcd.io/v1beta2
kind: GitRepository
metadata:
  namespace: flux-system
  name: flux-example
spec:
  timeout: 20s
  gitImplementation: libgit2
  interval: 1m
  url: "https://github.com/KabarukhinAlexey/flux-example"
  ref:
    branch: main
---
apiVersion: kustomize.toolkit.fluxcd.io/v1beta2
kind: Kustomization
metadata:
  namespace: flux-system
  name: platform-flux-monitoring
spec:
  timeout: 2m
  path: ./clusters/infrastructure/flux
  interval: 5m
  prune: true
  force: false
  sourceRef:
    name: flux-example
    kind: GitRepository
---
apiVersion: kustomize.toolkit.fluxcd.io/v1beta2
kind: Kustomization
metadata:
  namespace: flux-system
  name: multi-tenant-infrastructure-kyverno
spec:
  timeout: 2m
  path: ./clusters/infrastructure/kyverno
  interval: 5m
  prune: true
  force: false
  sourceRef:
    name: flux-example
    kind: GitRepository

###Staging Application Deployment

apiVersion: kustomize.toolkit.fluxcd.io/v1beta2
kind: Kustomization
metadata:
  namespace: flux-system
  name: multi-tenant-staging
spec:
  timeout: 2m
  path: ./clusters/staging
  interval: 5m
  prune: true
  force: false
  sourceRef:
    name: flux-example
    kind: GitRepository
```
