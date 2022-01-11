

# 基础入门



## 1.、Spring

### 1.1、简介

- Spring框架以interface21框架为基础，经过重新设计，并不断丰富其内涵，于2004年3月24日发布了1.0正式版

- Spring的理念：使用现有的技术更加容易使用，本身是一个大杂烩，整合了现有的技术框架！

- SSH：Struct2 + Spring + Hibernate（全自动持久化框架）！
- SSM：SpringMVC + Spring + MyBatis（半自动持久化框架，可自定义性质更强）！

Spring Web MVC和Spring-JDBC的pom配置文件：

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.7.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.7.RELEASE</version>
</dependency>
```

### 1.2 优点

- Spring是一个开源的免费的框架（容器）！
- Spring是一个轻量级的、非入侵式的框架！

- 控制反转（IoC），面向切面编程（AOP）
- 支持事务的处理，对框架整合的支持！

### 1.3 组成



![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614904072127-b0dffb54-9463-4142-a851-13ef568443ec.png)



### 1.4、扩展

现代化的java开发 -> 基于Spring的开发！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614904099889-8c9fe42c-fe3b-4986-b9df-ef1940069ac9.png)



- Spring Boot

- - 一个快速开发的脚手架
  - 基于SpringBoot可以快速开发单个微服务

- - 约定大于配置！

- Spring Cloud

- - SpringCloud是基于SpringBoot实现的！

因为现在大多数公司都在使用SpringBoot进行快速开发，学习SpringBoot的前提，需要完全掌握Spring及SpringMVC！承上启下的作用！

## 2、IoC（控制反转）

- 之前，程序是主动创建对象！**控制权在程序猿手上**！
- 使用了set注入后，程序不再具有主动性，而是变成了被动的接受对象！（**主动权在客户手上**）

本质上解决了问题，程序员不用再去管理对象的创建，系统的耦合性大大降低，可以更专注在业务的实现上。

这是IoC（控制反转）的原型，反转(理解)：主动权交给了用户

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614904805084-e687487f-aa98-4f35-8f2c-01f4d38827d5.png)



### IoC本质

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614904861083-7ce23078-1e66-420f-bee6-36c80bb83978.png)

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614904815154-e6a1a342-2d25-43e7-9ab1-ca2f98c3b6bd.png)

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614904822996-aac3e762-a48f-474a-a70d-73cb6db55a8c.png)

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614904850692-880cd6bc-b989-4942-b6e7-704bd88f4871.png)



## 3、HelloSpring

在父模块中导入jar包

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-webmvc</artifactId>
	<version>5.2.7.RELEASE</version>
</dependency>
```

pojo的Hello.java

```java
package pojo;

public class Hello {

	private String str;
	
	public String getStr() {
		return str;
	}

	public void setStr(String str) {
		this.str = str;
	}
	
	@Override
	public String toString() {
		return "Holle [str=" + str + "]";
	}
}
```

在resource里面的xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--在Spring中创建对象，在Spring这些都称为bean
    	类型 变量名 = new 类型();
    	Holle holle = new Holle();
    	
    	bean = 对象(holle)
    	id = 变量名(holle)
    	class = new的对象(new Holle();)
    	property 相当于给对象中的属性设值,让str="Spring"
    -->
    
    <bean id="hello" class="pojo.Hello">
        <property name="str" value="Spring"/>
    </bean>
</beans>
```

测试类MyTest

```java
package holle1;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import pojo.Hello;

public class MyTest {

	public static void main(String[] args) {
		//获取Spring的上下文对象
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		//我们的对象下能在都在spring·中管理了，我们要使用，直接取出来就可以了
		Hello holle = (Hello) context.getBean("hello");
		System.out.println(holle.toString());
	}

}
```

核心用set注入，所以必须要有下面的set()方法

```java
//Hello类
public void setStr(String str) {
		this.str = str;
	}
```

在前面的module试试引入Spring

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userDaomSql" class="dao.UserDaoMysqlImpl"></bean>

    <bean id="userServiceImpl" class="service.UserServiceImp">
        <!--ref引用spring中已经创建很好的对象-->
        <!--value是一个具体的值,基本数据类型-->
        <property name="userDao" ref="userDaomSql"/>
    </bean>

</beans>
```

第一个module改良后测试

```java
package holle0;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import service.UserServiceImpl;

public class MyTest0 {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		UserServiceImpl userServiceImpl = (UserServiceImpl) context.getBean("userServiceImpl");
		userServiceImpl.getUser();
	}
}
```

**总结：**

- 所有的类都要装配的beans.xml 里面；

- 所有的bean 都要通过容器去取；

- 容器里面取得的bean，拿出来就是一个对象，用对象调用方法即可；

## 4、IoC创建对象的方式

1. 使用无参构造创建对象，默认。
2. 使用有参构造（如下）

**下标赋值**

index指的是有参构造中参数的下标，下标从0开始;

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="pojo.User">
        <constructor-arg index="0" value="chen"/>
    </bean>
</beans>
```

**类型赋值（不建议使用）**

```xml
<bean id="user" class="pojo.User">
    <constructor-arg type="java.lang.String" value="kuang"/>
