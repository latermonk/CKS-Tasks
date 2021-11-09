#  AppArmor

https://kubernetes.io/docs/tutorials/clusters/apparmor/  



加载rule的方法
1.
```
apparmor_parser -q ./profile


```

2. 把规则放到目录下 重启进程



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
