title: Complied nodejs in ubuntu
date: 2015-08-30 15:54:22
categories: [javascript,nodejs]
tags: [linux,node,npm]
---
![](/images/s03.jpg)

# 在ubuntu中配置node环境

## 下载
```bash
wget https://nodejs.org/dist/v0.12.7/node-v0.12.7.tar.gz
tar -zxvf node-v0.12.7.tar.gz
cd node-v0.12.7
```


## 编译

```bash
./configure
make
sudo make install

```

## 测试

```
node -v
npm -v
```
当前版本　npm是直接集成在node源文件中的　不需要进行独立安装

nodejs中的网络加加密是依赖openSSL的，记得安装libssl-dev依赖


```
sudo apt-get install libssl-dev

```

由于国内连接npm不稳定，我们需要借助国内npm镜像进行加速,这里使用淘宝的cnpm:
```bash
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
```

## 安装express

```
sudo cnpm install -gd express
```