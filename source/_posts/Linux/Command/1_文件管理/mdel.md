---
title: mdel
date: 2023-12-24 12:24:00
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
# mdel

mdel命令用来删除MSDOS格式的档案。在删除只读之前会有提示信息产生。

## 语法

```shell
$ mdel [-v] msdosfile [msdosfiles ...]
```

### 参数

- -v：显示更多讯息。

## 实例

```shell
# 将A槽磁片根目录中的autoexec.bat删除
$ mdel a:autoexec.bat .
```

