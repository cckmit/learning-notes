#  SpringBoot简介

## 回顾什么是Spring

Spring是一个开源框架，2003 年兴起的一个轻量级的Java 开发框架，作者：Rod Johnson 。

Spring是为了解决企业级应用开发的复杂性而创建的，简化开发。

## Spring是如何简化Java开发的

为了降低Java开发的复杂性，Spring采用了以下4种关键策略：

1、基于POJO的轻量级和最小侵入性编程，所有东西都是bean；

2、通过IOC，依赖注入（DI）和面向接口实现松耦合；

3、基于切面（AOP）和惯例进行声明式编程；

4、通过切面和模版减少样式代码，RedisTemplate，xxxTemplate；

## 什么是SpringBoot

Spring Boot期望通过结合自动配置和starters来解决了这个问题。 另外，Spring Boot还提供了一些功能，可以更快地构建可用于生产环境的应用程序。

Spring Boot Starter是一组被依赖第三方类库的集合。

如果你要开发一个web应用程序，就通过包管理工具(如maven)引入spring-boot-starter-web就可以了，而不用分别引入下面这么多依赖类库，spring-boot-starter-web一次性帮你引入下面的这些常用类库。

- Spring — spring 核心, beans, context上下文, AOP面向切面
- Web MVC — Spring MVC
- Jackson — JSON数据的序列化与反序列化
- Validation — Hibernate参数校验及校验API
- 嵌入式 Servlet Container — Tomcat
- 日志框架Logging — logback, slf4j

## Spring Boot的主要优点：

- 为所有Spring开发者更快的入门

- 开箱即用，提供各种默认配置来简化项目配置

- 内嵌式容器简化Web项目

- 没有冗余代码生成和XML配置的要求

官方文档： https://spring.io/projects/spring-boot

中文文档： https://www.springcloud.cc/spring-boot.html

# 微服务介绍

## 什么是微服务

 随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及流动计算架构势在必行，亟需一个治理系统确保架构有条不紊的演进。

微服务是一种架构风格，他要求我们在开发一个应用的时候，这个应用必须构建成一系列小服务的组合；可以通过http的方式进行互通。要说微服务架构，先得说说过去的单体应用架构。

**单一应用架构**

 所谓单体应用架构（all in one）是指，我们将一个应用的中的所有应用服务都封装在一个应用中。

优点：易于开发和测试、方便部署

缺点：维护繁琐、业务耦合

**垂直应用架构**

当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，提升效率的方法之一是将应用拆成互不相干的几个应用，以提升效率。此时，用于加速前端页面开发的Web框架(MVC)是关键。

**分布式服务架构**

当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的分布式服务框架(RPC)是关键。

**流动计算架构**

当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。此时，用于提高机器利用率的资源调度和治理中心(SOA)是关键。

**微服务架构**

all in one的架构方式，我们把所有的功能单元放在一个应用里面。然后我们把整个应用部署到服务器上。如果负载能力不行，我们将整个应用进行水平复制，进行扩展，然后在负载均衡。

所谓微服务架构，就是打破之前all in one的架构方式，把每个功能元素独立出来。把独立出来的功能元素的动态组合，需要的功能元素才去拿来组合。需要多一些时可以整合多个功能元素。所以微服务架构是对功能元素进行复制,而没有对整个应用进行复制。

优点：节省了调用资源、每个功能元素的服务都度是一个可替换的、可独立升级的软件代码。

## 如何构建微服务

一个大型系统的微服务架构，各功能模块通过http相互请求调用。比如一个电商系统，查缓存、连数据库、浏览页面、结账、支付等服务都是一个个独立的功能服务，都被微化了，它们作为一个个微服务共同构建了一个庞大的系统。如果修改其中的一个功能，只需要更新升级其中一个功能服务单元即可。

但是这种庞大的系统架构给部署和运维带来很大的难度。于是,spring为我们带来了构建大型分布式微服务的全套、全程产品:

构建一个个功能独立的微服务应用单元，可以使用springboot，可以帮我们快速构建一个应用;

大型分布式网络服务的调用，这部分由spring cloud来完成，实现分布式;

在分布式中间，进行流式数据计算、批处理，我们有spring cloud data flow,

spring为我们想清楚了整个从开始构建应用到大型分布式应用全流程方案。

# 第一个SpringBoot程序

Spring官方提供了非常方便的工具让我们快速构建应用

Spring Initializr： https://start.spring.io/

## 创建SpringBoot项目

**创建方式一：**使用Spring Initializr 的 Web页面创建项目

**创建方式二：**使用 IDEA 直接创建项目

项目结构目录整体上符合maven规范要求：

| 目录位置                                  | 功能                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| src/main/java                             | 项目java文件存放位置，初始化包含主程序入口 XxxApplication，可以通过直接运行该类来 启动 Spring Boot应用 |
| src/main/resources                        | 存放静态资源，图片、CSS、JavaScript、web页面模板文件等       |
| src/test                                  | 单元测试代码目录                                             |
| .gitignore                                | git版本管理排除文件                                          |
| target文件夹                              | 项目代码构建打包结果文件存放位置，不需要人为维护             |
| pom.xml                                   | maven项目配置文件                                            |
| application.properties（application.yml） | 用于存放程序的各种依赖模块的配置信息，比如服务端口，数据库连接配置等 |

- src/main/resources/static主要用来存放css、图片、js等开发用静态文件
- src/main/resources/public用来存放可以直接用于访问的html文件
- src/main/resources/templates用来存放web开发模板文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!-- 父依赖 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.0</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>hello</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>hello</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <!-- web场景启动器 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
  <!-- springboot单元测试 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <!-- 打包插件 -->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```



## Hello World

在主程序的同级目录下，新建一个controller包，一定要在同级目录下，否则识别不到

```java
@RestController
public class HelloSpringBoot {
    //接口 http://localhost:8080/hello
    @RequestMapping("/hello")
    public String hello(){
        return "Hello World";
    }
}
```

访问 http://localhost:8080/hello

## 将项目打成jar包

若遇到错误，可以配置打包时跳过项目运行测试用例

```xml
<!--    
	在工作中,很多情况下我们打包是不想执行测试用例的
    可能是测试用例不完事,或是测试用例会影响数据库数据
    跳过测试用例执行
-->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <!--跳过项目运行测试用例-->
        <skipTests>true</skipTests>
    </configuration>
</plugin>
```

如果打包成功，则会在target目录下生成一个 jar 包，便可在其他地方运行

## 彩蛋

1、修改项目的端口号

2、更改启动时显示的字符拼成的字母

只需一步：到项目下的 resources 目录下新建一个banner.txt 即可。

图案可以到：https://www.bootschool.net/ascii 这个网站生成，然后拷贝到文件中即可！

# 运行原理初探

## 父依赖

其中它主要是依赖一个父项目，主要是管理项目的资源过滤及插件！

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.5.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

点进去，发现还有一个父依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.2.5.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
```

这是真正管理SpringBoot应用里面所有依赖版本的地方，SpringBoot的版本控制中心；

## 启动器 spring-boot-starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

启动器：说白了就是SpringBoot的启动场景；

springboot-boot-starter-xxx：就是spring-boot的场景启动器

spring-boot-starter-web：帮我们导入了web模块正常运行所依赖的组件；

SpringBoot将所有的功能场景都抽取出来，做成starter （启动器），只需要在项目中引入这些starter即可，所有相关的依赖都会导入进来。

## 主启动类

默认的主启动类

```java
//@SpringBootApplication 来标注一个主程序类
//说明这是一个Spring Boot应用
@SpringBootApplication
public class SpringbootApplication {
   public static void main(String[] args) {
     //以为是启动了一个方法，没想到启动了一个服务
      SpringApplication.run(SpringbootApplication.class, args);
   }
}
```

**@SpringBootApplication**

作用：标注在某个类上说明这个类是SpringBoot的主配置类 ， SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；是个复合注解。

```java
@Target({ElementType.TYPE})			//可以给一个类型进行注解，比如类、接口、枚举
@Retention(RetentionPolicy.RUNTIME)  //可以保留到程序运行的时候，它会被加载进入到 JVM 中
@Documented						   //将注解中的元素包含到 Javadoc 中去。
@Inherited						   //继承，比如A类上有该注解，B类继承A类，B类就也拥有该注解
@SpringBootConfiguration
@EnableAutoConfiguration			//开启自动配置
/*
*创建一个配置类，在配置类上添加 @ComponentScan 注解。
*该注解默认会扫描该类所在的包下所有的配置类，相当于之前的 <context:component-scan>。
*/
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
```

**@ComponentScan**

这个注解在Spring中很重要 ,它对应XML配置中的元素。

作用：自动扫描并加载符合条件的组件或者bean ， 将这个bean定义加载到IOC容器中

**@SpringBootConfiguration**

作用：SpringBoot的配置类 ，标注在某个类上 ， 表示这是一个SpringBoot的配置类；

```java
// 点进去得到下面的 @Component
@Configuration
public @interface SpringBootConfiguration {}
@Component //说明这也是一个spring的组件
public @interface Configuration {}
```

-  @Configuration：说明这是一个配置类 ，配置类就是对应Spring的xml 配置文件；

- @Component：说明启动类本身也是Spring中的一个组件而已，负责启动应用！

**@EnableAutoConfiguration ：自动配置功能**，告诉SpringBoot开启自动配置功能，这样自动配置才能生效；

```java
 @Target({ElementType.TYPE})
 @Retention(RetentionPolicy.RUNTIME)
 @Documented
 @Inherited
 @AutoConfigurationPackage
 @Import({AutoConfigurationImportSelector.class})
 public @interface EnableAutoConfiguration
```

- @Import({AutoConfigurationImportSelector.class}) ：给容器导入组件 ；AutoConfigurationImportSelector ：自动配置导入选择器。

点进注解接续查看：

- **@AutoConfigurationPackage ：自动配置包**

```java
@Import({Registrar.class})    //导入选择器包注册
public @interface AutoConfigurationPackage {
    String[] basePackages() default {};
    Class<?>[] basePackageClasses() default {};
```

@import ：Spring底层注解@import ， 给容器中导入一个组件

Registrar.class 作用：将主启动类的所在包及包下面所有子包里面的所有组件扫描到Spring容器 ；

1、这个类中有一个这样的方法

```java
// 获得候选的配置
protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
    //这里的getSpringFactoriesLoaderFactoryClass（）方法
    //返回的就是我们最开始看的启动自动导入配置文件的注解类；EnableAutoConfiguration
    List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
    Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
    return configurations;
}
```

2、这个方法又调用了 SpringFactoriesLoader 类的静态方法！我们进入SpringFactoriesLoader类loadFactoryNames() 方法

```java
public static List<String> loadFactoryNames(Class<?> factoryClass, @Nullable ClassLoader classLoader) {
    String factoryClassName = factoryClass.getName();
    //这里它又调用了 loadSpringFactories 方法
    return (List)loadSpringFactories(classLoader).getOrDefault(factoryClassName, Collections.emptyList());
```

3、我们继续点击查看 loadSpringFactories 方法

```java
private static Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader) {
    //获得classLoader ， 我们返回可以看到这里得到的就是EnableAutoConfiguration标注的类本身
    MultiValueMap<String, String> result = (MultiValueMap)cache.get(classLoader);
    if (result != null) {
        return result;
    } else {
        try {
            //去获取一个资源 "META-INF/spring.factories"
            Enumeration<URL> urls = classLoader != null ? classLoader.getResources("META-INF/spring.factories") : ClassLoader.getSystemResources("META-INF/spring.factories");
            LinkedMultiValueMap result = new LinkedMultiValueMap();
            //将读取到的资源遍历，封装成为一个Properties
            while(urls.hasMoreElements()) {
                URL url = (URL)urls.nextElement();
                UrlResource resource = new UrlResource(url);
                Properties properties = PropertiesLoaderUtils.loadProperties(resource);
                Iterator var6 = properties.entrySet().iterator();
                while(var6.hasNext()) {
                    Entry<?, ?> entry = (Entry)var6.next();
                    String factoryClassName = ((String)entry.getKey()).trim();
                    String[] var9 = StringUtils.commaDelimitedListToStringArray((String)entry.getValue());
                    int var10 = var9.length;
                    for(int var11 = 0; var11 < var10; ++var11) {
                        String factoryName = var9[var11];
                        result.add(factoryClassName, factoryName.trim());
                    }
                }
            }
            cache.put(classLoader, result);
            return result;
        } catch (IOException var13) {
            throw new IllegalArgumentException("Unable to load factories from location [META-INF/spring.factories]", var13);
        }
    }
}
```

4、发现一个多次出现的文件：spring.factories，全局搜索它，看到了很多自动配置的文件。

所以，**自动配置真正实现是从classpath中搜寻所有的META-INF/spring.factories配置文件 ，并将其中对应的org.springframework.boot.autoconfigure. 包下的配置项，通过反射实例化为对应标注了 @Configuration的JavaConfig形式的IOC容器配置类 ， 然后将这些都汇总成为一个实例并加载到IOC容器中。**

SpringBoot所有自动装配都是在启动的时候扫描并加载：spring.factories所有的自动配置类都在这里面，但是不一定生效，要判断条件是否成立，只要导入了对应的start，就有对应的启动器了，有了启动器，我们自动装配就会生效，然后就配置成功！

**结论：**

- **SpringBoot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值**

- **将这些值作为自动配置类导入容器 ， 自动配置类就生效 ， 帮我们进行自动配置工作。**

- 整个J2EE的整体解决方案和自动配置都在springboot-autoconfigure的jar包中；

- 它会把所有需要导入的组件，以类名的方式返回，这些组件就会被添加到容器

- 它会给容器中导入非常多的自动配置类 （xxxAutoConfiguration）, 就是给容器中导入这个场景需要的所有组件 ， 并配置好这些组件 ；

------

**SpringApplication**

```java
@SpringBootApplication
public class SpringbootApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootApplication.class, args);
    }
}
```

**SpringApplication.run分析**

分析该方法主要分两部分，一部分是SpringApplication的实例化，二是run方法的执行；

**SpringApplication**

这个类主要做了以下四件事情：

1、推断应用的类型是普通的项目还是Web项目

2、查找并加载所有可用初始化器 ， 设置到initializers属性中

3、找出所有的应用程序监听器，设置到listeners属性中

4、推断并设置main方法的定义类，找到运行的主类



查看构造器：

```java
public SpringApplication(ResourceLoader resourceLoader, Class... primarySources) {
    // ......
    this.webApplicationType = WebApplicationType.deduceFromClasspath();
    this.setInitializers(this.getSpringFactoriesInstances();
    this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class));
    this.mainApplicationClass = this.deduceMainApplicationClass();
}
```



## run方法流程分析

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615214764080-a6a130cc-aa11-4ea0-9d6b-e2a1083b20b3.png)![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615214771249-cc3aacb1-9377-41e4-a8f7-a6e0c8ba43e9.png)

# yaml配置注入

## 配置文件

SpringBoot使用一个全局的配置文件 ， 配置文件名称是固定的

**application.properties**

语法结构 ：key=value



**application.yaml**

语法结构 ：key：空格 value

**全局配置文件的作用**：修改SpringBoot自动配置的默认值，SpringBoot在底层给我们做自动加载。

## yaml概述

在开发的yaml时，YAML 的意思其实是：“Yet Another Markup Language”（仍是一种标记语言）

这种语言以数据作为中心，而不是以标记语言为重点！

以前的配置文件，大多数都是使用xml来配置；比如一个简单的端口配置，我们来对比下yaml和xml



传统xml配置：

```xml
<server>
    <port>8081<port>
</server>
```



yaml配置：

```yaml
server: 
 port: 8080
```

## yaml基础语法

说明：语法要求严格！

1、空格不能省略

2、以缩进来控制层级关系，只要是左边对齐的一列数据都是同一个层级的。

3、属性和值的大小写都是十分敏感的。

字面量：普通的值 [ 数字，布尔值，字符串 ]

字面量直接写在后面就可以 ， 字符串默认不用加上双引号或者单引号；

```yaml
k: v
```

注意：

- “ ” 双引号，不会转义字符串里面的特殊字符 ， 特殊字符会作为本身想表示的意思；

- ‘’ 单引号，会转义特殊字符 ， 特殊字符最终会变成和普通字符一样输出

**对象、Map（键值对）**

```yaml
#对象、Map格式
k: 
    v1:
    v2:
```

在下一行来写对象的属性和值得关系，注意缩进；比如：

```yaml
student:
    name: qinjiang
    age: 3
```

**数组（ List、set ）**

用 - 值表示数组中的一个元素,比如：

```yaml
pets:
 - cat
 - dog
 - pig
```

**行内写法**

```yaml
student: {name: qinjiang,age: 3}
pets: [cat,dog,pig]
```

**修改SpringBoot的默认端口号**

配置文件中添加，端口号的参数，就可以切换端口；

```yaml
server:
  port: 8082
```

## yaml注入配置文件

yaml文件更强大的地方在于，他可以给我们的实体类直接注入匹配值！

1、在springboot项目中的resources目录下新建一个文件 application.yml

2、编写一个实体类 Dog；

```java
package com.kuang.springboot.pojo;

@Component  //注册bean到容器中
public class Dog {
    private String name;
    private Integer age;
    
    //有参无参构造、get、set方法、toString()方法  
}
```

3、思考，我们原来是如何给bean注入属性值的！@Value，给狗狗类测试一下：

```java
@Component //注册bean
public class Dog {
    @Value("阿黄")
    private String name;
    @Value("18")
    private Integer age;
}
```

4、在SpringBoot的测试类下注入狗狗输出一下；

```java
@SpringBootTest
class DemoApplicationTests {
    @Autowired //将狗狗自动注入进来
    Dog dog;
    @Test
    public void contextLoads() {
        System.out.println(dog); //打印看下狗狗对象
    }
}
```

结果成功输出，@Value注入成功，这是我们原来的办法。

5、我们在编写一个复杂一点的实体类：Person 类

```java
@Component //注册bean到容器中
public class Person {
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
   
    //有参无参构造、get、set方法、toString()方法  
}
```

6、我们来使用yaml配置的方式进行注入，大家写的时候注意区别和优势，我们编写一个yaml配置！

```yaml
person:
  name: qinjiang
  age: 3
  happy: false
  birth: 2000/01/01
  maps: {k1: v1,k2: v2}
  lists:
   - code
   - girl
   - music
  dog:
    name: 旺财
    age: 1
```

7、我们刚才已经把person这个对象的所有值都写好了，我们现在来注入到我们的类中！

```java
/*
@ConfigurationProperties作用：
将配置文件中配置的每一个属性的值，映射到这个组件中；
告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
参数 prefix = “person” : 将配置文件中的person下面的所有属性一一对应
*/
@Component //注册bean
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```

8、IDEA 提示，springboot配置注解处理器没有找到，让我们看文档，我们可以查看文档，找到一个依赖！

```xml
<!-- 导入配置文件处理器，配置文件进行绑定就会有提示，需要重启 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-configuration-processor</artifactId>
  <optional>true</optional>
</dependency>
```

9、确认以上配置都OK之后，我们去测试类中测试一下：

```java
@SpringBootTest
class DemoApplicationTests {
    @Autowired
    Person person; //将person自动注入进来
    @Test
    public void contextLoads() {
        System.out.println(person); //打印person信息
    }
}
```

结果：所有值全部注入成功！

## 加载指定的配置文件

**@PropertySource ：**加载指定的配置文件；

@configurationProperties：默认从全局配置文件中获取值；

1、我们去在resources目录下新建一个person.properties文件

```xml
name=kuangshen
```

2、然后在我们的代码中指定加载person.properties文件

```java
@PropertySource(value = "classpath:person.properties")
//javaconfig 绑定我们的配置文件的值，可以采取这些方式！
@Component //注册bean
public class Person {
    @Value("${name}")
    private String name;
    ......  
}
```

3、再次输出测试一下：指定配置文件绑定成功！

可以不用@ConfigurationProperties(prefix = “person”)注解 的方式，使用@Value的方式注入属性值

@Value注解等价于：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615215348834-c0a764e9-2ae4-41b3-a261-3110f18057d6.png)

以三个属性字段为例，我们既可以从配置文件取值，也可以通过字面量直接赋值，当属性值少的时候这种方式特别方便。

```java
@Value("${person.lastName}")
private String lastName;
@Value("${person.age}")
private Integer age;
@Value("true")
private Boolean boss;
```

## 配置文件占位符

配置文件还可以编写占位符生成随机数

```yaml
person:
    name: qinjiang${random.uuid} # 随机uuid
    age: ${random.int}  # 随机int
    happy: false
    birth: 2000/01/01
    maps: {k1: v1,k2: v2}
    lists:
      - code
      - girl
      - music
    dog:
      name: ${person.hello:other}_旺财
      age: 1
```

测试一下！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615215422356-3384ce1d-6ef5-4cd2-82bf-e2c8751a4ee9.png)

## 回顾properties配置

配置文件除了yml还有我们之前常用的properties ，

【注意】properties配置文件在写中文的时候，会有乱码 ， 我们需要去IDEA中设置编码格式为UTF-8；

settings–>FileEncodings 中配置；

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615215438360-33ced0ce-28a7-4805-8f00-f0cd0b35ce13.png)

测试步骤：

1、新建一个实体类User

```java
@Component //注册bean
public class User {
    private String name;
    private int age;
    private String sex;
}
```

2、编辑配置文件 user.properties

```java
user1.name=kuangshen
user1.age=18
user1.sex=男
```

3、我们在User类上使用@Value来进行注入！

```java
@Component //注册bean
@PropertySource(value = "classpath:user.properties")
public class User {
    //直接使用@value
    @Value("${user.name}") //从配置文件中取值
    private String name;
    @Value("#{9*2}")  // #{SPEL} Spring表达式
    private int age;
    @Value("男")  // 字面量
    private String sex;
}
```

4、Springboot测试

```java
@SpringBootTest
class DemoApplicationTests {
    @Autowired
    User user;
    @Test
    public void contextLoads() {
        System.out.println(user);
    }
}
```

结果正常输出：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615215544163-4eaf9a81-9d0c-4db4-9ab1-db5954a9d816.png)

## 对比小结

@Value这个使用起来并不友好！我们需要为每个属性单独注解赋值，比较麻烦；我们来看个功能对比图

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615215554517-0751b409-3199-48ec-b59a-99ad427ad6f5.png)

1、@ConfigurationProperties只需要写一次即可 ， @Value则需要每个字段都添加

2、松散绑定：这个什么意思呢? 比如我的yaml中写的last-name，这个和lastName是一样的， - 后面跟着的字母默认是大写的。这就是松散绑定。可以测试一下

3、JSR303数据校验 ， 这个就是我们可以在字段是增加一层过滤器验证 ， 可以保证数据的合法性

4、复杂类型封装，yaml中可以封装对象 ， 使用value就不支持

结论：

- 配置yaml和配置properties都可以获取到值，强烈推荐 yaml；
- 如果我们在某个业务中，只需要获取配置文件中的某个值，可以使用一下 @value；
- 如果说，我们专门编写了一个JavaBean来和配置文件进行一一映射，就直接@configurationProperties，不要犹豫！

# JSR303数据校验

需要先导入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

Springboot中可以用@validated来校验数据，如果数据异常则会统一抛出异常，方便异常中心统一处理。我们这里来写个注解让我们的name只能支持Email格式；

```java
@Component //注册bean
@ConfigurationProperties(prefix = "person")
@Validated  //数据校验
public class Person {
    @Email(message="邮箱格式错误") //name必须是邮箱格式
    private String name;
}
```

运行结果 ：default message [不是一个合法的电子邮件地址];

使用数据校验，可以保证数据的正确性；

## 常见参数

```java
@NotNull(message="名字不能为空")
private String userName;
@Max(value=120,message="年龄最大不能查过120")
private int age;
@Email(message="邮箱格式错误")
private String email;
空检查
@Null       验证对象是否为null
@NotNull    验证对象是否不为null, 无法查检长度为0的字符串
@NotBlank   检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
@NotEmpty   检查约束元素是否为NULL或者是EMPTY.
   
Booelan检查
@AssertTrue     验证 Boolean 对象是否为 true  
@AssertFalse    验证 Boolean 对象是否为 false  
   
长度检查
@Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内  
@Length(min=, max=) string is between min and max included.
日期检查
@Past       验证 Date 和 Calendar 对象是否在当前时间之前  
@Future     验证 Date 和 Calendar 对象是否在当前时间之后  
@Pattern    验证 String 对象是否符合正则表达式的规则
```

除此以外，我们还可以自定义一些数据校验规则

#  多环境切换

profile是Spring对不同环境提供不同配置功能的支持，可以通过激活不同的环境版本，实现快速切换环境；

## 多配置文件

我们在主配置文件编写的时候，文件名可以是 application-{profile}.properties/yml , 用来指定多个环境版本；

例如：

application-test.properties 代表测试环境配置

application-dev.properties 代表开发环境配置

但是Springboot并不会直接启动这些配置文件，它默认使用application.properties主配置文件；

我们需要通过一个配置来选择需要激活的环境：

```xml
#比如在配置文件中指定使用dev环境，我们可以通过设置不同的端口号进行测试；
#我们启动SpringBoot，就可以看到已经切换到dev下的配置了；
spring.profiles.active=dev  
#springboot的多环境配置： 可以选择激活哪一个配置文件
```

## yaml的多文档模块

和properties配置文件中一样，但是使用yml去实现不需要创建多个配置文件，更加方便了 !

```yaml
server:
  port: 8081
#选择要激活那个环境块
spring:
  profiles:
    active: prod
---
server:
  port: 8083
spring:
  profiles: dev #配置环境的名称
---
server:
  port: 8084
spring:
  profiles: prod  #配置环境的名称
```

注意：如果yml和properties同时都配置了端口，并且没有激活其他环境 ， 默认会使用properties配置文件的！

## 配置文件加载位置

外部加载配置文件的方式十分多，我们选择最常用的即可，在开发的资源文件中进行配置！

官方外部配置文件说明参考文档

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615216783311-7d418348-7e07-4895-933c-607029fb26bc.png)

springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件：

- 优先级1：项目路径下的config文件夹配置文件

- 优先级2：项目路径下配置文件

- 优先级3：资源路径下的config文件夹配置文件

- 优先级4：资源路径下配置文件

优先级由高到底，高优先级的配置会覆盖低优先级的配置；

SpringBoot会从这四个位置全部加载主配置文件；互补配置；

我们在最低级的配置文件中设置一个项目访问路径的配置来测试互补问题；

```yaml
#配置项目的访问路径
server.servlet.context-path=/kuang
```



## 拓展，运维小技巧

指定位置加载配置文件

我们还可以通过spring.config.location来改变默认的配置文件位置

项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；这种情况，一般是后期运维做的多，相同配置，外部指定的配置文件优先级最高

```yaml
java -jar spring-boot-config.jar --spring.config.location=F:/application.properties
```

# 自动配置原理

配置文件到底能写什么？怎么写？

SpringBoot官方文档中有大量的配置，我们无法全部记住

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615216859342-03d93c43-f68c-4c01-aefa-665a7ebf2a36.png)

## 分析自动配置原理

```java
//表示这是一个配置类，和以前编写的配置文件一样，也可以给容器中添加组件；
@Configuration 
//启动指定类的ConfigurationProperties功能；
  //进入这个HttpProperties查看，将配置文件中对应的值和HttpProperties绑定起来；
  //并把HttpProperties加入到ioc容器中
@EnableConfigurationProperties({HttpProperties.class}) 
//Spring底层@Conditional注解
  //根据不同的条件判断，如果满足指定的条件，整个配置类里面的配置就会生效；
  //这里的意思就是判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnWebApplication(
    type = Type.SERVLET
)
//判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；
@ConditionalOnClass({CharacterEncodingFilter.class})
//判断配置文件中是否存在某个配置：spring.http.encoding.enabled；
  //如果不存在，判断也是成立的
  //即使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的；
@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
)
public class HttpEncodingAutoConfiguration {
    //他已经和SpringBoot的配置文件映射了
    private final Encoding properties;
    //只有一个有参构造器的情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpProperties properties) {
        this.properties = properties.getEncoding();
    }
   
    //给容器中添加一个组件，这个组件的某些值需要从properties中获取
    @Bean
    @ConditionalOnMissingBean //判断容器没有这个组件？
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));
        return filter;
    }
    //。。。。。。。
}
```

一句话总结 ：根据当前不同的条件判断，决定这个配置类是否生效！

- 一但这个配置类生效；这个配置类就会给容器中添加各种组件；

- 这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；

- 所有在配置文件中能配置的属性都是在xxxxProperties类中封装着；

- 配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
//从配置文件中获取指定的值和bean的属性进行绑定
@ConfigurationProperties(prefix = "spring.http") 
public class HttpProperties {
    // .....
}
```

我们去配置文件里面试试前缀，看提示！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615217228439-ab3f2519-bd84-46a5-a3d4-9669497e4034.png)

这就是自动装配的原理！

## 精髓

1、SpringBoot启动会加载大量的自动配置类

2、我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中；

3、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需要再手动配置了）

4、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可；

**xxxxAutoConfigurartion：自动配置类；**给容器中添加组件

**xxxxProperties:封装配置文件中相关属性；**

## 了解：@Conditional

了解完自动装配的原理后，我们来关注一个细节问题，自动配置类必须在一定的条件下才能生效；

**@Conditional派生注解（Spring注解版原生的@Conditional作用）**

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615217330697-6f981ebf-1ccc-4204-8297-9366cf488d9b.png)

可通过启用 debug=true属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效；

```java
#开启springboot的调试类
debug=true
```

Positive matches:（自动配置类启用的：正匹配）

Negative matches:（没有启动，没有匹配成功的自动配置类：负匹配）

Unconditional classes: （没有条件的类）

# MVC自动配置原理

**官网阅读**

地址 ：https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-auto-configuration

Spring Boot为Spring MVC提供了自动配置，可与大多数应用程序完美配合。

以下是SpringBoot对SpringMVC的默认配置

**`org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration`**

自动配置在Spring的默认值之上添加了以下功能：

- 包含`ContentNegotiatingViewResolver`和`BeanNameViewResolver`。--> 视图解析器
- 支持服务静态资源，包括对WebJars的支持（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content)）。--> 静态资源文件夹路径
- 自动注册`Converter`，`GenericConverter`和`Formatter`beans。--> 转换器，格式化器
- 支持`HttpMessageConverters`（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-message-converters)）。--> SpringMVC用来转换Http请求和响应的；User---Json；
- 自动注册`MessageCodesResolver`（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-message-codes)）。--> 定义错误代码生成规则
- 静态`index.html`支持。--> 静态首页访问
- 定制`Favicon`支持（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-favicon)）。--> 网站图标
- 自动使用`ConfigurableWebBindingInitializer`bean（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-web-binding-initializer)）。

如果想保留 Spring Boot MVC 的功能，并且需要添加其他 [MVC 配置](https://docs.spring.io/spring/docs/5.1.3.RELEASE/spring-framework-reference/web.html#mvc)（拦截器，格式化程序和视图控制器等），可以添加自己的 `WebMvcConfigurer` 类型的 `@Configuration` 类，但**不能**带 `@EnableWebMvc` 注解。

如果想自定义 `RequestMappingHandlerMapping`、`RequestMappingHandlerAdapter` 或者 `ExceptionHandlerExceptionResolver` 实例，可以声明一个 `WebMvcRegistrationsAdapter` 实例来提供这些组件。

如果您想完全掌控 Spring MVC，可以添加自定义注解了 `@EnableWebMvc` 的 @Configuration 配置类。

**视图解析器**

视图解析器：根据方法的返回值得到视图对象（View），视图对象决定如何渲染（转发？重定向？）:

- 自动配置了ViewResolver
- ContentNegotiatingViewResolver：组合所有的视图解析器的；

 WebMvcAutoConfiguration ， 然后搜索ContentNegotiatingViewResolver。找到如下方法！

```java
@Bean
@ConditionalOnBean({ViewResolver.class})
@ConditionalOnMissingBean(
    name = {"viewResolver"},
    value = {ContentNegotiatingViewResolver.class}
)
public ContentNegotiatingViewResolver viewResolver(BeanFactory beanFactory) {
    ContentNegotiatingViewResolver resolver = new ContentNegotiatingViewResolver();
    resolver.setContentNegotiationManager((ContentNegotiationManager)beanFactory.getBean(ContentNegotiationManager.class));
    // ContentNegotiatingViewResolver使用所有其他视图解析器来定位视图，因此它应该具有较高的优先级
    resolver.setOrder(Ordered.HIGHEST_PRECEDENCE);
    return resolver;
}
```

找到对应的解析视图的代码；

```java
@Nullable // 注解说明：@Nullable 即参数可为null
public View resolveViewName(String viewName, Locale locale) throws Exception {
    RequestAttributes attrs = RequestContextHolder.getRequestAttributes();
    Assert.state(attrs instanceof ServletRequestAttributes, "No current ServletRequestAttributes");
    List<MediaType> requestedMediaTypes = this.getMediaTypes(((ServletRequestAttributes)attrs).getRequest());
    if (requestedMediaTypes != null) {
        // 获取候选的视图对象
        List<View> candidateViews = this.getCandidateViews(viewName, locale, requestedMediaTypes);
        // 选择一个最适合的视图对象，然后把这个对象返回
        View bestView = this.getBestView(candidateViews, requestedMediaTypes, attrs);
        if (bestView != null) {
            return bestView;
        }
    }
    // .....
}
```

getCandidateViews中看到他是把所有的视图解析器拿来，进行while循环，挨个解析！

```java
Iterator var5 = this.viewResolvers.iterator();
```

所以得出结论：ContentNegotiatingViewResolver 这个视图解析器就是用来组合所有的视图解析器的

再研究下他的组合逻辑，看到有个属性viewResolvers，看看它是在哪里进行赋值的！

```java
protected void initServletContext(ServletContext servletContext) {
    // 这里它是从beanFactory工具中获取容器中的所有视图解析器
    // ViewRescolver.class 把所有的视图解析器来组合的
    Collection<ViewResolver> matchingBeans = BeanFactoryUtils.beansOfTypeIncludingAncestors(this.obtainApplicationContext(), ViewResolver.class).values();
    ViewResolver viewResolver;
    if (this.viewResolvers == null) {
        this.viewResolvers = new ArrayList(matchingBeans.size());
        Iterator var3 = matchingBeans.iterator();
    }
    // ...............
}
```

1、我们在我们的主程序中去写一个视图解析器来试试；

```java
// 如果 想DIY一些定制化的功能，只要写这个组件，然后将它交给springboot，springboot就会帮我们自动装配
//扩展springmvc
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    //ViewResolver 实现类视图解析器接口的类，我们就可以把它看做视图解析器
    @Bean
    public ViewResolver myViewResolver(){
        return new MyViewResolver();
    }
    //我们写一个静态内部类，视图解析器就需要实现ViewResolver接口
    public static class MyViewResolver implements ViewResolver {
        @Override
        public View resolveViewName(String s, Locale locale) throws Exception {
            return null;
        }
    }
```

2、怎么看我们自己写的视图解析器有没有起作用呢？

我们给 DispatcherServlet 中的 doDispatch方法 加个断点进行调试一下，因为所有的请求都会走到这个方法中

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615219210862-52729e25-1d18-42e7-9eb7-ef64e81b91e7.png)

3、我们启动我们的项目，然后随便访问一个页面，看一下Debug信息；

找到this

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615219233769-72887c88-86c2-4ba7-af40-95bd46603db3.png)

找到视图解析器，我们看到我们自己定义的就在这里了；

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615219237941-f51cf178-d37a-42dc-8a68-43b55dab0d96.png)

## 转换器和格式化器

在WebMvcAutoConfiguration中找到格式化转换器：

```java
@Bean
@Override
public FormattingConversionService mvcConversionService() {
    // 拿到配置文件中的格式化规则
    WebConversionService conversionService = 
        new WebConversionService(this.mvcProperties.getDateFormat());
    addFormatters(conversionService);
    return conversionService;
}
```

点进去：

```java
public String getDateFormat() {
    return this.dateFormat;
}
/**
* Date format to use. For instance, `dd/MM/yyyy`. 默认的
 */
