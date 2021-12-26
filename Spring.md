

Spring 是个java企业级应用的开源开发框架。Spring主要用来开发Java应用，但是有些扩展是针对构建J2EE平台的web应用。Spring 框架目标是简化Java企业级应用开发，并通过POJO为基础的编程模型促进良好的编程习惯。

# 1、使用Spring框架的好处是什么？

轻量：Spring 是轻量的，基本的版本大约2MB

控制反转（IOC）：Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们

面向切面的编程(AOP)：Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开

容器：Spring 包含并管理应用中对象的生命周期和配置

MVC框架：Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品

事务管理：Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）

异常处理：Spring 提供方便的API把具体技术相关的异常（比如由JDBC，Hibernate or JDO抛出的）转化为一致的unchecked 异常

# 2、Spring由哪些模块组成？

1. **Core Container - 核心容器（IOC）**

- spring - core：Spring 中的核心工具类包

- spring - beans：Spring 中定义 bean 的组件

- spring - context：Spring 的运行容器

- spring - context - support：Spring 容器的扩展支持

- spring - expression：Spring 的表达式语言支持

2. **AOP - 面向切面支持**

- spring - aop：基于代理的 AOP 支持

- spring - aspects：集成 Aspects 的 AOP 支持

3. **WEB（MVC）**

- spring - web：提供 web 的基础功能

- spring - webmvc：提供 springmvc 的功能

- spring - websocket：提供 web socket 支持

- spring - webmvc - portlet：提供 Portlet 环境的支持

4. **Data Access/Integration - 数据访问 / 集成**

- spring - jdbc：提供对 jdbc 连接的封装功能

- spring - tx：提供对事务的支持

- spring - orm：提供对象 - 关系映射支持

- spring - oxm：提供对象 - XML 映射支持

- spring - jms：提供消息队列的支持

5. **Test - 测试**

- spring - test：提供对测试功能的支持

## 一、核心容器模块

这是基本的Spring模块，提供spring 框架的基础功能，BeanFactory 是 任何以spring为基础的应用的核心。Spring 框架建立在此模块之上，它使Spring成为一个容器。

**BeanFactory 工厂是工厂模式的一个实现**，提供了控制反转功能，用来把应用的配置和依赖从正真的应用代码中分离。最常用的BeanFactory 实现是XmlBeanFactory 类。它根据XML文件中的定义加载beans。该容器从XML 文件读取配置元数据并用它去创建一个完全配置的系统或应用。

## 二、AOP模块

AOP模块用于发给我们的Spring应用做面向切面的开发， 很多支持由AOP联盟提供，这样就确保了Spring和其他AOP框架的共通性。这个模块将元数据编程引入Spring。



## 三、JDBC抽象和DAO模块

通过使用JDBC抽象和DAO模块，保证数据库代码的简洁，并能避免数据库资源错误关闭导致的问题，它在各种不同的数据库的错误信息之上，提供了一个统一的异常访问层。它还利用Spring的AOP 模块给Spring应用中的对象提供事务管理服务。



## 四、对象/关系映射集成模块

Spring 通过提供ORM模块，支持我们在直接JDBC之上使用一个对象/关系映射映射(ORM)工具，Spring 支持集成主流的ORM框架，如Hiberate,JDO和 iBATIS SQL Maps。Spring的事务管理同样支持以上所有ORM框架及JDBC。



## 五、WEB 模块

Spring的WEB模块是构建在application context 模块基础之上，提供一个适合web应用的上下文。这个模块也包括支持多种面向web的任务，如透明地处理多个文件上传请求和程序级请求参数的绑定到你的业务对象。它也有对Jakarta Struts的支持。

# 3、Spring 配置文件

Spring配置文件是个XML 文件，这个文件包含了类信息，描述了如何配置它们，以及如何相互调用。



# 4、Spring IOC 容器

![202105092051244201.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105092051244201.png)

Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

**优点**：IOC 或 依赖注入把应用的代码量降到最低。它使应用容易测试，单元测试不再需要单例和JNDI查找机制。最小的代价和最小的侵入性使松散耦合得以实现。IOC容器支持加载服务时的饿汉式初始化和懒加载。

