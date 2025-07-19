# 什么是Maven

## 1.Maven的两大核心功能（重点）

#### 1.项目构建

**Maven是一个一键式的自动化的构建工具**

（1）能自动帮我们下载jar包

（2）能够进行多个项目同时的编译运行

（3）Maven提供了自动化的测试插件，来进行项目的测试功能的运行

【**什么是构建**】

构建就是以我们编写的 Java 代码、框架配置文件、国际化等其他资源文件、JSP 页面和图片等静态资源作为“原材料”，去“生产”出一个可以运行的项目的过程。

**构建过程的几个主要环节**

①清理：删除以前的编译结果，为重新编译做好准备。
②编译：将 Java 源程序编译为字节码文件。
③测试：针对项目中的关键点进行测试，确保项目在迭代开发过程中关键点的正确性。
④报告：在每一次测试后以标准的格式记录和展示测试结果。
⑤打包：将一个包含诸多文件的工程封装为一个压缩文件用于安装或部署。Java 工程对应 jar 包，Web 工程对应 war 包。
⑥安装：在 Maven 环境下特指将打包的结果——jar 包或 war 包安装到本地仓库中。
⑦部署：将打包的结果部署到远程仓库或将 war 包部署到服务器上运行。

#### **2.依赖管理**

对jar包的统一管理，Maven提供中央仓库，私服，本地仓库解决jar包的依赖喝相关依赖的下载。

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42995594/1710167507407-a3db57eb-0be4-45df-b304-a170351d10b5.png#averageHue=%23f0e1ba&clientId=u2edacb95-b01a-4&from=paste&height=391&id=uef3966fd&originHeight=587&originWidth=1007&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=355052&status=done&style=none&taskId=uc21d13e0-70d1-4a82-a676-89305c8dc12&title=&width=671.3333333333334)

## 2.Maven的核心概念

#### 1.什么是POM

POM(Project Object Model)项目对象模型，它是Maven的核心组件。它是Maven中的基本工作单元。它是一个xml文件，以pom.xml驻留在项目的根目录中。POM不仅包含有关项目的信息及Maven用于构建项目的各种配置的详细信息，还包含目标和插件。

主要描述了项目：包括配置文件，开发者需要遵循的规则，组织和项目的url，项目的依赖性，以及其他所有的项目相关因素。

#### 2.什么是约定的目录结构

所有Maven项目必须要遵循的目录规范，将源码和测试代码拆分开来。便于项目的管理和扩展。

约定的目录结构对于 Maven 实现自动化构建而言是必不可少的一环，就拿自动编译来说，Maven 必须 能找到 Java 源文件，下一步才能编译，而编译之后也必须有一个准确的位置保持编译得到的字节码文件。

​		springMvc01

​				src

​						main							====>所有的Java源码文件和相关的配置文件	

​								java 						   ====>所有的Java源码文件		

​								resources				  ====>所有的配置文件				

​						test							====>所有的测试相关的代码和配置文件	

​								java							====>所有的测试代码		

​								resources				 ====>所有的测试相关的配置文件		

​				pom.xml



#### 3.什么是坐标GAV

坐标就是资源资源的唯一定位。创建项目时使用坐标拟定一个名称，访问资源时通过坐标找到资源。

也称为gav定位。使用三个标签来唯一定位jar资源。项目的唯一的名称，创建项目时定义gav名称，引用项目时使用gav名称。相当于项目的身份证号。

[1]groupid：定义了项目属于哪个组，举个例子，如果你的公司是mycom，有一个项目为myapp，那么groupId就应该是com.mycom.myapp.
[2]artifactId：当前项目的模块名称
[3]version：当前模块的版本

#### 4.什么是仓库

仓库就是用来存放jar包的。

本机存储jar包的位置称为本地仓库。远程存储jar包称为远程仓库。

##### 本地仓库

本地仓库，存在于当前电脑上，默认存放在~\.m2\repository中,为本机上所有的Maven工程服务。你也可以通过Maven的配置文件Maven_home/conf/settings.xml中修改本地仓库所在的目录。~ 是用户的主目录，windows系统中是 c:/user/登录系统的用户名

我课程里是存放在本机上的某个磁盘的位置(一定是没有中文的路径).    D:\repository

秘密:  gav就是仓库中一级一级的目录名称

##### 远程仓库

远程仓库，分为为全世界范围内的开发人员提供服务的中央仓库、为全世界范围内某些特定的用户提供服务的中央仓库镜像、为本公司提供服务自己架设的私服。