</bean>
```

**直接通过参数名（掌握），需有set方法**

```xml
<bean id="user" class="pojo.User">
    <constructor-arg name="name" value="kuang"></constructor-arg>
</bean>
<!-- 比如参数名是name，则有name="具体值" -->
```

总结：在配置文件加载的时候，容器(< bean>)中管理的对象就已经初始化了

## 5、Spring配置

### 5.1、别名

```xml
<bean id="user" class="pojo.User">
    <constructor-arg name="name" value="chen"></constructor-arg>
</bean>

<alias name="user" alias="userLove"/>
<!-- 使用时
	User user2 = (User) context.getBean("userLove");	
-->
```

### 5.2、Bean的配置

```xml
<!--id：bean的唯一标识符，也就是相当于我们学的对象名
class：bean对象所对应的会限定名：包名+类型
name：也是别名，而且name可以同时取多个别名 -->
<bean id="user" class="pojo.User" name="u1 u2,u3;u4">
    <property name="name" value="chen"/>
</bean>
<!-- 使用时
	User user2 = (User) context.getBean("u1");	
-->
```

### 5.3、import

import一般用于团队开发使用，它可以将多个配置文件，导入合并为一个

```xml
<import resource="beans.xm1"/>
<import resource="beans2.xml"/>
<import resource="beans3.xm1"/>
```

**使用的时候，直接使用总的配置就可以了，按照在总的xml中的导入顺序来进行创建，后导入的会重写先导入的，最终实例化的对象会是后导入xml中的那个**

## 6、依赖注入（DI）

### 6.1、构造器注入

第4点有提到

### 6.2、set方式注入【重点】

依赖注入：set注入！

- 依赖：bean对象的创建依赖于容器
- 注入：bean对象中的所有属性，由容器来注入

**beans.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="address" class="pojo.Address">
		<property name="address" value="address你好" />
	</bean>

	<bean id="student" class="pojo.Student">
		<!--第一种，普通值注入 -->
		<property name="name" value="name你好" />
		<!--第二种，ref注入 -->
		<property name="address" ref="address" />

		<!--数组注入 -->
		<property name="books">
			<array>
				<value>三国</value>
				<value>西游</value>
				<value>水浒</value>
			</array>
		</property>

		<!--list列表注入 -->
		<property name="hobbies">
			<list>
				<value>唱</value>
				<value>跳</value>
				<value>rap</value>
				<value>篮球</value>
			</list>
		</property>

		<!--map键值对注入 -->
		<property name="card">
			<map>
				<entry key="username" value="root" />
				<entry key="password" value="root" />
			</map>
		</property>

		<!--set(可去重)注入 -->
		<property name="game">
			<set>
				<value>wangzhe</value>
				<value>lol</value>
				<value>galname</value>
			</set>
		</property>

		<!--空指针null注入 -->
		<property name="wife">
			<null></null>
		</property>

		<!--properties常量注入 -->
		<property name="infor">
			<props>
				<prop key="id">20200802</prop>
				<prop key="name">cbh</prop>
			</props>
		</property>
	</bean>
</beans>
```

### 6.3、拓展注入

官方文档位置

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614905685152-3eb4a87e-cf21-4a50-97ae-89d60e4ee0a7.png)

pojo增加User类

```java
package pojo;

public class User {
    private String name;
    private int id;
	public User() {
        
	}
	public User(String name, int id) {
		super();
		this.name = name;
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	@Override
	public String toString() {
		return "User [name=" + name + ", id=" + id + "]";
	}
}
```

注意： beans 里面加上这下面两行

使用p和c命名空间需导入xml约束

xmlns:p=“http://www.springframework.org/schema/p”

xmlns:c=“http://www.springframework.org/schema/c”

```xml
?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--p命名空间注入/set注入，可以直接注入属性的值-》property-->
    <bean id="user" class="pojo.User" p:name="cxk" p:id="20" >
    </bean>

    <!--c命名空间，通过构造器注入，需要写入有参和无参构造方法-》construct-args-->
    <bean id="user2" class="pojo.User" c:name="cbh" c:id="22"></bean>
</beans>
```

测试

```java
ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
User user = context.getBean("user",User.class);//确定class对象，就不用再强转了
System.out.println(user.toString());
```



### 6.4、Bean作用域



![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614905784203-7766277a-324e-436a-9373-c436ed73e825.png)



![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614905791799-b4ba27fa-f125-4272-a3ea-7186a355ba47.png)

1. 单例模式（默认）

```xml
<bean id="user2" class="pojo.User" c:name="cxk" c:age="19" scope="singleton"></bean>
```



![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614905845586-d40d9c4b-08b5-4729-9340-7a6aa08f956f.png)

2. 原型模式: 每次从容器中get的时候，都产生一个新对象！

```xml
<bean id="user2" class="pojo.User" c:name="cxk" c:age="19" scope="prototype"></bean>
```



![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614905863206-78d70a42-c472-467c-8b6d-4c2e410fc482.png)

3. 其余的request、session、application这些只能在web开放中使用！

## 7、Bean的自动装配

- 自动装配是Spring满足bean依赖的一种方式
- Spring会在上下文自动寻找，并自动给bean装配属性

1. byType自动装配：byType会自动查找，和自己对象set方法参数的类型相同的bean
   保证所有的class唯一(类为全局唯一)
