---
title: split
date: 2024-01-13 14:36:52
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
# split

split命令用于将一个文件分割成多个。该指令将大文件分割成较小的文件，在默认情况下将按每1000行切割成一个小文件。

## 语法

```shell
$ split [--help][--version][-<行数>][-b <字节>][-C <字节>][要切割的文件][要输出的文件]
```

### 参数

- -<行数>：指定每多少行切成一个小文件。
- -b <字节>：指定每多少字节切成一个小文件。
- --help：在线帮助。
- --version：显示版本信息。
- -C <字节>：与参数"-b"相似，区别在于-C在切割的时候将尽量维持每行的完整性。
- [要输出的文件]：设置切割后文件的前置文件名，split会自动在前置文件名后再加上编号。

## 实例

```bash
# 使用指令"split"将文件"README"每6行切割成一个文件
$ split -6 README
# 命令执行后"split"会将原来的大文件"README"切割成多个以"x"开头的小文件。而在这些小文件中，每个文件都只有6行内容
# 使用指令"ls"查看当前目录结构
$ ls
# >>> README xaa xad xag xab xae xah xac xaf xai
```