private String dateFormat;
```

可以看到在我们的Properties文件中，我们可以进行自动配置它！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615219305643-479e7603-d3dc-4cc6-bcac-dad4869d4db6.png)

如果配置了自己的格式化方式，就会注册到Bean中生效，我们可以在配置文件中配置日期格式化的规则：

## 修改SpringBoot的默认配置

SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（如果用户自己配置@bean），如果有就用用户配置的，如果没有就用自动配置的；

如果有些组件可以存在多个，比如我们的视图解析器，就将用户配置的和自己默认的组合起来！

编写一个@Configuration注解类，并且**类型要为WebMvcConfigurer，还不能标注@EnableWebMvc注解；**我们去自己写一个；我们新建一个包叫config，写一个类MyMvcConfig；

```java
//应为类型要求为WebMvcConfigurer，所以我们实现其接口
//可以使用自定义类扩展MVC的功能
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        // 浏览器发送/test ， 就会跳转到test页面；
        registry.addViewController("/test").setViewName("test");
    }
}
```

我们去浏览器访问一下：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615219411733-64d3e859-70d1-451b-b6d5-a1a4c8aaf706.png)

我们可以去分析一下原理：

1、WebMvcAutoConfiguration 是 SpringMVC的自动配置类，里面有一个类WebMvcAutoConfigurationAdapter

2、这个类上有一个注解，在做其他自动配置时会导入：@Import(EnableWebMvcConfiguration.class)

3、我们点进EnableWebMvcConfiguration这个类看一下，它继承了一个父类：DelegatingWebMvcConfiguration

这个父类中有这样一段代码：

```java
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
    private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
   
  // 从容器中获取所有的webmvcConfigurer
    @Autowired(required = false)
    public void setConfigurers(List<WebMvcConfigurer> configurers) {
        if (!CollectionUtils.isEmpty(configurers)) {
            this.configurers.addWebMvcConfigurers(configurers);
        }
    }
}
```

4、我们可以在这个类中去寻找一个我们刚才设置的viewController当做参考，发现它调用了一个

```java
protected void addViewControllers(ViewControllerRegistry registry) {
    this.configurers.addViewControllers(registry);
}
```

5、我们点进去看一下

```java
public void addViewControllers(ViewControllerRegistry registry) {
    Iterator var2 = this.delegates.iterator();
    while(var2.hasNext()) {
        // 将所有的WebMvcConfigurer相关配置来一起调用！包括我们自己配置的和Spring给我们配置的
        WebMvcConfigurer delegate = (WebMvcConfigurer)var2.next();
        delegate.addViewControllers(registry);
    }
}
```

容器中所有的WebMvcConfigurer都会一起起作用；

我们的配置类也会被调用；

效果：SpringMVC的自动配置和我们的扩展配置都会起作用；



## 全面接管SpringMVC

全面接管即：SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己去配置！

只需在我们的配置类中要加一个@EnableWebMvc。

我们看下如果我们全面接管了SpringMVC了，我们之前SpringBoot给我们配置的静态资源映射一定会无效，我们可以去测试一下；

不加注解之前，访问首页：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615219654848-89a05ccb-6d66-48f9-8084-c95591fada06.png)

**给配置类加上注解：@EnableWebMvc**

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615219674044-fbde63ec-7e6d-4c2e-aae4-df9c30256bbb.png)

发现所有的SpringMVC自动配置都失效了，开发中，不推荐使用全面接管SpringMVC

思考问题？为什么加了一个注解，自动配置就失效了！我们看下源码：

1、这里发现它是导入了一个类，我们可以继续进去看

```java
@Import({DelegatingWebMvcConfiguration.class})
public @interface EnableWebMvc {
}
```

2、它继承了一个父类 WebMvcConfigurationSupport

```java
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
  // ......
}
```

3、我们来回顾一下Webmvc自动配置类

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
// 这个注解的意思就是：容器中没有这个组件的时候，这个自动配置类才生效
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
    ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {
   
}
```

总结：

1. @EnableWebMvc将WebMvcConfigurationSupport组件导入进来；
2. 导入的WebMvcConfigurationSupport只是SpringMVC最基本的功能；

# 自定义starter

## 说明

启动器模块是一个 空 jar 文件，仅提供辅助性依赖管理，这些依赖可能用于自动装配或者其他类库；

**命名归约：**

官方命名：

前缀：spring-boot-starter-xxx

比如：spring-boot-starter-web…

自定义命名：

xxx-spring-boot-starter

比如：mybatis-spring-boot-starter

## 编写启动器

1、在IDEA中新建一个空项目 spring-boot-starter-diy

2、新建一个普通Maven模块：kuang-spring-boot-starter

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615217409685-62e2193a-80ee-49f1-9f51-1d671d68f65e.png)

3、新建一个Springboot模块：kuang-spring-boot-starter-autoconfigure

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615217409556-72656a88-c55d-430e-bff5-9575d8bac0a5.png)

4、点击apply即可，基本结构

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615217413299-19d283ce-54e6-406d-8fa2-f9afc6fdd081.png)

5、在我们的 starter 中 导入 autoconfigure 的依赖！

```xml
<!-- 启动器 -->
<dependencies>
    <!--  引入自动配置模块 -->
    <dependency>
        <groupId>com.kuang</groupId>
        <artifactId>kuang-spring-boot-starter-autoconfigure</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </dependency>
</dependencies>
```

6、将 autoconfigure 项目下多余的文件都删掉，Pom中只留下一个 starter，这是所有的启动器基本配置！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615217457572-bc5ae913-ed45-463f-b666-015597c1fb7b.png)

7、我们编写一个自己的服务

```java
public class HelloService {
    HelloProperties helloProperties;
    public HelloProperties getHelloProperties() {
        return helloProperties;
    }
    public void setHelloProperties(HelloProperties helloProperties) {
        this.helloProperties = helloProperties;
    }
    public String sayHello(String name){
        return helloProperties.getPrefix() + name + helloProperties.getSuffix();
    }
}
```

8、编写HelloProperties 配置类

```java
// 前缀 kuang.hello
@ConfigurationProperties(prefix = "kuang.hello")
public class HelloProperties {
    private String prefix;
    private String suffix;
    public String getPrefix() {
        return prefix;
    }
    public void setPrefix(String prefix) {
        this.prefix = prefix;
    }
    public String getSuffix() {
        return suffix;
    }
    public void setSuffix(String suffix) {
        this.suffix = suffix;
    }
}
```

9、编写我们的自动配置类并注入bean，测试

```java
@Configuration
@ConditionalOnWebApplication //web应用生效
@EnableConfigurationProperties(HelloProperties.class)
public class HelloServiceAutoConfiguration {
    @Autowired
    HelloProperties helloProperties;
    @Bean
    public HelloService helloService(){
        HelloService service = new HelloService();
        service.setHelloProperties(helloProperties);
        return service;
    }
}
```

10、在resources编写一个自己的 META-INF\spring.factories

```java
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.kuang.HelloServiceAutoConfiguration
```

11、编写完成后，可以安装到maven仓库中！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615217560837-0b43a51c-3c3b-4540-84e3-ffab3ff51c0b.png)

## 新建项目测试我们自己写的启动器

1、新建一个SpringBoot 项目

2、导入我们自己写的启动器

```xml
<dependency>
    <groupId>com.kuang</groupId>
    <artifactId>kuang-spring-boot-starter</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

3、编写一个 HelloController 进行测试我们自己的写的接口！

```java
@RestController
public class HelloController {
    @Autowired
    HelloService helloService;
    @RequestMapping("/hello")
    public String hello(){
        return helloService.sayHello("zxc");
    }
}
```

4、编写配置文件 application.properties

```java
kuang.hello.prefix="ppp"
kuang.hello.suffix="sss"
```

5、启动项目进行测试，结果成功 !

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615217591727-0bc83527-146c-47ad-95eb-8ce15711af72.png)

# 拦截器、过滤器、监听器

## 监听器

Servlet 监听器是 Servlet 规范中定义的一种特殊类，用于监听 ServletContext、HttpSession 和 ServletRequest 等域对象的创建与销毁事件，以及监听这些域对象中属性发生修改的事件。监听器是观察者模式的应用，它关注特定事物，并伺机而动，所以监听器具有**异步**的特性。

Servlet Listener 监听三大域对象的创建和销毁事件，三大对象分别是：

1. ServletContext：application 级别，整个应用只存在一个，可以进行全局应用配置。
2. HttpSession：session 级别，针对每一个对话，如统计会话总数。
3. ServletRequest：request 级别，针对每一个客户请求。

**场景**

Servlet 规范设计监听器的作用是在事件发生前、发生后进行一些处理，一般可以用来统计在线人数和在线用户、统计网站访问量、系统启动时初始化信息等。我们可以在容器启动时初始化 Log4j 信息，添加自己对容器状态的监控，初始化 Spring 组件等。

**实现**

创建一个ServletRequest监听器(其他监听器类似创建)

```java
@WebListener
@Slf4j
public class Customlister implements ServletRequestListener{

    @Override
    public void requestDestroyed(ServletRequestEvent sre) {
        log.info(" request监听器：销毁");
    }

    @Override
    public void requestInitialized(ServletRequestEvent sre) {
        log.info(" request监听器：可以在这里记录访问次数哦");
    }

}
```

在启动类中加入@ServletComponentScan进行自动注册即可。

## 过滤器

过滤器是一种可重用的代码，可以转换 HTTP 请求、响应和头信息，通俗来说就是过滤器可以在请求到达服务器之前，对请求头进行预先处理，在响应内容到达客户端之前，对服务器做出的响应进行后置处理。
位于 Listener 的下游，Servlet 的上游。在 Listener 执行之后，在请求到达 Servlet 之前进行预处理。

**场景**

Servlet 3.1 中定义了几种常见的过滤器组件：

| 过滤器                                        | 作用                         |
| :-------------------------------------------- | :--------------------------- |
| Authentication filters：                      | 授权类，如用户登陆会话校验； |
| Logging and auditing filters：                | 日志和安全审计类；           |
| Image conversion filters：                    | 图片转换；                   |
| Data compression filters：                    | 数据压缩；                   |
| Encryption filters：                          | 加密、解密类；               |
| Tokenizing filters：                          | 词法类；                     |
| Filters that trigger resource access events： | 触发资源访问事件类；         |
| XSL/T filters that transform XML content：    | XML文件转换类；              |
| MIME-type chain filters：                     | MIME文件；                   |
| Caching filters：                             | 缓存类；                     |

或者我们社交应用经常需要的敏感词过滤，都可以使用过滤器。过滤器主要的特点在于，它能够改变请求内容。

**实现**

**利用WebFilter注解配置**

@WebFilter是Servlet3.0新增的注解，原先实现过滤器，需要在web.xml中进行配置，而现在通过此注解，启动启动时会自动扫描自动注册。

编写Filter类：

```java
//注册器名称为customFilter,拦截的url为所有
@WebFilter(filterName="customFilter",urlPatterns={"/*"})
@Slf4j
public class CustomFilter implements Filter{

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("filter 初始化");
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        log.info("customFilter 请求处理之前");
        //对request、response进行一些预处理
        // 比如设置请求编码
        // request.setCharacterEncoding("UTF-8");
        // response.setCharacterEncoding("UTF-8");

        //链路 直接传给下一个过滤器
        chain.doFilter(request, response);

        log.info("customFilter 请求处理之后");
    }

    @Override
    public void destroy() {
        log.info("filter 销毁");
    }
}
```

在启动类加入@ServletComponentScan注解即可。
使用这种方法，当注册多个过滤器时，无法指定执行顺序，可通过FilterRegistrationBean进行过滤器的注册。

> –小技巧–
> 通过过滤器的java类名称，进行顺序的约定，比如LogFilter和AuthFilter，此时AuthFilter就会比LogFilter先执行，因为首字母A比L前面。

**FilterRegistrationBean方式**

FilterRegistrationBean是springboot提供的，此类提供setOrder方法，可以为filter设置排序值。

```java
@Configuration
public class FilterRegistration {
    @Bean
    public FilterRegistrationBean filterRegistrationBean() {
        FilterRegistrationBean registration = new FilterRegistrationBean();
        //当过滤器有注入其他bean类时，可直接通过@bean的方式进行实体类过滤器，这样不可自动注入过滤器使用的其他bean类。
        //当然，若无其他bean需要获取时，可直接new CustomFilter()，也可使用getBean的方式。
        registration.setFilter(customFilter());
        //过滤器名称
        registration.setName("customFilter");
        //拦截路径
        registration.addUrlPatterns("/*");
        //设置顺序
        registration.setOrder(10);
        return registration;
    }

    @Bean
    public Filter customFilter() {
        return new CustomFilter();
    }
}
```

注册多个时，就注册多个FilterRegistrationBean即可,启动后，效果和第一种是一样的。

## 拦截器

在 Servlet 规范中并没有拦截器的概念，它是面向切面编程的一种应用：在需要对方法进行增强的场景下，例如在方法调用前执行一段代码，或者在方法完成后额外执行一段操作，拦截器的一种实现方式就是动态代理。

**场景：AOP**

**实现**

编写自定义拦截器类

```java
@Slf4j
public class CustomHandlerInterceptor implements HandlerInterceptor{

 @Override
 public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
   throws Exception {
  log.info("preHandle:请求前调用");
  //返回 false 则请求中断
  return true;
 }

 @Override
 public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
   ModelAndView modelAndView) throws Exception {
  log.info("postHandle:请求后调用");

 }

 @Override
 public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
   throws Exception {
  log.info("afterCompletion:请求调用完成后回调方法，即在视图渲染完成后回调");

 }

}
```

实现WebMvcConfigurer接口完成拦截器的注册。

```java
@Configuration
//废弃：public class MyWebMvcConfigurer extends WebMvcConfigurerAdapter{
public class MyWebMvcConfigurer implements WebMvcConfigurer 
 @Override
  public void addInterceptors(InterceptorRegistry registry) {
   //注册拦截器 拦截规则
  //多个拦截器时 以此添加 执行顺序按添加顺序
  registry.addInterceptor(getHandlerInterceptor()).addPathPatterns("/*");
  }
	
 @Bean
 public static HandlerInterceptor getHandlerInterceptor() {
  return new CustomHandlerInterceptor();
 }
}
```

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200426151331.png)

## 自定义事件发布与监听

**自定义事件和自定义监听器类的实现方式**

- 自定义事件：继承自ApplicationEvent抽象类，然后定义自己的构造器
- 自定义监听：实现ApplicationListener接口，然后实现onApplicationEvent方法

**springboot进行事件监听有四种方式**

- 1.手工向ApplicationContext中添加监听器
- 2.将监听器装载入spring容器
- 3.在application.properties中配置监听器
- 4.通过@EventListener注解实现事件监听

**方式一**

创建MyListener1类

```java
@Slf4j
public class MyListener1 implements ApplicationListener<MyEvent> {
    public void onApplicationEvent(MyEvent event) {
        log.info(String.format("%s监听到事件源：%s.", MyListener1.class.getName(), event.getSource()));
    }
}
```

在springboot应用启动类中获取ConfigurableApplicationContext上下文，装载监听

```java
@SpringBootApplication
public class BootLaunchApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(BootLaunchApplication.class, args);
        //装载监听
        context.addApplicationListener(new MyListener1());
    }
}
```

**方式二**

创建MyListener2类，并使用@Component注解将该类装载入spring容器中

```java
@Component
@Slf4j
public class MyListener2 implements ApplicationListener<MyEvent> {

    public void onApplicationEvent(MyEvent event) {
        log.info(String.format("%s监听到事件源：%s.", MyListener2.class.getName(), event.getSource()));
    }

}
```

**方式三**

创建MyListener3类

```java
@Slf4j
public class MyListener3 implements ApplicationListener<MyEvent> {
    public void onApplicationEvent(MyEvent event) {
        log.info(String.format("%s监听到事件源：%s.", MyListener3.class.getName(), event.getSource()));
    }
}
```

在application.yml中配置监听

```yaml
context:
  listener:
    classes: club.krislin.customlistener.MyListener3
```

**方式四**

创建MyListener4类，该类无需实现ApplicationListener接口，使用@EventListener装饰具体方法

```java
@Slf4j
@Component
public class MyListener4 {
    @EventListener
    public void listener(MyEvent event) {
        log.info(String.format("%s监听到事件源：%s.", MyListener4.class.getName(), event.getSource()));
    }
}
```

自定义事件代码如下：

```java
@SuppressWarnings("serial")
public class MyEvent extends ApplicationEvent
{
 public MyEvent(Object source)
 {
  super(source);
 }
}
```

**测试**

有了applicationContext，想在哪发布事件就在哪发布事件

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class CustomListenerTest {

    @Resource private
    ApplicationContext applicationContext;

    @Test
    public void testEvent(){
        applicationContext.publishEvent(new MyEvent("测试事件."));
    }
}
```

启动后，日志打印如下。（下面截图是在启动类发布事件后的截图，在单元测试里面监听器1监听不到，执行顺序问题）：

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200426153729.png)

由日志打印可以看出，SpringBoot四种事件的实现方式监听是有序的。无论执行多少次都是这个顺序。

## 应用启动的监听

Spring Boot提供了两个接口：CommandLineRunner、ApplicationRunner，用于启动应用时做特殊处理，这些代码会在SpringApplication的run()方法运行完成之前被执行。

通常用于应用启动前的特殊代码执行、特殊数据加载、垃圾数据清理、微服务的服务发现注册、系统启动成功后的通知等。相当于Spring的ApplicationListener、Servlet的ServletContextListener。**使用二者的好处在于，可以方便的使用应用启动参数**，根据参数不同做不同的初始化操作。

**实现**

**通过@Component定义方式实现**，CommandLineRunner：参数是字符串数组

```java
@Slf4j
@Component
public class CommandLineStartupRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        log.info("CommandLineRunner传入参数：{}", Arrays.toString(args));
    }
}
```

ApplicationRunner：参数被放入ApplicationArguments，通过getOptionNames()、getOptionValues()、getSourceArgs()获取参数

```java
@Slf4j
@Component
public class AppStartupRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        log.info("ApplicationRunner参数: {}", args.getOptionNames());
    }
}
```

**通过@Bean定义方式实现**

```java
@Configuration
public class BeanRunner {
    @Bean
    @Order(1)
    public CommandLineRunner runner1(){
        return new CommandLineRunner() {
            public void run(String... args){
                System.out.println("CommandLineRunner run1()" + Arrays.toString(args));
            }
        };
    }

    @Bean
    @Order(2)
    public CommandLineRunner runner2(){
        return new CommandLineRunner() {
            public void run(String... args){
                System.out.println("CommandLineRunner run2()" + Arrays.toString(args));
            }
        };
    }

    @Bean
    @Order(3)
    public CommandLineRunner runner3(){
        return new CommandLineRunner() {
            public void run(String... args){
                System.out.println("CommandLineRunner run3()" + Arrays.toString(args));
            }
        };
    }
}
```

可以通过@Order设置执行顺序

![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img/20200426155029.png)



# 模板引擎

jsp支持非常强大的功能，包括能写Java代码，而SpringBoot这个项目首先是以jar的方式，不是war，其次用的还是嵌入式的Tomcat，所以呢，他现在**默认是不支持jsp**的。

模板引擎有jsp、freemarker，包括SpringBoot给我们推荐的Thymeleaf，思想都一样：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615218142610-a69cccea-e4f7-43c9-ad58-0d735042957b.png)

模板引擎的作用就是配置页面模板后，从后台封装一些数据，将模板和数据交给我们模板引擎解析。

## 引入Thymeleaf

Thymeleaf 官网：https://www.thymeleaf.org/

Thymeleaf 在Github 的主页：https://github.com/thymeleaf/thymeleaf

Spring官方文档：找到我们对应的版本

https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter

找到对应的pom依赖：可以适当点进源码看下本来的包！

```xml
<!--thymeleaf-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

Maven会自动下载jar包，我们可以去看下载的东西；

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615218194997-61d7cc64-9a44-4d17-8c42-01a7b875891a.png)

## Thymeleaf分析

找下Thymeleaf的自动配置类：ThymeleafProperties

```java
@ConfigurationProperties(
    prefix = "spring.thymeleaf"
)
public class ThymeleafProperties {
    private static final Charset DEFAULT_ENCODING;
    public static final String DEFAULT_PREFIX = "classpath:/templates/";
    public static final String DEFAULT_SUFFIX = ".html";
    private boolean checkTemplate = true;
    private boolean checkTemplateLocation = true;
    private String prefix = "classpath:/templates/";
    private String suffix = ".html";
    private String mode = "HTML";
    private Charset encoding;
}
```

使用thymeleaf什么都不需要配置，只需要将他放在指定的文件夹下即可！

**测试**

1、增加数据传输；

```java
@Controller
public class TestController {
    @RequestMapping("/test")
    public String test(Model model){
        model.addAttribute("msg","hello springboot");
        return "test";
    }
}
```

2、编写前端页面

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <!--注意：  所有的html元素都可以被thymeleaf替换接管；   th: 元素名-->
    <!--th:text就是将div中的内容设置为它指定的值，和之前学习的Vue一样-->
<div th:text="${msg}"></div>
</body>
</html>
```

3、测试

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615218442897-f422ae4c-cf0a-49f0-a07f-35f26fb9e398.png)

- 小结：只要需要使用thymeleaf，只需要导入对应的依赖就可以了，我们需要将html页面放在我们的templates目录下即可

## Thymeleaf 语法学习

- Thymeleaf 官网：https://www.thymeleaf.org/ ， 简单看一下官网！我们去下载Thymeleaf的官方文档！

要使用thymeleaf，需要在html文件中导入命名空间的约束

```xml
xmlns:th="http://www.thymeleaf.org"
```

**Thymeleaf的使用语法**

1、我们可以使用任意的 th:attr 来替换Html中原生属性的值！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615218453640-c8123423-cde1-47cf-914a-e831f7db174e.png)

### 表达式

**变量表达式 `${}`**

使用方法：直接使用`th:xx = "${}"` 获取对象属性 。例如：

```html
<form id="userForm">
    <input id="id" name="id" th:value="${user.id}"/>
    <input id="username" name="username" th:value="${user.username}"/>
    <input id="password" name="password" th:value="${user.password}"/>
</form>

<div th:text="hello"></div>

<div th:text="${articles[0].username}"></div>
```

**选择变量表达式 `*{}`**

使用方法：首先通过`th:object` 获取对象，然后使用`th:xx = "*{}"`获取对象属性。

```html
<form id="userForm" th:object="${user}">
    <input id="id" name="id" th:value="*{id}"/>
    <input id="username" name="username" th:value="*{username}"/>
    <input id="password" name="password" th:value="*{password}"/>
</form>
```

**链接表达式 `@{}`**

使用方法：通过链接表达式`@{}`直接拿到应用路径，然后拼接静态资源路径。例如：

```html
<script th:src="@{/webjars/jquery/jquery.js}"></script>
<link th:href="@{/webjars/bootstrap/css/bootstrap.css}" rel="stylesheet" type="text/css">
```

**其它表达式**

在基础语法中，默认支持字符串连接、数学运算、布尔逻辑和三目运算等。例如：

```html
<input name="name" th:value="${'I am '+(user.name!=null?user.name:'NoBody')}"/>
```

### **迭代循环**

想要遍历`List`集合很简单，配合`th:each` 即可快速完成迭代。例如遍历用户列表：

```html
<div th:each="article:${articles}" >
    作者：<input th:value="${article.author}"/>
    标题：<input th:value="${article.title}"/>
    内容：<input th:value="${article.content}"/>
</div>
```

在集合的迭代过程还可以获取状态变量，只需在变量后面指定状态变量名即可，状态变量可用于获取集合的下标/序号、总数、是否为单数/偶数行、是否为第一个/最后一个。例如：

```html
<div th:each="article,stat:${articles}" th:class="${stat.even}?'even':'odd'">
    下标：<input th:value="${stat.index}"/>
    序号：<input th:value="${stat.count}"/>
    作者：<input th:value="${article.author}"/>
    标题：<input th:value="${article.title}"/>
    内容：<input th:value="${article.content}"/>
</div>
```

**迭代下标变量用法：**
状态变量定义在一个th:每个属性和包含以下数据:

1. 当前迭代索引,从0开始。这是索引属性。index
2. 当前迭代索引,从1开始。这是统计属性。count
3. 元素的总量迭代变量。这是大小属性。　size
4. iter变量为每个迭代。这是目前的财产。　current
5. 是否当前迭代是奇数还是偶数。这些even/odd的布尔属性。
6. 是否第一个当前迭代。这是first布尔属性。
7. 是否最后一个当前迭代。这是last布尔属性。

### 条件判断

条件判断通常用于动态页面的初始化，例如：

```html
<div th:if="${articles}">
    <div>的确存在..</div>
</div>
```

如果想取反则使用unless 例如：

```html
<div th:unless="${articles}">
    <div>不存在..</div>
</div>
```

### 内置对象

> 官方文档： [Thymeleaf 3.0 基础对象](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#appendix-a-expression-basic-objects)

**七大基础对象：**

- `${#ctx}` 上下文对象，可用于获取其它内置对象。
- `${#param}`: 上下文变量。
- `${#locale}`：上下文区域设置。
- `${#request}`: `HttpServletRequest`对象。
- `${#response}`: `HttpServletResponse`对象。
- `${#session}`: `HttpSession`对象。
- `${#servletContext}`: `ServletContext`对象。

***用法示例***

**locale对象操作：**

```html
<div th:text="${#locale.getLanguage() + '_' + #locale.getCountry()}"></div>
```

**session对象操作：**

```html
<div th:text="${session.foo}?:('zoo')"></div>
<div th:text="${session.size()}"></div>
<div th:text="${session.isEmpty()}"></div>
<div th:text="${session.containsKey('foo')}"></div>
```

### 常用工具类

> 官方文档： [Thymeleaf 3.0 工具类](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#appendix-b-expression-utility-objects)

- `#strings`：字符串工具类
- `#lists`：List 工具类
- `#arrays`：数组工具类
- `#sets`：Set 工具类
- `#maps`：常用Map方法。
- `#objects`：一般对象类，通常用来判断非空
- `#bools`：常用的布尔方法。
- `#execInfo`：获取页面模板的处理信息。
- `#messages`：在变量表达式中获取外部消息的方法，与使用＃{...}语法获取的方法相同。
- `#uris`：转义部分URL / URI的方法。
- `#conversions`：用于执行已配置的转换服务的方法。
- `#dates`：时间操作和时间格式化等。
- `#calendars`：用于更复杂时间的格式化。
- `#numbers`：格式化数字对象的方法。
- `#aggregates`：在数组或集合上创建聚合的方法。
- `#ids`：处理可能重复的id属性的方法。

**用法举例：**

**date工具类之日期格式化**
使用默认的日期格式(`toString`方法) 并不是我们预期的格式：`Mon Dec 03 23:16:50 CST 2018`
此时可以通过时间工具类`#dates`来对日期进行格式化：`2018-12-03 23:16:50`

```html
<input type="text" th:value="${#dates.format(user.createTime,'yyyy-MM-dd HH:mm:ss')}"/>
```

**首字母大写**

```html
/*
 * Convert the first character of every word to upper-case
 */
${#strings.capitalizeWords(str)}                    // also array*, list* and set*
```

**list方法**

```html
/*
 * Compute size
 */
${#lists.size(list)}

/*
 * Check whether list is empty
 */
${#lists.isEmpty(list)}
```

### 公共片段

使用方法：首先通过`th:fragment`定制片段 ，然后通过`th:replace` 填写片段路径和片段名。例如：
我们通常将项目里面经常重用的代码抽取为代码片段(标签)

```html
<!-- /views/common/head.html-->
<head th:fragment="static(version)">
        <script th:src="@{/webjars/jquery/${version}/jquery.js}"></script>
</head>
```

然后在不同的页面引用该片段，达到代码重用的目的

```html
<!-- /views/your.html -->
<div th:replace="~{common/head::static(1.12.4)}"></div>
```

在实际使用中，我们往往使用更简洁的表达，去掉表达式外壳直接填写片段名。例如：

```html
<!-- your.html -->
<div th:replace="common/head::static"></div>
<div th:insert="common/head::static"></div>
<div th:include="common/head::static"></div>
```

关于thymeleaf th:replace th:include th:insert 的区别

- th:insert ：保留自己的主标签，保留th:fragment的主标签。
- th:replace ：不要自己的主标签，保留th:fragment的主标签。
- th:include ：保留自己的主标签，不要th:fragment的主标签。（官方3.0后不推荐）

> 值得注意的是，使用替换路径`th:replace` 开头请勿添加斜杠`/`，避免部署运行的时候出现路径报错。（因为默认拼接的路径为`spring.thymeleaf.prefix = classpath:/templates/`）

片段表达式是Thymeleaf的特色之一，细粒度可以达到标签级别，这是JSP无法做到的。
片段表达式拥有三种语法：

- `~{ viewName } 表示引入完整页面`
- `~{ viewName ::selector} 表示在指定页面寻找片段 其中selector可为片段名、jquery选择器等`,即可以在一个html页面内定义多个片段.
- `~{ ::selector} 表示在当前页寻找`

### 内联语法

thymeleaf内联js中使用的标准格式为：`[[${xx}]]` ，可以读取服务端变量，也可以调用内置对象的方法。例如获取用户变量和应用路径：

```html
    <script th:inline="javascript">
        var user = [[${user}]];
        var APP_PATH = [[${#request.getContextPath()}]];
        var LANG_COUNTRY = [[${#locale.getLanguage()+'_'+#locale.getCountry()}]];
    </script>
```

标签（代码片段）内引入的JS里面能使用内联表达式吗？

答：不能！内联表达式仅在页面生效，因为`Thymeleaf`只负责解析一级视图，不能识别外部标签JS里面的表达式。



# 页面国际化（i18n）

有的时候，我们的网站会去涉及中英文甚至多语言的切换，这时候就需要学习国际化.

## 准备工作

先在IDEA中统一设置properties的编码问题！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248221537-1c8bb0a5-e49a-447c-805f-d353c3cde728.png)



编写国际化配置文件，抽取页面需要显示的国际化页面消息。我们可以去登录页面查看一下，哪些内容我们需要编写国际化的配置！

## 配置文件编写

1、我们在resources资源文件下新建一个i18n目录，存放国际化配置文件

2、建立一个login.properties文件，还有一个login_zh_CN.properties；发现IDEA自动识别了我们要做国际化操作；文件夹变了！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248244952-2d0b8f79-00b8-45e7-8072-6715a1c386a8.png)

3、我们可以在这上面去新建一个文件；

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248250465-c7d82ea7-c746-4d12-a793-370fa606f4dd.png)



弹出如下页面：我们再添加一个英文的；

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248263465-1434ff99-3dc5-4b7f-a5a8-73f13b06f707.png)

这样就快捷多了！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248277232-c68ccc8b-02ad-4899-9936-4c2b7a269b0e.png)

4、接下来，我们就来编写配置，我们可以看到idea下面有另外一个视图；

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248285565-e7f73204-542d-4d4b-8903-ff02dd2ce54b.png)

这个视图我们点击 + 号就可以直接添加属性了；我们新建一个login.tip，可以看到边上有三个文件框可以输入

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248295700-5f37742e-efbc-4a02-b8c8-872888c575b6.png)

我们添加一下首页的内容！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248308080-a1cc2f55-a31a-4ed4-8a18-9a7cc1324e36.png)

然后依次添加其他页面内容即可！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248316022-9bbde89c-32c3-4736-930c-aecabd902937.png)

然后去查看我们的配置文件；

login.properties ：默认

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248334232-b299c8ab-398f-40d6-8bb3-1e8316293b47.png)

英文：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248346919-1defb4dd-af59-44f8-8624-84d908d36006.png)

中文：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248351024-f198106a-657e-43ac-9cfd-d49c75fe86db.png)



OK，配置文件步骤搞定！

## 配置文件生效探究

我们去看一下SpringBoot对国际化的自动配置！这里又涉及到一个类：MessageSourceAutoConfiguration

里面有一个方法，这里发现SpringBoot已经自动配置好了管理我们国际化资源文件的组件 ResourceBundleMessageSource；

```java
// 获取 properties 传递过来的值进行判断
@Bean
public MessageSource messageSource(MessageSourceProperties properties) {
    ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
    if (StringUtils.hasText(properties.getBasename())) {
        // 设置国际化文件的基础名（去掉语言国家代码的）
        messageSource.setBasenames(
            StringUtils.commaDelimitedListToStringArray(
                                       StringUtils.trimAllWhitespace(properties.getBasename())));
    }
    if (properties.getEncoding() != null) {
        messageSource.setDefaultEncoding(properties.getEncoding().name());
    }
    messageSource.setFallbackToSystemLocale(properties.isFallbackToSystemLocale());
    Duration cacheDuration = properties.getCacheDuration();
    if (cacheDuration != null) {
        messageSource.setCacheMillis(cacheDuration.toMillis());
    }
    messageSource.setAlwaysUseMessageFormat(properties.isAlwaysUseMessageFormat());
    messageSource.setUseCodeAsDefaultMessage(properties.isUseCodeAsDefaultMessage());
    return messageSource;
}
```

我们真实 的情况是放在了i18n目录下，所以我们要去配置这个messages的路径；

```java
spring.messages.basename=i18n.login
```

## 配置页面国际化值

去页面获取国际化的值，查看Thymeleaf的文档，找到message取值操作为：#{…}。我们去页面测试下：

IDEA还有提示，非常智能的！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248420761-dfd8cebf-332a-4a11-b88b-7258733b2edb.png)

我们可以去启动项目，访问一下，发现已经自动识别为中文的了！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248434363-0f84951b-453d-47a9-b883-c7e8a1123a2b.png)

但是我们想要更好！可以根据按钮自动切换中文英文！

## 配置国际化解析

在Spring中有一个国际化的Locale （区域信息对象）；里面有一个叫做LocaleResolver （获取区域信息对象）的解析器！

我们去我们webmvc自动配置文件，寻找一下！看到SpringBoot默认配置：

```java
@Bean
@ConditionalOnMissingBean
@ConditionalOnProperty(prefix = "spring.mvc", name = "locale")
public LocaleResolver localeResolver() {
    // 容器中没有就自己配，有的话就用用户配置的
    if (this.mvcProperties.getLocaleResolver() == WebMvcProperties.LocaleResolver.FIXED) {
        return new FixedLocaleResolver(this.mvcProperties.getLocale());
    }
    // 接收头国际化分解
    AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
    localeResolver.setDefaultLocale(this.mvcProperties.getLocale());
    return localeResolver;
}
```

AcceptHeaderLocaleResolver 这个类中有一个方法

```java
public Locale resolveLocale(HttpServletRequest request) {
    Locale defaultLocale = this.getDefaultLocale();
    // 默认的就是根据请求头带来的区域信息获取Locale进行国际化
    if (defaultLocale != null && request.getHeader("Accept-Language") == null) {
        return defaultLocale;
    } else {
        Locale requestLocale = request.getLocale();
        List<Locale> supportedLocales = this.getSupportedLocales();
        if (!supportedLocales.isEmpty() && !supportedLocales.contains(requestLocale)) {
            Locale supportedLocale = this.findSupportedLocale(request, supportedLocales);
            if (supportedLocale != null) {
                return supportedLocale;
            } else {
                return defaultLocale != null ? defaultLocale : requestLocale;
            }
        } else {
            return requestLocale;
        }
    }
}
```

那假如我们现在想点击链接让我们的国际化资源生效，就需要让我们自己的Locale生效！

我们去自己写一个自己的LocaleResolver，可以在链接上携带区域信息！

修改一下前端页面的跳转连接：

```html
<!-- 这里传入参数不需要使用 ？使用 （key=value）-->
<a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>
<a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
```

我们去写一个处理的组件类！

```java
//可以在链接上携带区域信息
public class MyLocaleResolver implements LocaleResolver {
    //解析请求
    @Override
    public Locale resolveLocale(HttpServletRequest request) {
        String language = request.getParameter("l");
        Locale locale = Locale.getDefault(); // 如果没有获取到就使用系统默认的
        //如果请求链接不为空
        if (!StringUtils.isEmpty(language)){
            //分割请求参数
            String[] split = language.split("_");
            //国家，地区
            locale = new Locale(split[0],split[1]);
        }
        return locale;
    }
    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {
    }
}
```

为了让我们的区域化信息能够生效，我们需要再配置一下这个组件！在我们自己的MvcConofig下添加bean；

```java
@Bean
public LocaleResolver localeResolver(){
    return new MyLocaleResolver();
}
```

我们重启项目，来访问一下，发现点击按钮可以实现成功切换！搞定收工！

**小结：**

页面国际化

- 配置i18n文件

- 如果需要在项目中进行按钮自动切换，我们需要自定义一个组件LocaleResolver

- 将自己写的组件配置到spring容器中----@Bean

# 异步、定时、邮件任务



## 异步任务

相关方法添加@sync注解后，SpringBoot就会自己开一个线程池，进行调用！但是要让这个注解生效，我们还需要在主程序上添加一个注解@EnableAsync ，开启异步注解功能；

> 注意：@Async所修饰的函数不要定义为static类型，这样异步调用不会生效。

```java
@EnableAsync //开启异步注解功能
@SpringBootApplication
public class SpringbootTaskApplication {
   public static void main(String[] args) {
       SpringApplication.run(SpringbootTaskApplication.class, args);
  }
}
```

**异步回调**

我们需要使用 `Future` 来返回 **异步调用** 的 **结果**。

- 创建 `AsyncCallBackTask` 类，声明 `doTaskOneCallback()`，`doTaskTwoCallback()`，`doTaskThreeCallback()` 三个方法，对原有的三个方法进行包装。

```java
@Component
public class AsyncCallBackTask extends AbstractTask {
    @Async
    public Future<String> doTaskOneCallback() throws Exception {
        super.doTaskOne();
        return new AsyncResult<>("任务一完成");
    }

    @Async
    public Future<String> doTaskTwoCallback() throws Exception {
        super.doTaskTwo();
        return new AsyncResult<>("任务二完成");
    }

    @Async
    public Future<String> doTaskThreeCallback() throws Exception {
        super.doTaskThree();
        return new AsyncResult<>("任务三完成");
    }
}
```

循环调用 `Future` 的 `isDone()` 方法等待三个 **并发任务** 执行完成，记录最终执行时间。

