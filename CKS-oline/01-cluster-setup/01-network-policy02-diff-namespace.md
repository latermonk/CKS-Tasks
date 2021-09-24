#  新建namespace并加标签   ns:cassandra


```
kubectl create ns cassandra
kubectl edit ns cassandra	

```

```
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: "2021-04-20T07:19:22Z"
  name: cassandra
  resourceVersion: "533198"
  uid: 766ae069-4dc9-4acd-a4db-ce852c293cc6
  labels:  #添加
    ns: cassandra #添加
spec:
  finalizers:
  - kubernetes
status:
  phase: Active

```

#  新namespace内部新建一个POD


```
 k  -n cassandra run cassandra --image=nginx


```

```bash
 k -n cassandra get pod -owide
```

```bash
k exec backend -- curl   cassandra‘ IP  address          发现不通 
```

#  需要new netpol，让后端能够通过ingress接收 frontend的流量[podSelector]，并通过egress访问后端的 da pod[namespaceSelector]


```
vim backend.yaml

```


```
# allows backend pods to have incoming traffic from frontend pods and to cassandra namespace
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: backend
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
      - podSelector:
          matchLabels:
            run: frontend
  egress:
    - to:
      - namespaceSelector:
          matchLabels:
            ns: cassandra
```

```bash
 k apply -f backend.yaml
```


```bash
 k exec backend -- curl 192.168.104.26
```


#   cassandra nsmespace 内部新建策略-deny-all

```bash
vim  cassandra-deny.yaml
```

```
# deny all incoming and outgoing traffic from all pods in namespace cassandra
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: cassandra-deny
  namespace: cassandra
spec:
  podSelector: {}
  policyTypes:
    - Ingress
    - Egress
```



```
k create -f cassandra-deny.yaml 
```
这时访问就不行了：

```
k exec backend -- curl 192.168.104.26
```

需要新建策略才行：


```
vim  cassandra-accept.yaml 
```

```
# allows cassandra pods having incoming connection from backend namespace
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: cassandra
  namespace: cassandra
spec:
  podSelector:
    matchLabels:
      run: cassandra
  policyTypes:
    - Ingress
  ingress:
    - from:
      - namespaceSelector:
          matchLabels:
            ns: default
```

```
k create -f cassandra.yaml 
```
然后default ns添加相应的标签：

```
 k edit ns default



```

```
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: "2021-01-19T03:27:58Z"
  labels: #添加
    ns: default   #添加
  name: default
  resourceVersion: "541475"
  uid: 2d566715-f0a4-49b3-b590-dfa7df30d0ba
spec:
  finalizers:
  - kubernetes
status:
  phase: Active
```
#  再次测试访问cassandra内部的pod ok 

```bash
 k exec backend -- curl 192.168.104.26
```
