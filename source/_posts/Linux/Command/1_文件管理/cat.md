---
title: cat
date: 2023-12-01 09:34:02
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
# cat

cat（英文全拼：concatenate）命令用于连接文件并打印到标准输出设备上。

## 使用权限

所有使用者

## 语法格式

```shell
cat [-AbeEnstTuv] [--help] [--version] fileName
```

### 参数说明：

- -n 或 --number：由1开始对所有输出的行数编号。
- -b 或 --number-nonblank：和 `-n` 相似，只不过对于空白行不编号。
- -s 或 --squeeze-blank：当遇到有连续两行以上的空白行，就换为一行的空白行。
- -v 或 --show-nonprinting：使用 `^`和 `M-` 符号，除了 `LFD` 和 `TAB` 之外。
- -E 或 --show-ends：在每行结束处显示 `$` 。
- -T 或 --show-tabs：将 `TAB` 字符显示为 `^I` 。
- -A 或 --show-all：等价于 `-vET` 。
- -e ：等价于 `-vE` 选项。
- -t ：等价于 `-vT` 选项。

## 实例

把textfile的文档内容加上行号后输入到 filetext 文档里：

```shell
cat -n textfile > filetext
```

将textfile和filetext的文档内容加上行号（空白行不加）之后将内容附加到text文件中：

```shell
cat -b textfile filetext > text
```

清空 /etc/text.txt 文件内容：

```shell
cat /dev/null > /etc/text.txt
```

cat也可以用来制作镜像文件。例如要制作软盘的镜像文件，将软盘放好后输入：

```shell
cat /dev/fd0 > OUTFILE
```

相反，如果想把image file写道软盘，输入：

```shell
cat IMG_FILE > /dev/fd0
```

### 注：

- 1、OUTFILE指输出的镜像文件名。
- 2、IMG_FILE指镜像文件名。
- 3、若从镜像文件写会device时，device容量需与镜像相当。
- 4、通常用制作开机磁片。

## 笔记

dev/null：在类Unix系统中，/dev/null称空设备，是一个特殊的设备文件，它丢弃一切写入其中的数据(但报告写入操作成功)，读取它则会立即得到一个EOF。

使用 `cat $filename > /dev/null` 则不会得到任何信息，因为我们将本来通过标准输出显示的文件信息重定向到了 `/dev/null` 中。

使用 `cat $filename 1 > /dev/null ` 也会得到同样的效果，因为默认重定向的1 就是标准输出。如果你对 shell 脚本或者重定向比较熟悉的话，应该会联想到2，也即标准错误输出。

如果我们不想看到错误输出呢？我们可以禁止标准错误 `cat $badname 2 > /dev/null` 。