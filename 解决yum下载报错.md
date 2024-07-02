---
title: 解决yum下载报错
date: 2024/7/2 17:30:22
tag: [解决报错]
categories: 解决报错
description: 有关yum换源的优化
top_img: https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240702174040.png
cover: https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240702173901.png
---

### 报错信息

~~~
    │               (SSH client, X server and network tools)               │
    │                                                                      │
    │ ⮞ SSH session to root@192.168.117.166                                │
    │   • Direct SSH      :  ✓                                             │
    │   • SSH compression :  ✓                                             │
    │   • SSH-browser     :  ✓                                             │
    │   • X11-forwarding  :  ✗  (disabled or not supported by server)      │
    │                                                                      │
    │ ⮞ For more info, ctrl+click on help or visit our website.            │
    └──────────────────────────────────────────────────────────────────────┘

Last failed login: Tue Jul  2 16:39:04 CST 2024 from 192.168.117.1 on ssh:notty
There were 2 failed login attempts since the last successful login.
Last login: Tue Jul  2 16:38:05 2024
[root@localhost ~]# wget -O /etc/yum.repos.d/docker-ce.repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
-bash: wget: 未找到命令
[root@localhost ~]# yum -y install wget
已加载插件：fastestmirror
Determining fastest mirrors
Could not retrieve mirrorlist http://mirrorlist.centos.org/?release=7&arch=x86_64&repo=os&infra=stock error was
14: curl#6 - "Could not resolve host: mirrorlist.centos.org; 未知的错误"


 One of the configured repositories failed (未知),
 and yum doesn't have enough cached data to continue. At this point the only
 safe thing yum can do is fail. There are a few ways to work "fix" this:

     1. Contact the upstream for the repository and get them to fix the problem.

     2. Reconfigure the baseurl/etc. for the repository, to point to a working
        upstream. This is most often useful if you are using a newer
        distribution release than is supported by the repository (and the
        packages for the previous distribution release still work).

     3. Run the command with the repository temporarily disabled
            yum --disablerepo=<repoid> ...

     4. Disable the repository permanently, so yum won't use it by default. Yum
        will then just ignore the repository until you permanently enable it
        again or use --enablerepo for temporary usage:

            yum-config-manager --disable <repoid>
        or
            subscription-manager repos --disable=<repoid>

     5. Configure the failing repository to be skipped, if it is unavailable.
        Note that yum will try to contact the repo. when it runs most commands,
        so will have to try and fail each time (and thus. yum will be be much
        slower). If it is a very temporary problem though, this is often a nice
        compromise:

            yum-config-manager --save --setopt=<repoid>.skip_if_unavailable=true

Cannot find a valid baseurl for repo: base/7/x86_64

~~~

## 解决思想：换源

#### 第一步：查看系统版本

~~~
cat /etc/redhat-release
~~~

我的：

~~~
[root@localhost yum.repos.d]# cat /etc/redhat-release
CentOS Linux release 7.6.1810 (Core)
~~~

是7.6.1810版本

#### 第二步：下载源

~~~
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
~~~

注意，这边的`7`是大版本号

如果没有安装wget：

~~~
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
~~~

第三步：

执行：

~~~
yum clean all

yum makecache

yum -y update

~~~







解决报错

curl#6 - "Could not resolve host: mirrorlist.centos.org; 未知的错误"