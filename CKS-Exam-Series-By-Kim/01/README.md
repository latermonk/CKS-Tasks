#  vgrant up 2 machine

## Install Controlplane using Kubeadm


```
> sudo -i
> bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-challenge-series/master/cluster-setup/latest/install_controlplane.sh) 
exit
sudo -i # login again to get nicer bash with autocompletion
```

## Install Node using Kubeadm

```
> sudo -i
> bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-challenge-series/master/cluster-setup/latest/install_node.sh)
exit
sudo -i # login again to get nicer bash with autocompletion
```
**如果沒有再次打印：**

```
kubeadm token create -—print-join-command —-ttl 0
```

