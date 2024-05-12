---
title: nginx安装
data: 2024/5/12 10:50
tag: "实战"
---

[TOC]

# nginx 编译安装

## 1.0.关闭防火墙：

~~~
systemctl stop firewalld
setenforce 0
~~~

## 1.1.注意！如果你是服务器安装nginx

不要关闭防火墙！！！！！！！！

不要关闭防火墙！！！！！！！！

不要关闭防火墙！！！！！！！！

步骤1.0省略：

### 1.1.1开放防火墙端口

我以阿里云为例：

![](https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/%7BEEDADE1E-B038-4ae7-9220-9AB072D7338F%7D.png)

![](https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240512114337.png)

## 2.编译环境安装：（c++环境）

~~~
yum -y install gcc pcre-devel openssl-devel zlib-devel
~~~

## 3.安装系统用户：

~~~
useradd -r -s /sbin/nologin nginx
~~~

创建一个新的系统用户，通常用于运行服务或应用程序，而不是供人类用户登录。

## 4.官网获取源码并将压缩包放到`/usr/local/src`目录下：

官网网址：[Index of /download/ (nginx.org)](https://nginx.org/download/)

~~~
wget https://nginx.org/download/nginx-1.18.0.tar.gz -P /usr/local/src/
~~~

## 5.进入压缩包目录并解压并删除压缩包：

~~~
 cd /usr/local/src
 tar xzvf nginx-1.18.0.tar.gz 
 rm -rf nginx-1.18.0.tar.gz 
~~~

## 6.进入解压目录并开始配置，并为编译做准备：

~~~
cd nginx-1.18.0

./configure --prefix=/apps/nginx \
--user=nginx \
--group=nginx \
--with-http_ssl_module \
--with-http_v2_module \
--with-http_realip_module \
--with-http_stub_status_module \
--with-http_gzip_static_module \
--with-pcre \
--with-stream \
--with-stream_ssl_module \
--with-stream_realip_module
~~~

## 7.源代码开始编译并安装软件

~~~
make -j 2 && make install
~~~

## 8.将安装目录及其所有子目录和文件的所有者，所属组更改为 nginx

~~~
chown -R nginx.nginx /apps/nginx
~~~

## 9.通过软链接设置环境变量，快捷启动nginx

~~~
ln -s /apps/nginx/sbin/nginx /usr/bin/
~~~

## 10.启动nginx

~~~
nginx
~~~

## 11.网址： ip

![](https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240512114732.png)