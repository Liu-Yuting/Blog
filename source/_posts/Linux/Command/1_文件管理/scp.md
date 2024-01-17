---
title: scp
date: 2024-01-11 14:36:05
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

# scp

## 介绍

csp命令用于Linux之间进行复制文件和目录。

scp是 `secure copy` 的缩写，scp是Linux系统下基于ssh登录进行安全的远程文件拷贝命令。 `scp` 是加密的， `rcp` 是不加密的， `scp` 是 `rcp` 的加强版。

## 语法

```sh
scp [-1246BCpqrv] [-c cipher] [-F ssh_config] [-i identity_file] [-l limit] [-o ssh_option] [-P port] [-S program] [[user@]host1:]file1 [...] [[user@]host2:] file2
```

### 参数说明：

- -1：强制scp命令使用协议ssh1。
- -2：强制scp命令使用协议ssh2。
- -4：强制scp命令只是用IPv4寻址。
- -6：强制scp命令只是用IPv6寻址。
- -B：使用批处理模式（传输过程中不询问传输口令或短语）。
- -C：允许压缩。（将-C标志传递给ssh，从而打开压缩功能）。
- -p：保留源文件的修改时间，访问时间和访问权限。
- -q：不显示传输进度条。
- -r：递归复制整个目录。
- -v：详细方式显示输出。scp和ssh(1) 会显示出整个过程的调试信息。这些信息用于调试连接，验证和配置问题。
- -c cipher：以cipher将数据传输进行加密，这个选项将直接传递给ssh。
- -F ssh_config：指定一个替代的ssh配置文件，此参数直接传递给ssh。
- -i identity_file：从指定文件中读取传输时使用的秘钥文件，此参数直接传递给ssh。
- -l limit：限定用户多能使用的宽带，以Kbit/s为单位。
- -o ssh_option：如习惯于使用ssh_config(5)中的参数传递方式。
- -P port：注意是大学的P，port是指定数据传输用到的端口号。
- -S program：指定加密传输时所使用的程序。此程序必须能够理解ssh(1)的选项。

## 实例

### 1、从本地复制到远程

命令格式：

```shell
scp local_file remote_username@remote_ip:remote_folder
# 或者
scp local_file remote_username@remote_ip:remote_file
# 或者
scp local_file remote_ip:remote_folder
# 或者
scp local_file remote_ip:remote_file
```

- 第 1、2 指定了用户名，命令后需要在输入密码，第一个金之鼎了远程目录，文件名不变，第二个指定了文件名；
- 第 3、4 没有指定用户名，命令执行后需要输入用户名和密码，第三个指定了远程的目录，文件名不变，第四个指定了文件名；

应用实例：

```shell
scp /home/liuyuting/1.mp3 liuyuting@192.168.0.1:/home/liuyuting/other/music
# 或者
scp /home/liuyuting/1.mp3 liuyuting@192.168.0.1:/home/liuyuting/other/music/001.mp3
# 或者
scp /home/liuyuting/1.mp3 192.168.0.1:/home/liuyuting/other/music
# 或者
scp /home/liuyuting/1.mp3 192.168.0.1:/home/liuyuting/other/music/001.mp3
```

复制目录命令格式：

```shell
scp -r local_folder remote_username@remote_ip:remote_folder 
# 或者 
scp -r local_folder remote_ip:remote_folder 
```

应用实例：

```shell
scp -r /home/liuyuting/ liuyuting@192.168.0.1:/home/liuyuting/others/
# 或者
scp -r /home/liuyuting/ 192.168.0.1:/home/liuyuting/other/
```

上述将本地的liuyuting目录复制到远程other目录下。

### 2、从远程复制到本地

从远程复制到本地，只要将从本地复制到远程的命令后两个参数调换顺序即可，如下实例。

应用实例：

```shell
scp liuyuting@192.168.0.1:/home/liuyuting/other/music /home/space/music/1.mp3
scp -r 192.168.0.1:/home/liuyuting/other/ /home/space/music/
```

## 说明：

1、如果远程服务器防火墙有为scp命令设置 了指定的端口，我们需要使用-P参数来设置命令的端口号

```shell
# scp命令使用端口号为 4588
scp -P 4588 liuyuting@192.168.0.1:/usr/local/***.sh /home/liuyuting
```

2、使用scp命令要确保使用的用户具有可读取远程服务器响应文件的权限，否则scp命令是无法起作用的
