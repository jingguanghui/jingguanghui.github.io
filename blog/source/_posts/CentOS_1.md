---
title: Centos7中查看本机ip地址
date: 2018-05-17 21:56:20
tags:
  - Linux
  - CentOS
categories: Linux
---
<img src="http://p6v6hsmcp.bkt.clouddn.com/pexels-photo-930530.jpeg" alt="CentOS" style="width:100%" />
<!-- more -->
可以输入以下命令查询
```
ip addr
```
 
```
ifconfig
```
如果输入ifconfig，出现
```
-bash: ifconfig: command not found
```
说明没有ifconfig，可以输入以下命令安装net-tools package
```
sudo yum install net-tools
```
安装完成后，则可以用ifconfig命令来查询ip地址。

输入命令会出现3个条目，CentOS的ip地址是ens33条目中的inet值。 
<img src="http://p6v6hsmcp.bkt.clouddn.com/1497185138263086154.jpg" alt="CentOS" style="width:100%" />
发现ens33没有inet这个属性，那么就没法通过ip地址做远程连接了。
接着来输入以下命令来查看ens33网卡的配置：

``` 
vi /etc/sysconfig/network-scripts/ifcfg-ens33 
```
 
vi是Linux内置的文本编辑器命令，打开文件的意思。
<img src="http://p6v6hsmcp.bkt.clouddn.com/1497185328169099657.jpg" alt="CentOS" style="width:100%" />

从配置清单中可以发现CentOS7默认是不启动网卡的（ONBOOT=no）。
把这一项改为yes（ONBOOT=yes）,如下图所示：

<img src="http://p6v6hsmcp.bkt.clouddn.com/1497185415060083062.jpg" alt="CentOS" style="width:100%" />

<div class="note warning"><p>vi打开时是处于只读状态的，可以按insert键或者A使其处于编辑状态。</p></div>

修改完成后，按Esc退出，然后输入命令:wq，再按Enter即可。

<div class="note info"><p>:wq是保存然后退出的意思；:q!是不保存直接退出的意思</p></div>
最后输入以下命令重启网络服务即可：

```
sudo service network restart
```

然后我们再输入ip attr或者ifconfig,即可看到ip地址
```
[root@localhost ~]# ip attr
Object "attr" is unknown, try "ip help".
[root@localhost ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:6f:51:d5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.43.137/24 brd 192.168.43.255 scope global noprefixroute dynamic ens33
       valid_lft 3198sec preferred_lft 3198sec
    inet6 fe80::dc72:7667:d658:eff6/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

这里的192.168.43.137即是我们需要的ip地址。