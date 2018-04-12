---
title: 《Maven实战》学习（二）
date: 2018-04-12 09:35:20
tags:
  - Maven
  - Java
categories: Java
---
<img src="http://p6v6hsmcp.bkt.clouddn.com/1184808_e345.jpg" alt="tools" style="width:100%" />
<!-- more -->
## Maven使用入门
### 编写POM
Maven的核心就是pom.xml。POM(Project Object Model,项目对象模型)定义了项目的基本信息，用于描述项目如何构建，声明项目如何依赖等等。

新建一个hello-world文件夹，打开该文件夹，新建pom.xml,输入如下内容：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd ">
  <modelVersion>4.0.0</modelVersion>
  <groupId>cn.jgh.hello</groupId>
  <artifactId>hello-world</artifactId>
  <version>1.0-SNAPSHOT</version>
  <name>hello-world</name>
</project>
```
代码第一行是XML头，指定了该xml的版本和编码方式。

project是所有pom.xml的根元素，他声明了一些POM相关的命名空间和xsd元素。

modelVersion指定了当前POM模型的版本，对于Maven2和Maven3来说，它只能是4.0.0。

这段代码中最重要的是包含groupId、artifactId和verison三行，这三个元素定义了一个项目的基本坐标，在Maven世界中，任何的jar、pom或者war都是以基于这些基本的坐标来进行区分的。

groupId定义了项目属于哪个组，这些组往往和项目所在的公司或组织存在关联，如果你的公司是mycom,有一个项目为myapp,那么groupId就应该为com.mycom.myapp。

artifactId定义了当前Maven项目在组中的唯一Id。

version指定了Hello World项目的当前版本——1.0-SNAPSHOT。SNAPSHOT意为快照，说明该项目还处于开发中，是不稳定的版本，随着项目的发展，verison会不断更新。

name不是必须的，但推荐写上，当在MyEclipse中导入时，这个名字将作为项目名。

### 编写主代码
项目主代码和测试代码不同，主代码会被打包到最终构件中（如jar）,而测试代码只在运行测试时用到，不会被打包。

默认情况下，Maven假设项目主代码位于src/main/java目录，我们遵循Maven的预定，创建该目录，然后在该目录下创建cn/jgh/hello/helloworld/HelloWorld.java,代码如下：
```java
package cn.jgh.hello.helloworld;
public class HelloWorld
{
	public String sayHello(){
		return "Hello Maven";
	}
	public static void main(String[] args){
		System.out.print(new HelloWorld().sayHello());
	}
}
```
关于该Java代码有两点需要注意，首先，在绝大多数情况下应该把项目主代码放到src/main/java目录下（遵循Maven的约定），而无需额外的配置，Maven会自动搜寻该目录找到项目主代码。其次，包名为cn.jgh.hello.helloworld，这与之前在POM中的定义的groupId和artifactId相吻合。一般来说，项目中的Jave包都应该基于项目的groupId和artifactId，这样更加清晰，更加符合逻辑，也方便搜索构件或Java类。

在项目根目录下运行如下命令行：
```
mvn clean compile
```
clean告诉Maven清理输出目录target/,compile告诉Maven编译项目主代码。首先Maven执行clean任务，删除target/目录。默认情况下，Maven构建的所有输出都在target/目录下，执行compile任务，将项目主代码编译至target/classes目录中（编译好的类为：cn/jgh/hello/helloworld/HelloWorld.class）

### 编写测试代码
为了使项目结构保持清晰，主代码与测试代码应该分别位于独立的目录中。

Maven项目中，默认的测试代码目录是src/test/java,因此，在编写测试用例之前，应该先创建该目录。

在Java世界中，一般使用JUnit进行单元测试，要使用JUnit,首先需要为Hello World项目添加一个JUnit依赖，修改项目的POM如下代码所示:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd ">
  <modelVersion>4.0.0</modelVersion>
  <groupId>cn.jgh.hello</groupId>
  <artifactId>hello-world</artifactId>
  <version>1.0-SNAPSHOT</version>
  <name>hello-world</name>
  <dependencies>
	<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.7</version>
		<scope>test</scope>
	</dependency>
  </dependencies>
</project>
```
代码中添加了dependencies元素，该元素下可包含多个dependency元素以声明项目的依赖。有了JUnit这段声明，Maven就会自动下载junit-4.7.jar。Maven从哪里下载这些Jar呢？在Maven之前，可以去JUnit的官方网站下载分发包，有了Maven，他会自动访问[Maven中央仓库](https://repo.maven.apache.org/maven2/)，下载需要的文件。我们也可以自己访问该仓库，打开路径junit/junit/4.7/，就可以看到junit-4.7.jar和junit-4.7.pom。

代码中scope为依赖范围，若依赖范围为test，则表示该依赖只对测试有效。换句话说，测试代码中的import Junit代码是没问题的，但是在主代码中用import Junit代码，就会造成编译错误。如果不声明依赖范围，则默认值为compile，表示该依赖对主代码和测试代码均有效。

接下来编写测试类，来验证上面编写的Jave类的sayHello()方法是否能在控制台输出“Hello Maven”。在src/test/java目录下创建文件，其内容如下代码所示：
```java
package cn.jgh.hello.helloworld;
import org.junit.Test;
public class HelloWorldTest
{
	@Test
	public void testSayHello(){
		HelloWorld helloWorld=new HelloWorld();
		System.out.println(helloWorld.sayHello());
	}
}
```
一个典型的单元测试包含三个步骤：
1. 准备测试类及数据
2. 执行要测试的行为
3. 检查结果

同时，在JUnit4中，需要执行的测试方法都应该以**@Test**进行标注。

测试用例编写完毕后就可以在项目根目录下调用Maven执行以下命令，进行测试
```
mvn clean test
```
命令行输入的是mvn clean test,而maven执行的可不止这两个任务，在执行test之前，它会先自动执行项目的主资源处理、主代码编译、测试资源处理、测试代码编译等工作，这是Maven生命周期的一个特性，关于Maven的生命周期，后面将会看到。

Maven从中央仓库下载了junit4.7.pom和junit4.7.jar这两个文件到本地仓库中（~/.m2/reposity）中，供所有的Maven项目使用。

如果控制台最后输出如下类似代码，则说明测试通过。
```
···
-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running cn.jgh.hello.helloworld.HelloWorldTest
Hello Maven
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.047 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
···
```
### 打包和运行
Hello World的POM中没有指定打包类型，使用默认的打包类型jar。简单地在项目根目录下调用以下命令进行打包
```
mvn clean package
```
可以看到类似如下结果
```
···
-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running cn.jgh.hello.helloworld.HelloWorldTest
Hello Maven
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.048 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
···
[INFO] Building jar: E:\mavenLearn\hello-world\target\hello-world-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 10.205 s
[INFO] Finished at: 2018-04-12T15:00:34+08:00
[INFO] ------------------------------------------------------------------------
···
```
类似地Maven会在打包之前进行编译、测试等操作。打包之后的文件也位于target/输出目录下，文件名为hello-world-1.0-SNAPSHOT.jar，它是根据artifact-version.jar规则进行命名的。如果有需要，就可以复制这个jar文件到其他项目的classpath中从而使用HelloWorld类。

如果要让其他Maven项目直接引用这个jar呢？还需要一个步骤，在项目根目录下执行以下代码
```
mvn clean install
```
可以看到类似如下结果
```
···
[INFO] Installing E:\mavenLearn\hello-world\target\hello-world-1.0-SNAPSHOT.jar t
epository\cn\jgh\hello\hello-world\1.0-SNAPSHOT\hello-world-1.0-SNAPSHOT.jar
[INFO] Installing E:\mavenLearn\hello-world\pom.xml to C:\Users\Administrator\.m2
-world\1.0-SNAPSHOT\hello-world-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 27.854 s
[INFO] Finished at: 2018-04-12T15:19:03+08:00
[INFO] ------------------------------------------------------------------------
···
```
同样地，在执行install之前，会执行删除target/,编译，测试和打包等操作。从输出结果中可以看出，install将项目输出的jar包安装到了Maven本地仓库中，可以打开相应的文件夹看到Hello World项目的pom和jar文件。只要将Hello World的构件安装到本地仓库之后，其他的Maven项目就可以使用它。

因为Hello World类是有一个main方法的，所以是可以运行的，但是默认打包成的jar是不能直接运行的，因为带有main方法的信息不会添加到manifest中(打开jar文件的META-INF/MANIFEST.MF文件，将无法看到Main-Class一行)。我们可以手动添加如下代码到jar文件的META-INF/MANIFEST.MF文件中：
```
Main-Class: cn.jgh.hello.helloworld.HelloWorld
```
需要注意的是，Main-Class:之后是一个英文状态下的空格

然后，在项目根目录下运行以下代码执行jar文件：
```
java -jar target\hello-world-1.0-SNAPSHOT.jar
```
控制台输出Hello Maven,这正是我们想要的。
### 使用Archetype生成项目骨架
离开当前目录另建一个项目，来测试Maven提供的Archetype帮助我们快速勾勒出项目骨架。

在新建的目录下，运行以下代码:
```
mvn archetype:generate
```
接下来将看到一段长长的输出，有很多可用的Archetype可供选择。每一个Archetype前面都会对应一个编号，同时命令行将会提示一个默认的编号，其对应的Archetype为maven-archetype-quickstart,直接回车以选择该Archetype,紧接着Maven会提示要输入项目的groupId、artifactId、version、以及包名package。如下输入并确认：
```
Define value for property 'groupId': cn.jgh.maven
Define value for property 'artifactId': hello-world
Define value for property 'version' 1.0-SNAPSHOT: :
Define value for property 'package' cn.jgh.maven: : cn.jgh.maven.helloworld
Confirm properties configuration:
groupId: cn.jgh.maven
artifactId: hello-world
version: 1.0-SNAPSHOT
package: cn.jgh.maven.helloworld
 Y: : Y
```
Archetype插件将根据我们提供的信息创建项目骨架。在当前目录下，Archetype插件会创建一个名为hello-world（根据我们定义的artifactId）的子目录，从中可以看到项目的基本结构：基本的pom.xml已经被创建，里面包含了必要的信息以及一个junit依赖；主代码目录src/main/java，在该目录下还有一个Java类cn.jgh.maven.helloworld.APP,同时测试代码目录src/test/java也已经被创建好,并且包含一个测试用例cn.jgh.maven.helloworld.APPTest。

Archetype可以帮助我们迅速地构建起项目的骨架，在以后的开发中我们就可以使用此种方法生成骨架，然后在此骨架的基础上开发项目以节省大量时间。
### m2eclipse的简单使用
#### 导入maven项目

1. 选择菜单项File->选择Import
2. 在Import对话框中选择Maven4MyEclipse下的Existing Maven Projects，然后单击next按钮
3. 在Impott Maven Projects对话框中单击Browse按钮选择Hello World的根目录（即包含pom.xml文件的那个目录）
4. 单击Finish按钮之后，就会将项目导入到当前的workspace中了

在Package Explore视图中,可以看到主代码目录结构src/main/java和测试代码目录结构src/test/java成了	MyEclipse中的资源目录，当然pom.xml永远在根目录下，还可以看到项目的依赖junit-4.7.jar,其实际位置指向了指向了Maven本地仓库。

#### 创建Maven项目
1. 选择菜单项File->New->Other
2. 在弹出的对话框中选择Maven4MyEclipse下的Maven Project,然后单击Next按钮
3. 在弹出的窗口中默认的选项，单击Next按钮
4. 此时会提示我们选择一个ArcheType，这里选择maven-archetype-quickstart插件创建项目，因此这一步骤和上一节使用的Archetype创建项目骨架类似，输入groupId、artifactId、version、package
5. 输入完毕后，单击Finish按钮，Maven项目就创建完成了

#### 运行Maven命令
在Maven项目或者pom.xml上右击，在弹出的快捷菜单中选择Run As，就能看到常见的Maven命令。选择想要执行的命令，就能执行相应的构件，同时还能在MyEclipse控制台看到构建输出。

可以看到Run As右边没有我们想要的命令，比如maven clean test,我们可以选择Maven build以自定义Maven运行命令,在弹出对话框的Goals一项中输入我们想要执行的命令，如clean test,设置一下Name,单击Run即可。