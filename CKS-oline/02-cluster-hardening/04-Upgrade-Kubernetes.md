# Upgrade Kubernetes


```
root@master:~/cks/create_cluster# k drain   master --ignore-daemonsets
node/master cordoned
WARNING: ignoring DaemonSet-managed Pods: kube-system/calico-node-ngbm8, kube-system/kube-proxy-lfkn9
node/master drained
root@master:~/cks/create_cluster# k get node
NAME     STATUS                     ROLES                  AGE   VERSION
master   Ready,SchedulingDisabled   control-plane,master   42h   v1.20.1
node1    Ready                      <none>                 42h   v1.20.1
node2    Ready                      <none>                 42h   v1.20.1

root@master:~/cks/create_cluster# apt-cache show kubeadm  |grep -e '1.20'
Version: 1.20.2-00
Filename: pool/kubeadm_1.20.2-00_amd64_38fa4593055ef1161d4cb322437eedda95186fb850819421b8cf75f3a943dc51.deb
Version: 1.20.1-00
Filename: pool/kubeadm_1.20.1-00_amd64_7cd8d4021bb251862b755ed9c240091a532b89e6c796d58c3fdea7c9a72b878f.deb
Version: 1.20.0-00
Filename: pool/kubeadm_1.20.0-00_amd64_18afc5e3855cf5aaef0dbdfd1b3304f9e8e571b3c4e43b5dc97c439d62a3321a.deb


root@master:~/cks/create_cluster# apt-get install kubeadm=1.20.2-00 kubectl=1.20.2-00 kubelet=1.20.2-00
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following package was automatically installed and is no longer required:
  rename
Use 'sudo apt autoremove' to remove it.
The following packages will be upgraded:
  kubeadm kubectl kubelet
3 upgraded, 0 newly installed, 0 to remove and 146 not upgraded.
Need to get 34.5 MB of archives.
After this operation, 32.8 kB of additional disk space will be used.
Get:1 https://mirrors.aliyun.com/kubernetes/apt kubernetes-xenial/main amd64 kubelet amd64 1.20.2-00 [18.9 MB]
Get:2 https://mirrors.aliyun.com/kubernetes/apt kubernetes-xenial/main amd64 kubectl amd64 1.20.2-00 [7,940 kB]
Get:3 https://mirrors.aliyun.com/kubernetes/apt kubernetes-xenial/main amd64 kubeadm amd64 1.20.2-00 [7,706 kB]
Fetched 34.5 MB in 7s (4,914 kB/s)                                                                                                   
(Reading database ... 110090 files and directories currently installed.)
Preparing to unpack .../kubelet_1.20.2-00_amd64.deb ...
Unpacking kubelet (1.20.2-00) over (1.20.1-00) ...
Preparing to unpack .../kubectl_1.20.2-00_amd64.deb ...
Unpacking kubectl (1.20.2-00) over (1.20.0-00) ...
Preparing to unpack .../kubeadm_1.20.2-00_amd64.deb ...
Unpacking kubeadm (1.20.2-00) over (1.20.0-00) ...
Setting up kubelet (1.20.2-00) ...
Setting up kubectl (1.20.2-00) ...
Setting up kubeadm (1.20.2-00) ...



root@master:~/cks/create_cluster# kubeadm upgrade plan
[upgrade/config] Making sure the configuration is correct:
[upgrade/config] Reading configuration from the cluster...
[upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[preflight] Running pre-flight checks.
[upgrade] Running cluster health checks
[upgrade] Fetching available versions to upgrade to
[upgrade/versions] Cluster version: v1.20.0
[upgrade/versions] kubeadm version: v1.20.2
I0426 23:07:55.970280   68360 version.go:251] remote version is much newer: v1.21.0; falling back to: stable-1.20
[upgrade/versions] Latest stable version: v1.20.6
[upgrade/versions] Latest stable version: v1.20.6
[upgrade/versions] Latest version in the v1.20 series: v1.20.6
[upgrade/versions] Latest version in the v1.20 series: v1.20.6

Components that must be upgraded manually after you have upgraded the control plane with 'kubeadm upgrade apply':
COMPONENT   CURRENT       AVAILABLE
kubelet     2 x v1.20.1   v1.20.6
            1 x v1.20.2   v1.20.6

Upgrade to the latest version in the v1.20 series:

COMPONENT                 CURRENT    AVAILABLE
kube-apiserver            v1.20.0    v1.20.6
kube-controller-manager   v1.20.0    v1.20.6
kube-scheduler            v1.20.0    v1.20.6
kube-proxy                v1.20.0    v1.20.6
CoreDNS                   1.7.0      1.7.0
etcd                      3.4.13-0   3.4.13-0

You can now apply the upgrade by executing the following command:

	kubeadm upgrade apply v1.20.6

Note: Before you can perform this upgrade, you have to update kubeadm to v1.20.6.

_____________________________________________________________________


The table below shows the current state of component configs as understood by this version of kubeadm.
Configs that have a "yes" mark in the "MANUAL UPGRADE REQUIRED" column require manual config upgrade or
resetting to kubeadm defaults before a successful upgrade can be performed. The version to manually
upgrade to is denoted in the "PREFERRED VERSION" column.

API GROUP                 CURRENT VERSION   PREFERRED VERSION   MANUAL UPGRADE REQUIRED
kubeproxy.config.k8s.io   v1alpha1          v1alpha1            no
kubelet.config.k8s.io     v1beta1           v1beta1             no
_____________________________________________________________________


root@master:~/cks/create_cluster# kubeadm upgrade apply v1.20.6
root@master:~/cks/create_cluster# k get nodes
NAME     STATUS                     ROLES                  AGE   VERSION
master   Ready,SchedulingDisabled   control-plane,master   42h   v1.20.2
node1    Ready                      <none>                 42h   v1.20.1
node2    Ready                      <none>                 42h   v1.20.1


root@master:~/cks/create_cluster# k uncordon master
node/master uncordoned
root@master:~/cks/create_cluster# k get node
NAME     STATUS   ROLES                  AGE   VERSION
master   Ready    control-plane,master   42h   v1.20.2
node1    Ready    <none>                 42h   v1.20.1
node2    Ready    <none>                 42h   v1.20.1




```

