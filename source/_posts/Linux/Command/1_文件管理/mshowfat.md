---
title: mshowfat
date: 2023-12-31 12:31:00
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
# mshowfat

Linux mshowfat命令用于显示MS-DOS文件在FAT中的记录。mshowfat为mtools工具指令，可显示MS-DOS文件在FAT中的记录编号。

## 语法

```bash
$ mshowfat [文件...]
```

### 参数

- [文件 ...]：执行操作的文件相对路径或绝对路径。

## 实例

```bash
# 使用指令mshowfat查看文件"autorun.bat"的FAT信息：
$ mshowfat autorun.bat
# 以上命令执行后，文件"autorun.bat"的FAT相关信息将会被显示出来。
```

**注意**：执行操作的文件必须是DOS文件系统下的文件。