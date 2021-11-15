#  seccomp

https://kubernetes.io/zh/docs/tutorials/clusters/seccomp/  




seccomp 可以在用户代码中增加一些模块来进行系统调用的拦截 ， 从而达到保护系统的目的     

#  for  docker 

```
docker run --security-opt seccomp=default.json nginx

```
删除 syscall中的 write 调用

```
docker run --security-opt seccomp=default-without-write.json  nginx  
```

导致无法创建NGINX docker





#  for k8s


```
mkdir /var/lib/kubelet/seccomp

cat /var/lib/kubelet/seccomp/default.json 
```


```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: secure
  name: secure
spec:
  securityContext:                     # spec添加此模块儿
    seccompProfile:
      type: Localhost
      localhostProfile: default.json
  containers:
  - image: nginx
    name: secure
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
