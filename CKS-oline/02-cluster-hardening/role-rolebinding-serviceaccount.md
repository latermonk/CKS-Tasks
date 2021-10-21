# Role and Rolebinding


```



root@master:~/k8slib# k create ns red
namespace/red created
root@master:~/k8slib# k create ns blue
namespace/blue created


root@master:~/k8slib# k -n red create role secret-manager --verb=get --resource=secrets -oyaml --dry-run=client
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  creationTimestamp: null
  name: secret-manager
  namespace: red
rules:
- apiGroups:
  - ""
  resources:
  - secrets
  verbs:
  - get


root@master:~/k8slib# k -n red create role secret-manager --verb=get --resource=secrets 
role.rbac.authorization.k8s.io/secret-manager created

root@master:~/k8slib# k -n red create rolebinding secret-manager --role=secret-manager --user=jane -oyaml --dry-run=client
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  creationTimestamp: null
  name: secret-manager
  namespace: red
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: secret-manager
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: jane
root@master:~/k8slib# k -n red create rolebinding secret-manager --role=secret-manager --user=jane
rolebinding.rbac.authorization.k8s.io/secret-manager created



root@master:~/k8slib# k -n blue create role secret-manager --verb=get --verb=list --resource=secrets 
role.rbac.authorization.k8s.io/secret-manager created
root@master:~/k8slib# k -n blue create rolebinding secret-manager --role=secret-manager --user=jane
rolebinding.rbac.authorization.k8s.io/secret-manager created


#测试
root@master:~/k8slib# k -n red auth can-i get secrets --as jane
yes
root@master:~/k8slib# k -n red auth can-i get secrets --as tom
no
root@master:~/k8slib# k -n red auth can-i delete secrets --as jane
no
root@master:~/k8slib# k -n red auth can-i list secrets --as jane
no
root@master:~/k8slib# k -n blue auth can-i list secrets --as jane
yes
root@master:~/k8slib# k -n blue auth can-i get secrets --as jane
yes
root@master:~/k8slib# k -n blue auth can-i get pods --as jane
no


```




