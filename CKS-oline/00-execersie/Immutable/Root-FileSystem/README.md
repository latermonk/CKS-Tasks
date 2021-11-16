#   Immutable Root FileSystem

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    securityContext: 
      readOnlyRootFilesystem: true
    name: nginx
    resources: {}
    volumeMounts:
    - name: tmp
      mountPath: /tmp
  volumes:
    - name: tmp
      emptyDir: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}

```



# error


```
2021/11/16 13:14:17 [emerg] 1#1: mkdir() "/var/cache/nginx/client_temp" failed (30: Read-only file system)
nginx: [emerg] mkdir() "/var/cache/nginx/client_temp" failed (30: Read-only file system)

```
