# MetalLb


##  MetalLb with KIND


##  MetalLb with  VAGRANT 


**Install  kubectl**
https://kubernetes.io/docs/tasks/tools/install-kubectl/

```
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"

```


**kubectl  CHEESHEET**
https://kubernetes.io/docs/reference/kubectl/cheatsheet/   

```
source <(kubectl completion bash)

echo "source <(kubectl completion bash)" >> ~/.bashrc


alias k=kubectl
complete -F __start_kubectl k
```


**KIND**

INSTALL
https://kind.sigs.k8s.io/docs/user/quick-start/#installation   
```
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.9.0/kind-linux-amd64
chmod +x ./kind
mv ./kind /some-dir-in-your-PATH/kind
```
CREATE  A  CLUSTER 

```
kind create  cluster
```
CONFIG & CREATE A  Multinode cluster 

```
https://kind.sigs.k8s.io/docs/user/configuration/





kind create cluster --config=config.yaml
```

**MetalLb**
INSTALL    
https://metallb.universe.tf/installation/    

```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.5/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.5/manifests/metallb.yaml
# On first install only
kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"

```


configuration   
https://metallb.universe.tf/configuration/   

```
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 192.168.1.240-192.168.1.250

```


#  Reference 

https://mauilion.dev/posts/kind-metallb/    

https://banzaicloud.com/blog/multi-cluster-testing/    