**异步线程池**

创建一个 **线程池配置类**`TaskConfiguration` ，并配置一个 **任务线程池对象**`taskExecutor`。

```java
@Configuration
public class TaskConfiguration {
    @Bean("taskExecutor")
    public Executor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(10);
        executor.setMaxPoolSize(20);
        executor.setQueueCapacity(200);
        executor.setKeepAliveSeconds(60);
        executor.setThreadNamePrefix("taskExecutor-");
        executor.setRejectedExecutionHandler(new CallerRunsPolicy());
        //优雅的遇到错误关闭线程池
        executor.setWaitForTasksToCompleteOnShutdown(true);
        executor.setAwaitTerminationSeconds(60);
        return executor;
    }
}
```

上面我们通过使用 `ThreadPoolTaskExecutor` 创建了一个 **线程池**，同时设置了以下这些参数：

| 线程池属性                 | 属性的作用                                                   | 设置初始值       |
| :------------------------- | :----------------------------------------------------------- | :--------------- |
| 核心线程数                 | 线程池创建时候初始化的线程数                                 | 10               |
| 最大线程数                 | 线程池最大的线程数，只有在缓冲队列满了之后，才会申请超过核心线程数的线程 | 20               |
| 缓冲队列                   | 用来缓冲执行任务的队列                                       | 200              |
| 允许线程的空闲时间         | 当超过了核心线程之外的线程，在空闲时间到达之后会被销毁       | 60秒             |
| 线程池名的前缀             | 可以用于定位处理任务所在的线程池                             | taskExecutor-    |
| 线程池对拒绝任务的处理策略 | 这里采用CallerRunsPolicy策略，当线程池没有处理能力的时候，该策略会直接在execute方法的调用线程中运行被拒绝的任务；如果执行程序已关闭，则会丢弃该任务 | CallerRunsPolicy |

- 创建 `AsyncExecutorTask`类，三个任务的配置和 `AsyncTask` 一样，不同的是 `@Async` 注解需要指定前面配置的 **线程池的名称**`taskExecutor`。

```java
@Component
public class AsyncExecutorTask extends AbstractTask {
    @Async("taskExecutor")
    public Future<String> doTaskOneCallback() throws Exception {
        super.doTaskOne();
        System.out.println("任务一，当前线程：" + Thread.currentThread().getName());
        return new AsyncResult<>("任务一完成");
    }

    @Async("taskExecutor")
    public Future<String> doTaskTwoCallback() throws Exception {
        super.doTaskTwo();
        System.out.println("任务二，当前线程：" + Thread.currentThread().getName());
        return new AsyncResult<>("任务二完成");
    }

    @Async("taskExecutor")
    public Future<String> doTaskThreeCallback() throws Exception {
        super.doTaskThree();
        System.out.println("任务三，当前线程：" + Thread.currentThread().getName());
        return new AsyncResult<>("任务三完成");
    }
}
```



## 定时任务

项目开发中经常需要执行一些定时任务，比如需要在每天凌晨的时候，分析一次前一天的日志信息，Spring为我们提供了异步执行任务调度的方式，提供了两个接口。

- TaskExecutor接口
- TaskScheduler接口

两个注解：

- @EnableScheduling
- @Scheduled

**cron表达式：**

corn从左到右（用空格隔开）：秒 分 小时 月份中的日期 月份 星期中的日期 年份

| 字段                     | 允许值                                 | 允许的特殊字符           |
| ------------------------ | -------------------------------------- | ------------------------ |
| 秒（Seconds）            | 0~59的整数                             | , - * / 四个字符         |
| 分（*Minutes*）          | 0~59的整数                             | , - * / 四个字符         |
| 小时（*Hours*）          | 0~23的整数                             | , - * / 四个字符         |
| 日期（*DayofMonth*）     | 1~31的整数（但是你需要考虑你月的天数） | ,- * ? / L W C 八个字符  |
| 月份（*Month*）          | 1~12的整数或者 JAN-DEC                 | , - * / 四个字符         |
| 星期（*DayofWeek*）      | 1~7的整数或者 SUN-SAT （1=SUN）        | , - * ? / L C # 八个字符 |
| 年(可选，留空)（*Year*） | 1970~2099                              | , - * / 四个字符         |

| 特殊字符 | 代表含义                                                     |
| -------- | ------------------------------------------------------------ |
| ,        | 枚举 在Minutes域使用5,20，则意味着在5和20分每分钟触发一次。  |
| -        | 表示范围 例如在Minutes域使用5-20，表示从5分到20分钟每分钟触发一次 |
| *        | 任意                                                         |
| /        | 表示起始时间开始触发，然后每隔固定时间触发一次，             |
| ?        | 日/星期冲突匹配 只能用在DayofMonth和DayofWeek两个域          |
| L        | 最后 如果在DayofWeek域使用5L,意味着在最后的一个星期四触发。  |
| W        | 工作日                                                       |
| C        | 和calendar联系后计算过的值                                   |
| #        | 用于确定每个月第几个星期几，4#2 某月的第二个星期三           |

**测试步骤：-**

1、创建一个ScheduledService

我们里面存在一个hello方法，他需要定时执行，怎么处理呢？

```java
@Service
public class ScheduledService {
 
   //秒   分   时     日   月   周几
   //0 * * * * MON-FRI
   //注意cron表达式的用法；
   @Scheduled(cron = "0 * * * * 0-7")
   public void hello(){
       System.out.println("hello.....");
  }
}
```

2、这里写完定时任务之后，我们需要在主程序上增加@EnableScheduling 开启定时任务功能

```java
@EnableAsync //开启异步注解功能
@EnableScheduling //开启基于注解的定时任务
@SpringBootApplication
public class SpringbootTaskApplication {
   public static void main(String[] args) {
       SpringApplication.run(SpringbootTaskApplication.class, args);
  }
}
```

3、我们来详细了解下cron表达式；

http://www.bejson.com/othertools/cron/

4、常用的表达式

```java
（1）0/2 * * * * ?   表示每2秒 执行任务
（1）0 0/2 * * * ?   表示每2分钟 执行任务
（1）0 0 2 1 * ?   表示在每月的1日的凌晨2点调整任务
（2）0 15 10 ? * MON-FRI   表示周一到周五每天上午10:15执行作业
（3）0 15 10 ? 6L 2002-2006   表示2002-2006年的每个月的最后一个星期五上午10:15执行作
（4）0 0 10,14,16 * * ?   每天上午10点，下午2点，4点
（5）0 0/30 9-17 * * ?   朝九晚五工作时间内每半小时
（6）0 0 12 ? * WED   表示每个星期三中午12点
（7）0 0 12 * * ?   每天中午12点触发
（8）0 15 10 ? * *   每天上午10:15触发
（9）0 15 10 * * ?     每天上午10:15触发
（10）0 15 10 * * ?   每天上午10:15触发
（11）0 15 10 * * ? 2005   2005年的每天上午10:15触发
（12）0 * 14 * * ?     在每天下午2点到下午2:59期间的每1分钟触发
（13）0 0/5 14 * * ?   在每天下午2点到下午2:55期间的每5分钟触发
（14）0 0/5 14,18 * * ?     在每天下午2点到2:55期间和下午6点到6:55期间的每5分钟触发
（15）0 0-5 14 * * ?   在每天下午2点到下午2:05期间的每1分钟触发
（16）0 10,44 14 ? 3 WED   每年三月的星期三的下午2:10和2:44触发
（17）0 15 10 ? * MON-FRI   周一至周五的上午10:15触发
（18）0 15 10 15 * ?   每月15日上午10:15触发
（19）0 15 10 L * ?   每月最后一日的上午10:15触发
（20）0 15 10 ? * 6L   每月的最后一个星期五上午10:15触发
（21）0 15 10 ? * 6L 2002-2005   2002年至2005年的每月的最后一个星期五上午10:15触发
（22）0 15 10 ? * 6#3   每月的第三个星期五上午10:15触发
```

**quartz**

在 springboot2.0 后官方添加了 Quartz 框架的依赖，所以只需要在 pom 文件当中引入

```xml
      <!--引入quartz定时框架-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-quartz</artifactId>
        </dependency>
```

**创建任务类**

由于 springboot2.0 自动进行了依赖所以创建的定时任务类直接继承 QuzrtzJobBean 就可以了，新建一个定时任务类：QuartzSimpleTask

```java
public class QuartzSimpleTask extends QuartzJobBean {
    @Override
    protected void executeInternal(JobExecutionContext jobExecutionContext) throws JobExecutionException {
        System.out.println("quartz简单的定时任务执行时间："+new Date().toLocaleString());
    }
}
```

**创建 Quartz 定时配置类**

将之前创建的定时任务添加到定时调度里面

```java
@Configuration
public class QuartzSimpleConfig {
    //指定具体的定时任务类
    @Bean
    public JobDetail uploadTaskDetail() {
        return JobBuilder.newJob(QuartzSimpleTask.class).withIdentity("QuartzSimpleTask").storeDurably().build();
    }

    @Bean
    public Trigger uploadTaskTrigger() {
        //这里设定触发执行的方式
        CronScheduleBuilder scheduleBuilder = CronScheduleBuilder.cronSchedule("*/5 * * * * ?");
        // 返回任务触发器
        return TriggerBuilder.newTrigger().forJob(uploadTaskDetail())
                .withIdentity("QuartzSimpleTask")
                .withSchedule(scheduleBuilder)
                .build();
    }
}
```

**动态定时任务**

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/testdb?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
  quartz:
    job-store-type: JDBC #数据库存储quartz任务配置
    jdbc:
      initialize-schema: NEVER #自动初始化表结构，第一次启动的时候这里写always
```

**定时任务类**

```java
@Data
public class QuartzBean {
    /** 任务id */
    private String id;

    /** 任务名称 */
    private String jobName;

    /** 任务执行类 */
    private String jobClass;

    /** 任务状态 启动还是暂停*/
    private Integer status;

    /** 任务运行时间表达式 */
    private String cronExpression;
}
```

**创建定时任务工具类，具有暂停，修改，启动，单次启动功能**

```java
public class QuartzUtils {

    /**
     * 创建定时任务 定时任务创建之后默认启动状态
     * @param scheduler 调度器
     * @param quartzBean 定时任务信息类
     */
    @SuppressWarnings("unchecked")
    public static void createScheduleJob(Scheduler scheduler, QuartzBean quartzBean) throws ClassNotFoundException, SchedulerException {
            //获取到定时任务的执行类 必须是类的绝对路径名称
            //定时任务类需要是job类的具体实现 QuartzJobBean是job的抽象类。
            Class<? extends Job> jobClass = (Class<? extends Job>) Class.forName(quartzBean.getJobClass());
            // 构建定时任务信息
            JobDetail jobDetail = JobBuilder.newJob(jobClass).withIdentity(quartzBean.getJobName()).build();
            // 设置定时任务执行方式
            CronScheduleBuilder scheduleBuilder = CronScheduleBuilder.cronSchedule(quartzBean.getCronExpression());
            // 构建触发器trigger
            CronTrigger trigger = TriggerBuilder.newTrigger().withIdentity(quartzBean.getJobName()).withSchedule(scheduleBuilder).build();
            scheduler.scheduleJob(jobDetail, trigger);
    }

    /**
     * 根据任务名称暂停定时任务
     * @param scheduler 调度器
     * @param jobName 定时任务名称
     */
    public static void pauseScheduleJob(Scheduler scheduler, String jobName) throws SchedulerException {
        JobKey jobKey = JobKey.jobKey(jobName);
        scheduler.pauseJob(jobKey);
    }

    /**
     * 根据任务名称恢复定时任务
     * @param scheduler 调度器
     * @param jobName 定时任务名称
     */
    public static void resumeScheduleJob(Scheduler scheduler, String jobName) throws SchedulerException {
        JobKey jobKey = JobKey.jobKey(jobName);
        scheduler.resumeJob(jobKey);
    }

    /**
     * 根据任务名称立即运行一次定时任务
     * @param scheduler 调度器
     * @param jobName 定时任务名称
     */
    public static void runOnce(Scheduler scheduler, String jobName) throws SchedulerException {
        JobKey jobKey = JobKey.jobKey(jobName);
        scheduler.triggerJob(jobKey);
    }

    /**
     * 更新定时任务
     * @param scheduler 调度器
     * @param quartzBean 定时任务信息类
     */
    public static void updateScheduleJob(Scheduler scheduler, QuartzBean quartzBean) throws SchedulerException {

            //获取到对应任务的触发器
            TriggerKey triggerKey = TriggerKey.triggerKey(quartzBean.getJobName());
            //设置定时任务执行方式
            CronScheduleBuilder scheduleBuilder = CronScheduleBuilder.cronSchedule(quartzBean.getCronExpression());
            //重新构建任务的触发器trigger
            CronTrigger trigger = (CronTrigger) scheduler.getTrigger(triggerKey);
            trigger = trigger.getTriggerBuilder().withIdentity(triggerKey).withSchedule(scheduleBuilder).build();
            //重置对应的job
            scheduler.rescheduleJob(triggerKey, trigger);
    }

    /**
     * 根据定时任务名称从调度器当中删除定时任务
     * @param scheduler 调度器
     * @param jobName 定时任务名称
     */
    public static void deleteScheduleJob(Scheduler scheduler, String jobName) throws SchedulerException {
        JobKey jobKey = JobKey.jobKey(jobName);
        scheduler.deleteJob(jobKey);
    }
}
@Controller
@RequestMapping("/quartz/job/")
public class QuartzController {
    //注入任务调度
    @Resource
    private Scheduler scheduler;

    @PostMapping("/create")
    @ResponseBody
    public String createJob(@RequestBody QuartzBean quartzBean) throws SchedulerException, ClassNotFoundException {
        QuartzUtils.createScheduleJob(scheduler,quartzBean);
        return "已创建任务";//这里return不是生产级别代码，测试简单写一下
    }

    @PostMapping("/pause")
    @ResponseBody
    public String pauseJob(String jobName) throws SchedulerException {
        QuartzUtils.pauseScheduleJob (scheduler,jobName);
        return "已暂停成功";//这里return不是生产级别代码，测试简单写一下
    }

    @PostMapping("/run")
    @ResponseBody
    public String runOnce(String jobName) throws SchedulerException {
        QuartzUtils.runOnce (scheduler,jobName);
        return "运行任务" + jobName + "成功";//这里return不是生产级别代码，测试简单写一下
    }

    @PostMapping("/resume")
    @ResponseBody
    public String resume(String jobName) throws SchedulerException {
        QuartzUtils.resumeScheduleJob(scheduler,jobName);
        return "恢复定时任务成功：" + jobName;
    }

    @PostMapping("/update")
    @ResponseBody
    public String update(@RequestBody QuartzBean quartzBean) throws SchedulerException {
        QuartzUtils.updateScheduleJob(scheduler,quartzBean);
        return "更新定时任务调度信息成功";
    }
}
```



## 邮件任务

邮件发送，在我们的日常开发中，也非常的多，Springboot也帮我们做了支持

- 邮件发送需要引入spring-boot-start-mail
- SpringBoot 自动配置MailSenderAutoConfiguration

- 定义MailProperties内容，配置在application.yml中
- 自动装配JavaMailSender

- 测试邮件发送

**测试：**

1、引入pom依赖

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

看它引入的依赖，可以看到 jakarta.mail

```xml
<dependency>
   <groupId>com.sun.mail</groupId>
   <artifactId>jakarta.mail</artifactId>
   <version>1.6.4</version>
   <scope>compile</scope>
</dependency>
```

2、查看自动配置类：MailSenderAutoConfiguration

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615375803662-63532564-5ff8-4af5-9572-ed0fe890da3a.png)

这个类中存在bean，JavaMailSenderImpl

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615375822928-e17de5a3-567a-4ac8-bf24-ec1dfa8469bc.png)

然后我们去看下配置文件

```java
@ConfigurationProperties(
   prefix = "spring.mail"
)
public class MailProperties {
   private static final Charset DEFAULT_CHARSET = StandardCharsets.UTF_8;
   private String host;
   private Integer port;
   private String username;
   private String password;
   private String protocol = "smtp";
   private Charset defaultEncoding;
   private Map<String, String> properties;
   private String jndiName;
}
```

3、配置文件：

```properties
spring.mail.username=24736743@qq.com
spring.mail.password=你的qq授权码
spring.mail.host=smtp.qq.com
# qq需要配置ssl
spring.mail.properties.mail.smtp.ssl.enable=true
```

获取授权码：在QQ邮箱中的设置->账户->开启pop3和smtp服务

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615375906529-88757cde-4ef5-46c5-b800-e597553b1794.png)

4、Spring单元测试

```java
@Autowired
JavaMailSenderImpl mailSender;

@Test
public void contextLoads() {
   //邮件设置1：一个简单的邮件
   SimpleMailMessage message = new SimpleMailMessage();
   message.setSubject("通知-明天来狂神这听课");
   message.setText("今晚7:30开会");

   message.setTo("24736743@qq.com");
   message.setFrom("24736743@qq.com");
   mailSender.send(message);
}

@Test
public void contextLoads2() throws MessagingException {
   //邮件设置2：一个复杂的邮件
   MimeMessage mimeMessage = mailSender.createMimeMessage();
   MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true);

   helper.setSubject("通知-明天来狂神这听课");
   helper.setText("<b style='color:red'>今天 7:30来开会</b>",true);

   //发送附件
   helper.addAttachment("1.jpg",new File(""));
   helper.addAttachment("2.jpg",new File(""));

   helper.setTo("24736743@qq.com");
   helper.setFrom("24736743@qq.com");

   mailSender.send(mimeMessage);
}
```

查看邮箱，邮件接收成功！

我们只需要使用Thymeleaf进行前后端结合即可开发自己网站邮件收发功能了！

封装工具类：

```java
/**
     * 
     * @param html:   是否开启html
     * @param subject 邮件标题
     * @param text   邮件内容
     * @throws Exception
     */
@Test
public void sendMail(Boolean html, String subject, String text) throws Exception {
    //邮件设置2：一个复杂的邮件
    MimeMessage mimeMessage = mailSender.createMimeMessage();
    MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, html);

    helper.setSubject(subject);
    helper.setText(text,true);

    helper.setTo("24736743@qq.com");
    helper.setFrom("24736743@qq.com");

    mailSender.send(mimeMessage);
}
```

# 服务器推送

1、WebSocket：全双工双向通信，本质上是一个额外的tcp连接，建立和关闭时握手使用http协议，其他数据传输不使用http协议 ，更加复杂一些。

2、SSE：用来从服务端实时推送数据到浏览器端， 直接建立在当前http连接上，本质上是保持一个http长连接，轻量协议 。



|           | 是否基于新协议 | 是否双向通信     | 是否支持跨域           | 编码难度 |
| :-------- | :------------- | :--------------- | :--------------------- | :------- |
| SSE       | 否（`Http`）   | 否（服务器单向） | 否（Firefox 支持跨域） | 低       |
| WebSocket | 是（`ws`）     | 是               | 是                     | 略高     |

## **SSE**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>SSE</title>
</head>
<body>
    <div id = "message"></div>
    <script>
        if (window.EventSource) {
            var source = new EventSource('orderpay');
            innerHTML = '';
            source.addEventListener('message', function(e) {
                innerHTML += e.data + "<br/>";
                document.getElementById("message").innerHTML = innerHTML;
            });

            source.addEventListener('open', function(e) {
                console.log("连接打开.");
            }, false);

            // 响应finish事件，主动关闭EventSource
            source.addEventListener('finish', function(e) {
                console.log("数据接收完毕，关闭EventSource");
                source.close();
                console.log(e);
            }, false);

            source.addEventListener('error', function(e) {
                if (e.readyState == EventSource.CLOSED) {
                    console.log("连接关闭");
                } else {
                    console.log(e);
                }
            }, false);
        } else {
            console.log("你的浏览器不支持SSE");
        }
    </script>

</body>
</html>
```

```java
@Controller
@RequestMapping("sse")
public class SSEControler {

    public static final ConcurrentHashMap<Long,SseEmitter> sseEmitters = new ConcurrentHashMap<>();


    @GetMapping("/test")
    public String ssetest(){
        return "ssetest";
    }

    @GetMapping("/orderpay")
    public SseEmitter orderpay(){
        Long payRecordId = 1L;
        //设置默认的超时时间60秒
        final SseEmitter emitter = new SseEmitter(60 * 1000L);
        try {
            System.out.println("连接建立成功");
            //TODO 这里可以做一些订单保存的操作
            sseEmitters.put(payRecordId,emitter);
        }catch (Exception e){
            emitter.completeWithError(e);
        }

        return emitter;
    }


    @GetMapping("/payback")
    public @ResponseBody String payback (){

        SseEmitter emitter = sseEmitters.get(1L);
        try {
            emitter.send("支付成功");

            System.out.println("发送finish事件");
            emitter.send(SseEmitter.event().name("finish").id("6666").data("哈哈"));
            System.out.println("调用complete");
            emitter.complete();
        } catch (IOException e) {
            emitter.completeWithError(e);
        }
        return "ok";
    }
}
```

## **websocket**

```xml
<!-- 引入websocket依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
```

```java
@Configuration
public class WebSocketConfig {
 
    @Bean
    public ServerEndpointExporter serverEndpointExporter() {
        return new ServerEndpointExporter();
    }
}
```


WebSocketServer本节内容的核心代码，websocket服务端代码

1. @ServerEndpoint(value = "/ws/asset")表示websocket的接口服务地址
2. @OnOpen注解的方法，为连接建立成功时调用的方法
3. @OnClose注解的方法，为连接关闭调用的方法
4. @OnMessage注解的方法，为收到客户端消息后调用的方法
5. @OnError注解的方法，为出现异常时调用的方法

```java
/**
 * WebSocket服务端示例
 */
@Component
@Slf4j
@ServerEndpoint(value = "/ws/asset")
public class WebSocketServer {  
  
    private static final AtomicInteger OnlineCount = new AtomicInteger(0);
    // concurrent包的线程安全Set，用来存放每个客户端对应的Session对象。  
    private static CopyOnWriteArraySet<Session> SessionSet = new CopyOnWriteArraySet<>();
  
  
    /** 
     * 连接建立成功调用的方法 
     */  
    @OnOpen
    public void onOpen(Session session) throws IOException {
        SessionSet.add(session);   
        int cnt = OnlineCount.incrementAndGet(); // 在线数加1  
        log.info("有连接加入，当前连接数为：{}", cnt);  
        SendMessage(session, "连接成功");  
    }  
  
    /** 
     * 连接关闭调用的方法 
     */  
    @OnClose
    public void onClose(Session session) {  
        SessionSet.remove(session);  
        int cnt = OnlineCount.decrementAndGet();  
        log.info("有连接关闭，当前连接数为：{}", cnt);  
    }  
  
    /** 
     * 收到客户端消息后调用的方法
     * @param message 客户端发送过来的消息
     */  
    @OnMessage
    public void onMessage(String message, Session session) throws IOException {
        log.info("来自客户端的消息：{}",message);  
        SendMessage(session, "收到消息，消息内容："+message);
    }  
  
    /** 
     * 出现错误
     */  
    public void onError(Session session, Throwable error) {  
        log.error("发生错误：{}，Session ID： {}",error.getMessage(),session.getId());
    }  
  
    /** 
     * 发送消息，实践表明，每次浏览器刷新，session会发生变化。 
     * @param session  session
     * @param message  消息
     */  
    private static void SendMessage(Session session, String message) throws IOException {

        session.getBasicRemote().sendText(String.format("%s (From Server，Session ID=%s)",message,session.getId()));

    }  
  
    /** 
     * 群发消息 
     * @param message  消息
     */  
    public static void BroadCastInfo(String message) throws IOException {
        for (Session session : SessionSet) {  
            if(session.isOpen()){  
                SendMessage(session, message);  
            }  
        }  
    }  
  
    /** 
     * 指定Session发送消息
     * @param sessionId sessionId
     * @param message  消息
     */  
    public static void SendMessage(String sessionId,String message) throws IOException {  
        Session session = null;  
        for (Session s : SessionSet) {  
            if(s.getId().equals(sessionId)){  
                session = s;  
                break;  
            }  
        }  
        if(session!=null){  
            SendMessage(session, message);  
        } else{
            log.warn("没有找到你指定ID的会话：{}",sessionId);  
        }  
    }  
      
} 
```

客户端代码，做几次实验，自然明了代码的意思，先不要看代码，先看效果。
public/wstest/html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>websocket测试</title>
    <style type="text/css">
        h3,h4{
            text-align:center;
        }
    </style>
</head>
<body>

<h3>WebSocket测试，在<span style="color:red">控制台</span>查看测试信息输出！</h3>
<h4>
    <br>
    http://localhost:8888/api/ws/sendOne?message=单发消息内容&id=none
    <br>
    http://localhost:8888/api/ws/sendAll?message=群发消息内容
</h4>

<h3>请输入要发送给服务器端的消息：</h3><br/>

<input id="text" type="text" />
<button onclick="sendToServer()">发送服务器消息</button>
<button onclick="closeWebSocket()">关闭连接</button>
<br>信息:<span id="message"></span>


<script type="text/javascript">
    var socket;
    if (typeof (WebSocket) == "undefined") {
        console.log("遗憾：您的浏览器不支持WebSocket");
    } else {
        socket = new WebSocket("ws://localhost:8888/ws/asset");
        //连接打开事件
        socket.onopen = function() {
            console.log("Socket 已打开");
            socket.send("消息发送测试(From Client)");
        };
        //收到消息事件
        socket.onmessage = function(msg) {
            document.getElementById('message').innerHTML += msg.data + '<br/>';
        };
        //连接关闭事件
        socket.onclose = function() {
            console.log("Socket已关闭");
        };
        //发生了错误事件
        socket.onerror = function() {
            alert("Socket发生了错误");
        }

        //窗口关闭时，关闭连接
        window.unload=function() {
            socket.close();
        };
    }

    //关闭连接
    function closeWebSocket(){
        socket.close();
    }

    //发送消息给服务器
    function sendToServer(){
        var message = document.getElementById('text').value;
        socket.send(message);
    }
</script>

</body>
</html>
```

测试页面

http://localhost:8888/wstest.html

**服务端广播与指定session消息发送**

```java
@RestController
@RequestMapping("/api/ws")
public class WebSocketController {  
  

    /** 
     * 群发消息内容 
     * @param message 消息内容
     */
    @RequestMapping(value="/sendAll", method=RequestMethod.GET)
    AjaxResponse sendAllMessage(@RequestParam String message){
        try {  
            WebSocketServer.BroadCastInfo(message);
        } catch (IOException e) {
            throw new CustomException(CustomExceptionType.SYSTEM_ERROR,"群发消息失败");
        }  
        return AjaxResponse.success();
    }  

    /** 
     * 指定会话ID发消息 
     * @param message 消息内容 
     * @param id 连接会话ID
     */
    @RequestMapping(value="/sendOne", method=RequestMethod.GET)
    AjaxResponse sendOneMessage(@RequestParam String message,@RequestParam String id){
        try {  
            WebSocketServer.SendMessage(id,message);  
        } catch (IOException e) {
            throw new CustomException(CustomExceptionType.SYSTEM_ERROR,"指定会话ID发消息失败");
        }  
        return AjaxResponse.success();
    }  
}  
```

# Spring事务

**事务的具体定义**

事务提供一种机制将一个活动涉及的所有操作纳入到一个不可分割的执行单元，组成事务的所有操作只有在所有操作均能正常执行的情况下方能提交，只要其中任一操作执行失败，都将导致整个事务的回滚。
简单地说，事务提供一种“要么什么都不做，要么做全套（All or Nothing）”机制。ACID就是对这句话的一个解释。

**并发环境下的数据库事务**

**事务并发执行会出现的问题**

我们先来看一下事务并发，数据库可能会出现更新丢失、脏读、不可重复读、幻读等。

数据库一共有如下四种隔离级别：

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200425125418.png)

- 隔离级别越高，越能保证数据的完整性和一致性，但是对并发性能的影响也越大。

**事务传播行为**

事务传播行为用来描述由某一个事务传播行为修饰的方法被嵌套进另一个方法的时事务如何传播。

用伪代码说明：

```
ServiceA {
           
         void methodA() {
             ServiceB.methodB();
         }

}
      
ServiceB {
           
         void methodB() {
         }
           
}
```

代码中`methodA()`方法嵌套调用了`methodB()`方法，`methodB()`的事务传播行为由`@Transaction(Propagation=XXX)`设置决定。

**Spring中七种事务传播行为**

| 事务传播行为类型          | 说明                                                         |
| :------------------------ | :----------------------------------------------------------- |
| PROPAGATION_REQUIRED      | 如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。这是最常见的选择。 |
| PROPAGATION_SUPPORTS      | 支持当前事务，如果当前没有事务，就以非事务方式执行。         |
| PROPAGATION_MANDATORY     | 使用当前的事务，如果当前没有事务，就抛出异常。               |
| PROPAGATION_REQUIRES_NEW  | 新建事务，如果当前存在事务，把当前事务挂起。                 |
| PROPAGATION_NOT_SUPPORTED | 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。   |
| PROPAGATION_NEVER         | 以非事务方式执行，如果当前存在事务，则抛出异常。             |
| PROPAGATION_NESTED        | 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED类似的操作。 |

**@Transactional 注解**

| 属性名           | 说明                                                         |
| :--------------- | :----------------------------------------------------------- |
| value            | 当在配置文件中有多个 TransactionManager , 可以用该属性指定选择哪个事务管理器。 |
| propagation      | 事务的传播行为，默认值为 REQUIRED。                          |
| isolation        | 事务的隔离度，默认值采用 DEFAULT。                           |
| timeout          | 事务的超时时间，默认值为-1。如果超过该时间限制但事务还没有完成，则自动回滚事务。 |
| read-only        | 指定事务是否为只读事务，默认值为 false；为了忽略那些不需要事务的方法，比如读取数据，可以设置 read-only 为 true。 |
| rollback-for     | 用于指定能够触发事务回滚的异常类型，如果有多个异常类型需要指定，各类型之间可以通过逗号分隔。 |
| no-rollback- for | 抛出 no-rollback-for 指定的异常类型，不回滚事务。            |

**spring事务的实现**

spring事务本质上是依赖于数据库事务
![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/83b7902520146de4b8468a6c12210d56_955x538.png)
![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/312679a3bf90c280b9f4fed1f77a8e37_640x369.png)

# Spring Data

## JPA

**Spring Data JPA** 是 Spring 基于 ORM 框架、JPA 规范的基础上封装的一套 JPA 应用框架，底层使用了 Hibernate 的 JPA 技术实现，可使开发者用极简的代码即可实现对数据的访问和操作。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
 <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

修改application.yml，配置好数据库连接和jpa的相关配置

```java
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/testdb?useUnicode=true&characterEncoding=utf-8
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver
  jpa:
    hibernate:
      ddl-auto: update
    database: mysql
    show-sql: true
```

`spring.jpa.properties.hibernate.hbm2ddl.auto`是hibernate的配置属性，其主要作用是：自动根据实体类的定义创建、更新、验证数据库表结构。该参数的几种配置如下：

- `create`：每次加载hibernate时都会删除上一次的生成的表，然后根据你的model类再重新来生成新表，哪怕两次没有任何改变也要这样执行，这就是导致数据库表数据丢失的一个重要原因。
- `create-drop`：每次加载hibernate时根据model类生成表，但是sessionFactory一关闭,表就自动删除。
- `update`：最常用的属性，第一次加载hibernate时根据model类会自动建立起表的结构（前提是先建立好数据库），以后加载hibernate时根据model类自动更新表结构，即使表结构改变了但表中的行仍然存在不会删除以前的行。要注意的是当部署到服务器后，表结构是不会被马上建立起来的，是要等应用第一次运行起来后才会。
- `validate`：每次加载hibernate时，验证创建数据库表结构，只会和数据库中的表进行比较，不会创建新表，但是会插入新值。

### 基本使用

**实体类**

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
@Entity
@Table(name="article")
public class Article {

    @Id
    @GeneratedValue
    private Long id;

    @Column(nullable = false,length = 32)
    private String author;

    @Column(nullable = false, unique = true,length = 32)
    private String title;

    @Column(length = 512)
    private String content;

    private Date createTime;
}
```

- `@Entity` 表示这个类是一个实体类，接受JPA控制管理，对应数据库中的一个表
- `@Table` 指定这个类对应数据库中的表名。如果这个类名和数据库表名符合驼峰及下划线规则，可以省略这个注解。如`FlowType`类名对应表名`flow_type`。
- `@Id` 指定这个字段为表的主键
- `@GeneratedValue(strategy=GenerationType.IDENTITY)` 指定主键的生成方式，一般主键为自增的话，就采用`GenerationType.IDENTITY`的生成方式
- `@Column` 注解针对一个字段，对应表中的一列。`nullable = false`表示数据库字段不能为空, `unique = true`表示数据库字段不能有重复值,`length = 32`表示数据库字段最大程度为32.
- `@Data`、`@AllArgsConstructor`、`@NoArgsConstructor`、`@Builder`都是插件`lombok`的注解，用来帮助我们生成set、get方法、构造函数等实体类的模板代码。

