# tenant-x

## install cluster

TODO:

### install argocd

```bash
kubectl create namespace argocd
```

Install argocd-operator into argocd namespace from OperatorHub in new cluster.  
New instance of ArgoCD from installed operator will be created by `k8s-reconsiler`.  

### install namespace reconciler

Follow instructions in https://github.com/sadhal/ns-reconciler with newly created cluster's name.  
`k8s-reconciler` pointing to this repository will use cluster's kustomization.yaml found in [overlays](overlays/). Overlay kustomization will point out [bases](bases/). Bases have own kustomization.yaml which includes resources that will be created eg. `Namespace`s, `LimitRange`s and `ResourceQuota`s.

`k8s-reconciler` pointing to [tenant-x-argocd](https://github.com/sadhal/tenant-x-argocd) repository will use cluster's kustomization.yaml found in [overlays](overlays/). Overlay kustomization will point out [bases](bases/). Bases have own kustomization.yaml which includes ArgoCD resources that will be created eg. `ArgoCD`, `Appproject`s and `Applications`s.  

#### setup ArgoCD projects

`k8s-reconciler` does that automatically. Changes done to ArgoCD projects need to be commited to GitHub. Pull Request to [tenant-x-argocd](https://github.com/sadhal/tenant-x-argocd) repository have to be used for that purpose.  

#### setup ArgoCD applications

`k8s-reconciler` does that automatically. Changes done to ArgoCD projects need to be commited to GitHub. Pull Request to [tenant-x-argocd](https://github.com/sadhal/tenant-x-argocd) repository have to be used for that purpose.  
