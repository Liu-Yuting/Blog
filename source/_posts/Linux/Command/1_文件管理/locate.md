---
title: locate
date: 2023-12-19 12:19:00
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
# locate

locate命令用于查找符合条件的文档，它会去保存文档和目录名称的数据库内，查找合乎范本样式条件的文档或目录。一般情况我们只需要数据`locate <文件名称>`即可查找指定文件。

## 语法

```shell
locate [-d ][--help][--version][范本样式...]
```

### 参数

- -b 或 --basename：仅匹配路径名的基本名称。
- -c 或 --count：只输出找到的数量。
- -d 或 --database <文件路径>：指定自定义数据库，而不是默认数据库`/var/lib/mlocate/mlocate.db`。
- -e 或 --existing：仅打印当前现有文件的条目。
- -1：如果是1，则启动安全模式。在安全模式下，使用者不会看到权限无法看到的档案。这会使速度减慢，因为locate必须至实际的档案系统中取得档案的权限资料。
- -0 或 --null：在输出上带有NULL的单独条目。
- -S 或 --statistics：不搜索条目，打印有关每个数据库的统计信息。
- -P 或 --nofollow 或 -H：检查文件存在时不要遵循尾随的符号链接。
- -l 或 --limit 或 -n LIMIT：将输出（或计数）限制为LIMIT个条目。
- -n：至多显示n个输出。
- -m 或 --mmap：被忽略，为了向后兼容。
- -r 或 --regexp REGEXP：使用基本正则表达式。
- --regex：使用扩展正则表达式。
- -q 或 --quiet：安静模式，不会显示任何错误讯息。
- -s 或 --stdio：被忽略，为了向后兼容。
- -o：指定资料库存的名称。
- -h 或 --help：在线帮助。
- -i 或 --ignore-case：忽略大小写。
- -V 或 --version：显示版本信息。

## 实例

```shell
# 查找passwd文件
$ locate passwd
# 搜索etc目录下所有以sh开头的文件
$ locate /etc/sh
# 忽略大小写搜索当前用户目录下所有以r开头的文件
$ locate -i ~/r
```

## 附加说明

locate与find不同：find是去硬盘查找，locate只在/var/lib/slocate资料库中找。

locate的速度比find快，它并不是真的查找，而是查询数据库，一般文件数据库在`/var/lib/slocate/slocate.db`中，所以locate的查找并不是实时的，而是以数据库更新为准，一般是系统自己维护的，也可以手工升级数据库，命令为（默认情况下updatedb每天执行一次）：

```shell
$ updatedb
```
