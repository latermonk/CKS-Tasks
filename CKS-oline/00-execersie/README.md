
```
k api-resources|grep role
```


```
k -n prod create  role  test  --verb=get  --resource=secrets


 k -n prod describe role test

```