2. byName自动装配：byName会自动查找，和自己对象set对应的值对应的id
   保证所有id唯一，并且和set注入的值一致

```xml
<!-- 找不到id和多个相同class -->
<bean id="cat1" class="pojo.Cat"/>
<bean id="cat2" class="pojo.Cat"/>
<!-- 找不到 id=cat，且有两个Cat -->
```

### 7.1测试：自动装配

xml配置 -> byType 自动装配

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="cat" class="pojo.Cat"/>
    <bean id="dog" class="pojo.Dog"/>
    
    <!--byType会在容器自动查找，和自己对象属性相同的bean
		例如，Dog dog; 那么就会查找pojo的Dog类，再进行自动装配
	-->
    <bean id="people" class="pojo.People" autowire="byType">
        <property name="name" value="cbh"></property>
    </bean>

</beans>
```

xml配置 -> byName 自动装配

```xml
<bean id="cat" class="pojo.Cat"/>
<bean id="dog" class="pojo.Dog"/>
<!--byname会在容器自动查找，和自己对象set方法的set后面的值对应的id
  例如:setDog()，取set后面的字符作为id，则要id = dog 才可以进行自动装配
  
 -->
<bean id="people" class="pojo.People" autowire="byName">
	<property name="name" value="cbh"></property>
</bean>
```



### 7.2、使用注解实现自动装配

jdk1.5支持的注解，spring2.5支持的注解

The introduction of annotation-based configuration raised the question of whether this approach is “better” than XML.（翻译：基于注释的配置的引入提出了一个问题，即这种方法是否比XML“更好”）

1. 导入context约束

**xmlns:context="**[**http://www.springframework.org/schema/context**](http://www.springframework.org/schema/context)**"**

2. 配置注解的支持：< context:annotation-config/>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
</beans>
```

#### 7.2.1、[@Autowired ]()

**默认是byType方式，如果匹配不上，就会byName**

在属性上个使用，也可以在set上使用

我们可以不用编写set方法了，前提是自动装配的属性在Spring容器里，且要符合ByName 自动装配

```java
public class People {
    @Autowired
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
}
```

[@Nullable ]() 字段标记了这个注解，说明该字段可以为空 

public name([@Nullable ]() String name){ 

}

```java
//源码
public @interface Autowired { 
	boolean required() default true; 
}
```

如果定义了Autowire的require属性为false，说明这个对象可以为null，否则不允许为空（false表示找不到装配，不抛出异常）

#### 7.2.2、@Autowired+[@Qualifier ]()

**@Autowired不能唯一装配时，需要@Autowired+**[**@Qualifier** ]() 

如果[@Autowired自动装配环境比较复杂。自动装配无法通过一个注解完成的时候，可以使用@Qualifier(value ]() = “dog”)去配合使用，指定一个唯一的id对象 

```java
public class People {
    @Autowired
    private Cat cat;
    @Autowired
    @Qualifier(value = "dog")
    private Dog dog;
    private String name;
}
```

#### 7.2.3、[@Resource ]()

**默认是byName方式，如果匹配不上，就会byType**

```java
public class People {
    Resource(name="cat")
    private Cat cat;
    Resource(name="dog")
    private Dog dog;
    private String name;
}
```

Autowired是byType，@Autowired+[@Qualifier ]() = byType || byName 

Autowired是先byType,如果唯一則注入，否则byName查找。resource是先byName,不符合再继续byType

#### 区别：

@Resource和@Autowired的区别：

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired通过byType的方式实现，而且必须要求这个对象存在！【常用】

- @Resource默认通过byname的方式实现，如果找不到名字，则通过byType实现！如果两个都找不到的情况下，就报错！【常用】
- 执行顺序不同：@Autowired通过byType的方式实现。@Resource默认通过byName的方式实现

## 8、使用注解开发

在spring4之后，使用注解开发，必须要保证aop包的导入

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614906803445-4ccb920d-7ed0-45ad-8d08-bd828b9691d7.png)

使用注解需要导入contex的约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

### 8.1、bean

弹幕评论：

有了< context:component-scan>，另一个< context:annotation-config/>标签可以移除掉，因为已经被包含进去了。

```xml
<!--指定要扫描的包，这个包下面的注解才会生效
	别只扫一个com.kuang.pojo包--> 
<context:component-scan base-package="com.kuang"/> 
<context:annotation-config/>
```

```java
//@Component 组件
//等价于<bean id="user" class="pojo.User"/> 
@Component
public class User {  
     public String name ="秦疆";
}
```

### 8.2、属性如何注入[@value ]()

```java
@Component
public class User { 
    //相当于<property name="name" value="kuangshen"/> 
    @value("kuangshen") 
    public String name; 
    
    //也可以放在set方法上面
    //@value("kuangshen")
    public void setName(String name) { 
        this.name = name; 
    }
}
```

### 8.3、衍生的注解

@Component有几个衍生注解，会按照web开发中，mvc架构中分层。

- dao （@Repository）
- service（@Service）

- controller（@Controller）

**这四个注解的功能是一样的，都是代表将某个类注册到容器中**

### 8.4、自动装配置

@Autowired：默认是byType方式，如果匹配不上，就会byName

@Nullable：字段标记了这个注解，说明该字段可以为空

