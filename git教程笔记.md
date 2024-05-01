---
title: git教程笔记
data: 2024/4/30 19:59:11
tag: "笔记"
---

[TOC]



# 开头部分（操作）：

## 一：录入信息

~~~
admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git -v
git version 2.40.0.windows.1

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git config --global --list
user.name=xiaowang872
user.email=2826551098@qq.com
color.ui=true

#保存写入信息
admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git config --global credential.helper store
~~~

## 二：建立仓库

### 方法一：

~~~
git init
~~~

### 方法二：

~~~
git clone (仓库地址)
如：
git clone git@github.com:xiaowang872/theme-next-docs.git
~~~

注意，不要在初始化后修改.git隐藏目录下的任何内容（你猜他为什么要隐藏）

# 理论部分：

git的三个区域：

![](https://cdn.jsdelivr.net/gh/xiaowang872/blogimage@main/images/image-20240430201758137.png)

# 正式命令部分：

## git status

查看状态

~~~
admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git status
On branch master
Your branch is ahead of 'origin1/master' by 2 commits.
  (use "git push" to publish your local commits)

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        1.txt
        test3/

nothing added to commit but untracked files present (use "git add" to track)
#让我清一下
admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ rm -rf test3/

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git add 1.txt

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git status
On branch master
Your branch is ahead of 'origin1/master' by 2 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   1.txt

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git push origin1 master
Everything up-to-date

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git status
On branch master
Your branch is up to date with 'origin1/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   1.txt

~~~

一切恢复了正常，就这？？简简单单，好吧

## git restore --staged <file>

将文件从暂存区再挪到工作区

~~~
admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git restore --staged 1.txt

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git status
On branch master
Your branch is up to date with 'origin1/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        1.txt

nothing added to commit but untracked files present (use "git add" to track)

~~~

## git add <file>

将文件从工作区挪到暂存区

~~~
admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git add 1.txt

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git status
On branch master
Your branch is up to date with 'origin1/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   1.txt
~~~

### git add .

添加所有工作区文件到暂存区

### git add *.txt

添加所有以.txt为后缀的文件到暂存区



## git commit -m "注释"

提交暂存区文件，而不会提交工作区文件

~~~
admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ echo "guixiang_yyds" >> 2.txt

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git status
On branch master
Your branch is up to date with 'origin1/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   1.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   2.txt


admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git commit
Aborting commit due to empty commit message.

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git commit -m "第一次提交"
[master a58c995] 第一次提交
 1 file changed, 1 insertion(+)
 create mode 100644 1.txt

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git status
On branch master
Your branch is ahead of 'origin1/master' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   2.txt

no changes added to commit (use "git add" and/or "git commit -a")

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$

~~~

发现1.txt已经提交了，不见了，只剩下2.txt

对了中途commit时 没加 -m 

于是咱补一下：

~~~
admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git push origin1 master
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 20 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 343 bytes | 114.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:xiaowang872/test3.git
   32ea9de..a58c995  master -> master

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git status
On branch master
Your branch is up to date with 'origin1/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   2.txt

no changes added to commit (use "git add" and/or "git commit -a")
~~~

这下正常多了

## git commit

~~~
admin@rootwang MINGW64 ~/Desktop/gitk (master)
$  touch 3.txt

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git add 3.txt

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git status
On branch master
Your branch is up to date with 'origin1/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   3.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   2.txt

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git commit


~~~



进入交互页面：

`i`进入编辑在第一行写注释

忘了后`:wq`保存

~~~
这是3.txt还有，guixiangyyds、
#Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# Your branch is up to date with 'origin1/master'.
#
# Changes to be committed:
#       new file:   3.txt
#
# Changes not staged for commit:
#       modified:   2.txt
#
~
~
~
~
~
~
~
~
~
.git/COMMIT_EDITMSG[+] [unix] (20:51 30/04/2024)                     1,36-30 All
-- INSERT --

~~~

~~~

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git commit
[master e4524a9] 这是3.txt还有，guixianyds、
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 3.txt

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$

~~~

## git log

查看提交记录

~~~

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git log
commit e4524a9f4368e41739863cf1f30cd471e410f4f8 (HEAD -> master)
Author: xiaowang872 <2826551098@qq.com>
Date:   Tue Apr 30 20:51:09 2024 +0800

    这是3.txt还有，guixianyds、

commit a58c99507f5933d8ba53b82190a44f1f6ff06fa1 (origin1/master)
Author: xiaowang872 <2826551098@qq.com>
Date:   Tue Apr 30 20:39:08 2024 +0800

    第一次提交

commit 32ea9dee57249a805850a9711e862a106f7526a2 (origin/master)
Merge: 58d3a88 4826775
Author: xiaowang872 <2826551098@qq.com>
Date:   Sat Apr 27 16:18:30 2024 +0800

    Merge branch 'master' of github.com:xiaowang872/test3

commit 58d3a88d797bbf44f21fcb9de9569bafdc6a1d7c
Author: xiaowang872 <2826551098@qq.com>
Date:   Sat Apr 27 16:14:06 2024 +0800

:

~~~

每次你提交到Git仓库时，Git都会为这个提交生成一个唯一的哈希值。

### git log --oneline

更简洁的显示方式

~~~
admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ git log --oneline
e4524a9 (HEAD -> master) 这是3.txt还有，guixianyds、
a58c995 (origin1/master) 第一次提交
32ea9de (origin/master) Merge branch 'master' of github.com:xiaowang872/test3
58d3a88 docs: jia_ru_9.txt
4826775 Create 2.txt
10ad
~~~

## git reset

reset 回退版本，可以退回到之前的某一个提交状态

![image-20240501184953214](./../images/image-20240501184953214.png)

准备：

~~~
admin@rootwang MINGW64 ~/Desktop
$ mkdir testk

admin@rootwang MINGW64 ~/Desktop
$ cd testk/

admin@rootwang MINGW64 ~/Desktop/testk
$ git init
Initialized empty Git repository in C:/Users/admin/Desktop/testk/.git/

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ ll
total 0

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ echo "111" > file1

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ echo "222" > file2

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ echo "333" > file3

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git add file1

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git commit -m "file1的提交，2,3没交"
[master (root-commit) d28f47b] file1的提交，2,3没交
 1 file changed, 1 insertion(+)
 create mode 100644 file1

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git add file2

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git commit -m "file2的提交，1交了,3没交"
[master a0147ab] file2的提交，1交了,3没交
 1 file changed, 1 insertion(+)
 create mode 100644 file2

admin@rootwang MINGW64 ~/Desktop/testk (master)
$
Display all 3162 possibilities? (y or n)

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git add file3

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git commit -m "file3的提交，1,2交了"
[master 3de8283] file3的提交，1,2交了
 1 file changed, 1 insertion(+)
 create mode 100644 file3

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git log
commit 3de82837a95ddfb52d929b1648ebfb2dd1592c78 (HEAD -> master)
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:48:22 2024 +0800

    file3的提交，1,2交了

commit a0147aba8bddc66902bebe7ab4e4041db1fdade2
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:47:03 2024 +0800

    file2的提交，1交了,3没交

commit d28f47b53428989a9354bbe0bc143218a7822ab3
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:46:30 2024 +0800

    file1的提交，2,3没交
~~~

之后再将testk复制下，咱分别讨论：

~~~
admin@rootwang MINGW64 ~/Desktop/testk (master)
$ cd ..

admin@rootwang MINGW64 ~/Desktop
$ cp -rf testk/ testk_soft

admin@rootwang MINGW64 ~/Desktop
$ cp -rf testk/ testk_hard

admin@rootwang MINGW64 ~/Desktop
$ cp -rf testk/ testk_mixed

admin@rootwang MINGW64 ~/Desktop
$ ll
total 996
-rw-r--r-- 1 admin 197121    296 Apr 21 09:03  01master02node103node2192.md
-rwxr-xr-x 1 admin 197121   1484 Dec 17 14:51  BCompare.exe.lnk*
drwxr-xr-x 1 admin 197121      0 Mar 24 09:56  BLOG/
drwxr-xr-x 1 admin 197121      0 Sep  3  2023  Burp-2021.12带注册/
-rwxr-xr-x 1 admin 197121   2073 Feb 29 15:43  CCtalk.lnk*
-rwxr-xr-x 1 admin 197121   1415 Mar  3 11:51 'Clash for Windows.exe.lnk'*
-rwxr-xr-x 1 admin 197121   1177 Nov 18 17:30  EV加密播放2.lnk*
drwxr-xr-x 1 admin 197121      0 Sep  7  2023  English/
-rwxr-xr-x 1 admin 197121   1160 Sep  5  2023  FinalShell.lnk*
-rwxr-xr-x 1 admin 197121   1360 Mar 18 19:12  Firefox.lnk*
-rwxr-xr-x 1 admin 197121   2820 Mar  3 20:36  GitHub.lnk*
-rwxr-xr-x 1 admin 197121   1117 Sep 23  2023  HBuilderX.exe.lnk*
-rwxr-xr-x 1 admin 197121   2427 Mar 18 19:12 'Microsoft Edge.lnk'*
-rwxr-xr-x 1 admin 197121    680 Sep  5  2023 'Nmap - Zenmap GUI.lnk'*
-rwxr-xr-x 1 admin 197121    680 Mar  5 21:45  PicGo.lnk*
-rw-r--r-- 1 admin 197121  33894 Apr 21 08:58  QQ截图20240421085814.png
-rwxr-xr-x 1 admin 197121   1283 Sep 22  2023  ScreenPal.lnk*
drwxr-xr-x 1 admin 197121      0 Mar 23 09:44  Thunder/
-rwxr-xr-x 1 admin 197121    862 Nov  4 09:01 'Visual Studio Code.lnk'*
-rwxr-xr-x 1 admin 197121   2360 Aug 12  2023 'WPS Office.lnk'*
-rw-r--r-- 1 admin 197121    207 Dec  5 23:27 'Wallpaper Engine：壁纸引擎.url'
-rwxr-xr-x 1 admin 197121    816 Nov 29 16:25  Xftp.lnk*
-rwxr-xr-x 1 admin 197121    824 Nov 29 16:25  Xshell.lnk*
-rw-r--r-- 1 admin 197121    282 Aug 12  2023  desktop.ini
drwxr-xr-x 1 admin 197121      0 Apr 30 20:50  gitk/
drwxr-xr-x 1 admin 197121      0 Apr 20 15:16  markerdown文档/
-rwxr-xr-x 1 admin 197121   1546 Mar  2 20:27  notepad++.exe.lnk*
drwxr-xr-x 1 admin 197121      0 May  1 19:45  testk/
drwxr-xr-x 1 admin 197121      0 May  1 19:54  testk_hard/
drwxr-xr-x 1 admin 197121      0 May  1 19:54  testk_mixed/
drwxr-xr-x 1 admin 197121      0 May  1 19:53  testk_soft/
-rwxr-xr-x 1 admin 197121   2244 Sep 23  2023  uTools.lnk*
drwxr-xr-x 1 admin 197121      0 Nov  5 09:30  usbwebserver/
drwxr-xr-x 1 admin 197121      0 Nov  5 09:09  vcCodee/
-rw-r--r-- 1 admin 197121    449 Mar  3 11:10  xuexinwang.txt
-rwxr-xr-x 1 admin 197121   3044 Mar 13 19:19 '实习僧 大学生实习 校招求职 校园招聘.lnk'*
-rwxr-xr-x 1 admin 197121    590 Apr  3 23:17  崩坏：星穹铁道.lnk*
-rw-r--r-- 1 admin 197121    117 Sep  6  2023 '新建 Text Document (2).rar'
-rw-r--r-- 1 admin 197121    914 Mar 28 21:53 '新建 Text Document (2).txt'
-rw-r--r-- 1 admin 197121    164 Jan 21 12:58 '新建 Text Document.txt'
-rwxr-xr-x 1 admin 197121   1063 Aug 20  2023  百度网盘.lnk*
-rwxr-xr-x 1 admin 197121 849920 Nov 12 10:30  磁盘分析工具.exe*
-rwxr-xr-x 1 admin 197121   2295 Mar  9 14:46  语雀.lnk*
-rwxr-xr-x 1 admin 197121   2960 Mar 13 16:10 '超级简历WonderCV - HR推荐简历模板,智能简历制作工具,专业中英文简历模板免费下载.lnk'*
-rwxr-xr-x 1 admin 197121   1073 Mar 23 09:43  迅雷.lnk*

admin@rootwang MINGW64 ~/Desktop
$
~~~

准备好之后，开始讨论命令区别：

### git reset --soft (id号)

#### 补充（git reset --soft HEAD^）

#### 查看状态：

~~~git
admin@rootwang MINGW64 ~/Desktop
$ cd testk_soft/

admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ git log
commit 3de82837a95ddfb52d929b1648ebfb2dd1592c78 (HEAD -> master)
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:48:22 2024 +0800

    file3的提交，1,2交了

commit a0147aba8bddc66902bebe7ab4e4041db1fdade2
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:47:03 2024 +0800

    file2的提交，1交了,3没交

commit d28f47b53428989a9354bbe0bc143218a7822ab3
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:46:30 2024 +0800

    file1的提交，2,3没交
    
admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ git status
On branch master
nothing to commit, working tree clean

~~~

#### 命令执行：（  我挑的是“file2的提交，1交了,3没交” ）

~~~
admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ git reset --soft a0147aba8bddc66902bebe7ab4e4041db1fdade2

admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ ll
total 3
-rw-r--r-- 1 admin 197121 4 May  1 19:53 file1
-rw-r--r-- 1 admin 197121 4 May  1 19:53 file2
-rw-r--r-- 1 admin 197121 4 May  1 19:53 file3

#可见工作区没有发生变化

admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ git log
commit a0147aba8bddc66902bebe7ab4e4041db1fdade2 (HEAD -> master)
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:47:03 2024 +0800

    file2的提交，1交了,3没交

commit d28f47b53428989a9354bbe0bc143218a7822ab3
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:46:30 2024 +0800

    file1的提交，2,3没交

#发现提交记录少了file3

admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ git ls-files
file1
file2
file3

#发现暂存区没有改变
#git ls-files 是一个 Git 命令，用于列出 Git 仓库中索引或工作目录中的文件。这些文件是 #Git 跟踪的，但不一定是已提交的。

admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file3


#本地仓库提示我们file3是一个新文件
~~~

#### 总结：

使用`--soft`回退版本时，工作区与暂存区都不会发生改变。

这个时候我们就可以修改file3的内容，然后重新暂存和提交就好了

实例：

~~~、
admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ cat file3
333

admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ echo "666" >> file3

admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ cat file3
333
666

admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file3

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file3


admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ git add file3

admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ git commit file3 -m "file3的二次提交，添加了666"
[master 7f7ce9c] file3的二次提交，添加了666
 1 file changed, 2 insertions(+)
 create mode 100644 file3

admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ git status
On branch master
nothing to commit, working tree clean

admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ git log
commit 7f7ce9c18b04bbfe470e7d444b3be5a656db4f84 (HEAD -> master)
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 20:19:32 2024 +0800

    file3的二次提交，添加了666

commit a0147aba8bddc66902bebe7ab4e4041db1fdade2
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:47:03 2024 +0800

    file2的提交，1交了,3没交

commit d28f47b53428989a9354bbe0bc143218a7822ab3
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:46:30 2024 +0800

    file1的提交，2,3没交

admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$

~~~

### git reset --hard (id号)

#### 补充：（git reset (参数)  HEAD^）

~~~git
git reset (参数) HEAD^
~~~

表示回退到上一个版本

#### 准备+命令执行

~~~
admin@rootwang MINGW64 ~/Desktop/testk_soft (master)
$ cd ..

admin@rootwang MINGW64 ~/Desktop
$ cd t
Thunder/     testk/       testk_hard/  testk_mixed/ testk_soft/

admin@rootwang MINGW64 ~/Desktop
$ cd testk_hard

admin@rootwang MINGW64 ~/Desktop/testk_hard (master)
$ git log
commit 3de82837a95ddfb52d929b1648ebfb2dd1592c78 (HEAD -> master)
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:48:22 2024 +0800

    file3的提交，1,2交了

commit a0147aba8bddc66902bebe7ab4e4041db1fdade2
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:47:03 2024 +0800

    file2的提交，1交了,3没交

commit d28f47b53428989a9354bbe0bc143218a7822ab3
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:46:30 2024 +0800

    file1的提交，2,3没交

admin@rootwang MINGW64 ~/Desktop/testk_hard (master)
$ git reset --hard HEAD^
HEAD is now at a0147ab file2的提交，1交了,3没交

admin@rootwang MINGW64 ~/Desktop/testk_hard (master)
$ git log
commit a0147aba8bddc66902bebe7ab4e4041db1fdade2 (HEAD -> master)
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:47:03 2024 +0800

    file2的提交，1交了,3没交

commit d28f47b53428989a9354bbe0bc143218a7822ab3
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:46:30 2024 +0800

    file1的提交，2,3没交

admin@rootwang MINGW64 ~/Desktop/testk_hard (master)
$ ll
total 2
-rw-r--r-- 1 admin 197121 4 May  1 20:23 file1
-rw-r--r-- 1 admin 197121 4 May  1 20:23 file2

admin@rootwang MINGW64 ~/Desktop/testk_hard (master)
$ git status
On branch master
nothing to commit, working tree clean

admin@rootwang MINGW64 ~/Desktop/testk_hard (master)
$ git ls-files
file1
file2

~~~

#### 总结：

工作区，暂存区中file3全部消失

### git reset --mixed （id号）

#### 补充：

因为`--mixed`是默认选项，因此，可以写为

~~~
git reset （id号）
~~~

~~~
当然，也可以回退上一个版本：
git reset HEAD^
~~~

#### 准备+命令执行

~~~
admin@rootwang MINGW64 ~/Desktop/testk_hard (master)
$ cd ..

admin@rootwang MINGW64 ~/Desktop
$ cd testk_
testk_hard/  testk_mixed/ testk_soft/

admin@rootwang MINGW64 ~/Desktop
$ cd testk_
testk_hard/  testk_mixed/ testk_soft/

admin@rootwang MINGW64 ~/Desktop
$ cd testk_mixed/

admin@rootwang MINGW64 ~/Desktop/testk_mixed (master)
$ git reset HEAD^

admin@rootwang MINGW64 ~/Desktop/testk_mixed (master)
$ git log
commit a0147aba8bddc66902bebe7ab4e4041db1fdade2 (HEAD -> master)
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:47:03 2024 +0800

    file2的提交，1交了,3没交

commit d28f47b53428989a9354bbe0bc143218a7822ab3
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:46:30 2024 +0800

    file1的提交，2,3没交

admin@rootwang MINGW64 ~/Desktop/testk_mixed (master)
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file3

nothing added to commit but untracked files present (use "git add" to track)

admin@rootwang MINGW64 ~/Desktop/testk_mixed (master)
$ git ls-files
file1
file2

admin@rootwang MINGW64 ~/Desktop/testk_mixed (master)
$ ll
total 3
-rw-r--r-- 1 admin 197121 4 May  1 19:54 file1
-rw-r--r-- 1 admin 197121 4 May  1 19:54 file2
-rw-r--r-- 1 admin 197121 4 May  1 19:54 file3

~~~

#### 总结：

工作区中file3存在，暂存区中没有

## git reflog

### 作用：

主要用于查看本地仓库的 HEAD 和分支引用的变动历史，目的是恢复丢失的提交或理解引用的变化。它记录了 HEAD 和分支引用的每一次变动，包括每次变化的简要说明（如 commit、reset 等）和对应的提交哈希值。

~~~
admin@rootwang MINGW64 ~/Desktop/testk_mixed (master)
$ git reflog
a0147ab (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
3de8283 HEAD@{1}: commit: file3的提交，1,2交了
a0147ab (HEAD -> master) HEAD@{2}: commit: file2的提交，1交了,3没交
d28f47b HEAD@{3}: commit (initial): file1的提交，2,3没交

admin@rootwang MINGW64 ~/Desktop/testk_mixed (master)
$ git log --oneline
a0147ab (HEAD -> master) file2的提交，1交了,3没交
d28f47b file1的提交，2,3没交

admin@rootwang MINGW64 ~/Desktop/testk_mixed (master)
$ git log
commit a0147aba8bddc66902bebe7ab4e4041db1fdade2 (HEAD -> master)
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:47:03 2024 +0800

    file2的提交，1交了,3没交

commit d28f47b53428989a9354bbe0bc143218a7822ab3
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:46:30 2024 +0800

    file1的提交，2,3没交
~~~

### 应用：

通过reflog回退版本：

~~~

admin@rootwang MINGW64 ~/Desktop/testk_mixed (master)
$ git reset --hard a0147ab
HEAD is now at a0147ab file2的提交，1交了,3没交
~~~



# 结束语：在操作时遇到一些问题：

## 问题一：不同操作系统对换行符转化报错问题

示例：

~~~

admin@rootwang MINGW64 ~/Desktop/gitk (master)
$ cd ..

admin@rootwang MINGW64 ~/Desktop
$ git init testk
Initialized empty Git repository in C:/Users/admin/Desktop/testk/.git/

admin@rootwang MINGW64 ~/Desktop
$ cd t
Thunder/ testk/

admin@rootwang MINGW64 ~/Desktop
$ cd testk/

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ echo "file1" > file1.txt

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ echo "file2" > file2.txt

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ echo "file3" > file3.txt

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git add .
warning: in the working copy of 'file1.txt', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'file2.txt', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'file3.txt', LF will be replaced by CRLF the next time Git touches it

#这边出现了一个报错信息，原因是：
#这些警告信息表明在你的工作目录中的某些文件（在这个例子中是file1.txt、file2.txt和#file3.txt）具有与Git仓库中预期的换行符风格不同的换行符。具体来说，这些文件当前可能使用#Unix风格的换行符（LF，即line feed），但Git仓库期望在Windows系统上使用Windows风格的换#行符（CRLF，即carriage return line feed）。

#在Unix和Linux系统中，文本文件的行通常以单个换行符（LF）结束。而在Windows系统中，文本文
#件的行通常以回车符（CR）后跟换行符（LF，即CRLF）结束。Git提供了一个叫做core.autocrlf的
#配置选项，用于在检出（checkout）和提交（commit）时自动转换这些换行符。

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   file1.txt
        new file:   file2.txt
        new file:   file3.txt

#为了解决一下隐患，我们用core.autocrlf解决

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git config --global core.autocrlf input

#测试一下是否会报错

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ echo "file4" > file4.txt

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ echo "file5" > file5.txt

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git add .

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   file1.txt
        new file:   file2.txt
        new file:   file3.txt
        new file:   file4.txt
        new file:   file5.txt

没有报错，十分的完美。

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git commit .
[master (root-commit) dd04093] 我交了file1到5
 5 files changed, 5 insertions(+)
 create mode 100644 file1.txt
 create mode 100644 file2.txt
 create mode 100644 file3.txt
 create mode 100644 file4.txt
 create mode 100644 file5.txt

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git log
commit dd040937987ab9531bc2f9e5fd635e39d52d915f (HEAD -> master)
Author: xiaowang872 <2826551098@qq.com>
Date:   Wed May 1 19:05:51 2024 +0800

    我交了file1到5
admin@rootwang MINGW64 ~/Desktop/testk (master)
$ rm -rf file1.txt file2.txt file3.txt file4.txt file5.txt

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ ll
total 0

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git add .

admin@rootwang MINGW64 ~/Desktop/testk (master)
$ git commit -m "删除"
[master eebd65a] 删除
 5 files changed, 5 deletions(-)
 delete mode 100644 file1.txt
 delete mode 100644 file2.txt
 delete mode 100644 file3.txt
 delete mode 100644 file4.txt
 delete mode 100644 file5.txt

admin@rootwang MINGW64 ~/Desktop/testk (master)
$

~~~

