---
title: rcp
date: 2024-01-07 17:52:09
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
# rcp

rcp命令用于复制远程文件或目录。rcp命令用在远端复制文件或目录，如同时指定两个以上的文件或目录且最后的目的地是一个已经存在的目录则它会把前面指定的所有文件或目录复制到该目录中。

## 语法

```shell
$ rcp [-pr][源文件或目录][目标文件或目录]
# 或
$ rcp [-pr][源文件或目录...][目标文件]
```

### 参数

- -p：保留源文件或目录的属性，包括拥有者、所属组群、权限和时间。
- -r：递归处理，将指定目录下的文件与子目录一并处理。

## 实例

```shell
# 使用rcp指令复制远程文件到本地进程保存。假设本地主机当前帐户root，远程主机账户为admin，要将远程主机（210.6.132.50）目录下的文件"testfile"复制到本地目录"test"中
$ rcp admin@210.6.132.50:./testfile testfile  # 复制远程文件到本地
$ rcp admin@210.6.132.50:home/root/testfile testfile
$ rcp 210.6.132.5:./testfile testfile
```

**注意**：指令"rcp"执行以后不会有返回信息，仅需要在目录"test"下查看是否存在文件"testfile"。若存在，则表示远程复制操作成功，否则远程复制操作失败。
