# 基础入门

## **1、MVC简介**

### 1.1、什么是MVC

- MVC是模型(Model)、视图(View)、控制器(Controller)的简写，是一种软件设计规范。
- 是将业务逻辑、数据、显示分离的方法来组织代码。

- MVC主要作用是**降低了视图与业务逻辑间的双向偶合**。
- MVC不是一种设计模式，**MVC是一种架构模式**。当然不同的MVC存在差异。

**Model（模型）：**数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据Dao） 和 服务层（行为Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

**View（视图）：**负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。

**Controller（控制器）：**接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。也就是说控制器做了个调度员的工作。

**最典型的MVC就是JSP + servlet + javabean的模式。**

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615082859469-22a8612a-9c81-4016-b8b0-44bcf0e757c0.png)

### 1.2、Model1时代

- 在web早期的开发中，通常采用的都是Model1。
- Model1中，主要分为两层，视图层和模型层。

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615082859519-a930c28a-5b5d-43f3-a577-192f9a6982e5.webp)

Model1优点：架构简单，比较适合小型项目开发；

Model1缺点：JSP职责不单一，职责过重，不便于维护；

### 1.3、Model2时代

Model2把一个项目分成三部分，包括**视图、控制、模型。**

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615082859439-3f94d43b-1ab0-4449-85cc-9c1f107b8176.webp)

- 用户发请求

- Servlet接收请求数据，并调用对应的业务逻辑方法

- 业务处理完毕，返回更新后的数据给servlet

- servlet转向到JSP，由JSP来渲染页面

- 响应给前端更新后的页面

**职责分析：**

**Controller：控制器**

- 取得表单数据

- 调用业务逻辑

- 转向指定的页面

**Model：模型**

- 业务逻辑

- 保存数据的状态

**View：视图**

- 显示页面

Model2这样不仅提高的代码的复用率与项目的扩展性，且大大降低了项目的维护成本。

### 1.4、回顾Servlet

新建一个Maven工程当做父工程，pom依赖：

```xml
<dependencies>
   <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
   </dependency>
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-webmvc</artifactId>
       <version>5.1.9.RELEASE</version>
   </dependency>
   <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>servlet-api</artifactId>
       <version>2.5</version>
   </dependency>
   <dependency>
       <groupId>javax.servlet.jsp</groupId>
       <artifactId>jsp-api</artifactId>
       <version>2.2</version>
   </dependency>
   <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>jstl</artifactId>
       <version>1.2</version>
   </dependency>
</dependencies>
```

建立一个Moudle：springmvc-01-servlet ， 添加Web app的支持！

导入servlet 和 jsp 的 jar 依赖

```xml
<dependency>
   <groupId>javax.servlet</groupId>
   <artifactId>servlet-api</artifactId>
   <version>2.5</version>
</dependency>
<dependency>
   <groupId>javax.servlet.jsp</groupId>
   <artifactId>jsp-api</artifactId>
   <version>2.2</version>
</dependency>
```

编写一个Servlet类，用来处理用户的请求

```java
package com.kuang.servlet;
//实现Servlet接口
public class HelloServlet extends HttpServlet {
   @Override
   protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       //取得参数
       String method = req.getParameter("method");
       if (method.equals("add")){
           req.getSession().setAttribute("msg","执行了add方法");
      }
       if (method.equals("delete")){
           req.getSession().setAttribute("msg","执行了delete方法");
      }
       //业务逻辑
       //视图跳转
       req.getRequestDispatcher("/WEB-INF/jsp/hello.jsp").forward(req,resp);
  }
   @Override
   protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       doGet(req,resp);
  }
}
```

编写Hello.jsp，在WEB-INF目录下新建一个jsp的文件夹，新建hello.jsp

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
   <title>Kuangshen</title>
</head>
<body>
${msg}
</body>
</html>
```

在web.xml中注册Servlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
        version="4.0">
   <servlet>
       <servlet-name>HelloServlet</servlet-name>
       <servlet-class>com.kuang.servlet.HelloServlet</servlet-class>
   </servlet>
   <servlet-mapping>
       <servlet-name>HelloServlet</servlet-name>
       <url-pattern>/user</url-pattern>
   </servlet-mapping>
  </web-app>
```

配置Tomcat，并启动测试

- - localhost:8080/user?method=add
  - localhost:8080/user?method=delete

**MVC框架要做哪些事情**

- 将url映射到java类或java类的方法

- 封装用户提交的数据

- 处理请求--调用相关的业务处理--封装响应数据 .

- 将响应的数据进行渲染 . jsp / html 等表示层数据 .

**说明：**

常见的服务器端MVC框架有：Struts、Spring MVC、ASP.NET MVC、Zend Framework、JSF；常见前端MVC框架：vue、angularjs、react、backbone；由MVC演化出了另外一些模式如：MVP、MVVM 等。

