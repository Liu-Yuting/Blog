---
title: mv
date: 2024-01-03 17:45:20
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
# mv

## 介绍

Linux mv (英文全拼：move file)命令用来为文件或目录改名、或将文件或目录移入其他位置。

## 语法

```shell
mv [options] source dest
mv [options] source... directory
```

### 参数说明：

- -b：当目标文件或目录存在时，在执行覆盖前，会为其创建一个备份。
- -i：如果指定移动的源目录或文件与目标的目录或文件同名，则会先询问是否覆盖旧文件，输入 `y` 表示直接覆盖，输入 `n` 表示取消该操作。
- -f：如果指定移动的源目录或文件与目标的目录或文件同名，不会询问，直接覆盖旧文件。
- -n：不要覆盖任何已经存在的文件或目录。
- -u：党员文件比目标文件新或者目标文件不存在时，才执行移动操作。

mv 参数设置与运行结果

| 命令格式                                       | 运行结果                                                     |
| ---------------------------------------------- | ------------------------------------------------------------ |
| mv source_file(文件) dest_file(文件)           | 将源文件名 `source_file` 改为目标文件名 `dest_file` 。       |
| mv source_file(文件) dest_directory(目录)      | 将文件 `source_file` 移动到目标目录 `dest_directory` 中。    |
| mv source_directory(目录) dest_directory(目录) | 目录名 `dest_directory` 已存在，将 `source_directory` 移动到目录名 `dest_directory` 中；目录名 `dest_directory` 不存在则 `source_directory` 改名为目录名 `dest_directory` 。 |
| mv source_directory(目录) dest_file(文件)      | 出错                                                         |

## 实例

将文件 `aaa` 改名为 `bbb` ：

```shell
mv aaa bbb
```

将 `info` 目录放入 `logs` 目录中。注意：如果 `logs` 目录不存在，则该命令将 `info` 改名为 `logs` 。

```shell
mv info/ logs
```

再如将 `/usr/runoob` 下的所有文件和目录移动到当前目录下，命令行为：

```shell
mv /usr/* .
```

## 笔记

mv 操作文件时是移动并且重命名。

目标目录与原目录一致，制动了新文件名，效果就是仅仅重命名。

```shell
mv /home/liuyt/a.txt /home/liuyt/b/txt
```

目标目录与原目录不一致，没有指定新文件名，效果就是仅仅移动。

```shell
mv /home/liuyt/a.txt /home/liuyt/test/
# 或
mv /home/liuyt/a.txt /home/liuyt/test
```

目标目录与原目录一致，指定了新文件名，效果就是：移动 + 重命名。

```shell 
mv /home/liuyt/a.txt /home/liuyt/test/b.txt
```

批量移动文件和文件夹：（Ubuntu 18.04 奏效）

例如，将 `/home/liuyt/` 目录里面的所有文件 `&` 文件夹 挪到 `/home/liuxx/` 

```shell 
mv /home/liuyt/* /home/liuxx/
```

注意：需要先执行显示隐藏文件命令，否则，隐藏文件以及隐藏文件夹不会被移动到新目录。

英语点号开头的文件会被作为隐藏文件处理，英语点号开头的文件夹也被作为隐藏文件夹处理。

```shell
# 例如：文件 .a.txt 目录 .tp5
```
