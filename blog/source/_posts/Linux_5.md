---
title: Linux入门（五）Linux文件与目录管理
date: 2018-05-19 17:34:27
tags:
  - Linux
categories: Linux
---
<img src="http://p6v6hsmcp.bkt.clouddn.com/linux.jpg" alt="Linux" style="width:100%" />
<!-- more -->
## 绝对路径和相对路径
在Linux中什么是一个文件的路径呢，说白了就是这个文件存在的地方，例如/root/.ssh/authorized_keys这就是一个文件的路径。如果你告诉系统这个文件的路径，那么系统就可以找到这个文件。在Linux的世界中，存在着绝对路径和相对路径。

### 绝对路径 
路径的写法一定由根目录”/”写起，例如/usr/local/mysql 这就是绝对路径。
### 相对路径
路径的写法不是由根目录”/”写起，例如，首先用户进入到/ ，然后再进入到home，命令为 cd /home，然后 cd test，此时用户所在的路径为/home/test。第一个cd命令后跟 /home，第二个cd命令后跟test，并没有斜杠，这个test是相对于/home 目录来讲的，所以叫做相对路径。
## 一些常用的指令
**pwd** 这个命令打印出当前所在目录
```
[root@localhost ~]# pwd
/root
[root@localhost ~]#
```
**cd** 进入到某一个目录
```
[root@localhost ~]# cd /usr/local
[root@localhost local]# pwd
/usr/local
[root@localhost local]#
```
**./** 指的是当前目录

**../** 指的是当前目录的上一级目录。
```
[root@localhost local]# cd /usr/local/lib/
[root@localhost lib]# pwd
/usr/local/lib
[root@localhost lib]# cd ./
[root@localhost lib]# pwd
/usr/local/lib
[root@localhost lib]# cd ../
[root@localhost local]# pwd
/usr/local
[root@localhost local]#
```
首先进入到/usr/local/lib/目录下，然后再进入./其实还是进入到当前目录下，用pwd查看当前目录，并没有发生变化，然后再进入../ 则是进入到了/usr/local/目录下，即/usr/local/lib目录的上一级目录。

**mkdir** 创建一个目录。mkdir其实就是make directory的缩写。其语法为mkdir [-mp] [目录名称]，其中-m , –p 为其选项，-m：这个参数用来指定要创建目录的权限，该参数不常用，所以不做重点解释。-p：这个参数很管用的，先来做个试验，就会一目了然了。
```
[root@localhost local]# mkdir /tmp/test/123
mkdir: 无法创建目录"/tmp/test/123": 没有那个文件或目录
[root@localhost local]# ls /tmp/test/
ls: 无法访问/tmp/test/: 没有那个文件或目录
[root@localhost local]# 
```
当我们想创建/tmp/test/123目录，可是提示不能创建，原因是/tmp/test目录不存在，你会说，这个Linux怎么这傻，/tmp/test目录不存在就自动创建不就OK了嘛，的确Linux确实很傻，如果它发现要创建的目录的上一级目录不存在就会报错。然后Linux也为我们想好了解决办法，即-p参数。
```
[root@localhost local]# mkdir -p /tmp/test/123
[root@localhost local]# ls /tmp/test/
123
[root@localhost local]# 
```
-p 它的作用就是递归创建目录，即使上级目录不存在。还有一种情况就是如果你想要创建的目录存在的话，会提示报错，然后你加上-p参数后，就不会报错，如下所示：
```
[root@localhost local]# mkdir /tmp/test/123
mkdir: 无法创建目录"/tmp/test/123": 文件已存在
[root@localhost local]# mkdir -p /tmp/test/123
[root@localhost local]# 
```
**rmdir** 删除一个目录。

```
[root@localhost local]# ls /tmp/test
123
[root@localhost local]# rmdir /tmp/test/123
[root@localhost local]# ls /tmp/test
[root@localhost local]# 
```
rmdir,其实是rmove directory缩写，其只有一个选项-p 类似与mkdir命令，这个参数的作用是将上级目录一起删除。
举个例子吧，新建目录mkdir -p d1/d2/d3 ，rmdir -p d1/d2/d3相当于是删除了d1,d1/d2, d1/d2/d3。

**rm** 删除目录或者文件。rmdir只能删除目录但不能删除文件，要想删除一个文件，则要用rm命令了。rm同样也有很多选项。可以通过man rm来获得详细帮助信息。在这里只列举较常用的几个选项。

-f 强制的意思，如果不加这个选项，当删除一个不存在的文件时会报错。
```
[root@localhost local]# ls /tmp/111
ls: 无法访问/tmp/111: 没有那个文件或目录
[root@localhost local]# /bin/rm /tmp/111
rm: 无法删除"/tmp/111": 没有那个文件或目录
[root@localhost local]# /bin/rm -f /tmp/111
[root@localhost local]# 
```
当用户用rm删除一个文件时会提示用户是否真的删除。
```
[root@localhost ~]# rm /root/anaconda-ks.cfg
rm：是否删除普通空文件 "/root/anaconda-ks.cfg"？n
[root@localhost ~]# 
```
如果删除，输入y, 否则输入n

-r 当删除目录时，加该选项，如果不加这个选项会报错。rm是可以删除不为空的目录的。
```
[root@localhost ~]# mkdir /tmp/test/123/
[root@localhost ~]# ls /tmp/test/
123
[root@localhost ~]# rm /tmp/test/123
rm: 无法删除"/tmp/test/123": 是一个目录
[root@localhost ~]# rm -r /tmp/test/123
rm：是否删除目录 "/tmp/test/123"？y
[root@localhost ~]# ls /tmp/test/
[root@localhost ~]# 
```
```
[root@localhost ~]# mkdir /tmp/test/123/
[root@localhost ~]# ls /tmp/test/
123
[root@localhost ~]# rm -r /tmp/test/
rm：是否进入目录"/tmp/test/"? y
rm：是否删除目录 "/tmp/test/123"？y
rm：是否删除目录 "/tmp/test/"？y
[root@localhost ~]# ls /tmp/test/
ls: 无法访问/tmp/test/: 没有那个文件或目录
[root@localhost ~]# 
```
关于rm，使用最多便是-rf两个选项合用了。不管删除文件还是目录都可以。但是方便的同时也要多注意，万一你的手太快后边跟了/那样就会把你的系统文件全部删除的，切记切记。
```
[root@localhost ~]# mkdir -p /d1/d2/d3
[root@localhost ~]# rm -r /d1
rm：是否进入目录"/d1"? y
rm：是否进入目录"/d1/d2"? y
rm：是否删除目录 "/d1/d2/d3"？y
rm：是否删除目录 "/d1/d2"？y
rm：是否删除目录 "/d1"？y
[root@localhost ~]# mkdir -p /d1/d2/d3
[root@localhost ~]# rm -rf /d1
[root@localhost ~]# ls /d1
ls: 无法访问/d1: 没有那个文件或目录
[root@localhost ~]# 
```
**which** 用来查找一个命令的绝对路径。
```
[root@localhost ~]# which rm
alias rm='rm -i'
        /usr/bin/rm
[root@localhost ~]# 
```
**alias** 用来设置指令的别名。语法：alias[别名]=[指令名称]，例如 alias rm='rm -i' ，即当我们使用rm命令时，实际上是使用的是rm –i ，而用绝对路径的/user/bin/rm 则不会被alias，该命令在以后会详细介绍。

**ls** 在前面的命令中多次用到它。它的作用就是查看某个目录或者某个文件，是list的简写。ls 后可以跟一个目录，也可以跟一个文件。以下是ls的选项，在这里并没有完全列出，只是列出了平时使用最多的选项。其他选项，你可以自行通过man ls查询。

-a 全部的档案都列出，包括隐藏的。linux文件系统中同样也有隐藏文件。这些隐藏文件的文件名是以.开头的。例如.test, /root/.123, /root/.ssh 等等，隐藏文件可以是目录也可以是普通文件。

-l 详细列出文件的属性信息，包括大小、创建日期、所属主所属组等等。ll这个命令等同于ls –l 。
```
[root@localhost ~]# which ll
alias ll='ls -l --color=auto'
        /usr/bin/ls
[root@localhost ~]# 
```
--color=never/always/auto never即不要显示颜色，always即总显示颜色，auto是由系统自行判断。

-d 后边跟目录，如果不加这个选项则列出目录下的文件，加上后只列车目录本身。
```
[root@localhost ~]# ls -d /root/
/root/
[root@localhost ~]# ls /root
anaconda-ks.cfg
[root@localhost ~]# 
```
**cp** copy的简写，即拷贝。格式为cp [选项] [来源文件] [目的文件]，例如我想把test1拷贝成test2 输入以下命令即可：
```
cp test1 test2
```
以下介绍几个常用的选项

-d 这里涉及到一个“连接”的概念。连接分为软连接和硬连接。这在以后会详细解释，现在只需要明白这里的软连接跟Windows中的快捷方式类似即可。如果不加这个-d 则拷贝软连接时会把软连接的目标文件拷贝过去，而加上后，其实只是拷贝了一个连接文件（即快捷方式）。
```
[root@localhost ~]# mkdir /tmp/test/
[root@localhost ~]# cd /tmp/test/
[root@localhost test]# touch test
[root@localhost test]# ln -s test test1
[root@localhost test]# ls -l
总用量 0
-rw-r--r--. 1 root root 0 5月  20 12:48 test
lrwxrwxrwx. 1 root root 4 5月  20 12:49 test1 -> test
[root@localhost test]# cp test1 test2
[root@localhost test]# ls -l
总用量 0
-rw-r--r--. 1 root root 0 5月  20 12:48 test
lrwxrwxrwx. 1 root root 4 5月  20 12:49 test1 -> test
-rw-r--r--. 1 root root 0 5月  20 12:50 test2
[root@localhost test]# cp -d test1 test3
[root@localhost test]# ls -l
总用量 0
-rw-r--r--. 1 root root 0 5月  20 12:48 test
lrwxrwxrwx. 1 root root 4 5月  20 12:49 test1 -> test
-rw-r--r--. 1 root root 0 5月  20 12:50 test2
lrwxrwxrwx. 1 root root 4 5月  20 12:50 test3 -> test
[root@localhost test]# 
```
上面的ln命令即为建立连接的，以后再做详细解释。

-r 如果你要拷贝一个目录，必须要加-r选项，否则你是拷贝不了目录的。
```
[root@localhost test]# mkdir 123
[root@localhost test]# ls
123  test  test1  test2  test3
[root@localhost test]# cp 123 111
cp: 略过目录"123"
[root@localhost test]# ls
123  test  test1  test2  test3
[root@localhost test]# cp -r 123 111
[root@localhost test]# ls
111  123  test  test1  test2  test3
[root@localhost test]# 
```
-i 如果遇到一个存在的文件，会问是否覆盖。在Redhat/CentOS系统中，我们使用的cp其实是cp –i
```
[root@localhost test]# which cp
alias cp='cp -i'
        /usr/bin/cp
[root@localhost test]#
```
```
[root@localhost test]# cd 123
[root@localhost 123]# ls
[root@localhost 123]# touch 111
[root@localhost 123]# touch 222
[root@localhost 123]# /usr/bin/cp -i 111 222
/usr/bin/cp：是否覆盖"222"？ n
[root@localhost 123]# echo 'abc' >111
[root@localhost 123]# echo 'def' >222
[root@localhost 123]# cat 111
abc
[root@localhost 123]# cat 222
def
[root@localhost 123]# /usr/bin/cp 111 222
[root@localhost 123]# cat 222
abc
[root@localhost 123]# 
```
上面，touch 命令，看字面意思就是摸一下，没错，如果有这个文件，则会改变文件的访问时间，如果没有这个文件就会创建这个文件。前面说过echo，其实就是打印，在这里所echo的内容”abc”和“def”并没有显示在屏幕上，而是分别写进了文件111和222, 其写入作用的就是这个大于号”>” 在Linux中这叫做重定向，即把前面产生的输出写入到后面的文件中。这在以后会做详细介绍，这里需要明白它的含义即可。而cat命令则是读一个文件，并把读出的内容打印到当前屏幕上。

-u 该选项仅当目标文件存在时才会生效，如果源文件比目标文件新才会拷贝，否则不做任何动作。

**mv** 移动的意思，是move的简写。格式为mv [选项] [源文件] [目标文件]，下面介绍几个常用的选项。

-i 和cp的-i 一样，当目标文件存在时会问用户是否要覆盖。在Redhat/CentOS系统中，我们使用的mv其实是mv –i

-u 和上边cp命令的-u选项一个作用，当目标文件存在时才会生效，如果源文件比目标文件新才会移动，否则不做任何动作。

该命令有以下中情况

1. 目标文件是目录，而且目标文件不存在；
2. 目标文件是目录，而且目标文件存在；
3. 目标文件不是目录不存在；
4. 目标文件不是目录存在；

目标文件是目录，存在和不存在，移动的结果是不一样的，如果存在，则会把源文件移动到目标文件目录中。不存在的话移动完后，目标文件是一个文件。
```
[root@localhost 123]# cd ../111
[root@localhost 111]# ls
[root@localhost 111]# mkdir aa bb
[root@localhost 111]# ls
aa  bb
[root@localhost 111]# mv aa cc
[root@localhost 111]# ls
bb  cc
[root@localhost 111]# mv cc bb
[root@localhost 111]# ls
bb
[root@localhost 111]# ls bb
cc
[root@localhost 111]# touch dd
[root@localhost 111]# ls
bb  dd
[root@localhost 111]# mv dd ee
[root@localhost 111]# ls
bb  ee
[root@localhost 111]# mv ee bb
[root@localhost 111]# ls bb
cc  ee
[root@localhost 111]# 
```
Windows下的重命名，在linux下用mv就可以搞定。
**cat** 比较常用的一个命令，即查看一个文件的内容并显示在屏幕上。

-n 查看文件时，把行号也显示到屏幕上。
```
[root@localhost 111]# echo '123123 ' >bb/ee
[root@localhost 111]# echo '456456 ' >>bb/ee
[root@localhost 111]# cat bb/ee
123123
456456
[root@localhost 111]# cat -n bb/ee
     1  123123
     2  456456
[root@localhost 111]# 
```
上面出现了一个”>>”，这个符号跟前面介绍的”>”的作用都是重定向，即把前面输出的东西输入到后边的文件中，只是”>>”是追加的意思，而用”>”，如果文件中有内容则会删除文件中内容，而”>>”则不会。

-A 显示所有东西出来，包括特殊字符
```
[root@localhost 111]# cat -A bb/ee
123123 $
456456 $
[root@localhost 111]#
```
**tac** 其实是cat的反写，同样的功能也是反向打印文件的内容到屏幕上。
```
[root@localhost 111]# tac bb/ee
456456 
123123 
[root@localhost 111]# 
```
**more** 也是用来查看一个文件的内容。当文件内容太多，一屏幕不能占下，而你用cat肯定是看不前面的内容的，那么使用more就可以解决这个问题了。当看完一屏后按空格键继续看下一屏。但看完所有内容后就会退出。如果想提前退出，只需按q键即可。

**less** 作用跟more一样，但比more好在可以上翻，下翻。空格键同样可以翻页，而按”j”键可以向下移动（按一下就向下移动一行），按”k”键向上移动。在使用more和less查看某个文件时，你可以按一下”/” 键，然后输入一个word回车，这样就可以查找这个word了。如果是多个该word可以按”n”键显示下一个。另外你也可以不按”/”而是按”?”后边同样跟word来搜索这个word，唯一不同的是，”/”是在当前行向下搜索，而”?”是在当前行向上搜索。

**head** head后直接跟文件名，则显示文件的前十行。如果加 –n 选项则显示文件前n行。
```
[root@localhost 111]# head /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
[root@localhost 111]# head -n 5 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
[root@localhost 111]# 
```
**tail** 和head一样，后面直接跟文件名，则显示文件最后十行。如果加-n 选项则显示文件最后n行。
```
[root@localhost 111]# tail /etc/passwd
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:999:998:User for polkitd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
jgh:x:1000:1000:jgh:/home/jgh:/bin/bash
[root@localhost 111]# tail -n 2 /etc/passwd
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
jgh:x:1000:1000:jgh:/home/jgh:/bin/bash
[root@localhost 111]# 
```
-f 动态显示文件的最后十行，如果文件是不断增加的，则用-f 选项。如：tail -f /var/log/messages
## 环境变量PATH
上边提到了alias，也提到了绝对路径的/user/bin/rm ，然后你意识到没有，为什么我们输入很多命令时是直接打出了命令，而没有去使用这些命令的绝对路径？这是因为环境变量PATH在起作用了。请输入 echo $PATH，这里的echo其实就是打印的意思，而PATH前面的$表示后面接的是变量。
```
[root@localhost ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
[root@localhost ~]# echo hello
hello
[root@localhost ~]# 
```
因为/usr/bin 在PATH的设定中，所以自然就可以找到ls了。如果你将ls移动到/root底下的话，然后你自己本身也在/root底下，但是当你执行ls的时候，他就是不理你？怎么办？这是因为PATH没有/root 这个目录，而你又将ls移动到/root底下了，自然系统就找不到可执行文件了，因此就会告诉你， command not found！那么该怎么克服这种问题呢？

有两个方法，一种方法是直接将/root的路径加入PATH当中！可以使用以下命令： 　
```
PATH=”$PATH”:/root
```

另一种方式则是使用完整档名，亦即直接使用相对或绝对路径来执行，例如：
```
/root/ls
./ls
```
## 文件的所属主以及所属组
一个Linux目录或者文件，都会有一个所属主和所属组。所属主，即文件的拥有者，而所属组，即该文件所属主所在的一个组。Linux这样设置文件属性的目的是为了文件的安全。例如，test文件的所属主是user0 而test1文件的所属主是user1，那么user1是不能查看test文件的，相应的user0也不能查看test1文件。然后有这样一个应用，我想创建一个文件同时让user0和user1来查看怎么办呢？

这时“所属组”就派上用场了。即，创建一个群组users，让user0和user1同属于users组，然后建立一个文件test2，且其所属组为users，那么user0和user1都可以访问test2文件。

Linux文件属性不仅规定了所属主和所属组，还规定了所属主（user）、所属组(group)以及其他用户（others）对该文件的权限。你可以通过ls -l 来查看这些属性。
```
[root@localhost ~]# cd /tmp/test/
[root@localhost test]# ls -l
总用量 0
drwxr-xr-x. 3 root root 16 5月  20 13:39 111
drwxr-xr-x. 2 root root 28 5月  20 13:23 123
-rw-r--r--. 1 root root  0 5月  20 12:48 test
lrwxrwxrwx. 1 root root  4 5月  20 12:49 test1 -> test
-rw-r--r--. 1 root root  0 5月  20 13:16 test2
lrwxrwxrwx. 1 root root  4 5月  20 12:50 test3 -> test
```
## Linux文件属性
上面用ls –l查看当前目录下的文件时，共显示了9列内容（用空格划分列），都代表了什么含义呢？

第1列，包含的东西有该文件类型和所属主、所属组以及其他用户对该文件的权限。第一列共10位。其中第一位用来描述该文件的类型。我们看到的类型有”d”, “-“，”l” ，其实除了这两种外还有“b”, “c”,”s”等。

"d" 表示该文件为目录；

"-" 表示该文件为普通文件；

"l" 表示该文件为连接文件（Linux file），上边提到的软连接即为该类型；

"b" 表示该文件为块设备文件，比如磁盘分区

"c" 表示该文件为串行端口设备，例如键盘、鼠标。

"s" 表示该文件为套接字文件（socket），用于进程间通信。

后边的9位，每三个为一组。均为rwx三个参数的组合。其中r代表可读，w代表可写，x代表可执行。前三位为所属主（user）的权限，中间三位为所属组（group）的权限，最后三位为其他非本群组（others）的权限。下面拿一个具体的例子来述说一下。

一个文件的属性为-rwxr-xr-- ，它代表的意思是，该文件为普通文件，文件拥有者可读可写可执行，文件所属组对其可读不可写可执行，其他用户对其只可读。

对于一个目录来讲，打开这个目录即为执行这个目录，所以任何一个目录必须要有x权限才能打开并查看该目录。例如一个目录的属性为drwxr--r-- 其所属主为root，那么除了root外的其他用户是不能打开这个目录的。

第2列，表示为连接占用的节点（inode），若为目录时，通常与该目录地下还有多少目录有关系，关于连接（link）在以后详细介绍。

第3列，表示该文件的所属主。

第4列，表示该文件的所属组。

第5列，表示该文件的大小。

第6列、第7列和第8列为该文件的创建日期或者最近的修改日期，分别为月份日期以及时间。

第9列，文件名。如果前面有一个. 则表示该文件为隐藏文件。

## 更改文件的权限
更改文件的权限，也就是更改所属主、所属组以及他们对应的读写执行权限。

### 更改所属组

**chgrp** chgrp其实就是change group的缩写。

语法：chgrp [组名] [文件名]
```
[root@localhost ~]# cd /tmp/test
[root@localhost test]# ls
111  123  test  test1  test2  test3
[root@localhost test]# groupadd testgroup
[root@localhost test]# ls -l test
-rw-r--r--. 1 root root 0 5月  20 12:48 test
[root@localhost test]# chgrp testgroup test
[root@localhost test]# ls -l test
-rw-r--r--. 1 root testgroup 0 5月  20 12:48 test
[root@localhost test]# 
```
这里用到了groupadd命令，其含义即增加一个用户组。该命令在以后会做详细介绍，现在只需要知道它是用来增加用户组的即可。
### 更改文件的所属主

**chown** chown其实就是change owner的缩写。
语法：
* chown [ -R ] 账户名 文件名

* chown [ -R ] 账户名：组名 文件名

这里的-R选项只作用于目录，作用是级联更改，即不仅更改当前目录，连目录里的目录或者文件全部更改。
```
[root@localhost test]# useradd user1
[root@localhost test]# ls -ld 111
drwxr-xr-x. 3 root root 16 5月  20 13:39 111
[root@localhost test]# touch 111/test2
[root@localhost test]# ls -l 111
总用量 0
drwxr-xr-x. 3 root root 26 5月  20 13:45 bb
-rw-r--r--. 1 root root  0 5月  20 17:15 test2
[root@localhost test]# chown user1 111
[root@localhost test]# ls -ld 111
drwxr-xr-x. 3 user1 root 29 5月  20 17:15 111
[root@localhost test]# ls -l 111
总用量 0
drwxr-xr-x. 3 root root 26 5月  20 13:45 bb
-rw-r--r--. 1 root root  0 5月  20 17:15 test2
[root@localhost test]# chown -R user1:testgroup 111
[root@localhost test]# ls -ld 111
drwxr-xr-x. 3 user1 testgroup 29 5月  20 17:15 111
[root@localhost test]# ls -l 111
总用量 0
drwxr-xr-x. 3 user1 testgroup 26 5月  20 13:45 bb
-rw-r--r--. 1 user1 testgroup  0 5月  20 17:15 test2
[root@localhost test]#
```
useradd是增加一个账户，以后会详细介绍。在111目录下创建一个普通文件test2，因为是以root的身份创建的目录和文件，所以所属主以及所属组都是root。chown user1 111这使111的目录所属主由root变为了user1 ，然后111目录下的test2文件所属主以及所属组还是root。接着chown –R user1:testgroup 111这样把111连同111目录下的test2 的所属主以及所属组都改变了。
### 改变用户对文件的读写执行权限 
**chmod** 

语法： chmod [-R] xyz 文件名 （这里的xyz，表示数字）

-R 选项作用同chown，级联更改。

在Linux中为了方便更改这些权限，Linux使用数字去代替rwx，具体规则为r:4 w:2 x:1 -:0 举个例子，-rwxrwx---用数字表示就是770，具体是这样来的：

rwx = 4+2+1=7; rwx= 4+2+1=7; --- = 0+0+0=0

值得提一下的是，在Linux系统中，默认一个目录的权限为755，而一个文件的默认权限为644。 
```
[root@localhost test]# ls -dl 111
drwxr-xr-x. 3 user1 testgroup 29 5月  20 17:15 111
[root@localhost test]# ls -l 111
总用量 0
drwxr-xr-x. 3 user1 testgroup 26 5月  20 13:45 bb
-rw-r--r--. 1 user1 testgroup  0 5月  20 17:15 test2
[root@localhost test]# chmod 750 111
[root@localhost test]# ls -ld 111
drwxr-x---. 3 user1 testgroup 29 5月  20 17:15 111
[root@localhost test]# ls -l 111/test2
-rw-r--r--. 1 user1 testgroup 0 5月  20 17:15 111/test2
[root@localhost test]# chmod -R 700 111
[root@localhost test]# ls -ld 111
drwx------. 3 user1 testgroup 29 5月  20 17:15 111
[root@localhost test]# ls -l 111
总用量 0
drwx------. 3 user1 testgroup 26 5月  20 13:45 bb
-rwx------. 1 user1 testgroup  0 5月  20 17:15 test2
[root@localhost test]# 
```
如果你创建了一个目录，而该目录不想让其他人看到内容，则只需设置成 rwxr----- (740) 即可。

chmod还支持使用rwx的方式来设置权限。从之前的介绍中我们可以发现，基本上就九个属性分别是(1)user (2)group (3)others 三群啦。那么我们就可以藉由 u, g, o 来代表三群的属性！此外， a则代表all, 亦即全部的三群。那么读写的属性就可以写成了 r, w, x！也就是可以使用底下的方式来看： 

<img src="http://p6v6hsmcp.bkt.clouddn.com/6_86.png" alt="chmod" style="width:100%" />
现在我想把一个文件设置成这样的权限 rwxr-xr-x (755)，使用这种方式改变权限的命令为:
```
[root@localhost test]# chmod u=rwx,go=rx 111/test2
[root@localhost test]# ls -l 111/test2
-rwxr-xr-x. 1 user1 testgroup 0 5月  20 17:15 111/test2
[root@localhost test]# 
```
另外还可以针对u, g, o, a增加或者减少某个权限（读，写，执行），例如：
```
[root@localhost test]# chmod u-x 111/test2
[root@localhost test]# ls -l 111/test2
-rw-r-xr-x. 1 user1 testgroup 0 5月  20 17:15 111/test2
[root@localhost test]# chmod a-x 111/test2
[root@localhost test]# ls -l 111/test2
-rw-r--r--. 1 user1 testgroup 0 5月  20 17:15 111/test2
[root@localhost test]# chmod u+x 111/test2
[root@localhost test]# ls -l 111/test2
-rwxr--r--. 1 user1 testgroup 0 5月  20 17:15 111/test2
[root@localhost test]# 
```
**umask** 上边也提到了默认情况下，目录权限值为755，普通文件权限值为644。那么这个值是由谁规定呢？追究其原因就涉及到了umask。

umask语法： umask xxx （这里的xxx代表三个数字）

查看umask值只要输入umask然后回车。 umask预设是0022，其代表什么含义？先看一下下面的规则：

1）若用户建立为普通文件，则预设“没有可执行权限”，只有rw两个权限。最大为666（-rw-rw-rw-）

2）若用户建立为目录，则预设所有权限均开放，即777（drwxrwxrwx）

umask数值代表的含义为，上边两条规则中的默认值（文件为666，目录为777）需要减掉的权限。所以目录的权限为(rwxrwxrwx) – (----w--w-) = (rwxr-xr-x)，普通文件的权限为(rw-rw-rw-) – (----w--w-) = (rw-r--r--)。umask的值是可以自定义的，比如设定umask为002，你再创建目录或者文件时，默认权限分别为(rwxrwxrwx) – (-------w-) = (rwxrwxr-x)和(rw-rw-rw-) – (-------w-) = (rw-rw-r--)。
```
[root@localhost test]# umask
0022
[root@localhost test]# umask 002
[root@localhost test]# mkdir test4
[root@localhost test]# ls -ld test4
drwxrwxr-x. 2 root root 6 5月  20 20:55 test4
[root@localhost test]# touch test5
[root@localhost test]# ls -l test5
-rw-rw-r--. 1 root root 0 5月  20 20:56 test5
[root@localhost test]# 
```
umask可以在/etc/bashrc里面更改，预设情况下，root的umask为022，而一般使用者则为002，因为可写的权限非常重要，因此预设会去掉写权限。
### 修改文件的特殊属性
**chattr** chattr其实就是change attribute的缩写。

语法： chattr [+-=][ASaci] 文件或者目录名

+-= ：分别为增加、减少、设定

A：增加该属性后，文件或目录的atime将不可被修改；

S：增加该属性后，会将数据同步写入磁盘中；

a：增加该属性后，只能追加不能删除，非root用户不能设定该属性；

c：自动压缩该文件，读取时会自动解压；

i：增加后，使文件不能被删除、重命名、设定连接、写入、新增数据；
```
[root@localhost test]# chattr +i test4
[root@localhost test]# touch test4/123
touch: 无法创建"test4/123": 权限不够
[root@localhost test]# chattr -i test4
[root@localhost test]# touch test4/123
[root@localhost test]# ls test4
123
[root@localhost test]# 
```
增加i属性后不能在该目录中建立文件。
```
[root@localhost test]# touch test4/456
[root@localhost test]# ls test4
123  456
[root@localhost test]# chattr +a test4
[root@localhost test]# rm test4/456
rm：是否删除普通空文件 "test4/456"？y
rm: 无法删除"test4/456": 不允许的操作
[root@localhost test]# touch test4/789
[root@localhost test]# ls test4
123  456  789
[root@localhost test]# chattr -a test4
[root@localhost test]# ls test4
123  456  789
[root@localhost test]# rm test4/456
rm：是否删除普通空文件 "test4/456"？y
[root@localhost test]# ls test4
123  789
[root@localhost test]# 
```
增加a属性后，只能追加不能删除。
### 列出文件/目录的特殊属性
**lsattr**

语法： lsattr [-aR] [文件/目录名]

-a：类似与ls的-a选项，即连同隐藏文件一同列出；

-R：连同子目录的数据一同列出
```
[root@localhost test]# lsattr -a test4
---------------- test4/.
---------------- test4/..
---------------- test4/123
---------------- test4/789
[root@localhost test]# mkdir test4/test
[root@localhost test]# touch test4/test/aa
[root@localhost test]# lsattr -R test4
---------------- test4/123
---------------- test4/789
---------------- test4/test

test4/test:
---------------- test4/test/aa

[root@localhost test]# lsattr test4
---------------- test4/123
---------------- test4/789
---------------- test4/test
[root@localhost test]# 
```
## 在Linux下搜索一个文件
在Windows下有一个搜索工具，可以让我们很快的找到一个文件，这是很有用的。然而在linux下搜索功能更加强大。

**which** 用来查找可执行文件的绝对路径。

在前面已经多次用到该命令，需要注意的一点是，which只能用来查找PATH环境变量中出现的路径下的可执行文件。这个命令用的也是蛮多的，有时候我们不知道某个命令的绝对路径，which一下很容易就知道了。
```
[root@localhost test]# which ls
alias ls='ls --color=auto'
        /usr/bin/ls
[root@localhost test]# which cd
/usr/bin/cd
[root@localhost test]# 
```
当查找的文件在PATH变量中并没有时，就会报错。

**whereis** 通过预先生成的一个文件列表库去查找跟给出的文件名相关的文件。

语法： whereis [-bmsu] [文件名称]

-b：只找binary 文件

-m：只找在说明文件manual路径下的文件

-s：只找source来源文件

-u：没有说明档的文件
```
[root@localhost test]# whereis passwd
passwd: /usr/bin/passwd /etc/passwd /usr/share/man/man1/passwd.1.gz
[root@localhost test]# where -b passwd
-bash: where: 未找到命令
[root@localhost test]# whereis -b passwd
passwd: /usr/bin/passwd /etc/passwd
[root@localhost test]# whereis -m passwd
passwd: /usr/share/man/man1/passwd.1.gz
[root@localhost test]# 
```
locate 类似于whereis，也是通过查找预先生成的文件列表库来告诉用户要查找的文件在哪里。后边直接跟文件名。如果你的Linux没有这个命令，请安装软件包mlocate，这个软件包在你的系统安装盘里，后缀名是RPM，随后介绍的find命令会告诉你如何查找这个包。如果你装的CentOS你可以使用这个命令来安装 yum install –y mlocate 。 前提是你的CentOS能连互联网。至于yum这个命令如何使用，到后续章节你自然会明白。如果你刚装上这个命令，初次使用会报错。
```
[root@localhost test]# locate passwd
-bash: locate: 未找到命令
[root@localhost test]#  yum install –y mlocate
已加载插件：fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.sohu.com
 * extras: mirrors.163.com
 * updates: mirrors.163.com
正在解决依赖关系
--> 正在检查事务
---> 软件包 mlocate.x86_64.0.0.26-8.el7 将被 安装
--> 解决依赖关系完成

依赖关系解决

=======================================================================================================================================================================
 Package                                 架构                                   版本                                        源                                    大小
=======================================================================================================================================================================
正在安装:
 mlocate                                 x86_64                                 0.26-8.el7                                  base                                 113 k

事务概要
=======================================================================================================================================================================
安装  1 软件包

总下载量：113 k
安装大小：379 k
Is this ok [y/d/N]: y
Downloading packages:
mlocate-0.26-8.el7.x86_64.rpm                                                                                                                   | 113 kB  00:00:01     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在安装    : mlocate-0.26-8.el7.x86_64                                                                                                                          1/1 
  验证中      : mlocate-0.26-8.el7.x86_64                                                                                                                          1/1 

已安装:
  mlocate.x86_64 0:0.26-8.el7                                                                                                                                          

完毕！
[root@localhost test]# 
```
```
[root@localhost test]# locate passwd
locate: 无法执行 stat () `/var/lib/mlocate/mlocate.db': 没有那个文件或目录
```
这是因为系统还没有生成那个文件列表库。你可以使用updatedb命令立即生成（更新）这个库。如果你的服务器上正跑着重要的业务，那么你最好不要去运行这个命令，因为一旦运行，服务器的压力会变大。这个数据库默认情况下每周更新一次。所以你用locate命令去搜索一个文件，正好是在两次更新时间段内，那你肯定是得不到结果的。你可以到/etc/updated.conf去配置这个数据库生成（更新）的规则。locate命令用的也并不多，所以你只要明白有这么一个东西即可。
```
[root@localhost test]# updatedb
[root@localhost test]# locate passwd
/etc/passwd
/etc/passwd-
/etc/pam.d/passwd
/etc/security/opasswd
/usr/bin/gpasswd
/usr/bin/grub2-mkpasswd-pbkdf2
/usr/bin/passwd
/usr/lib/firewalld/services/kpasswd.xml
/usr/lib64/security/pam_unix_passwd.so
/usr/sbin/chpasswd
/usr/sbin/lpasswd
/usr/sbin/saslpasswd2
/usr/share/awk/passwd.awk
/usr/share/doc/passwd-0.79
/usr/share/doc/passwd-0.79/AUTHORS
/usr/share/doc/passwd-0.79/COPYING
/usr/share/doc/passwd-0.79/ChangeLog
/usr/share/doc/passwd-0.79/NEWS
/usr/share/locale/ar/LC_MESSAGES/passwd.mo
/usr/share/locale/as/LC_MESSAGES/passwd.mo
/usr/share/locale/ast/LC_MESSAGES/passwd.mo
/usr/share/locale/bg/LC_MESSAGES/passwd.mo
/usr/share/locale/bn/LC_MESSAGES/passwd.mo
/usr/share/locale/bn_IN/LC_MESSAGES/passwd.mo
/usr/share/locale/bs/LC_MESSAGES/passwd.mo
/usr/share/locale/ca/LC_MESSAGES/passwd.mo
/usr/share/locale/cs/LC_MESSAGES/passwd.mo
/usr/share/locale/cy/LC_MESSAGES/passwd.mo
/usr/share/locale/da/LC_MESSAGES/passwd.mo
/usr/share/locale/de/LC_MESSAGES/passwd.mo
/usr/share/locale/el/LC_MESSAGES/passwd.mo
/usr/share/locale/en_GB/LC_MESSAGES/passwd.mo
/usr/share/locale/es/LC_MESSAGES/passwd.mo
/usr/share/locale/et/LC_MESSAGES/passwd.mo
/usr/share/locale/eu/LC_MESSAGES/passwd.mo
/usr/share/locale/fa/LC_MESSAGES/passwd.mo
/usr/share/locale/fi/LC_MESSAGES/passwd.mo
/usr/share/locale/fr/LC_MESSAGES/passwd.mo
/usr/share/locale/gl/LC_MESSAGES/passwd.mo
/usr/share/locale/gu/LC_MESSAGES/passwd.mo
/usr/share/locale/he/LC_MESSAGES/passwd.mo
/usr/share/locale/hi/LC_MESSAGES/passwd.mo
/usr/share/locale/hr/LC_MESSAGES/passwd.mo
/usr/share/locale/hu/LC_MESSAGES/passwd.mo
/usr/share/locale/hy/LC_MESSAGES/passwd.mo
/usr/share/locale/id/LC_MESSAGES/passwd.mo
/usr/share/locale/is/LC_MESSAGES/passwd.mo
/usr/share/locale/it/LC_MESSAGES/passwd.mo
/usr/share/locale/ja/LC_MESSAGES/passwd.mo
/usr/share/locale/ka/LC_MESSAGES/passwd.mo
/usr/share/locale/kn/LC_MESSAGES/passwd.mo
/usr/share/locale/ko/LC_MESSAGES/passwd.mo
/usr/share/locale/ku/LC_MESSAGES/passwd.mo
/usr/share/locale/lo/LC_MESSAGES/passwd.mo
/usr/share/locale/mk/LC_MESSAGES/passwd.mo
/usr/share/locale/ml/LC_MESSAGES/passwd.mo
/usr/share/locale/mr/LC_MESSAGES/passwd.mo
/usr/share/locale/ms/LC_MESSAGES/passwd.mo
/usr/share/locale/my/LC_MESSAGES/passwd.mo
/usr/share/locale/nb/LC_MESSAGES/passwd.mo
/usr/share/locale/nds/LC_MESSAGES/passwd.mo
/usr/share/locale/nl/LC_MESSAGES/passwd.mo
/usr/share/locale/nn/LC_MESSAGES/passwd.mo
/usr/share/locale/or/LC_MESSAGES/passwd.mo
/usr/share/locale/pa/LC_MESSAGES/passwd.mo
/usr/share/locale/pl/LC_MESSAGES/passwd.mo
/usr/share/locale/pt/LC_MESSAGES/passwd.mo
/usr/share/locale/pt_BR/LC_MESSAGES/passwd.mo
/usr/share/locale/ro/LC_MESSAGES/passwd.mo
/usr/share/locale/ru/LC_MESSAGES/passwd.mo
/usr/share/locale/si/LC_MESSAGES/passwd.mo
/usr/share/locale/sk/LC_MESSAGES/passwd.mo
/usr/share/locale/sl/LC_MESSAGES/passwd.mo
/usr/share/locale/sq/LC_MESSAGES/passwd.mo
/usr/share/locale/sr/LC_MESSAGES/passwd.mo
/usr/share/locale/sr@latin/LC_MESSAGES/passwd.mo
/usr/share/locale/sv/LC_MESSAGES/passwd.mo
/usr/share/locale/ta/LC_MESSAGES/passwd.mo
/usr/share/locale/te/LC_MESSAGES/passwd.mo
/usr/share/locale/tr/LC_MESSAGES/passwd.mo
/usr/share/locale/uk/LC_MESSAGES/passwd.mo
/usr/share/locale/ur/LC_MESSAGES/passwd.mo
/usr/share/locale/vi/LC_MESSAGES/passwd.mo
/usr/share/locale/wa/LC_MESSAGES/passwd.mo
/usr/share/locale/zh_CN/LC_MESSAGES/passwd.mo
/usr/share/locale/zh_TW/LC_MESSAGES/passwd.mo
/usr/share/man/cs/man1/gpasswd.1.gz
/usr/share/man/de/man1/gpasswd.1.gz
/usr/share/man/de/man8/chpasswd.8.gz
/usr/share/man/fr/man1/gpasswd.1.gz
/usr/share/man/fr/man8/chpasswd.8.gz
/usr/share/man/hu/man1/gpasswd.1.gz
/usr/share/man/it/man1/gpasswd.1.gz
/usr/share/man/it/man8/chpasswd.8.gz
/usr/share/man/ja/man1/gpasswd.1.gz
/usr/share/man/ja/man1/passwd.1.gz
/usr/share/man/ja/man8/chpasswd.8.gz
/usr/share/man/man1/gpasswd.1.gz
/usr/share/man/man1/grub2-mkpasswd-pbkdf2.1.gz
/usr/share/man/man1/lpasswd.1.gz
/usr/share/man/man1/passwd.1.gz
/usr/share/man/man1/sslpasswd.1ssl.gz
/usr/share/man/man8/chpasswd.8.gz
/usr/share/man/pt_BR/man1/gpasswd.1.gz
/usr/share/man/ru/man1/gpasswd.1.gz
/usr/share/man/ru/man8/chpasswd.8.gz
/usr/share/man/zh_CN/man1/gpasswd.1.gz
/usr/share/man/zh_CN/man8/chpasswd.8.gz
/usr/share/man/zh_TW/man8/chpasswd.8.gz
[root@localhost test]#
```
**find** 这个搜索工具是用的最多的一个，所以请你务必要熟悉它。

语法： find [路径] [参数] 

下面介绍几个常用的参数:

-atime +n ：访问或执行时间大于n天的文件

-ctime +n ：写入、更改inode属性（例如更改所有者、权限或者连接）时间大于n天的文件

-mtime +n ：写入时间大于n天的文件

文件的Access time，atime是在读取文件或者执行文件时更改的。
文件的Modified time，mtime是在写入文件时随文件内容的更改而更改的。
文件的Create time，ctime是在写入文件、更改所有者、权限或链接设置时随Inode的内容更改而更改的。 
因此，更改文件的内容即会更改mtime和ctime，但是文件的ctime可能会在mtime未发生任何变化时更改，例如，更改了文件的权限，但是文件内容没有变化。 如何获得一个文件的atime mtime 以及ctime ？

ls -l 命令可用来列出文件的atime、ctime 和mtime。

ls -lc filename         列出文件的 ctime

ls -lu filename         列出文件的 atime

ls -l filename          列出文件的 mtime    

atime不一定在访问文件之后被修改，因为：使用ext3文件系统的时候，如果在mount的时候使用了noatime参数那么就不会更新atime的信息。而这是加了noatime取消了, 不代表真实情況。反正, 这三个time stamp都放在inode中。若 mtime, atime修改inode就一定會改, 既然inode改了, 那ctime也就跟著要改了。

继续讲find常用的参数。

-name filename 直接查找该文件名的文件，这个使用最多了。
```
[root@localhost test]# ls
111  123  test  test1  test2  test3  test4  test5
[root@localhost test]# find /tmp/test -name test
/tmp/test
/tmp/test/test
/tmp/test/test4/test
[root@localhost test]# 
```
-type：通过文件类型查找。文件类型在前面部分已经简单介绍过，相信你已经大体上了解了。type包含了f,b,c,d,l, s等等。后续的内容还会介绍文件类型的。
```
[root@localhost test]# mkdir file1
[root@localhost test]# mkdir file1/file2
[root@localhost test]# touch file1/file3
[root@localhost test]# touch file1/file2/file4
[root@localhost test]# find ./file1 -type d
./file1
./file1/file2
[root@localhost test]# find ./file1 -type f
./file1/file2/file4
./file1/file3
[root@localhost test]#
```
## Linux文件类型
在前面的内容中简单介绍了普通文件(-)，目录(d)等，在Linux文件系统中，主要有以下几种类型的文件。

1. 正规文件（regular file）：就是一般类型的文件，当用ls –l查看某个目录时，第一个属性为“-”的文件就是正规文件，或者叫普通文件。正规文件又可分成纯文字文件（ascii）和二进制文件（binary）。纯文本文件是可以通过cat, more, less等工具直接查看内容的，而二进制文件并不能。例如我们用的命令/usr/bin/ls这就是一个二进制文件。

2. 目录（directory）：这个很容易理解，就是目录，跟Windows下的文件夹一个意思，只不过在Linux中我们不叫文件夹，而是叫做目录。ls –l查看第一个属性为“d”。

3. 连接档（link）：ls –l查看第一个属性为“l”，类似Windows下的快捷方式。这种文件在Linux中很常见，而且在日常的系统运维工作中用的很多，所以你要特意留意一下这种类型的文件。在后面还会有介绍。

4. 设备档（device）：与系统周边相关的一些档案，通常都集中在/dev这个目录之下！通常又分为两种：区块(block) 设备档 ：就是一些储存数据，以提供系统存取的接口设备，简单的说就是硬盘啦！例如你的一号硬盘的代码是/dev/hda1 等等的档案啦！第一个属性为“b”；字符(character)设备档：亦即是一些串行端口的接口设备，例如键盘、鼠标等等！第一个属性为“c”。

## Linux文件后缀名

在Linux系统中，文件的后缀名并没有具体意义，也就是说，你加或者不加，都无所谓。但是为了容易区分，Linux爱好者们都习惯给文件加一个后缀名，这样当用户看到这个文件名时就会很快想到它到底是一个什么文件。例如1.sh,2.tar.gz,my.cnf,test.zip等等，如果你首次接触这些文件，你也许会感到很晕，没有关系，随着学习的深入，你就会逐渐的了解这些文件了。列举的几个文件名中1.sh代表它是一个shell script ，2.tar.gz代表它是一个压缩包，my.cnf代表它是一个配置文件，test.zip代表它是一个压缩文件。

另外需要你知道的是，早期Unix系统文件名最多允许14个字符，而新的Unix或者Linux系统中，文件名最长可以到达 256个字符！
