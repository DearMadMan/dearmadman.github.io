title: "php file_get_contents can't get https url"
date: 2015-10-15 17:12:43
categories: php
tags: php
---

![](/images/s29.jpg)

# support file_get_contents for hhtps url

## install openssl
```
 yum install openssl
```
## get cacert.pem

```
curl -o cacert.pem http://curl.haxx.se/ca/cacert.pem

```


## config your php.ini

```
#add 
openssl.cafile=/home/www/cacert.pem

```

## reload the server

```
service php-fpm reload
```