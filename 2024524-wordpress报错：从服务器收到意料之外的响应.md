---
title: wordpress报错：从服务器收到意料之外的响应
date: 2024/5/24 0:01:11
tag: [lnmp服务器设置更新]
categories: 服务器报错
description: 服务器的优化
top_img:https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240524000457.png
cover: https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE202405232318111.jpg
---

### 报错信息：

![](https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240520195299.png)

### 原因：

因为nginx还限制了请求体大小，需要在nginx的虚拟机配置文件中添加：

~~~
client_max_body_size 50m;
~~~

### 之后重启nginx

~~~
systemctl restart nginx
~~~

![](https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240523235702.png)