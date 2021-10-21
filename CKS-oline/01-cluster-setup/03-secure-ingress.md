#  secure  ingress 


```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.40.2/deploy/static/provider/baremetal/deploy.yaml

```

```
k get pod,svc -n ingress-nginx

```





#  App


```
cat secure-ingress.yaml
```


```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: secure-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /service1
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 80
      - path: /service2
        pathType: Prefix
        backend:
          service:
            name: service2
            port:
              number: 80
              
```



#

```
k create -f secure-ingress.yaml 

k get ing


```



```
k run pod1 --image=nginx

k run pod2 --image=httpd

```


```
k expose pod pod1 --port 80 --name service1
k expose pod pod2 --port 80 --name service2
```

curl  http://192.168.211.40:31459/service1

curl  http://192.168.211.40:31459/service2