@Resource：默认是byName方式，如果匹配不上，就会byType

### 8.5、作用域[@scope ]()

```java
//原型模式prototype，单例模式singleton
//scope("prototype")相当于<bean scope="prototype"></bean>
@Component 
@scope("prototype")
public class User { 
    
    //相当于<property name="name" value="kuangshen"/> 
    @value("kuangshen") 
    public String name; 
    
    //也可以放在set方法上面
    @value("kuangshen")
    public void setName(String name) { 
        this.name = name; 
    }
}
```

### 8.6、小结

**xml与注解：**

- xml更加万能，维护简单，适用于任何场合
- 注解，不是自己的类使用不了，维护复杂

**最佳实践：**

- xml用来管理bean
- 注解只用来完成属性的注入

- 要开启注解支持

## 9、使用Java的方式配置Spring

不使用Spring的xml配置，完全交给java来做！

Spring的一个子项目，在spring4之后，它成为了核心功能

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614906924264-1f442528-daa0-45c3-8785-d05e0f56093d.png)

**实体类：pojo的User.java**

```java
//这里这个注解的意思,就是说明这个类被Spring接管了,注册到了容器中 
@component 
public class User { 
    private String name;
    
    public String getName() { 
    	return name; 
    } 
    //属性注入值
    @value("QINJIANG')  
    public void setName(String name) { 
    	this.name = name; 
    } 
    @Override 
    public String toString() { 
        return "user{" + 
        "name='" + name + '\''+ 
        '}'; 
    } 
}
```

**配置文件：config中的kuang.java**

@Import(KuangConfig2.class)，用@import来包含KuangConfig2.java

```java
//这个也会Spring容器托管,注册到容器中,因为他本米就是一个@Component 
// @Configuration表这是一个配置类,就像我们之前看的beans.xml，类似于<beans>标签
@Configuration 
@componentScan("com.Kuang.pojo") //开启扫描
//@Import(KuangConfig2.class)
public class KuangConfig { 
    //注册一个bean , 就相当于我们之前写的一个bean 标签 
    //这个方法的名字,就相当于bean 标签中的 id 属性 ->getUser
    //这个方法的返同值,就相当于bean 标签中的class 属性 ->User
    
    //@Bean 
    public User getUser(){ 
    	return new User(); //就是返回要注入到bean的对象! 
    } 
}
```

弹幕评论：ComponentScan、@Component("pojo”) 这两个注解配合使用

**测试类**

```java
public class MyTest { 
    public static void main(String[ ] args) { 
    //如果完全使用了配置类方式去做,我们就只能通过 Annotationconfig 上下文来获取容器,通过配置类的class对象加载! 
    ApplicationContext context = new AnnotationConfigApplicationContext(KuangConfig.Class); //class对象
    User getUser =(User)context.getBean( "getUser"); //方法名getUser
    System.out.Println(getUser.getName()); 
    } 
}
```

**会创建两个相同对象问题的说明：**

**弹幕总结 - -> @Bean是相当于< bean>标签创建的对象，而我们之前学的@Component是通过spring自动创建的这个被注解声明的对象，所以这里相当于有两个User对象被创建了。一个是bean标签创建的（@Bean），一个是通过扫描然后使用@Component，spring自动创建的User对象，所以这里去掉@Bean这些东西，然后开启扫描。之后在User头上用@Component即可达到spring自动创建User对象了**

## 10、动态代理

代理模式是SpringAOP的底层

分类：动态代理和静态代理

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614907155776-235f1816-8bb9-426e-b7a3-a9aa25860f72.png)

### 10.1、静态代理

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614907166003-5df8cf48-79ad-46e7-bc72-21ab4fba7644.png)

代码步骤：

1、接口

```java
package pojo;
public interface Host {
	public void rent();
}
```

2、真实角色

```java
package pojo;
public class HostMaster implements Host{	
    
	public void rent() {
		System.out.println("房东要出租房子");
	}
}
```

3、代理角色

```java
package pojo;
public class Proxy {

	public Host host;
	
	public Proxy() {
		
	}
	
	public Proxy(Host host) {
		super();
		this.host = host;
	}
	
	public void rent() {
		seeHouse();
		host.rent();
		fee();
		sign();
	}
	//看房
	public void seeHouse() {
		System.out.println("看房子");
	}
	//收费
	public void fee() {
		System.out.println("收中介费");
	}
	//合同
	public void sign() {
		System.out.println("签合同");
	}		
}
```

4、客户端访问代理角色

```java
package holle4_proxy;

import pojo.Host;
import pojo.HostMaster;
import pojo.Proxy;

public class My {

	public static void main(String[] args) {
		//房东要出租房子
		Host host = new HostMaster();
		//中介帮房东出租房子，但也收取一定费用（增加一些房东不做的操作）
		Proxy proxy = new Proxy(host);
		//看不到房东，但通过代理，还是租到了房子
		proxy.rent();
		
	}
}
```

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614907251554-89227f7e-272b-452a-a1f4-9a16d11a9129.png)

代码翻倍：几十个真实角色就得写几十个代理

AOP横向开发

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614907268099-66eff18d-f043-410e-82b3-8c9c11be3144.png)

### 10.2、动态代理

