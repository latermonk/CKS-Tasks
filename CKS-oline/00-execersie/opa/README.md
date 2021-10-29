#  01  Install opa

```
k create -f gatekeeper.yaml 
```


```
k get ns

k get pod,svc -n gatekeeper-system


```


#  02 Deny All Policy

```
k get crd 

k get constrainttemplates

```

```
k apply -f  template.yaml 

k get k8salwaysdeny
k get constrainttemplates

```



```
k apply -f constraint.yaml

k get k8sAlwaysDeny

```


创建pod测试
```
k run pod --image=nginx
```



vim constraint.yaml
```
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sAlwaysDeny
metadata:
  name: pod-always-deny
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Pod"]
  parameters:
    message: "NO WAY!"   #修改此行
```


```
k apply -f constraint.yaml
```

再次创建pod ,提示信息已经改变
```
k run pod --image=nginx

```


vim cat template.yaml  添加一行
```
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8salwaysdeny
spec:
  crd:
    spec:
      names:
        kind: K8sAlwaysDeny
      validation:
        # Schema for the `parameters` field
        openAPIV3Schema:
          properties:
            message:
              type: string
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8salwaysdeny

        violation[{"msg": msg}] {
          1 > 0
          1 > 2   #添加此行
          msg := input.parameters.message
        }
        
 ```
 
 
 ```
 k apply -f template.yaml
 
 ```
 
 
 
 再次创建,这次是ok的 
 
 ```
 k run pod --image=nginx
 
 ```
 
 
 
 # 03 Enforce Namespace Labels
 
 ```
 k apply -f template_label.yaml
 
 k apply -f all_ns_must_have_cks.yaml
 
 ```
 
 
 ```
 k describe K8sRequiredLabels ns-must-have-cks
 ```
 
 
 编辑default ns 
 
```
 k  edit ns default
```
 
 
 ```
 apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: "2021-05-14T08:37:43Z"
  labels:   # 添加此行
    cks: amazing   # 添加此行
  name: default
  resourceVersion: "86550"
  uid: 773025bb-2f60-40f9-a10d-924c21e45016
spec:
  finalizers:
  - kubernetes
status:
  phase: Active
 ```


```
k describe K8sRequiredLabels ns-must-have-cks

```


创建ns  失败

```
k create ns test

```



修改ns yaml文件

```
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: ns-must-have-cks
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Namespace"]
  parameters:
    labels: ["cks","team"]  #修改此行
    
```


再次创建
```
k apply -f all_ns_must_have_cks.yaml 

```


再次测试
```
k create ns test
```

```
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: null
  name: test
spec: {}
status: {}
root@master:~/cks/opa# k create ns test -o yaml --dry-run=client > ns.yaml

oot@master:~/cks/opa# vim ns.yaml 
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: null
  name: test
  labels:    #添加此行
    cks: amazing  #添加此行
    team: killer-sh  #添加此行
spec: {}
status: {}

```

```
k create -f ns.yaml 


```







#  04 Enforce Deployment replica count


 ```
 k apply -f template_replicas.yaml 
 
 k apply -f all_deployment_must_have_min_replicacount.yaml 
 
 
 ```
 
 ```
 k describe k8sminreplicacount deployment-must-have-min-replicas
 
 ```
 
 
 测试  :  无法创建
 ```
 k create deploy test --image=nginx
 ```
 
  
 测试2  :  加上replicas参数就可以创建
 ```
 k create deploy test --image=nginx --replicas=2
 ```
 
 
 ---
 END 
 
