---
title: paste
date: 2024-01-05 17:47:22
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
# paste

paste命令用于合并文件的列。paste指令会把每个文件以列对列的方式，一列列地加以合并。

## 语法

```shell
$ paste [-s][-d <间隔字符>][--help][--version][文件]
```

### 参数

- -d <间隔字符> 或 --delimiters=<间隔字符>：用指定的间隔字符取代跳格字符。
- -s 或 --serial：串列进行而非平行处理。
- --help：在线帮助。
- --version：显示版本信息。
- [文件]：指定操作的文件路径。

## 实例

```shell
# 使用paste指令将文件"file"、"testfile"、"testfile1"进行合并
$ paste file testfile testfile1
# 但是，在执行以上命令之前，首先使用"cat"指令对3个文件内容进行查看
$ cat file
# >>> xiongdan 200
# >>> lihaihui 233
# >>> lymlrl 231
$ cat testfile
# >>> liangyuanm ss
$ cat testfile1
# >>> huanggai 56
# >>> zhixi 73
# 当合并指令"$ paste file testfile testfile2"执行后，程序界面中将显示合并后的文件内容
# >>> xiongdan 200
# >>> lihaihui 233
# >>> lymlrl 231
# >>> liangyuanm  ss
# >>> huanggai 56
# >>> zhixi 73
# 若使用paste指令的参数"-s"，则可以将一个文件中的多行数据合并为一行进行显示
# 例如：将文件"file"中的三行数据合并为一行数据进行显示
$ paste -s file
# >>> xiongdan 200 lihaihui 233 lymlrl
# 注意：参数"-s"只是将testfile文件的内容调整显示方式并不会改变源文件的内容格式
```

## 笔记

按行合并，即数据一行一行拼接，用cat；按列合并，则用paste。

```shell
$ more ts1                   # 查看文件ts1
# >>> 1
# >>> 2
$ more ts2                   # 查看文件ts2
# >>> cat
# >>> paste
$ cat ts1 ts2                # 按行合并
# >>> 1
# >>> 2
# >>> cat
# >>> paste
$ paste ts1 ts2              # 按列合并
# >>> 1 cat
# >>> 2 paste
$ cat ts1 ts2 > new_row.txt  # 生成新的文件new_row.txt
$ paste ts1 ts2 > new_col    # 生成新的文件new_col，文件格式一般为.txt，在Linux中可不加，因为系统可以识别不加.txt的文件
# 合并n个文件通常使用
$ cat * > new_file           # 合并当前目录下的所有文件
```