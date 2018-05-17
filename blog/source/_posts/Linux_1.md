---
title: Linux入门（一）初始Linux
date: 2018-05-16 20:50:20
tags:
  - Linux
categories: Linux
---
<img src="http://p6v6hsmcp.bkt.clouddn.com/linux.jpg" alt="Linux" style="width:100%" />
<!-- more -->
在介绍Linux前，我想先针对大家如何对Linux的发音说一下。我发现我身边的朋友对Linux的发音大致有这么几种： “里那克斯”与“里你克斯”“里扭克斯”等。其实官方的标准发音为 ['li:nэks]，因为这个发音是创始人Linus的发音。如果你不认识这个音标，那么就读成“里那克斯”。而我习惯发音成“里扭克斯”，当然你发音成什么，并没有人会说你，完全是一个人的习惯而已。
## 操作系统的发展史
### Unix
1965年之前的时候，电脑并不像现在一样普遍，它可不是一般人能碰的起的，除非是军事或者学院的研究机构，而且当时大型主机至多能提供30台终端（30个键盘、显示器)，连接一台电脑。
<img src="http://p6v6hsmcp.bkt.clouddn.com/Unix.png" alt="Unix" style="width:100%" />

为了解决数量不够用的问题,1965年左右由贝尔实验室、麻省理工学院以及通用电气共同发起了Multics项目，想让大型主机支持300台终端。

1969年前后这个项目进度缓慢，资金短缺，贝尔实验室退出了研究。

1969年从这个项目中退出的Ken Thompson当时在实验室无聊时，为了让一台空闲的电脑上能够运行“星际旅行”游行，在8月份左右趁着其妻子探亲的时间，用了1个月的时间 编写出了Unix操作系统的原型。

1970年，美国贝尔实验室的Ken Thompson，以BCPL语言为基础，设计出很简单且很接近硬件的B语言（取BCPL的首字母），并且他用B语言写了第一个Unix操作系统。

因为B语言的跨平台性较差，为了能够在其他的电脑上也能够运行这个非常棒的Unix操作系统，Dennis Ritchie和Ken Thompson从B语言的基础上准备研究一个更好的语言。

1972年，美国贝尔实验室的Dennis Ritchie在B语言的基础上最终设计出了一种新的语言，他取了BCPL的第二个字母作为这种语言的名字，这就是C语言。

1973年初，C语言的主体完成。Thompson和Ritchie迫不及待地开始用它完全重写了现在大名鼎鼎的Unix操作系统。
### Minix
因为AT&T(通用电气)的政策改变，在Version7 Unix推出之后，发布新的使用条款，将Unix源代码私有化，在大学中不再能使用Unix源代码。AndrewS.Tanenbaum(塔能鲍姆)教授为了能在课堂上教授学生操作系统运作的实务细节，决定在不使用任何AT&T的源代码前提下，自行开发与Unix兼容的操作系统，以避免版权上的争议。他以小型Unix（mini-UNIX）之意，将它称为Minix。
### Linux
因为Minix只是教学使用，因此功能并不强，因此Torvalds利用GNU的bash当做开发环境，gcc当做编译工具，编写了Linux内核-v0.02，但是一开始Linux并不能兼容Unix，即Unix上跑的应用程序不能在Linux上跑，即应用程序与内核之间的接口不一致，因为Unix是遵循POSIX规范的，因此Torvalds修改了Linux，并遵循POSIX（Portable Operating System Interface，他规范了应用程序与内核的接口规范）； 一开始Linux只适用于386，后来经过全世界的网友的帮助，最终能够兼容多种硬件。
### 操作系统的发展
Unix和Linux的应用范围
<img src="http://p6v6hsmcp.bkt.clouddn.com/os.gif" alt="Unix" style="width:100%" />
Linux的应用领域
<img src="http://p6v6hsmcp.bkt.clouddn.com/os1.png" alt="Unix" style="width:100%" />
ios的来源
<img src="http://p6v6hsmcp.bkt.clouddn.com/os2.png" alt="Unix" style="width:100%" />
### Minix没有火起来的原因
> Minix的创始人说，Minix3没有统治世界是源于他在1992年犯下的一个错误，当时他认为BSD必然会一统天下，因为它是一个更稳定和更成熟的系统，其它操作系统难以与之竞争。因此他的Minix的重心集中在教育上。四名BSD开发者已经成立了一家公司销售BSD系统，他们甚至还有一个有趣的电话号码1-800-ITS-UNIX。然而他们正因为这个电话号码而惹火上身。美国电话电报公司因电话号码而提起诉讼。官司打了三年才解决。在此期间，BSD陷于停滞，而Linux则借此一飞冲天。他的错误在于没有意识官司竟然持续了如此长的时间，以及BSD会因此受到削弱。如果美国电话电报公司没有起诉，Linux永远不会流行起来，BSD将统治世界。

## Linux的不同版本以及应用领域
### Linux内核版本
内核(kernel)是系统的心脏，是运行程序和管理像磁盘和打印机等硬件设备的核心程序，它提供了一个在裸设备与应用程序间的抽象层。

Linux内核版本又分为稳定版和开发版，两种版本是相互关联，相互循环：
 
