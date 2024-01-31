---
title: whereis
date: 2024-01-19 16:59:51
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
# whereis

Linux whereis命令用于查找文件。该指令会在特定目录中查找符合条件的文件。这些文件应属于原始代码、二进制文件或是帮助文件。该指令只能用于查找二进制文件、源代码文件和man手册页，一般文件的定位需要使用locate命令。

## 语法

```bash
$ whereis [-bfmsu][-B <目录>...][-M <目录>...][-s <目录>...][文件...]
```

### 参数说明

- -b：只查找二进制文件。
- -f：不显示文件名前的路径名称。
- -m：只查找说明文件。
- -s：只查找原始代码文件。
- -u：查找不包含指定类型的文件。
- -B <目录>：只在设置的目录下查找二进制文件。
- -M <目录>：只在设置的目录下查找说明文件。
- -S <目录>：只在设置的目录下查找原始代码文件。

## 实例

```bash
# 使用指令"whereis"查看指令"bash"的位置：
$ whereis bash
# >>> bash:/bin/bash/etc/bash.bashrc/usr/share/man/man1/bash.1.gz 
```

**注意**：以上输出信息从左至右分别为查询的程序名、bash路径、bash的man手册路径。

```bash
# 如果用于需要单独查询二进制文件或帮助文件：
$ whereis -b bash  # 显示bash命令的二进制程序
$ whereis -m bash  # 显示bash命令的帮助文件
# >>> bash: /bin/bash /etc/bash.bashrc /usr/share/bash
# >>> bash: /usr/share/man/man1/bash.1.gz
```
