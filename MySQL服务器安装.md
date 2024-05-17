---
title: MySQL服务器安装
data: 2024/5/17 16:34:11
tag: 知识集锦
---

# 引言：

`mysql`安装有两个方式，第一种较慢，第二种快,还有要注意磁盘空间留足

# 优缺点

- 二进制
  - 不需要安装，解压即可使用，不能定制功能
- 编译安装
  - 可定制，安装慢
  - 四个步骤
    - 解压(tar)
    - 生成(./configure) cmake
    - 编译(make)
    - 安装(make install)

# 第一种：源码编译安装：

MySQL版本选择：

- https://downloads.mysql.com/archives/community/

- 下载源码，并且配置编译环境

```bash
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-5.6.40.tar.gz
tar xzvf mysql-5.6.40.tar.gz
cd mysql-5.6.40
yum install -y ncurses-devel libaio-devel cmake gcc gcc-c++ glibc
```

- 创建mysql用户

```bash
useradd mysql -s /sbin/nologin -M
```

- 编译并安装

```bash
mkdir /application
cmake . -DCMAKE_INSTALL_PREFIX=/application/mysql-5.6.40 \
-DMYSQL_DATADIR=/application/mysql-5.6.40/data \
-DMYSQL_UNIX_ADDR=/application/mysql-5.6.40/tmp/mysql.sock \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_EXTRA_CHARSETS=all \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_FEDERATED_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DWITHOUT_EXAMPLE_STORAGE_ENGINE=1 \
-DWITH_ZLIB=bundled \
-DWITH_SSL=bundled \
-DENABLED_LOCAL_INFILE=1 \
-DWITH_EMBEDDED_SERVER=1 \
-DENABLE_DOWNLOADS=1 \
-DWITH_DEBUG=0




make && make install
```

- 创建配置文件

```bash
ln -s /application/mysql-5.6.40/ /application/mysql
cd /application/mysql/support-files/
cp my-default.cnf /etc/my.cnf
cp：是否覆盖"/etc/my.cnf"？ y
```

- 创建启动脚本

```bash
cp mysql.server /etc/init.d/mysqld
```

- 初始化数据库

```bash
cd /application/mysql/scripts/
yum -y install autoconf
./mysql_install_db --user=mysql --basedir=/application/mysql --datadir=/application/mysql/data/
```

-  启动数据库

```bash
mkdir /application/mysql/tmp
chown -R mysql.mysql /application/mysql*
/etc/init.d/mysqld start
```

- 配置环境变量

```bash
[root@m scripts]# vim /etc/profile.d/mysql.sh
export PATH="/application/mysql/bin:$PATH"

[root@m scripts]# source /etc/profile
```

- systemd管理mysql

```bash
vim /usr/lib/systemd/system/mysqld.service
[Unit]
Description=MySQL Server
Documentation=man:mysqld(8)
Documentation=https://dev.mysql.com/doc/refman/en/using-systemd.html
After=network.target
After=syslog.target
[Install]
WantedBy=multi-user.target
[Service]
User=mysql
Group=mysql
ExecStart=/application/mysql/bin/mysqld --defaults-file=/etc/my.cnf
LimitNOFILE = 5000
```

- 设置mysql开机启动，并且开始mysql服务

```bash
systemctl start mysqld
systemctl enable mysqld
```

- 设置mysql密码，并且登录测试

```bash
mysqladmin -uroot password '123456'
mysql -uroot -p123456
mysql> show databases;
mysql> \q
Bye
```

## 二进制安装

当前运维和开发最常见的做法是二进制安装mysql

- 下载二进制包并且解压

```bash
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-5.6.40-linux-glibc2.12-x86_64.tar.gz
tar xzvf mysql-5.6.40-linux-glibc2.12-x86_64.tar.gz
```

- 然后的步骤就和编译安装一样

```bash
mkdir /application
mv mysql-5.6.40-linux-glibc2.12-x86_64 /application/mysql-5.6.40
ln -s /application/mysql-5.6.40 /application/mysql
cd /application/mysql/support-files
cp my-default.cnf /etc/my.cnf
cp：是否覆盖"/etc/my.cnf"？ y
cp mysql.server /etc/init.d/mysqld
cd /application/mysql/scripts
useradd mysql -s /sbin/nologin -M
 yum -y install autoconf
./mysql_install_db --user=mysql --basedir=/application/mysql --data=/application/mysql/data
vim /etc/profile.d/mysql.sh
export PATH="/application/mysql/bin:$PATH"

source /etc/profile
```

- 需要注意，官方编译的二进制包默认是在`/usr/local`目录下的，我们需要修改配置文件

```bash
sed -i 's#/usr/local#/application#g' /etc/init.d/mysqld /application/mysql/bin/mysqld_safe
```

- 创建systemd管理文件，并且测试是否正常使用

```bash
vim /usr/lib/systemd/system/mysqld.service
[Unit]
Description=MySQL Server
Documentation=man:mysqld(8)
Documentation=https://dev.mysql.com/doc/refman/en/using-systemd.html
After=network.target
After=syslog.target
[Install]
WantedBy=multi-user.target
[Service]
User=mysql
Group=mysql
ExecStart=/application/mysql/bin/mysqld --defaults-file=/etc/my.cnf
LimitNOFILE = 5000

vim /etc/my.cnf
basedir = /application/mysql/
datadir = /application/mysql/data

systemctl daemon-reload
systemctl start mysqld
systemctl enable mysqld
mysqladmin -uroot password '123456'
mysql -uroot -p123456
```