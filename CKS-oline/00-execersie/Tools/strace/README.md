#  strace

```
Using crictl pods we first searched for the Pods of Deployment collector1, which has two replicas
We then took one pod-id to find it's containers using crictl ps
And finally we used crictl inspect to find the process name, which is collector1-process

```





```


strace   ls 




strace  -cw ls /
```



```
strace cat test

strace -cw cat test

```

#   strace  etcd

通过 /proc 找到 etcd 的db文件 可以查看 etcd内部的明文


```
ps -ef |grep etcd   找到进程ID

cd /proc/process-id-you-just-find

cd /proc/process-id-you-just-find/fd

ls -la

/proc/process-id-you-just-find   


tail   fd 



```


测试：

```

k create secret generic credit-card --from-literal cc=111222333444 

cat    fd  | strings | grep  111222333444
```


测试2：

```
k create secret generic hello --from-literal=kugou=947

cat 10 | strings |grep kugou -A20 -B20    #  一定要注意  -A  -B 参数的使用


```




#  strace  proc   env


```
cat  /proc/912111/environ
```


