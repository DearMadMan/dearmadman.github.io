title: lnmp for ubuntu
date: 2015-08-31 10:38:33
categories: [linux,ubuntu]
tags: [lnmp,ubuntu,linux]
---
![](/images/s06.jpg)
## nginx

```bash
wget -c http://nginx.org/download/nginx-1.9.3.tar.gz
tar -zxvf nginx-1.9.3.tar.gz
cd nginx-1.9.3
./configure --prefix=/usr/local/web/nginx
make 
sudo make install

```

### 配置
```
sudo vi /usr/local/web/nginx/conf/nginx.conf     #进行配置修改

```
### 设置根目录
```
server {
       listen       80;
       server_name  localhost;

       #charset koi8-r;

       #access_log  logs/host.access.log  main;

       location / {
           root   /vagrant;   #网站根目录
           index  index.html index.htm;
       }
```

### 启动
```
/usr/local/web/nginx/sbin/nginx
```

### 查看 进程
```
ps -ef | grep nginx
```
如果发现 `nginx: master` 表明 `nginx` 已经启动

## mysql

### 安装
```
sudo apt-get install mysql-server
sudo apt-get isntall mysql-client
sudo apt-get install libmysqlclient-dev
```
### 查看
```
　　sudo netstat -tap | grep mysql
```
安装成功则`mysql`处于`listen`状态


## php

###安装依赖
```
#libxml2，若无php安装一些解析xml的扩展时会提示xml2-config not found
sudo apt-get install libxml2 libxml2-dev libxslt-dev
#libevent1.4.11及以上版本，安装php的fpm模块时需要
sudo apt-get install libevent-1.4-2 libevent-dev
#libcurl，安装curl扩展需要
sudo apt-get install libcurl4-openssl-dev
#GD库，安装gd图片处理扩展需要
sudo apt-get install libgd2-xpm-dev
#zlib1g-dev，安装zlib和bz2扩展或编译mysqld阶段需要
sudo apt-get install zlib1g-dev libbz2-dev
#configure: error: mcrypt.h not found. Please reinstall libmcrypt.
sudo  apt-get install libmcrypt-dev
```

### 编译
```bash
wget -c http://cn2.php.net/get/php-5.6.11.tar.gz/from/this/mirror -O php-5.6.11.tar.gz
tar -zxvf php-5.6.11.tar.gz
cd php-5.6.11
./configure --prefix=/usr/local/web/php --with-config-file-path=/usr/local/web/php/etc --enable-fpm  --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-iconv-dir --with-freetype-dir=/usr/local/freetype --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl --enable-mbregex --enable-mbstring --with-mcrypt --enable-ftp --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip --enable-soap --with-gettext --disable-fileinfo
make
sudo make install
```

### 配置
配合`nginx`使用 需要进行配置`nginx.conf`
```
ocation ~ \.php$ {
       #   set $root /vagrant
           root $root;    #网站根目录
           fastcgi_pass   127.0.0.1:9000;
           fastcgi_index  index.php;
           fastcgi_param  SCRIPT_FILENAME  $root$fastcgi_script_name;
           include        fastcgi_params;
       }
```
### 启动
使用`php-fpm`：
```
sudo cp /usr/local/web/php/etc/php-fpm.conf.default /usr/local/web/php/etc/php-fpm.conf
sudo vi /usr/local/web/php/etc/php-fpm.conf
	
```
将`php-fpm`作为服务启动
```
sudo cp ./sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
sudo chmod a+x /etc/init.d/php-fpm
```
```
sudo service php-fpm start
```



