#  vgrant up 2 machine

## Install Controlplane using Kubeadm





```
>  sudo -i
bash <(curl -s https://raw.githubusercontent.com/latermonk/CKS-Tasks/master/CKS-Exam-Series-By-Kim/01-Create-Cluster-Best-Practices/install_controlplane.sh) > 
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
**Print the command again if ：**

```
kubeadm token create -—print-join-command —-ttl 0
```




-----------------

https://blog.csdn.net/wangmiaoyan/article/details/101216496   

https://blog.csdn.net/z_asdf/article/details/105491168?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control     

