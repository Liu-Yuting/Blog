---
title: tmpwatch
date: 2024-01-15 14:37:31
cover: true
coverImg: /medias/images/03.jpg
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
# tmpwatch

Linux tmpwatch 用于删除暂存文件。执行tmpwatch指令可删除不必要的暂存文件，您可以设置文件超期时间，单位以小时计算。

## 语法

```bash
$ tmpwatch [-afqv][--test][<超时时间>][目录...]
```

### 参数

- -a 或 --all：删除任何类型的文件。
- -f 或 --force：强制删除文件或目录，效果类似"rm"指令中的"-f"。
- -q 或 --quiet：不显示指令执行过程。
- -v 或 --verbose：详细显示指令执行过程。
- --test：仅做测试，并不真实删除文件或目录。

## 实例

```bash
# 使用指令"tmpwatch"删除目录"/tmp"中超过一天未使用的文件
$ tmpwatch 24 /tmp/  # 删除/tmp目录中超过一天未使用的文件
# >>> removing directctmp/orbit-tom if not empty
# 注意：该指令需要root权限，因此在使用tmpwatch命令前应该使用su命令切换用户。切换管理权限操作如下所示:
$ su                 # 切换到root用户  
$> **************    # 输入用户密码  
```

