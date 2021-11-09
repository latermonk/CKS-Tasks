#  Audit Log Policy 



做一个 Auditlog 的实验



#  创建 dir 

```
mkdir  /etc/kubernetes/audit/logs

```

#  创建 policy文件

```
# Log all requests at the Metadata level.
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
- level: Metadata
```


#  修改 api config

```

    - --audit-policy-file=/etc/kubernetes/audit/policy.yaml       # add
    - --audit-log-path=/etc/kubernetes/audit/logs/audit.log       # add
    - --audit-log-maxsize=500                                     # add
    - --audit-log-maxbackup=5  
    
    
    
        - mountPath: /etc/kubernetes/audit      # add
      name: audit      
      
      
        - hostPath:                               # add
      path: /etc/kubernetes/audit           # add
      type: DirectoryOrCreate               # add
    name: audit   
```
