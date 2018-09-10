---
title: Maven基础
tags: 
  - maven
  - 构建工具
categories: 工具
date: 
---
# What is Maven ?
maven: 专家，内行，可以理解为知识的累积
maven官网：[http://maven.apache.org/](http://maven.apache.org/)
本文参考书籍：Maven实战（Maven in action 许晓斌 著）
定义：它是一个跨平台的项目管理工具
功能：Maven主要服务于基于Java平台的项目构建、依赖管理和项目信息管理
# 项目构建

几种构建工具,如 IDE、 Make、 Ant、 Gradle...

## Make
Make由一个名为Makefile的脚本文件驱动，该文件使用Make自己定义的语法格式。它的组成成分为一系列的规则，每一条规则又包括目标（Target）、依赖（Prerequisite）和命令(Command)。

例子：HelloWorld的makefile文件如下:

		HelloWorld : HelloWorld.java
        	echo "开始编译HelloWorld.java"
        	javac HelloWorld.java
        	echo "开始执行HelloWorld.class"
        	java HelloWorld

执行: make -s
	
		开始编译HelloWorld.java
		开始执行HelloWorld.class
		HelloWorld
优点：可以利用系统的本地命令，能够快速高效的完成任务。
缺点：将自身与操作系统邦定在一起，很难实现跨平台的构建，语法格式要求严格。
## Ant
(Another Neat Tool),意为“另一个简洁的工具”，Ant使用XML定义构建脚本。与Make相似，Ant有一个构建脚本build.xml，该文件的基本结构也是目标（target)、依赖（depends）以及实现目标的任务，可以把它看作Java版本的Make。

优点：跨平台，构建项目更加的友好
缺点：显示声明较为繁琐，代码重复较高

## Maven
Maven项目的核心是pom.xml。POM(Project Object Model,项目对象模型)定义了项目的基本信息，用于描述项目如何构建，声明项目依赖等等。
example POM如下：

		<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
		  <modelVersion>4.0.0</modelVersion>
		  <groupId>com.mvn.test</groupId>
		  <artifactId>hello-world</artifactId>
		  <version>0.0.1-SNAPSHOT</version>
		  <name>Maven Hello World Test</name>
		  <description>Hello World 测试</description>
		</project>

优点：快速构建，依赖管理方便（拥有Java开源软件包的中央仓库，可直接使用），便于敏捷开发。
缺点：Maven 过于复杂，中央仓库不完美（可能存在某个jar出现在不同的路径下），当无法从中央仓库获取所需的类库时，需要手工复制到本地仓库。

## Gradle
Gradle是一个基于Apache Ant和Apache Maven概念的项目自动化构建开源工具。
它使用一种基于Groovy的特定领域语言（DSL）来声明项目设置，抛弃了基于XML的各种繁琐配置。当前其支持的语言限于Java|Groovy和Scala,计划未来会支持更多的语言。（Gradle是一个开源的专注于灵活性和性能的自动化构建工具。Gradle的构件脚本是采用Groovy或Kotlin DSL语言编写的。）
在gradle.build文件中加入如下配置：
	
	task hello {
		
		doLast {
			println 'Hello World'
		}
	}


# 依赖管理
## Maven仓库
Maven可以在某个位置存储所有Maven项目共享的构件，这个统一位置就是仓库。
坐标和依赖是任何一个构件在Maven世界中的逻辑表示方式，而构建的物理方式是文件，Maven通过仓库来统一管理这些文件。
### 仓库分类
#### 本地仓库
在maven中的配置文件settings.xml中的settings标签之内配置如下，即为本地仓库。

	<localRepository>D:/LocalRepository/repo</localRepository>

一个构件只有在本地仓库中之后，才能由其他Maven项目使用。

对于Maven来说，每个用户只有一个仓库，但可以配置访问多个远程仓库。
#### 远程仓库
- 中央仓库

默认的远程仓库

- 私服

它是架设在局域网内的仓库服务，私服代理广域网上的远程仓库，供局域网内的Maven用户使用。

当Maven需要下载构建时，它先从私服请求，如果私服不存在，则从外部的远程仓库下载，缓存在私服后，再为Maven下载请求提供服务。

*优点*：节省自己的外网带宽，加速Maven构建，还能部署第三方构件，提高稳定性，增强控制，降低中央仓库的负荷。

