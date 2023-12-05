---
title: chgrp
date: 2023-12-03 16:32:19
author: 刘宇亭
cover: true
coverImg: /medias/images/03.jpg
category:
    - Linux
    - Command
    - 文件管理
tag:
    - Linux
    - Command
    - 文件管理
---
# chgrp

Linux chgrp （英文全拼：change group）命令用于变更文件或目录的所属群组。与[chown]()命令不同，chgrp允许普通用户改变文件所属的组，只要该用户是该组的一员。在UNIX系统家族里，文件或目录权限的掌控以拥有者及所属群组来管理。可以使用chgrp指令去变更文件与目录的所属群组，设置方式采用群组名或群组识别码皆可。

## 语法

```shell
chgrp [-cfhRv][--help][--version][所属群组][文件或目录] 或 chgrp [-cfhRv][--help][--reference=<参考文件或目录>][--version][文件或目录]
```

## 参数说明

- -c 或 --changes: 效果类似"-v"参数，但仅汇报更改的部分。
- -f 或 --quiet 或 --silent: 不显示错误信息。
- -h 或 --no-dereference: 只对符号连接的文件作修改，而不改动其他任何相关文件。
- -R 或 --recursive: 递归处理，将指定目录下的所有文件及子目录一并处理。
- -v 或 --verbose: 显示命令行执行过程。
- --help: 在线帮助。
- --reference=<参考文件或目录>: 把指定文件或目录的所属群组全部设成和参考文件或目录的所属群组相同。
- --version: 显示版本信息。

## 实例

```shell
# 1、改变文件的群组属性
$ chgrp -v bin <文件名>
# 输出(使用别人的，自己没有通过)
$ ll
$ >>> ---xrw-r-- 1 root root 302108 11-13 06:03 log2012.log
$ chgrp -v bin log2012.log
# "log2012.log" 的所属组已更改为 bin
$ ll
$ >>> ---xrw-r-- 1 root bin  302108 11-13 06:03 log2012.log
# 说明：将log2012.log文件由root群组改为bin群组。

# 2、根据指定文件改变文件的群组属性
chgrp --regerence=log2012.log log2013.log
# 输出
$ ll
$ >>> ---xrw-r-- 1 root bin  302108 11-13 06:03 log2012.log
$ >>> -rw-r--r-- 1 root root     61 11-13 06:03 log2013.log
$ chgrp --reference=log2012.log log2013.log
$ ll
$ >>> ---xrw-r-- 1 root bin  302108 11-13 06:03 log2012.log
$ >>> -rw-r--r-- 1 root bin      61 11-13 06:03 log2013.log
# 说明：改变文件log2013.log的群组属性，使得文件log2013.log的群组属性和参考文件log2012.log的群组属性相同。
```
