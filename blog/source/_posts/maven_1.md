---
title: 《Maven实战》学习（一）
date: 2018-04-11 13:08:20
tags:
  - Maven
  - Java
categories: Java
---
<img src="http://p6v6hsmcp.bkt.clouddn.com/1184808_e345.jpg" alt="tools" style="width:100%" />
<!-- more -->
## Maven简介
### 何为Maven？
Maven是优秀的构建工具。
### 为什么要用Maven？
Maven能够帮助我们自动化构建过程，从清理、编译、测试到生成报告，再到打包和部署。

Maven是跨平台的，这意味着无论在Windows、Linux还是Mac上都可以使用相同的命令。

Maven最大化地消除了构建的重复，抽象了构建生命周期，并且为绝大部分的构建任务提供了插件，我们不需要实现过程，甚至不再需要去实现这些过程中的一些任务。最简单的例子就是测试，我们没必要告诉Maven去测试，更不需要告诉Maven如何运行测试，只需要遵循Maven的约定编好测试用例，当我们运行构建的时候，这些测试便会自动运行。

Maven不仅是构建工具，还是一个依赖管理工具和项目信息管理工具。它提供中央仓库，能帮我们自动下载构件。
## Maven的安装和配置
### 在Windows上安装Maven
#### 检查JDK的安装
Maven可以运行在JDK1.4及以上版本。

打开命令行，输入以下命令来检查JDK安装

```
echo %JAVA_HOME%
```

```
java -version
```
#### Maven下载
[Maven下载地址](https://maven.apache.org/download.cgi),Windows下是zip文件，如apache-maven-3.5.3-bin.zip。
#### 本地安装
在指定的目录中运行以下命令将安装文件解压到当前目录（如D:\Maven）
```
D:\Maven>jar xvf "D:\Downloads\apache-maven-3.5.3-bin.zip"
```

设置环境变量，将Maven安装配置到操作系统中：

1. 右击“计算机”->“属性”->“高级系统设置”->"环境变量"；
1. 在**系统变量**中新建一个变量，变量名为“M2_HOME”,变量值为Maven的安装目录D:\Maven\apache-maven-3.5.3，单击‘确定’按钮；
1. 在**系统变量**中找到Path变量，双击Path变量，在变量值末尾添加%M2_HOME%\bin;，（多个值之间用英文分号（;）隔开），单击‘确定’按钮。

<div class="note danger no-icon"><p>值得注意的是Path环境变量。当我们在cmd中执行命令行时，Windows首先会在当前目录中寻找可执行的脚本或文件，如果没找到，Windows会接着遍历环境变量Path中定义的路径。</p></div>打开一个新的命令行窗口，执行以下命令验证Maven是否安装成功  
```
mvn -v
```
执行以上之后命令出现类似如下结果，则说明安装成功<div class="note success"><p>Apache Maven 3.5.3 (3383c37e1f9e9b3bc3df5050c29c8aff9f295297; 2018-02-25T03:49:05+08:00)
Maven home: D:\Maven\apache-maven-3.5.3\bin\..
Java version: 1.8.0_144, vendor: Oracle Corporation
Java home: D:\Java\jdk1.8.0_144\jre
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 7", version: "6.1", arch: "amd64", family: "windows"
</p></div>

#### 升级Maven
只需要把新下载下来的文件解压到文件目录中，然后替换掉系统变量M2_HOME的变量值即可。
### 在基于UNIX的系统上安装Maven
#### 检查JDK的安装

打开命令行，输入以下命令来检查JDK安装

```
echo %JAVA_HOME%
```

```
java -version
```
#### Maven下载
[Maven下载地址](https://maven.apache.org/download.cgi),UNIX下是tar.gz文件，如apache-maven-3.5.3-bin.tar.gz。
#### 本地安装
在指定的目录中运行以下命令将安装文件解压到当前目录（如D:\Maven）
```
D:\Maven>tar-xvzf "D:\Downloads\apache-maven-3.5.3-bin.tar.gz"
```
现在已经创建好了一个Maven安装目录apache-maven-3.5.3，虽然直接使用该目录配置环境后就能使用Maven了，但这里推荐的做法是，在安装目录旁平行地创建一个符号链接，以便日后的升级：
```
ln-s apache-maven-3.5.3 apache-maven
```

```
ls-l
```

接下来，设置M2_HOME环境变量指向符号链接apache-maven,并且把Maven安装目录下的bin/文件添加到系统环境变量PATH中
```
export M2_HOME=/home/juven/bin/apache-maven
```

```
export PATH=$ PATH: $ M2-HOME/bin
```
运行以下命令检查Maven是否安装成功

```
mvn -v
```  
### 安装目录分析
#### M2_HOME
M2_HOME即Maven的安装目录，例如D:\Maven\apache-maven-3.5.3。

1. bin 该目录包含了mvn运行的脚本，这些脚本用来配置Java命令，准备好classpath和相关的Java系统属性，然后执行Java命令。
2. boot 该目录下只有一个文件，对于一般的Maven用户，不用关心该文件
3. conf 该目录下包含一个重要的文件settings.xml，后面会多次介绍该文件
4. lib 该目录包含了所有Maven运行的Java类库，用户还可以
5. LICENSE.txt记录了Maven使用的软件许可证Apache License Version2.0
6. NOTICE.txt记录了Maven包含的第三方软件
7. README.txt包含了Maven的简要介绍，以及安装需求和如何安装的简要指令

#### ~/.m2
在命令行窗口中执行mvn help:system（非必须）,然后可以在用户目录中（例如：C:\Users\Administrator）看到生成的.m文件夹，默认情况下，该文件夹下放置了Maven本地仓库.m2/repository。所有的Maven构件都被存储到该仓库中，以方便重用。
### maven安装最佳实践
#### 设置MAVEN_OPTS环境变量
通常设置MAVEN_OPTS的值为-Xms128m -Xmx512m,因为Java默认的最大可用内存往往不能满足Maven运行的需要,容易得到java.lang.OutofMemeoryError,所以一开始就配置是推荐的做法。

关于如何设置环境变量，请参考前面设置M2_HOME环境变量的做法，尽量不要直接修改mvn.bat或者mvn这两个Maven执行脚本文件。因为如果修改了脚本文件，升级Maven时就不得不再次去修改，同理，应该尽可能地不去修改Maven安装目录下的任何文件。
#### 配置用户范围的settings.xml
用户可以选择配置M2_HOME/conf/settings,xml或者~/.m2/settings.xml。前者是全局范围的，整台机器上的用户都会受到该配置的影响，而后者是用户范围的，只有当前用户才会受到该配置的影响。

推荐使用用户范围的settings.xml,一方面既不影响其他用户，另一方面升级之后也不用替换新升级文件夹下的settings.xml。
#### 不要使用IDE内嵌的maven