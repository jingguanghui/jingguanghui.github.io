---
title: SecureCRT的使用
date: 2018-05-17 23:26:20
tags:
  - Linux
  - SecureCRT
categories: Linux
---
<img src="http://p6v6hsmcp.bkt.clouddn.com/pexels-photo.jpg" alt="SecureCRT" style="width:100%" />
<!-- more -->
## SecureCRT简介
SecureCRT是一款支持SSH（SSH1和SSH2）的终端仿真程序，简单地说是Windows下登录UNIX或Linux服务器主机的软件。
## 解决SecureCRT中文乱码
本地windows机器。修改SecureCRT的设置。找到“选项”->“会话选项”->“外观”：

* 字符编码设置为utf-8。
* 字体设置，选择中文字体，例如新宋体。
* 重新启动SecureCRT即可

## 发送文件到Linux系统
1. 右键单击SecureCRT控制台左上角的ip地址
2. 找到“连接SFTP标签页(S)”并单击，进入SFTP标签页
3. 可以使用help命令查看SFTP下面所有的功能
4. 使用命令lcd进入客户端要发送给服务器所在文件的文件夹中，例如：lcd D:\software
5. 使用命令lls可查看该文件下所有的文件，例如：lls D:\software
6. 使用命令cd进入服务器接受文件的文件中。例如： cd /usr/jdk
7. 使用命令put 加上要发送的文件名即可发送文件到服务器，例如：put 1.txt
8. 使用命令ls查看服务器是否收到该文件
