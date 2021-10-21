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




---




#  secure ingress


```
curl  https://192.168.211.40:32300/service1 -kv


```




```

openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes

ls

k create secret tls secure-ingress --cert=cert.pem --key=key.pem

k get sec

 k get secret
 
```


```
 vim secure-ingress.yaml
```
```
kind: Ingress
metadata:
  name: secure-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
      - secure-ingress.com
    secretName: secure-ingress
  rules:
  - host: secure-ingress.com  
    http:
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


```
k apply -f secure-ingress.yaml 
```

```
curl  https://secure-ingress.com:32300/service2 -kv --resolv secure-ingress.com:32300:192.168.211.41     

```



