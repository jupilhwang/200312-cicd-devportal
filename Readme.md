# CICD with PKS @200312

### PKS w/NSX-T on vSphere
  - pks 

### Installation

#### Helm Tiller
``` yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
 - kind: ServiceAccount
   name: tiller
   namespace: kube-system
```

#### Default Storage Class

```bash
kubectl create -f -<<-EOF
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name:  thick
provisioner: kubernetes.io/vsphere-volume
parameters:
  diskformat: zeroedthick
EOF
```
```bash
kubectl patch storageclass <storage-class-name> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'

kubectl patch storageclass thick -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

###### Helm Repo Add (Stable, Incubator)

```bash
helm repo add stable https://kubernetes-charts.storage.googleapis.com
helm repo add incubator https://kubernetes-charts-incubator.storage.googleapis.com/
helm repo add bitnami https://charts.bitnami.com/bitnami

helm repo update
```

