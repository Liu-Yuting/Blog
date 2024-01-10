---
title: mattrib
date: 2023-12-21 12:21:00
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
# mattrib

mattrib命令用来变更或显示MS-DOS文件的属性。mattrib为mtools工具命令，模拟MS-DOS的attrib指令，可变更MS-DOS文件的属性。

## 语法

```shell
$ mattrib [-a|+a][-h|+h][-r|+r][-s|+s][-/][-X] msdosfile [msdosfiles ...]
```

### 参数

- -a | +a：[除去 | 设定] 备份属性。
- -h | +h：[除去 | 设定] 隐藏属性。
- -r | +r：[除去 | 设定] 只读属性。
- -s | +s：[除去 | 设定] 系统属性。
- -/：递归处理包含所有子目录下的档案。
- -X：以较短的格式输出结果。

## 实例

```shell
# 列出A槽MSDOS格式磁片上所有文件的属性
$ mattrib a:
# 除去A槽MSDOS格式磁片上msdos.sys档案的隐藏、系统与只读属性
$ mattrib -h -s -r a:msdos.sys
# 除去A槽MSDOS格式磁片上包含子目录下所有档案的只读属性
$ mattrib -r -/ a:*.*
```
