---
title: 《Maven实战》学习（三）
date: 2018-04-13 18:48:20
tags:
  - Maven
  - Java
categories: Java
---
<img src="http://p6v6hsmcp.bkt.clouddn.com/1184808_e345.jpg" alt="tools" style="width:100%" />
<!-- more -->
## 背景案例
接下里的Maven学习，将引入一个真实的案例来进行学习，该案例的目的还是帮助我们理解Maven的概念， 以及展示大部分Maven项目需要面对和处理的一些问题。
### 简单的注册服务
注册互联网账户是日常生活中再熟悉不过的一件事悄 ，作为一个用户，注册账户的时候往往需要做以下事情：
* 提供一个未被使用的账号ID
* 提供一个未被使用的Email地址
* 提供一个任意的显示名称
* 设置安全密码，并重复输入以确认
* 输入验证码
* 前往邮箱查收激活链接并单击激活账号
* 登录

写这些的主要目的是让读者清楚地了解这个背景案例 即账户注册服务，它的需求是什么？基于这样的一个需求 ，我们会怎样设计这个小型的系统。
### 需求阐述
下面从软件工程的视角来分析一下该服务的需求：
#### 需求用例
> **注册账户**
> 主耍场景：
>>1.用户访问注册页面
>>2.系统生成验证码图片
>>3.用户输入想要的ID、Email地址，想要的显示名称、密码、确认密码
>>4.用户输入验证码
>>5.用户提交注册请求
>>6.系统检查验证码
>>7.系统检查ID是否已经被注册，Email是否已经被注册，密码和确认密码是否一致
>>8.系统保存未激活的账户信息
>>9.系统生成激活链接，井发送至用户邮箱
>>10.用户打开邮箱，访问激活链接
>>11.系统解析激活链接，激活相关账户
>>12.用户使用ID和密码登录

> 扩展场景
>> 4a: 用户无法看清验证玛，请求重新生成
>>>1.跳转到步骤2

>> 6a: 系统检测到用户输入的验证码错误
>>>1.系统提示验证码错误
>>>2.跳转到步骤2

>> 7a: 系统检测到ID已被注册，或者Email已被注册，或者密码和确认密码不一致
>>>1.系统提示相关错误信息 
>>>2.跳转到步骤2

该用例的角色只有两个：用户和系统。“主要场景”描述了用户如何与系统一步一步地交互，并且成功完成注册; "扩展场景”则描述了一些中途发生意外的情形，比如用户输错验证码的时候，系统就需要从新生成验证码，用户也需要重新输入验证码。
### 简要设计
#### 接口
从需求用例中可以看到，系统对外的接口包括生成验证码图片、处理注册请求、激活账户以及处理登录等。
#### 模块结构
* cn.jgh.account.service: 系统的核心，它封装了所有下层细节，对外暴露简单的接口。
* cn.jgh.account.web: 顾名思义该模块包含所有与web相关的内容，包括可能的JSP、Servlet、web.xml等，它直接依赖于cn.jgh.account.service模块，使用其提供的服务。
* cn.jgh.maven.account.persist: 处理账户信息的持久化，包括增、删、改、查等。根据实现，可以基于数据库或者文件。
* cn.jgh.account.captcha: 处理验证码的key生成、图片生成以及验证等。这里需要第三方的类库来帮助实现这些功能。
* cn.jgh.account.email: 处理邮件服务的配置、激活邮件的编写和发送。