关于更多注解的详细用法，请参考：[# Hibernate Annotations 参考文档](https://docs.jboss.org/hibernate/annotations/3.4/reference/zh_cn/html_single/#entity)

**数据操作接口**

```java
public interface ArticleRepository extends JpaRepository<Article,Long> {
}
```

XxxRepository继承 JpaRepository<T,ID>为我们提供了各种针对单表的数据操作方法：增删改查。

**Service接口**

```java
public interface ArticleRestService {

     ArticleVO saveArticle(ArticleVO article);

     void deleteArticle(Long id);

     void updateArticle(ArticleVO article);

     ArticleVO getArticle(Long id);

     List<ArticleVO> getAll();
}
```

接口实现

```java
@Service
public class ArticleJPARestService implements  ArticleRestService  {

    //将JPA仓库对象注入
    @Resource
    private ArticleRepository articleRepository;

    @Resource
    private Mapper dozerMapper;

    public ArticleVO saveArticle( ArticleVO article) {

        Article articlePO = dozerMapper.map(article,Article.class);
        articleRepository.save(articlePO);    //保存一个对象到数据库，insert

        return  article;
    }

    @Override
    public void deleteArticle(Long id) {
        articleRepository.deleteById(id);   //根据id删除1条数据库记录
    }

    @Override
    public void updateArticle(ArticleVO article) {
        Article articlePO = dozerMapper.map(article,Article.class);
        articleRepository.save(articlePO);   //更新一个对象到数据库，仍然使用save方法
    }

    @Override
    public ArticleVO getArticle(Long id) {
        Optional<Article> article = articleRepository.findById(id);  //根据id查找一条数据
        return dozerMapper.map(article.get(),ArticleVO.class);
    }

    @Override
    public List<ArticleVO> getAll() {
        List<Article> articleLis = articleRepository.findAll();  //查询article表的所有数据
        return DozerUtils.mapList(articleLis,ArticleVO.class);
    }
}
```

注意：虽然新增和修改都是使用的save方法，但是完成的功能是不一样的。当保存的对象有主键id的时候，save方法会根据id更新记录；当保存的对象没有主键id的时候，save方法会向数据库里面insert一条记录。

**关键字查询接口**

除了上文中JpaRepository为我们提供的增删改查的方法。我们还可以自定义方法，非常简单。

```java
    //注意这个方法的名称，jPA会根据方法名自动生成SQL执行
    Article findByAuthor(String author);
```

Web开发其他具体的关键字，使用方法和生产成 SQL 如下表所示

| Keyword           | Sample                                  | JPQL snippet                                                 |
| :---------------- | :-------------------------------------- | :----------------------------------------------------------- |
| And               | findByLastnameAndFirstname              | … where x.lastname = ?1 and x.firstname = ?2                 |
| Or                | findByLastnameOrFirstname               | … where x.lastname = ?1 or x.firstname = ?2                  |
| Is,Equals         | findByFirstnameIs,findByFirstnameEquals | … where x.firstname = ?1                                     |
| Between           | findByStartDateBetween                  | … where x.startDate between ?1 and ?2                        |
| LessThan          | findByAgeLessThan                       | … where x.age < ?1                                           |
| LessThanEqual     | findByAgeLessThanEqual                  | … where x.age ⇐ ?1                                           |
| GreaterThan       | findByAgeGreaterThan                    | … where x.age > ?1                                           |
| GreaterThanEqual  | findByAgeGreaterThanEqual               | … where x.age >= ?1                                          |
| After             | findByStartDateAfter                    | … where x.startDate > ?1                                     |
| Before            | findByStartDateBefore                   | … where x.startDate < ?1                                     |
| IsNull            | findByAgeIsNull                         | … where x.age is null                                        |
| IsNotNull,NotNull | findByAge(Is)NotNull                    | … where x.age not null                                       |
| Like              | findByFirstnameLike                     | … where x.firstname like ?1                                  |
| NotLike           | findByFirstnameNotLike                  | … where x.firstname not like ?1                              |
| StartingWith      | findByFirstnameStartingWith             | … where x.firstname like ?1 (parameter bound with appended %) |
| EndingWith        | findByFirstnameEndingWith               | … where x.firstname like ?1 (parameter bound with prepended %) |
| Containing        | findByFirstnameContaining               | … where x.firstname like ?1 (parameter bound wrapped in %)   |
| OrderBy           | findByAgeOrderByLastnameDesc            | … where x.age = ?1 order by x.lastname desc                  |
| Not               | findByLastnameNot                       | … where x.lastname <> ?1                                     |
| In                | findByAgeIn(Collection ages)            | … where x.age in ?1                                          |
| NotIn             | findByAgeNotIn(Collection age)          | … where x.age not in ?1                                      |
| TRUE              | findByActiveTrue()                      | … where x.active = true                                      |
| FALSE             | findByActiveFalse()                     | … where x.active = false                                     |
| IgnoreCase        | findByFirstnameIgnoreCase               | … where UPPER(x.firstame) = UPPER(?1)                        |

可以看到我们这里没有任何类SQL语句就完成了两个条件查询方法。这就是Spring-data-jpa的一大特性：**通过解析方法名创建查询**。

### 分页、排序

定义一个接口`ArticleRepository`继承`PagingAndSortingRepository`。`PagingAndSortingRepository`接口不仅包含基础的CURD函数，还支持排序、分页的接口函数定义。

```java
public interface ArticleRepository extends PagingAndSortingRepository<Article,Long> {
     //查询article表的所有数据，传入Pageable分页参数，不需要自己写SQL
    Page<Article> findAll(Pageable pageable);
    //根据author字段查询article表数据，传入Pageable分页参数，不需要自己写SQL
    Page<Article> findByAuthor(String author, Pageable pageable);
    //根据author字段和title字段，查询article表数据，传入Pageable分页参数，不需要自己写SQL
    Slice<Article> findByAuthorAndTitle(String author, String title, Pageable pageable);
}
```

**分页**

```java
Pageable pageable = PageRequest.of(0, 10);   //第一页
//Pageable pageable = PageRequest.of(0, 10);  //第二页
//Pageable pageable = PageRequest.of(0, 10);  // 第三页
//数据库操作获取查询结果
Page<Article> articlePage = articleRepository.findAll(pageable);
//将查询结果转换为List
List<Article> articleList = articlePage.getContent();
```

**排序**

Spring Data JPA提供了一个 `Sort`对象，用以提供一种排序机制。让我们看一下排序的方式。

```java
articleRepository.findAll(Sort.by("createTime"));

articleRepository.findAll(Sort.by("author").ascending()
                        .and(Sort.by("createTime").descending()));
```

- 第一个`findAll`方法是按照`createTime`的升序进行排序
- 第一个`findAll`方法是按照author的升序排序，再按照`createTime`的降序进行排序

**分页并排序**

```java
Pageable pageable = PageRequest.of(0, 10,Sort.by("createTime"));
```

### Slice与Page

在`ArticleRepository`我们看到了一个方法返回Slice和另一个方法返回了Page。它们都是Spring Data JPA的数据响应接口，其中 Page 是 Slice的子接口。它们都用于保存和返回数据。

**Slice**

让我们看一下 Slice的一些重要方法。

```java
List <T>  getContent（）; //获取切片的内容

Pageable  getPageable（）; //当前切片的分页信息

boolean  hasContent（）; //是否有查询结果？

boolean  isFirst（）;  //是否是第一个切片

boolean  isLast（）;  //是否是最后一个切片

Pageable nextPageable(); // 下一个切片的分页信息

Pageable previousPageable(); // 上一个切片的分页信息
```

**Page**

Page是Slice的子接口，以下是的一些重要方法。

```java
//总页数
int getTotalPages();

//总数据条数
long getTotalElements();
```

Slice关心是不是存在下一个分片(分页)，不关心总共有多少页，只关心有没有下一页，适合大数据量。

Page比较适合传统应用中的table开发，需要知道总页数和总条数。

### 多数据源

**方式**

1. 将数据源对象作为参数，传递到调用方法内部，这种方式增加额外的编码。
2. 将Repository操作接口分包存放，Spring扫描不同的包，自动注入不同的数据源。这种方式实现简单，也是一种“约定大于配置”思想的典型应用。
3. 使用Spring AOP面向切面编程，然后在持久层接口方法上面加注解，不同的注解使用表示使用不同的数据源。

此处介绍的是第二种。

**修改yaml文件**

```yaml
spring:
  datasource:
    primary:
        jdbc-url: jdbc:mysql://localhost:3306/testdb?useUnicode=true&characterEncoding=utf-8
        username: root
        password: 123456
        driver-class-name: com.mysql.jdbc.Driver
    secondary:
        jdbc-url: jdbc:mysql://localhost:3306/testdb?useUnicode=true&characterEncoding=utf-8
        username: root
        password: 123456
        driver-class-name: com.mysql.jdbc.Driver
  jpa:
    hibernate:
      ddl-auto: update
    database: mysql
    show-sql: true
```

**两组数据持久化接口及实体类,放到不同的package里面**

**创建配置**

```java
@Configuration
public class JPADataSourceConfig {

    @Primary
    @Bean(name = "primaryDataSource")
    @Qualifier("primaryDataSource")
    @ConfigurationProperties(prefix="spring.datasource.primary")    //结合application.yml的配置
    public DataSource primaryDataSource() {
        return DataSourceBuilder.create().build();
    }

  

    @Bean(name = "secondaryDataSource")
    @Qualifier("secondaryDataSource")
    @ConfigurationProperties(prefix="spring.datasource.secondary")   //结合application.yml的配置
    public DataSource secondaryDataSource() {
        return DataSourceBuilder.create().build();
    }
}
```

配置实体扫描以及事务管理,注意看@Primary和带注释的地方

```java
@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(
        entityManagerFactoryRef="entityManagerFactoryPrimary",
        transactionManagerRef="transactionManagerPrimary",
        basePackages= { "club.krislin.bootlaunch.jpa.testdb" }) //设置Repository所在位置
public class JPAPrimaryConfig {

 
    @Resource
    @Qualifier("primaryDataSource")
    private DataSource primaryDataSource;        //primary数据源注入

    @Primary
    @Bean(name = "entityManagerPrimary")        //primary实体管理器
    public EntityManager entityManager(EntityManagerFactoryBuilder builder) {
        return entityManagerFactoryPrimary(builder).getObject().createEntityManager();
    }

  

    @Primary
    @Bean(name = "entityManagerFactoryPrimary")    //primary实体工厂
    public LocalContainerEntityManagerFactoryBean entityManagerFactoryPrimary (EntityManagerFactoryBuilder builder) {

        return builder
                .dataSource(primaryDataSource)
                .properties(getVendorProperties())
                .packages("club.krislin.bootlaunch.jpa.testdb")     //设置实体类所在位置
                .persistenceUnit("primaryPersistenceUnit")
                .build();
    }

  

    @Resource
    private JpaProperties jpaProperties;

    private Map getVendorProperties() {
        return jpaProperties.getHibernateProperties(new HibernateSettings());
    }

  

    @Primary
    @Bean(name = "transactionManagerPrimary")         //primary事务管理器
    public PlatformTransactionManager transactionManagerPrimary(EntityManagerFactoryBuilder builder) {
        return new JpaTransactionManager(entityManagerFactoryPrimary(builder).getObject());
    }
}
```

上面的代码将扫描`club.krislin.bootlaunch.jpa.testdb`下面的实体对象和Repository，并使用primary数据源。同理仿照即可配置另一个。

测试：

# Spring Cache

**常用缓存操作流程**

- **失效**：应用程序先从 cache 取数据，没有得到，则从数据库中取数据，成功后，放到缓存中。
- **命中**：应用程序从 cache 中取数据，取到后返回。
- **更新**：先把数据存到数据库中，成功后，再让缓存**失效或更新**。缓存操作失败，数据库事务回滚。
- **删除**: 先从数据库里面删掉，再从缓存里面删掉。缓存操作失败，数据库事务回滚。

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

添加入口启动类 `@EnableCaching`注解开启 Caching。

`@Cacheable` 通常应用到读取数据的方法上，如查找方法：先从缓存中读取，如果没有再调用方法获取数据，然后把数据查询结果添加到缓存中。如果缓存中查找到数据，被注解的方法将不会执行。

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200429154434.png)

`@CachePut`通常应用于保存和修改方法配置，能够根据方法的请求参数对其结果进行缓存，和 @Cacheable 不同的是，它每次都会触发被注解方法的调用。

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200429154505.png)

`@CachEvict` 通常应用于删除方法配置，能够根据一定的条件对缓存进行清空。可以清除一条或多条缓存。

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200429154630.png)

Caching为组合注解（非常有用），请看下文。

**常用策略**

**只要发生删改查，就把集合类缓存销毁**

对于查询方法：
`@Cacheable(value=“obj”)` 或 `@Cacheable(value=“objList”)`

对于修改和新增方法：

```java
@Caching(evict = {@CacheEvict(cacheNames = "objList",allEntries = true)},
                  put={@CachePut(cacheNames = "obj",key = "#id")})
```

对于删除方法：

```java
@Caching(evict = {@CacheEvict(cacheNames = "objList",allEntries = true),
                  @CacheEvict(cacheNames = "obj",key = "#id")})
```

****

**指定redis缓存**

在 application.yml指定 spring.cache.type=redis。

```properties
spring.cache.type=
spring.cache.redis.cache-null-values=true      # Allow caching null values.
spring.cache.redis.key-prefix=                        # Key prefix.
spring.cache.redis.time-to-live=                      # 缓存到期时间，默认不主动删除永远不到期
spring.cache.redis.use-key-prefix=true            # Whether to use the key prefix when writing to Redis.
```

由于redis缓存设置的到期时间是统一的，没有办法根据缓存名称(value属性)分别设置缓存到期的时间，所以我们进行一个简单的改造。

```java
@Data
@Configuration
@Component
@ConfigurationProperties(prefix = "caching")
public class RedisConfig {

    @Bean
    public RedisTemplate redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate redisTemplate = new RedisTemplate();
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);

        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);

        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);

        //序列化重点在这四行代码
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);

        redisTemplate.afterPropertiesSet();
        return redisTemplate;
    }



    //自定义redisCacheManager
    @Bean
    public RedisCacheManager redisCacheManager(RedisTemplate redisTemplate) {
        RedisCacheWriter redisCacheWriter = RedisCacheWriter.nonLockingRedisCacheWriter(redisTemplate.getConnectionFactory());

        RedisCacheManager redisCacheManager = new RedisCacheManager(redisCacheWriter,
                this.buildRedisCacheConfigurationWithTTL(redisTemplate,RedisCacheConfiguration.defaultCacheConfig().getTtl().getSeconds()),  //默认的redis缓存配置
                this.getRedisCacheConfigurationMap(redisTemplate)); //针对每一个cache做个性化缓存配置

        return  redisCacheManager;
    }

    //配置注入，key是缓存名称，value是缓存有效期
    private Map<String,Long> ttlmap;

    //根据ttlmap的属性装配结果，个性化RedisCacheConfiguration
    private Map<String, RedisCacheConfiguration> getRedisCacheConfigurationMap(RedisTemplate redisTemplate) {
        Map<String, RedisCacheConfiguration> redisCacheConfigurationMap = new HashMap<>();

        for(Map.Entry<String, Long> entry : ttlmap.entrySet()){
            String cacheName = entry.getKey();
            Long ttl = entry.getValue();
            redisCacheConfigurationMap.put(cacheName,this.buildRedisCacheConfigurationWithTTL(redisTemplate,ttl));
        }

        return redisCacheConfigurationMap;
    }


    //让缓存的序列化方式使用redisTemplate.getValueSerializer()，并为每一个缓存分别设置ttl
    private RedisCacheConfiguration buildRedisCacheConfigurationWithTTL(RedisTemplate redisTemplate,Long ttl){
        return  RedisCacheConfiguration.defaultCacheConfig()
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(redisTemplate.getValueSerializer()))
                .entryTtl(Duration.ofSeconds(ttl));
    }

}
```

**自定义配置实现缓存失效时间个性化**

```yaml
caching:
  ttlmap:
    article: 10
    articleAll: 20
```



# 统一全局异常处理

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200427102245.png)

1. 后端开发人员职责单一，只需要将异常捕获并转换为自定义异常一直对外抛出。不需要去想页面跳转404，以及异常响应的数据结构的设计。
2. 面向前端人员友好，后端返回给前端的数据应该有统一的数据结构，统一的规范。不能一个人一个响应的数据结构。而在此过程中不需要后端开发人员做更多的工作，交给全局异常处理器去处理“异常”到“响应数据结构”的转换。
3. 面向用户友好，用户能够清楚的知道异常产生的原因。这就要求自定义异常，全局统一处理，ajax接口请求响应统一的异常数据结构，页面模板请求统一跳转到404页面。
4. 面向运维友好，将异常信息合理规范的持久化，以便查询。

**规范**

1. Controller、Service、DAO层拦截异常转换为自定义异常，不允许将异常私自截留。必须对外抛出。
2. 统一数据响应代码，使用httpstatusode，不要自定义。自定义不方便记忆。200请求成功，400用户输入错误导致的异常，500系统内部异常，999未知异常。
3. 自定义异常里面有message属性，一定用友好的语言描述异常，并赋值给message.
4. 不允许对父类Excetion统一catch，要分小类catch，这样能够清楚地将异常转换为自定义异常传递给前端。

## 数据结构设计

原则：

1. CustomException 自定义异常。核心要素：异常错误编码（200正常,400,500），异常错误信息message。
2. ExceptionTypeEnum 枚举异常分类，将异常分类固化下来，防止开发人员思维发散。 核心要素 异常分类编码（200正常,400,500），异常分类描述。
3. AjaxResponse 用于响应Ajax请求。核心要素：是否请求成功 isok；响应code零与非零，零表示成功（200,400,500）；响应成功与否信息描述message；响应成功的数据data。
4. error.html
   另外还需要有一个统一处理CustomException的地方，即@ControllerAdvice和@ExceptionHandler。

**枚举异常类**

```java
public enum CustomExceptionType {
    USER_INPUT_ERROR(400,"用户输入异常"),
    SYSTEM_ERROR (500,"系统服务异常"),
    OTHER_ERROR(999,"其他未知异常");

    CustomExceptionType(int code, String typeDesc) {
        this.code = code;
        this.typeDesc = typeDesc;
    }

    private String typeDesc;//异常类型中文描述

    private int code; //code

    public String getTypeDesc() {
        return typeDesc;
    }

    public int getCode() {
        return code;
    }
}
```

**自定义异常类**

```java
public class CustomException extends RuntimeException {
    //异常错误编码
    private int code ;
    //异常信息
    private String message;

    private CustomException(){}

    public CustomException(CustomExceptionType exceptionTypeEnum, 
                           String message) {
        this.code = exceptionTypeEnum.getCode();
        this.message = message;
    }

    public int getCode() {
        return code;
    }

    @Override
    public String getMessage() {
        return message;
    }
}
```

**统一响应**

```java
/**
 * 接口数据请求统一响应数据结构
 */
@Data
public class AjaxResponse {
    //判断是否处理
    private  boolean isok;
    //状态码
    private  int code;
    //提示信息
    private  String message;
    //数据
    private  Object data;

    private AjaxResponse() {

    }

    //请求出现异常时的响应数据封装
    public static AjaxResponse error(CustomException e) {
        AjaxResponse resultBean = new AjaxResponse();
        resultBean.setIsok(false);
        resultBean.setCode(e.getCode());
        if(e.getCode() == CustomExceptionType.USER_INPUT_ERROR.getCode()){
            resultBean.setMessage(e.getMessage());
        }else if(e.getCode() == CustomExceptionType.SYSTEM_ERROR.getCode()){
            resultBean.setMessage(e.getMessage() + ",请将该异常信息发送给管理员!");
        }else{
            resultBean.setMessage("系统出现未知异常，请联系管理员!");
        }
        //TODO 这里最好将异常信息持久化
        return resultBean;
    }

    //请求出现异常时的响应数据封装
    public static AjaxResponse error(CustomExceptionType customExceptionType,
                                     String errorMessage) {
        AjaxResponse resultBean = new AjaxResponse();
        resultBean.setIsok(false);
        resultBean.setCode(customExceptionType.getCode());
        resultBean.setMessage(errorMessage);
        return resultBean;
    }

    //请求处理成功时的数据响应
    public static AjaxResponse success() {
        AjaxResponse resultBean = new AjaxResponse();
        resultBean.setIsok(true);
        resultBean.setCode(200);
        resultBean.setMessage("success");
        return resultBean;
    }

    //请求处理成功，并响应结果数据
    public static AjaxResponse success(Object data){
        AjaxResponse resultBean = new AjaxResponse();
        resultBean.setOk(true);
        resultBean.setCode(200);
        resultBean.setMessage("success");
        resultBean.setData(data);
        return resultBean;
    }

}
```

测试：

例如：更新操作，Controller无需返回额外的数据

```java
return AjaxResponse.success();
```

​												![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200427104313.png)

例如：查询接口，Controller需返回结果数据(data可以是任何类型数据)

```java
 return AjaxResponse.success(data);
```

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200427104408.png)

## 全局异常处理器

ControllerAdvice注解的作用就是监听所有的Controller，一旦Controller抛出CustomException，就会在@ExceptionHandler(CustomException.class)对该异常进行处理。

```java
@ControllerAdvice
public class WebExceptionHandler {

    @ExceptionHandler(CustomException.class)
    @ResponseBody
    public AjaxResponse customerException(CustomException e) {
        if(e.getCode() == CustomExceptionType.SYSTEM_ERROR.getCode()){
                 //400异常不需要持久化，将异常信息以友好的方式告知用户就可以
                //TODO 将500异常信息持久化处理，方便运维人员处理
        }
        return AjaxResponse.error(e);
    }

    @ExceptionHandler(Exception.class)
    @ResponseBody
    public AjaxResponse exception(Exception e) {
        //TODO 将异常信息持久化处理，方便运维人员处理

        //没有被程序员发现，并转换为CustomException的异常，都是其他异常或者未知异常.
        return AjaxResponse.error(new CustomException(CustomExceptionType.OTHER_ERROR,"未知异常"));
    }


}
```

**测试**

```java
@Service
public class ExceptionService {

    //服务层，模拟系统异常
    public void systemBizError() throws CustomException {
        try {
            Class.forName("com.mysql.jdbc.xxxx.Driver");
        } catch (ClassNotFoundException e) {
            throw new CustomException(CustomExceptionType.SYSTEM_ERROR,"在XXX业务，myBiz()方法内，出现ClassNotFoundException");
        }
    }

     //服务层，模拟用户输入数据导致的校验异常
    public List<String> userBizError(int input) throws CustomException {
        if(input < 0){ //模拟业务校验失败逻辑
            throw new CustomException(CustomExceptionType.USER_INPUT_ERROR,"您输入的数据不符合业务逻辑，请确认后重新输入！");
        }else{ //
            List<String> list = new ArrayList<>();
            list.add("科比");
            list.add("詹姆斯");
            list.add("库里");
            return list;
        }
    }

}
```

**友好的数据校验异常处理**

```java
@ExceptionHandler(MethodArgumentNotValidException.class)
@ResponseBody
public AjaxResponse handleBindException(MethodArgumentNotValidException ex) {
    FieldError fieldError = ex.getBindingResult().getFieldError();
    return AjaxResponse.error(new CustomException(CustomExceptionType.USER_INPUT_ERROR,fieldError.getDefaultMessage()));
}


@ExceptionHandler(BindException.class)
@ResponseBody
public AjaxResponse handleBindException(BindException ex) {
    FieldError fieldError = ex.getBindingResult().getFieldError();
    return AjaxResponse.error(new CustomException(CustomExceptionType.USER_INPUT_ERROR,fieldError.getDefaultMessage()));
}
```

## AOP异常处理

**程序员抛出自定义异常CustomException，全局异常处理截获之后返回@ResponseBody AjaxResponse，不是ModelAndView，所以我们无法跳转到error.html页面，那我们该如何做页面的全局的异常处理？**
答：

1. 用面向切面的方式，将CustomException转换为ModelAndViewException。
2. 全局异常处理器拦截ModelAndViewException，返回ModelAndView，即error.html页面
3. 切入点是带@ModelView注解的Controller层方法

**使用这种方法处理页面类异常，程序员只需要在页面跳转的Controller上加@ModelView注解即可**

因为用到了面向切面编程，所以引入maven依赖包

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>
```

新定义一个异常类ModelViewException

```java
public class ModelViewException extends RuntimeException{

    //异常错误编码
    private int code ;
    //异常信息
    private String message;

    public static ModelViewException transfer(CustomException e) {
        return new ModelViewException(e.getCode(),e.getMessage());
    }

    private ModelViewException(int code, String message){
        this.code = code;
        this.message = message;
    }

    int getCode() {
        return code;
    }

    @Override
    public String getMessage() {
        return message;
    }

}
```

ModelView 注解，只起到标注的作用

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD})//只能在方法上使用此注解
public @interface ModelView {

}
```

以@ModelView注解为切入点，面向切面编程,将CustomException转换为ModelViewException抛出。

```java
@Aspect
@Component
@Slf4j
public class ModelViewAspect {
    
    //设置切入点：这里直接拦截被@ModelView注解的方法
    @Pointcut("@annotation(club.krislin.exception.ModelView)")
    public void pointcut() { }
    
    /**
     * 当有ModelView的注解的方法抛出异常的时候，做如下的处理
     */
    @AfterThrowing(pointcut="pointcut()",throwing="e")
    public void afterThrowable(Throwable e) {
        log.error("切面发生了异常：", e);
        if(e instanceof  CustomException){
            throw ModelViewException.transfer((CustomException) e);
        }
    }
}
```

全局异常处理器:

```java
@ExceptionHandler(ModelViewException.class)
    public ModelAndView viewExceptionHandler(HttpServletRequest req, ModelViewException e) {
        ModelAndView modelAndView = new ModelAndView();

        //将异常信息设置如modelAndView
        modelAndView.addObject("exception", e);
        modelAndView.addObject("url", req.getRequestURL());
        modelAndView.setViewName("error");

        //返回ModelAndView
        return modelAndView;
    }
```

# Web开发

1. 后端开发人员职责单一，只需要将异常捕获并转换为自定义异常一直对外抛出。不需要去想页面跳转404，以及异常响应的数据结构的设计。
2. 面向前端人员友好，后端返回给前端的数据应该有统一的数据结构，统一的规范。不能一个人一个响应的数据结构。而在此过程中不需要后端开发人员做更多的工作，交给全局异常处理器去处理“异常”到“响应数据结构”的转换。
3. 面向用户友好，用户能够清楚的知道异常产生的原因。这就要求自定义异常，全局统一处理，ajax接口请求响应统一的异常数据结构，页面模板请求统一跳转到404页面。
4. 面向运维友好，将异常信息合理规范的持久化，以便查询。

## 原理

**使用SpringBoot的步骤：**

1、创建一个SpringBoot应用，选择我们需要的模块，SpringBoot就会默认将我们的需要的模块自动配置好

2、手动在配置文件中配置部分配置项目就可以运行起来了

3、专注编写业务代码，不需要考虑以前那样一大堆的配置了。

**自动配置**

1. `WebMvcAutoConfiguration`
2. `WebMvcProperties`
3. `ViewResolver`自动配置
4. 静态资源自动映射
5. Formatter与Converter自动配置
6. `HttpMessageConverter`自动配置
7. 静态首页
8. favicon
9. 错误处理

## 静态资源映射规则

SpringBoot中，SpringMVC的web配置都在 WebMvcAutoConfiguration 这个配置类里面；

我们可以去看看 WebMvcAutoConfigurationAdapter 中有很多配置方法；

有一个方法：addResourceHandlers 添加资源处理

```java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    if (!this.resourceProperties.isAddMappings()) {
        // 已禁用默认资源处理
        logger.debug("Default resource handling disabled");
        return;
    }
    // 缓存控制
    Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
    CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
    // webjars 配置
    if (!registry.hasMappingForPattern("/webjars/**")) {
        customizeResourceHandlerRegistration(registry.addResourceHandler("/webjars/**")
                                             .addResourceLocations("classpath:/META-INF/resources/webjars/")
                                             .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
    }
    // 静态资源配置
    String staticPathPattern = this.mvcProperties.getStaticPathPattern();
    if (!registry.hasMappingForPattern(staticPathPattern)) {
        customizeResourceHandlerRegistration(registry.addResourceHandler(staticPathPattern)
                                             .addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations()))
                                             .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
    }
}
```

所有 `/webjars/**` ，都去 `classpath:/META-INF/resources/webjars/` 找资源

`webjars`：以jar包的方式引入静态资源；

网站：https://www.webjars.org

要使用jQuery，我们只要要引入jQuery对应版本的pom依赖即可！

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.4.1</version>
</dependency>
```

导入完毕，查看webjars目录结构，并访问Jquery.js文件！

访问：只要是静态资源，SpringBoot就会去对应的路径寻找资源，我们这里访问：http://localhost:8080/webjars/jquery/3.4.1/jquery.js

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615217858036-419f758e-5b5d-42ad-946f-e1689a5a0ea8.png)



**第二种静态资源映射规则**

那我们项目中要是使用自己的静态资源该怎么导入呢？我们看下一行代码；

我们去找staticPathPattern发现第二种映射规则 ：/** , 访问当前的项目任意资源，它会去找 resourceProperties 这个类，我们可以点进去看一下分析：

```java
// 进入方法
public String[] getStaticLocations() {
    return this.staticLocations;
}
// 找到对应的值
private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
// 找到路径
private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { 
    "classpath:/META-INF/resources/",
  "classpath:/resources/", 
    "classpath:/static/", 
    "classpath:/public/" 
};
```

ResourceProperties 可以设置和我们静态资源有关的参数；这里面指向了它会去寻找资源的文件夹，即上面数组的内容。

所以得出结论，以下四个目录存放的静态资源可以被我们识别：

```java
"classpath:/META-INF/resources/"
"classpath:/resources/"
"classpath:/static/"
"classpath:/public/"
```

我们可以在resources根目录下新建对应的文件夹，都可以存放我们的静态文件；

| 数组中的值                     | 在项目中的位置                         |
| ------------------------------ | -------------------------------------- |
| classpath:/META-INF/resources/ | src/main/resources/META-INF/resources/ |
| classpath:/resources/          | src/main/resources/resources/          |
| classpath:/static/             | src/main/resources/static/             |
| classpath:/public/             | src/main/resources/public/             |

优先级：resources>static>public

**自定义静态资源路径**

我们也可以自己通过配置文件来指定一下，哪些文件夹是需要我们放静态资源文件的，在application.properties中配置；

```java
spring.resources.static-locations=classpath:/coding/,classpath:/kuang/
```

一旦自己定义了静态文件夹的路径，原来的自动配置就都会失效了！

## 欢迎页映射

静态资源文件夹说完后，继续向下看源码！可以看到一个欢迎页的映射，就是我们的首页

```java
@Bean
public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
                                                           FormattingConversionService mvcConversionService,
                                                           ResourceUrlProvider mvcResourceUrlProvider) {
    WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
        new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(), // getWelcomePage 获得欢迎页
        this.mvcProperties.getStaticPathPattern());
    welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
    return welcomePageHandlerMapping;
}
```

点进去继续看

```java
private Optional<Resource> getWelcomePage() {
    String[] locations = getResourceLocations(this.resourceProperties.getStaticLocations());
    // ::是java8 中新引入的运算符
    // Class::function的时候function是属于Class的，应该是静态方法。
    // this::function的funtion是属于这个对象的。
    // 简而言之，就是一种语法糖而已，是一种简写
    return Arrays.stream(locations).map(this::getIndexHtml).filter(this::isReadable).findFirst();
}
// 欢迎页就是一个location下的的 index.html 而已
private Resource getIndexHtml(String location) {
    return this.resourceLoader.getResource(location + "index.html");
}
```

欢迎页，静态资源文件夹下的所有 index.html 页面；被 /** 映射。

关于网站图标说明：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615218077235-053dd2c4-f2ef-4401-8064-817e4425ce5b.jpeg)

与其他静态资源一样，Spring Boot在配置的静态内容位置中查找 favicon.ico。如果存在这样的文件，它将自动用作应用程序的favicon。

1、关闭SpringBoot默认图标

```java
#关闭默认图标
spring.mvc.favicon.enabled=false
```

2、放个图标在静态资源目录下

3、清除浏览器缓存！刷新网页，发现图标已经变化

## 整合jsp

spring-boot-starter-web 包依赖了 spring-boot-starter-tomcat 不需要再单独配置。
引入 jstl 和内嵌的 tomcat，jstl 是一个 JSP 标签集合，它封装了 JSP 应用的通用核心功能。
tomcat-embed-jasper 主要用来支持 JSP 的解析和运行。

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!-- spring boot 内置tomcat jsp支持 -->
<dependency>
  <groupId>org.apache.tomcat.embed</groupId>
  <artifactId>tomcat-embed-jasper</artifactId>
</dependency>
<!--jsp页面使用jstl标签-->
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>jstl</artifactId>
</dependency>
```

spring.mvc.view.prefix 指明 jsp 文件在 webapp 下的哪个目录
spring.mvc.view.suffix 指明 jsp 以什么样的后缀结尾

```yaml
spring:
  mvc:
    view:
      suffix: .jsp
      prefix: /WEB-INF/jsp/

debug: true
```

**测试**

```java
@Controller
@RequestMapping("/template")
public class TemplateController {

    @Resource(name="articleMybatisRestServiceImpl")
    ArticleRestService articleRestService;

    @GetMapping("/jsp")
    public String index(String name, Model model) {

        List<ArticleVO> articles = articleRestService.getAll();

        model.addAttribute("articles", articles);

        //模版名称，实际的目录为：src/main/webapp/WEB-INF/jsp/jsptemp.jsp
        return "jsptemp";
    }
}
```

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<table class="">
    <tr>
        <td>作者</td>
        <td>教程名称</td>
        <td>内容</td>
    </tr>
    <c:forEach var="article" items="${articles}">
        <tr class="text-info">
            <td>${article.author}</td>
            <td>${article.title}</td>
            <td>${article.content}</td>
        </tr>
    </c:forEach>


</table>
<img src="/image/jsp.png">
</body>
</html>
```

因为jsp对jar运行的方式支持不好，所以要一一进行测试:

1. 使用IDEA启动类启动测试，没有问题
2. 使用`spring-boot:run -f pom.xml`测试，没有问题
3. 打成jar包通过`java -jar`方式运行，页面报错
4. 打成war包，运行于外置的tomcat，没有问题

所以，无法用jar包的形式运行jsp应用。

## 整合freemarker

FreeMarker是一个模板引擎，一个基于模板生成文本输出的通用工具，使用纯Java编写。

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-freemarker</artifactId>
</dependency> 
```

```yaml
spring:
  freemarker:
    cache: false # 缓存配置 开发阶段应该配置为false 因为经常会改
    suffix: .html # 模版后缀名 默认为ftl
    charset: UTF-8 # 文件编码
    template-loader-path: classpath:/templates/ 
```

**测试**

```java
@Controller
@RequestMapping("template")
public class TemplateController {
    @GetMapping("/freemarker")
    public String index(Model model){
        Article article = new Article();
        article.setAuthor("krislin");
        article.setTitle("spring boot");
        article.setContent("spring boot学习");

        model.addAttribute("article",article);

        //模版名称，实际的目录为：resources/templates/freemarkerTemplate.html
        return "freemarkerTemplate";
    }
}
```

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8" />
    <title>freemarker简单示例</title>
</head>
<body>
<h1>Hello Freemarker</h1>

<table class="">
    <tr>
        <td>作者</td>
        <td>教程名称</td>
        <td>内容</td>
    </tr>

    <tr>
        <td>${article.author}</td>
        <td>${article.title}</td>
        <td>${article.content}</td>
    </tr>

</table>


<img src="/img/template.jpg">
</body>
</html>
```



## 整合thymeleaf

Thymeleaf 是一个服务器端 Java 模板引擎，能够处理 HTML、XML、CSS、JAVASCRIPT 等模板文件。Thymeleaf 模板可以直接当作静态原型来使用，它主要目标是为开发者的开发工作流程带来优雅的自然模板，也是 Java 服务器端 HTML5 开发的理想选择。

```xml
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

```yaml
spring:
  thymeleaf:
    cache: false # 启用缓存:建议生产开启
    check-template-location: true # 检查模版是否存在
    enabled: true # 是否启用
    encoding: UTF-8 # 模版编码
    excluded-view-names: # 应该从解析中排除的视图名称列表（用逗号分隔）
    mode: HTML5 # 模版模式
    prefix: classpath:/templates/ # 模版存放路径
    suffix: .html # 模版后缀
```

```java
@Controller
@RequestMapping("template")
public class TemplateController {
    @GetMapping("/thymeleaf")
    public String index(Model model){
        List<Article> articles = new ArrayList<>();
        Article article = new Article();
        article.setAuthor("krislin");
        article.setTitle("spring boot");
        article.setContent("spring boot学习");

        Article article1 = new Article();
        article1.setAuthor("krislin1");
        article1.setTitle("spring boot1");
        article1.setContent("spring boot学习1");

        articles.add(article);
        articles.add(article1);

        model.addAttribute("articles",articles);

        return "thymeleafTemplate";
    }
}
```

```html
<!DOCTYPE html>
<--!导入thymeleaf的名称空间-->
<html xmlns:th="http://www.thymeleaf.org">
<head lang="en">
    <meta charset="UTF-8" />
    <title>thymeleaf简单示例</title>
</head>
<body>
<h1>Hello Thymeleaf</h1>

<table class="">
    <tr>
        <td>作者</td>
        <td>教程名称</td>
        <td>内容</td>
    </tr>
    <tr th:each="item : ${articles}">
        <td th:text="${item.author}"></td>
        <td th:text="${item.title}"></td>
        <td th:text="${item.content}"></td>
    </tr>
</table>

<img src="/img/template.jpg">
</body>
</html>
```

## PO、BO、VO

**PO:** persistent object 持久对象，对应数据库中的entity。

**BO:** business object 业务对象，业务对象主要作用是把业务逻辑封装为一个对象。通常一个BO是多个PO的组合体。

**VO:** view object，主要与web页面的展示结构相对应，所以VO也是前端与后端的数据交换定义。

<img src="https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200422161747.png"  />

**如果业务可用一个实体类对象，就可以贯穿持久层到展现层，就没有必要做映射赋值转换，也没有必要去分VO、BO、PO。**

**Dozer**

```xml
<dependency>
    <groupId>com.github.dozermapper</groupId>
    <artifactId>dozer-spring-boot-starter</artifactId>
    <version>6.2.0</version>
</dependency>
```

dozer是一个能把实体和实体之间进行转换的工具，只要建立好映射关系，就像是ORM的数据库和实体映射一样。

使用方法：

```java
    Mapper mapper = DozerBeanMapperBuilder.buildDefault();
    // article(PO) -> articleVO
    ArticleVO articleVO = mapper.map(article, ArticleVO.class);
```

转换过程将**所有同名同类型**的数据自动赋值给articleVO的成员变量，相当于以下代码

```java
articleVO.setId(article.getId());
articleVO.setAuthor(article.getAuthor());
articleVO.setTitle(article.getTitle());
articleVO.setContent(article.getContent());
articleVO.setCreateTime(article.getCreateTime());
```

**自定义类型转换**

配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mappings xmlns="http://dozermapper.github.io/schema/bean-mapping"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://dozermapper.github.io/schema/bean-mapping
          http://dozermapper.github.io/schema/bean-mapping.xsd">
    <configuration>
        <date-format>yyyy-MM-dd HH:mm:ss</date-format>
    </configuration>
    <mapping>
        <class-a>club.krislin.bootlaunch.dozer.TestA</class-a>
        <class-b>club.krislin.bootlaunch.dozer.TestB</class-b>
        <field>
            <a>createDate</a>
            <b>cDate</b>
        </field>
    </mapping>

</mappings>
```

然后把dozer转换配置文件通知application.yml，进行加载生效

```yaml
dozer:
  mapping-files: classpath:/dozer/dozer-mapping.xml
