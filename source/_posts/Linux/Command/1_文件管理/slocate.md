---
title: slocate
date: 2024-01-12 14:36:34
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
# slocate

slocate命令查找文件或目录。slocate本身具有一个数据库，里面存放了系统中文与目录的相关信息。

## 语法

```shell
$ slocate [-u][--help][--version][-d <目录>][查找的文件]
```

### 参数

- -d <目录> 或 --database=<目录>：指定数据库所在的目录。
- -u：更新slocate数据库。
- --help：显示帮助。
- --version：显示版本信息。

## 实例

```shell
# 使用指令"slocate"显示文件名中含有关键字"fdisk"的文件路径信息
$ slocate fdisk
# >>> /root/cfdisk        #搜索到的文件路径列表
# >>> /root/fdisk
# >>> /root/sfdisk
# >>> /usr/include/grub/ieee1275/ofdisk.h
# >>> /usr/share/doc/util-Linux/README.cfdisk
# >>> /usr/share/doc/util-Linux/README.fdisk.gz
# >>> /usr/share/doc/util-Linux/examples/sfdisk.examples.gz
```
