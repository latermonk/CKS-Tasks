#  kind cluster for  cks


```

kind create cluster --config kind-1m3w-nocni.yaml

```


```
kubectl apply -f calico.yaml
```






#  minikube for cks
```
minikube start   --kubernetes-version=1.21.2   --network-plugin=cni --cni=calico
```
