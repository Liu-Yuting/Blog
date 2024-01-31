---
title: umask
date: 2024-01-17 14:37:57
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
# umask

Linux umask命令指定在建立文件时预设权限掩码。umask可用来设定[权限掩码]。[权限掩码]是由3个八进制的数字所组成，将现有的存取权限减掉权限掩码后，即可产生建立文件时预设的权限。

## 语法

```bash
$ umask [-S][权限掩码]
```

### 参数说明

- -S：以文字的方式来表示权限掩码。

## 实例

```bash
# 使用指令"umask"查看当前权限掩码：
$ umask
# >>> 0022
# 接下来，使用指令"mkdir"创建一个目录，并使用指令"ls"获取该目录的详细信息：
$ mkdir test1      # 创建目录
$ ls -d -l test1/  # 显示目录的详细信息
# >>> drwxr-xr-x    2 root     root          4096 Dec 18 16:55 test1/
```

**注意**：在上面的输出信息中，"drwxr-xr-x"="777-022=755"。
