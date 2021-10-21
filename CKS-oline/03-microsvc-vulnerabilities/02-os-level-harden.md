#  os level 

```

root@master:~/cks/securitytext# k run pod --image=busybox --command -oyaml --dry-run=client > pod.yaml -- sh -c 'sleep 1d'
root@master:~/cks/securitytext# cat pod.yaml 
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod
  name: pod
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
  containers:
  - command:
    - sh
    - -c
    - sleep 1d
    image: busybox
    name: pod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}


root@master:~/cks/securitytext# k create -f pod.yaml 
root@master:~/cks/security# k get pods
NAME   READY   STATUS    RESTARTS   AGE
pod    1/1     Running   0          83s


root@master:~/cks/securitytext# k exec -ti pod -- sh
/ $ id
uid=1000 gid=3000

/ $ touch test
touch: test: Permission denied
/ $ cd /tmp
/tmp $ touch test
/tmp $ ls -lh
total 0      
-rw-r--r--    1 1000     3000           0 May 15 15:00 test





```
