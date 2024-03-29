---
title: mdir
date: 2023-12-25 12:25:00
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
# mdir

mdir命令用于显示MS-DOS目录。mdir为mtools工具指令，模拟MS-DOS的dir指令，可显示MS-DOS文件系统中的目录内容。

## 语法

```shell
$ mdir [-afwx/][目录]
```

### 参数

- -/：显示目录下所有子目录与文件。
- -a：显示隐藏文件。
- -f：不显示磁盘所剩余的可用空间。
- -w：仅显示目录或文件名称，并以横排方式呈现，以便一次能显示较多的目录或文件。
- -x：仅显示目录下多有子目录与文件的完整路径，不显示其它信息。

## 实例

```shell
# 显示A盘中的内容
$ mdir -/ a:\*
# 上述命令执行后，mdir将显示指定盘"a:\"中的所有子目录及其中的文件信息
# >>> Volume in drive A has no label     # 加载信息
# >>> Volume Serial Number is 13D2~055C
# >>> Directory for A:\                  # 以下为目录信息
# >>> ./TEST <DIR> 2011-08-23 16:59      # 显示格式为文件名，目录大小，修改时间
# >>> AUTORUN.INF 265 2011-08-23 16:53
# >>> AUTORUN.BAT 43 2011-08-23 16:56
# >>> 3 files 308 bytes                  # 统计总大小  
# >>> 724 325 bytes free                 # 剩余空间
```
