
过滤的时候一定要注意大小写！！！

```
trivy image docker.io/fanux/lvscare:latest |grep -E "HIGH|CRITICAL"

```





#  trivy


https://aquasecurity.github.io/trivy/v0.21.0/getting-started/installation/    




**查看目前POD的image是什么，并扫描**
```
k get po -A -oyaml |grep image:
```
