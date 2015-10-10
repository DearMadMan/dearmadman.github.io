title: linux 手动安装 lnmp 环境
date: 2015-07-15 17:44:31
categories: linux
tags: [centOs ,lnmp ,nginx ,mysql ,php] 
---

linux 环境手动进行 nginx mysql php 环境配置
===========================================
![shoes](/images/shoes.jpg)

### Nginx 

###### 依赖项：
* gcc 
* gcc-c++ 
* pcre 
* pcre-devel
* zlib-devel
* openssl
* openssl-devel

```bash
 sudo yum install -y gcc gcc-c++ pcre pcre-devel zlib-devel openssl openssl-devel
```


###### 下载 & 解压

```bash
 wget -c http://nginx.org/download/nginx-1.9.3.tar.gz
 tar -zxvf nginx-1.9.3.tar.gz
```
 
###### 编译

```bash
 cd nginx-1.9.3
 ./configure --prefix=/usr/local/web/nginx
 make & make install 
 ```

###### 配置
```bash
 vi /usr/local/web/nginx/conf/nginx.conf     #进行配置修改
 ```
###### 设置根目录

```bash
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

###### 启动
```bash
	/usr/local/web/nginx/sbin/nginx
```


###### 查看 进程

```bash
 ps -ef | grep nginx
 ```

如果发现 `nginx: master`  表明 `nginx` 已经启动

### Mysql

###### 下载 & 安装

优先查看是否已经安装了 `mysql`
```bash 
	rpm -qa | grep mysql 
```
如果已经安装 则可通过 `rpm -e mysql-libs` or `rpm -e mysql` 命令进行卸载操作 `grep` 获取的信息来判断 
如果遇到有依赖关系无法删除的情况 可以使用 `rpm -e --nodeps mysql-libs` 来进行强制删除

我们可以通过`yum`命令进行mysql 安装操作
```bash
	yum list | grep mysql
	yum install -y mysql-server mysql mysql-devel
```
可以通过命令 `rpm -qi mysql-server` 来查看 `mysql-server` 的版本信息
通过yum安装的一般不是最新版本
如果想进行安装最新版本 可以通过如下方式进行安装  请注意选择 `32位` or `64位` 
下载地址:`http://dev.mysql.com/downloads/mysql/5.5.html#downloads`

###### 相关依赖
* perl
* libaio

```bash
yum install -y perl libaio
```

`64位 :`
```bash
wget -c http://cdn.mysql.com/Downloads/MySQL-5.5/MySQL-server-5.5.44-1.el6.x86_64.rpm
wget -c http://cdn.mysql.com/Downloads/MySQL-5.5/MySQL-client-5.5.44-1.el6.x86_64.rpm
wget -c http://cdn.mysql.com/Downloads/MySQL-5.5/MySQL-devel-5.5.44-1.el6.x86_64.rpm
```
下载完成后 进行安装操作
```bash
rpm -ivh MySQL-server-5.5.44-1.el6.x86_64.rpm MySQL-client-5.5.44-1.el6.x86_64.rpm MySQL-devel-5.5.44-1.el6.x86_64.rpm
```
安装完成后可以通过命令
```bash
mysql
```
查看是否安装成功
如果出现以下内容`ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)` 表明需要启动mysql服务
```bash
service mysql start 
```
`注意：` 通过yum安装方式 服务启动为
```bash
service mysqld start  # or /etc/init.d/mysqld start  
```

### PHP



###### 下载 & 安装
```bash
yum install -y php 
	```
但是 `yum` 安装的`php` 通常不是最新的版本 (可以通过命令`php -v` 进行查看当前安装的php 版本)

###### 手动安装

###### 依赖项：
* bzip2-devel
* libxml2-devel
* curl-devel
* db4-devel
* libpng-devel
* freetype-devel
* libjpeg-devel
* php-mcrypt  
* libmcrypt 
* libmcrypt-devel
* openssl-devel

```bash
yum remove php # 先删除通过yum安装的php
#安装依赖
yum install -y libxml2 libxml2-devel libjpeg libjpeg-devel bzip2-devel curl-devel db4-devel libpng-devel freetype-devel php-mcrypt libmcrypt libmcrypt-devel openssl-devel
##### 有的依赖需要用第三方源安装
# wget http://www.atomicorp.com/installers/atomic
# sh ./atomic
#####
wget -c http://cn2.php.net/get/php-5.6.11.tar.gz/from/this/mirror
```
下载完成后：
```bash
tar -zxvf php-5.6.11.tar.gz
cd php-5.6.11
./configure --prefix=/usr/local/web/php
 #常规编译选项
 #./configure --prefix=/usr/local/web/php --with-config-file-path=/usr/local/web/php/etc --enable-fpm  --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-iconv-dir --with-freetype-dir=/usr/local/freetype --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl --enable-mbregex --enable-mbstring --with-mcrypt --enable-ftp --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip --enable-soap --with-gettext --disable-fileinfo
make && make install
```
###### 使用php-cgi 

使用`php-cgi`启动前需要先进行`php.ini`配置
设置如下：
```php
cgi.fix_pathinfo = 1
```

配合`nginx`使用 需要进行配置`nginx.conf`
```nginx
 location ~ \.php$ {
        #   set $root /vagrant
            root $root;    #网站根目录
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $root$fastcgi_script_name;
            include        fastcgi_params;
        }
```

启动：
```bash
/usr/local/web/nginx/sbin/nginx -s reload
/usr/local/web/php/bin/php-cgi -b 9000           #启动并绑定9000端口
```

###### 使用php-fpm
除了上边`php-cgi`的方式配配合`nginx` ,我们还可以使用`php-fpm`配合`nginx`使用
使用`php-fpm`需要在编译php的时候 开启`php-fpm`才行 
```bash
    ./configure --enable-fpm --enable-mbstring --prefix=/usr/local/web/php
    make && make install 
```
优点:修改`php.ini`可以平滑启动 不需要重启服务
`php-cgi` 因为是`long-live`模式 修改`php.ini`必须要重启服务才能生效

安装完成后 进行`php-fpm`配置
```bash
cp /usr/local/web/php/etc/php-fpm.conf.default /usr/local/web/php/etc/php-fpm.conf
vi /usr/local/web/php/etc/php-fpm.conf
```
将`php-fpm`作为服务启动
```bash
cp ./sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
chmod a+x /etc/init.d/php-fpm
```
启动：
```bash
service php-fpm start
```