```

这样一个对象里面有String属性到Date属性转换的时候，就会自动应用这个转换规则， 不再报错。

# RESTful接口开发及测试

API（Application Programming Interface），顾名思义：是一组编程接口规范，客户端与服务端通过请求响应进行数据通信。REST（Representational State Transfer）决定了接口的形式与规则。**RESTful是基于http方法的API设计风格，而不是一种新的技术.**

1. 看Url就知道要什么资源
2. 看http method就知道针对资源干什么
3. 看http status code就知道结果如何

对接口开发提供了一种可以广泛适用的规范，为前端后端交互减少了接口交流的口舌成本，是**约定大于配置**的体现。通过下面的设计，大家来理解一下这三句话。

>并不是所有的接口，都能用REST的形式来表述。在实际工作中，灵活运用，我们用RESTful风格的目的是为大家提供统一标准，避免不必要的沟通成本的浪费，形成一种通用的风格。

### 特性

**REST 是面向资源的（名词）**

REST 通过 URI 暴露资源时，会强调不要在 URI 中出现动词。比如：

| 不符合REST的接口URI      | 符合REST接口URI       | 功能           |
| :----------------------- | :-------------------- | :------------- |
| GET /api/getDogs         | GET /api/dogs/{id}    | 获取一个小狗狗 |
| GET /api/getDogs         | GET /api/dogs         | 获取所有小狗狗 |
| GET /api/addDogs         | POST /api/dogs        | 添加一个小狗狗 |
| GET /api/editDogs/{id}   | PUT /api/dogs/{id}    | 修改一个小狗狗 |
| GET /api/deleteDogs/{id} | DELETE /api/dogs/{id} | 删除一个小狗狗 |

**用HTTP方法体现对资源的操作（动词）**

- GET ： 获取、读取资源
- POST ： 添加资源
- PUT ： 修改资源
- DELETE ： 删除资源

实际上，这四个动词实际上就对应着增删改查四个操作，这就利用了HTTP动词来表示对资源的操作。

**HTTP状态码**

通过HTTP状态码体现动作的结果,不要自定义

```
200 OK 
400 Bad Request 
500 Internal Server Error
```

在 APP 与 API 的交互当中，其结果逃不出这三种状态：

- 所有事情都按预期正确执行完毕 - 成功
- APP 发生了一些错误 – 客户端错误
- API 发生了一些错误 – 服务器端错误

**Get方法和查询参数不应该改变数据**

改变数据的事交给POST、PUT、DELETE

**使用复数名词**

/dogs 而不是 /dog

**复杂资源关系的表达**

GET /cars/711/drivers/ 返回 使用过编号711汽车的所有司机

GET /cars/711/drivers/4 返回 使用过编号711汽车的4号司机

**高级用法:HATEOAS**

**HATEOAS**:Hypermedia as the Engine of Application State 超媒体作为应用状态的引擎。
RESTful API最好做到HATEOAS，**即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么**。

```json
{"link": {
  "rel":   "collection https://www.example.com/zoos",
  "href":  "https://api.example.com/zoos",
  "title": "List of zoos",
  "type":  "application/vnd.yourformat+json"
}}
```

上面代码表示，文档中有一个link属性，用户读取这个属性就知道下一步该调用什么API或者可以调用什么API了。

**资源过滤、排序、选择和分页的表述**

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200416131439.png)

**版本化你的API**

强制性增加API版本声明，不要发布无版本的API。如：/api/v1/blog

**面向扩展开放，面向修改关闭**：一个版本的接口开发完成测试上线之后，一般不会对接口进行修改，如果有新的需求就开发新的接口进行功能扩展。

### 相关注解

**`@RequestBody`与`@ResponseBody`**

```java
//注意并不要求@RequestBody与@ResponseBody成对使用。
public @ResponseBody  AjaxResponse saveArticle(@RequestBody ArticleVO article)
```

如上代码所示：

- `@RequestBody`修饰请求参数，注解用于接收HTTP的body，默认是使用JSON的格式，**真正意义在于能够使用对象或者嵌套对象接收前端数据。
  **
- `@ResponseBody`修饰返回值，注解用于在HTTP的body中携带响应数据，默认是使用JSON的格式。如果不加该注解，spring响应字符串类型，是跳转到模板页面或JSP页面的开发模式。

**`@RequestMapping`**

`@RequestMapping`注解是所有常用注解中，最有看点的一个注解，用于标注HTTP服务端点。它的很多属性对于丰富我们的应用开发方式方法，都有很重要的作用。如：

- `value`： 应用请求端点，最核心的属性，用于标志请求处理方法的唯一性；
- `method`： HTTP协议的method类型， 如：GET、POST、PUT、DELETE等；
- `consumes`： HTTP协议请求内容的数据类型（Content-Type），例如`application/json`, `text/html`;
- `produces`: HTTP协议响应内容的数据类型。下文会详细讲解。
- `params`： HTTP请求中必须包含某些参数值的时候，才允许被注解标注的方法处理请求。
- `headers`： HTTP请求中必须包含某些指定的header值，才允许被注解标注的方法处理请求。

```java
@RequestMapping(value = "/article", method = POST)
@PostMapping(value = "/article")
```

上面代码中两种写法起到的是一样的效果，也就是`PostMapping`等同于`@RequestMapping`的method等于POST。同理：`@GetMapping`、`@PutMapping`、`@DeleteMapping`也都是简写的方式。

**`@RestController`与`@Controller`**

`@Controller`注解是开发中最常使用的注解，它的作用有两层含义：

- 一是告诉Spring，被该注解标注的类是一个Spring的Bean，需要被注入到Spring的上下文环境中。
- 二是该类里面所有被`RequestMapping`标注的注解都是HTTP服务端点。

`@RestController`相当于 `@Controller`和`@ResponseBody`结合。它有两层含义：

- 一是作为Controller的作用，将控制器类注入到Spring上下文环境，该类`RequestMapping`标注方法为HTTP服务端点。
- 二是作为`ResponseBody`的作用，请求响应默认使用的序列化方式是JSON，而不是跳转到`jsp`或模板页面。

**`@PathVariable` 与`@RequestParam`**

`PathVariable`用于URI上的{参数}，而`RequestParam`用于接收普通表单方式或者ajax模拟表单提交的参数数据。

```java
@DeleteMapping("/article/{id}")
public @ResponseBody AjaxResponse deleteArticle(@PathVariable Long id) {

@PostMapping("/article")
public @ResponseBody AjaxResponse deleteArticle(@RequestParam Long id) {
```

### 规范接口

1. **定义资源**

```java
@Data
@Builder
public class Article {
    private Long  id;
    private String author;
    private String title;
    private String content;
    private Date createTime;
}
```

`Data`、`Builder`都是`lombok`提供给我们的注解，有利于我们简化代码。

- `@Builder`为我们提供了通过对象属性的链式赋值构建对象的方法，下文中代码会有详细介绍。
- `@Data`注解帮我们定义了一系列常用方法，如：`getters`、`setters`、`hashcode`、`equals`等

2. **定义HTTP方法与Controller**

我们实现一个简单的RESTful接口

- 增加一篇Article ，使用POST方法
- 删除一篇Article，使用DELETE方法，参数是id
- 更新一篇Article，使用PUT方法，以id为主键进行更新
- 获取一篇Article，使用GET方法

下面代码中并未真正的进行数据库操作

```java
@Slf4j
@RestController
@RequestMapping("/rest")
public class ArticleRestController {
 
    //增加一篇Article ，使用POST方法
    @RequestMapping(value = "/article", method = POST, produces = "application/json")
    public AjaxResponse saveArticle(@RequestBody Article article) {
        //因为使用了lombok的Slf4j注解，这里可以直接使用log变量打印日志
        log.info("saveArticle：{}",article);
        return  AjaxResponse.success(article);
    }
 
    
    //删除一篇Article，使用DELETE方法，参数是id
    @RequestMapping(value = "/article/{id}", method = DELETE, produces = "application/json")
    public AjaxResponse deleteArticle(@PathVariable Long id) {
        log.info("deleteArticle：{}",id);
        return AjaxResponse.success(id);
    }
 
     //更新一篇Article，使用PUT方法，以id为主键进行更新
    @RequestMapping(value = "/article/{id}", method = PUT, produces = "application/json")
    public AjaxResponse updateArticle(@PathVariable Long id, @RequestBody Article article) {
        article.setId(id);
        log.info("updateArticle：{}",article);
        return AjaxResponse.success(article);
    }
 
    //获取一篇Article，使用GET方法
    @RequestMapping(value = "/article/{id}", method = GET, produces = "application/json")
    public AjaxResponse getArticle(@PathVariable Long id) {

        //使用lombok提供的builder构建对象
        Article article = Article.builder()
                .id(1L)
                .author("krislin")
                .title("t1")
                .content("spring boot学习")
                .createTime(new Date()).build();
        return AjaxResponse.success(article);
    }
}
```

因为使用了`lombok`的`@Slf4j`注解（类的定义处），就可以直接使用log变量打印日志。不需要写下面的这行代码。

```java
private static final Logger log = LoggerFactory.getLogger(HelloController.class);
```

3. **规范返回格式**

   ```java
   @Data
   public class AjaxResponse {
       private boolean isOk; //请求是否处理成功
       private int code;   //请求响应状态码（200、400、500）
       private String message; //请求结果描述信息
       private Object data; //请求结果数据
   
       public AjaxResponse() {
       }
   
       //请求出现异常时的响应数据封装
       public static AjaxResponse error(CustomException e){
           AjaxResponse resultBean = new AjaxResponse();
           resultBean.setOk(false);
           resultBean.setCode(e.getCode());
           if (e.getCode() == CustomExceptionType.USER_INPUT_ERROR.getCode()){
               resultBean.setMessage(e.getMessage());
           }else if (e.getCode() == CustomExceptionType.SYSTEM_ERROR.getCode()){
               resultBean.setMessage(e.getMessage()+",系统出现异常，请联系管理员");
           }else {
               resultBean.setMessage("系统出现未知异常，请联系管理员");
           }
           return resultBean;
       }
   
       //请求成功的响应，不带查询数据（用于删除、修改、新增接口）
       public static AjaxResponse success(){
           AjaxResponse resultBean = new AjaxResponse();
           resultBean.setOk(true);
           resultBean.setCode(200);
           resultBean.setMessage("success");
           return resultBean;
       }
       
       //请求成功的响应，带有查询数据（用于数据查询接口）
       public static AjaxResponse success(Object data){
           AjaxResponse resultBean = new AjaxResponse();
           resultBean.setOk(true);
           resultBean.setCode(200);
           resultBean.setMessage("success");
           resultBean.setData(data);
           return resultBean;
       }
   }
   ```

### json处理相关依赖

**开源的Jackson**：SpringBoot默认是使用Jackson作为JSON数据格式处理的类库，Jackson在各方面都比较优秀，所以不建议将Jackson替换为Gson或fastjson。

**Google的Gson**：Gson是Google为满足内部需求开发的JSON数据处理类库，其核心结构非常简单，toJson与fromJson两个转换函数实现对象与JSON数据的转换，

**阿里巴巴的FastJson**：Fastjson是阿里巴巴开源的JSON数据处理类库，其主要特点是序列化速度快。当并发数据量越大的时候，越能体现出fastjson的优势。

**Jackson相关注解**

这些注解通常用于标注java实体类或实体类的属性。

- @JsonPropertyOrder(value={"pname1","pname2"}) 改变子属性在JSON序列化中的默认定义的顺序。如：param1在先，param2在后。
- @JsonIgnore 排除某个属性不做序列化与反序列化
- @JsonProperty(anotherName) 为某个属性换一个名称，体现在JSON数据里面
- @JsonInclude(JsonInclude.Include.NON_NULL) 排除为空的元素不做序列化反序列化
- @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8") 指定日期类型的属性格式

### Mock测试接口

#### 简介

**什么是Mock?**

在面向对象程序设计中，模拟对象（英语：mock object，也译作模仿对象）是以可控的方式模拟真实对象行为的**假的对象**。比如:对象B依赖于对象A，但是A代码还没写是一个空类空方法不能用，我们来mock一个假的A来完成测试。

**为什么要使用Mock?**

> 在单元测试中，模拟对象可以模拟复杂的、真实的对象的行为， 如果真实的对象无法放入单元测试中，使用模拟对象就很有帮助。

在下面的情形，可能需要使用模拟对象来代替真实对象：

- 真实对象的行为是不确定的（例如，当前的时间或当前的温度）；
- 真实对象很难搭建起来；
- 真实对象的行为很难触发（例如，网络错误）；
- 真实对象速度很慢（例如，一个完整的数据库，在测试之前可能需要初始化）；
- 真实的对象是用户界面，或包括用户界面在内；
- 真实的对象使用了回调机制；
- 真实对象可能还不存在（例如，其他程序员还为完成工作）；
- 真实对象可能包含不能用作测试的信息（高度保密信息等）和方法。

**Mockito**

Mockito是GitHub上使用最广泛的Mock框架,并与JUnit结合使用.Mockito框架可以创建和配置mock对象.使用Mockito简化了具有外部依赖的类的测试开发!

#### 模拟网络请求进行测试

在开始书写测试代码之前，我们先回顾一下JUnit常用的测试注解。在junit4和junit5中，注解的写法有些许变化。

| junit4         | junit5        | 特点                                                   |
| :------------- | :------------ | :----------------------------------------------------- |
| `@Test`        | `@Test`       | 声明一个测试方法                                       |
| `@BeforeClass` | `@BeforeAll`  | 在当前类的所有测试方法之前执行。注解在【静态方法】上   |
| `@AfterClass`  | `@AfterAll`   | 在当前类中的所有测试方法之后执行。注解在【静态方法】上 |
| `@Before`      | `@BeforeEach` | 在每个测试方法之前执行。注解在【非静态方法】上         |
| `@After`       | `@AfterEach`  | 在每个测试方法之后执行。注解在【非静态方法】           |



```java
//@Transactional
@Slf4j
@SpringBootTest
public class ArticleRestControllerTest {

    //mock对象
    private MockMvc mockMvc;

    //mock对象初始化
    @Before
    public void setUp() {
        mockMvc = MockMvcBuilders.standaloneSetup(new ArticleRestController()).build();
    }

    //测试方法
    @Test
    public void saveArticle() throws Exception {

        String article = "{\n" +
                "    \"id\": 1,\n" +
                "    \"author\": \"krislin\",\n" +
                "    \"title\": \"spring boot学习\",\n" +
                "    \"content\": \"c\",\n" +
                "    \"createTime\": \"2017-07-16 05:23:34\",\n" +
                "    \"reader\":[{\"name\":\"krislin\",\"age\":18},{\"name\":\"kobe\",\"age\":37}]\n" +
                "}";
        MvcResult result = mockMvc.perform(
                MockMvcRequestBuilders.request(HttpMethod.POST, "/rest/article")
                .contentType("application/json").content(article))
                .andExpect(MockMvcResultMatchers.status().isOk())
                .andExpect(MockMvcResultMatchers.jsonPath("$.data.author").value("zimug"))
                .andExpect(MockMvcResultMatchers.jsonPath("$.data.reader[0].age").value(18))
                .andDo(print())
                .andReturn();

        log.info(result.getResponse().getContentAsString());

    }
}
```

**`@SpringBootTest` 注解说明:**

是用来创建Spring的上下文`ApplicationContext`，保证测试在上下文环境里运行。单独使用`@SpringBootTest`不会启动servlet容器。所以**只是使用`SpringBootTest` 注解时不可以使用`@Resource`和`@Autowired`等注解进行bean的依赖注入**。

**@Transactional**

可以使单元测试进行事务回滚，以保证数据库表中没有因测试造成的垃圾数据，因此保证单元测试可以反复执行；
但是不建议这么做，使用该注解会破坏测试真实性。

**`MockMvc`对象有以下几个基本的方法:**

- `perform `: 执行一个`RequestBuilder`请求，会自动执行`SpringMVC`的流程并映射到相应的控制器Controller执行处理。
- `contentType`：发送请求内容的序列化的格式，`"application/json"`表示JSON数据格式
- `andExpect`: 添加`RequsetMatcher`验证规则，验证控制器执行完成后结果是否正确，或者说是结果是否与我们期望（Expect）的一致。
- `andDo`: 添加`ResultHandler`结果处理器，比如调试时打印结果到控制台
- `andReturn`: 最后返回相应的`MvcResult`,然后进行自定义验证/进行下一步的异步处理

***真实servlet容器环境下的测试***

换一种写法：看看有没有什么区别。在测试类上面额外加上这样两个注解，并且`mockMvc`对象使用@Resource自动注入，删掉`Before`注解及`setUp`函数。

```java
@RunWith(SpringRunner.class)
@AutoConfigureMockMvc
```

没加上这两个注解之前，日志中是不会打印这个`SpringBoot`的flag的。

**`@RunWith`注解**

- `RunWith`方法为我们构造了一个的Servlet容器运行运行环境，并在此环境下测试。然而为什么要构建servlet容器？因为使用了依赖注入，注入了`MockMvc`对象，而在上一个例子里面是我们自己new的。
- 而`@AutoConfigureMockMvc`注解，该注解表示 `MockMvc`由spring容器构建，你只负责注入之后用就可以了。这种写法是为了让测试在Spring容器环境下执行。
- Spring的容器环境是什么呢？比如常见的 Service、Dao 都是Spring容器里的bean，装载到容器里面都可以使用`@Resource`和`@Autowired`来注入引用。

简单的说：**如果你单元测试代码使用了依赖注入就加上`@RunWith`，如果你不是手动`new MockMvc`对象就加上`@AutoConfigureMockMvc`**

#### **轻量级测试**

在`RunWith`的`AutoConfigureMockMvc`注解的共同作用下，启动了`SpringMVC`的运行容器，并且把项目中所有的@Bean全部都注入进来。

```java
@RunWith(SpringRunner.class)
@WebMvcTest(ArticleRestController.class)
public class WebMockTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private ArticleRestService service;

    @Test
    public void saveArticle() throws Exception {
        String article = "{\n" +
                "    \"id\": 1,\n" +
                "    \"author\": \"zimug\",\n" +
                "    \"title\": \"手摸手教你开发spring boot\",\n" +
                "    \"content\": \"c\",\n" +
                "    \"createTime\": \"2017-07-16 05:23:34\",\n" +
                "    \"reader\":[{\"name\":\"zimug\",\"age\":18},{\"name\":\"kobe\",\"age\":37}]\n" +
                "}";
    
    
        ObjectMapper objectMapper = new ObjectMapper();
        Article articleObj = objectMapper.readValue(article,Article.class);
    
        //打桩
        when(articleRestService.saveArticle(articleObj)).thenReturn("ok");
    
    
        MvcResult result = mockMvc.perform(MockMvcRequestBuilders.request(HttpMethod.POST, "/rest/article")
                .contentType("application/json").content(article))
                .andExpect(MockMvcResultMatchers.status().isOk())
                .andExpect(MockMvcResultMatchers.jsonPath("$.data.author").value("zimug"))
                .andExpect(MockMvcResultMatchers.jsonPath("$.data.reader[0].age").value(18))
                .andDo(print())
                .andReturn();
    
        log.info(result.getResponse().getContentAsString());
    
    }
}
```

**`@SpringBootTest`与`@WebMvcTest`区别**

- `@SpringBootTest`注解告诉`SpringBoot`去寻找一个主配置类(例如带有`@SpringBootApplication`的配置类)，并使用它来启动Spring应用程序上下文。`SpringBootTest`加载完整的应用程序并注入所有可能的bean，因此速度会很慢。
- `@WebMvcTest`注解主要用于controller层测试，只覆盖应用程序的controller层，HTTP请求和响应是Mock出来的，因此不会创建真正的网络连接。`WebMvcTest`要快得多，因为我们只加载了应用程序的一小部分。

**`@MockBean`**

如果我们使用了`WebMvcTest`，只加载了Controller层的bean，那么Controller所依赖的Service没有被加载进来，我们可以用`MockBean`伪造模拟一个Service 。

```java
when(articleRestService.saveArticle(articleObj)).thenReturn("ok");
```

也就是告诉测试用例程序，当你调用`articleRestService.saveArticle(articleObj)`方法的时候，不要去真的调用这个方法，直接返回一个结果（“ok”）就好了。`articleRestService`就是那个不能被测试或者不能测试的类，需要我们来模拟Mock一下。

**`MockMvc`更多的用法总结**

```java
//模拟GET请求：
mockMvc.perform(MockMvcRequestBuilders.get("/user/{id}", userId));

//模拟Post请求：
mockMvc.perform(MockMvcRequestBuilders.post("uri", parameters));

//模拟文件上传：
mockMvc.perform(MockMvcRequestBuilders.multipart("uri").file("fileName", "file".getBytes("UTF-8")));


//模拟session和cookie：
mockMvc.perform(MockMvcRequestBuilders.get("uri").sessionAttr("name", "value"));
mockMvc.perform(MockMvcRequestBuilders.get("uri").cookie(new Cookie("name", "value")));

//设置HTTP Header：
mockMvc.perform(MockMvcRequestBuilders
                        .get("uri", parameters)
                        .contentType("application/x-www-form-urlencoded")
                        .accept("application/json")
                        .header("", ""));
```

**为什么要写代码做测试？使用接口测试工具Postman很方便啊**

- 因为在做系统的自动化持续集成的时候，会要求自动的做单元测试，只有所有的单元测试都跑通了，才能打包构建。比如：使用maven。这里重点是**自动化**，所以postman这种工具很难插入到持续集成的自动化流程中去。所以需要我们自己写代码完成单元测试。
- 另外写代码测试能模拟出更多复杂的测试场景，类似Postman工具只能完成简单的接口测试。

# 整合日志框架

## 简介

Spring Boot选用 `SLF4j`和`logback`

Spring Boot 默认的日志记录框架使用的是 Logback，此外我们还可以选择 Log4j 和 Log4j2。其中 Log4j 可以认为是一个过时的函数库，已经停止更新，不推荐使用，相比之下，性能和功能也是最差的。logback 虽然是 Spring Boot 默认的，但性能上还是不及 Log4j2，因此，在现阶段，日志记录首选 Log4j2。

常见的日志级别。

1. TRACE：追踪。一般上对核心系统进行性能调试或者跟踪问题时有用，此级别很低，一般上是不开启的，开启后日志会很快就打满磁盘的。
2. DEBUG:调试。这个大家应该不陌生了。开发过程中主要是打印记录一些运行信息之类的。
3. INFO:信息。这个是最常见的了，大部分默认就是这个级别的日志。一般上记录了一些交互信息，一些请求参数等等。可方便定位问题，或者还原现场环境的时候使用。此日志相对来说是比较重要的。
4. WARN:警告。这个一般上是记录潜在的可能会引发错误的信息。比如启动时，某某配置文件不存在或者某个参数未设置之类的。
5. ERROR:错误。这个也是比较常见的，一般上是在捕获异常时输出，虽然发生了错误，但不影响系统的正常运行。但可能会导致系统出错或是宕机等。

**常见术语概念解析**

1. appender：主要控制日志输出到哪里，比如：文件、数据库、控制台打印等
2. logger: 用来设置某一个包或者具体某一个类的日志打印级别、以及指定appender
3. root：也是一个logger，是一个特殊的logger。所有的logger最终都会将输出流交给root，除非设置logger中配置了additivity="false"。
4. rollingPolicy：所有日志都放在一个文件是不好的，所以可以指定滚动策略，按照一定周期或文件大小切割存放日志文件。
5. RolloverStrategy：日志清理策略。通常是指日志保留的时间。
6. 异步日志：单独开一个线程做日志的写操作，达到不阻塞主线程的目的

**如何让系统中所有的日志都统一到slf4j**

1. 将系统中其他日志框架先排除出去；
2. 用中间包来替换原有的日志框架（适配器的类名和包名与替换的被日志框架一致）；
3. 我们导入slf4j其他的实现

**SpringBoot能自动适配所有的日志，而且底层使用slf4j+logback的方式记录日志，引入其他框架的时候，只需要把这个框架依赖的日志框架排除掉即可；**

## 日志配置

**默认配置**

Spring Boot默认帮我们配置好了日志

```java
    //记录器
    Logger logger = LoggerFactory.getLogger(getClass());
    @Test
    public void contextLoads() {
        //System.out.println();

        //日志的级别；
        //由低到高   trace<debug<info<warn<error
        //可以调整输出的日志级别；日志就只会在这个级别以以后的高级别生效
        logger.trace("这是trace日志...");
        logger.debug("这是debug日志...");
        //SpringBoot默认给我们使用的是info级别的，没有指定级别的就用SpringBoot默认规定的级别；root级别
        logger.info("这是info日志...");
        logger.warn("这是warn日志...");
        logger.error("这是error日志...");


    }
```

```xml
    日志输出格式：
        %d表示日期时间，
        %thread表示线程名，
        %-5level：级别从左显示5个字符宽度
        %logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
        %msg：日志消息，
        %n是换行符
    -->
    %d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
```

**Spring Boot修改日志的默认配置**

```properties
# 也可以指定一个包路径 logging.level.com.xxx=error
logging.level.root=error


#logging.path=
# 不指定路径在当前项目下生成springboot.log日志
# 可以指定完整的路径；
#logging.file=G:/springboot.log

# 在当前磁盘的根路径下创建spring文件夹和里面的log文件夹；使用 spring.log 作为默认文件
logging.path=/spring/log

#  在控制台输出的日志的格式
logging.pattern.console=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n
# 指定文件中日志输出的格式
logging.pattern.file=%d{yyyy-MM-dd} === [%thread] === %-5level === %logger{50} ==== %msg%n
```

| logging.file | logging.path | Example  | Description                        |
| ------------ | ------------ | -------- | ---------------------------------- |
| (none)       | (none)       |          | 只在控制台输出                     |
| 指定文件名   | (none)       | my.log   | 输出日志到my.log文件               |
| (none)       | 指定目录     | /var/log | 输出到指定目录的 spring.log 文件中 |

**指定配置**

给类路径下放上每个日志框架自己的配置文件即可；Spring Boot就不使用他默认配置的了

| Logging System          | Customization                                                |
| ----------------------- | ------------------------------------------------------------ |
| Logback                 | `logback-spring.xml`, `logback-spring.groovy`, `logback.xml` or `logback.groovy` |
| Log4j2                  | `log4j2-spring.xml` or `log4j2.xml`                          |
| JDK (Java Util Logging) | `logging.properties`                                         |

logback.xml：直接就被日志框架识别了；

**logback-spring.xml**：日志框架就不直接加载日志的配置项，由Spring Boot解析日志配置，可以使用Spring Boot的高级Profile功能

```xml
<springProfile name="staging">
    <!-- configuration to be enabled when the "staging" profile is active -->
      可以指定某段配置只在某个环境下生效
</springProfile>
```

如：

```xml
<appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <!--
        日志输出格式：
            %d表示日期时间，
            %thread表示线程名，
            %-5level：级别从左显示5个字符宽度
            %logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
            %msg：日志消息，
            %n是换行符
        -->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <springProfile name="dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ----> [%thread] ---> %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
            <springProfile name="!dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ==== [%thread] ==== %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
        </layout>
    </appender>
```

如果使用logback.xml作为日志配置文件，还要使用profile功能，会有以下错误

```
no applicable action for [springProfile]
```

## logback

因为logback是spring boot的默认日志框架，所以不需要引入maven依赖，直接上logback-spring.xml放在resources下面

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!--引入默认配置-->
    <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
    <!--web信息-->
    <logger name="org.springframework.web" level="info"/>

    <!--写入日志到控制台的appender,用默认的,但是要去掉charset,否则windows下tomcat下乱码-->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>${CONSOLE_LOG_PATTERN}</pattern>
        </encoder>
    </appender>

    <!--定义日志文件的存储地址 勿在 LogBack 的配置中使用相对路径-->
    <property name="LOG_PATH" value="F:\Program\Java\Practice_Projects\SpringBoot_Practices\springboot_exception\logs"/>
    <!--写入日志到文件的appender-->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志文件输出的文件名,每天一个文件-->
            <FileNamePattern>${LOG_PATH}.%d{yyyy-MM-dd}.log</FileNamePattern>
            <!--日志文件保留天数-->
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>
        <!--日志文件最大的大小-->
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <MaxFileSize>10MB</MaxFileSize>
        </triggeringPolicy>
    </appender>

    <!--异步到文件-->
    <appender name="asyncFileAppender" class="ch.qos.logback.classic.AsyncAppender">
        <discardingThreshold>0</discardingThreshold>
        <queueSize>500</queueSize>
        <appender-ref ref="FILE"/>
    </appender>

    <!--生产环境:打印控制台和输出到文件-->
    <springProfile name="prod">
        <root level="info">
            <appender-ref ref="CONSOLE"/>
            <appender-ref ref="asyncFileAppender"/>
        </root>
    </springProfile>

    <!--开发环境:打印控制台-->
    <springProfile name="dev">
        <!-- 打印sql -->
        <logger name="club.krislin" level="DEBUG"/>
        <root level="DEBUG">
            <appender-ref ref="CONSOLE"/>
        </root>
    </springProfile>

    <!--测试环境:打印控制台-->
    <springProfile name="test">
        <root level="info">
            <appender-ref ref="CONSOLE"/>
        </root>
    </springProfile>
</configuration>
```

**测试一下**

```java
@RestController
@Slf4j
public class LogDemoController {
    //private static Logger log= LoggerFactory.getLogger(LogDemo.class);
 
    @GetMapping("/logdemo")
    public String log(){
        log.trace("======trace");
        log.debug("======debug");
        log.info("======info");
        log.warn("======warn");
        log.error("======error");
        return "logok";
    }
}
```

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200524124536.png)

## log4j2


如要使用Log4j2，需要从spring-boot-starter-web中去掉spring-boot-starter-logging依赖，同时显示声明使用Log4j2的依赖jar包，具体如下:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

另外log4j是之前使用比较广泛的软件，容易与log4j2发生冲突，如果冲突将它从相应的软件里面排除掉,比如:dozer

```xml
<dependency>
    <groupId>net.sf.dozer</groupId>
    <artifactId>dozer</artifactId>
    <version>5.4.0</version>
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

**添加配置文件log4j2.xml**

在resources目录下新建一个log4j2.xml文件。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <Appenders>
        <Console name="CONSOLE" target="SYSTEM_OUT">
            <PatternLayout charset="UTF-8" pattern="[%-5p] %d %c - %m%n" />
        </Console>

        <RollingFile name="runtimeFile" fileName="./logs/boot-launch.log" filePattern="./logs/boot-launch-%d{yyyy-MM-dd}.log"
                     append="true">
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS Z}\t%level\t%class\t%line\t%thread\t%msg%n"/>
            <Policies>
                <TimeBasedTriggeringPolicy/>
            </Policies>
            <!-- 此行以下为自动清理日志的配置 -->
            <DefaultRolloverStrategy>
                <Delete basePath="./logs">
                    <!-- glob 项为需要自动清理日志的pattern -->
                    <IfFileName glob="*.log"/>
                    <!-- 30d 表示自动清理掉30天以前的日志文件 -->
                    <IfLastModified age="30d"/>
                </Delete>
            </DefaultRolloverStrategy>
            <!-- 此行以上为自动清理日志的配置 -->
        </RollingFile>


    </Appenders>

    <Loggers>
        <root level="info">
            <AppenderRef ref="CONSOLE" />
            <AppenderRef ref="runtimeFile" />
        </root>
    </Loggers>
</configuration>
```

注意：关于log4j2的定时删除如果filePattern的粒度为HH，那么在中如果age=30d则不生效

**修改application.yml配置**

但是这样还不够，Spring Boot并不知道log4j2.xml是干嘛的，需要通过在application.properties文件中显示声明才行

```yaml
logging:
    config: classpath:log4j2.xml
```

**测试**

```java
@RestController
@Slf4j
public class LogDemoController {
    //private static Logger log= LoggerFactory.getLogger(LogDemo.class);
 
    @GetMapping("/logdemo")
    public String log(){
        log.trace("======trace");
        log.debug("======debug");
        log.info("======info");
        log.warn("======warn");
        log.error("======error");
        return "logok";
    }
}
```

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200524125929.png)

## 拦截器实现统一访问日志

**一、定义访问日志内容记录实体类**

```java
@Data
public class AccessLog {
    //访问者用户名
    private String username;
    //请求路径
    private String url;
    //请求消耗时长
    private Integer duration;
    //http 方法：GET、POST等
    private String httpMethod;
    //http 请求响应状态码
    private Integer httpStatus;
    //访问者ip
    private String ip;
    //此条记录的创建时间
    private Date createTime;
}
```

**二、自定义日志拦截器**

```java
@Slf4j
public class AccessLogInterceptor implements HandlerInterceptor {
    //请求开始时间标识
    private static final String LOGGER_SEND_TIME = "SEND_TIME";
    //请求日志实体标识
    private static final String LOGGER_ACCESSLOG = "ACCESSLOG_ENTITY";

    /**
     * 进入SpringMVC的Controller之前开始记录日志实体
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object o) throws Exception {

        //创建日志实体
        AccessLog accessLog = new AccessLog();

        // 设置IP地址
        accessLog.setIp(AdrressIpUtils.getIpAdrress(request));

        //设置请求方法,GET,POST...
        accessLog.setHttpMethod(request.getMethod());

        //设置请求路径
        accessLog.setUrl(request.getRequestURI());

        //设置请求开始时间
        request.setAttribute(LOGGER_SEND_TIME,System.currentTimeMillis());

        //设置请求实体到request内，方便afterCompletion方法调用
        request.setAttribute(LOGGER_ACCESSLOG,accessLog);
        return true;
    }


    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object o, Exception e) throws Exception {

        //获取本次请求日志实体
        AccessLog accessLog = (AccessLog) request.getAttribute(LOGGER_ACCESSLOG);

        //获取请求错误码，根据需求存入数据库，这里不保存
        int status = response.getStatus();
        accessLog.setHttpStatus(status);

        //设置访问者(这里暂时写死）
        // 因为不同的应用可能将访问者信息放在session里面，有的通过request传递，
        // 总之可以获取到，但获取的方法不同
        accessLog.setUsername("admin");

        //当前时间
        long currentTime = System.currentTimeMillis();

        //请求开始时间
        long snedTime = Long.valueOf(request.getAttribute(LOGGER_SEND_TIME).toString());


        //设置请求时间差
        accessLog.setDuration(Integer.valueOf((currentTime - snedTime)+""));

        accessLog.setCreateTime(new Date());
        //将sysLog对象持久化保存
        log.info(accessLog.toString());
    }
}
```

**三、拦截器注册**

```java
@Configuration
public class MyWebMvcConfigurer implements WebMvcConfigurer {

    //设置排除路径，spring boot 2.*，注意排除掉静态资源的路径，不然静态资源无法访问
    private final String[] excludePath = {"/static"};

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new AccessLogInterceptor()).addPathPatterns("/**").excludePathPatterns(excludePath);
    }
}
```

**四、获取ip访问地址的工具类**

```java
public class AdrressIpUtils {
    /**
     * 获取Ip地址
     * @param request HttpServletRequest
     * @return ip
     */
    public static String getIpAdrress(HttpServletRequest request) {
        String Xip = request.getHeader("X-Real-IP");
        String XFor = request.getHeader("X-Forwarded-For");
        if(StringUtils.isNotEmpty(XFor) && !"unKnown".equalsIgnoreCase(XFor)){
            //多次反向代理后会有多个ip值，第一个ip才是真实ip
            int index = XFor.indexOf(",");
            if(index != -1){
                return XFor.substring(0,index);
            }else{
                return XFor;
            }
        }
        XFor = Xip;
        if(StringUtils.isNotEmpty(XFor) && !"unKnown".equalsIgnoreCase(XFor)){
            return XFor;
        }
        if (StringUtils.isBlank(XFor) || "unknown".equalsIgnoreCase(XFor)) {
            XFor = request.getHeader("Proxy-Client-IP");
        }
        if (StringUtils.isBlank(XFor) || "unknown".equalsIgnoreCase(XFor)) {
            XFor = request.getHeader("WL-Proxy-Client-IP");
        }
        if (StringUtils.isBlank(XFor) || "unknown".equalsIgnoreCase(XFor)) {
            XFor = request.getHeader("HTTP_CLIENT_IP");
        }
        if (StringUtils.isBlank(XFor) || "unknown".equalsIgnoreCase(XFor)) {
            XFor = request.getHeader("HTTP_X_FORWARDED_FOR");
        }
        if (StringUtils.isBlank(XFor) || "unknown".equalsIgnoreCase(XFor)) {
            XFor = request.getRemoteAddr();
        }
        return XFor;
    }
}
```



# 整合JDBC

## SpringData简介

对于数据访问层，无论是 SQL(关系型数据库) 还是 NOSQL(非关系型数据库)，Spring Boot 底层都是采用 Spring Data 的方式进行统一处理。

Spring Boot 底层都是采用 Spring Data 的方式进行统一处理各种数据库，Spring Data 也是 Spring 中与 Spring Boot、Spring Cloud 等齐名的知名项目。

Sping Data 官网：https://spring.io/projects/spring-data

数据库相关的启动器 ：可以参考官方文档：

https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter



## 整合JDBC

创建测试项目测试数据源

1、新建一个项目测试：springboot-data-jdbc ; 引入相应的模块！基础模块

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248739250-8a493a07-1fe0-465d-aaaa-49ec43c12ac5.png)

2、项目建好之后，发现自动帮我们导入了如下的启动器：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
```

3、编写yaml配置文件连接数据库；

```yaml
spring:
  datasource:
    username: root
    password: root
    #?serverTimezone=UTC解决时区的报错
    url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.jc.jdbc.Driver
    #新的驱动程序类不推荐使用 com.mysql.jdbc.Driver 驱动程序通过SPI自动注册，通常不需要手动加载驱动程序类。
