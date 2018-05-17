---
title: Linux入门（四）初步进入	Linux世界
date: 2018-05-17 21:03:20
tags:
  - Linux
categories: Linux
---
<img src="http://p6v6hsmcp.bkt.clouddn.com/linux.jpg" alt="Linux" style="width:100%" />
<!-- more -->
## Linux的启动过程
Linux的启动其实和Windows的启动过程很类似，不过Windows我们是无法看到启动信息的，而Linux启动时我们会看到许多启动信息，例如某个服务是否启动。

Linux系统的启动过程大体上可分为五部分：

1. 内核的引导
2. 运行init
3. 系统初始化
4. 建立终端
5. 用户登录系统

## 学会使用快捷键
Ctrl+C：这个是用来终止当前命令的快捷键，当然你也可以输入一大串字符，不想让它运行直接Ctrl + C，光标就会跳入下一行。 

Tab：这个键是最有用的键了，因为当你打一个命令打一半时，它会帮你补全的。不光是命令，当你打一个目录时，同样可以补全，不信你试试。

Ctrl+D：退出当前终端，同样你也可以输入exit。

Ctrl+Z：暂停当前进程，比如你正运行一个命令，突然觉得有点问题想暂停一下，就可以使用这个快捷键。暂停后，可以使用fg恢复它。

Ctrl+L：清屏，使光标移动到第一行，同样你也可以输入clear。
## 查询帮助文档——man
<div class="note info"><p>有问题找男人——man</p></div>
这个man通常是用来看一个命令的帮助文档的。例如：
```
[root@localhost ~]# man ls
LS(1)                            User Commands                           LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List  information  about  the FILEs (the current directory by default).
       Sort entries alphabetically if none of -cftuvSUX nor --sort  is  speci鈥[m
       fied.

       Mandatory  arguments  to  long  options are mandatory for short options
       too.

       -a, --all
              do not ignore entries starting with .

       -A, --almost-all
              do not list implied . and ..

       --author
 Manual page ls(1) line 1 (press h for help or q to quit)...skipping...
LS(1)                            User Commands                           LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List  information  about  the FILEs (the current directory by default).
       Sort entries alphabetically if none of -cftuvSUX nor --sort  is  speci鈥[m
       fied.

       Mandatory  arguments  to  long  options are mandatory for short options
       too.

       -a, --all
              do not ignore entries starting with .

       -A, --almost-all
              do not list implied . and ..

       --author
              with -l, print the author of each file

       -b, --escape
              print C-style escapes for nongraphic characters

       --block-size=SIZE
              scale sizes by SIZE before printing them; e.g., '--block-size=M'
              prints sizes in units of 1,048,576 bytes; see SIZE format below

       -B, --ignore-backups
              do not list implied entries ending with ~

       -c     with -lt: sort by, and show, ctime (time of last modification of
              file status information); with -l: show ctime and sort by  name;
              otherwise: sort by ctime, newest first

       -C     list entries by columns

       --color[=WHEN]
              colorize  the  output;  WHEN can be 'never', 'auto', or 'always'
              (the default); more info below
```

输入man ls,其实格式为man+ 命令

你就会看到相关的帮助文档了。从命令的介绍到命令的参数以及用法介绍的都非常详细的。不错吧。