动态代理和静态角色一样，动态代理底层是反射机制，动态代理中两大类：基于接口，基于类

- 基于接口：JDK的动态代理【使用ing】
- 基于类：cglib

了解两个类

1、Proxy：代理

2、InvocationHandler：调用处理程序

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614907302756-59111a35-45b2-440a-89c0-15fe993ae424.png)



实例：

接口 Host.java

```java
//接口
package pojo2;
public interface Host {
	public void rent();
	
}
```

接口Host实现类 HostMaster.java

```java
//接口实现类
package pojo2;
public class HostMaster implements Host{	
	public void rent() {
		System.out.println("房东要租房子");
	}
}
```

代理角色的处理程序类 ProxyInvocationHandler.java

```java
package pojo2;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

///用这个类，自动生成代理
public class ProxyInvocationHandler implements InvocationHandler {

	// Foo f =(Foo) Proxy.NewProxyInstance(Foo. Class.GetClassLoader(),
	// new Class<?>[] { Foo.Class },
	// handler);

	// 被代理的接口
	public HostMaster hostMaster ;
	
	public void setHostMaster(HostMaster hostMaster) {
		this.hostMaster = hostMaster;
	}

	// 得到生成的代理类 
	public Object getProxy() {
		// newProxyInstance() -> 生成代理对象，就不用再写具体的代理类了
		// this.getClass().getClassLoader() -> 找到加载类的位置
		// hostMaster.getClass().getInterfaces() -> 代理的具体接口
		// this -> 代表了接口InvocationHandler的实现类ProxyInvocationHandler
		return Proxy.newProxyInstance(this.getClass().getClassLoader(), hostMaster.getClass().getInterfaces(), this);


	// 处理代理实例并返回结果
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		seeHouse();
		// 动态代理的本质，就是使用反射机制实现的
        // invoke()执行它真正要执行的方法
		Object result = method.invoke(hostMaster, args);
		fee();
		return result;
	}

	public void seeHouse() {
		System.out.println("看房子");
	}

	public void fee() {
		System.out.println("收中介费");
	}

}
```

用户类 My2.java

```java
package holle4_proxy;

import pojo2.Host;
import pojo2.Host2;
import pojo2.HostMaster;
import pojo2.ProxyInvocationHandler;

public class My2 {

	public static void main(String[] args) {
        
		//真实角色
		HostMaster hostMaster = new HostMaster();
        
		//代理角色，现在没有；用代理角色的处理程序来实现Host接口的调用
		ProxyInvocationHandler pih = new ProxyInvocationHandler();
        
        //pih -> HostMaster接口类 -> Host接口
		pih.setHostMaster(hostMaster);
        
		//获取newProxyInstance动态生成代理类
		Host proxy = (Host) pih.getProxy();
		
		proxy.rent();

	}
}
```

**万能代理类**

```java
///用这个类，自动生代理
public class ProxyInvocationHandler implements InvocationHandler {

	// 被代理的接口
	public Object target;

	public void setTarget(Object target) {
		this.target = target;
	}

	// 得到生成的代理类 -> 固定的代码
	public Object getProxy() {
		return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(), this);
	}

	// 处理代理实例并返回结果
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		// 动态代理的本质，就是使用反射机制实现的
		// invoke()执行它真正要执行的方法
		Object result = method.invoke(target, args);
		return result;
	}

}
```



![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614907527749-27e54feb-fffb-4b93-802e-0b1921dbeb9d.png)



## 11、AOP

### 11.1、什么是AOP

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614907534903-404c4d72-140c-460c-8415-d412a4c11bee.png)

### 11.2、AOP在Spring中的使用

提供声明式事务，允许用户自定义切面

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等…
- 切面(Aspect)：横切关注点 被模块化的特殊对象。即，它是一个类。（Log类）

- 通知(Advice)：切面必须要完成的工作。即，它是类中的一个方法。（Log类中的方法）
- 目标(Target)：被通知对象。（生成的代理类)

- 代理(Proxy)：向目标对象应用通知之后创建的对象。（生成的代理类）
- 切入点(PointCut)：切面通知执行的”地点”的定义。（最后两点：在哪个地方执行，比如：method.invoke()）

- 连接点(JointPoint)：与切入点匹配的执行点。

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614907633137-2db46353-70d1-43c9-a6f5-9b1a47d3b380.png)

SpringAOP中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice:

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614907641211-09a0de38-af90-4fc9-94f3-bac3ac744a76.png)

**即AOP在不改变原有代码的情况下，去增加新的功能。**（代理）

### 11.3、使用Spring实现AOP

导入jar包

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

#### 11.3.1、方法一：使用原生spring接口

springAPI接口实现

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--注册bean-->
    <bean id="userservice" class="service.UserServiceImpl"/>
    <bean id="log" class="log.Log"/>
    <bean id="afterLog" class="log.AfterLog"/>
	<!--方式一，使用原生Spring API接口-->
    <!--配置aop,还需要导入aop约束-->
    <aop:config>
        <!--切入点：expression:表达式，execution（要执行的位置）-->
        <aop:pointcut id="pointcut" expression="execution(* service.UserServiceImpl.*(..))"/>
        <!--UserServiceImpl.*(..) -》 UserServiceImpl类下的所以方法(参数)-->
        <!--执行环绕增加-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
        <!-- 环绕,在id="pointcut"的前后切入 -->
    </aop:config>

