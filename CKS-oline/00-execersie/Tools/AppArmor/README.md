#  AppArmor

https://kubernetes.io/docs/tutorials/clusters/apparmor/  



加载rule的方法

```
apparmor_parser -q ./profile


```


删除 rule 

```
  apparmor_parser  -R ./apparmor-profile-you-want-to-remove
```



#  安装工具集

```
apt install apparmor-utils -y
```


#  产生 curl 
```
aa-genprof curl


```


check
```
aa-status |grep curl

cd  /etc/apparmor.d


cat usr.bin.curl
```


```
# Last Modified: Tue Nov  9 10:32:52 2021
#include <tunables/global>

/usr/bin/curl {
  #include <abstractions/base>

  /usr/bin/curl mr,

}

```


```
aa-logprof
```


---


#   docker 

```
apparmor_parse docker-nginx
```

```
docker run --security-opt apparmor=docker-nginx  -d gninx    
```


exec 进去发现诸多限制



#  k8s

```
annotations:
    container.apparmor.security.beta.kubernetes.io/secure : localhost/docker-nginx 

```




pod

 ```
 kind: Pod
metadata:
  creationTimestamp: null
  annotations:    #添加此行
    container.apparmor.security.beta.kubernetes.io/secure: localhost/hello  #添加此行   不行
    container.apparmor.security.beta.kubernetes.io/secure: localhost/hello  #添加此行    可以
  labels:
    run: secure
  name: secure
spec:
  containers:
  - image: nginx
    name: secure
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

 ```