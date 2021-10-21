#  node metadata 



```
k run nginx --image=nginx

k get pods

k exec -ti nginx bash

curl "http://metadata.google.internal/computeMetadata/v1/instance/disks/" -H "Metadata-Flavor: Google"


```


#  Protect Node Metadata via NetworkPolicy


deny.yaml    

accept.yaml