## 2、SpringMVC简介

### 概述

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1615082859468-a4c6c93a-5e30-443e-8a13-aa5ad28bce2b.png)

Spring MVC是Spring Framework的一部分，是基于Java实现MVC的轻量级Web框架。

查看官方文档：https://docs.spring.io/spring/docs/5.2.0.RELEASE/spring-framework-reference/web.html#spring-web

**我们为什么要学习SpringMVC呢?**

 Spring MVC的特点：

- 轻量级，简单易学

- 高效 , 基于请求响应的MVC框架

- 与Spring兼容性好，无缝结合

- 约定优于配置

- 功能强大：RESTful、数据验证、格式化、本地化、主题等

- 简洁灵活

Spring的web框架围绕**DispatcherServlet** [ 调度Servlet ] 设计。

DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解形式进行开发，十分简洁；

### 中心控制器

​	Spring的web框架围绕DispatcherServlet设计。DispatcherServlet的作用是将请求分发到不同的处理器。

​	Spring MVC框架像许多其他MVC框架一样, **以请求为驱动** , **围绕一个中心Servlet分派请求及提供其他功能**，**DispatcherServlet是一个实际的Servlet (它继承自HttpServlet 基类)**。

![202105031012358813.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105031012358813.png)

SpringMVC的原理如下图所示：

- ![图片](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/640.jpg)

1. 向服务器发送 HTTP 请求，请求被前端控制器 DispatcherServlet 捕获。
2. DispatcherServlet 根据 -servlet.xml 中的配置对请求的 URL 进行解析，得到请求资源标识符（URI）。然后根据该 URI，调用 HandlerMapping 获得该 Handler 配置的所有相关的对象（包括 Handler 对象以及 Handler 对象对应的拦截器），最后以HandlerExecutionChain 对象的形式返回。
3. DispatcherServlet 根据获得的Handler，选择一个合适的 HandlerAdapter。（附注：如果成功获得HandlerAdapter后，此时将开始执行拦截器的 preHandler(…)方法）。
4. 提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller)。在填充Handler的入参过程中，根据你的配置，Spring 将帮你做一些额外的工作：
   - HttpMessageConveter：将请求消息（如 Json、xml 等数据）转换成一个对象，将对象转换为指定的响应信息。
   - 数据转换：对请求消息进行数据转换。如`String`转换成`Integer`、`Double`等。
   - 数据根式化：对请求消息进行数据格式化。如将字符串转换成格式化数字或格式化日期等。
   - 数据验证：验证数据的有效性（长度、格式等），验证结果存储到`BindingResult`或`Error`中。

5.Handler(Controller)执行完成后，向 DispatcherServlet 返回一个 ModelAndView 对象；

6.根据返回的ModelAndView，选择一个适合的 ViewResolver（必须是已经注册到 Spring 容器中的ViewResolver)返回给DispatcherServlet。

7.ViewResolver 结合Model和View，来渲染视图。

8.视图负责将渲染结果返回给客户端。

**核心原理**

本质上将controller对象都托管到ioc容器后，运用反射扫描controller对象中的Method的方法，提取相应的*@RequestMapping*等注解，将访问路径和方法连接起来。

通过访问路径，即可通过反射调用相应方法进行处理，期间在方法调用前以及调用后可以运用反射增强。 

## 3、使用

### **配置版**

1、新建一个Moudle，添加web的支持

2、确定导入了SpringMVC 的依赖

3、配置web.xml，注册DispatcherServlet

```xml
<?xml version="1.0" encoding="utf-8"?>

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" version="4.0">  
  <!--1.注册DispatcherServlet-->  
  <servlet> 
    <servlet-name>springmvc</servlet-name>  
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
    <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->  
    <init-param> 
      <param-name>contextConfigLocation</param-name>  
      <param-value>classpath:springmvc-servlet.xml</param-value> 
    </init-param>  
    <!--启动级别-1-->  
    <load-on-startup>1</load-on-startup> 
  </servlet>  
  <!--/ 匹配所有的请求；（不包括.jsp）-->  
  <!--/* 匹配所有的请求；（包括.jsp）-->  
  <servlet-mapping> 
    <servlet-name>springmvc</servlet-name>  
    <url-pattern>/</url-pattern> 
  </servlet-mapping> 
</web-app>

```

4、编写配置文件springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans       http://www.springframework.org/schema/beans/spring-beans.xsd"> </beans>

<!--处理器映射器-->
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>

<!--处理器适配器-->
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>

<!--视图解析器-->
<?xml version="1.0" encoding="utf-8"?>

<!--视图解析器:DispatcherServlet给他的ModelAndView-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver"> 
  <!--前缀-->  
  <property name="prefix" value="/WEB-INF/jsp/"/>  
  <!--后缀-->  
  <property name="suffix" value=".jsp"/>