中央仓库是maven默认的远程仓库，其地址是[:h](http://repo.maven.apache.org/maven2/)t[tp://repo.maven.apache.org/maven2/](http://repo.maven.apache.org/maven2/)，中央仓库包含了绝大多数流行的开源Java构件，以及源码、作者信息、许可证信息等。一般来说，简单的Java项目依赖的构件都可以在这里下载得到。

私服是一种特殊的远程仓库，它是架设在局域网内的仓库服务，私服代理广域网上的远程仓库，供局域网内的Maven用户使用。当Maven需要下载构件的时候，它从私服请求，如果私服上不存在该构件，则从外部的远程仓库下载，缓存在私服上之后，再为Maven的下载请求提供服务。我们还可以把一些无法从外部仓库下载到的构件上传到私服上。
**程序员常用的一个仓库:**
[**http://mvnrepository.com/**](http://mvnrepository.com/)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42995594/1710230122267-f7fc65cb-47fd-49e7-a5e4-dd78391a329a.png#averageHue=%23f8f7f7&clientId=u2edacb95-b01a-4&from=paste&height=649&id=u73398b2b&originHeight=974&originWidth=1852&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=323216&status=done&style=none&taskId=uf1df6884-54ab-4866-8d5a-bf25a13c375&title=&width=1234.6666666666667)

#### 5.什么是依赖

添加jar包

所有的资源是以gav进行定义的，也是通过gav来添加引用的。

一个Maven 项目正常运行需要其它项目的支持，Maven 会根据坐标自动到本地仓库中进行查找。对于程序员自己的 Maven 项目需要进行安装，才能保存到仓库中。不用maven 的时候所有的 jar 都不是你的，需要去各个地方下载拷贝，用了 maven 所有的 jar 包都是你的，想用谁，叫谁的名字就行。maven 帮你下载。

除了管理当前要使用的jar包，并且同时管理与其有依赖关系的jar包，自动去下载，并添加到当前的仓库，并给项目添加引用。是通过<dependencies>大标签中的子标签<dependency>，使用gav添加依赖。

```xml
<dependencies>
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.11</version>
  </dependency>
</dependencies>
```

Maven解析依赖信息时会到本地仓库中查找被依赖的jar包。对于我们自己开发的Maven工程，使用mvn install命令安装后就可以进入仓库。

A项目需要引用B项目中的类，那么A对B就产生了依赖。

Commons-fileupload依赖Commons-io包。

#### 6.什么是生命周期

Maven的生命周期就是对所有的构建过程进行统一。包含了项目的清理、初始化、编译、测试、打包、集成测试、验证、部署等几乎所有的构建步骤。



对项目的构建是建立在生命周期模型上的，它明确定义项目生命周期各个阶段，并且对于每一个阶段提供相对应的命令，对开发者而言仅仅需要掌握一小堆的命令就可以完成项目各个阶段的构建工作。

构建项目时按照生命周期顺序构建，每一个阶段都有特定的插件来完成。不论现在要执行生命周期中的哪个阶段，都是从这个生命周期的最初阶段开始的。

对于我们程序员而言，无论我们要进行哪个阶段的构建，直接执行相应的命令即可，无需担心它前边阶段是否构建，Maven 都会自动构建。这也就是 Maven 这种自动化构建工具给我们带来的好处。

使用idea后，生命周期要调用的命令被集成化一些按钮，只需要双击即可调用相应的插件来运行。

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42995594/1710230536719-09afed3c-fe6b-4923-ba37-52686b407cc0.png#averageHue=%233a3f42&clientId=u2edacb95-b01a-4&from=paste&height=212&id=u4c24fe5b&originHeight=318&originWidth=591&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=753361&status=done&style=none&taskId=ucf96402c-8a54-4db4-ab62-d8dd3314460&title=&width=394)

生命周期对应的Maven命令(了解）：

1. mvn clean 清理(会删除原来编译和测试的目录，即 target 目录，但是已经 install 到仓库里的包不会删除) 
2. mvn compile  编译主程序(会在当前目录下生成一个 target,里边存放编译主程序之后生成的字节码文件)
3. mvn test-compile  编译测试程序(会在当前目录下生成一个 target,里边存放编译测试程序之后生成的字节码文件)
4. mvn test  测试(会生成一个目录surefire-reports，保存测试结果) 
5. mvn package  打包主程序(会编译、编译测试、测试、并且按照 pom.xml 配置把主程序打包生成 jar 包或者 war 包)
6. mvn install 安装主程序(会把本工程打包，并且按照本工程的坐标保存到本地仓库中)
7. mvn deploy 部署主程序(部署到私服仓库中）。



#### 7.什么是插件

**Maven本质上是一个插件框架**，它的核心并不执行任何具体的构建任务，所有这些任务都交给插件来完成，例如编译源代码是由maven- compiler-plugin完成的。进一步说，每个任务对应了一个插件目标（goal），每个插件会有一个或者多个目标，例如maven- compiler-plugin的compile目标用来编译位于src/main/java/目录下的主源码，testCompile目标用来编译位于src/test/java/目录下的测试源码。

Maven支持极简化的插件添加.使用<plugins>大标签中添加<plugin>子标签引用插件.

```xml
<plugins>
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <configuration>
      <source>17</source>
      <target>17</target>
      <encoding>UTF-8</encoding>
    </configuration>
  </plugin>
</plugins>
```

1.生命周期与插件是相互绑定的。

1)执行生命周期命令时，需要通过插件来完成。
2)compile和test-compile都通过同一个插件完成的。
3)目标:就是插件所需要做的事情，例如：maven-compiler-plugin既可以执行compile，也可以执行testCompile, compile和testCompile是maven-compiler-plugin插件的两个功能；当maven-compiler-plugin插件执行compile的时候，那么compile就是插件的一个最终目标；

# Maven的应用

### 下载Maven

官网：[http://maven.apache.org/](http://maven.apache.org/)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42995594/1710207605997-5488cc1d-1e4b-426d-94c5-be14f0499e2b.png#averageHue=%23f4f2f1&clientId=u2edacb95-b01a-4&from=paste&height=511&id=u1dedc3b9&originHeight=766&originWidth=1874&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=270109&status=done&style=shadow&taskId=udad52ba6-2e8f-482a-be86-ef58dc3182d&title=&width=1249.3333333333333)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42995594/1710208724973-df375112-b2cc-4990-8ce7-4979212e72bb.png#averageHue=%23faf8f8&clientId=u2edacb95-b01a-4&from=paste&height=533&id=u5e5afe90&originHeight=799&originWidth=1873&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=252564&status=done&style=shadow&taskId=u902e6114-6e9c-4505-bf32-8bddeb05b7a&title=&width=1248.6666666666667)

### 配置Maven

### 配置Maven工具参数

打开apache-maven-3.9.6\conf\settings.xml文件，进行本地仓库，远程仓库和JDK参数设置。

1. 配置本地仓库，将注释中53行的代码提取出注释，放置在第55行，设置本地仓库的地址路径。如果已有本地仓库则直接指定地址，如果没有本地仓库则指定一个目录，在idea配置后会自动生成目录。

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42995594/1710222022190-7d23106a-01e4-4dc3-9c1f-558e4cb3fba8.png#averageHue=%23fdfbfa&clientId=u2edacb95-b01a-4&from=paste&height=501&id=u39b3489b&originHeight=751&originWidth=1859&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=237919&status=done&style=none&taskId=u9d8fc137-82eb-4e0c-86a8-6f338cef6cf&title=&width=1239.3333333333333)

2. 配置远程仓库

找到</mirrors>结束标签，将以下代码贴在其前面。

```xml
<!--配置阿里远程仓库-->
<mirror>
  <id>alimaven</id>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  <mirrorOf>central</mirrorOf>        
</mirror>
```

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42995594/1710222406864-0ca53cb9-5b19-40ca-b5aa-6224835e99a7.png#averageHue=%23fdf9f8&clientId=u2edacb95-b01a-4&from=paste&height=467&id=iqsQ0&originHeight=701&originWidth=1859&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=202059&status=done&style=shadow&taskId=u1677f5db-35bf-4529-a85f-961167cc250&title=&width=1239.3333333333333)

远程仓库配置后，经常出现以下bug，连网去点try...就行，如果还是出现try...，就需要到本地仓库中，搜索last*，将出现的所有文件都删除后，再来点try...就行。
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42995594/1710230234589-504260b6-d7ea-420a-ac3c-4a311f32fa1b.png#averageHue=%23302c2c&clientId=u2edacb95-b01a-4&from=paste&height=145&id=uc2963f98&originHeight=218&originWidth=830&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=725228&status=done&style=none&taskId=uabb3d539-ee09-44a1-abc0-2b6b2edc7eb&title=&width=553.3333333333334)

3. 配置JDK属性

在<profiles>标签中进行配置，一定要小心，找到</profiles>结束标签，在其前面配置以下代码。因为在<profiles></profiles>标签中全部是注释，粘到哪里都在注释中，只有找到结束标签</profiles>前才是注释外的，配置才会生效。

```xml
<profile>
  <id>jdk17</id>
  <activation>
    <activeByDefault>true</activeByDefault>
    <jdk>17</jdk>
  </activation>
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
    <maven.compiler.compilerVersion>17</maven.compiler.compilerVersion>
  </properties>
</profile>
```

### 基于Maven开发Java SE的项目

### 基于Maven开发Java Web的项目

### 导入Maven项目

### Maven的依赖管理

### Maven的继承和聚合

### Maven的私服