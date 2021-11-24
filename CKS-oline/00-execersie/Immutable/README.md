#  immutable

```
kubectl -n kube-system  get pods  kube-apiserver-i-104a999f -ojsonpath={.spec.volumes} |jq

```


or
```
kubectl get pods NAME -o yaml -n testing | grep "privi.*: true"

```
