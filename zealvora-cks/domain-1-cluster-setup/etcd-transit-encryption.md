
#### Step 1 - Creating the ETCD Certificate Key:
```sh
cd /root/certificates
openssl genrsa -out etcd.key 2048
```
#### Step 2 - Creating Configuration for ETCD CSR:
```sh
cat > etcd.cnf <<EOF
[req]
req_extensions = v3_req
distinguished_name = req_distinguished_name
[req_distinguished_name]
[ v3_req ]
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names
[alt_names]
IP.1 = [IP-1]
IP.2 = 127.0.0.1
EOF
```
#### Step 3 - Creating CSR:
```sh
openssl req -new -key etcd.key -subj "/CN=etcd" -out etcd.csr -config etcd.cnf
```
#### Step 4 - Signing CSR with Certificate Authority Certificate:
```sh
openssl x509 -req -in etcd.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out etcd.crt -extensions v3_req -extfile etcd.cnf -days 1000
```

#### Step 5 - Start ETCD Server with Certificates:
```sh
etcd --cert-file=/root/certificates/etcd.crt --key-file=/root/certificates/etcd.key --advertise-client-urls=https://127.0.0.1:2379 --listen-client-urls=https://127.0.0.1:2379
```
#### Step 6 - Verification:
```sh
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --insecure-skip-tls-verify  --insecure-transport=false put course "kplabs is awesome"

ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --insecure-skip-tls-verify  --insecure-transport=false get course
```

#### Additional Optional Step:
```sh
yum -y install tcpdump
tcpdump -i lo -X  port 2379
```
