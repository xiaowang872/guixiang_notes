---
title: git教程笔记二
data: 2024/5/2 16:07:23
tag: 笔记
---

[TOC]

# git diff

![](https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240502170544.png)



## git diff

工作区与暂存区的区别

~~~
admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ cat 3.txt
333
777

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ echo "3331" > 3.txt

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git diff
diff --git a/3.txt b/3.txt
index 68597d5..f006881 100644
--- a/3.txt
+++ b/3.txt
@@ -1,2 +1 @@
-333
-777
+3331

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git add .

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git diff

~~~



## git diff HEAD

工作区与版本库的差异

~~~
admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git diff HEAD
diff --git a/3.txt b/3.txt
index 68597d5..f006881 100644
--- a/3.txt
+++ b/3.txt
@@ -1,2 +1 @@
-333
-777
+3331


~~~



## git diff --cached

暂存区与版本库的区别

~~~
admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git diff --cached
diff --git a/3.txt b/3.txt
index 68597d5..f006881 100644
--- a/3.txt
+++ b/3.txt
@@ -1,2 +1 @@
-333
-777
+3331

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git commit -m "abababab"
[master ac1eb1e] abababab
 1 file changed, 1 insertion(+), 2 deletions(-)

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git diff --cached

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$

~~~



## git diff (id) (id)

比较两个版本的差异

~~~
admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git log
commit ac1eb1e23497c177036a49f88d75068cf2cddabd (HEAD -> master)
Author: xiaowang872 <2826551098@qq.com>
Date:   Thu May 2 16:40:18 2024 +0800

    abababab

commit 1ea5d616e45266ec0d235293e9e52386a2900284
Author: xiaowang872 <2826551098@qq.com>
Date:   Thu May 2 16:34:58 2024 +0800

    122

commit 2eb45dd3436afbc085a7721008c47bb0f8ad35f0
Author: xiaowang872 <2826551098@qq.com>
Date:   Thu May 2 16:17:10 2024 +0800

    this is 3.txt

commit 077ee244bd459c2edb26669bb432acccba499e5c

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git diff 077ee244bd459c2edb26669bb432acccba499e5c
1ea5d616e45266ec0d235293e9e52386a2900284
diff --git a/3.txt b/3.txt
new file mode 100644
index 0000000..f006881
--- /dev/null
+++ b/3.txt
@@ -0,0 +1 @@
+3331
bash: 1ea5d616e45266ec0d235293e9e52386a2900284: command not found

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git diff 077ee244bd459c2edb26669bb432acccba499e5c 1ea5d616e45266ec0d235293e9e52386a2900284
diff --git a/3.txt b/3.txt
new file mode 100644
index 0000000..68597d5
--- /dev/null
+++ b/3.txt
@@ -0,0 +1,2 @@
+333
+777

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$

~~~

## git diff (id) HEAD

表示当前版本与id版本区别

~~~
admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git diff 077ee244bd459c2edb26669bb432acccba499e5c HEAD
diff --git a/3.txt b/3.txt
new file mode 100644
index 0000000..f006881
--- /dev/null
+++ b/3.txt
@@ -0,0 +1 @@
+3331


~~~

## git diff HEAD HEAD~

当前版本与上一个版本的区别

或者`git diff HEAD HEAD^`

`git diff HEAD HEAD~2`

表示提交之前的第二个版本与现版本区别

~~~
admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git diff HEAD HEAD~
diff --git a/3.txt b/3.txt
index f006881..68597d5 100644
--- a/3.txt
+++ b/3.txt
@@ -1 +1,2 @@
-3331
+333
+777


~~~

## git diff HEAD HEAD~ (文件名)

懂？

~~~
admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git diff HEAD HEAD~2 3.txt
diff --git a/3.txt b/3.txt
index f006881..55bd0ac 100644
--- a/3.txt
+++ b/3.txt
@@ -1 +1 @@
-3331
+333


~~~

## git diff (分支名) （分支名）

