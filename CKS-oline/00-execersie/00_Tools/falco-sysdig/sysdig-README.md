#  sysdig


# INSTALl

```
curl -s https://s3.amazonaws.com/download.draios.com/DRAIOS-GPG-KEY.public | sudo apt-key add -  
curl -s -o /etc/apt/sources.list.d/draios.list https://s3.amazonaws.com/download.draios.com/stable/deb/draios.list  
apt-get update


```



```
apt-get -y install linux-headers-$(uname -r)


```



```
apt-get -y install sysdig


```




#  use

```

sysdig -l 

sysdia -M 30 -p "*%evt.time"
```


