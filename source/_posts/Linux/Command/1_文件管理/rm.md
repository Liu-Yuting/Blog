---
title: rm
date: 2024-01-10 17:54:30
cover: true
coverImg: /medias/images/01.jpg
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
# rm

## 介绍

rm英文全拼：remove用于删除一个文件或者目录。

## 语法

```shell
rm [options] name ...
```

### 参数

- -i 删除前逐一询问确认。
- -f 使原档案属性设置为只读，亦直接删除，无需逐一确认。
- -r 将目录及以下之档案亦逐一删除。

## 实例

删除文件可以直接使用rm命令，若删除目录必须配合选项 "-r"，例如：

```shell
# rm test.txt
rm：是否删除 一般文件 "test.txt"? y  
# rm  homework  
rm: 无法删除目录"homework": 是一个目录  
# rm  -r  homework  
rm：是否删除 目录 "homework"? y 
```

删除当前目录下的所有文件及目录，命令行为：

```shell
rm -r *
```

文件一旦通过删除名令，则无法恢复，所以必须格外小心的使用该命令。

## 笔记

```shell
# 删除当前目录下的所有文件及目录，并且是直接删除无需逐一确认命令行为：
rm -rf "要删除的文件名或目录"
# 删除文件名 test.txt
rm -rf test.txt
# 删除目录test，不管该目录下是否有子目录或文件，都直接删除
rm -rf test/
```

