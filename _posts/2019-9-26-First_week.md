---
layout: post
title: "环境搭配"
date: 2019-9-26
description: "小白计划第一周，环境"
tag: 信安之路
---

### nginx的安装

+ 检查当前虚拟机是否安装了gcc，因为nginx是用c写的得使用gcc进行编译

![nginx_gcc](/
images/posts/first_week/1.png)

```
yum install gcc gcc-c++ -y      //安装gcc
```

+ 下载nginx、zlib、openssl、pcre、perl5源码包。nginx依赖zlib、openssl和pcre，需要先把它们三安装完成后在安装nginx。而openssl需要使用perl5的编译环境，pcre不能使用2.x.x的版本。

```
wget http://nginx.org/download/nginx-1.8.0.tar.gz
wget ftp://ftp.pcre.org/pub/pcre/pcre-8.42.tar.gz
wget http://zlib.net/zlib-1.2.11.tar.gz
wget https://www.openssl.org/source/openssl-1.0.2t.tar.gz
wget https://www.cpan.org/src/5.0/perl-5.28.0.tar.gz
```

![2](/
images/posts/first_week/2.png)

+ 因为是提前弄过了一遍，nginx包已经解压完了。将这些包全部解压到目录/usr/local/src

```
tar -xzvf openssl-1.0.26.tar.gz -C /usr/local/src/
tar -xzvf pcre-8.42.tar.gz -C /usr/local/src/
tar -xzvf perl-5.28.0.tar.gz -C /usr/local/src/
tar -xzvf zlib-1.2.11.tar.gz -C /usr/local/src/
```
![3](/
images/posts/first_week/3.png)

+ 进入perl目录，执行
```
./Configure -des -Dprefix=$HOME/localperl
make
make text
make install
```

+ 进入openssl的目录，执行
```
./config -prefix=/usr/local/openssl     //设置安装后的目录
make
make install
```

+ 进入zlib的目录，执行
```
./configure -prefix=/usr/local/zlib
make
make install
``` 

+ 进入pcre目录，执行
```
./configure -prefix=/usr/local/pcre
make
make install
```

+ 前期的准备工作都做好了，现在就开始进行nginx的安装。进入你nginx的解压目录，对其进行简单的初始化配置，后续再根据需求具体配置
```
./configure \
--user=nobody \
--group=nobody \
--prefix=/usr/local/nginx \
--with-http_stub_status_module \
--with-http_gzip_static_module \
--with-http_realip_module \
--with-http_sub_module \
--with-http_ssl_module \
--with-pcre=/usr/local/src/pcre-8.42/       //指定它的源码目录
--with-openssl=/usr/local/src/openssl-1.0.2t/ \
--with-zlib=/usr/local/src/zlib-1.2.11/
make
make install
```

+ 现在nginx已经安装好了，我们现在启动nginx
```
/usr/local/nginx/sbin/nginx
```
+ 如果你的物理机不能通过浏览器访问到虚拟机的index的话可能是你虚拟机的防火墙未关
```
systemctl stop firewalld.service    //关闭防火墙
systemctl disable firewalld.service     //禁止防火墙开机自启
```

+ 接下来在访问一遍你的虚拟机IP

![4](/images/posts/first_week/4.png)