</bean>
```

5、编写Controller（实现接口或注解），返回modelandview

```java
package com.kuang.controller;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


//注意：这里我们先导入Controller接口
public class HelloController implements Controller {
    public ModelAndView handleRequest(HttpServletRequest request,
        HttpServletResponse response) throws Exception { //ModelAndView 模型和视图       

        ModelAndView mv = new ModelAndView(); //封装对象，放在ModelAndView中。Model       
        mv.addObject("msg", "HelloSpringMVC!"); //封装要跳转的视图，放在ModelAndView中       
        mv.setViewName("hello"); //: /WEB-INF/jsp/hello.jsp       

        return mv;
    }
}
```

6、注册bean到IOC

```xml
<!--Handler--><bean id="/hello" class="com.kuang.controller.HelloController"/>
```

7、编写对应的页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>  
<title>Kuangshen</title>
</head>
<body>${msg}</body>
</html>
```

### 注解版

1、新建一个Moudle，添加web支持！

2、由于Maven可能存在资源过滤的问题，我们将配置完善

```xml
<?xml version="1.0" encoding="utf-8"?>

<build> 
  <resources> 
    <resource> 
      <directory>src/main/java</directory>  
      <includes> 
        <include>**/*.properties</include>  
        <include>**/*.xml</include> 
      </includes>  
      <filtering>false</filtering> 
    </resource>  
    <resource> 
      <directory>src/main/resources</directory>  
      <includes> 
        <include>**/*.properties</include>  
        <include>**/*.xml</include> 
      </includes>  
      <filtering>false</filtering> 
    </resource> 
  </resources>
</build>

```

3、在pom.xml文件引入相关的依赖：主要有Spring框架核心库、Spring MVC、servlet , JSTL等。我们在父依赖中已经引入了！

**4、配置web.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" version="4.0">  
  <!--1.注册servlet-->  
  <servlet> 
    <servlet-name>SpringMVC</servlet-name>  
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
    <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->  
    <init-param> 
      <param-name>contextConfigLocation</param-name>  
      <param-value>classpath:springmvc-servlet.xml</param-value> 
    </init-param>  
    <!-- 启动顺序，数字越小，启动越早 -->  
    <load-on-startup>1</load-on-startup> 
  </servlet>  
  <!--所有请求都会被springmvc拦截 -->  
  <servlet-mapping> 
    <servlet-name>SpringMVC</servlet-name>  
    <url-pattern>/</url-pattern> 
  </servlet-mapping> 
</web-app>

```

### 静态资源访问

**/ 和 /*的区别：**

< url-pattern > / </ url-pattern > 不会匹配到.jsp，只针对我们编写的请求；即：.jsp 不会进入spring的 DispatcherServlet类 。

< url-pattern > /* </ url-pattern > 会匹配 *.jsp，会出现返回 jsp视图 时再次进入spring的DispatcherServlet 类，导致找不到对应的controller所以报404错。

- 注意web.xml版本问题，要最新版！
- 注册DispatcherServlet
- 关联SpringMVC的配置文件
- 启动级别为1
- 映射路径为 / 

**因其覆盖了tomcat默认的servlet，会使得我们项目无法访问静态资源，解决方案：**

**一、web.xml配置**

```xml
    <servlet-mapping>
    	<servlet-name>default</servlet-name>
    	<url-pattern>*.html</url-pattern>
    	<url-pattern>*.jpg</url-pattern>
    	<url-pattern>*.css</url-pattern>
    	<url-pattern>*.js</url-pattern>
    	<url-pattern>*.png</url-pattern>
    </servlet-mapping>
```

二、Spring MVC提供了**mvc:resources标签，该标签的作用可以把页面的不同请求，转发到项目内部的某个目录下。该标签配置在springmvc.xml文件下。

```xml
     <!--静态资源处理-->
    <mvc:resources mapping="/images/**" location="/images/"/>
    <mvc:resources mapping="/css/**" location="/css/"/>
    <mvc:resources mapping="/js/**" location="/js/"/>
```

三、**mvc:default-servlet-handler**，该标签相当于一次帮助我们把所有静态资源目录的文件对外映射出去。

```xml
    <mvc:default-servlet-handler/>
```

**5、添加Spring MVC配置文件**，在resource目录下添加springmvc-servlet.xml配置文件，为了支持基于注解的IOC，设置了自动扫描包的功能，具体配置信息如下：

```xml
<?xml version="1.0" encoding="utf-8"?>

