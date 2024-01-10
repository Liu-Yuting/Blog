---
title: ln
date: 2023-12-18 16:17:16
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
# ln

## 介绍

Linux ln（英文全拼：link files）命令是一个非常重要的命令，它的功能是为某一个文件在另外一个位置建立一个同步的链接。如果我们需要在不同的目录，用到相同的文件时，我们不需要在每一个需要的目录下面都放一个必须相同的文件，我们只要在某一个固定的目录，放上该文件，然后其他的目录下用ln命令链接 `link` 它就可以，不必重复的占用磁盘空间。

## 语法

```shell
ln [参数] [源文件或目录] [目标文件或目录]
```

其中参数格式为：

```shell
[-bdfinsvF] [-S backup-suffix] [-V {numbered, existing, simple}] [--help] [--version] [--]
```

### 命令功能：

Linux文件系统中，有所谓的链接 `link` ，我们可以将其是为档案的别名，而链接又可以分为两种：硬链接（hard link）与软连接（symbolic link），硬链接的意思是一个档案可以有多个名称，而软连接的方式则是产生一个特殊的档案，该党案的内容是指向另一个档案的位置。硬链接是存在同一个文件系统中，而软连接却可以跨越不同的文件系统。

不论是硬链接还是软链接都不会将原本的档案复制一份，只会占非常少量的磁盘空间。

### 软连接：

- 软连接，一路进的形式存在。类似于windows操作系统中的快捷方式。
- 软连接可以跨文件系统，硬链接不可以。
- 软连接可以对一个不存在的文件名进行连接。
- 软连接可以对目录进行连接。

### 硬链接：

- 硬链接，以文件副本的形式存在。但不占用实际空间。
- 不允许给目录创建硬链接。
- 硬链接只有在同一个文件系统中才能创建。

## 命令参数

### 必要参数：

- -b 删除，覆盖以前建立的链接。
- -d 允许超级用户制作目录的硬链接。
- -f 强制执行。
- -i 交互模式，文件存在则提示用户是否覆盖。
- -n 把符号链接视为一般目录。
- -s 软连接（符号连接）。
- -v 显示详细的处理过程。

### 选择参数：

- -S "-S<字尾备份字符串>" 或 "--suffix=<字尾备份字符串>"。
- -V "-V<备份方式>" 或 "--version-control=<备份方式>"。
- --help 显示帮助信息。
- --version 显示版本信息。

## 实例

给文件创建软连接，为log2022.log文件创建软连接link2022，如果log2022丢失，link2022将失效：

```shell
ln -s log2022.log link2022
# 输出
[root@localhost test]# ll
-rw-r--r-- 1 root bin      61 11-13 06:03 log2022.log
[root@localhost test]# ln -s log2022.log link2022
[root@localhost test]# ll
lrwxrwxrwx 1 root root     11 12-07 16:01 link2022 -> log2022.log
-rw-r--r-- 1 root bin      61 11-13 06:03 log2022.log
```

给文件创建硬链接吗，为log2022.log创建硬链接为ln2022，log2022.log与ln2022的各项属性相同：

```shell
ln log2022.log ln2022
# 输出
[root@localhost test]# ll
lrwxrwxrwx 1 root root     11 12-07 16:01 link2022 -> log2022.log
-rw-r--r-- 1 root bin      61 11-13 06:03 log2022.log
[root@localhost test]# ln log2022.log ln2022
[root@localhost test]# ll
lrwxrwxrwx 1 root root     11 12-07 16:01 link2022 -> log2022.log
-rw-r--r-- 2 root bin      61 11-13 06:03 ln2022
-rw-r--r-- 2 root bin      61 11-13 06:03 log2022.log
```
