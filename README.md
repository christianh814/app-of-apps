# app-of-apps

1. `kind create cluster`
2. `kustomize build bootstrap/ | kubectl apply -f -`
3. `kubectl apply -f apps/app-of-apps.yaml`
4. Add more apps

```shell
cat <<EOF > apps/welcome.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: welcome-app
  namespace: argocd
spec:
  destination:
    name: in-cluster
    namespace: welcome-app
  source:
    path: welcome-k8s/overlays/default
    repoURL: 'https://github.com/christianh814/gitops-examples'
    targetRevision: main
  project: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
    retry:
      limit: 5
      backoff:
        duration: 5s
        maxDuration: 3m0s
        factor: 2
EOF
```

5. Commit and push...profit!