```

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248810754-a587db9b-5284-40a8-8cdf-d0070934d679.png)

**注意：**

- 新的驱动程序类不推荐使用 com.mysql.jdbc.Driver
- 驱动程序通过SPI自动注册，通常不需要手动加载驱动程序类。

- 新的驱动程序类推荐使用 com.mysql.jc.jdbc.Driver 可以不配置，有默认配置

4、配置完这一些东西后，我们就可以直接去使用了，因为SpringBoot已经默认帮我们进行了自动配置；去测试类测试一下

```java
@SpringBootTest
class SpringbootDataJdbcApplicationTests {
    //DI注入数据源
    @Autowired
    DataSource dataSource;
    @Test
    public void contextLoads() throws SQLException {
        //看一下默认数据源
        System.out.println(dataSource.getClass());
        //获得连接
        Connection connection =   dataSource.getConnection();
        System.out.println(connection);
        //关闭连接
        connection.close();
    }
}
```

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248864691-0266f8df-d434-43b9-b4d7-5a79d5a4a674.png)

结果：我们可以看到他默认给我们配置的数据源为 : class com.zaxxer.hikari.HikariDataSource ， 我们并没有手动配置

我们来全局搜索一下，找到数据源的所有自动配置都在 ：DataSourceAutoConfiguration文件：

```java
@Import(
    {Hikari.class, Tomcat.class, Dbcp2.class, Generic.class, DataSourceJmxConfiguration.class}
)
protected static class PooledDataSourceConfiguration {
    protected PooledDataSourceConfiguration() {
    }
}
```

这里导入的类都在 DataSourceConfiguration 配置类下，可以看出 Spring Boot 2.2.5 默认使用HikariDataSource 数据源，而以前版本，如 Spring Boot 1.5 默认使用 org.apache.tomcat.jdbc.pool.DataSource 作为数据源；

HikariDataSource 号称 Java WEB 当前速度最快的数据源，相比于传统的 C3P0 、DBCP、Tomcat jdbc 等连接池更加优秀；

可以使用 spring.datasource.type 指定自定义的数据源类型，值为 要使用的连接池实现的完全限定名。

关于数据源我们并不做介绍，有了数据库连接，显然就可以 CRUD 操作数据库了。但是我们需要先了解一个对象 JdbcTemplate

**JDBCTemplate**

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248933551-2631d567-1d3c-414f-bbc8-194122485ab8.png)

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615248942481-fc28fc1e-c00e-485f-bd66-b0e262d9738c.png)

1、有了数据源(com.zaxxer.hikari.HikariDataSource)，然后可以拿到数据库连接(java.sql.Connection)，有了连接，就可以使用原生的 JDBC 语句来操作数据库；

2、即使不使用第三方第数据库操作框架，如 MyBatis等，Spring 本身也对原生的JDBC 做了轻量级的封装，即JdbcTemplate。

3、数据库操作的所有 CRUD 方法都在 JdbcTemplate 中。

4、Spring Boot 不仅提供了默认的数据源，同时默认已经配置好了 JdbcTemplate 放在了容器中，程序员只需自己注入即可使用

5、JdbcTemplate的自动配置是依赖 org.springframework.boot.autoconfigure.jdbc 包下的 JdbcTemplateConfiguration 类

**JdbcTemplate主要提供以下几类方法：**

- execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；

- update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批处理相关语句；

- query方法及queryForXXX方法：用于执行查询相关语句；

- call方法：用于执行存储过程、函数相关语句。

测试

编写一个Controller，注入 jdbcTemplate，编写测试方法进行访问测试；

```java
@RestController
@RequestMapping("/jdbc")
public class JdbcController {
    /**
     * Spring Boot 默认提供了数据源，默认提供了 org.springframework.jdbc.core.JdbcTemplate
     * JdbcTemplate 中会自己注入数据源，用于简化 JDBC操作
     * 还能避免一些常见的错误,使用起来也不用再自己来关闭数据库连接
     */
    @Autowired
    JdbcTemplate jdbcTemplate;
    //查询employee表中所有数据
    //List 中的1个 Map 对应数据库的 1行数据
    //Map 中的 key 对应数据库的字段名，value 对应数据库的字段值
    @GetMapping("/list")
    public List<Map<String, Object>> userList(){
        String sql = "select * from employee";
        List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
        return maps;
    }
   
    //新增一个用户
    @GetMapping("/add")
    public String addUser(){
        //插入语句，注意时间问题
        String sql = "insert into employee(last_name, email,gender,department,birth)" +
                " values ('狂神说','24736743@qq.com',1,101,'"+ new Date().toLocaleString() +"')";
        jdbcTemplate.update(sql);
        //查询
        return "addOk";
    }
    //修改用户信息
    @GetMapping("/update/{id}")
    public String updateUser(@PathVariable("id") int id){
        //插入语句
        String sql = "update employee set last_name=?,email=? where id="+id;
        //数据
        Object[] objects = new Object[2];
        objects[0] = "秦疆";
        objects[1] = "24736743@sina.com";
        jdbcTemplate.update(sql,objects);
        //查询
        return "updateOk";
    }
    //删除用户
    @GetMapping("/delete/{id}")
    public String delUser(@PathVariable("id") int id){
        //插入语句
        String sql = "delete from employee where id=?";
        jdbcTemplate.update(sql,id);
        //查询
        return "deleteOk";
    }
   
}
```

测试请求，结果正常；

到此，CURD的基本操作，使用 JDBC 就搞定了。

## 配置多数据源

**配置多个数据源**

application.yml配置2个数据源，第一个叫做primary，第二个叫做secondary。注意两个数据源连接的是不同的库，testdb和testdb2.

```java
spring:
  datasource:
    primary:
      jdbc-url: jdbc:mysql://localhost:3306/testdb?useUnicode=true&characterEncoding=utf-8
      username: root
      password: 123456
      driver-class-name: com.mysql.jdbc.Driver
    secondary:
      jdbc-url: jdbc:mysql://localhost:3306/testdb2?useUnicode=true&characterEncoding=utf-8
      username: root
      password: 123456
      driver-class-name: com.mysql.jdbc.Driver
```

**二、通过Java Config将数据源注入到Spring上下文。**

- primaryJdbcTemplate使用primaryDataSource数据源操作数据库testdb。
- secondaryJdbcTemplate使用secondaryDataSource数据源操作数据库testdb2。

```java
@Configuration
public class DataSourceConfig {
    @Primary
    @Bean(name = "primaryDataSource")
    @Qualifier("primaryDataSource")
    @ConfigurationProperties(prefix="spring.datasource.primary")
    public DataSource primaryDataSource() {
            return DataSourceBuilder.create().build();
    }

    @Bean(name = "secondaryDataSource")
    @Qualifier("secondaryDataSource")
    @ConfigurationProperties(prefix="spring.datasource.secondary")
    public DataSource secondaryDataSource() {
        return DataSourceBuilder.create().build();
    }

    @Bean(name="primaryJdbcTemplate")
    public JdbcTemplate primaryJdbcTemplate (
        @Qualifier("primaryDataSource") DataSource dataSource ) {
        return new JdbcTemplate(dataSource);
    }

    @Bean(name="secondaryJdbcTemplate")
    public JdbcTemplate secondaryJdbcTemplate(
            @Qualifier("secondaryDataSource") DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }
}
```

**操作**

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringJdbcTest {

    @Resource
    private ArticleJDBCDAO articleJDBCDAO;
    @Resource
    private JdbcTemplate primaryJdbcTemplate;
    @Resource
    private JdbcTemplate secondaryJdbcTemplate;


    @Test
    public void testJdbc() {
        articleJDBCDAO.save(
                Article.builder()
                .author("zimug").title("primaryJdbcTemplate").content("ceshi").createTime(new Date())
                .build(),
                primaryJdbcTemplate);
        articleJDBCDAO.save(
                Article.builder()
               .author("zimug").title("secondaryJdbcTemplate").content("ceshi").createTime(new Date())
               .build(),
                secondaryJdbcTemplate);
    }


}
```

## JTA实现分布式事务

> 分布式事务就是跨数据库连接的事务。一个事务的数据库操作，要么都成功，要么都失败回滚。后面我们专门做一个章节讲解事务与分布式事务。

JTA优点： 能够支持分布式事务
缺点： 性能开销大，不适合用于高并发场景

**引入maven依赖包**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jta-atomikos</artifactId>
</dependency>
```

**修改application配置文件**

双数据源配置。删掉原有数据库连接配置.

```yaml
primarydb:
  uniqueResourceName: primary
  xaDataSourceClassName: com.mysql.jdbc.jdbc2.optional.MysqlXADataSource
  xaProperties:
    url: jdbc:mysql://localhost:3306/testdb?useUnicode=true&characterEncoding=utf-8&useSSL=false
    user: test
    password: 4rfv$RFV
  exclusiveConnectionMode: true
  minPoolSize: 3
  maxPoolSize: 10
  testQuery: SELECT 1 from dual #由于采用HikiriCP，用于检测数据库连接是否存活。

secondarydb:
  uniqueResourceName: secondary
  xaDataSourceClassName: com.mysql.jdbc.jdbc2.optional.MysqlXADataSource
  xaProperties:
    url: jdbc:mysql://localhost:3306/testdb2?useUnicode=true&characterEncoding=utf-8&useSSL=false
    user: test
    password: 4rfv$RFV
  exclusiveConnectionMode: true
  minPoolSize: 3
  maxPoolSize: 10
  testQuery: SELECT 1 from dual #由于采用HikiriCP，用于检测数据库连接是否存活。
```

MysqlXADataSource的解释：XA数据源生成能够在全局/分布式事务中使用的XA连接。如果需要跨多个数据库或JMS调用的事务，则可能需要此类连接。

如果不使用分布式事务，则在声明驱动程序时无需指定xa-datasource-class。这个xa-datasource-class是专门为分布式事务准备的。

**数据源配置**

```java
@Configuration
public class DataSourceConfig {
     //jta数据源primarydb
    @Bean(initMethod="init", destroyMethod="close", name="primaryDataSource")
    @Primary
    @ConfigurationProperties(prefix = "primarydb")
    public DataSource primaryDataSource() {
         //这里是关键，返回的是AtomikosDataSourceBean，所有的配置属性也都是注入到这个类里面
        return new AtomikosDataSourceBean();
    }

    //jta数据源secondarydb
    @Bean(initMethod="init", destroyMethod="close", name="secondaryDataSource")
    @ConfigurationProperties(prefix = "secondarydb")
    public DataSource secondaryDataSource()  {
        return new AtomikosDataSourceBean();
    }

    //primaryJdbcTemplate使用primaryDataSource数据源
    @Bean
    public JdbcTemplate primaryJdbcTemplate(
                    @Qualifier("primaryDataSource") DataSource primaryDataSource) {
        return new JdbcTemplate(primaryDataSource);
    }

    //secondaryJdbcTemplate使用secondaryDataSource数据源
    @Bean
    public JdbcTemplate secondaryJdbcTemplate(
                    @Qualifier("secondaryDataSource") DataSource secondaryDataSource) {
        return new JdbcTemplate(secondaryDataSource);
    }
}
```

**事务管理器**

负责协调多个JTA数据源实现事务机制。UserTransaction 、TransactionManager 、JtaTransactionManager 都是帮我们实现JTA分布式事务的事务管理器。

```java
@Configuration
public class TransactionManagerConfig {

    @Bean
    public UserTransaction userTransaction() throws SystemException {
        UserTransactionImp userTransactionImp = new UserTransactionImp();
        userTransactionImp.setTransactionTimeout(10000);
        return userTransactionImp;
    }

    @Bean(name = "atomikosTransactionManager", initMethod = "init", destroyMethod = "close")
    public TransactionManager atomikosTransactionManager() throws Throwable {
        UserTransactionManager userTransactionManager = new UserTransactionManager();
        userTransactionManager.setForceShutdown(false);
        return userTransactionManager;
    }

    @Bean(name = "transactionManager")
    @DependsOn({ "userTransaction", "atomikosTransactionManager" })
    public PlatformTransactionManager transactionManager() throws Throwable {
        UserTransaction userTransaction = userTransaction();

        JtaTransactionManager manager = new JtaTransactionManager(userTransaction, atomikosTransactionManager());
        return manager;
    }
}
```



# 整合Druid

## Druid简介

Java程序很大一部分要操作数据库，为了提高性能操作数据库的时候，又不得不使用数据库连接池。

Druid 是阿里巴巴开源平台上一个数据库连接池实现，结合了 C3P0、DBCP 等 DB 池的优点，同时加入了日志监控。

Druid 可以很好的监控 DB 池连接和 SQL 的执行情况，天生就是针对监控而生的 DB 连接池。

Spring Boot 2.0 以上默认使用 Hikari 数据源，可以说 Hikari 与 Driud 都是当前 Java Web 上最优秀的数据源，我们来重点介绍 Spring Boot 如何集成 Druid 数据源，如何实现数据库监控。

Github地址：https://github.com/alibaba/druid/

**com.alibaba.druid.pool.DruidDataSource 基本配置参数如下：**

| 配置                          | 缺省值             | 说明                                                         |
| ----------------------------- | ------------------ | ------------------------------------------------------------ |
| name                          |                    | 配置这个属性的意义在于，如果存在多个数据源，监控的时候可以通过名字来区分开来。 如果没有配置，将会生成一个名字，格式是：“DataSource-” + System.identityHashCode(this) |
| jdbcUrl                       |                    | 连接数据库的url，不同数据库不一样。例如： mysql : jdbc:mysql://10.20.153.104:3306/druid2 oracle : jdbc:oracle:thin:@10.20.149.85:1521:ocnauto |
| username                      |                    | 连接数据库的用户名                                           |
| password                      |                    | 连接数据库的密码。如果你不希望密码直接写在配置文件中，可以使用ConfigFilter。详细看这里：https://github.com/alibaba/druid/wiki/%E4%BD%BF%E7%94%A8ConfigFilter |
| driverClassName               | 根据url自动识别    | 这一项可配可不配，如果不配置druid会根据url自动识别dbType，然后选择相应的driverClassName(建议配置下) |
| initialSize                   | 0                  | 初始化时建立物理连接的个数。初始化发生在显示调用init方法，或者第一次getConnection时 |
| maxActive                     | 8                  | 最大连接池数量                                               |
| maxIdle                       | 8                  | 已经不再使用，配置了也没效果                                 |
| minIdle                       |                    | 最小连接池数量                                               |
| maxWait                       |                    | 获取连接时最大等待时间，单位毫秒。配置了maxWait之后，缺省启用公平锁，并发效率会有所下降，如果需要可以通过配置useUnfairLock属性为true使用非公平锁。 |
| poolPreparedStatements        | false              | 是否缓存preparedStatement，也就是PSCache。PSCache对支持游标的数据库性能提升巨大，比如说oracle。在mysql下建议关闭。 |
| maxOpenPreparedStatements     | -1                 | 要启用PSCache，必须配置大于0，当大于0时，poolPreparedStatements自动触发修改为true。在Druid中，不会存在Oracle下PSCache占用内存过多的问题，可以把这个数值配置大一些，比如说100 |
| validationQuery               |                    | 用来检测连接是否有效的sql，要求是一个查询语句。如果validationQuery为null，testOnBorrow、testOnReturn、testWhileIdle都不会其作用。 |
| testOnBorrow                  | true               | 申请连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能。 |
| testOnReturn                  | false              | 归还连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能 |
| testWhileIdle                 | false              | 建议配置为true，不影响性能，并且保证安全性。申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效。 |
| timeBetweenEvictionRunsMillis |                    | 有两个含义： 1) Destroy线程会检测连接的间隔时间2) testWhileIdle的判断依据，详细看testWhileIdle属性的说明 |
| numTestsPerEvictionRun        |                    | 不再使用，一个DruidDataSource只支持一个EvictionRun           |
| minEvictableIdleTimeMillis    |                    |                                                              |
| connectionInitSqls            |                    | 物理连接初始化的时候执行的sql                                |
| exceptionSorter               | 根据dbType自动识别 | 当数据库抛出一些不可恢复的异常时，抛弃连接                   |
| filters                       |                    | 属性类型是字符串，通过别名的方式配置扩展插件，常用的插件有： 监控统计用的filter:stat日志用的filter:log4j防御sql注入的filter:wall |
| proxyFilters                  |                    | 类型是List<com.alibaba.druid.filter.Filter>，如果同时配置了filters和proxyFilters，是组合关系，并非替换关系 |



## 配置数据源

1、添加上 Druid 数据源依赖。

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.3</version>
</dependency>
```

2、切换数据源；之前已经说过 Spring Boot 2.0 以上默认使用 com.zaxxer.hikari.HikariDataSource 数据源，但可以 通过 spring.datasource.type 指定数据源。

```yaml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource # 自定义数据源
```

3、数据源切换之后，在测试类中注入 DataSource，然后获取到它，输出一看便知是否成功切换；

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615249552312-f3950a90-1bcc-4b90-8494-74924e2520d5.png)



4、切换成功！既然切换成功，就可以设置数据源连接初始化大小、最大连接数、等待时间、最小连接数 等设置项；可以查看源码

```yaml
spring:
  datasource:
    username: root
    password: 123456
    #?serverTimezone=UTC解决时区的报错
    url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    #Spring Boot 默认是不注入这些属性值的，需要自己绑定
    #druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

5、导入Log4j 的依赖

```xml
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

6、现在需要程序员自己为 DruidDataSource 绑定全局配置文件中的参数，再添加到容器中，而不再使用 Spring Boot 的自动生成了；我们需要 自己添加 DruidDataSource 组件到容器中，并绑定属性；

```java
@Configuration
public class DruidConfig {
    /*
       将自定义的 Druid数据源添加到容器中，不再让 Spring Boot 自动创建
       绑定全局配置文件中的 druid 数据源属性到 com.alibaba.druid.pool.DruidDataSource从而让它们生效
       @ConfigurationProperties(prefix = "spring.datasource")：作用就是将 全局配置文件中
       前缀为 spring.datasource的属性值注入到 com.alibaba.druid.pool.DruidDataSource 的同名参数中
     */
    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druidDataSource() {
        return new DruidDataSource();
    }
}
```

7、去测试类中测试一下；看是否成功！

```java
@SpringBootTest
class SpringbootDataJdbcApplicationTests {
    //DI注入数据源
    @Autowired
    DataSource dataSource;
    @Test
    public void contextLoads() throws SQLException {
        //看一下默认数据源
        System.out.println(dataSource.getClass());
        //获得连接
        Connection connection =   dataSource.getConnection();
        System.out.println(connection);
        DruidDataSource druidDataSource = (DruidDataSource) dataSource;
        System.out.println("druidDataSource 数据源最大连接数：" + druidDataSource.getMaxActive());
        System.out.println("druidDataSource 数据源初始化连接数：" + druidDataSource.getInitialSize());
        //关闭连接
        connection.close();
    }
}
```

输出结果 ：可见配置参数已经生效！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615249747225-f97cd7ad-ec4b-4b81-a635-ae2cca67cf01.png)

## 配置Druid数据源监控

Druid 数据源具有监控的功能，并提供了一个 web 界面方便用户查看，类似安装 路由器 时，人家也提供了一个默认的 web 页面。

所以第一步需要设置 Druid 的后台管理页面，比如 登录账号、密码 等；配置后台管理；

```java
//配置 Druid 监控管理后台的Servlet；
//因为springboot内置了Servlet 容器，所以没有web.xml文件，所以使用 SpringBoot 的注册 Servlet 方式
@Bean
public ServletRegistrationBean statViewServlet() {
    ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
    // 这些参数可以在 com.alibaba.druid.support.http.StatViewServlet 
    // 的父类 com.alibaba.druid.support.http.ResourceServlet 中找到
    Map<String, String> initParams = new HashMap<>();
    initParams.put("loginUsername", "admin"); //后台管理界面的登录账号
    initParams.put("loginPassword", "123456"); //后台管理界面的登录密码
    //后台允许谁可以访问
    //initParams.put("allow", "localhost")：表示只有本机可以访问
    //initParams.put("allow", "")：为空或者为null时，表示允许所有访问
    initParams.put("allow", "");
    //deny：Druid 后台拒绝谁访问
    //initParams.put("kuangshen", "192.168.1.20");表示禁止此ip访问
    //设置初始化参数
    bean.setInitParameters(initParams);
    return bean;
}
```

配置完毕后，我们可以选择访问 ：http://localhost:8080/druid

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615249833913-4b91dd65-824d-49bf-9c35-db6c61f96adb.png)

进入之后

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615249845229-c529e772-62af-4be1-92e5-bdbb92928638.png)

**配置 Druid web 监控 filter 过滤器**

```java
//配置 Druid 监控 之  web 监控的 filter
//WebStatFilter：用于配置Web和Druid数据源之间的管理关联监控统计
@Bean
public FilterRegistrationBean webStatFilter() {
    FilterRegistrationBean bean = new FilterRegistrationBean();
    bean.setFilter(new WebStatFilter());
    //exclusions：设置哪些请求进行过滤排除掉，从而不进行统计
    Map<String, String> initParams = new HashMap<>();
    initParams.put("exclusions", "*.js,*.css,/druid/*,/jdbc/*");
    bean.setInitParameters(initParams);
    //"/*" 表示过滤所有请求
    bean.setUrlPatterns(Arrays.asList("/*"));
    return bean;
}
```

平时在工作中，按需求进行配置即可，主要用作监控！

# 整合MyBatis

官方文档：http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/

Maven仓库地址：https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter/2.1.4

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615249918815-1192bee0-5186-40cd-8275-f21f3747266c.png)

**对比JPA**

| 对比项       | Spring Data JPA                                              | Mybatis                                                      |
| :----------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 单表操作方式 | 只需继承，代码量极少，非常方便。而且支持方法名用关键字生成SQL | 可以使用代码生成工具或Mybatis-Plus等工具，也很方便，但相对JPA要弱一些。 |
| 多表关联查询 | 不太友好，动态SQL使用不够方便，而且SQL和代码耦合到一起       | 友好，可以有非常直观的动态SQL                                |
| 自定义SQL    | SQL写在注解里面，写动态SQL有些费劲                           | SQL可以写在XML里面，是书写动态SQL语法利器。也支持注解SQL。   |
| 学习成本     | 略高                                                         | 较低 ,基本会写SQL就会用                                      |

## 整合测试

1、导入 MyBatis 所需要的依赖

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>
```

2、配置数据库连接信息（不变）

```yaml
spring:
  datasource:
    username: root
    password: 123456
    #?serverTimezone=UTC解决时区的报错
    url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    #Spring Boot 默认是不注入这些属性值的，需要自己绑定
    #druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

3、测试数据库是否连接成功！

4、创建实体类，导入 Lombok！

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
}
```

5、创建mapper目录以及对应的 Mapper 接口

```java
//@Mapper : 表示本类是一个 MyBatis 的 Mapper
@Mapper
@Repository
public interface UserMapper {
    List<User> queryUserList();
    User queryUserById(int id);
    int addUser(User user);
    int updateUser(User user);
    int deleteUser(int id);
}
```

6、对应的Mapper映射文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.mapper.DepartmentMapper">
    <select id="queryUserList" resultType="User">
        select * from user;
    </select>
    <select id="queryUserById" resultType="User" parameterType="int">
        select * from user where id = #{id};
    </select>
    <insert id="addUser" parameterType="User">
        insert into user(id,name,pwd) values(#{id},#{name},#{pwd});
    </insert>
    <update id="updateUser" parameterType="User">
        update user set name=#{name},pwd=#{pwd} where id=#{id};
    </update>
    <delete id="deleteUser" parameterType="int">
        delete from user where id = #{id};
    </delete>
</mapper>
```

7、maven配置资源过滤问题

```xml
<resources>
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.xml</include>
        </includes>
        <filtering>true</filtering>
    </resource>
</resources>
```

或者 在yaml中配置

```yaml
mybatis:
  type-aliases-package: com.kuang.pojo
  mapper-locations: classpath:/mybatis/mapper/*.xml
```

8、编写Controller 进行测试！

```java
@RestController
public class UserController {
@Autowired
    private UserMapper userMapper;
    @GetMapping("/queryUserList")
    public List<User>  queryUserList(){
        List<User> users = userMapper.queryUserList();
        for (User user : users) {
            System.out.println(user);
        }
        return users;
    }
    @GetMapping("/addUser")
    public String  addUser(){
       userMapper.addUser(new User(6,"阿毛","123456"));
        return "ok";
    }
    @GetMapping("/updateUser")
    public String  udpateUser(){
        userMapper.updateUser(new User(6,"阿毛","111111"));
        return "ok";
    }
    @GetMapping("/deleteUser")
    public String  deleteUser(){
        userMapper.deleteUser(5);
        return "ok";
    }
}
```

启动项目访问进行测试！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615250439789-76b5d8c2-8123-4d97-9ae7-e53d76889159.png)

## 多数据源

在application.yml配置双数据源，第一个数据源访问testdb库，第二个数据源访问testdb2库

```java
spring:
  datasource:
    primary:
      url: jdbc:mysql://localhost:3306/testdb?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
      username: root
      password: 123456
      driver-class-name: com.mysql.cj.jdbc.Driver
    secondary:
      url: jdbc:mysql://localhost:3306/testdb2?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
      username: root
      password: 123456
      driver-class-name: com.mysql.cj.jdbc.Driver
```

**主数据源配置**

去掉SpringBoot程序主入口上的@MapperScan注解，将注解移到下面的MyBatis专用配置类上方。 

DataSource数据源、SqlSessionFactory、TransactionManager事务管理器、SqlSessionTemplate依据不同的数据源分别配置。

第一组是primary，第二组是secondary。

```java
@Configuration
@MapperScan(basePackages = "club.krislin.testdb",    //数据源primary-testdb库接口存放目录
        sqlSessionTemplateRef = "primarySqlSessionTemplate")
public class PrimaryDataSourceConfig {

    @Bean(name = "primaryDataSource")
    @ConfigurationProperties(prefix = "spring.datasource.primary")   //数据源primary配置
    @Primary
    public DataSource testDataSource() {
        return DataSourceBuilder.create().build();
    }

    @Bean(name = "primarySqlSessionFactory")
    @Primary
    public SqlSessionFactory testSqlSessionFactory(
                        @Qualifier("primaryDataSource") DataSource dataSource) throws Exception {
        SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        bean.setDataSource(dataSource);
         //设置XML文件存放位置，如果参考上一篇Mybatis最佳实践，将xml和java放在同一目录下，这里不用配置
        //bean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mybatis/mapper/test1/*.xml"));
        return bean.getObject();
    }

    @Bean(name = "primaryTransactionManager")
    @Primary
    public DataSourceTransactionManager testTransactionManager(
                        @Qualifier("primaryDataSource") DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }

    @Bean(name = "primarySqlSessionTemplate")
    @Primary
    public SqlSessionTemplate testSqlSessionTemplate(
                        @Qualifier("primarySqlSessionFactory") SqlSessionFactory sqlSessionFactory) throws Exception {
        return new SqlSessionTemplate(sqlSessionFactory);
    }

}
```

**第二个数据源配置**

参照primary配置，书写第二份secondary数据源配置的代码。

```java
@Configuration
@MapperScan(basePackages = "club.krislin.testdb2",     //注意这里testdb2目录
        sqlSessionTemplateRef = "secondarySqlSessionTemplate")
public class SecondaryDataSourceConfig {

    @Bean(name = "secondaryDataSource")
    @ConfigurationProperties(prefix = "spring.datasource.secondary")    //注意这里secondary配置
    public DataSource testDataSource() {
        return DataSourceBuilder.create().build();
    }

    @Bean(name = "secondarySqlSessionFactory")
    public SqlSessionFactory testSqlSessionFactory(
                        @Qualifier("secondaryDataSource") DataSource dataSource) throws Exception {
        SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        bean.setDataSource(dataSource);
        //bean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mybatis/mapper/test1/*.xml"));
        return bean.getObject();
    }

    @Bean(name = "secondaryTransactionManager")
    public DataSourceTransactionManager testTransactionManager(
                        @Qualifier("secondaryDataSource") DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }

    @Bean(name = "secondarySqlSessionTemplate")
    public SqlSessionTemplate testSqlSessionTemplate(
                        @Qualifier("secondarySqlSessionFactory") SqlSessionFactory sqlSessionFactory) throws Exception {
        return new SqlSessionTemplate(sqlSessionFactory);
    }

}
```

# 集成SpringSecurity

## 安全简介

Spring 是一个非常流行和成功的 Java 应用开发框架。Spring Security 基于 Spring 框架，提供了一套 Web 应用安全性的完整解决方案。一般来说，Web 应用的安全性包括用户认证（Authentication）和用户授权（Authorization）两个部分。

对于上面提到的两种应用情景，Spring Security 框架都有很好的支持。在用户认证方面，Spring Security 框架支持主流的认证方式，包括 HTTP 基本认证、HTTP 表单验证、HTTP 摘要认证、OpenID 和 LDAP 等。在用户授权方面，Spring Security 提供了基于角色的访问控制和访问控制列表（Access Control List，ACL），可以对应用中的领域对象进行细粒度的控制。

## 实战测试

**实验环境搭建**

1、新建一个初始的springboot项目web模块，thymeleaf模块

2、导入静态资源

3、controller跳转！

```java
@Controller
public class RouterController {
   @RequestMapping({"/","/index"})
   public String index(){
       return "index";
  }
   @RequestMapping("/toLogin")
   public String toLogin(){
       return "views/login";
  }
   @RequestMapping("/level1/{id}")
   public String level1(@PathVariable("id") int id){
       return "views/level1/"+id;
  }
   @RequestMapping("/level2/{id}")
   public String level2(@PathVariable("id") int id){
       return "views/level2/"+id;
  }
   @RequestMapping("/level3/{id}")
   public String level3(@PathVariable("id") int id){
       return "views/level3/"+id;
  }
}
```

4、测试实验环境是否OK！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615250539796-55054887-5256-45b7-9a34-dfb9b31af9f0.png)

**认识SpringSecurity**

Spring Security 是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型，他可以实现强大的Web安全控制，对于安全控制，我们仅需要引入 spring-boot-starter-security 模块，进行少量的配置，即可实现强大的安全管理！

**记住几个类：**

- **WebSecurityConfigurerAdapter：自定义Security策略**

- **AuthenticationManagerBuilder：自定义认证策略**

- **@EnableWebSecurity：开启WebSecurity模式 @Enablexxxx 开启某个功能**

**Spring Security的两个主要目标是 “认证” 和 “授权”（访问控制）。**



**“认证”（Authentication）**

身份验证是关于验证您的凭据，如用户名/用户ID和密码，以验证您的身份。

身份验证通常通过用户名和密码完成，有时与身份验证因素结合使用。

**“授权” （Authorization）**

授权发生在系统成功验证您的身份后，最终会授予您访问资源（如信息，文件，数据库，资金，位置，几乎任何内容）的完全权限。

这个概念是通用的，而不是只在Spring Security 中存在。

**认证和授权**

目前，我们的测试环境，是谁都可以访问的，我们使用 Spring Security 增加上认证和授权的功能

1、引入 Spring Security 模块

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

2、编写 Spring Security 配置类

参考官网：https://spring.io/projects/spring-security

查看我们自己项目中的版本，找到对应的帮助文档：

https://docs.spring.io/spring-security/site/docs/5.3.0.RELEASE/reference/html5 #servlet-applications 8.16.4

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615250649147-22c1b0f6-f5f7-4721-bde2-1c4cf53cdb44.png)

3、编写基础配置类

```java
@EnableWebSecurity // 开启WebSecurity模式
public class SecurityConfig extends WebSecurityConfigurerAdapter {
   @Override
   protected void configure(HttpSecurity http) throws Exception {
     
  }
}
```

4、定制请求的授权规则

```java
@EnableWebSecurity // 开启WebSecurity模式
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    //链式编程
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //首页所有人可以访问，功能页只有对应有权限的人能访问
        //请求授权的规则
        http.authorizeRequests()
                .antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");
        //没有权限默认调到登录页面
            //   /login
        http.formLogin();

    }
}
```

5、测试一下：发现除了首页都进不去了！因为我们目前没有登录的角色，因为请求需要登录的角色拥有对应的权限才可以！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615250728236-94388a32-ed7d-43f7-88ae-8c4cf0aa84af.png)

6、在configure()方法中加入以下配置，开启自动配置的登录功能！

```java
// 开启自动配置的登录功能
// /login 请求来到登录页
// /login?error 重定向到这里表示登录失败
http.formLogin();
```

7、测试一下：发现，没有权限的时候，会跳转到登录的页面！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615250761935-9976e025-6c51-4909-afa6-f67e2b15d4d7.png)

8、查看刚才登录页的注释信息；

我们可以定义认证规则，重写configure(AuthenticationManagerBuilder auth)方法

```java
//定义认证规则
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
 
   //在内存中定义，也可以在jdbc中去拿....
   auth.inMemoryAuthentication()
          .withUser("kuangshen").password("123456").roles("vip2","vip3")
          .and()
          .withUser("root").password("123456").roles("vip1","vip2","vip3")
          .and()
          .withUser("guest").password("123456").roles("vip1","vip2");
}
```

9、测试，我们可以使用这些账号登录进行测试！发现会报错！

There is no PasswordEncoder mapped for the id “null”

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615250855955-dd3cc358-f42d-488e-b4b0-214cc9aa6989.png)![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615250865472-0836027a-1943-454f-9001-288c617e25eb.png)

10、原因，我们要将前端传过来的密码进行某种方式加密，否则就无法登录，修改代码

```java
//定义认证规则
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
   //在内存中定义，也可以在jdbc中去拿....
   //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
   //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
   //spring security 官方推荐的是使用bcrypt加密方式。
 
   auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
          .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
          .and()
          .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
          .and()
          .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2");
}
```

11、测试，发现，登录成功，并且每个角色只能访问自己认证下的规则！搞定

**权限控制和注销**

1、开启自动配置的注销的功能

```java
//定制请求的授权规则
@Override
protected void configure(HttpSecurity http) throws Exception {
   //....
   //开启自动配置的注销的功能
      // /logout 注销请求
   http.logout();
}
```

2、我们在前端，增加一个注销的按钮，index.html 导航栏中

```html
<a class="item" th:href="@{/logout}">
   <i class="address card icon"></i> 注销
</a>
```

3、我们可以去测试一下，登录成功后点击注销，发现注销完毕会跳转到登录页面！

4、但是，我们想让他注销成功后，依旧可以跳转到首页，该怎么处理呢？

```java
// .logoutSuccessUrl("/"); 注销成功来到首页
http.logout().logoutSuccessUrl("/");
```

5、测试，注销完毕后，发现跳转到首页OK

6、我们现在又来一个需求：用户没有登录的时候，导航栏上只显示登录按钮，用户登录之后，导航栏可以显示登录的用户信息及注销按钮！还有就是，比如kuangshen这个用户，它只有 vip2，vip3功能，那么登录则只显示这两个功能，而vip1的功能菜单不显示！这个就是真实的网站情况了！该如何做呢？

我们需要结合thymeleaf中的一些功能

sec：authorize=“isAuthenticated()”:是否认证登录！来显示不同的页面

**Maven依赖：**

```xml
<!-- https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-extras-springsecurity4 -->
<dependency>
   <groupId>org.thymeleaf.extras</groupId>
   <artifactId>thymeleaf-extras-springsecurity5</artifactId>
   <version>3.0.4.RELEASE</version>
</dependency>
```

7、修改我们的 前端页面

导入命名空间

```xml
xmlns:sec="http://www.thymeleaf.org/extras/spring-security"
```

修改导航栏，增加认证判断

```html
<!--登录注销-->
<div class="right menu">
   <!--如果未登录-->
   <div sec:authorize="!isAuthenticated()">
       <a class="item" th:href="@{/login}">
           <i class="address card icon"></i> 登录
       </a>
   </div>
   <!--如果已登录-->
   <div sec:authorize="isAuthenticated()">
       <a class="item">
           <i class="address card icon"></i>
          用户名：<span sec:authentication="principal.username"></span>
          角色：<span sec:authentication="principal.authorities"></span>
       </a>
   </div>
   <div sec:authorize="isAuthenticated()">
       <a class="item" th:href="@{/logout}">
           <i class="address card icon"></i> 注销
       </a>
   </div>
</div>
```

8、重启测试，我们可以登录试试看，登录成功后确实，显示了我们想要的页面；

9、如果注销404了，就是因为它默认防止csrf跨站请求伪造，因为会产生安全问题，我们可以将请求改为post表单提交，或者在spring security中关闭csrf功能；我们试试：在 配置中增加 http.csrf().disable();

```java
http.csrf().disable();//关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
http.logout().logoutSuccessUrl("/");
```

10、我们继续将下面的角色功能块认证完成！

```html
<!-- sec:authorize="hasRole('vip1')" -->
<div class="column" sec:authorize="hasRole('vip1')">
   <div class="ui raised segment">
       <div class="ui">
           <div class="content">
               <h5 class="content">Level 1</h5>
               <hr>
               <div><a th:href="@{/level1/1}"><i class="bullhorn icon"></i> Level-1-1</a></div>
               <div><a th:href="@{/level1/2}"><i class="bullhorn icon"></i> Level-1-2</a></div>
               <div><a th:href="@{/level1/3}"><i class="bullhorn icon"></i> Level-1-3</a></div>
           </div>
       </div>
   </div>
</div>
<div class="column" sec:authorize="hasRole('vip2')">
   <div class="ui raised segment">
       <div class="ui">
           <div class="content">
               <h5 class="content">Level 2</h5>
               <hr>
               <div><a th:href="@{/level2/1}"><i class="bullhorn icon"></i> Level-2-1</a></div>
               <div><a th:href="@{/level2/2}"><i class="bullhorn icon"></i> Level-2-2</a></div>
               <div><a th:href="@{/level2/3}"><i class="bullhorn icon"></i> Level-2-3</a></div>
           </div>
       </div>
   </div>
</div>
<div class="column" sec:authorize="hasRole('vip3')">
   <div class="ui raised segment">
       <div class="ui">
           <div class="content">
               <h5 class="content">Level 3</h5>
               <hr>
               <div><a th:href="@{/level3/1}"><i class="bullhorn icon"></i> Level-3-1</a></div>
               <div><a th:href="@{/level3/2}"><i class="bullhorn icon"></i> Level-3-2</a></div>
               <div><a th:href="@{/level3/3}"><i class="bullhorn icon"></i> Level-3-3</a></div>
           </div>
       </div>
   </div>
</div>
```

