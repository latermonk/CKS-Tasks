#  SA

```

root@master:~/cks/RBAC# k get sa,secrets
NAME                     SECRETS   AGE
serviceaccount/default   1         9m50s

NAME                         TYPE                                  DATA   AGE
secret/default-token-9srgx   kubernetes.io/service-account-token   3      9m50s
root@master:~/cks/RBAC#  k describe sa default
Name:                default
Namespace:           default
Labels:              <none>
Annotations:         <none>
Image pull secrets:  <none>
Mountable secrets:   default-token-9srgx
Tokens:              default-token-9srgx
Events:              <none>

root@master:~/cks/RBAC# k create sa accessor
serviceaccount/accessor created
root@master:~/cks/RBAC# k get sa,secrets
NAME                      SECRETS   AGE
serviceaccount/accessor   1         5s
serviceaccount/default    1         17m

NAME                          TYPE                                  DATA   AGE
secret/accessor-token-bnd4s   kubernetes.io/service-account-token   3      5s
secret/default-token-9srgx    kubernetes.io/service-account-token   3      17m



root@master:~/cks/RBAC# k describe secret accessor-token-bnd4s
Name:         accessor-token-bnd4s
Namespace:    default
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: accessor
              kubernetes.io/service-account.uid: 9e763e70-71da-431a-a813-df838420341b

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1066 bytes
namespace:  7 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IkJhb1NtQ21TRlpKWHBYbUV3VHZ6OW9FOFZoOV9BSlNrLUN1WEJ4SjZtc1EifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImFjY2Vzc29yLXRva2VuLWJuZDRzIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFjY2Vzc29yIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiOWU3NjNlNzAtNzFkYS00MzFhLWE4MTMtZGY4Mzg0MjAzNDFiIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50OmRlZmF1bHQ6YWNjZXNzb3IifQ.lAy_-h3rMcZSNHtwm2THelbj-2O635N75Hx92-4t9Ulaplk0WOg9Ja72LlReasU39VS1DMAwYfgNgsyurDme2HVolO4IEeyl56BrOgKC73LWLQ1d6waNqPVzU_GRKuzXqpDXJID3CODcuNBOld1VHyIbmK2YNzgPMaR0CLexpx_p_wU5mg_XZpfccL4KvFBNmWh_cj3eFz4t1yxsP2TycwC2WKkXMvpaVqY_YFFpge2ddTwBf-xgtcpoRAQpfEkxZSVWqA12ZTi0I2wdK--XMcJcqmTTor1rcAws_aLUxT7VajL4sgd4LT_OuJk4iQdLmQZzwDYS4-Ca354pNIK0PA



root@master:~/cks/RBAC# k run accessor --image=nginx --dry-run=client -oyaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: accessor
  name: accessor
spec:
  containers:
  - image: nginx
    name: accessor
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
root@master:~/cks/RBAC# k run accessor --image=nginx --dry-run=client -oyaml > accessor.yaml


root@master:~/cks/RBAC# vim accessor.yaml
root@master:~/cks/RBAC# cat accessor.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: accessor
  name: accessor
spec:
  serviceAccountName: accessor  #添加此行
  containers:
  - image: nginx
    name: accessor
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
root@master:~/cks/RBAC# k create -f accessor.yaml
pod/accessor created



root@master:~/cks/RBAC# k exec -ti accessor -- bash
root@accessor:/# mount |grep sec
tmpfs on /run/secrets/kubernetes.io/serviceaccount type tmpfs (ro,relatime)
root@accessor:/# cd /run/secrets/kubernetes.io/serviceaccount
root@accessor:/run/secrets/kubernetes.io/serviceaccount# cat token 
eyJhbGciOiJSUzI1NiIsImtpZCI6IkJhb1NtQ21TRlpKWHBYbUV3VHZ6OW9FOFZoOV9BSlNrLUN1WEJ4SjZtc1EifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImFjY2Vzc29yLXRva2VuLWJuZDRzIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFjY2Vzc29yIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiOWU3NjNlNzAtNzFkYS00MzFhLWE4MTMtZGY4Mzg0MjAzNDFiIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50OmRlZmF1bHQ6YWNjZXNzb3IifQ.lAy_-h3rMcZSNHtwm2THelbj-2O635N75Hx92-4t9Ulaplk0WOg9Ja72LlReasU39VS1DMAwYfgNgsyurDme2HVolO4IEeyl56BrOgKC73LWLQ1d6waNqPVzU_GRKuzXqpDXJID3CODcuNBOld1VHyIbmK2YNzgPMaR0CLexpx_p_wU5mg_XZpfccL4KvFBNmWh_cj3eFz4t1yxsP2TycwC2WKkXMvpaVqY_YFFpge2ddTwBf-xgtcpoRAQpfEkxZSVWqA12ZTi0I2wdK--XMcJcqmTTor1rcAws_aLUxT7VajL4sgd4LT_OuJk4iQdLmQZzwDYS4-Ca354pNIK0PA



tes.io/serviceaccount# curl https://kubernetes
curl: (60) SSL certificate problem: unable to get local issuer certificate
More details here: https://curl.haxx.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.
root@accessor:/run/secrets/kubernetes.io/serviceaccount# curl https://kubernetes -k
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {
    
  },
  "status": "Failure",
  "message": "forbidden: User \"system:anonymous\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {
    
  },
  "code": 403
}

#以serviceaccount用户accessor访问
root@accessor:/run/secrets/kubernetes.io/serviceaccount# curl https://kubernetes -k -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtZCI6IkJhb1NtQ21TRlpKWHBYbUV3VHZ6OW9FOFZoOV9BSlNrLUN1WEJ4SjZtc1EifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImFjY2Vzc29yLXRva2VuLWJuZDRzIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFjY2Vzc29yIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiOWU3NjNlNzAtNzFkYS00MzFhLWE4MTMtZGY4Mzg0MjAzNDFiIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50OmRlZmF1bHQ6YWNjZXNzb3IifQ.lAy_-h3rMcZSNHtwm2THelbj-2O635N75Hx92-4t9Ulaplk0WOg9Ja72LlReasU39VS1DMAwYfgNgsyurDme2HVolO4IEeyl56BrOgKC73LWLQ1d6waNqPVzU_GRKuzXqpDXJID3CODcuNBOld1VHyIbmK2YNzgPMaR0CLexpx_p_wU5mg_XZpfccL4KvFBNmWh_cj3eFz4t1yxsP2TycwC2WKkXMvpaVqY_YFFpge2ddTwBf-xgtcpoRAQpfEkxZSVWqA12ZTi0I2wdK--XMcJcqmTTor1rcAws_aLUxT7VajL4sgd4LT_OuJk4iQdLmQZzwDYS4-Ca354pNIK0PA"
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {
    
  },
  "status": "Failure",
  "message": "forbidden: User \"system:serviceaccount:default:accessor\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {
    
  },
  "code": 403





```