<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context" xmlns:mvc="http://www.springframework.org/schema/mvc" xsi:schemaLocation="http://www.springframework.org/schema/beans       http://www.springframework.org/schema/beans/spring-beans.xsd       http://www.springframework.org/schema/context       https://www.springframework.org/schema/context/spring-context.xsd       http://www.springframework.org/schema/mvc       https://www.springframework.org/schema/mvc/spring-mvc.xsd">  
  <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->  
  <context:component-scan base-package="com.kuang.controller"/>  
  <!-- 让Spring MVC不处理静态资源 -->  
  <mvc:default-servlet-handler/>  
  <!--   支持mvc注解驱动       在spring中一般采用@RequestMapping注解来完成映射关系       要想使@RequestMapping注解生效       必须向上下文中注册DefaultAnnotationHandlerMapping       和一个AnnotationMethodHandlerAdapter实例       这两个实例分别在类级别和方法级别处理。       而annotation-driven配置帮助我们自动完成上述两个实例的注入。    -->  
  <mvc:annotation-driven/>  
  <!-- 视图解析器 -->  
  <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver"> 
    <!-- 前缀 -->  
    <property name="prefix" value="/WEB-INF/jsp/"/>  
    <!-- 后缀 -->  
    <property name="suffix" value=".jsp"/> 
  </bean> 
</beans>

```

在视图解析器中我们把所有的视图都存放在/WEB-INF/目录下，这样可以保证视图安全，因为这个目录下的文件，客户端不能直接访问。

- 让IOC的注解生效
- 静态资源过滤 ：HTML、JS 、CSS 、图片、视频

- MVC的注解驱动
- 配置视图解析器

6、编写Controller

```java
package com.kuang.controller;

import org.springframework.stereotype.Controller;

import org.springframework.ui.Model;

import org.springframework.web.bind.annotation.RequestMapping;


@Controller
@RequestMapping("/HelloController")
public class HelloController {
    //真实访问地址 : 项目名/HelloController/hello
    @RequestMapping("/hello")
    public String sayHello(Model model) {
        model.addAttribute("msg", "hello,SpringMVC"); //向模型中添加属性msg与值，可以在JSP页面中取出并渲染
        return "hello"; //web-inf/jsp/hello.jsp       
    }
}

```

7、创建视图

1. 在WEB-INF/ jsp目录中创建hello.jsp ， 视图可以直接取出并展示从Controller带回的信息；
2. 可以通过EL表示取出Model中存放的值，或者对象；

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html
><head> 
<title>SpringMVC</title>
</head>
<body>${msg}</body>
</html>
```

### 使用Servlet API

```java
    @RequestMapping("/param.do")
    public void update(HttpServletRequest request, 
                           HttpServletResponse response,
                           HttpSession session) 
                throws IOException {
       request.setAttribute("request","一点教程网");
       session.setAttribute("session","一点教程网");
       response.sendRedirect("/success.jsp");
    }
```

### 解决乱码

web.xml配置添加过滤器

```xml
    <!--字符编码过滤器-->
    <filter>
    	<filter-name>characterEncodingFilter</filter-name>
    	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    	<init-param>
    		<!--指定转换的编码-->
    		<param-name>encoding</param-name>
    		<param-value>UTF-8</param-value>
    	</init-param>
    </filter>
    <filter-mapping>
    	<filter-name>characterEncodingFilter</filter-name>
    	<url-pattern>/*</url-pattern>
    </filter-mapping>
```

### 整合SSM

## 4、RestFul和控制器

### **RestFul **

**风格**

Restful就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

**功能**

资源：互联网所有的事物都可以被抽象为资源

资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作。

分别对应 添加、 删除、修改、查询。

```
传统方式操作资源 ：通过不同的参数来实现不同的效果！方法单一，post 和 get
	http://127.0.0.1/item/queryItem.action?id=1 查询,GET
	http://127.0.0.1/item/saveItem.action 新增,POST
	http://127.0.0.1/item/updateItem.action 更新,POST
	http://127.0.0.1/item/deleteItem.action?id=1 删除,GET或POST
使用RESTful操作资源 ：可以通过不同的请求方式来实现不同的效果！如下：请求地址一样，但是功能可以不同！
	http://127.0.0.1/item/1 查询,GET
	http://127.0.0.1/item 新增,POST
	http://127.0.0.1/item 更新,PUT
	http://127.0.0.1/item/1 删除,DELETE
```

### **控制器**

- 控制器复杂提供访问应用程序的行为，通常通过接口定义或注解定义两种方法实现。
- 控制器负责解析用户的请求并将其转换为一个模型。

- 在Spring MVC中一个控制器类可以包含多个方法
- 在Spring MVC中，对于Controller的配置方式有很多种

**Controller接口**

```java
//实现该接口的类获得控制器功能
public interface Controller {
   //处理请求且返回一个模型与视图对象
   ModelAndView handleRequest(HttpServletRequest var1, HttpServletResponse var2) throws Exception;
}
```

**使用注解实现Controller**

- @Controller注解类型用于声明Spring类的实例是一个控制器
- Spring可以使用扫描机制来找到应用程序中所有基于注解的控制器类，需要在配置文件中声明组件扫描。

