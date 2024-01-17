---
title: touch
date: 2024-01-16 14:37:43
cover: true
coverImg: /medias/images/02.jpg
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
# touch

Linux touch命令用于修改文件或者目录的时间属性，包括存取时间和更改时间。若文件不存在，系统会创建一个新文件。`ls -l`可以显示档案的时间记录。

## 语法

```bash
$ touch [-acfm][-d<日期时间>][-r<参考文件或目录>][-t<日期时间>][--help][--version][文件或目录。。。]
```

### 参数说明

- -a：改变档案的读取时间记录。
- -m：改变档案的修改时间记录。
- -c：假如目的档案不存在，不会建立新的档案。与--no-create的效果一样。
- -f：不使用，是为了与其它unix系统的相容性而保留。
- -r：使用参考档案的时间记录，与--file的效果一样。
- -d：设定时间与日期，可以使用各种不同的格式。
- -t：设定档案的时间记录，格式与date指令相同。
- --no-create：不会建立新档案。
- --help：在线帮助。
- --version：版本信息。

## 实例

```bash
# 使用指令"touch"修改文件"testfile"的时间属性为当前系统时间：
$ touch testfile  # 修改文件的时间属性
# 首先，使用ls命令查看testfile文件的属性：
$ ls -l testfile  # 查看文件的时间属性
# >>> -rw-r--r--    1 root     root             0 Dec 18 16:41 testfile
# 执行指令"touch"修改文件属性以后，再次查看该文件的时间属性：
$ touch testfile  # 修改文件的时间属性为当前系统时间
$ ls -l testfile  # 查看文件的时间属性
# >>> -rw-r--r--    1 root     root             0 Dec 18 16:44 testfile
# 使用指令"touch"时，如果指定的文件不存在，则将创建一个新的空白文件。例如，在当前目录下，使用该指令创建一个空白文件"file"：
$ touch file
```
