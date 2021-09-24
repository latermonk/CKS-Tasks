#  RBAC


```
k -n kubernetes-dashboard get sa

```


```
 k get clusterroles  |grep view
```


```
 k -n kubernets-dashboard create rolebinding insecure --serviceaccount kubernetes-dashboard:kubernetes-dashboard --clusterrole view -oyaml --dry-run=client
```



#  创建 rolebinding 

```
k -n kubernetes-dashboard create rolebinding insecure --serviceaccount kubernetes-dashboard:kubernetes-dashboard --clusterrole view
```

#  创建 clusterrolebinding 

```
 k -n kubernetes-dashboard create clusterrolebinding insecure --serviceaccount kubernetes-dashboard:kubernetes-dashboard --clusterrole view

```

