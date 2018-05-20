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
## Linux系统目录结构
登录系统后，在当前命令窗口下输入 ls / 你会看到
```
[root@localhost ~]# ls /
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```
**/bin**  bin是Binary的缩写。这个目录存放着最经常使用的命令。

**/boot** 这里存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。

**/dev** dev是Device(设备)的缩写。该目录下存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。

**/etc** 这个目录用来存放所有的系统管理所需要的配置文件和子目录。

**/home** 用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。

**/lib** 这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。几乎所有的应用程序都需要用到这些共享库。

**/lib64** 包含许多被/bin和/sbin中的程序使用的库文件。目录/usr/lib中含有更多用于用户程序的库文件。

**/media** Linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，Linux会把识别的设备挂载到这个目录下。

**/mnt** 系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。

**/opt** 这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。

**/proc** 这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。这个目录的内容不在硬盘上而是在内存里，我们也可以直接修改里面的某些文件，比如可以通过下面的命令来屏蔽主机的ping命令，使别人无法ping你的机器：
```
echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all。
```

**/root** 该目录为系统管理员，也称作超级权限者的用户主目录。

**/run** 里面的东西是系统运行时需要的,不能随便删除。但是重启的时候应该抛弃。下次系统运行时重新生成。 

**/sbin** s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。

**/srv** 该目录存放一些服务启动之后需要提取的数据。

**/sys** 这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统 sysfs ，sysfs文件系统集成了下面3种文件系统的信息：

1. 针对进程信息的proc文件系统
2. 针对设备的devfs文件系统
3. 针对伪终端的devpts文件系统

该文件系统是内核设备树的一个直观反映。当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统种被创建。

**/tmp** 这个目录是用来存放一些临时文件的。

**/usr** 这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似与Windows下的program files目录。

**/usr/bin**：系统用户使用的应用程序。

**/usr/sbin**：超级用户使用的比较高级的管理程序和系统守护程序。

**/usr/src**：内核源代码默认的放置目录。

**/var** 这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。

**在linux系统中，有几个目录是比较重要的，平时需要注意不要误删除或者随意更改内部文件。**

* /etc： 上边也提到了，这个是系统中的配置文件，如果你更改了该目录下的某个文件可能会导致系统不能启动。
* /bin，/sbin，/usr/bin，/usr/sbin: 这是系统预设的执行文件的放置目录，比如ls就是在/bin/ls目录下的。值得提出的是，/bin, /usr/bin是给系统用户使用的指令（除root外的通用户），而/sbin, /usr/sbin则是给root使用的指令。
* /var： 这是一个非常重要的目录，系统上跑了很多程序，那么每个程序都会有相应的日志产生，而这些日志就被记录到这个目录下，具体在/var/log 目录下，另外mail的预设放置也是在这里。

## 如何正确关机
其实，在Linux领域内大多用在服务器上，很少遇到关机的操作。毕竟服务器上跑一个服务是永无止境的，除非特殊情况下，不得已才会关机。

Linux和Windows不同，在Linux底下，由于每个程序（或者说是服务）都是在在背景下执行的，因此，在你看不到的屏幕背后其实可能有相当多人同时在你的主机上面工作，例如浏览网页啦、传送信件啦以 FTP传送档案啦等等的，如果你直接按下电源开关来关机时，则其它人的数据可能就此中断！那可就伤脑筋了！此外，最大的问题是，若不正常关机，则可能造成文件系统的毁损（因为来不及将数据回写到档案中，所以有些服务的档案会有问题！）。

如果你要关机，必须要保证当前系统中没有其他用户在线。可以下达who这个指令，而如果要看网络的联机状态，可以下达netstat -a这个指令，而要看背景执行的程序可以执行ps -aux这个指令。使用这些指令可以让你稍微了解主机目前的使用状态！

关于关机的一些指令

sync 将数据由内存同步到硬盘中。

shutdown 关机指令，你可以man shutdown来看一下帮助文档。例如你可以运行如下命令关机：

shutdown –h 10 
```
This server will shutdown after 10 mins
```
这个命令告诉大家，计算机将在10分钟后关机，并且会显示在登陆用户的当前屏幕中。

shutdown –h now 立马关机

shutdown –h 20:25 系统会在今天20:25关机

shutdown –h +10 十分钟后关机

shutdown –r now 系统立马重启

shutdown –r +10 系统十分钟后重启

reboot 就是重启，等同于shutdown –r now

halt关闭系统，等同于shutdown –h now和poweroff

最后总结一下，不管是重启系统还是关闭系统，首先要运行sync命令，把内存中的数据写到磁盘中。