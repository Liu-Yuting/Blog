---
title: cp
date: 2023-12-08 15:06:07
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
# cp

## 介绍

cp（英文全拼：copy file）命令主要用于复制文件或目录。

## 语法

```shell
cp [options] source dest
```

或

```shell
cp [options] source... directory
```

### 参数说明

- -a：通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于dpR参数组合。
- -d：复制时保留链接。这里所说的链接相当于Windows系统中的快捷方式。
- -f：覆盖已经存在的目标文件而不给出提示。
- -i：与 `-f` 选项相反，在覆盖目标文件之前给出提示，要求用户确认是否覆盖，回答 `y` 时目标文件将会覆盖。
- -p：除复制文件的内容外，还把修改时间和访问权限也复制到新文件中。
- -r：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
- -l：不复制文件，只是生成链接文件。

### 实例

使用指令 `cp` 将当前目录 test/ 下的所有文件复制到新目录newtest下，输入：

```shell
$ cp -r test/ newtest
```

注意：用户使用该指令复制目录时，必须使用参数 `-r` 或者 `-R`。

## 笔记

Linux讲一个文件夹的所有内容拷贝到另外一个文件夹

cp命令使用 `-r` 参数可以将 packageA 下的所有文件拷贝到 packageB 中：

```shell
cp -r /home/packageA/* /home/cp/packageB/
```

将一个文件夹复制到另一个文件夹下，以下实例 packageA 文件会拷贝到 packageB 中：

```shell
cp -r /home/packageA /home/packageB
```

运行命令之后 packageB 文件夹下就有 packageA 文件夹了。