</beans>
```

execution(返回类型，类名，方法名(参数)) -> execution( *com.service.*,*(…))

UserService.java

```java
package service;
public interface UserService {   
	    public void add() ;
	    public void delete() ;
	    public void query() ;
	    public void update();
}
```

UserService 的实现类 UserServiceImp.java

```java
package service;

public class UserServiceImpl implements UserService {

    public void add() {
        System.out.println("add增");
    }
    public void delete() {
        System.out.println("delete删");
    }
    public void update() {
        System.out.println("update改");
    }
    public void query() {
        System.out.println("query查");
    }
}
```

前置Log.java

```java
package log;
import org.springframework.aop.MethodBeforeAdvice;
import java.lang.reflect.Method;

public class Log implements MethodBeforeAdvice {
    //method：要执行的目标对象的方法
    //args：参数
    //target：目标对象
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName()+"的"+method.getName()+"被执行了");
    }
}
```

后置AfterLog.java

```java
package log;
import java.lang.reflect.Method;
import org.springframework.aop.AfterReturningAdvice;

public class AfterLog implements AfterReturningAdvice {
    //returnVaule: 返回值
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
    	System.out.println("执行了"+method.getName()+"方法，返回值是"+returnValue);
    }
}
```

测试类MyTest5

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import service.UserService;

public class MyTest5 {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //注意:动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userservice");
        userService.add();
    }
}
```

#### 11.3.2、方法二：自定义类实现AOP

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:aop="http://www.springframework.org/schema/aop"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
   	https://www.springframework.org/schema/beans/spring-beans.xsd
   	http://www.springframework.org/schema/aop
   	https://www.springframework.org/schema/aop/spring-aop.xsd">

   <!--注册bean-->
   <bean id="userservice" class="service.UserServiceImpl"/>
   <bean id="log" class="log.Log"/>
   <bean id="afterLog" class="log.AfterLog"/>
   <!-- 方式二，自定义 -->
   <bean id="diy" class="diy.DiyPointcut"/>
   <aop:config>
       <!--自定义切面-->
       <aop:aspect ref="diy">
           <!--切入点-->
           <aop:pointcut id="point" expression="execution(* service.UserServiceImpl.*(..))"/>
           <aop:before method="before" pointcut-ref="point"/>
           <aop:after method="after" pointcut-ref="point"/>
       </aop:aspect>
   </aop:config>

</beans>
```

```java
package diy;
public class DiyPointcut {

    public void before(){
        System.out.println("插入到前面");
    }

    public void after(){
        System.out.println("插入到后面");
    }
}
```

```java
//测试
public class MyTest5 {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //注意:动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userservice");
        userService.add();
    }
}
```

#### 11.3.3、方法三：使用注解实现

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">
	
    <!-- 注册 -->
    <bean id="userservice" class="service.UserServiceImpl"/>
    <!--方式三，使用注解实现-->
    <bean id="diyAnnotation" class="diy.DiyAnnotation"></bean>
    
    <!-- 开启自动代理 
		实现方式：默认JDK (proxy-targer-class="fasle")
    			 cgbin (proxy-targer-class="true")-->
	<aop:aspectj-autoproxy/>
    
</beans>
```

DiyAnnotation.java

```java
package diy;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect  //标注这个类是一个切面
public class DiyAnnotation {
	
    @Before("execution(* service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("=====方法执行前=====");
    }

    @After("execution(* service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("=====方法执行后=====");
    }

    //在环绕增强中，我们可以给地暖管一个参数，代表我们要获取切入的点
    @Around("execution(* service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("环绕前");

        Object proceed = joinPoint.proceed();

        System.out.println("环绕后");
    }
}
```

测试

```java
public class MyTest5 {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //注意:动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userservice");
        userService.add();
    }
}
```

输出结果：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614907987976-3ccb0bff-7c89-42b1-937e-bd2968e20354.png)



## 12、整合mybatis

mybatis-spring官网：https://mybatis.org/spring/zh/

**mybatis的配置流程：**

- 编写实体类
- 编写核心配置文件
- 编写接口
- 编写Mapper.xml

### 12.1、mybatis-spring-方式一

- 编写数据源配置
- sqISessionFactory
- sqISessionTemplate（相当于sqISession）
- 需要给接口加实现类【new】
- 将自己写的实现类，注入到Spring中

先导入jar包

```xml
<dependencies>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.4</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.2</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.4</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.12</version>
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.12</version>
    </dependency>

</dependencies>
	
<!--在build中配置resources，来防止资源导出失败的问题-->
<!-- Maven解决静态资源过滤问题 -->
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



![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614908046914-e0b177c7-72fa-44b6-99ed-cb02731de661.png)

**编写顺序：**

**User -> UserMapper -> UserMapper.xml -> spring-dao.xml -> UserServiceImpl -> applicationContext.xml -> MyTest6**

**代码步骤：**

pojo实体类 User

```java
package pojo;
import lombok.Data;
@Data
public class User {
	private int id;
	private String name;
	private String pwd;
}
```

mapper目录下的 UserMapper、UserMapperImpl、UserMapper.xml

接口UserMapper

```java
package mapper;
import java.util.List;
import pojo.User;
public interface UserMapper {
	public List<User> getUser();
}
```

UserMapperImpl

```java
package mapper;
import java.util.List;
import org.mybatis.spring.SqlSessionTemplate;
import pojo.User;

