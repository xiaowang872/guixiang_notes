# 第一：数据库+根目录备份

cd到网站根目录上一级（我的是`/code/wordpress`）：

（这三个在命令行操做）

~~~
cd /code
#网站根目录下有wp-admin,wp-content等等
~~~

压缩根目录

~~~bash
tar -czvf wordpress_files.tar.gz wordpress/
~~~

备份数据库

~~~bash
mysqldump -u root -p wordpress > wordpress_database.sql 
~~~

# 第二：重装环境

这个自己怎么装lnmp架构的，再装一遍





到根目录下解压

~~~
cd /code
tar -xzvf wordpress_files.tar.gz
~~~

将

~~~
mysql -u用户名 -p密码
mysql > use wordpress
mysql > source /backup/mysqldump/wordpress_database.sql 
~~~

好了
