#  falco


## install


https://falco.org/docs/getting-started/installation/



## use


```
cat /var/log/syslog | grep falco | grep nginx | grep process

crictl ps -id 7a5ea6a080d1

crictl pods -id 7a864406b9794

```



 vim falco_rules.yaml
 
 
 ```
 chnage rules  here
 
 ```




---


#  crictl 

##   isntall

```
wget https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.22.0/crictl-v1.22.0-linux-amd64.tar.gz
tar -zxvf crictl-v1.22.0-linux-amd64.tar.gz
install  crictl /usr/local/bin 

```

