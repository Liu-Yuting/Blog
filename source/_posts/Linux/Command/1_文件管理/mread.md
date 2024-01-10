---
title: mread
date: 2023-12-29 12:29:00
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
# mread

mread命令用于将MS-DOS文件复制到Linux/Unix目录中。mread为mtools工具命令，可将MS-DOS文件复制到Linux的文件系统中。这个命令目前不经常用，一般都是用mcopy命令来代替。

## 语法

```shell
$ mread [MS-DOS文件][Linux文件或目录]
```

### 参数

- [MS-DOS文件]：执行操作的DOS源文件或目录路径。
- [Linux文件或目录]：执行操作后的Linux目标文件或目录路径。

## 实例

```shell
# 使用指令mread将盘"a:\"中的所有内容复制到当前工作目录下
$ mread a:\* ./
# 执行上述命令前，可以先试用mdir命令查看原来的目录结构。执行mread之后，可使用ls命令再次查看复制之后的文件结构
$ mdir -/ a:\*                           # 查看a盘中的文件
# >>> Volume in drive A has no label     # 加载信息
# >>> Volume Serial Number is 13D2~055C
# >>> Directory for A:/                  # 以下为目录信息
# >>> ./TEST <DIR> 2011-08-23 16:59      # 显示格式为文件名，目录大小，修改时间
# >>> AUTORUN.INF 265 2011-08-23 16:53
# >>> AUTORUN.BAT 43 2011-08-23 16:56
# >>> 3 files 308 bytes                  # 统计总大小
# >>> 724 325 bytes free                 # 剩余空间
$ mread A:\* ./                          # 将a盘上所有文件复制到当前工作目录
$ ls                                     # 查看文件或子目录信息
# >>> TEST AUTORUN.INF AUTORUN.BAT       # 显示复制后的内容
```
