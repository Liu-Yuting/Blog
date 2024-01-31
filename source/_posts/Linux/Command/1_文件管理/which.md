---
title: which
date: 2024-01-20 17:00:05
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
# which

Linux which命令用于查找文件。which指令会在环境变量$PATH设置的目录里查找符合条件的文件。

## 语法

```bash
$ which [-wV][-n<文件名长度>][-p<文件名长度>][文件。。。]
```

### 参数

- -n <文件名长度>：是定文件名长度，指定的长度必须大于或等于所有文件中最长的文件名。
- -p <文件名长度>：与-n参数相同，但此处的<文件名长度>包括了文件的路径。
- -w：指定输出时栏位的宽度。
- -V：显示版本信息。

## 实例

```bash
# 使用指令"which"查看指令"bash"的绝对路径：
$ which bash
# >>> /bin/bash  # 可执行程序的绝对路径
```
