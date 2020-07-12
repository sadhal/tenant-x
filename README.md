# tenant-x

## install cluster

TODO:

## install gitops tools and workloads with argocd

### install argocd

```bash
kubectl create namespace argocd
```

Install argocd-operator into argocd namespace from OperatorHub in new cluster.  
Install new ArgoCD instance from operator using ArgoCD manifest for given cluster in this repository.  

Update RBAC for `argocd-application-controller` so it can apply changes.  

```bash
cat <<EOF | kubectl create -f - 
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: argocd-application-controller-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: argocd-application-controller
  namespace: argocd
EOF
```

### create namespaces with ArgoCD application

Install ArgoCD Application, responsible for creating namespaces, from this repository using declarative style.  
Create ArgoCD Application with manifest `argocd-application-namespaces.yaml` for given cluster in this repository.  

### create AppProjects with ArgoCD application

Install ArgoCD Application, responsible for creating AppProjects, from this repository using declarative style.  
Create ArgoCD Application with manifest `argocd-application-argocd-projects.yaml` for given cluster in this repository.  

`AppProjects` and `Applications` for subtenants workloads are stored in GitHub. Pull Request to [tenant-x-argocd](https://github.com/sadhal/tenant-x-argocd) repository have to be used for changing their manifest files which are synced back to ArgoCD.  
Only workloads in `staging` and `production` environments are handled with ArgoCD as GitOps tool. Other environments are handled by each subtenant either through pipelines or manually.  

Manifest files and/or helm charts repos for workloads are under version control for each subtenant in one config repo, eg [subtenant-1](https://github.com/sadhal/subtenant-1) repository.  
