### Documentation:

https://github.com/draios/sysdig/wiki/Sysdig-User-Guide

#### Install sysdig:
```sh
apt install sysdig
```
#### Commands Used:
```sh
sysdig
sysdig proc.name=nano
sysdig proc.name=cat or proc.name=nano
```
```sh
sysdig -l
sysdig proc.name=cat and container.name!=host
```
```sh
csysdig
```
#### Sysdig Chisels:
```sh
sysdig -cl
sysdig -c topprocs_cpu
sysdig -c spectrogram
sysdig -c spy_users
sysdig -c ps
```