11、测试一下！

12、权限控制和注销搞定！

**记住我**

现在的情况，我们只要登录之后，关闭浏览器，再登录，就会让我们重新登录，但是很多网站的情况，就是有一个记住密码的功能，这个该如何实现呢？很简单

1、开启记住我功能

```java
//定制请求的授权规则
@Override
protected void configure(HttpSecurity http) throws Exception {
//。。。。。。。。。。。
   //记住我
   http.rememberMe();
}
```

2、我们再次启动项目测试一下，发现登录页多了一个记住我功能，我们登录之后关闭 浏览器，然后重新打开浏览器访问，发现用户依旧存在！

思考：如何实现的呢？其实非常简单

我们可以查看浏览器的cookie

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615251373998-f8aefa86-540a-4ccb-b5e3-fe6ec1872e23.png)

3、我们点击注销的时候，可以发现，spring security 帮我们自动删除了这个 cookie

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615251412428-e98f815a-ad60-409e-a0b6-f27ce6f7bb9b.png)

4、结论：登录成功后，将cookie发送给浏览器保存，以后登录带上这个cookie，只要通过检查就可以免登录了。如果点击注销，则会删除这个cookie，具体的原理我们在JavaWeb阶段都讲过了，这里就不在多说了！

**定制登录页**

现在这个登录页面都是spring security 默认的，怎么样可以使用我们自己写的Login界面呢？

1、在刚才的登录页配置后面指定 loginpage

```java
http.formLogin().loginPage("/toLogin");
```

2、然后前端也需要指向我们自己定义的 login请求

```html
<a class="item" th:href="@{/toLogin}">
   <i class="address card icon"></i> 登录
</a>
```

3、我们登录，需要将这些信息发送到哪里，我们也需要配置，login.html 配置提交请求及方式，方式必须为post:

在 loginPage()源码中的注释上有写明：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615251691936-3e682f93-60ed-4955-8905-199fcbc840a2.png)

```html
<form th:action="@{/login}" method="post">
   <div class="field">
       <label>Username</label>
       <div class="ui left icon input">
           <input type="text" placeholder="Username" name="username">
           <i class="user icon"></i>
       </div>
   </div>
   <div class="field">
       <label>Password</label>
       <div class="ui left icon input">
           <input type="password" name="password">
           <i class="lock icon"></i>
       </div>
   </div>
   <input type="submit" class="ui blue submit button"/>
</form>
```

4、这个请求提交上来，我们还需要验证处理，怎么做呢？我们可以查看formLogin()方法的源码！我们配置接收登录的用户名和密码的参数！

```java
http.formLogin()
  .usernameParameter("username")
  .passwordParameter("password")
  .loginPage("/toLogin")
  .loginProcessingUrl("/login"); // 登陆表单提交请求
```

5、在登录页增加记住我的多选框

```java
<input type="checkbox" name="remember"> 记住我
```

6、后端验证处理！

```java
//定制记住我的参数！
http.rememberMe().rememberMeParameter("remember");
```

7、测试，OK

## 完整配置代码

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
   //定制请求的授权规则
   @Override
   protected void configure(HttpSecurity http) throws Exception {
       http.authorizeRequests().antMatchers("/").permitAll()
      .antMatchers("/level1/**").hasRole("vip1")
      .antMatchers("/level2/**").hasRole("vip2")
      .antMatchers("/level3/**").hasRole("vip3");
       //开启自动配置的登录功能：如果没有权限，就会跳转到登录页面！
           // /login 请求来到登录页
           // /login?error 重定向到这里表示登录失败
       http.formLogin()
          .usernameParameter("username")
          .passwordParameter("password")
          .loginPage("/toLogin")
          .loginProcessingUrl("/login"); // 登陆表单提交请求
       //开启自动配置的注销的功能
           // /logout 注销请求
           // .logoutSuccessUrl("/"); 注销成功来到首页
       http.csrf().disable();//关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
       http.logout().logoutSuccessUrl("/");
       //记住我
       http.rememberMe().rememberMeParameter("remember");
  }
   //定义认证规则
   @Override
   protected void configure(AuthenticationManagerBuilder auth) throws Exception {
       //在内存中定义，也可以在jdbc中去拿....
       //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
       //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
       //spring security 官方推荐的是使用bcrypt加密方式。
       auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
              .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
              .and()
              .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
              .and()
              .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2");
  }
}
```

# 集成Shiro

## 简介

Apache Shiro是一个Java 的安全（权限）框架。

Shiro 可以非常容易的开发出足够好的应用，其不仅可以用在JavaSE环境，也可以用在JavaEE环境。

Shiro可以完成，认证，授权，加密，会话管理，Web集成，缓存等。

下载地址:http://shiro.apache.orgl

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615251707769-7b0e51fa-8931-4982-ab97-44266d275870.png)

**有哪些功能？**

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615251733093-3669aac3-9fbc-4ebc-aaea-cbb8af446180.png)



**Authentication**：身份认证 / 登录，验证用户是不是拥有相应的身份；

**Authorization**：授权，即权限验证，验证某个已认证的用户是否拥有某个权限；即判断用户是否能做事情，常见的如：验证某个用户是否拥有某个角色。或者细粒度的验证某个用户对某个资源是否具有某个权限；

**Session Management**：会话管理，即用户登录后就是一次会话，在没有退出之前，它的所有信息都在会话中；会话可以是普通 JavaSE 环境的，也可以是如 Web 环境的；

**Cryptography**：加密，保护数据的安全性，如密码加密存储到数据库，而不是明文存储；

**Web Support**：Web 支持，可以非常容易的集成到 Web 环境；

**Caching**：缓存，比如用户登录后，其用户信息、拥有的角色 / 权限不必每次去查，这样可以提高效率；

**Concurrency**：shiro 支持多线程应用的并发验证，即如在一个线程中开启另一个线程，能把权限自动传播过去；

**Testing**：提供测试支持；

**Run As**：允许一个用户假装为另一个用户（如果他们允许）的身份进行访问；

**Remember Me**：记住我，这个是非常常见的功能，即一次登录后，下次再来的话不用登录了。

**Shiro 架构（外部）**

从外部来看 Shiro ，即从应用程序角度的来观察如何使用 Shiro 完成工作。如下图：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615251834133-52c53e41-ccbb-4a41-acee-a4eeb2d50e11.png)



**Subject**：主体，代表了当前 “用户”，这个用户不一定是一个具体的人，与当前应用交互的任何东西都是 Subject，如网络爬虫，机器人等；即一个抽象概念；所有 Subject 都绑定到 SecurityManager，与 Subject 的所有交互都会委托给 SecurityManager；可以把 Subject 认为是一个门面；SecurityManager 才是实际的执行者；

**SecurityManage**r：安全管理器；即所有与安全有关的操作都会与 SecurityManager 交互；且它管理着所有 Subject；可以看出它是 Shiro 的核心，它负责与后边介绍的其他组件进行交互，如果学习过 SpringMVC，你可以把它看成 DispatcherServlet 前端控制器；

**Realm**：域，Shiro 从从 Realm 获取安全数据（如用户、角色、权限），就是说 SecurityManager 要验证用户身份，那么它需要从 Realm 获取相应的用户进行比较以确定用户身份是否合法；也需要从 Realm 得到用户相应的角色 / 权限进行验证用户是否能进行操作；可以把 Realm 看成 DataSource，即安全数据源。



也就是说对于我们而言，最简单的一个 Shiro 应用：

- 应用代码通过 Subject 来进行认证和授权，而 Subject 又委托给 SecurityManager；
- 我们需要给 Shiro 的 SecurityManager 注入 Realm，从而让 SecurityManager 能得到合法的用户及其权限进行判断。

- 从以上也可以看出，Shiro 不提供维护用户 / 权限，而是通过 Realm 让开发人员自己注入。



**Shiro架构（内部）**

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615251915571-5e8c7b82-63f6-4db3-a237-731209a3c841.png)

**Subject**：主体，可以看到主体可以是任何可以与应用交互的 “用户”；

**SecurityManager**：相当于 SpringMVC 中的 DispatcherServlet 或者 Struts2 中的 FilterDispatcher；是 Shiro 的心脏；所有具体的交互都通过 SecurityManager 进行控制；它管理着所有 Subject、且负责进行认证和授权、及会话、缓存的管理。

**Authenticator**：认证器，负责主体认证的，这是一个扩展点，如果用户觉得 Shiro 默认的不好，可以自定义实现；其需要认证策略（Authentication Strategy），即什么情况下算用户认证通过了；

**Authrizer**：授权器，或者访问控制器，用来决定主体是否有权限进行相应的操作；即控制着用户能访问应用中的哪些功能；

**Realm**：可以有 1 个或多个 Realm，可以认为是安全实体数据源，即用于获取安全实体的；可以是 JDBC 实现，也可以是 LDAP 实现，或者内存实现等等；由用户提供；注意：Shiro 不知道你的用户 / 权限存储在哪及以何种格式存储；所以我们一般在应用中都需要实现自己的 Realm；

**SessionManager**：如果写过 Servlet 就应该知道 Session 的概念，Session 呢需要有人去管理它的生命周期，这个组件就是 SessionManager；而 Shiro 并不仅仅可以用在 Web 环境，也可以用在如普通的 JavaSE 环境、EJB 等环境；所以呢，Shiro 就抽象了一个自己的 Session 来管理主体与应用之间交互的数据；这样的话，比如我们在 Web 环境用，刚开始是一台 Web 服务器；接着又上了台 EJB 服务器；这时想把两台服务器的会话数据放到一个地方，这个时候就可以实现自己的分布式会话（如把数据放到 Memcached 服务器）；

**SessionDAO：**DAO 大家都用过，数据访问对象，用于会话的 CRUD，比如我们想把 Session 保存到数据库，那么可以实现自己的 SessionDAO，通过如 JDBC 写到数据库；比如想把 Session 放到 Memcached 中，可以实现自己的 Memcached SessionDAO；另外 SessionDAO 中可以使用 Cache 进行缓存，以提高性能；

**CacheManager**：缓存控制器，来管理如用户、角色、权限等的缓存的；因为这些数据基本上很少去改变，放到缓存中后可以提高访问的性能

**Cryptography**：密码模块，Shiro 提高了一些常见的加密组件用于如密码加密 / 解密的。



## 快速开始

查看官网文档：http://shiro.apache.org/tutorial.html

官方的quickstart：https://github.com/apache/shiro/tree/master/samples/quickstart

- 创建一个maven父工程，用于学习shiro，删掉src
- 创建一个普通的maven子工程：shiro-01-helloworld

- 根据官方文档，导入Shiro的依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-core</artifactId>
    </dependency>
    <!-- configure logging -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>jcl-over-slf4j</artifactId>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

**更改细节**

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-core</artifactId>
        <version>1.4.1</version>
    </dependency>
    <!-- configure logging -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>jcl-over-slf4j</artifactId>
        <version>1.7.21</version>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.7.21</version>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615252066063-b19c1664-3bf0-4ee3-9041-f4014def1f7f.png)

**log4j.properties**

```xml
log4j.rootLogger=INFO, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m %n
# General Apache libraries
log4j.logger.org.apache=WARN
# Spring
log4j.logger.org.springframework=WARN
# Default Shiro logging
log4j.logger.org.apache.shiro=INFO
# Disable verbose logging
log4j.logger.org.apache.shiro.util.ThreadContext=WARN
log4j.logger.org.apache.shiro.cache.ehcache.EhCache=WARN
```

**shiro.ini**

```bash
[users]
# user 'root' with password 'secret' and the 'admin' role
root = secret, admin
# user 'guest' with the password 'guest' and the 'guest' role
guest = guest, guest
# user 'presidentskroob' with password '12345' ("That's the same combination on
# my luggage!!!" ;)), and role 'president'
presidentskroob = 12345, president
# user 'darkhelmet' with password 'ludicrousspeed' and roles 'darklord' and 'schwartz'
darkhelmet = ludicrousspeed, darklord, schwartz
# user 'lonestarr' with password 'vespa' and roles 'goodguy' and 'schwartz'
lonestarr = vespa, goodguy, schwartz
# -----------------------------------------------------------------------------
# Roles with assigned permissions
# 
# Each line conforms to the format defined in the
# org.apache.shiro.realm.text.TextConfigurationRealm#setRoleDefinitions JavaDoc
# -----------------------------------------------------------------------------
[roles]
# 'admin' role has all permissions, indicated by the wildcard '*'
admin = *
# The 'schwartz' role can do anything (*) with any lightsaber:
schwartz = lightsaber:*
# The 'goodguy' role is allowed to 'drive' (action) the winnebago (type) with
# license plate 'eagle5' (instance specific id)
goodguy = winnebago:drive:eagle5
```

**Quickstart.java**

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615252203827-40a58980-fb02-4087-ae57-d5b363d53915.png)

```java
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.*;
import org.apache.shiro.ini.IniSecurityManagerFactory;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.session.Session;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.lang.util.Factory;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
/**
 * Simple Quickstart application showing how to use Shiro's API.
 *
 * @since 0.9 RC2
 */
public class Quickstart {
    private static final transient Logger log = LoggerFactory.getLogger(Quickstart.class);
    public static void main(String[] args) {
        // The easiest way to create a Shiro SecurityManager with configured
        // realms, users, roles and permissions is to use the simple INI config.
        // We'll do that by using a factory that can ingest a .ini file and
        // return a SecurityManager instance:
        // Use the shiro.ini file at the root of the classpath
        // (file: and url: prefixes load from files and urls respectively):
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        SecurityManager securityManager = factory.getInstance();
        // for this simple example quickstart, make the SecurityManager
        // accessible as a JVM singleton.  Most applications wouldn't do this
        // and instead rely on their container configuration or web.xml for
        // webapps.  That is outside the scope of this simple quickstart, so
        // we'll just do the bare minimum so you can continue to get a feel
        // for things.
        SecurityUtils.setSecurityManager(securityManager);
        // Now that a simple Shiro environment is set up, let's see what you can do:
        // get the currently executing user:
        Subject currentUser = SecurityUtils.getSubject();
        // Do some stuff with a Session (no need for a web or EJB container!!!)
        Session session = currentUser.getSession();
        session.setAttribute("someKey", "aValue");
        String value = (String) session.getAttribute("someKey");
        if (value.equals("aValue")) {
            log.info("Retrieved the correct value! [" + value + "]");
        }
        // let's login the current user so we can check against roles and permissions:
        if (!currentUser.isAuthenticated()) {
            UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
            token.setRememberMe(true);
            try {
                currentUser.login(token);
            } catch (UnknownAccountException uae) {
                log.info("There is no user with username of " + token.getPrincipal());
            } catch (IncorrectCredentialsException ice) {
                log.info("Password for account " + token.getPrincipal() + " was incorrect!");
            } catch (LockedAccountException lae) {
                log.info("The account for username " + token.getPrincipal() + " is locked.  " +
                        "Please contact your administrator to unlock it.");
            }
            // ... catch more exceptions here (maybe custom ones specific to your application?
            catch (AuthenticationException ae) {
                //unexpected condition?  error?
            }
        }
        //say who they are:
        //print their identifying principal (in this case, a username):
        log.info("User [" + currentUser.getPrincipal() + "] logged in successfully.");
        //test a role:
        if (currentUser.hasRole("schwartz")) {
            log.info("May the Schwartz be with you!");
        } else {
            log.info("Hello, mere mortal.");
        }
        //test a typed permission (not instance-level)
        if (currentUser.isPermitted("lightsaber:wield")) {
            log.info("You may use a lightsaber ring.  Use it wisely.");
        } else {
            log.info("Sorry, lightsaber rings are for schwartz masters only.");
        }
        //a (very powerful) Instance Level permission:
        if (currentUser.isPermitted("winnebago:drive:eagle5")) {
            log.info("You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  " +
                    "Here are the keys - have fun!");
        } else {
            log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
        }
        //all done - log out!
        currentUser.logout();
        System.exit(0);
    }
}
```

## SpringBoot整合Shiro环境搭建

新建SpringBoot项目,勾选web和thymeleaf

保持项目整洁,删除

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615252363544-19b9aa04-4db1-4135-b731-32e0059695a0.png)



templates下新建 index.html

```html
<!DOCTYPE html>
<html lang="en"
      xmlns:th="https://www.thymeleaf.org"
      xmlns:shiro="https://www.thymeleaf.org/thymeleaf-extras-shiro">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<h1>首页</h1>

<p th:text="${msg}"></p>

</body>
</html>
```

**controller包下新建MyController**

```java
@Controller
public class MyController {
    @RequestMapping({"/","/index"})
    public String toIndex(Model model){
        model.addAttribute("msg","hello,shiro");
        return "index";
    }
}
```

**测试**

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615252441545-20c1c17a-4c6c-4fa6-9565-48516cdbd27a.png)

**Subject用户**

**SecurityManager管理所有用户**

**Realm连接数据**

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615252500137-e96d6f79-5314-4077-97ba-221d2dc97ec3.png)

```xml
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-spring-boot-web-starter</artifactId>
</dependency>
```

发现并不好使更换为

```xml
<!--shiro-->
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-spring</artifactId>
    <version>1.4.1</version>
</dependency>
```



## shiro整合mybatis

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.19</version>
</dependency>
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.12</version>
</dependency>
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.1</version>
</dependency>
```

**编写application.yaml**

```yaml
spring:
  datasource:
    username: root
    password: jia5211314
    url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=UTF-8&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    #Spring Boot 默认是不注入这些属性值的，需要自己绑定
    #druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
mybatis:
  type-aliases-package: com.huang.pojo
  mapper-locations: classpath:mybatis/mapper/*.xml
```

编写实体类

导入lombok

User

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
    private String perms;
}
```

**mapper包编写UserMapper**

```java
@Mapper//这个注解表示了这是一个mybatis的mapper类
@Repository
public interface UserMapper {
    List<User> queryUserList();
    User queryUserById(int id);
    int addUser(User user);
    int updateUser(User user);
    int deleteUser(int id);
}
```

resource包下新建mybatis包下mapper

UserMapper.xml

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.huang.mapper.UserMapper">
    <select id="queryUserList" resultType="User">
       select * from user;
    </select>
    <select id="queryUserByName" resultType="User" parameterType="String">
        select * from user where name=#{name}
    </select>
</mapper>
```

service层

UserService接口

```java
import com.huang.pojo.User;
public interface UserService {
    User queryUserByName(String name);
}
```

**UserServiceImpl**

```java
@Service
public class UserServiceImpl implements UserService{
    @Autowired
    UserMapper userMapper;
    @Override
    public User queryUserByName(String name) {
        return userMapper.queryUserByName(name);
    }
}
```

ShiroSpringbootApplicationTests中进行测试

```java
import com.huang.service.UserServiceImpl;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
@SpringBootTest
class ShiroSpringbootApplicationTests {
    @Autowired
    UserServiceImpl userService;
    @Test
    void contextLoads() {
        System.out.println(userService.queryUserByName("张三"));
    }
}
```

**测试成功**

## Shiro整合Thymeleaf

```xml
<!--shiro-->
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-spring</artifactId>
    <version>1.4.1</version>
</dependency>
```

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615252856833-0a180578-bb69-44ba-82f0-1c05ea1c380d.png)

**ShiroConfig**

```java
@Configuration
public class ShiroConfig {
    //ShiroFilterBean
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("getDefaultWebSecurityManager") DefaultWebSecurityManager defaultWebSecurityManager){
        ShiroFilterFactoryBean bean=new ShiroFilterFactoryBean();
        //设置安全管理器
        bean.setSecurityManager(defaultWebSecurityManager);
        //添加shiro的内置过滤器
        /*
            anon:无需认证就能访问
            authc:必须认证才能访问
            user:必须拥有记住我功能才能访问
            perms:拥有某个资源的权限才能访问
            role:拥有某个角色权限才能访问
         */
        //拦截
        Map<String,String> filterMap =new LinkedHashMap<>();
        //授权
        filterMap.put("/user/add","perms[user:add]");
        filterMap.put("/user/update","perms[user:update]");
        //filterMap.put("/user/add","authc");
        //filterMap.put("/user/update","authc");
        //设置登陆的请求
        bean.setLoginUrl("/toLogin");
        //设置未授权的请求
        bean.setUnauthorizedUrl("/noauth");
        bean.setFilterChainDefinitionMap(filterMap);
        return bean;
    }
    //DefaultWebSecurityManager
    @Bean
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
        DefaultWebSecurityManager securityManager=new DefaultWebSecurityManager();
        //关联UserRealm
        securityManager.setRealm(userRealm);
        return securityManager;
    }
    //创建realm对象
    @Bean
    public UserRealm userRealm(){
        return new UserRealm();
    }
    //整合shiroDialect:用来整合shiro thymeleaf
    @Bean
    public ShiroDialect getShiroDialect(){
        return new ShiroDialect();
    }
}
```



**UserRealm**

```java
public class UserRealm extends AuthorizingRealm {
    @Autowired
    UserService userService;
    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        System.out.println("执行了授权");
        SimpleAuthorizationInfo info=new SimpleAuthorizationInfo();
        //info.addStringPermission("user:add");
        //拿到当前用户登陆对象
        Subject subject= SecurityUtils.getSubject();
        User currentUser= (User) subject.getPrincipal();//拿到User对象
        info.addStringPermission(currentUser.getPerms());//设置当前用户对象
        return info;
    }
    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        System.out.println("执行了认证");
        //用户名，密码，数据库中获取
        UsernamePasswordToken userToken=(UsernamePasswordToken) authenticationToken;
        User user=userService.queryUserByName(userToken.getUsername());//获取用户名
        String name=user.getName();
        String password=user.getPwd();
        if(user==null){//说明查无此人
            return null;
        }
        //密码认证,shiro做
        return new SimpleAuthenticationInfo(user,password,"");//放入User对象
    }
}
```

**MyController**

```java
package com.huang.controller;
import org.apache.catalina.security.SecurityUtil;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.subject.Subject;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import javax.servlet.http.HttpSession;
@Controller
public class MyController {
    @RequestMapping({"/","/index"})
    public String toIndex(Model model){
        model.addAttribute("msg","hello,shiro");
        return "index";
    }
    @RequestMapping("/user/add")
    public String add(){
        return "user/add";
    }
    @RequestMapping("/user/update")
    public String update(){
        return "user/update";
    }
    @RequestMapping("/toLogin")
    public String toLogin(){
        return "login";
    }
    @RequestMapping("/login")
    public String login(String username, String password, Model model, HttpSession session){
        //获取当前用户
        Subject subject= SecurityUtils.getSubject();
        //封装用户的登陆数据
        UsernamePasswordToken token=new UsernamePasswordToken(username,password);
        try{
            subject.login(token);//执行登陆的方法
            session.setAttribute("loginUser",username);//设置session
            return "index";
        }catch (UnknownAccountException e){//用户名不存在
            model.addAttribute("msg","用户名错误");
            return "login";
        }catch (IncorrectCredentialsException e){//密码不存在
            model.addAttribute("msg","密码错误");
            return "login";
        }
    }
    @RequestMapping("/noauth")
    @ResponseBody
    public String unauthorized(){
        return "未经授权禁止访问";
    }
}
```

**UserMapper**

```java
@Mapper
@Repository
public interface UserMapper {
    User queryUserByName(String name);
    List<User> queryUserList();
    User queryUserById(int id);
    int addUser(User user);
    int updateUser(User user);
    int deleteUser(int id);
}
```

**pojo**

**user**

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
    private String perms;
}
```

**UserService**

```java
public interface UserService {
    User queryUserByName(String name);
}
```

**UserServiceImpl**

```java
@Service
public class UserServiceImpl implements UserService{
    @Autowired
    UserMapper userMapper;
    @Override
    public User queryUserByName(String name) {
        return userMapper.queryUserByName(name);
    }
}
```

**UserMapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.huang.mapper.UserMapper">
    <select id="queryUserList" resultType="User">
       select * from user;
    </select>
    <select id="queryUserByName" resultType="User" parameterType="String">
        select * from user where name=#{name}
    </select>
</mapper>
```

**add.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>添加一个用户</h1>
</body>
</html>
-----------------------------------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>修改一个用户</h1>
</body>
</html>
-----------------------------------------------
<!DOCTYPE html>
<html lang="en"
      xmlns:th="https://www.thymeleaf.org"
      xmlns:shiro="https://www.thymeleaf.org/thymeleaf-extras-shiro">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>首页</h1>
<p th:text="${msg}"></p>
<hr>
<div th:if="${session.loginUser==null}">
    <a th:href="@{/toLogin}">登陆</a>
</div>
<div shiro:hasPermission="user:add">
    <a th:href="@{/user/add}">add</a>
</div>
<div shiro:hasPermission="user:update">
    <a th:href="@{/user/update}">update</a>
</div>
</body>
</html>
-----------------------------------------------
<!DOCTYPE html>
<html lang="en" xmlns:th="https://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>登陆</h1>
<hr>
<p style="color: red" th:text="${msg}"></p>
<form th:action="@{/login}">
    <p>用户名：<input type="text" name="username"></p>
    <p>密码：<input type="password" name="password"></p>
    <p><input type="submit"></p>
</form>
</body>
</html>
```

**application.yaml**

```yaml
spring:
  datasource:
    username: root
    password: jia5211314
    url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=UTF-8&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    #Spring Boot 默认是不注入这些属性值的，需要自己绑定
    #druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
