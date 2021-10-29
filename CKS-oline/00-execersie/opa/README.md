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
 template.yaml
 
 ```
 
 
 
 再次创建,这次是ok的 
 
 ```
 k run pod --image=nginx
 
 ```
 
 
 


#  03

