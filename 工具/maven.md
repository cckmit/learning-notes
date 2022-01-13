## maven仓库的分类

Maven的仓库分为两类： 本地仓库和远程仓库，当maven根据坐标寻找构件的时候，它首先会查看本地仓库，如果本地仓库存在此构件，则直接使用，如果不存在，或者需要查看是否有更新的构件版本，Maven就会去远程仓库查找，发现需要的构件之后，下载到本地仓库再使用。如果本地仓库和远程仓库都没有需要的构建，就会报错。

中央仓库是Maven核心自带的远程仓库，它包含了绝大部分开源的构建，在默认配置下，当本地仓库没有maven需要的的构件时，它就会尝试从中央仓库中下载。

私服是另一种特殊的远程仓库，为了节省带宽和事件，应该在局域网内架设一个私有的仓库服务器，用其代理所有外部的远程仓库，内部的项目还能部署到私服上供其他项目使用。

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933099240-9e73f97b-0d45-42c3-b5bc-13692f97f44f.webp)

### 本地仓库

默认情况下，不管是在Windows还是linux下，每个用户在自己的用户目录下都有一个路径名为.m2/repository/的仓库目录。

有时候，因为某些原因（例如C盘空间不够），用户会想要自定义本地仓库目录地址，这时，可以编辑文件~/.m2/settings.xml，设置localRepository想要的仓库地址，例如：

```plain
<localRepository>d:maven/maven.m2/repository</localRepository>
```

默认情况下，~/.m2/settings.xml文件是不存在的，用户需要从maven安装目录下/conf/settings.xml文件再进行编辑。

一个构件只有在本地仓库之后，才能由其他maven项目使用。

### 远程仓库

安装好maven之后，如果不执行任何maven命令，本地仓库目录是不存在的，当用户输入第一条maven命令之后，maven才会创建本地仓库，然后根据配置和需要，从远程仓库下载构件到本地仓库

#### 远程仓库的认证

大部分远程仓库都不需要认证，但是还是有些仓库需要认证，认证配置如下：

```plain
<server>
      <id>deploymentRepo</id>
      <username>repouser</username>
      <password>repopwd</password>
    </server>
```

- username用户名
- password密码

- id必须与POM中需要认证的repository元素的id一致。

### 中央仓库

本地原始的本地仓库是空的，maven必须知道至少一个可用的远程仓库，才能在执行maven命令的时候下载到需要的构件，中央仓库就是这样一个默认的远程仓库，maven的安装文件自带了中央仓库的配置。/MAVEN_HOME/lib/maven2.2x-uber.jar

### 私服

私服是一种特殊的远程仓库，它是架设到局域网内的仓库服务，私服代理广域网上的远程仓库，供局域网内的maven用户使用。当maven需要下载构件的时候，它从私服请求，如果私服不存在该条件，则从外部的远程仓库下载，缓存到私服上之后。

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933099329-3296d13c-746e-4dc9-8f6a-5aab764e9596.webp)

### 镜像

如果仓库X可以提供仓库Y存储的所有内容，那么就可以认为X是Y的一个镜像，换句话说，任何一个可以从仓库Y获得的构件，都能够从它的镜像中获取。

比如说，[maven.net.cn/content/gro…](http://maven.net.cn/content/groups/public/) 是中央仓库 [repo1.maven.org/maven2/](http://repo1.maven.org/maven2/) 在中国的镜像，由于地理位置的因素，该镜像往往能够提供比中央仓库更快的服务。在国内我们更多的是使用阿里云提供的镜像服务：

```plain
<mirror> 
      <id>alimaven</id>
      <name>aliyun maven</name> 
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url> 
      <mirrorOf>central</mirrorOf>
</mirror> 
```

- mirrorOf为central，表示该配置为中央仓库的镜像，任何对于中央仓库的请求都会转到该镜像，而如果为*,那么匹配所有的远程仓库
- id唯一标识符

- name名称
- url地址

### maven的快照版本

在maven的世界中，任何一个项目都有自己的版本，版本的值可能是1.0.0，1.3-alpha-4,2.0,2.1-SNAPSHOT等

- 1.0.0,1.3-alpha-4和2.0是稳定的发布版本
- 2.1-SNAPSHOT是不稳定的快照版本

那为什么还区分稳定版和快照版呢？比如你接到一个需求，针对公司的二方库进行开发，新增某某接口等，但是你的需求不是很稳定，需要老是修改二方库里的内容，而公司某项目A又要依赖该二方库，如果没有快照版本，那么二方库每次修改好了，都会指定一个新的版本给项目A,项目A每次又要修改二方库的版本，这样就会显得非常麻烦。

而Maven的快照版本机制就是为了解决上述问题的，项目A只需要引入快照版本号，比如2.1-SNAPSHOT版本，因为是快照版本，针对二方库的每次修改并且发布后，在发布过程中，maven会自动为构件打上时间戳，有了该时间戳，Maven就可以随时找到仓库中该构件2.1-SNAPSHOT版本最新的文件，也就是说项目A可以拉取到最新的版本内容。

默认情况下，Maven每天检查一次更新，当然用户也可以使用命令行-U参数强制让maven检查更新，如 `mvn clean install -U`

## maven的生命周期

maven的生命周期就是为了对所有的构建过程进行抽象和统一，这个生命周期包含了项目的清理、初始化、编译、测试、打包、集成测试、验证、部署和站点生成等几乎所有构建步骤，但是**maven的生命周期是抽象的**，这意味着生命周期本身不做任何实际的工作，在maven的设计中，实际的任务都交给插件来完成。

maven有三个内置的构建生命周期：default、clean和site，但是平时工作中我们更多的关注的是maven更加细分的生命周期阶段：

- validate:  验证项目是否正确，所有必要的信息是否可用
- complie: 编译项目的源代码

- test: 用合适的单元测试框架测试编译好的源代码
- package: 将编译好的代码使用它的指定的可分发的格式进行打包  （jar/war）

- verify: 对集成测试的结果执行检查，以确保满足质量标准
- install: 将软件包安装到本地仓库，可用作本地其他项目的依赖项

- deploy: 将软件包发布到远程仓库中

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933099573-dd26eb7a-490b-4f78-8d8e-3cefdbc3bba0.webp)