```xml
<!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
<context:component-scan base-package="com.kuang.controller"/>
```

- 在编写类的时候使用相应的注解即可

### Model与ModelMap

Spring MVC应用中，我们经常需要在Controller将数据传递到JSP页面，除了可以通过HttpServletRequest域传递外，Spring MVC还提供了两个Api，分别为Model接口和ModelMap类。

- Model 只有寥寥几个方法只适合用于储存数据，简化了新手对于Model对象的操作和理解；

- ModelMap 继承了 LinkedMap ，除了实现了自身的一些方法，同样的继承 LinkedMap 的方法和特性；
- ModelAndView 可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转。

**Controller**

```java
    package com.yiidian.controller;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.ui.ModelMap;
    import org.springframework.web.bind.annotation.RequestMapping;
    
    /**
     *  Model与ModelMap的使用
     */
    @Controller
    public class ModelController {
    
        /**
         * Model接口的使用
         * @return
         */
        @RequestMapping("/model")
        public String list(Model model){
            model.addAttribute("model","Model-一点教程网");
            return "success";
        }
    
        /**
         * ModelMap类的使用
         * @return
         */
        @RequestMapping("/modelMap")
        public String list(ModelMap modelMap){
            modelMap.addAttribute("modelMap","ModelMap-一点教程网");
            return "success";
        }
    }
```

**视图jsp**

```jsp
   此代码由 【Java 技术驿站】整理
    
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>一点教程网-提示页面</title>
    </head>
    <body>
    获取Model数据-${requestScope.model}
    <hr/>
    获取ModelMap数据-${requestScope.modelMap}
    </body>
    </html>
```

### 文件上传

文件上传是表现层常见的需求，在Spring MVC中底层使用Apache的Commons FileUpload工具来完成文件上传，对其进行封装，让开发者使用起来更加方便。

**导入common-fileupload包**

```xml
    <!-- commons-fileUpload -->
    <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.1</version>
    </dependency>
```

**配置文件解析器**

```xml
   <!-- 配置文件上传解析器
     注意：必须配置id，且名称必须为multipartResolver
    -->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!-- 配置限制文件上传大小 (字节为单位)-->
        <property name="maxUploadSize" value="1024000"/>
    </bean>
```

注意几个点：

1. 必须配置`CommonsMultipartResolver`解析器
2. 该解析器的id必须叫multipartResolver，否则无法成功接收文件
3. 可以通过`maxUploadSize`属性限制文件上传大小

**设计文件上传表单**

```jsp
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>一点教程网-文件上传</title>
    </head>
    <body>
    <h3>SpringMVC方式文件上传</h3>
    <form action="/upload" method="post" enctype="multipart/form-data">
        选择文件：<input type="file" name="imgFile"> <br/>
        文件描述：<input type="text" name="memo"> <br/>
        <input type="submit" value="上传">
    </form>
    </body>
    </html>
```

上传表单注意以下几点：

1. 表单的enctype必须改为multipart/form-data
2. 表单提交方式必须为POST，不能是GET

**编写控制器接收文件及参数**

```java
    package com.yiidian.controller;
    import com.yiidian.domain.User;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.ui.ModelMap;
    import org.springframework.web.bind.annotation.RequestBody;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.ResponseBody;
    import org.springframework.web.multipart.MultipartFile;
    
    import javax.servlet.http.HttpServletRequest;
    import java.io.File;
    import java.io.IOException;
    import java.util.UUID;
    
    /**
     *  演示Spring MVC文件上传
     */
    @Controller
    public class UploadController {
    
        /**
         * 接收文件
         */
        @RequestMapping("/upload")
        public String upload(HttpServletRequest request, MultipartFile imgFile,String memo){
            //1.获取网站的upload目录的路径： ServletContext对象
            String upload = request.getSession().getServletContext().getRealPath("/upload");
    
            //判断该目录是否存在，不存在，自己创建
            File uploadFile = new File(upload);
            if(!uploadFile.exists()){
                uploadFile.mkdir();
            }
    
            //把文件保存到upload目录
    
            //2.生成随机文件名称
            //2.1 原来的文件名
            String oldName = imgFile.getOriginalFilename();
    
            //2.2 随机生成文件名
            String uuid = UUID.randomUUID().toString();
            //2.3 获取文件后缀
            String extName = oldName.substring(oldName.lastIndexOf(".")); //.jpg
    
            //2.4 最终的文件名
            String fileName = uuid+extName;
    
            //3.保存
            try {
                imgFile.transferTo(new File(upload+"/"+fileName));
            } catch (IOException e) {
                e.printStackTrace();
            }
            System.out.println("文件描述："+memo);
    
            return "success";
    
        }
    }
```

注意：这里使用`MultipartFile`对象接收文件，并把文件存放在项目的upload目录下，同时还接收了普通参数。