## Spring 依赖注入

依赖注入，是IOC的一个方面，是个通常的概念，它有多种解释。这概念是说你不用创建对象，而只需要描述它如何被创建。你不在代码里直接组装你的组件和服务，但是要在配置文件里描述哪些组件需要哪些服务，之后一个容器（IOC容器）负责把他们组装起来。

**有哪些不同类型的IOC（依赖注入）方式？**

构造器依赖注入：构造器依赖注入通过容器触发一个类的构造器来实现的，该类有一系列参数，每个参数代表一个对其他类的依赖。

Setter方法注入：Setter方法注入是容器通过调用无参构造器或无参static工厂 方法实例化bean之后，调用该bean的setter方法，即实现了基于setter的依赖注入。

**哪种依赖注入方式你建议使用，构造器注入，还是 Setter方法注入？**

你两种依赖方式都可以使用，构造器注入和Setter方法注入。最好的解决方案是用构造器参数实现强制依赖，setter方法实现可选依赖。

**ApplicationContext通常的实现是什么？**

FileSystemXmlApplicationContext ：此容器从一个XML文件中加载beans的定义，XML Bean 配置文件的全路径名必须提供给它的构造函数。

ClassPathXmlApplicationContext：此容器也从一个XML文件中加载beans的定义，这里，你需要正确设置classpath因为这个容器将在classpath里找bean配置。

WebXmlApplicationContext：此容器加载一个XML文件，此文件定义了一个WEB应用的所有bean。



**Bean 工厂和 Application contexts  有什么区别？**

- BeanFactory - BeanFactory 就像一个包含 bean 集合的工厂类。它会在客户端要求时实例化 bean。
- ApplicationContext - ApplicationContext 接口扩展了 BeanFactory 接口。它在 BeanFactory 基础上提供了一些额外的功能。

| BeanFactory                | ApplicationContext       |
| :------------------------- | :----------------------- |
| 它使用懒加载               | 它使用即时加载           |
| 它使用语法显式提供资源对象 | 它自己创建和管理资源对象 |
| 不支持国际化               | 支持国际化               |
| 不支持基于依赖的注解       | 支持基于依赖的注解       |

 **Spring FactoryBean和BeanFactory 区别**

- BeanFactory是个bean 工厂，是一个工厂类(接口)， 它负责生产和管理bean的一个工厂
  是ioc 容器最底层的接口，是个ioc容器，是spring用来管理和装配普通bean的ioc容器。
- FactoryBean是个bean，在IOC容器的基础上给Bean的实现加上了一个简单工厂模式和装饰模式，是一个可以生产对象和装饰对象的工厂bean，由spring管理后，生产的对象是由getObject()方法决定的。

## bean 的转换过程

下面这张图演示了一个可用的 bean 是如何从 xml 配置文件中演变过来的。

![202105092052545181.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105092052545181.png)

## ApplicationContext 的架构图

![202105092052551332.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105092052551332.png)

## loadBean 的全流程

![202105092052560473.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105092052560473.png)

## getBean 的全流程

![202105092052566254](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105092052566254.png)

# 5、Spring Beans

- 它们是构成用户应用程序主干的对象。
- Bean 由 Spring IoC 容器管理。
- 它们由 Spring IoC 容器实例化，配置，装配和管理。
- Bean 是基于用户提供给容器的配置元数据创建。

当定义一个<bean> 在Spring里，我们还能给这个bean声明一个作用域。scope属性可为prototype、singleton。

**一个 Spring Bean 定义 包含什么？**

一个Spring Bean 的定义包含容器必知的所有配置元数据，包括如何创建一个bean，它的生命周期详情及它的依赖。

**如何给Spring 容器提供配置元数据？**

这里有三种重要的方法给Spring 容器提供配置元数据。

- XML配置文件。

- 基于注解的配置。

- 基于java的配置。

**什么是Spring的内部bean？**