- 其他公用库

常见的有Java.net Maven库和JBoss Maven库。

example:配置阿里云库，如下两种方式。

方式一：在setting.xml中的settings标签之内配置如下

	<mirror>
    	<id>alimaven</id>
		<name>aliyun maven</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
		<mirrorOf>central</mirrorOf>   <!-- 表示中央仓库的镜像 -->     
	</mirror>

方式二：在 pom.xml 中直接添加如下

	<repositories><!-- 代码库 -->
        <repository>
            <id>maven-ali</id>
            <url>http://maven.aliyun.com/nexus/content/groups/public//</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
                <updatePolicy>always</updatePolicy>
                <checksumPolicy>fail</checksumPolicy>
            </snapshots>
		</repository>
	</repositories>


### setting.xml文件配置介绍

	<?xml version="1.0" encoding="UTF-8"?>

	<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" 
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
         	 xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://	maven.apache.org/xsd/settings-1.0.0.xsd">
		 	
			 <localRepository> 构建系统本地仓库的路径 <localRepository/>
 			
			 <interactiveMode>Maven是否需要和用户交互以获得输入。如果Maven需要和用户交互以获得输入，则设置成true，反之则应为false。默认为true<interactiveMode/>
 			
		     <usePluginRegistry>Maven是否需要使用plugin-registry.xml文件来管理插件版本（值为true或false，默认为false）<usePluginRegistry/>
  		
          	 <offline/>Maven是否需要在离线模式下运行（值为true或者false，默认为false）<offline>
  			
			 <pluginGroups>当插件的组织Id（groupId）没有显式提供时，供搜寻插件组织Id（groupId）的列表<pluginGroups/>

 			 <servers>配置服务端的一些设置<servers>
  			 
             <mirrors/>为仓库列表配置的下载镜像列表<mirrors/>
  			
			 <proxies>用来配置不同的代理<proxies/>
 			
             <profiles>根据环境参数来调整构建配置的列表<profiles/>

 			 <activation>自动触发profile的条件逻辑<activation/>
				
			 <properties> 对应profile的扩展属性列表<properties/>
			 
			 <repositories>远程仓库列表，它是Maven用来填充构建系统本地仓库所使用的一组远程项目<repositories/>

			 <activeProfiles>手动激活profiles的列表，按照profile被应用的顺序定义activeProfile<activeProfiles/>

	</settings>

## 构件的定义与依赖
在Maven世界中，任何一个依赖、插件或者项目构建的输出，都可以成为插件。

### 坐标
Maven坐标为各种构件引入了秩序，任何一个构件都必须明确定义自己的坐标。

- **groupId**

定义当前Maven项目隶属的实际项目

- **artifactId**
	
定义实际项目中的一个Maven项目（模块）

- **version**
	
定义Maven项目当前所处的版本

- **packaging**
	
定义Maven项目的打包方式，默认为jar，为jar时可不必声明

- **classifier**

定义构建输出的一些附属构件

注意：groupId、artifactId、version是必须定义的，packaging是可选的（默认为jar）,classifier是不能直接定义的（因为附属构件不是项目直接默认生成的，而是由附加的插件帮助生成）。


### 依赖
根元素project下的dependencies可以包含一个或者多个dependency元素，以声明一个或多个项目依赖

- **groupId**、**artifactId**、 **version**

依赖的基本坐标，参见坐标。

- **type**

依赖的类型，对应坐标中的packaging，默认为jar，为jar时可不必声明

- **scope**

依赖范围就是用来控制依赖与三种classpath(编译classpath,测试classpath,运行classpath)的关系。

classpath:是指定在程序中所使用的类（.class）文件所在的位置。
	
依赖范围，如下：
		
		1.compile：编译依赖范围，如果没有指定，就会默认使用该依赖范围。
			有效范围：对于编译、测试、运行三种classpath都有效。
		2.test：测试依赖范围。
			有效范围：只对测试classpath都有效。
		3.provided：已提供依赖范围。
			有效范围：对于编译和测试classpath有效、但在运行时无效。
		4.runtime：运行时依赖范围。
			有效范围：对于测试、运行classpath有效，但在编译时无效。
		5.system：系统依赖范围。
			有效范围：对于编译和测试classpath有效、但在运行时无效。
			该依赖不是通过Maven仓库解析，往往与本机系统绑定，谨慎使用。
		6.import：导入依赖范围。
			不会对这三种classpath产生实际的影响。

