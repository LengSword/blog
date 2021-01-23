---
title: Bandit Writeup(Level 0-10)
layout: post
date: 2020/03/30
---

> Target Host: bandit.labs.overthewire.org
>
> Target Port: 2220

## 1. 推荐学习资料

> - Linux在线资源
>   - [菜鸟教程](http://www.runoob.com/linux/linux-tutorial.html)
>   - [Linux在线命令大全手册](http://man.linuxde.net/)
>   - [Linux思维导图整理](https://www.jianshu.com/p/59f759207862)
> - Shell在线资源
>   - [Shell脚本编程30分钟入门](https://github.com/qinjx/30min_guides/blob/master/shell.md)
>   - [Shell命令行分解分析](https://explainshell.com/)
> - 实体书籍
>   - 《鸟哥的Linux私房菜 基础学习篇 第四版》(京东,当当均有售)

## 2. Level 0

### 2.1. 关卡目标

> The password for the next level is stored in a file called **readme** located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

### 2.2. 解决方案

> 注意: 第一次登录会需要验证,输入yes就可以

```shell
$ ssh -p 2220 bandit0@bandit.labs.overthewire.org
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit0@bandit.labs.overthewire.org's password:
```

然后输入题目提供的密码 `bandit0` 就进去了.(以下略)

这关没啥思路,就是 `ls` 和 `cat` 命令最简单的使用.

```shell
$ ls
readme
$ cat readme
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

```
bandit1_password = boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

------

## 3. Level 1

### 3.1. 关卡目标

> The password for the next level is stored in a file called **-**located in the home directory

### 3.2. 解决方案

```shell
$ ls
-
```

换参数继续

```shell
$ ls -l
total 4
-rw-r----- 1 bandit2 bandit1 33 Oct 16 14:00 -
```

这里就要说到Linux的文件属性了.
上面的一行输出,除了`total 4`之外
第一个字符代表这个文件的类型.
具体情况如下:

- `d` = directory 目录
- `-` = 普通文件
- `l` = link file 链接文件
- `b` 装置文件里面的可供储存的接口设备(可随机存取装置)
- `c` 装置文件里面的串行端口设备,例如键盘,鼠标(一次性读取装置)

这里顺便说下一个小知识:

- `.` 代表当前目录
- `..` 代表上一级目录
- `~` 代表用户的home目录
- `/` 代表根目录

所以这个文件名叫`-`的其实是个文件,应该就是我们要找的了.

> 其实我们也可以用推荐命令中的`file`命令查看这个文件的类型.
>
> 比如输入`file ./*`就能列出当前目录下的所有文件类型了.
>
> ```shell
> $ file ./*
> ./-: ASCII text
> ```

而 `-` 在 `Bash` 中是一个特殊符号, 我们用 `cat` 输出这样的文件的时候需要在前面加上`./` , 以表示这是一个文件.

>关于 `-` 方面的知识请上网了解一下**管道和重定向**
>
>这里只说一下, 以下三条命令的结果是一样的
>
>```shell
>$ cat -
>$ cat > /dev/fd/0
>$ cat >&0
>```

```shell
$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```

```
bandit2_password = CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```

## 4. Level 2

### 4.1. 关卡目标

> The password for the next level stored in a file called **spaces in this filename** located in the home directory

### 4.2. 解决方案

```shell
$ ls
spaces in this filename
```

根据题目说这个应该是个普通文件, 那我们不用 `file` 来判断是什么文件了

不过注意使用 `cat` 输出带空格的文件需要加 反斜杠`\` 来转义

```shell
$ cat spaces\ in\ this\ filename
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```

```
bandit3_password = UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```

## 5. Level 3

### 5.1. 关卡目标

> The password for the next level is stored in a hidden file in the **inhere** directory.

### 5.2. 解决方案

```shell
$ ls
inhere
```

根据题目说这个应该是个文件夹, 那我们直接 `cd` 进去看看

里面有个隐藏文件, 用 `ls -a` 就行

```shell
$ cd inhere/
~/inhere$ ls -a
.  ..  .hidden
~/inhere$ cat .hidden
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
```

```
bandit4_password = pIwrPrtPN36QITSp3EQaw936yaFoFgAB
```

## 6. Level 4

### 6.1. 关卡目标

> The password for the next level is stored in the only human-readable
> file in the **inhere** directory. Tip: if your terminal is messed
> up, try the “reset” command.

### 6.2. 解决方案

与上一关类似

```shell
$ ls
inhere
```

```shell
$ cd inhere/
~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
```

有 10 个文件, 题目让我们查找个只有 **human-readable** 的文本文件

那么我们可以用第一关里说到的 `file`

```shell
~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
~/inhere$ cat ./-file07
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
```

```
bandit5_password = koReBOKuIDDepwhWk7jZC0RTdopnAYKh
```

## 7. Level 5

### 7.1. 关卡目标

> The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:
>
> - human-readable
> - 1033 bytes in size
> - not executable

### 7.2. 解决方案

```shell
$ ls
inhere
```

```shell
$ cd inhere/
~/inhere$ ls -l
total 80
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere00
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere01
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere02
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere03
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere04
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere05
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere06
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere07
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere08
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere09
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere10
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere11
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere12
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere13
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere14
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere15
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere16
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere17
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere18
drwxr-x--- 2 root bandit5 4096 Oct 16  2018 maybehere19
```

按照题目给出的信息, 我们得用 `find`

```shell
~/inhere$ find . -type f -size 1033c
./maybehere07/.file2
```

很有趣的是, 只用了第2个属性我们就查找到真正的文件了

不过如果要完整的解法, 应该这样

```shell
$ find . -type f ! -executable -size 1033c | xargs file | grep ASCII
./maybehere07/.file2: ASCII text, with very long lines

~/inhere$ find . -type f ! -executable -size 1033c -exec file {} + | grep ASCII
./maybehere07/.file2: ASCII text, with very long lines
```

```shell
~/inhere$ head -n 1 ./maybehere07/.file2
DXjZPULLxYr17uwoI01bNLQbtFemEgo7
```

```
bandit6_password = DXjZPULLxYr17uwoI01bNLQbtFemEgo7
```

## 8. Level 6

### 8.1. 关卡目标

> The password for the next level is stored **somewhere on the server** and has all of the following properties:
>
> - owned by user bandit7
> - owned by group bandit6
> - 33 bytes in size

### 8.2. 解决方案

```shell
$ ls
```

发现毛都没有

```shell
$ ls -a
.  ..  .bash_logout  .bashrc  .profile
```

结合题目, 我们知道文件存在服务器的某个地方

这里其实跟上一题差不多的, 换下参数就好

```shell
$ find / -user bandit7 -group bandit6 -size 33c
find: ‘/run/lvm’: Permission denied
find: ‘/run/screen/S-bandit15’: Permission denied
find: ‘/run/screen/S-bandit12’: Permission denied
...
```

但是我们发现无访问权限的也会输出出来, 虽然结果并不是很多, 我们很容易就能发现 flag 的位置

但是我们得排除掉错误信息

这里就要用到管道的知识了, 直接把错误信息输出到 `/dev/null` 里

```shell
$ find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
```

```shell
$ cat /var/lib/dpkg/info/bandit7.password
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
```

```
bandit7_password = HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
```

## 9. Level 7

### 9.1. 关卡目标

> The password for the next level is stored in the file **data.txt**
> next to the word **millionth**

### 9.2. 解决方案

```shell
$ ls
data.txt
$ ls -l
total 4088
-rw-r----- 1 bandit8 bandit7 4184396 Oct 16  2018 data.txt
```

文件很大, 我们得用 `grep` 来查找文件里的内容

```shell
$ grep "millionth" data.txt
millionth       cvX2JJa4CFALtqS87jk27qwqGhBM9plV
```

没有什么难度吧

```
bandit8_password = cvX2JJa4CFALtqS87jk27qwqGhBM9plV
```

## 10. Level 8

### 10.1. 关卡目标

> The password for the next level is stored in the file **data.txt**
> and is the only line of text that occurs only once

### 10.2. 解决方案

```shell
$ ls
data.txt
```

跟上一关不一样的是, 这关我们得筛选出只出现过一次的一行文本, 这行文本就是 flag

那么我们得用 `uniq` , 不过注意的是, 因为每行的字符串实际上是乱序的(不相信可以直接使用试试), 用这个之前得用 `sort` 排列一下, 否则 `uniq` 命令只会做相邻行的去重

```shell
$ sout data.txt | uniq
KerqNiDbY0zV2VxnOCmWX5XWxumldlAe
...
```

我们发现还是一大堆东西, 看来每行的文本是去重了, 但是没有输出只出现过一次的一行文本, `uniq` 还要加个参数

```shell
$ sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```

```
bandit9_password = UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```

## 11. Level 9

### 11.1. 关卡目标

> The password for the next level is stored in the file **data.txt**
> in one of the few human-readable strings, beginning with several ‘=’
> characters.

### 11.2. 解决方案

```shell
$ ls
data.txt
```

很明显这关要用到 `grep` 了

我们先看看这是什么文件

```shell
$ file data.txt
data.txt: data
```

跟上一关不同的是这文件内容是二进制文件, 而题目告诉我们有可读性的文本, 所以我们可以先用 `strings` 命令处理下再查找

这里用到了很简单的正则方面的知识, 不懂的可以去补一下相关基础, 这里给一个很不错的练习平台: https://www.regexone.com/

```shell
$ strings data.txt | grep "^=="
========== password
========== isa
========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
```

```
bandit10_password = truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
```

## 12. Level 10

### 12.1. 关卡目标

> The password for the next level is stored in the file **data.txt**,
> which contains base64 encoded data

### 12.2. 解决方案

```shell
$ ls
data.txt
```

让我们使用 `base64` 命令解密 base64 数据

```shell
$ base64 -d data.txt
The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
```

```
bandit11_password = IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
```
