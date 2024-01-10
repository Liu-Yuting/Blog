---
title: read
date: 2024-01-08 17:52:40
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
# read

Linux read命令用于从标准输入读取数值。read 内部命令被用来从标准输入读取单行数据。这个命令可以用来读取键盘输入，当使用重定向的时候，可以读取文件中的一行数据。

## 语法

```bash
$ read [-ers][-a aname][-d delim][-i text][-n nchars][-N nchars][-p prompt][-t timeout][-u fd][name ...]
```

### 参数

- -a：后面跟一个变量，该变量会被认为是一个数组，然后给其赋值，默认是以空格为分割符。
- -d：后面跟一个标识符，其实只有其后的第一个字符有用，作为结束的标志。
- -p：后面跟提示信息，即在输入前打印提示信息。
- -e：在输入的时候可以使用命令补全功能。
- -n：后面跟一个数字，定义输入文本的长度，很实用。
- -r：屏蔽\，如果没有该选项，则\作为一个转义符，有的话\就是个正常的字符了。
- -s：安静模式，在输入字符时不在屏幕上显示，例如login时输入密码。
- -t：后面跟秒数，定义输入字符的等待时间。
- -u：后面跟fd，从文件描述中读入，该文件秒数可以是exec新开启的。

## 实例

1. ### 简单读取。

   ```shell
   #!/bin/bash
   echo "输入网站名："  # 这里默认会换行
   read website       # 读取从键盘的输入
   echo "您输入的网站名是：$website"
   exit 0             # 退出
   # >>> 输入网站名：
   # >>> https://liu-yuting.github.io/
   # >>> 您输入的网站名是：https://liu-yuting.github.io/
   ```

2. ### -p参数

   允许在read命令中直接指定一个提示。

   ```bash
   #!/bin/bash
   read -p "输入网站名：" website
   echo "你输入的网站名是：$website" 
   exit 0
   # >>> 输入网站名：https://liu-yuting.github.io/
   # >>> 你输入的网站名是：https://liu-yuting.github.io/
   ```

3. ### -t参数

   指定read命令等待输入的秒数，当计时满时，read命令返回一个非零退出状态

   ```shell
   #!/bin/bash
   if read -t 5 -p "输入网站名：" website
   then
       echo "你输入的网站名是：$website"
   else
       echo "\n抱歉，你输入超时了。"
   fi
   exit 0
   # 执行程序不输入，等待五秒后：
   # >>> 输入网站名：
   # >>> 抱歉，你输入超时了
   ```

   除了输入时间计时，还可以使用 **-n** 参数设置 **read** 命令计数输入的字符。当输入的字符数目达到预定数目时，自动退出，并将输入的数据赋值给变量。

   ```shell
   #!/bin/bash
   read -n1 -p "Do you want to continue [Y/N]?" answer
   case $answer in
   Y | y)
         echo "fine ,continue";;
   N | n)
         echo "ok,good bye";;
   *)
        echo "error choice";;
   esac
   exit 0
   # 该例子使用了-n 选项，后接数值 1，指示 read 命令只要接受到一个字符就退出。只要按下一个字符进行回答，read 命令立即接受输入并将其传给变量，无需按回车键。只接收2个输入就退出：
   #!/bin/bash
   read -n2 -p "请随便输入两个字符：" any
   echo "\n您输入的两个字符是：$any"
   exit 0
   # >>> 请随便输入两个字符：12
   # >>> 您输入的两个字符是：12
   ```

4. ### -s参数

   能够使 **read** 命令中输入的数据不显示在命令终端上（实际上，数据是显示的，只是 **read** 命令将文本颜色设置成与背景相同的颜色）。输入密码常用这个选项。

   ```shell
   #!/bin/bash
   read  -s  -p "请输入您的密码：" pass
   echo "\n您输入的密码是：$pass"
   exit 0
   # 执行程序输入密码后不显示的：
   # >>> 请输入您的密码:
   # >>> 您输入的密码是 runoob
   ```

5. ### 读取文件

   每次调用 read 命令都会读取文件中的 "一行" 文本。当文件没有可读的行时，read 命令将以非零状态退出。

   通过什么样的方法将文件中的数据传给 read 呢？使用 cat 命令并通过管道将结果直接传送给包含 read 命令的 while 命令。

   ```shell
   # 测试文件 test.txt 内容如下：
   123
   456
   runoob
   # 测试代码
   #!/bin/bash
   count=1                         # 赋值语句，不加空格
   cat test.txt | while read line  # cat 命令的输出作为read命令的输入,read读到>的值放在line中
   do
      echo "Line $count:$line"
      count=$[ $count + 1 ]        # 注意中括号中的空格。
   done
   echo "finish"
   exit 0
   # >>> Line 1:123
   # >>> Line 2:456
   # >>> Line 3:runoob
   # >>> finish
   ```

6. ### -e参数

   ```shell
   # 以下实例输入字符 a 后按下 Tab 键就会输出相关的文件名(该目录存在的)：
   $ read -e -p "输入文件名:" str 
   # >>> 输入文件名:a
   # >>> a.out    a.py     a.pyc    abc.txt
   ```