### 文件下载

**下载页面**

```jsp
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>一点教程网-文件下载</title>
    </head>
    <body>
    <h3>SpringMVC文件下载</h3>
    <a href="/down">下载</a>
    </body>
    </html>
```

**编写DowmController**

```java
    package com.yiidian.controller;
    import com.yiidian.domain.User;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.ui.ModelMap;
    import org.springframework.web.bind.annotation.RequestBody;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.ResponseBody;
    import org.springframework.web.multipart.MultipartFile;
    
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import javax.servlet.http.HttpSession;
    import java.io.*;
    import java.util.UUID;
    
    /**
     *  演示Spring MVC文件下载
     * 一点教程网 - www.yiidian.com
     */
    @Controller
    public class DownController {
    
        /**
         * 下载文件
         */
        @RequestMapping("/down")
        public void upload(HttpSession session, HttpServletResponse response) throws Exception {
            //模拟文件下载
            //1.读取需要下载的文件
            InputStream inputStream = session.getServletContext()
                    .getResourceAsStream("/upload/spring.jpg");
    
            //2.输出文件
            //设置响应头
            response.setHeader("Content-Disposition","attachment;filename=spring.jpg");
            OutputStream outputStream = response.getOutputStream();
    
            byte[] buf = new byte[1024];
            int len = 0;
            while( (len = inputStream.read(buf))!=-1 ){
                outputStream.write(buf,0,len);
            }
    
            //3.关闭资源
            outputStream.close();
            inputStream.close();
        }
    }
```

### 拦截器

Spring MVC中的拦截器（Interceptor）类似于Servlet中的过滤器（Filter），它主要用于拦截用户请求并作相应的处理。例如通过拦截器可以进行权限验证、记录请求信息的日志、判断用户是否登录等。

要使用Spring MVC中的拦截器，就需要对拦截器类进行定义和配置。通常拦截器类可以通过两种方式来定义。

1. 通过实现HandlerInterceptor接口
2. 继承HandlerInterceptor接口的实现类（如：HandlerInterceptorAdapter）来定义。

**例子**

```java
    package com.yiidian.interceptor;
    
    import org.springframework.web.servlet.HandlerInterceptor;
    import org.springframework.web.servlet.ModelAndView;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    /**
     *  自定义拦截器
     *一点教程网 - www.yiidian.com
     */
    public class Demo1Interceptor implements HandlerInterceptor{
        /**
         *preHandle: 在控制器(目标）的方法之前被执行
         *   返回值：控制afterCompletion方法是否被执行
         *       true: 执行afterCompletion
         *       false: 不执行afterCompletion
         */
        @Override
        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
            System.out.println("1.Demo1Interceptor的preHandle");
            return true;
        }
    
        /**
         * postHandle: 在控制器（目标）的方法成功执行完成之后（注意：控制器方法失败不执行）
         */
        @Override
        public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
            System.out.println("5.Demo1Interceptor的postHandle");
        }
    
        /**
         *  afterCompletion: 在执行完前面所有（拦截器和目标）的方法之后执行（注意: 不管控制器方法执行成功与否都会被执行 ）
         */
        @Override
        public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
            System.out.println("7.Demo1Interceptor的afterCompletion");
        }
    }
```

注意：一个拦截器和多个拦截器的执行顺序看下图。

一个拦截器的执行顺序：
![202105031014397281.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105031014397281.png)

多个拦截器的执行顺序：

![202105031014399192.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105031014399192.png)

**配置：**

```xml
    <!-- 配置拦截器 -->
    <mvc:interceptors>
        <!-- 配置2个拦截器 -->
        <mvc:interceptor>
            <mvc:mapping path="/hello"/>
            <bean class="com.yiidian.interceptor.Demo1Interceptor"/>
        </mvc:interceptor>
    
        <mvc:interceptor>
            <mvc:mapping path="/hello"/>
            <bean class="com.yiidian.interceptor.Demo2Interceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

注意：拦截器配置的顺序决定了拦截器的执行顺序，先配置会先被执行！

### 异常处理

在控制器的方法发生异常后，默认情况会显示Tomcat的500页面，这种用户体验并不好。Spring MVC提供了两种全局异常处理机制：

1. 定义异常类，实现HandlerExceptionResolver接口
2. 定义异常类，使用@ControllerAdvice+@ExceptionHandler注解

**全局异常类方式一：**

```java
    package com.yiidian.exception;
    
    import org.springframework.web.servlet.HandlerExceptionResolver;
    import org.springframework.web.servlet.ModelAndView;
    
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    
    /**
     * 方式一：自定义异常处理类
     */
    public class MyCustomException1 implements HandlerExceptionResolver{
        @Override
        public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
            ModelAndView mv = new ModelAndView();
            mv.addObject("errorMsg",e.getMessage());
            mv.setViewName("error");
            return mv;
        }
    }
