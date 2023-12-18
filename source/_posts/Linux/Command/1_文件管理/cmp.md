---
title: cmp
date: 2023-12-07 16:30:57
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
# cmp

Linux cmp命令用于比较两个文件是否有差异。当相互比较的两个文件完全一样时，则该指令不会显示任何信息。若发现有所差异，预设会标示出第一个不同之处的字符和列数编号。若不指定任何文件名称或是所给予的文件名为"-"，则cmp指令会从标准输入设备读取数据。

## 语法

```shell
cmp [-clsv][-i <字符数目>][--help][第一个文件][第二个文件]
```

## 参数

- -c 或 --print-chars: 除了表明差异处的十进制字码之外，一并显示该字符所对应字符。
- -i <字符数目> 或 --ignore-initial=<字符数目>: 指定一个数目。
- -l 或 --verbose: 标示出所有不一样的地方。
- -s 或 --quiet 或 --silent: 不显示错误信息。
- -v 或 -- version: 版本信息。
- --help: 帮助信息。

## 实例

```shell
# 确定两个文件是否相同。
$ cmp prog.o.bak prog.o
# 如果文件相同，则不会显示消息。如果文件不同，则显示第一个不同的位置。
$ >>> prog.o.bak prog.o differ: char 4, line 1 
# 如果显示消息 cmp: EOF on prog.o.bak，则prog.o的第一部分与prog.o.bak相同，但在prog.o中还有其他数据。
```