public class UserMapperImpl implements UserMapper{
	
	//我们的所有操作，在原来都使用sqlSession来执行，现在都使用SqlSessionTemplate；
	private SqlSessionTemplate sqlSessionTemplate;

	public void setSqlSessionTemplate(SqlSessionTemplate sqlSessionTemplate) {
		this.sqlSessionTemplate = sqlSessionTemplate;
	}

	public List<User> getUser() {
		UserMapper mapper = sqlSessionTemplate.getMapper(UserMapper.class);
		return mapper.getUser();
	}
}
```

UserMapper.xml 

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        
<!-- 绑定接口 -->
<mapper namespace="mapper.UserMapper">
	<select id="getUser" resultType="pojo.User">
		select * from mybatis.mybatis
	</select>
</mapper>
```

resource目录下的 mybatis-config.xml、spring-dao.xml、applicationContext.xml

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!--开启日志-->
	<settings>
		<setting name="logImpl" value="STDOUT_LOGGING" />
	</settings>
	
	<!--可以给实体类起别名 -->
	<typeAliases> 
		<package name="pojo" />
	</typeAliases>

</configuration>
```

spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">
		
	<!--DataSource:使用Spring的数帮源替换Mybatis的配置 其他数据源：c3p0、dbcp、druid 
		这使用Spring提供的JDBC: org.springframework.jdbc.datasource -->
	<!--data source -->
	<bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName"
			value="com.mysql.cj.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;serverTimezone=Asia/Shanghai"/>
		<property name="username" value="root" />
		<property name="password" value="root" />
	</bean>
	
	<!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource" />
        <!--绑定 mybatis 配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:mapper/*.xml"/>
    </bean>

	<!-- sqlSessionTemplate 就是之前使用的：sqlsession -->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    	<!-- 只能使用构造器注入sqlSessionFactory 原因：它没有set方法-->	
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>
		
</beans>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">
    
	<!-- 导入spring-dao.xml -->
	<import resource="spring-dao.xml"/>
	
    <bean id="userMapper" class="mapper.UserMapperImpl">
        <property name="sqlSessionTemplate" ref="sqlSession"></property>
    </bean>

</beans>
```

测试类

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import mapper.UserMapper;
import pojo.User;
public class MyTest6 {
	public static void main(String[] args) {
        
		ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		
		UserMapper userMapper = (UserMapper) context.getBean("userMapper");
		
		for (User user : userMapper.getUser()) {
			System.out.println(user);
		}
	}
}
```

### 12.2、mybatis-spring-方式二

UserServiceImpl2

```java
package mapper;
import pojo.User;
import org.apache.ibatis.session.SqlSession;
import org.mybatis.spring.support.SqlSessionDaoSupport;
import java.util.List;
//继承SqlSessionDaoSupport 类
public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper {
    public List<User> getUser() {
        SqlSession sqlSession = getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.getUser();
        //或者一句话：return getSqlSession().getMapper(UserMapper.class).getUser();
    }
}
```

spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">
		
	<!--DataSource:使用Spring的数帮源替换Mybatis的配置 c3p0 dbcp druid 
		这使用Spring提供的JDBC: org.springframework.jdbc.datasource -->
	<!--data source -->
	<bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName"
			value="com.mysql.cj.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;serverTimezone=Asia/Shanghai"/>
		<property name="username" value="root" />
		<property name="password" value="root" />
	</bean>
	
	<!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource" />
        <!--绑定 mybatis 配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:mapper/*.xml"/>
    </bean>
    
	<!-- 方法二：SqlSessionTemplate 可以不写了-->
    
</beans>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">

	<import resource="spring-dao.xml" />

	<!-- 方法二 -->
	<bean id="userMapper2" class="mapper.UserMapperImpl2">
		<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
	</bean>
</beans>
```

测试

```java
public class MyTest6 {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		UserMapper userMapper = (UserMapper) context.getBean("userMapper2");
		for (User user : userMapper.getUser()) {
			System.out.println(user);
		}
	}
}
```



## 13. 声明式事务

- 把一组业务当成一个业务来做；要么都成功，要么都失败！
- 事务在项目开发中，十分的重要，涉及到数据的一致性问题

- 确保完整性和一致性

事务的ACID原则：

1、原子性

2、隔离性

3、一致性

4、持久性

Spring中的事务管理

- 声明式事务：AOP
- 编程式事务：需要再代码中，进行事务管理

**声明式事务**

先导入jar包

```xml
<dependencies>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.4</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.2</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.4</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.12</version>
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.12</version>
    </dependency>

</dependencies>
	
<!--在build中配置resources，来防止资源导出失败的问题-->
<!-- Maven解决静态资源过滤问题 -->
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

**代码步骤：**

pojo实体类 User

```java
package pojo;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
	private int id;
	private String name;
	private String pwd;
}
```

mapper目录下的 UserMapper、UserMapperImpl、UserMapper.xml

接口UserMapper

```java
package mapper;
import java.util.List;
import org.apache.ibatis.annotations.Param;
import pojo.User;