当一个bean仅被用作另一个bean的属性时，它能被声明为一个内部bean，为了定义inner bean，在Spring 的 基于XML的 配置元数据中，可以在 <property/>或 <constructor-arg/> 元素内使用<bean/> 元素，内部bean通常是匿名的，它们的Scope一般是prototype。

**在 Spring中如何注入一个java集合？**

Spring提供以下几种集合的配置元素：

<list>类型用于注入一列值，允许有相同的值。

<set> 类型用于注入一组值，不允许有相同的值。

<map> 类型用于注入一组键值对，键和值都可以为任意类型。

<props>类型用于注入一组键值对，键和值都只能为String类型。

**解释不同方式的自动装配**

有五种自动装配的方式，可以用来指导Spring容器用自动装配方式来进行依赖注入

- no：默认的方式是不进行自动装配，通过显式设置ref 属性来进行装配。

- byName：通过参数名 自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byname，之后容器试图匹配、装配和该bean的属性具有相同名字的bean。

- byType：通过参数类型自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byType，之后容器试图匹配、装配和该bean的属性具有相同类型的bean。如果有多个bean符合条件，则抛出错误。

- constructor：这个方式类似于byType， 但是要提供给构造器参数，如果没有确定的带参数的构造器参数类型，将会抛出异常。

- autodetect：首先尝试使用constructor来自动装配，如果无法工作，则使用byType方式。



**自动装配有哪些局限性？**

自动装配的局限性是：

- 重写：你仍需用 <constructor-arg>和 <property> 配置来定义依赖，意味着总要重写自动装配。

- 基本数据类型：你不能自动装配简单的属性，如基本数据类型，String字符串，和类。

- 模糊特性：自动装配不如显式装配精确，如果有可能，建议使用显式装配。

**Spring中可以注入一个null 和一个空字符串**



**Spring中bean的作用域**

Spring框架支持以下五种bean的作用域：

- singleton : bean在每个Spring ioc 容器中只有一个实例。**非线程安全**

- prototype：一个bean的定义可以有多个实例。

- request：每次http请求都会创建一个bean，该作用域仅在基于web的Spring ApplicationContext情形下有效。

- session：在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。

- global-session：在一个全局的HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。

缺省的Spring bean 的作用域是Singleton。

**解决并发线程安全**

- 在Controller中使用ThreadLocal变量
- 在spring配置文件Controller中声明 scope="prototype"，每次都创建新的controller
- 避免使用成员变量
- 使用并发安全的类，如ConcurrentHashMap、ConcurrentHashSet

**Spring中bean生命周期**

1. Spring容器 从XML 文件中读取bean的定义，并实例化bean。
2. Spring根据bean的定义填充所有的属性。
3. 如果bean实现了BeanNameAware 接口，Spring 传递bean 的ID 到 setBeanName方法。
4. 如果Bean 实现了 BeanFactoryAware 接口， Spring传递beanfactory 给setBeanFactory 方法。
5. 如果有任何与bean相关联的BeanPostProcessors，Spring会在postProcesserBeforeInitialization()方法内调用它们。
6. 如果bean实现IntializingBean了，调用它的afterPropertySet方法，如果bean声明了初始化方法，调用此初始化方法。
7. 如果有BeanPostProcessors 和bean 关联，这些bean的postProcessAfterInitialization() 方法将被调用。
8. 如果bean实现了 DisposableBean，它将调用destroy()方法。



**哪些是重要的bean生命周期方法？ 你能重载它们吗？**

有两个重要的bean 生命周期方法，第一个是setup ， 它是在容器加载bean的时候被调用。第二个方法是 teardown  它是在容器卸载类的时候被调用。

The bean 标签有两个重要的属性（init-method和destroy-method）。用它们你可以自己定制初始化和注销方法。它们也有相应的注解（@PostConstruct和@PreDestroy）。



# 6、Spring 注解



**什么是基于Java的Spring注解配置? **

基于Java的配置，允许你在少量的Java注解的帮助下，进行你的大部分Spring配置而非通过XML文件。



**什么是基于注解的容器配置？**

相对于XML文件，注解型的配置依赖于通过字节码元数据装配组件，而非尖括号的声明。

