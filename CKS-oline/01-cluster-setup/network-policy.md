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


#  再次测试互通性


```
 k exec frontend -- curl backend

 
 k exec backend -- curl frontend

```