public interface UserMapper {
	public List<User> getUser();
	
	public int insertUser(User user); 
	
	public int delUser(@Param("id") int id); 
}
```

UserMapperImpl

```java
package mapper;

import pojo.User;
import org.apache.ibatis.session.SqlSession;
import org.mybatis.spring.support.SqlSessionDaoSupport;
import java.util.List;

public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper {
    public List<User> getUser() {
    	User user = new User(5,"你好","ok");
    	insertUser(user);
    	delUser(5);
        SqlSession sqlSession = getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.getUser();
        //或者return  getSqlSession().getMapper(UserMapper.class).getUser();
    }
    //插入
	public int insertUser(User user) {
		return getSqlSession().getMapper(UserMapper.class).insertUser(user);
	}
	//删除
	public int delUser(int id) {
		return getSqlSession().getMapper(UserMapper.class).delUser(id);
	}
}
```

UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        
<!-- 绑定接口 -->
<mapper namespace="mapper.UserMapper">
	<select id="getUser" resultType="pojo.User">
		select * from mybatis.mybatis
	</select>
	
	<insert id="insertUser"  parameterType="pojo.User" >
		insert into  mybatis.mybatis (id,name,pwd) values (#{id},#{name},#{pwd})
	</insert>
	
	<delete id="delUser" parameterType="_int">
		deleteAAAAA from mybatis.mybatis where id = #{id}
		<!-- deleteAAAAA是故意写错的 -->
	</delete>

</mapper>
```

resource目录下的 mybatis-config.xml、spring-dao.xml、applicationContext.xml

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- configuration -->
<configuration>
	
	<!--开启日志-->
	<settings>
		<setting name="logImpl" value="STDOUT_LOGGING" />
	</settings>
	
	<!--可以给实体类起别名-->
	<typeAliases> 
		<package name="pojo" />
	</typeAliases>

</configuration>
```

spring-dao.xml（已导入约束）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        https://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
		
	<!--data source -->
	<bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName"
			value="com.mysql.cj.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;serverTimezone=Asia/Shanghai"/>
		<property name="username" value="root" />
		<property name="password" value="root" />
	</bean>
	
	<!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource" />
        <!--绑定 mybatis 配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:mapper/*.xml"/>
    </bean>
	
	<!--声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="datasource" />
    </bean>

    <!--结合aop实现事务织入-->
    <!--配置事务的通知类-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--给哪些方法配置事务-->
        <!--新东西：配置事务的传播特性 propagation-->
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <!-- *号包含上面4个方法：
            <tx:method name="*" propagation="REQUIRED"/> -->
        </tx:attributes>
    </tx:advice>

    <!--配置事务切入-->
    <aop:config>
        <aop:pointcut id="txpointcut" expression="execution(* mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txpointcut"/>
    </aop:config>

</beans>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">

	<import resource="spring-dao.xml" />

	<bean id="userMapper" class="mapper.UserMapperImpl">
		<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
	</bean>
</beans>
```

测试类

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import mapper.UserMapper;import pojo.User;
public class MyTest7 {
	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		
		UserMapper userMapper = (UserMapper) context.getBean("userMapper");
		
		for (User user : userMapper.getUser()) {
			System.out.println(user);
		}
	}
}
```

**思考：**

为什么需要事务？

- 如果不配置事务，可能存在数据提交不一致的情况下；
- 如果不在spring中去配置声明式事务，我们就需要在代码中手动配置事务！

- 事务在项目的开发中非常重要，涉及到数据的一致性和完整性问题！

# 高级



Spring 是个java企业级应用的开源开发框架。Spring主要用来开发Java应用，但是有些扩展是针对构建J2EE平台的web应用。Spring 框架目标是简化Java企业级应用开发，并通过POJO为基础的编程模型促进良好的编程习惯。

## 1、使用Spring框架的好处是什么？

轻量：Spring 是轻量的，基本的版本大约2MB

控制反转（IOC）：Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们

面向切面的编程(AOP)：Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开

容器：Spring 包含并管理应用中对象的生命周期和配置

MVC框架：Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品

事务管理：Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）

异常处理：Spring 提供方便的API把具体技术相关的异常转化为一致的unchecked 异常

## 2、Spring由哪些模块组成？

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

## 3、Spring 配置文件

Spring配置文件是个XML 文件，这个文件包含了类信息，描述了如何配置它们，以及如何相互调用。



## 4、Spring IOC 容器

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

## 5、Spring Beans

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



## 6、Spring 注解



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

## 7、Spring 数据访问





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



## 8、Spring 面向切面编程AOP

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

## 9、Spring 体现的设计模式

- **工厂设计模式** : Spring使用工厂模式通过 `BeanFactory`、`ApplicationContext` 创建 bean 对象。

- **代理设计模式** : Spring AOP 功能的实现。

- **单例设计模式** : Spring 中的 Bean 默认都是单例的。

- **模板方法模式** : Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。

- **包装器设计模式** : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。

- **观察者模式:** Spring 事件驱动模型就是观察者模式很经典的一个应用。

- **适配器模式** :Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller`。

## 10、Spring MVC



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