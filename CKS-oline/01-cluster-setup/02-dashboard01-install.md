#  新建dashboard
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.1.0/aio/deploy/recommended.yaml
```

修改 deployment , svc

```
 k -n kubernetes-dashboard edit deploy kubernetes-dashboard
```


```
edit deploy


 --auto-generate-certificates
 - --namespace=kubernetes-dashboard



change  to:

- --namespace=kubernetes-dashboard
- --insecure-port=9090


```



```

edit  svc 


  clusterIP: 10.99.150.161
  clusterIPs:
  - 10.99.150.161
  ports:
  - port: 9090             #443改为9090
    protocol: TCP
    targetPort: 9090        #8443改为9090
  selector:
    k8s-app: kubernetes-dashboard
  sessionAffinity: None
  type: NodePort           #ClusterIP改为NodePort
```
#  通过NodePort访问 kube-dashboard

http://192.168.211.40:30613/


