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



#   k  explain PodSecurityPolicy.spec --recursive

```
k explain PodSecurityPolicy.spec --recursive
```


```
KIND:     PodSecurityPolicy
VERSION:  policy/v1beta1

RESOURCE: spec <Object>

DESCRIPTION:
     spec defines the policy enforced.

     PodSecurityPolicySpec defines the policy enforced.

FIELDS:
   allowPrivilegeEscalation	<boolean>
   allowedCSIDrivers	<[]Object>
      name	<string>
   allowedCapabilities	<[]string>
   allowedFlexVolumes	<[]Object>
      driver	<string>
   allowedHostPaths	<[]Object>
      pathPrefix	<string>
      readOnly	<boolean>
   allowedProcMountTypes	<[]string>
   allowedUnsafeSysctls	<[]string>
   defaultAddCapabilities	<[]string>
   defaultAllowPrivilegeEscalation	<boolean>
   forbiddenSysctls	<[]string>
   fsGroup	<Object>
      ranges	<[]Object>
         max	<integer>
         min	<integer>
      rule	<string>
   hostIPC	<boolean>
   hostNetwork	<boolean>
   hostPID	<boolean>
   hostPorts	<[]Object>
      max	<integer>
      min	<integer>
   privileged	<boolean>
   readOnlyRootFilesystem	<boolean>
   requiredDropCapabilities	<[]string>
   runAsGroup	<Object>
      ranges	<[]Object>
         max	<integer>
         min	<integer>
      rule	<string>
   runAsUser	<Object>
      ranges	<[]Object>
         max	<integer>
         min	<integer>
      rule	<string>
   runtimeClass	<Object>
      allowedRuntimeClassNames	<[]string>
      defaultRuntimeClassName	<string>
   seLinux	<Object>
      rule	<string>
      seLinuxOptions	<Object>
         level	<string>
         role	<string>
         type	<string>
         user	<string>
   supplementalGroups	<Object>
      ranges	<[]Object>
         max	<integer>
         min	<integer>
      rule	<string>
   volumes	<[]string>
   
 ```
