#  kind cluster for  cks


```

kind create cluster --config kind-1m3w-nocni.yaml


https://raw.githubusercontent.com/latermonk/CKS-Tasks/master/CKS-oline/env/kind-1m3w-nocni.yaml

```


```
kubectl apply -f calico.yaml

https://raw.githubusercontent.com/latermonk/CKS-Tasks/master/CKS-oline/env/calico.yaml


```






#  minikube for cks
```
minikube start   --kubernetes-version=1.21.2   --network-plugin=cni --cni=calico
```