* 稳定版：具有工业级强度，可以广泛地应用和部署。新的稳定版相对于较旧的只是修正一些bug或加入一些新的驱动程序。
* 开发版：由于要试验各种解决方案，所以变化很快。

[Linux官网](https://www.kernel.org/)，所有来自全世界的对Linux源码的修改最终都会汇总到这个网站，由Linus领导的开源社区对其进行甄别和修改最终决定是否进入到Linux主线内核源码中。
### Linux发行版本
Linux发行版(也被叫做GNU/Linux发行版)说简单点就是将Linux内核与应用软件做一个打包。较知名的发行版有：Ubuntu、RedHat、CentOS、Debain、Fedora、SuSE、OpenSUSE、TurboLinux、BluePoint、RedFlag、Xterm、SlackWare等。
#### RHEL (RedHat Enterprise Linux)
RHEL的地位就像Ubuntu在Linux桌面发行版中的地位一样。Red Hat Enterprise Linux背后的红帽公司是Linux早期最大的企业。多年来，他们不断地对RHEL进行改进，确保大多数软件包和硬件都是RHEL支持或“认证”的。除了认证状态外，长期支持在顶级Linux服务器操作系统中也很重要。该公司声称，全球“财富”500强中有90％的公司使用RHEL，这个数字可以说是非常多了。
#### CentOS
如果你不想花钱，但又希望有使用RHEL一般的体验，最好的方法是下载免费的Linux服务器发行版CentOS。从另一个角度来看，这是社区支持的RHEL，但是没有Red Hat的支持。CentOS与RHEL是二进制兼容的，RHEL也成为了该选项很重要的加分点。就像RHEL一样，CentOS软件库包含经过测试的软件，它对生产系统来说是安全和稳定的。说到控制面板，cPanel让CentOS可以提供更好的支持。如果熟悉.rpm软件包和yum软件包管理器，CentOS就是最佳Linux服务器操作系统。
#### Ubuntu LTS
与RHEL和CentOS相比，Ubuntu LTS获取软件包的速度更快。这个选择再次归结为用户的特殊需求，如果想要某些应用程序和软件上的所有最新功能，请转到Ubuntu。此外，它拥有广泛的社区，对于愿意跳入Linux世界的初学者而言，也建议这样做。

谈到性能，Ubuntu提供了一个灵活的性能。开发者可以选择Ubuntu服务器选项，它提供了一些有用的软件包比如邮件服务器，LAMP服务器，Samba文件服务器，OpenStackMitaka，Nginx等。该发行版的5年LTS支持也保证了初学者的使用，开发者可以设置媒体服务器，电子邮件服务器或游戏服务器等场景。
#### Debian
为什么一些流行的Linux发行版是基于Debian的呢？这归结于Debian的稳定性。尽管开发者希望在Debian上做的任何事情都可以在Ubuntu上实现，但是如果高于平均水平，稳定性就变得非常重要，我们建议开发者选择Debian。另外要记住的是，Debian只附带免费软件。与Ubuntu相比，它更轻巧，速度更快，这使得它成为旧硬件的合适选择。

简而言之，如果你正在考虑安全问题并且熟悉Linux的企业环境，并且你需要一个Linux服务器操作系统，相比于Ubuntu，选择Debian更合适。
#### SLES (SUSE Linux Enterprise Server)
就像RHEL和Canonical一样，SUSE在开源世界中也是一个非常有名的发行版。它始于1992年，是第一家为企业客户推销Linux的公司。该公司的主要产品是SUSE Linux Enterprise Server。 最近与SAP和微软达成合作关系，这也将其带入了不同的企业服务市场。

使用SLES（和OpenSUSE）的一大优势是YaST软件管理系统，这使许多复杂的任务变得更简单和自动化。它有用户的GUI和命令行界面，这就是SLES也被称为管理友好型Linux发行版的原因。它是为正在寻找多用途Linux服务器发行版的高级用户而打造的。

### 应用领域
#### 个人桌面领域
此领域是传统linux应用最薄弱的环节，传统linux由于界面简单、操作复杂、应用软件少的缺点，一直被windows所压制，但近些年来随着ubuntu、fedora等优秀桌面环境的兴起，同时各大硬件厂商对其支持的加大，linux在个人桌面领域的占有率在逐渐的提高。

典型代表：ubuntu、fedora、suse linux等

#### 服务器领域
linux在服务器领域的应用是其重要分支。

linux免费、稳定、高效等特点在这里得到了很好的体现，但早期因为维护、运行等原因同样受到了很大的限制，但近些年来linux服务器市场得到了飞速的提升，尤其在一些高端领域尤为广泛。

典型代表：Red Hat公司的AS系列、完全开源的debian系列、suse EnterPrise11系列等。
#### 嵌入式领域

linux运行稳定、对网络的良好支持性、低成本，且可以根据需要进行软件裁剪，内核最小可以达到几百KB等特点，使其近些年来在嵌入式领域的应用得到非常大的提高。

主要应用：机顶盒、数字电视、网络电话、程控交换机、手机、PDA、等都是其应用领域，得到了摩托罗拉、三星、NEC、Google等公司的大力推广。