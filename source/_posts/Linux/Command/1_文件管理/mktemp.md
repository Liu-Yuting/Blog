---
title: mktemp
date: 2023-12-26 12:26:00
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
# mktemp

mktemp命令用于建立暂存文件。mktemp建立的一个暂存文件，供shell script使用。

## 语法

```shell
$ mktemp [-qu][文件名称参数]
```

### 参数

- -q：执行时若发生错误，不会显示任何信息。
- -u：暂存文件会在mktemp结束前先行删除。
- [文件名参数]：文件名参数必须是以“自定义名称.xxxx”的格式。

## 实例

```shell
# 使用mktemp命令生成临时文件时，文件名参数应当以"文件名.xxx"的形式给出，mktemp会根据文件名参数建立一个临时文件，输入如下
$ mktemp tmp.xxxx   # 生成临时文件
# 使用该命令后，可使用dir或ls查看当前目录
$ mktemp tmp.xxxx   # 生成临时文件
$ dir               # 查看当前目录
# >>> *** tmp.3847  # 生成了tmp.3847（***是其他文件名）
# 由此可见，生成的临时文件为tmp.3847，其中，文件名参数中的xxxx被4个随机产生的字符所取代。
```
