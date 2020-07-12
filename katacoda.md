# Install on katacoda

https://www.katacoda.com/courses/kubernetes/getting-started-with-kubeadm

## Install argocd-operator

```bash
kubectl create ns argocd
git clone https://github.com/argoproj-labs/argocd-operator.git && cd argocd-operator
kubectl create -f deploy/service_account.yaml -n argocd
kubectl create -f deploy/role.yaml -n argocd
kubectl create -f deploy/role_binding.yaml -n argocd
kubectl create -f deploy/argo-cd -n argocd
kubectl create -f deploy/crds -n argocd
kubectl get crd
kubectl create -f deploy/operator.yaml -n argocd
kubectl get pods -n argocd
#kubectl create -n argocd -f examples/argocd-ingress.yaml
#kubectl get pods -n argocd
cd ..
```

## Install k8s-reconciler for namespaces

```bash
kubectl create ns appinfra-k8s-reconciler
git clone https://github.com/sadhal/k8s-reconciler && cd k8s-reconciler
kubectl -n appinfra-k8s-reconciler apply -f shell-operator-rbac.yaml
#oc adm policy add-scc-to-user anyuid -z shell-operator-k8s-reconciler -n appinfra-k8s-reconciler
kubectl -n appinfra-k8s-reconciler apply -f shell-operator-deployment-ns-cluster-0.yaml
kubectl -n appinfra-k8s-reconciler get pods
kubectl -n appinfra-k8s-reconciler logs ns-reconciler-....
kubectl -n appinfra-k8s-reconciler get ns
```

## Install k8s-reconciler for argocd projects and applications

```bash
kubectl get appprojects.argoproj.io -n argocd
kubectl -n appinfra-k8s-reconciler apply -f shell-operator-deployment-argocd-cluster-0.yaml
kubectl get appprojects.argoproj.io -n argocd
kubectl get applications.argoproj.io -n argocd

# kubectl create clusterrole my-admin --verb=create,delete,get,list,update,watch,patch --resource=*

# kubectl create clusterrolebinding my-admin-binding --clusterrole=my-admin --serviceaccount=argocd:system:serviceaccount:argocd:argocd-application-controller

# oc adm policy add-cluster-role-to-user my-admin system:serviceaccount:argocd:argocd-application-controller

cat <<EOF | kubectl create -f - 
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: my-argocd-admin-user
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