阶段的执行是按顺序的，一个阶段执行完成之后才会执行下一个阶段，比如执行mvn install命令，实际上它会执行install阶段之前的所有阶段，然后才会执行install阶段本身。

前面就提到过在maven的设计中，实际的任务（比如编译源代码）是交给插件来完成的，下面展示几个常用的插件：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933099259-15f42d05-f3f2-4fc2-8c09-117f215916e6.webp)

## maven坐标和依赖

maven的一大功能就是管理项目依赖，为了能自动化解析任何一个Java构件，maven就必须将它们唯一标识，这就依赖管理的底层基础——坐标，也就是**GAV，即groupId artifactId version**，由这三个属性可以唯一确定一个jar包

- groupId

- - 表示一个团体，可以是公司、组织等

- artifactId

- - 表示团体下的某个项目

- version

- - 表示某个项目的版本号

以下面这个依赖项为例

```plain
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <version>2.1.4.RELEASE</version>
    <scope>test</scope>
</dependency>
```

除了GAV，还是一个scope配置，其实就是依赖的范围，依赖范围就是用来控制依赖与三种classpath(编译 classpath、测试classpath、运行classpath)的关系，Maven主要有以下几种依赖范围：

- complie

- - 编译依赖范围，默认值，对三种classpath都有有效

- test

- - 测试依赖范围，只对测试classpath有效，比如JUnit,它只在编译测试代码及运行测试的时候需要

- provided

- - 对编译和测试classpath有效，但在运行时无效

- runtime

- - 运行时依赖范围，对于测试和运行classpath有效，但在编译主代码时无效

- system

- - 与provided依赖范围一致

### 依赖冲突

谈到依赖，就不得不提使用maven可能碰到的依赖冲突问题，当一个项目中不同的Jar包依赖了相同的jar包时，此时就会发生依赖冲突的问题，为了避免冲突的发生，maven使用以下几种策略来解决冲突：

- 短路优先

- - 短路优先是指，从项目一直到最终依赖的jar包的距离，哪个距离短就依赖哪个，距离长的被忽略
  - 比如情况1：a.jar——>b.jar ；情况2： d.jar——>c.jar——>b.jar，两种情况都依赖于b.jar，但是情况1距离最短，选择情况1的方式

- 声明优先

- - 声明优先的意思是，通过jar包声明的顺序来决定使用哪个，最先声明的jar包总是被选中，后声明的jar包则会被忽略

那么上面只是两种策略，具体如何做呢？有两种做法，手动分析冲突并解决和使用maven helper插件

- **手动法**（以commons-logging包为例）

- - 首先分析冲突jar包的依赖路径，使用命令： `mvn dependency:tree -Dverbose -Dincludes=commons-logging:commons-loggging`，该命令将打印出所有依赖了groupId和artifactId都为commons-logging的jar包的依赖路径。
  - 在两个冲突的版本中，选择一个所需的版本

- - 在pom.xml文件中将冲突的依赖排除，可使用某冲突jar包

- **maven helper 插件**

- - IDEA中安装该插件，选择confilcts，会列出有冲突的jar，右侧在要排除的版本上右键点击Exclude

## maven的聚合与继承

### 1. 聚合

如下所示为聚合：

```plain
<packaging>pom</packaging> 
<modules>
    <module>module-1</module>
    <module>module-2</module>
    <module>module-3</module>
 </modules>
```

- 聚合时packaging必须是pom

### 2. 继承

父POM:

```plain
<groupId>com.pjmike</groupId>
<artifactId>maven-parent</artifactId> 
<version>0.0.1-SNAPSHOT</version>
<dependencyManagement>
   <dependencies>
      <dependency>
            <groupId>mysql</groupId> 
            <artifactId>mysql-connector-java</artifactId> 
            <version>5.1.30</version>
      </dependency>
   </dependencies>
</dependencyManagement>
```

子POM，引入父POM

```plain
<!-- 指定parent，说明是从哪个pom继承 -->
<parent>
    <groupId>com.pjmike</groupId> 
    <artifactId>maven-parent</artifactId> 
    <version>0.0.1-SNAPSHOT</version>
    <!-- 指定相对路径 --> 
    <relativePath>../maven-parent</relativePath>
</parent>
<!-- 只需要指明groupId + artifactId，就可以到父pom找到了，无需指明版本 -->
<dependencies>
    <dependency>
        <groupId>mysql</groupId> 
        <artifactId>mysql-connector-java</artifactId> 
    </dependency>
</dependencies>
```

**使用dependencyManagement可对依赖进行管理，子类只要不引用这个里面写的依赖，则不会添加，这样防止了重复加包，如果不使用dependencyManagement，那么只要写了dependency，子pom中会全部添加到依赖中。**

## maven常用指令

下面列举一些比较常见的maven命令

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933099507-59a91728-6a06-4b39-b0db-85d2e7fd7119.webp)

## 小结

下面总结一个maven相关知识的思维导图，如下图

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933099248-85ea2419-be91-476c-bbd0-790d6286279a.webp)

上面总结的maven知识点并不是特别全，更多关于maven的讲解可以参阅《maven实战》这本书。