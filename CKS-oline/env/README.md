#  kind cluster for  cks


```
wget https://github.com/kubernetes-sigs/kind/releases/download/v0.11.1/kind-linux-amd64

install  kind-linux-amd64 /usr/local/bin/kind

```


```

wget https://raw.githubusercontent.com/latermonk/CKS-Tasks/master/CKS-oline/env/kind-1m3w-nocni.yaml

kind create cluster --config kind-1m3w-nocni.yaml


```


```

wget https://raw.githubusercontent.com/latermonk/CKS-Tasks/master/CKS-oline/env/calico.yaml


kubectl apply -f calico.yaml


```






#  minikube for cks
```
minikube start   --kubernetes-version=1.21.2   --network-plugin=cni --cni=calico
```