```

**全局异常类方式二：**

```java
    package com.yiidian.exception;
    
    import org.springframework.web.bind.annotation.ControllerAdvice;
    import org.springframework.web.bind.annotation.ExceptionHandler;
    import org.springframework.web.servlet.HandlerExceptionResolver;
    import org.springframework.web.servlet.ModelAndView;
    
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    
    /**
     * 方式二：自定义异常处理类
     */
    @ControllerAdvice
    public class MyCustomException2{
    
        @ExceptionHandler
        public ModelAndView handlerError(Exception e){
            ModelAndView mv = new ModelAndView();
            mv.setViewName("error");
            mv.addObject("errorMsg",e.getMessage());
            return mv;
        }
    
    }
    
```

配置：

```xml
    <!--创建自定义异常处理对象，方式一-->
    <bean class="com.yiidian.exception.MyCustomException1"/>
    <!--创建自定义异常处理对象，方式二-->
    <bean class="com.yiidian.exception.MyCustomException1"/>
```

### 表单验证

- Spring MVC表单数据验证需要先依赖相关包
- 编写Pojo，指定验证规则

表单数据验证的重点是在Pojo类使用相关注解指定每个属性的验证规则。以下为可以使用的注解：

| 注解名称                  | 说明                                                     |
| ------------------------- | -------------------------------------------------------- |
| @Null                     | 被注释的元素必须为null                                   |
| @NotNull                  | 被注释的元素必须不为null                                 |
| @AssertTrue               | 被注释的元素必须为true                                   |
| @AssertFalse              | 被注释的元素必须为false                                  |
| @Min(value)               | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值 |
| @Max(value)               | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值 |
| @DecimalMin(value)        | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值 |
| @DecimalMax(value)        | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值 |
| @Size(max,min)            | 被注释的元素的大小必须在指定的范围内                     |
| @Digits(integer,fraction) | 被注释的元素必须是一个数字，其值必须在可接受的范围内     |
| @Past                     | 被注释的元素必须是一个过去的日期                         |
| @Future                   | 被注释的元素必须是一个将来的日期                         |
| @Pattern(value)           | 被注释的元素必须符合指定的正则表达式                     |



### 返回值

- 普通字符串：自动结合视图解析器，跳到相应页面
- 转发字符串：forward:完整页面的路径，转发到特定页面
- 重定向字符串：redirect:完整页面的路径，请求到特定页面
- void：无需返回，例如文件下载等
- ModelAndView：对象既可以存储数据到request域，也可以设置视图，是一个比较底层的对象。
- Java对象：可能是普通JavaBean、List或Map集合等。一般希望把控制器的返回Java对象转换为Json字符串，才需要返回Java对象。

### Json数据转换

- Spring MVC默认是无法实现Json数据转换功能的，需要额外导入Jackson包来支持Json数据转换。
- 编写json.jsp，使用jQuery实现ajax异步请求后端Controller，同时发送Json字符串对象
- 编写JsonController，这里用到两个关键注解`@RequestBody`和`@ResponseBody`，返回相应对象
- 

## 5、常用注解

### @RequestMapping

用于映射url到控制器类或一个特定的处理程序方法。可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。

**value属性**：指定控制器的方法URI

**method属性**：指定请求的method类型，可以接受GET,POST,PUT,DELETE等

**consumes、produces属性**：

- consumes：指定处理请求的提交内容类型（Content-Type），例如application/json, text/html;
- produces：指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回。

**params、headers属性**：

- params：指定request中必须包含某些参数值，才让该方法处理。
- headers：指定request中必须包含某些指定的header值，才能让该方法处理请求。

### @RequestParam

注意：表单的name和控制器的形参并不一致，但是@RequestParam注解的value值必须和表单的name保持一致。

![202105031013223201.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105031013223201.png)

另外，@RequestParam注解还有两个属性：

1. required：参数是否必须。代表页面是否必须传递该参数。如果该值为true，但没有传递参数，会报错。
2. defaultValue：默认值。代表如果页面没有传递该参数，使用defaultValue的值代替。

### @RequestBody

完成页面Json字符串转换为后端Java对象

### @ResponseBody

完成后端Java对象转换为前端Json字符串

### @CookieValue

方便我们获取指定Cookie数据。

### @ModelAttribute

将请求参数绑定到Model对象。被@ModelAttribute注释的方法会在Controller每个方法执行前被执行（如果在一个Controller映射到多个URL时，要谨慎使用）。

在SpringMVC的Controller中使用`@ModelAttribute`时，其位置包括下面三种：

1. 应用在方法上
2. 应用在方法的参数上，本质相当于从Model对象中取出对应的属性值。
3. 应用在方法上，并且方法同时使用@RequestMapping

### @SessionAttributes

默认情况下Spring MVC将模型中的数据存储到request域中。当一个请求结束后，数据就失效了。如果要跨页面使用。那么需要使用到session。而@SessionAttributes注解就可以使得模型中的数据存储一份到session域中。

###  **@PathVariable**

让@RequestMapping注解的方法参数的值对应绑定到一个URI模板变量上。

## 6、参数封装

**基本类型封装：**方法形参与前端表单属性名字相同即可自动映射。

**POJO参数封装：**POJO属性名字与前端表单属性名字相同即可自动映射。

- 若内部含有对象，则前端使用内部对象名配合属性映射。
- 若内部含有List，则前端使用列表名及索引配合属性映射。
- 若内部含有Map，则前端使用相应键值对配合属性映射。

**自定义类型转换：**

希望把字符串参数转换为日期类型，必须自定义类型转换器。接下来讲解如何自定义类型转换器。

```java
    package com.yiidian.converter;
    import org.springframework.core.convert.converter.Converter;
    import java.text.ParseException;
    import java.text.SimpleDateFormat;
    import java.util.Date;
    /**
     * 字符串转换日期类型转换器
     */
    public class StringToDateConverter implements Converter<String,Date>{
        @Override
        public Date convert(String source) {
            Date date = null;
            try {
                //使用SimpleDateFormat对页面字符串日期转换为java.util.Date类型
                date = new SimpleDateFormat("yyyy-MM-dd").parse(source);
            } catch (ParseException e) {
                e.printStackTrace();
            }
            return date;
        }
    }
