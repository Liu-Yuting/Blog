---
title: chown
date: 2023-12-05 15:28:32
cover: true
coverImg: /medias/images/03.jpg
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
# chown

Linux chown（英文全拼：change owner）命令用于设置文件所有者和文件关联组的命令。Linux/Unix是多人多工操作系统，所有的文件皆有拥有者。利用chown将指定文件的拥有者改为指定的用户或组，用户可以是用户名或者用户ID，组可以是组名或者组ID，文件是以空格分开的要改变权限的文件列表，支持通配符。

chown需要`root`权限才能执行此命令。只用超级用户和属于组的文件所有者才能变更文件关联组。非超级用户如果需要设置关联组可能需要使用[chgrp](./chgrp.md)命令。

## 语法

```shell
chown [-cfhvR][--help][--version] <用户>[:<组>] <文件>
```

## 参数说明

- 用户: 新的文件拥有者或拥有者ID。
- 组: 新的文件拥有者所在的组名称或ID。
- -c: 显示更改部分的信息。
- -f: 忽略错误信息。
- -h: 修复符号链接。
- -v: 显示详细的处理信息。
- -R: 处理指定目录以及其子目录下的所有文件。
- --help: 显示辅助说明。
- --version: 显示版本信息。

## 实例

```shell
# 把 /var/run/httpd.pid 的所属者设置成root
$ chown root /var/run/httpd.pid
# 将文件 file.txt 的拥有者设置为admin, 群体的使用者设置成adminGroup
$ chown admin:adminGroup file.txt
# 将当前目录下的所有文件与子目录的拥有者皆设置为admin，群体的使用者adminGroup
$ chown -R admin:adminGroup *
# 把 /home/admin 的关联组设置为512(关联组ID)，不改变所有者
$ chown :512 /home/admin
```