## 坐标和依赖
正如前面介绍的， Maven的一大功能是管理项目依赖。 为了能自动化地解析任何一个Java构件，Maven就必须将它们唯一标识，这就依赖管理的底层基础——坐标。
### 何为Maven坐标？
Maven的世界中拥有数量非常巨大的构件，也就是平时用的一些jar、war等构件。在Maven为这些构件引人坐标概念之前，我们无法使用任何一种方式来唯一标识所有这些构件。 因此，当需要用到SpringFramework依赖的时候，大家会去SpringFramework网站寻找；当需要用到log4j依赖的时候,大家又会去Apache网站寻找。又因为各个项目的网站风格迥异，大量的时间花费在了搜索、浏览网页等工作上面。 没有统一的规范、统一的法则，该工作就无法自动化。Maven定义了这样一组规则：世界上任何一个构件都可以使用Maven坐标唯一标识，Maven坐标的元素包括groupID、artifactId、version、packaging、classifier。现在，只要我们提供正确的坐标元素，Maven就能找到对应的构件。Maven是从哪里下载构件的呢？答案其实很简单，Maven内置了一个[中央仓库](https://repo.maven.apache.org/maven2/)的地址,该中央仓库包含了世界上大部分流行的开源项目构件， Maven会在需要的时候去那里下载。
### 坐标详解
一组Maven坐标是通过一些元素定义的，它们是groupId、artifactId、version、packaging、classifier。看下面的代码：
```xml
<groupId>org.sonatype.nexus</groupId> 
<artifactId>nexus-indexer</artifactId> 
<version>2.0.0</version> 
<packaging>jar</packaging> 
```
**groupId：**定义当前Maven项目隶属的实际项目。 首先，Maven项目和实际项目不一定是一对一的关系。 比如 SpringFramework这一实际项目，其对应的Maven项目会有很多，如spring-core、spring-context等。 这是由于Maven中模块的概念，因此，一个实际项目往往会被划分成很多模块。其次，groupId不应该对应项目隶属的组织或公司。原因很简单，一个组织下会有很多实际项目，如果groupId只定义到组织级别,而后面我们会看到artifactId只能定义Maven项目（模块），那么实际项目这个层将难以定义。 最后，groupId的表示方式与Java 包名的表示方式类似，通常与域名反向一一对应。 上面的代码中，groupId为org.sonatype.nexus，org.sonatype表示Sonatype公司建立的一个非盈利性组织，nexus表示Nexus这一实际项目，该groupId与域名 nexus.sonatype.org对应。  

**artifactId：**该元素定义实际项目中的一个Maven项目（模块），推荐的做法是使用实际项目名称作为artifactId的前缀。比如上面的代码中的artifactId是nexus-indexer,使用了实际项目名nexus作为前缀，这样做的好处是方便寻找实际构件。在默认情况下，Ma­ven生成的构件，其文件名会以artifactId作为开头，如nexus-indexer-2.0.0.jar, 使用实际项目名称作为前缀之后，就能方便从一个lib文件夹中找到某个项目的一组构件。 考虑有5个项目，每个项目都有一个core模块，如果没有前缀，我们会行到很多core-1.2.jar这样的文件，加上实际项目名前缀之后， 便能很容易区分foo-core-1.2.jar、bar-core-1.2.jar......  

**version：**该元素定义Maven项目当前所处的版本，如上面的代码中，nexus-indexer的版本是2.0.0。需要注意的是，Maven定义了一套完成的版本规范，以及快照(SNAPSHOT)的概念。 后面会详细讨论版本管理内容。 
 
**packaging：**该元素定义Maven项目的打包方式。 首先，打包方式通常与所生成构件的文件扩展名对应，如上面的代码中packaging为jar。 最终的文件名为nexus-indexer-2.0.0.jar,而使用war打包方式的Maven项目，最终生成的构件会有一个.war文件，不过这不是绝对的。 其次，打包方式会影响到构建的生命周期，比如jar打包和war打包会使用不同的命令。最后，当不定义packaging的时候，Maven会使用默认值jar。  

­**classifier：**该元素用来帮助定义构建输出的一些附属构件。附属构件与主构件对应，如上例中的主构件是nexus-indexer-2.0.0.jar, 该项目可能还会通过使用一些插件生成如nexus-indexer-2.0.0-javadoc.jar、 nexus-indexer-2.0.0-sources.jar这样一些附属构件，其包含了Java文档和源代码。这时候，javadoc和sources就是这两个附属构件的classifier。这样，附属构件也就拥有了自己唯一的坐标。 还有一个关于classifier的典型例子是TestNG, TestNG的主构件是基于Java1.4平台的，而它又提供了一个clas­sifier为jdk5的附属构件。注意，不能直接定义项目的classifier, 因为附属构件不是项目直接默认生成的，而是由附加的插件帮助生成。  

上述5个元素中，groupId、artifactId、version是必须定义的，packaging是可选的（默认为jar),而classifier是不能直接定义的。  

同时，项目构件的文件名是与坐标相对应的，一般的规则为artifactId-version[-classifier].packaging, [-classifier]表示可选。比如上例nexus-indexer的主构件为nexus-indexer-2.0.0.jar,附属构件有nexus-indexer-2.0.0-javadoc.jar。 这里还要强调的一点是，packaging并非一定与构件扩展名对应，比如packaging为maven-plugin的构件扩展名为jar。
### account-email
回想在上面提到的背景案例，案例中有一个email模块负责发送账户激活的电子邮件，本节就详细阐述该模块的实现，包括POM配置、主代码和测试代码。由于该背景案例的实现是基于Spring Framework, 因此还会涉及相关的 Spring配置。
#### account-email的POM
该模块的POM代码如下所示
```