```

配置自定义类型转换器

```xml
      <bean id="stringToDateConverter" class="com.yiidian.converter.StringToDateConverter"/>
```

# 高级

## 1、介绍下Spring MVC



Spring 配备构建Web 应用的全功能MVC框架。Spring可以很便捷地和其他MVC框架集成，如Struts，Spring 的MVC框架用控制反转把业务对象和控制逻辑清晰地隔离。它也允许以声明的方式把请求参数和业务对象绑定。

**DispatcherServlet**

Spring的MVC框架是围绕DispatcherServlet来设计的，它用来处理所有的HTTP请求和响应。

**WebApplicationContext**

WebApplicationContext 继承了ApplicationContext  并增加了一些WEB应用必备的特有功能，它不同于一般的ApplicationContext ，因为它能处理主题，并找到被关联的servlet。

**Spring MVC框架的控制器**

控制器提供一个访问应用程序的行为，此行为通常通过服务接口实现。控制器解析用户输入并将其转换为一个由视图呈现给用户的模型。Spring用一个非常抽象的方式实现了一个控制层，允许用户创建多种用途的控制器。

**@Controller 注解**

该注解表明该类扮演控制器的角色，Spring不需要你继承任何其他控制器基类或引用Servlet API。

**@RequestMapping 注解**

该注解是用来映射一个URL到一个类或一个特定的方处理法上。

## 2、Spring MVC原理

![图片](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/640.jpg)

1. 向服务器发送 HTTP 请求，请求被前端控制器 DispatcherServlet 捕获。
2. DispatcherServlet 根据 -servlet.xml 中的配置对请求的 URL 进行解析，得到请求资源标识符（URI）。然后根据该 URI，调用 HandlerMapping 获得该 Handler 配置的所有相关的对象（包括 Handler 对象以及 Handler 对象对应的拦截器），最后以HandlerExecutionChain 对象的形式返回。
3. DispatcherServlet 根据获得的Handler，选择一个合适的 HandlerAdapter。（附注：如果成功获得HandlerAdapter后，此时将开始执行拦截器的 preHandler(…)方法）。
4. 提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller)。在填充Handler的入参过程中，根据你的配置，Spring 将帮你做一些额外的工作：
   - HttpMessageConveter：将请求消息（如 Json、xml 等数据）转换成一个对象，将对象转换为指定的响应信息。
   - 数据转换：对请求消息进行数据转换。如`String`转换成`Integer`、`Double`等。
   - 数据根式化：对请求消息进行数据格式化。如将字符串转换成格式化数字或格式化日期等。
   - 数据验证：验证数据的有效性（长度、格式等），验证结果存储到`BindingResult`或`Error`中。

5.Handler(Controller)执行完成后，向 DispatcherServlet 返回一个 ModelAndView 对象；

6.根据返回的ModelAndView，选择一个适合的 ViewResolver（必须是已经注册到 Spring 容器中的ViewResolver)返回给DispatcherServlet。

7.ViewResolver 结合Model和View，来渲染视图。

8.视图负责将渲染结果返回给客户端。

**核心原理**

本质上将controller对象都托管到ioc容器后，运用反射扫描controller对象中的Method的方法，提取相应的*@RequestMapping*等注解，将访问路径和方法连接起来。

通过访问路径，即可通过反射调用相应方法进行处理，期间在方法调用前以及调用后可以运用反射增强。 