表示分支间的区别

# 如何从版本库中删除文件

![](https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240502172342.png)

## 第一种：直接删除文件之后提交

~~~
admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ ll
total 3
-rw-r--r-- 1 admin 197121 4 May  2 16:14 1.txt
-rw-r--r-- 1 admin 197121 4 May  2 16:14 2.txt
-rw-r--r-- 1 admin 197121 5 May  2 16:35 3.txt

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ rm -rf 1.txt

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ ll
total 2
-rw-r--r-- 1 admin 197121 4 May  2 16:14 2.txt
-rw-r--r-- 1 admin 197121 5 May  2 16:35 3.txt

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git ls-files
1.txt
2.txt
3.txt


~~~

可见，工作区删掉了1.txt，但暂存区没删掉1.txt

~~~

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git add .

或者


admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git add 1.txt

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git ls-files
2.txt
3.txt


~~~

## 第二种：利用git自带的rm

~~~
admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git rm 2.txt
rm '2.txt'

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git ls-files
3.txt

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ ll
total 1
-rw-r--r-- 1 admin 197121 5 May  2 16:35 3.txt

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$

~~~

十分推荐

记得要提交

~~~
admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git commit -m "delete 1.txt and 2.txt"
[master ead165a] delete 1.txt and 2.txt
 2 files changed, 2 deletions(-)
 delete mode 100644 1.txt
 delete mode 100644 2.txt

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$

~~~





# 如何让git忽视掉一些敏感文件

## .gitignore

注意， `.gitignore`对已添加在版本库的文件不会有作用

~~~

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ echo "access.log" >> access.log

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ echo "other_log" >> other.log

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ ll
total 2
-rw-r--r-- 1 admin 197121 11 May  2 17:30 access.log
-rw-r--r-- 1 admin 197121 10 May  2 17:31 other.log

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        access.log
        other.log

nothing added to commit but untracked files present (use "git add" to track)

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ echo "access.log" >> .gitignore

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        other.log

nothing added to commit but untracked files present (use "git add" to track)

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ ll
total 2
-rw-r--r-- 1 admin 197121 11 May  2 17:30 access.log
-rw-r--r-- 1 admin 197121 10 May  2 17:31 other.log

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ ll -a
total 15
drwxr-xr-x 1 admin 197121  0 May  2 17:34 ./
drwxr-xr-x 1 admin 197121  0 May  2 16:14 ../
drwxr-xr-x 1 admin 197121  0 May  2 17:34 .git/
-rw-r--r-- 1 admin 197121 11 May  2 17:33 .gitignore
-rw-r--r-- 1 admin 197121 11 May  2 17:30 access.log
-rw-r--r-- 1 admin 197121 10 May  2 17:31 other.log

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git add .

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git ls-files
.gitignore
other.log

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git commit -m "ignore file 'access.log'"
[master bedd3af] ignore file 'access.log'
 2 files changed, 2 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 other.log

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ ll
total 2
-rw-r--r-- 1 admin 197121 11 May  2 17:30 access.log
-rw-r--r-- 1 admin 197121 10 May  2 17:31 other.log

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git ls-files
.gitignore
other.log

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$

~~~



*.log后缀也行

~~~

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ echo "*.log" > .
./          ../         .git/       .gitignore

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ echo "*.log" > .gitignore

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ cat .gitignore
*.log

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ echo "hello" > hello.log

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   .gitignore

no changes added to commit (use "git add" and/or "git commit -a")

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git commit -am "hello"
[master dab23a5] hello
 1 file changed, 1 insertion(+), 1 deletion(-)

admin@rootwang MINGW64 ~/Desktop/gitk/testk (master)
$ git ls-files
.gitignore
other.log


~~~

其他的匹配规则：

![](https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240503190526.png)

![](https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240503190759.png)

# github远程仓库托管代码

![](https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240503193140.png)

![](https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240503194231.png)

![](https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/QQ%E6%88%AA%E5%9B%BE20240504095159.png) 
