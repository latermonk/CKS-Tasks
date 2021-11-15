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


安装了 utils 之后可以用的两条命令：

* aa-genprof   生成配置文件    
* aa-logprof   修改配置文件   
 
https://documentation.suse.com/zh-cn/sles/15-SP2/html/SLES-all/cha-apparmor-commandline.html#sec-apparmor-commandline-profiling-summary-genprof  


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
 
 
 
 #  解释
 
 
 ```
 
 
 container.apparmor.security.beta.kubernetes.io/c1: localhost/very-secure
 
 
 c1  是容器名字
 
 localhost/very-secure  是apparmor_profile name
 
 ```




#   deploument


```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: apparmor
  name: apparmor
  annotations:
    container.apparmor.security.beta.kubernetes.io/c1: localhost/ping
spec:
  replicas: 1
  selector:
    matchLabels:
      app: apparmor
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: apparmor
    spec:
      nodeSelector:                    #  add here
        security: apparmor 
      containers:
      - image: nginx:1.19.2
        name: c1                        #  container -
        resources: {}
status: {}
```