mybatis:
  type-aliases-package: com.huang.pojo
  mapper-locations: classpath:mybatis/mapper/*.xml
```

**pom.xml**

```xml
<dependencies>
    <!-- https://mvnrepository.com/artifact/com.github.theborakompanioni/thymeleaf-extras-shiro -->
    <dependency>
        <groupId>com.github.theborakompanioni</groupId>
        <artifactId>thymeleaf-extras-shiro</artifactId>
        <version>2.0.0</version>
    </dependency>
    <!--
        Subject 用户
        SecurityManager 管理所有用户
        Realm 连接数据
        -->
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.19</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.12</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.1.1</version>
    </dependency>
    <!--lombok-->
    <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.10</version>
        <scope>provided</scope>
    </dependency>
    <!--shiro-->
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-spring</artifactId>
        <version>1.4.1</version>
    </dependency>
    <!--thymeleaf模板-->
    <dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring5</artifactId>
    </dependency>
    <dependency>
        <groupId>org.thymeleaf.extras</groupId>
        <artifactId>thymeleaf-extras-java8time</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

# 集成Swagger终极版

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615302133870-a008108b-e651-4bdf-9461-b893e989bc19.png)

## Swagger简介

**前后端分离**

- 前端 -> 前端控制层、视图层
- 后端 -> 后端控制层、服务层、数据访问层

- 前后端通过API进行交互
- 前后端相对独立且松耦合

**产生的问题**

前后端集成，前端或者后端无法做到“及时协商，尽早解决”，最终导致问题集中爆发

**解决方案**

首先定义schema [ 计划的提纲 ]，并实时跟踪最新的API，降低集成风险

早些年：制定Word计划文档

前后端分离：

- 前端测试后端接口：postman
- 后端提供接口，需要实时更新最新的消息及改动！

**Swagger**

号称世界上最流行的API框架

Restful Api 文档在线自动生成器 => API 文档 与API 定义同步更新

直接运行，在线测试API

支持多种语言 （如：Java，PHP等）

官网：https://swagger.io/

## SpringBoot集成Swagger

SpringBoot集成Swagger => springfox，两个jar包

- Springfox-swagger2
- swagger-springmvc

**使用Swagger**

要求：jdk 1.8 + 否则swagger2无法运行

步骤：

1、新建一个SpringBoot-web项目

2、添加Maven依赖

```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
   <groupId>io.springfox</groupId>
   <artifactId>springfox-swagger2</artifactId>
   <version>2.9.2</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
   <groupId>io.springfox</groupId>
   <artifactId>springfox-swagger-ui</artifactId>
   <version>2.9.2</version>
</dependency>
```

3、编写HelloController，测试确保运行成功！

```java
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String hello(){
        return  "hello";
    }
}
```

4、要使用Swagger，我们需要编写一个配置类-SwaggerConfig来配置 Swagger

```java
@Configuration //配置类
@EnableSwagger2// 开启Swagger2的自动配置
public class SwaggerConfig {  
}
```

5、访问测试 ：http://localhost:8080/swagger-ui.html ，可以看到swagger的界面；

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615302585774-2435bf8d-f54d-41fd-a5e8-b3f6b0e3c98d.png)



## 配置Swagger

1、Swagger实例Bean是Docket，所以通过配置Docket实例来配置Swaggger。

```java
@Bean //配置docket以配置Swagger具体参数
public Docket docket() {
   return new Docket(DocumentationType.SWAGGER_2);
}
```

2、可以通过apiInfo()属性配置文档信息

```java
//配置文档信息
private ApiInfo apiInfo() {
   Contact contact = new Contact("联系人名字", "http://xxx.xxx.com/联系人访问链接", "联系人邮箱");
   return new ApiInfo(
           "Swagger学习", // 标题
           "学习演示如何配置Swagger", // 描述
           "v1.0", // 版本
           "http://terms.service.url/组织链接", // 组织链接
           contact, // 联系人信息
           "Apach 2.0 许可", // 许可
           "许可链接", // 许可连接
           new ArrayList<>()// 扩展
  );
}
```

3、Docket 实例关联上 apiInfo()

```java
@Bean
public Docket docket() {
   return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo());
}
```

4、重启项目，访问测试 http://localhost:8080/swagger-ui.html 看下效果；

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615302785523-1e2b8846-b1e7-4571-a9aa-0faafbd231f8.png)



## 配置扫描接口

1、构建Docket时通过select()方法配置怎么扫描接口。

```java
@Bean
public Docket docket() {
   return new Docket(DocumentationType.SWAGGER_2)
      .apiInfo(apiInfo())
      .select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
      .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
      .build();
}
```

2、重启项目测试，由于我们配置根据包的路径扫描接口，所以我们只能看到一个类

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615302844227-fdd1d16b-c240-4467-8742-3cbbfcf21778.png)

3、除了通过包路径配置扫描接口外，还可以通过配置其他方式扫描接口，这里注释一下所有的配置方式：

```java
any() // 扫描所有，项目中的所有接口都会被扫描到
none() // 不扫描接口
// 通过方法上的注解扫描，如withMethodAnnotation(GetMapping.class)只扫描get请求
withMethodAnnotation(final Class<? extends Annotation> annotation)
// 通过类上的注解扫描，如.withClassAnnotation(Controller.class)只扫描有controller注解的类中的接口
withClassAnnotation(final Class<? extends Annotation> annotation)
basePackage(final String basePackage) // 根据包路径扫描接口
```

4、除此之外，我们还可以配置接口扫描过滤：

```java
@Bean
public Docket docket() {
   return new Docket(DocumentationType.SWAGGER_2)
      .apiInfo(apiInfo())
      .select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
      .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
       // 配置如何通过path过滤,即这里只扫描请求以/kuang开头的接口
      .paths(PathSelectors.ant("/kuang/**"))
      .build();
}
```

5、这里的可选值还有

```java
any() // 任何请求都扫描
none() // 任何请求都不扫描
regex(final String pathRegex) // 通过正则表达式控制
ant(final String antPattern) // 通过ant()控制
```

我只希望我的swagger在生产环境中使用，在发布的时候不适用？

- 判断是不是生产环境
- 注入enable（false）

## 配置Swagger开关

1、通过enable()方法配置是否启用swagger，如果是false，swagger将不能在浏览器中访问了

```java
@Bean
public Docket docket() {
   return new Docket(DocumentationType.SWAGGER_2)
      .apiInfo(apiInfo())
      .enable(false) //配置是否启用Swagger，如果是false，在浏览器将无法访问
      .select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
      .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
       // 配置如何通过path过滤,即这里只扫描请求以/kuang开头的接口
      .paths(PathSelectors.ant("/kuang/**"))
      .build();
}
```

2、如何动态配置当项目处于test、dev环境时显示swagger，处于prod时不显示？

```java
@Bean
public Docket docket(Environment environment) {
   // 设置要显示swagger的环境
   Profiles of = Profiles.of("dev", "test");
   // 通过environment.acceptsProfiles判断当前是否处于自己设定的环境中
   // 通过 enable() 接收此参数判断是否要显示
   boolean b = environment.acceptsProfiles(of);
   
   return new Docket(DocumentationType.SWAGGER_2)
      .apiInfo(apiInfo())
      .enable(b) //配置是否启用Swagger，如果是false，在浏览器将无法访问
      .select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors配置如何扫描接口
      .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"))
       // 配置如何通过path过滤,即这里只扫描请求以/kuang开头的接口
      .paths(PathSelectors.ant("/kuang/**"))
      .build();
}
```

3、可以在项目中增加一个dev的配置文件查看效果！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615303355322-8176d42c-85a7-4e47-869b-1ac53fa5fc23.png)



## 配置API分组



![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615303410812-22b8c95c-b0af-4027-b8b7-bd50fb31e9a1.png)

1、如果没有配置分组，默认是default。通过groupName()方法即可配置分组：

```java
@Bean
public Docket docket(Environment environment) {
   return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo())
      .groupName("hello") // 配置分组
       // 省略配置....
}
```

2、重启项目查看分组

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615303431608-bddf67fa-43a7-49ca-8558-bc22a8a2111d.png)

3、如何配置多个分组？配置多个分组只需要配置多个docket即可：

```java
@Bean
public Docket docket1(){
   return new Docket(DocumentationType.SWAGGER_2).groupName("group1");
}
@Bean
public Docket docket2(){
   return new Docket(DocumentationType.SWAGGER_2).groupName("group2");
}
@Bean
public Docket docket3(){
   return new Docket(DocumentationType.SWAGGER_2).groupName("group3");
}
```

4、重启项目查看即可

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615303458685-3bc9ad9f-27b5-489e-8188-918e9a4817f5.png)



## 实体配置

1、新建一个实体类

```java
@ApiModel("用户实体")
public class User {
   @ApiModelProperty("用户名")
   public String username;
   @ApiModelProperty("密码")
   public String password;
}
```

2、只要这个实体在请求接口的返回值上（即使是泛型），都能映射到实体项中：

```java
@RequestMapping("/getUser")
public User getUser(){
   return new User();
}
```

3、重启查看测试

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615303495206-557a8048-8520-4f15-91f0-9b81e329fcbb.png)

注：并不是因为@ApiModel这个注解让实体显示在这里了，而是只要出现在接口方法的返回值上的实体都会显示在这里，而@ApiModel和@ApiModelProperty这两个注解只是为实体添加注释的。

@ApiModel为类添加注释

@ApiModelProperty为类属性添加注释



## 常用注解

Swagger的所有注解定义在io.swagger.annotations包下

下面列一些经常用到的，未列举出来的可以另行查阅说明：

Swagger注解	简单说明

```java
@Api(tags = “xxx模块说明”)	作用在模块类上
@ApiOperation(“xxx接口说明”)	作用在接口方法上
@ApiModel(“xxxPOJO说明”)	作用在模型类上：如VO、BO
@ApiModelProperty(value = “xxx属性说明”,hidden = true)	作用在类方法和属性上，hidden设置为true可以隐藏该属性
@ApiParam(“xxx参数说明”)	作用在参数、方法和字段上，类似@ApiModelProperty
我们也可以给请求的接口配置一些注释
@ApiOperation("狂神的接口")
@PostMapping("/kuang")
@ResponseBody
public String kuang(@ApiParam("这个名字会被返回")String username){
   return username;
}
```

给一些难理解的属性或者接口增加一些配置信息，更容易阅读。

##  接口文档导出

使用了swagger2markup的插件。

**maven依赖**

```xml
<dependency>
    <groupId>io.github.swagger2markup</groupId>
    <artifactId>swagger2markup</artifactId>
    <version>1.3.1</version>
</dependency>
<dependency>
    <groupId>io.swagger</groupId>
    <artifactId>swagger-core</artifactId>
    <version>1.5.16</version>
</dependency>
<dependency>
    <groupId>io.swagger</groupId>
    <artifactId>swagger-models</artifactId>
    <version>1.5.16</version>
</dependency>
```

**生成adoc文件**

```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT)
public class DemoApplicationTests {
    @Test
    public void generateAsciiDocs() throws Exception {
        //    输出Ascii格式
        Swagger2MarkupConfig config = new Swagger2MarkupConfigBuilder()
                .withMarkupLanguage(MarkupLanguage.ASCIIDOC) //设置生成格式
                .withOutputLanguage(Language.ZH)  //设置语言中文还是其他语言
                .withPathsGroupedBy(GroupBy.TAGS)
                .withGeneratedExamples()
                .withoutInlineSchema()
                .build();

        Swagger2MarkupConverter.from(new URL("http://localhost:8888/v2/api-docs"))
                .withConfig(config)
                .build()
                .toFile(Paths.get("src/main/resources/docs/asciidoc"));
    }
}
```

- 使用RunWith注解和SpringBootTest注解，启动应用服务容器。 SpringBootTest.WebEnvironment.DEFINED_PORT表示使用application.yml定义的端口，而不是随机使用一个端口进行测试，这很重要。
- Swagger2MarkupConfig 是输出文件的配置，如文件的格式和文件中的自然语言等
- Swagger2MarkupConverter的from表示哪一个HTTP服务作为资源导出的源头(JSON格式)，可以自己访问试一下这个链接。8888是我的服务端口，需要根据你自己的应用配置修改。
- toFile表示将导出文件存放的位置，不用加后缀名。也可以使用toFolder表示文件导出存放的路径。二者区别在于使用toFolder导出为文件目录下按标签TAGS分类的多个文件，使用toFile是导出一个文件（toFolder多个文件的合集）。

**生成markdown文件**

```java
@Test
public void generateMarkdownDocsToFile() throws Exception {
    //    输出Markdown到单文件
    Swagger2MarkupConfig config = new Swagger2MarkupConfigBuilder()
            .withMarkupLanguage(MarkupLanguage.MARKDOWN)
            .withOutputLanguage(Language.ZH)
            .withPathsGroupedBy(GroupBy.TAGS)
            .withGeneratedExamples()
            .withoutInlineSchema()
            .build();

    Swagger2MarkupConverter.from(new URL("http://localhost:8888/v2/api-docs"))
            .withConfig(config)
            .build()
            .toFile(Paths.get("src/main/resources/docs/markdown"));
}
```

**通过maven生成html文档**

```xml
<plugin>
    <groupId>org.asciidoctor</groupId>
    <artifactId>asciidoctor-maven-plugin</artifactId>
    <version>1.5.6</version>
    <configuration>
         <!--asciidoc文件目录-->
        <sourceDirectory>src/main/resources/docs</sourceDirectory>
        <!---生成html的路径-->
        <outputDirectory>src/main/resources/html</outputDirectory>
        <backend>html</backend>
        <sourceHighlighter>coderay</sourceHighlighter>
        <attributes>
            <!--导航栏在左-->
            <toc>left</toc>
            <!--显示层级数-->
            <!--<toclevels>3</toclevels>-->
            <!--自动打数字序号-->
            <sectnums>true</sectnums>
        </attributes>
    </configuration>
</plugin>
```

adoc的sourceDirectory路径必须和之前生成的adoc文件路径一致。然后按照下图方式运行插件。
![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200417143758.png)
        

HTML接口文档显示的效果如下

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200417143833.png)

# 集成Redis

SpringBoot 操作数据: spring-data jpa jdbc mongodb redis !

SpringData也是和SpringBoot齐名的项目!

说明︰在SpringBoot2.x之后，原来使用的jedis被替换为了lettuce?

jedis：采用的直连，多个线程操作的话，是不安全的，如果想要避免不安全的，使用jedis pool连接池！更像BIO模式

lettuce：采用netty，实例可以再多个线程中进行共享，不存在线程不安全的情况!可以减少线程数据了，更像 NIO模式

**源码分析**

```java
public class RedisAutoConfiguration {

   @Bean
   @ConditionalOnMissingBean(name = "redisTemplate")   //我们可以自定义一个RedisTemplate 来替换这个默认的
   @ConditionalOnSingleCandidate(RedisConnectionFactory.class)
   public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
       //默认的RedisTemplate 没有过的这只，redis对象都是需要序列化的
       // 两个泛型都是 Object, Object 的类型，我们后使用需要强制转换<String, Obkect>
      RedisTemplate<Object, Object> template = new RedisTemplate<>();
      template.setConnectionFactory(redisConnectionFactory);
      return template;
   }

   @Bean
   @ConditionalOnMissingBean   //由于String 是人跌势中最常使用的类型，所以说单独提出来了一个bean
   @ConditionalOnSingleCandidate(RedisConnectionFactory.class)
   public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory) {
      StringRedisTemplate template = new StringRedisTemplate();
      template.setConnectionFactory(redisConnectionFactory);
      return template;
   }

}
```

测试：

 需要安装Redis才可以

1、导入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-redis</artifactId>
    <version>1.4.7.RELEASE</version>
</dependency>
```

2、配置连接

```yaml
spring:
  redis:
    database: 0 # Redis 数据库索引（默认为 0）
    host: 192.168.161.3 # Redis 服务器地址
    port: 6379 # Redis 服务器连接端口
    password: 123456 # Redis 服务器连接密码（默认为空）
    lettuce:
      pool:
        max-active: 8 # 连接池最大连接数（使用负值表示没有限制） 默认 8
        max-wait: -1 # 连接池最大阻塞等待时间（使用负值表示没有限制） 默认 -1
        max-idle: 8 # 连接池中的最大空闲连接 默认 8
        min-idle: 0 # 连接池中的最小空闲连接 默认 0
```

**redis模板封装类**

```java
redisTemplate.opsForValue();//操作字符串
redisTemplate.opsForHash();//操作 hash
redisTemplate.opsForList();//操作 list
redisTemplate.opsForSet();//操作 set
redisTemplate.opsForZSet();//操作有序 set
```

StringRedisTemplate 与 RedisTemplate 的封装的 Reids 操作要比我们第二节讲的自己调用 Jedis 的 API 的方式更优雅了一步。

**redis模板封装类的两种注入方式**

```java
public class Example {

  // 注入RedisTemplate，更通用
  @Autowired
  private RedisTemplate<String, String> template;

  // 如：注入RedisTemplate的子类ListOperations，只能操作List
  @Resource
  private ListOperations<String, String> listOps;

}
```

而 StringRedisTemplate 与 RedisTemplate 对应 API 详细请查看此文档 [Spring Data Redis 官方操作手册](https://docs.spring.io/spring-data/redis/docs/2.0.2.RELEASE/reference/html/#redis:template) ，这里不是本节的重点介绍对象了，不再赘述。

**测试**

```java
@SpringBootTest
public class RedisConfigTest {
    @Autowired
    private StringRedisTemplate stringRedisTemplate;

    @Autowired
    private RedisTemplate redisTemplate;

    @Resource(name = "redisTemplate")
    private ValueOperations<String,Object> valueOperations;

    @Resource(name = "redisTemplate")
    private HashOperations<String, String, Object> hashOperations;

    @Resource(name = "redisTemplate")
    private ListOperations<String, Object> listOperations;

    @Resource(name = "redisTemplate")
    private SetOperations<String, Object> setOperations;

    @Resource(name = "redisTemplate")
    private ZSetOperations<String, Object> zSetOperations;

    @Test
    public void testValueObj(){
        Person person = new Person("kobe","bryant");
        person.setAddress(new Address("武汉","中国"));
        //10秒后数据消失
        valueOperations.set("player:1",person,20, TimeUnit.SECONDS);

        Person getBack = (Person) valueOperations.get("player:1");
        System.out.println(getBack);
    }

    @Test
    public void testSetOperation() throws Exception{
        Person person = new Person("kobe","bryant");
        Person person2 = new Person("curry","stephen");
        setOperations.add("playerset",person,person2);
        Set<Object> result = setOperations.members("playerset");
        System.out.println(result);
    }

    @Test
    public void HashOperations() throws Exception{
        Person person = new Person("kobe","bryant");
        hashOperations.put("hash:player","firstname",person.getFirstname());
        hashOperations.put("hash:player","lastname",person.getLastname());
        hashOperations.put("hash:player","address",person.getAddress());
        System.out.println(hashOperations.get("hash:player","firstname"));
    }

    @Test
    public void  ListOperations() throws Exception{

        listOperations.leftPush("list:player",new Person("kobe","bryant"));
        listOperations.leftPush("list:player",new Person("Jordan","Mikel"));
        listOperations.leftPush("list:player",new Person("curry","stephen"));

        System.out.println(listOperations.leftPop("list:player"));
    }
}
```

**解决乱码**

```java
@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate redisTemplate = new RedisTemplate();
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);

        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);

        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);

        //重点在这四行代码
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);

        redisTemplate.afterPropertiesSet();
        return redisTemplate;
    }
}
```

乱码问题的症结在于对象的序列化问题，RedisTemplate默认使用的是JdkSerializationRedisSerializer，StringRedisTemplate默认使用的是StringRedisSerializer。

**操作对象入库**

第一种方式

```java
@Resource(name="redisTemplate")
private HashOperations<String, String, Object> jacksonHashOperations;
private HashMapper<Object, String, Object> jackson2HashMapper = new Jackson2HashMapper(false);
@Test
public void testHashPutAll(){

    Person person = new Person("kobe","bryant");
    person.setId("1");
    person.setAddress(new Address("南京","中国"));
    //将对象以hash的形式放入数据库
    Map<String,Object> mappedHash = jackson2HashMapper.toHash(person);
    jacksonHashOperations.putAll("player" + person.getId(), mappedHash);

    //将对象从数据库取出来
    Map<String,Object> loadedHash = jacksonHashOperations.entries("player" + person.getId());
    Object map = jackson2HashMapper.fromHash(loadedHash);
    Person getback = new ObjectMapper().convertValue(map,Person.class);
    Assert.assertEquals(person.getFirstname(),getback.getFirstname());
}
```

第二种方式：

```java
@RedisHash("people")
public class Person {
  @Id
  String id;
  
  //其他和上一节代码一样

}
public interface PersonRepository extends CrudRepository<Person, String> {
 // 继承CrudRepository，获取基本的CRUD操作
}
```

在项目入口方法上加上注解@EnableRedisRepositories

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class RedisRepositoryTest {

    @Autowired
    PersonRepository personRepository;

    @Test
    public void test(){

        Person rand = new Person("kris", "lin");
        rand.setAddress(new Address("杭州", "中国"));
        personRepository.save(rand);
        Optional<Person> op = personRepository.findById(rand.getId());
        Person p2 = op.get();
        personRepository.count();
        personRepository.delete(rand);

    }

}
```

# 集成Dubbo和Zookeeper

## 什么是分布式系统？

在《分布式系统原理与范型》一书中有如下定义：“分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像单个相关系统”；

分布式系统是由一组通过网络进行通信、为了完成共同的任务而协调工作的计算机节点组成的系统。分布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务。其目的是利用更多的机器，处理更多的数据。

分布式系统（distributed system）是建立在网络之上的软件系统。

首先需要明确的是，只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的提升（加内存、加磁盘、使用更好的CPU）高昂到得不偿失的时候，应用程序也不能进一步优化的时候，我们才需要考虑分布式系统。因为，分布式系统要解决的问题本身就是和单机系统一样的，而由于分布式系统多节点、通过网络通信的拓扑结构，会引入很多单机系统没有的问题，为了解决这些问题又会引入更多的机制、协议，带来更多的问题。。。



## Dubbo文档

随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及流动计算架构势在必行，急需一个治理系统确保架构有条不紊的演进。

在Dubbo的官网文档有这样一张图

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376193376-66d98fdb-11f8-4a38-9731-811706df08c0.png)



**单一应用架构**

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376209903-652e0c7d-a236-41a9-a9fa-a6c2df3933a4.png)

适用于小型网站，小型管理系统，将所有功能都部署到一个功能里，简单易用。

缺点：

1、性能扩展比较难

2、协同开发问题

3、不利于升级维护



**垂直应用架构**

当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，将应用拆成互不相干的几个应用，以提升效率。此时，用于加速前端页面开发的Web框架(MVC)是关键。![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376226411-28ae761e-ffd5-43bb-87e8-0b5baf7d83ef.png)





通过切分业务来实现各个模块独立部署，降低了维护和部署的难度，团队各司其职更易管理，性能扩展也更方便，更有针对性。

缺点：公用模块无法重复利用，开发性的浪费



**分布式服务架构**

当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的**分布式服务框架(RPC)**是关键。

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376260302-472a2f2f-6789-47ef-a404-62bdd9277f0c.png)





**流动计算架构**

当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。此时，用于提高机器利用率的资源调度和治理中心(SOA)[ Service Oriented Architecture]是关键。

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376291881-291e47f9-92bc-457f-b979-27e5bdc6650f.png)





### 什么是RPC

RPC【Remote Procedure Call】是指远程过程调用，是一种进程间通信方式，他是一种技术的思想，而不是规范。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数，而不用程序员显式编码这个远程调用的细节。即程序员无论是调用本地的还是远程的函数，本质上编写的调用代码基本相同。

也就是说两台服务器A，B，一个应用部署在A服务器上，想要调用B服务器上应用提供的函数/方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。为什么要用RPC呢？就是无法在一个进程内，甚至一个计算机内通过本地调用的方式完成的需求，比如不同的系统间的通讯，甚至不同的组织间的通讯，由于计算能力需要横向扩展，需要在多台机器组成的集群上部署应用。RPC就是要像调用本地的函数一样去调远程函数；

推荐阅读文章：https://www.jianshu.com/p/2accc2840a1b



### RPC基本原理

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376330625-0bc91a67-556b-4bde-ba0e-7a65747fb2c4.png)

**步骤解析：**

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376350039-28939c87-4f07-4468-8b83-333297af1e61.png)



**RPC两个核心模块：通讯，序列化。**

测试环境搭建



## Dubbo

Apache Dubbo |ˈdʌbəʊ| 是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。

dubbo官网 http://dubbo.apache.org/zh-cn/index.html

1.了解Dubbo的特性

2.查看官方文档

**dubbo基本概念**

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376407571-093d683f-ad1b-478a-9a47-42c7e7f973ac.png)



**服务提供者**（Provider）：暴露服务的服务提供方，服务提供者在启动时，向注册中心注册自己提供的服务。

**服务消费者**（Consumer）：调用远程服务的服务消费方，服务消费者在启动时，向注册中心订阅自己所需的服务，服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

**注册中心**（Registry）：注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者

**监控中心**（Monitor）：服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心



**调用关系说明**

l 服务容器负责启动，加载，运行服务提供者。

l 服务提供者在启动时，向注册中心注册自己提供的服务。

l 服务消费者在启动时，向注册中心订阅自己所需的服务。

l 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。

l 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

l 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。



## Dubbo环境搭建

点进dubbo官方文档，推荐我们使用Zookeeper 注册中心

什么是zookeeper呢？可以查看官方文档



***Window下安装zookeeper***

1、下载zookeeper ：地址， 我们下载3.4.14 ， 最新版！解压zookeeper

2、运行/bin/zkServer.cmd ，初次运行会报错，没有zoo.cfg配置文件；

可能遇到问题：闪退 !



解决方案：编辑zkServer.cmd文件末尾添加pause 。这样运行出错就不会退出，会提示错误信息，方便找到原因。

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376473904-b4465d53-8c50-408a-b6cb-182ab8b03ab5.png)

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376476849-321b4fe0-6abd-40a0-b1d8-847fb49e3931.png)



3、修改zoo.cfg配置文件

将conf文件夹下面的zoo_sample.cfg复制一份改名为zoo.cfg即可。

注意几个重要位置：

dataDir=./ 临时数据存储的目录（可写相对路径）

clientPort=2181 zookeeper的端口号

修改完成后再次启动zookeeper



![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376488200-ae4c74af-6c52-4e20-9cea-61b389e3e702.png)



4、使用zkCli.cmd测试

ls /：列出zookeeper根下保存的所有节点

```xml
[zk: 127.0.0.1:2181(CONNECTED) 4] ls /
[zookeeper]
```

create –e /kuangshen 123：创建一个kuangshen节点，值为123

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376516372-333fb567-8347-465c-a08e-0c6ad454d2cd.png)

get /kuangshen：获取/kuangshen节点的值

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376521994-0ff126e3-4f67-4ca5-9a75-52e9da9b6dcd.png)

我们再来查看一下节点

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376528482-ae05cc90-bace-4b6c-b310-856f631cf9fd.png)





**window下安装dubbo-admin**

dubbo本身并不是一个服务软件。它其实就是一个jar包，能够帮你的java程序连接到zookeeper，并利用zookeeper消费、提供服务。

但是为了让用户更好的管理监控众多的dubbo服务，官方提供了一个可视化的监控程序dubbo-admin，不过这个监控即使不装也不影响使用。

我们这里来安装一下：



**1、下载dubbo-admin**

地址 ：https://github.com/apache/dubbo-admin/tree/master



**2、解压进入目录**

修改 dubbo-admin\src\main\resources \application.properties 指定zookeeper地址

```plain
server.port=7001
spring.velocity.cache=false
spring.velocity.charset=UTF-8
spring.velocity.layout-url=/templates/default.vm
spring.messages.fallback-to-system-locale=false
spring.messages.basename=i18n/message
spring.root.password=root
spring.guest.password=guest
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

**3、在项目目录下打包dubbo-admin**

```plain
mvn clean package -Dmaven.test.skip=true
```

第一次打包的过程有点慢，需要耐心等待！直到成功！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376582932-a03177c8-9757-4a41-8b3f-6b8f9fd4a27d.png)

4、执行 dubbo-admin\target 下的dubbo-admin-0.0.1-SNAPSHOT.jar

```plain
java -jar dubbo-admin-0.0.1-SNAPSHOT.jar
```

【注意：zookeeper的服务一定要打开！】

执行完毕，我们去访问一下 http://localhost:7001/ ， 这时候我们需要输入登录账户和密码，我们都是默认的root-root；

登录成功后，查看界面

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376602043-d0eed41d-8c49-4300-aa6d-cbdc5ecb63c5.png)

安装完成！



## SpringBoot + Dubbo + zookeeper

**框架搭建**



**1. 启动zookeeper ！**

**2. IDEA创建一个空项目；**

**3.创建一个模块，实现服务提供者：provider-server ， 选择web依赖即可**

**4.项目创建完毕，我们写一个服务，比如卖票的服务；**



编写接口

```java
package com.kuang.provider.service;

public interface TicketService {
   public String getTicket();
}
```

**编写实现类**

```java
package com.kuang.provider.service;

public class TicketServiceImpl implements TicketService {
   @Override
   public String getTicket() {
       return "《狂神说Java》";
  }
}
```

5.创建一个模块，实现服务消费者：consumer-server ， 选择web依赖即可

6.项目创建完毕，我们写一个服务，比如用户的服务；

编写service

```java
package com.kuang.consumer.service;

public class UserService {
   //我们需要去拿去注册中心的服务
}
```

**需求：现在我们的用户想使用买票的服务，这要怎么弄呢 ？**



## 服务提供者

1、将服务提供者注册到注册中心，我们需要整合Dubbo和zookeeper，所以需要导包

我们从dubbo官网进入github，看下方的帮助文档，找到dubbo-springboot，找到依赖包

```xml
<!-- Dubbo Spring Boot Starter -->
<dependency>
   <groupId>org.apache.dubbo</groupId>
   <artifactId>dubbo-spring-boot-starter</artifactId>
   <version>2.7.3</version>
</dependency>
```

**zookeeper的包我们去maven仓库下载，zkclient；**

```xml
<!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
<dependency>
   <groupId>com.github.sgroschupf</groupId>
   <artifactId>zkclient</artifactId>
   <version>0.1</version>
</dependency>
```

**【新版的坑】zookeeper及其依赖包，解决日志冲突，还需要剔除日志依赖；**

```xml
<!-- 引入zookeeper -->
<dependency>
   <groupId>org.apache.curator</groupId>
   <artifactId>curator-framework</artifactId>
   <version>2.12.0</version>
</dependency>
<dependency>
   <groupId>org.apache.curator</groupId>
   <artifactId>curator-recipes</artifactId>
   <version>2.12.0</version>
</dependency>
<dependency>
   <groupId>org.apache.zookeeper</groupId>
   <artifactId>zookeeper</artifactId>
   <version>3.4.14</version>
   <!--排除这个slf4j-log4j12-->
   <exclusions>
       <exclusion>
           <groupId>org.slf4j</groupId>
           <artifactId>slf4j-log4j12</artifactId>
       </exclusion>
   </exclusions>
</dependency>
```

**2、在springboot配置文件中配置dubbo相关属性！**

```xml
#当前应用名字
dubbo.application.name=provider-server
#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
#扫描指定包下服务
dubbo.scan.base-packages=com.kuang.provider.service
```

**3、在service的实现类中配置服务注解，发布服务！注意导包问题**

```java
import org.apache.dubbo.config.annotation.Service;
import org.springframework.stereotype.Component;
@Service //将服务发布出去
@Component //放在容器中
public class TicketServiceImpl implements TicketService {
   @Override
   public String getTicket() {
       return "《狂神说Java》";
  }
}
```

**逻辑理解 ：应用启动起来，dubbo就会扫描指定的包下带有@component注解的服务，将它发布在指定的注册中心中！**



## 服务消费者

1、导入依赖，和之前的依赖一样；

```xml
<!--dubbo-->
<!-- Dubbo Spring Boot Starter -->
<dependency>
   <groupId>org.apache.dubbo</groupId>
   <artifactId>dubbo-spring-boot-starter</artifactId>
   <version>2.7.3</version>
</dependency>
<!--zookeeper-->
<!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
<dependency>
   <groupId>com.github.sgroschupf</groupId>
   <artifactId>zkclient</artifactId>
   <version>0.1</version>
</dependency>
<!-- 引入zookeeper -->
<dependency>
   <groupId>org.apache.curator</groupId>
   <artifactId>curator-framework</artifactId>
   <version>2.12.0</version>
</dependency>
<dependency>
   <groupId>org.apache.curator</groupId>
   <artifactId>curator-recipes</artifactId>
   <version>2.12.0</version>
</dependency>
<dependency>
   <groupId>org.apache.zookeeper</groupId>
   <artifactId>zookeeper</artifactId>
   <version>3.4.14</version>
   <!--排除这个slf4j-log4j12-->
   <exclusions>
       <exclusion>
           <groupId>org.slf4j</groupId>
           <artifactId>slf4j-log4j12</artifactId>
       </exclusion>
   </exclusions>
</dependency>
```

**2、配置参数**

```xml
#当前应用名字
dubbo.application.name=consumer-server
#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

\3. 本来正常步骤是需要将服务提供者的接口打包，然后用pom文件导入，我们这里使用简单的方式，直接将服务的接口拿过来，路径必须保证正确，即和服务提供者相同；

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376790079-4fa7f630-355f-4fbc-ac5c-83e38381c5e9.png)

**4. 完善消费者的服务类**

```java
package com.kuang.consumer.service;

import com.kuang.provider.service.TicketService;
import org.apache.dubbo.config.annotation.Reference;
import org.springframework.stereotype.Service;

@Service //注入到容器中
public class UserService {

   @Reference //远程引用指定的服务，他会按照全类名进行匹配，看谁给注册中心注册了这个全类名
   TicketService ticketService;

   public void bugTicket(){
       String ticket = ticketService.getTicket();
       System.out.println("在注册中心买到"+ticket);
  }

}
```

**5. 测试类编写；**

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class ConsumerServerApplicationTests {

   @Autowired
   UserService userService;

   @Test
   public void contextLoads() {

       userService.bugTicket();

  }

}
```

## 启动测试

1. 开启zookeeper

2. 打开dubbo-admin实现监控【可以不用做】

3. 开启服务者

4. 消费者消费测试，结果：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376831968-55ce7978-9741-4de9-928c-501f199c311d.png)

监控中心 ：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376837837-8cfbd18c-8320-484b-9da9-82278cca9251.png)

ok , 这就是SpingBoot + dubbo + zookeeper实现分布式开发的应用，其实就是一个服务拆分的思想；



# 集成富文本编辑器

## 简介

思考：我们平时在博客园，或者CSDN等平台进行写作的时候，有同学思考过他们的编辑器是怎么实现的吗？

在博客园后台的选项设置中，可以看到一个文本编辑器的选项：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376878990-bea4dc48-fbbf-4435-87a9-cfb0cf692b5f.png)





其实这个就是富文本编辑器，市面上有许多非常成熟的富文本编辑器，比如：

- **Editor.md**——功能非常丰富的编辑器，左端编辑，右端预览，非常方便，完全免费 
- 官网：https://pandao.github.io/editor.md/ 
- **wangEditor**——基于javascript和css开发的 Web富文本编辑器， 轻量、简洁、界面美观、易用、开源免费。    
- 官网：http://www.wangeditor.com/
- **TinyMCE**——TinyMCE是一个轻量级的基于浏览器的所见即所得编辑器，由JavaScript写成。它对IE6+和Firefox1.5+都有着非常良好的支持。功能齐全，界面美观，就是文档是英文的，对开发人员英文水平有一定要求。    
- 官网：https://www.tiny.cloud/docs/demo/full-featured/ 
- **百度ueditor**——UEditor是由百度web前端研发部开发所见即所得富文本web编辑器，具有轻量，功能齐全，可定制，注重用户体验等特点，开源基于MIT协议，允许自由使用和修改代码，缺点是已经没有更新了    
- 官网：https://ueditor.baidu.com/website/onlinedemo.html
- **kindeditor**——界面经典。    
- 官网：http://kindeditor.net/demo.php
- **Textbox**——Textbox是一款极简但功能强大的在线文本编辑器，支持桌面设备和移动设备。主要功能包含内置的图像处理和存储、文件拖放、拼写检查和自动更正。此外，该工具还实现了屏幕阅读器等辅助技术，并符合WAI-ARIA可访问性标准。    
- 官网：https://textbox.io/
- **CKEditor**——国外的，界面美观。    
- 官网：https://ckeditor.com/ckeditor-5/demo/
- **quill**——功能强大，还可以编辑公式等    
- 官网：https://quilljs.com/
- **simditor**——界面美观，功能较全。    
- 官网：https://simditor.tower.im/
- **summernote**——UI好看，精美    
- 官网：https://summernote.org/
- **jodit**——功能齐全    
- 官网：https://xdsoft.net/jodit/
- **froala Editor**——界面非常好看，功能非常强大，非常好用（非免费）    
- 官网：https://www.froala.com/wysiwyg-editor

## Editor.md

我这里使用的就是Editor.md，作为一个资深码农，Mardown必然是我们程序猿最喜欢的格式，看下面，就爱上了！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615376946179-28def9c0-6711-4daa-aad6-51aee754abbc.png)



我们可以在官网下载它：https://pandao.github.io/editor.md/ ， 得到它的压缩包！

解压以后，在examples目录下面，可以看到他的很多案例使用！学习，其实就是看人家怎么写的，然后进行模仿就好了！

我们可以将整个解压的文件倒入我们的项目，将一些无用的测试和案例删掉即可！



## 基础工程搭建

------

数据库设计

article：文章表

| 字段    |          | 备注         |
| ------- | -------- | ------------ |
| id      | int      | 文章的唯一ID |
| author  | varchar  | 作者         |
| title   | varchar  | 标题         |
| content | longtext | 文章的内容   |

**建表SQL：**



```sql
CREATE TABLE `article` (
`id` int(10) NOT NULL AUTO_INCREMENT COMMENT 'int文章的唯一ID',
`author` varchar(50) NOT NULL COMMENT '作者',
`title` varchar(100) NOT NULL COMMENT '标题',
`content` longtext NOT NULL COMMENT '文章的内容',
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```



------

基础项目搭建

1、建一个SpringBoot项目配置

```yaml
spring:
datasource:
  username: root
  password: 123456
  #?serverTimezone=UTC解决时区的报错
  url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
  driver-class-name: com.mysql.cj.jdbc.Driver
<resources>
   <resource>
       <directory>src/main/java</directory>
       <includes>
           <include>**/*.xml</include>
       </includes>
       <filtering>true</filtering>
   </resource>
</resources>
```



2、实体类：

```java
//文章类
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Article implements Serializable {

   private int id; //文章的唯一ID
   private String author; //作者名
   private String title; //标题
   private String content; //文章的内容

}
```

**3、mapper接口：**

```java
@Mapper
@Repository
public interface ArticleMapper {
   //查询所有的文章
   List<Article> queryArticles();

   //新增一个文章
   int addArticle(Article article);

   //根据文章id查询文章
   Article getArticleById(int id);

   //根据文章id删除文章
   int deleteArticleById(int id);

}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
       "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.kuang.mapper.ArticleMapper">

   <select id="queryArticles" resultType="Article">
      select * from article
   </select>
   
   <select id="getArticleById" resultType="Article">
      select * from article where id = #{id}
   </select>
   
   <insert id="addArticle" parameterType="Article">
      insert into article (author,title,content) values (#{author},#{title},#{content});
   </insert>
   
   <delete id="deleteArticleById" parameterType="int">
      delete from article where id = #{id}
   </delete>
   
</mapper>
```



**既然已经提供了 myBatis 的映射配置文件，自然要告诉 spring boot 这些文件的位置**

```yaml
mybatis:
mapper-locations: classpath:com/kuang/mapper/*.xml
type-aliases-package: com.kuang.pojo
```

编写一个Controller测试下，是否ok；



## 文章编辑整合（重点）

1、导入 editor.md 资源 ，删除多余文件



2、编辑文章页面 editor.html、需要引入 jQuery；

```html
<!DOCTYPE html>
<html class="x-admin-sm" lang="zh" xmlns:th="http://www.thymeleaf.org">
<head>
   <meta charset="UTF-8">
   <title>秦疆'Blog</title>
   <meta name="renderer" content="webkit">
   <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
   <meta name="viewport" content="width=device-width,user-scalable=yes, minimum-scale=0.4, initial-scale=0.8,target-densitydpi=low-dpi" />
   <!--Editor.md-->
   <link rel="stylesheet" th:href="@{/editormd/css/editormd.css}"/>
   <link rel="shortcut icon" href="https://pandao.github.io/editor.md/favicon.ico" type="image/x-icon" />
</head>
<body>
<div class="layui-fluid">
   <div class="layui-row layui-col-space15">
       <div class="layui-col-md12">
           <!--博客表单-->
           <form name="mdEditorForm">
               <div>
                  标题：<input type="text" name="title">
               </div>
               <div>
                  作者：<input type="text" name="author">
               </div>
               <div id="article-content">
                   <textarea name="content" id="content" style="display:none;"> </textarea>
               </div>
           </form>
       </div>
   </div>
</div>
</body>
<!--editormd-->
<script th:src="@{/editormd/lib/jquery.min.js}"></script>
<script th:src="@{/editormd/editormd.js}"></script>
<script type="text/javascript">
   var testEditor;
   //window.onload = function(){ }
   $(function() {
       testEditor = editormd("article-content", {
           width : "95%",
           height : 400,
           syncScrolling : "single",
           path : "../editormd/lib/",
           saveHTMLToTextarea : true,    // 保存 HTML 到 Textarea
           emoji: true,
           theme: "dark",//工具栏主题
           previewTheme: "dark",//预览主题
           editorTheme: "pastel-on-dark",//编辑主题
           tex : true,                   // 开启科学公式TeX语言支持，默认关闭
           flowChart : true,             // 开启流程图支持，默认关闭
           sequenceDiagram : true,       // 开启时序/序列图支持，默认关闭,
           //图片上传
           imageUpload : true,
           imageFormats : ["jpg", "jpeg", "gif", "png", "bmp", "webp"],
           imageUploadURL : "/article/file/upload",
           onload : function() {
               console.log('onload', this);
          },
           /*指定需要显示的功能按钮*/
           toolbarIcons : function() {
               return ["undo","redo","|",
                   "bold","del","italic","quote","ucwords","uppercase","lowercase","|",
                   "h1","h2","h3","h4","h5","h6","|",
                   "list-ul","list-ol","hr","|",
                   "link","reference-link","image","code","preformatted-text",
                   "code-block","table","datetime","emoji","html-entities","pagebreak","|",
                   "goto-line","watch","preview","fullscreen","clear","search","|",
                   "help","info","releaseIcon", "index"]
          },
           /*自定义功能按钮，下面我自定义了2个，一个是发布，一个是返回首页*/
           toolbarIconTexts : {
               releaseIcon : "<span bgcolor=\"gray\">发布</span>",
               index : "<span bgcolor=\"red\">返回首页</span>",
          },
           /*给自定义按钮指定回调函数*/
           toolbarHandlers:{
               releaseIcon : function(cm, icon, cursor, selection) {
                   //表单提交
                   mdEditorForm.method = "post";
                   mdEditorForm.action = "/article/addArticle";//提交至服务器的路径
                   mdEditorForm.submit();
              },
               index : function(){
                   window.location.href = '/';
              },
          }
      });
  });
</script>
</html>
```

**3、编写Controller，进行跳转，以及保存文章**

```java
@Controller
@RequestMapping("/article")
public class ArticleController {

   @GetMapping("/toEditor")
   public String toEditor(){
       return "editor";
  }
   
   @PostMapping("/addArticle")
   public String addArticle(Article article){
       articleMapper.addArticle(article);
       return "editor";
  }
   
}
```

------

图片上传问题



1、前端js中添加配置

```yaml
//图片上传
imageUpload : true,
imageFormats : ["jpg", "jpeg", "gif", "png", "bmp", "webp"],
imageUploadURL : "/article/file/upload", // //这个是上传图片时的访问地址
```

2、后端请求，接收保存这个图片, 需要导入 FastJson 的依赖！

```java
//博客图片上传问题
@RequestMapping("/file/upload")
@ResponseBody
public JSONObject fileUpload(@RequestParam(value = "editormd-image-file", required = true) MultipartFile file, HttpServletRequest request) throws IOException {
   //上传路径保存设置

   //获得SpringBoot当前项目的路径：System.getProperty("user.dir")
   String path = System.getProperty("user.dir")+"/upload/";

   //按照月份进行分类：
   Calendar instance = Calendar.getInstance();
   String month = (instance.get(Calendar.MONTH) + 1)+"月";
   path = path+month;

   File realPath = new File(path);
   if (!realPath.exists()){
       realPath.mkdir();
  }

   //上传文件地址
   System.out.println("上传文件保存地址："+realPath);

   //解决文件名字问题：我们使用uuid;
   String filename = "ks-"+UUID.randomUUID().toString().replaceAll("-", "");
   //通过CommonsMultipartFile的方法直接写文件（注意这个时候）
   file.transferTo(new File(realPath +"/"+ filename));

   //给editormd进行回调
   JSONObject res = new JSONObject();
   res.put("url","/upload/"+month+"/"+ filename);
   res.put("success", 1);
   res.put("message", "upload success!");

   return res;
}
```

3、解决文件回显显示的问题，设置虚拟目录映射！在我们自己拓展的MvcConfig中进行配置即可！

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
   // 文件保存在真实目录/upload/下，
   // 访问的时候使用虚路径/upload，比如文件名为1.png，就直接/upload/1.png就ok了。
   @Override
   public void addResourceHandlers(ResourceHandlerRegistry registry) {
       registry.addResourceHandler("/upload/**")
          .addResourceLocations("file:"+System.getProperty("user.dir")+"/upload/");
  }
}
```



------

表情包问题

自己手动下载，emoji 表情包，放到图片路径下：

修改editormd.js文件

```java
// Emoji graphics files url path
editormd.emoji     = {
   path : "../editormd/plugins/emoji-dialog/emoji/",
   ext   : ".png"
};
```

## 文章展示

1、Controller 中增加方法

```java
@GetMapping("/{id}")
public String show(@PathVariable("id") int id,Model model){
   Article article = articleMapper.getArticleById(id);
   model.addAttribute("article",article);
   return "article";
}
```

2、编写页面 article.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
   <title th:text="${article.title}"></title>
</head>
<body>

<div>
   <!--文章头部信息：标题，作者，最后更新日期，导航-->
   <h2 style="margin: auto 0" th:text="${article.title}"></h2>
  作者：<span style="float: left" th:text="${article.author}"></span>
   <!--文章主体内容-->
   <div id="doc-content">
       <textarea style="display:none;" placeholder="markdown" th:text="${article.content}"></textarea>
   </div>

</div>

<link rel="stylesheet" th:href="@{/editormd/css/editormd.preview.css}" />
<script th:src="@{/editormd/lib/jquery.min.js}"></script>
<script th:src="@{/editormd/lib/marked.min.js}"></script>
<script th:src="@{/editormd/lib/prettify.min.js}"></script>
<script th:src="@{/editormd/lib/raphael.min.js}"></script>
<script th:src="@{/editormd/lib/underscore.min.js}"></script>
<script th:src="@{/editormd/lib/sequence-diagram.min.js}"></script>
<script th:src="@{/editormd/lib/flowchart.min.js}"></script>
<script th:src="@{/editormd/lib/jquery.flowchart.min.js}"></script>
<script th:src="@{/editormd/editormd.js}"></script>

<script type="text/javascript">
   var testEditor;
   $(function () {
       testEditor = editormd.markdownToHTML("doc-content", {//注意：这里是上面DIV的id
           htmlDecode: "style,script,iframe",
           emoji: true,
           taskList: true,
           tocm: true,
           tex: true, // 默认不解析
           flowChart: true, // 默认不解析
           sequenceDiagram: true, // 默认不解析
           codeFold: true
      });});
</script>
</body>
</html>
```

重启项目，访问进行测试！大功告成！

小结：

有了富文本编辑器，我们网站的功能就会又多一项，大家到了这里完全可以有时间写一个属于自己的博客网站了，根据所学的知识是完全没有任何问题的！