传递性依赖: A ->  B -> C  ( A -> B:第一直接依赖。 B -> C:第二直接依赖。)

传递性依赖范围影响见下表,表中第一列为第一直接依赖范围，表中第一行为第二直接依赖范围。

&nbsp;|compile|test|provided|runtime
---|---|---|---|---
compile |compile|X|X|runtime
test|test|X|X|test
provided|provided|X|provided|provided
runtime|runtime|X|X|runtime

依赖调解：Maven依赖调解（Dependency Mediation）的原则为路径最近者优先，路径长度一样的，第一声明者优先。

- **optional**

	标记依赖是否可选

- **exclusions**

	用来排除传递性依赖

# Maven的生命周期与插件
## Maven生命周期
Maven拥有三套相互独立的生命周期，它们分别是clean、default和site。

clean生命周期的目的是清理项目，default的生命周期的目的是构建项目，而site生命周期的目的是建立项目站点。
### clean
clean生命周期的目的是清理项目，它主要包括三个阶段，如下：

阶段序号|生命周期阶段|解释说明|插件目标
---|---
1|pre-clean|执行一些清理前需要完成的工作|&nbsp;
2|clean|清理上一次构建生成的文件|maven-clean-plugin:clean
3|post-clean|执行一些清理后需要完成的工作|&nbsp;

### default
default生命周期定义了真正构建时所需要执行的所有步骤，它是所有生命周期中最核心的部分。

阶段序号|生命周期阶段|解释|插件目标|执行任务
---|---
1|validate|&nbsp;|&nbsp;|&nbsp;
2|initialize|&nbsp;|&nbsp;|&nbsp;
3|generate-sources|&nbsp;|&nbsp;|&nbsp;
4|process-sources|处理项目主资源文件，一般来说，对src/mian/resources目录的内容进行变量替换等工作后，复制到项目输出的主classpath目录中|maven-resources-plugin:resources|复制主资源文件至主输出目录
5|generate-resources|&nbsp;|&nbsp;|&nbsp;
6|process-resources|&nbsp;|&nbsp;|&nbsp;
7|compile|编译项目的主代码，编译src/main/java目录下的java文件至项目输出的主classpath目录中|maven-compiler-plugin:compile|编译主代码至主输出目录
8|process-classes|&nbsp;|&nbsp;|&nbsp;
9|generate-test-sources|&nbsp;|&nbsp;|&nbsp;
10|process-test-sources|处理项目测试资源文件，对src/test/resources目录下的内容进行变量替换后，复制到项目输出的测试classpath目录中|maven-resources-plugin:testResources|复制测试文件至测试输出目录
11|generate-test-resources|&nbsp;|&nbsp;|&nbsp;
12|process-test-resources|&nbsp;|&nbsp;|&nbsp;
13|test-compile|编译项目的测试代码，编译src/test/java目录下的Java文件至项目输出的测试classpath目录中|maven-compiler-plugin:testCompile|编译测试代码至测试输出目录
14|process-test-classes|&nbsp;|&nbsp;|&nbsp;
15|test|使用单元测试框架运行测试，测试代码不会被打包或部署|maven-surefire-plugin:test|执行测试用例
16|prepare-package|&nbsp;|&nbsp;|&nbsp;
17|package|接受编译好的代码，打包可发布的格式，如Jar|maven-jar-plugin:jar|创建项目jar包
18|pre-integration-test|&nbsp;|&nbsp;|&nbsp;
19|integration-test|&nbsp;|&nbsp;|&nbsp;
20|post-integration-test|&nbsp;|&nbsp;|&nbsp;
21|verify|&nbsp;|&nbsp;|&nbsp;
22|install|将包安装到Maven本地仓库，供本地其他Maven项目使用|maven-install-plugin:install|将项目输出构件安装到本地仓库
23|deploy|将最终的包复制到远程仓库，供其他开发人员和Maven项目使用|maven-deploy-plugin:deploy|将项目输出构件部署到远程仓库

### site
site生命周期的目的是建立和发布项目站点，如下表：