开发者通过在相应的类，方法或属性上使用注解的方式，直接组件类中进行配置，而不是使用xml表述bean的装配关系。



**怎样开启注解装配？**

注解装配在默认情况下是不开启的，为了使用注解装配，我们必须在Spring配置文件中配置 <context:annotation-config/>元素。



**@Required  注解（必须在XML配置）**

这个注解表明bean的属性必须在配置的时候设置，通过一个bean定义的显式的属性值或通过自动装配，若@Required注解的bean属性未被设置，容器将抛出BeanInitializationException。



**@Autowired 注解（默认按照类型查找）**

@Autowired 注解提供了更细粒度的控制，包括在何处以及如何完成自动装配。它的用法和@Required一样，修饰setter方法、构造器、属性或者具有任意名称和/或多个参数的PN方法。

**@Resource（默认按照名字查找）**

**@Qualifier 注解（添加名称）**

当有多个相同类型的bean却只有一个需要自动装配时，将@Qualifier 注解和@Autowire 注解结合使用以消除这种混淆，指定需要装配的确切的bean。

**@Component, @Controller, @Repository, @Service 有何区别？**

- @Component：这将 java 类标记为 bean。它是任何 Spring 管理组件的通用构造型。spring 的组件扫描机制现在可以将其拾取并将其拉入应用程序环境中。
- @Controller：这将一个类标记为 Spring Web MVC 控制器。标有它的 Bean 会自动导入到 IoC 容器中。
- @Service：此注解是组件注解的特化。它不会对 @Component 注解提供任何其他行为。您可以在服务层类中使用 @Service 而不是 @Component，因为它以更好的方式指定了意图。
- @Repository：这个注解是具有类似用途和功能的 @Component 注解的特化。它为 DAO 提供了额外的好处。它将 DAO 导入 IoC 容器，并使未经检查的异常有资格转换为 Spring DataAccessException。

# 7、Spring 数据访问





**在Spring框架中如何更有效地使用JDBC？**

使用SpringJDBC 框架，资源管理和错误处理的代价都会被减轻。所以开发者只需写statements 和 queries从数据存取数据，JDBC也可以在Spring框架提供的模板类的帮助下更有效地被使用，这个模板叫JdbcTemplate 



**JdbcTemplate**

JdbcTemplate 类提供了很多便利的方法解决诸如把数据库数据转变成基本数据类型或对象，执行写好的或可调用的数据库操作语句，提供自定义的数据错误处理。



**Spring对DAO的支持**

Spring对数据访问对象（DAO）的支持旨在简化它和数据访问技术如JDBC，Hibernate or JDO 结合使用。这使我们可以方便切换持久层。编码时也不用担心会捕获每种技术特有的异常。



**使用Spring通过什么方式访问Hibernate？**

在Spring中有两种方式访问Hibernate：

控制反转  Hibernate Template和 Callback

继承 HibernateDAOSupport提供一个AOP 拦截器



**Spring支持的ORM**

Spring支持以下ORM：

- Hibernate

- iBatis

- JPA (Java Persistence API)

- TopLink

- JDO (Java Data Objects)

- OJB



**如何通过HibernateDaoSupport将Spring和Hibernate结合起来？**

用Spring的 SessionFactory 调用 LocalSessionFactory。集成过程分三步：

- 配置the Hibernate SessionFactory

- 继承HibernateDaoSupport实现一个DAO

- 在AOP支持的事务中装配



**Spring支持的事务管理类型**

Spring支持两种类型的事务管理：

- 编程式事务管理：这意味你通过编程的方式管理事务，给你带来极大的灵活性，但是难维护。

- 声明式事务管理：这意味着你可以将业务代码和事务管理分离，你只需用注解和XML配置来管理事务。



**Spring框架的事务管理有哪些优点？**

- 它为不同的事务API  如 JTA，JDBC，Hibernate，JPA 和JDO，提供一个不变的编程模式。

- 它为编程式事务管理提供了一套简单的API而不是一些复杂的事务API如

- 它支持声明式事务管理。

- 它和Spring各种数据访问抽象层很好得集成。



