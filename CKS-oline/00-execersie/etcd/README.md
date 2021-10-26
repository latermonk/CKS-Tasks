#  etcd

```

ETCDCTL_API=3 etcdctl --cert=/etc/kubernetes/pki/apiserver-etcd-client.crt --key=/etc/kubernetes/pki/apiserver-etcd-client.key     --cacert=/etc/kubernetes/pki/etcd/ca.crt      endpoint health 

```


拆解开来：
```

ETCDCTL_API=3 etcdctl \
--cert /etc/kubernetes/pki/apiserver-etcd-client.crt \
--key /etc/kubernetes/pki/apiserver-etcd-client.key \
--cacert /etc/kubernetes/pki/etcd/ca.crt  \

get /registry/secrets/default/secret-name

```





所有secret都加密
```
k get secrets -A -oyaml |k replace -f  -

k get secret -A -oyaml | kubectl replace -f -

```
