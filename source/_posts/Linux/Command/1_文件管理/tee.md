---
title: tee
date: 2024-01-14 14:37:08
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
# tee

Linux tee 命令用于读取标准输入的内容，并将内容输出成文件。tee指令会从标准输入设备读取数据，将内容输出到标准输出设备，同时保存成文件。

## 语法

```bash
$ tee [-ai][--help][--version][文件...]
```

### 参数

- -a 或 --append：附加到既有文件的后面而不是覆盖它。
- -i 或 --ignore-interrupts：忽略中断信号。
- --help：在线帮助。
- --version：显示版本信息。

## 实例

```bash
# 使用指令"tee"将用户输入的数据同时保存到文件"file1"和"file2"中
$ tee file1 file2  # 在两个文件中复制内容
# 以上命令执行后，将显示用户输入需要保存到文件的数据
$> My Linux        # 提示用户输入
# >>> My Linux     # 输出数据，进行输出反馈
# 之后可以分别打开文件file1和file2，查看其中内容是否都是“My Linux”即可判断指令“tee”是否执行成功。
```