**你更倾向用那种事务管理类型？**

大多数Spring框架的用户选择声明式事务管理，因为它对应用代码的影响最小，因此更符合一个无侵入的轻量级容器的思想。声明式事务管理要优于编程式事务管理，虽然比编程式事务管理（这种方式允许你通过代码控制事务）少了一点灵活性。



# 8、Spring 面向切面编程AOP

AOP的底层实际上是动态代理，动态代理分成了JDK动态代理、CGLib动态代理。**Spring AOP默认是使用JDK动态代理，如果代理的类没有接口则会使用CGLib代理。**（关于动态代理反射实现细节看Java笔记）

- JDK在创建代理对象时的性能要高于CGLib代理，而生成代理对象的运行性能却比CGLib的低。
- 如果是单例的代理，推荐使用CGLib

面向切面的编程，或AOP， 是一种编程技术，允许程序模块化横向切割关注点，或横切典型的责任划分，如日志和事务管理。



**Aspect 切面**

AOP核心就是切面，它将多个类的通用行为封装成可重用的模块，该模块含有一组API提供横切功能。比如，一个日志模块可以被称作日志的AOP切面。根据需求的不同，一个应用程序可以有若干切面。在Spring AOP中，切面通过带有@Aspect注解的类实现。



**在Spring AOP 中，关注点和横切关注的区别是什么？**

关注点是应用中一个模块的行为，一个关注点可能会被定义成一个我们想实现的一个功能。



横切关注点是一个关注点，此关注点是整个应用都会使用的功能，并影响整个应用，比如日志，安全和数据传输，几乎应用的每个模块都需要的功能。因此这些都属于横切关注点。



**连接点**

**能够被拦截的地方**：Spring AOP是基于动态代理的，所以是方法拦截的。每个成员方法都可以称之为连接点。



**切点**

**具体定位的连接点**：每个方法都可以称之为连接点，**具体定位到某一个方法就成为切点**。



**通知**

通知是个在方法执行前或执行后要做的动作，实际上是程序执行时要通过SpringAOP框架触发的代码段。

Spring切面可以应用五种类型的通知：

- before：前置通知，在一个方法执行前被调用
- after：在方法执行之后调用的通知，无论方法执行是否成功
- after-returning：仅当方法成功完成后执行的通知
- after-throwing：在方法抛出异常退出时执行的通知
- around：在方法执行之前和之后调用的通知
- 

**引入**

引入允许我们在已存在的类中增加新的方法和属性。



**目标对象**

被一个或者多个切面所通知的对象。它通常是一个代理对象。也指被通知（advised）对象。



**代理**

代理是通知目标对象后创建的对象。从客户端的角度看，代理对象和目标对象是一样的。

**有几种不同类型的自动代理**

BeanNameAutoProxyCreator

DefaultAdvisorAutoProxyCreator

Metadata autoproxying



**什么是织入。什么是织入应用的不同点**

织入是将切面和到其他应用类型或对象连接或创建一个被通知对象的过程。

织入可以在编译时，加载时，或运行时完成。



**解释基于XML Schema方式的切面实现**

在这种情况下，切面由常规类以及基于XML的配置实现。



**解释基于注解的切面实现**

在这种情况下(基于@AspectJ的实现)，涉及到的切面声明的风格与带有java5标注的普通java类一致。



<img src="https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221134622912.png" alt="image-20211221134622912" style="zoom: 67%;" />

<img src="https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221134641836.png" alt="image-20211221134641836" style="zoom:73%;" />

# 9、Spring 体现的设计模式

- **工厂设计模式** : Spring使用工厂模式通过 `BeanFactory`、`ApplicationContext` 创建 bean 对象。

- **代理设计模式** : Spring AOP 功能的实现。

- **单例设计模式** : Spring 中的 Bean 默认都是单例的。

- **模板方法模式** : Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。

- **包装器设计模式** : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。

- **观察者模式:** Spring 事件驱动模型就是观察者模式很经典的一个应用。

- **适配器模式** :Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller`。

# 10、Spring MVC



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

- 原理：
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