#   disable  automount 

```
root@master:~/cks/serviceaccount# vim accessor.yaml 
root@master:~/cks/serviceaccount# cat accessor.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: accessor
  name: accessor
spec:
  serviceAccountName: accessor
  automountServiceAccountToken: false   #添加此行
  containers:
  - image: nginx
    name: accessor
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}


root@master:~/cks/serviceaccount# k -f accessor.yaml replace --force
pod "accessor" deleted
pod/accessor replaced
root@master:~/cks/serviceaccount# k get pods
NAME       READY   STATUS    RESTARTS   AGE
accessor   1/1     Running   0          13s
root@master:~/cks/serviceaccount# k exec -ti accessor -- bash
root@accessor:/# mount |grep ser



root@master:~/cks/serviceaccount# vim accessor.yaml 
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: accessor
  name: accessor
spec:
  serviceAccountName: accessor
  automountServiceAccountToken: true  #false改为true
  containers:
  - image: nginx
    name: accessor
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

#pod文件含挂载的token
root@master:~/cks/serviceaccount# k edit pod accessor

.....
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: accessor-token-bnd4s
      readOnly: true
.....
  volumes:
  - name: accessor-token-bnd4s
    secret:
      defaultMode: 420
      secretName: accessor-token-bnd4s

.....

```



