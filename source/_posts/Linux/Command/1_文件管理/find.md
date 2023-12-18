---
title: find
date: 2023-12-13 16:12:28
author: 刘宇亭
category:
    - Linux
    - Command
    - 文件管理
tag:
    - Linux
    - Command
    - 文件管理
---
# find

## 介绍

Linux find 命令用来在指定目录下查找文件。任何未予参数之前的字符串都将被视为与查找的目录名。如果使用该命令时，不设置任何参数，则 `find` 命令将在当前目录下查找子目录与文件。并且将查找到的子目录和文件全部进行显示。

## 语法

```shell
find path -option [  -print] [ -exec -ok command] {} \;
```

### 参数：

find 根据下列规则判断 `path` 和 `expression` ，在命令列上第一个 `-(),!` 之前的部分为 `path` ，之后的部分为 `expression` 。如果 `path` 是空字符串则使用目前的路径，如果 `expression` 是空则使用 `-print` 为预设的 `expression` 。

`expression` 中可使用的选项有二三十个之多。（这里只介绍常用的）

- -mount，-xdev：只检查和指定目录在同一个文件系统下的文件，避免列出其他文件系统中的文件。
- -amin n：在过去n分钟内容被读取过。
- -anewer file：比文件 file 更晚读取过的文件。
- -atime n：再过去n天内容被度去过的文件。
- -cmin n：在过去n分钟内被修改过。
- -cnewer file：比文件 file 更新的文件。
- -ctime n：再过去n天内被修改过的文件。
- -empty：空文件 -gid n or -group name：gid是n或是group名称是name。
- -ipath p, -path p：路径名称符合p的文件，ipath会忽略大小写。
- -name name, -iname name：文件名称符合name的文件，iname会忽略大小写。
- -size n：文件大小是n单位，b代表512位元组的区块，c表示字元数，k表示kilo bytes, w是两个位元组。
- -type c：文件类型是c的文件。

1. d：目录
2. c：字型装置文件
3. b：区块装置文件
4. p：具名贮列
5. f：一般文件
6. l：符号连接
7. s：socket

- -pid n：process id 是 n 的文件。

可以使用 `()` 将运算符分隔，并使用下列运算。

```shell
exp1 -and exp2
!expr
-not expr
exp1 -or exp2
exp1,exp2
```

## 实例

将当前，目录及其子目录下所有文件后缀为 `.c` 的文件列出来：

```shell
find . -name "*.c"
```

将当前目录及其子目录下所有文件列出：

```shell
find . -type f
```

将当前目录及其子目录下所有最近20天内更新过的文件列出：

```shell
find . -ctime -20
```

查找 /var/log 目录中更改时间在7天以前的普通文件，并在删除他们之前询问他们：

```shell
find /var/log -type f -ctime +7 -ok rm {} \;
```

查找当前目录中文件属主具有读、写权限的，并且文件所属组的用户和其他用户具有读权限的文件：

```shell
find . -type f -perm 644 -exec ls -l {} \;
```

查找系统中所有文件长度为 0 的普通文件，并列出他们的完整路径：

```shell
find / -type f -size 0 -exec ls -l {} \;
```
