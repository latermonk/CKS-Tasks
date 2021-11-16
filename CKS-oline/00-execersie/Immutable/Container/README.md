#  Immutable  Container

https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: imm
  name: imm
spec:
  containers:
  - image: httpd
    name: imm
    resources: {}
    startupProbe:     # Add from here
      exec:
        command:
        - rm
        - /bin/bash
      initialDelaySeconds: 1
      periodSeconds: 5          # the end
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```