阶段序号|生命周期阶段|解释说明|插件目标
---|---
1|pre-site|执行一些在生成项目站点之前需要完成的工作|&nbsp;
2|site|生成项目站点文档|maven-site-plugin:site
3|post-site|执行一些在生成项目站点之后需要完成的工作|&nbsp;
4|site-deploy|将生成的项目站点发布到服务器上|maven-site-plugin:deploy


## 插件
Maven的核心仅仅定义了抽象的生命周期。具体的任务是交由插件完成的，Maven会在需要的时候下载并使用插件。

### 插件目标
聚集在一个插件中的每个功能就是一个插件目标

例如：maven-dependency-plugin有十多个目标，其中目标包含 dependency:analyze、dependency:tree、denpendency:list等等，冒号前为插件前缀，冒号后为插件的目标

### 插件绑定
Maven的生命周期与插件相互绑定，用以完成实际的构件任务。分为内置绑定和自定义绑定。

**内置绑定**
Maven在核心为一些主要的生命周期阶段绑定了很多插件的目标，当用户通过命令调用生命周期阶段的时候，对应的插件目标就会执行相应的任务，详情见上述Maven生命周期。

**自定义绑定**
将某个插件目标绑定到生命周期的某个阶段上即为自定义绑定。
当多个插件目标绑定到同一个阶段的时候，这些插件的声明的先后顺序决定了目标的执行任务。

# Maven常用命令

Maven常用命令 | 命令解释
---|---
mvn –version | 显示版本信息
mvn clean | 清理项目生产的临时文件,一般是模块下的target目录
mvn compile | 编译源代码，一般编译模块下的src/main/java目录
mvn package | 项目打包工具,会在模块下的target目录生成jar或war等文件
mvn test | 测试命令,或执行src/test/java/下junit的测试用例.
mvn install | 将打包的jar/war文件复制到你的本地仓库中,供其他模块使用
mvn deploy | 将打包的文件发布到远程仓库,提供其他人员进行下载依赖
mvn site | 生成项目相关信息的网站
mvn eclipse:eclipse | 将项目转化为Eclipse项目
mvn dependency:tree | 打印出项目的整个依赖树  
mvn archetype:generate | 创建Maven的普通java项目
mvn tomcat:run | 在tomcat容器中运行web应用
mvn jetty:run | 调用 Jetty 插件的 Run 目标在 Jetty Servlet 容器中启动 web 应用
# POM文件
## POM特性

- 聚合

一次可以构建多个项目，即多模块
- 继承

Maven有一个超级POM，所有的POM均继承此文件。位置在$MAVEN_HOME\lib\maven-model-builder-\*\.jar\org\apache\maven\model\\*\.pom

父模块的package必须设置为pom,子POM继承父POM需要在子POM中加入以下标签：

	<parent>
		<groupId>父项目的隶属项目</groupId>
		<artifactId>父项目名称</artifactId>
		<version>版本</version> 
		<relativePath>..</relativePath> <!--父项目的位置，..表示上一层目录-->
	</parent>
子POM可以继承父POM的元素如下，其他元素则为私有元素，如name等等：

继承元素|元素描述
---|---
groupId|项目ID，项目坐标的核心元素
version|项目版本，项目坐标的核心元素
decription|项目的描述信息
organization|项目的组织信息
inception|项目的组织
inceptionYear|项目的创始年份
url|项目的URL地址
developers|项目的开发者信息
contributors|项目的贡献者信息
distributionManagement|项目的部署配置
issueManagement|项目的缺陷跟踪系统信息
ciManagement|项目的持续集成系统信息
scm|项目的版本控制系统信息
mailingLists|项目的邮件列表信息
properties|自定义的Maven属性
dependencyManagement|项目的依赖管理配置
repositories|项目的仓库配置
build|包括项目的源码目录配置，输出目录配置、插件配置、插件管理配置
reporting|包括项目的报告输出目录配置、 报告插件配置

- 约定优于配置

源码目录src/main/；编译输出目录为target/classes/;打包方式为jar;包输出目录为target/,
尊循约定虽然损失了一定的灵活性，用户不能随意安排目录结构，但是却能减少配置。

- 依赖管理

## 反应堆
在一个多模块的Maven项目中，反应堆（Reactor）是指所有模块组成的。

反应堆的构建顺序：Maven按序读取POM，如果该POM没有依赖模块，那么构建该模块，否则就先构建起依赖模块，如果依赖还依赖其他模块，则进一步先构建依赖的依赖。
