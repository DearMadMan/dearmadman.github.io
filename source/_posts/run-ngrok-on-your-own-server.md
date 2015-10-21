title: run ngrok on your own server
date: 2015-09-19 16:28:48
categories: linux
tags:
---

![](/images/s19.jpg)

# run ngrok on your own server

## dependencies
* git >= 1.7.9.5
* golang
* mercurial
* code.google.com
## install golang

```
yum install golang
```

## update git 

```
 rpm --import http://apt.sw.be/RPM-GPG-KEY.dag.txt  
 # http://pkgs.repoforge.org/rpmforge-release/ 
 # Select the appropriate with your system
 rpm -i http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.i686.rpm  
 yum --enablerepo=rpmforge-extras update  
 yum --enablerepo=rpmforge-extras install git

```

## install mercurial
```
yum install mercurial

```
## install ngrok
```
cd /usr/local/src/
git clone https://github.com/inconshreveable/ngrok.git
export GOPATH=/usr/local/src/ngrok/
export NGROK_DOMAIN="dearmadman.com"
```
## generater ssl certificate
```
cd ngrok
openssl genrsa -out rootCA.key 2048
openssl req -x509 -new -nodes -key rootCA.key -subj "/CN=$NGROK_DOMAIN" -days 5000 -out rootCA.pem
openssl genrsa -out device.key 2048
openssl req -new -key device.key -subj "/CN=$NGROK_DOMAIN" -out device.csr
openssl x509 -req -in device.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out device.crt -days 5000
cp rootCA.pem assets/client/tls/ngrokroot.crt
cp device.crt assets/server/tls/snakeoil.crt 
cp device.key assets/server/tls/snakeoil.key
GOOS=linux GOARCH=386
make clean
make release-server release-client
```

## run server
```
bin/ngrokd -domain="$NGROK_DOMAIN" -httpAddr=":8000"
```
## need 
```
#copy your ngork client to your client system

scp root@xxx.xxx.xxx.xxx /usr/local/src/ngrok/bin/ngrok  /usr/bin/

```

## client 
```
server_addr: "dearmadman.com:4443"
trust_host_root_certs: false

```