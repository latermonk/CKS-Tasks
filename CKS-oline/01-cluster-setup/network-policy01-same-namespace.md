#  创建pod & svc

```
 k run frontend --image=nginx
 k run backend --image=nginx
 k expose pod frontend --port 80
 k expose pod backend --port 80
 
 k get pods,svc
```



? 如何把一个POD故意调度到一个指定的node?

```

如何把一个POD故意调度到一个指定的node?


```




#  测试互通性


```
 k exec frontend -- curl backend

 
 k exec backend -- curl frontend

```


#  新增默认阻止策略  
***deny-all-policy.yaml***

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny
  namespace: default
spec:
  podSelector: {}
  policyTypes:
  - Egress
  - Ingress
 
```


#  再次测试互通性 这次应该就不通了


```
 k exec frontend -- curl backend

 
 k exec backend -- curl frontend

```

#  添加 ingree egress  policy

***frontend.yaml***   frontend pod可以访问 backend pod

```
# allows frontend pods to communicate with backend pods
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: frontend
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: frontend
  policyTypes:
  - Egress
  egress:
  - to:
    - podSelector:
        matchLabels:
          run: backend

```

#  再次测试互通性 这次还是不通，前端能访问后端，但是后端目前还没有设置ingreess policy,所以还不通


```
 k exec frontend -- curl backend

 
 k exec backend -- curl frontend

```

***frontend.yaml***   backend  pod可以接收来自frontend  pod的访问

```
# allows backend pods to have incoming traffic from frontend pods
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
  ingress:
  - from:
    - podSelector:
        matchLabels:
          run: frontend

```


#  再次测试互通性 这次本来该通了，但是实际还是不通，禁止了访问DNS，只能使用IP进行访问


```
 k exec frontend -- curl backend

 
 k exec backend -- curl frontend

```


#   获取前后端POD的 IP



```
k get pods --show-labels -owide
```

##  前端访问后端的IP ， ok的
```
k exec frontend -- curl  backend-ip
```
##  后端访问前端的IP ， 也是ok的
```
 k exec backend -- curl  frontend-ip
```





