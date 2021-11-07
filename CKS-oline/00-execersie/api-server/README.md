#  API-server 


https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/


##  manual request

1. 提取
2. 请求
```
curl https://127.0.0.1:54278  --cacert ca --cert cert --key key

```




#  查看apiserver.crt

```
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text
```



#   NodeRestriction

```
--enable-admission-plugins=NodeRestriction

```

# PodSecurityPolicy 
https://kubernetes.io/docs/concepts/policy/pod-security-policy/


