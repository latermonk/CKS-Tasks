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


一条规则：
```
- rule: Outbound or Inbound Traffic not to Authorized Server Process and Port
  desc: Detect traffic that is not to authorized server process and port.
  condition: >
    allowed_port and
    inbound_outbound and
    container and
    container.image.repository in (allowed_image) and
    not proc.name in (authorized_server_binary) and
    not fd.sport in (authorized_server_port)    
  output: >
    Network connection outside authorized port and binary
    (command=%proc.cmdline connection=%fd.name user=%user.name user_loginuid=%user.loginuid container_id=%container.id
    image=%container.image.repository)    
  priority: WARNING
  tags: [network]
  
  
  ```

修改规则：

```
grep -r "A shell was spawned in a container with an attached terminal" *

```





---


#  crictl 

##   isntall

```
wget https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.22.0/crictl-v1.22.0-linux-amd64.tar.gz
tar -zxvf crictl-v1.22.0-linux-amd64.tar.gz
install  crictl /usr/local/bin 

```




---


#  

```


 crictl ps -id b1339d5cc2de
 
 crictl pods -id 595af943c3245
 
```


