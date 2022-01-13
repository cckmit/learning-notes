# 基础入门

## 1、简介

### 1.1 什么是Mybatis

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865195985-b3bc0f7b-d24b-434e-aa32-7e739b3e195b.png)

- **MyBatis 是一款优秀的持久层框架;**
- 它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO 为数据库中的记录。

**JDBC层面存在的问题**

jdbc编程步骤

1. 加载数据库驱动
2. 创建并获取数据库链接
3. 创建jdbc statement对象
4. 设置sql语句
5. 设置sql语句中的参数(使用preparedStatement)
6. 通过statement执行sql并获取结果
7. 对sql执行结果进行解析处理
8. 释放资源(resultSet、preparedstatement、connection)

**问题**

1. 数据库连接用完关闭
2. sql硬编程到代码中，不利于维护
3. 向preparedStatement中设置参数，对占位符号位置和设置参数值，硬编码在java代码中，不利于系统维护
4. 从resultSet中遍历结果集数据时，存在硬编码，将获取表的字段进行硬编码，不利于系统维护

### 1.2 持久化

数据持久化

- 持久化就是将程序的数据在持久状态和瞬时状态转化的过程
- 内存：**断电即失**

- 数据库（Jdbc):io文件持久化。

**为什么要持久化？**

- 有一些对象，不能让他丢掉
- 内存太贵

### 1.3 持久层

Dao层、Service层、Controller层

- 完成持久化工作的代码块
- 层界限十分明显

### 1.4 为什么需要MyBatis

- 帮助程序员将数据存入到数据库中 、方便

- 传统的JDBC代码太复杂了，简化，框架，自动化 

- 不用MyBatis也可以，技术没有高低之分 

- 优点：    

  - 简单易学、灵活

  - sql和代码的分离，提高了可维护性。

  - 提供映射标签，支持对象与数据库的orm字段关系映射

  - 提供对象关系映射标签，支持对象关系组建维护

  - 提供xml标签，支持编写动态sql

## 2、第一个Mybatis程序

思路：搭建环境 --> 导入MyBatis --> 编写代码 --> 测试

### 2.1 搭建环境

新建项目

1.  创建一个普通的maven项目 
2.  删除src目录 （就可以把此工程当做父工程了，然后创建子工程） 

1.  导入maven依赖 

```xml
<!--导入依赖-->
<dependencies>
    <!--mysqlq驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.12</version>
    </dependency>
    <!--mybatis-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.4</version>
    </dependency>
    <!--junit-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

1.  创建一个Module 

### 2.2 创建一个模块

-  编写mybatis的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--configuration核心配置文件-->
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?userSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=UTC"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
</configuration>
```

-  编写mybatis工具类 

```java
//sqlSessionFactory --> sqlSession
public class MybatisUtils {

    static SqlSessionFactory sqlSessionFactory = null;

    static {
        try {
            //使用Mybatis第一步 ：获取sqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例.
    // SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}
```

### 2.3 编写代码

-  实体类 
-  Dao接口

```java
public interface UserDao {
    public List<User> getUserList();
}
```

-  接口实现类（由原来的UserDaoImpl转变为一个Mapper配置文件）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace=绑定一个指定的Dao/Mapper接口-->
<mapper namespace="com.kuang.dao.UserDao">
    <select id="getUserList" resultType="com.kuang.pojo.User">
    select * from USER
  </select>
</mapper>
```

-  junit测试 

```java
    @Test
    public void test(){

        //1.获取SqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //2.执行SQL
        // 方式一：getMapper
        UserDao userDao = sqlSession.getMapper(UserDao.class);
        List<User> userList = userDao.getUserList();
        for (User user : userList) {
            System.out.println(user);
        }

        //关闭sqlSession
        sqlSession.close();
    }
```

## 3、CURD

### 1. namespace

namespace中的包名要和Dao/Mapper接口的包名一致

### 2. select

选择，查询语句；

-  id：就是对应的namespace中的方法名； 
-  resultType : Sql语句执行的返回值； 

-  parameterType : 参数类型；    

**编写接口**

```java
public interface UserMapper {
    //查询所有用户
    public List<User> getUserList();
    //插入用户
    public void addUser(User user);
}
```

**编写对应的mapper中的sql语句**

```java
<insert id="addUser" parameterType="com.kuang.pojo.User">
    insert into user (id,name,password) values (#{id}, #{name}, #{password})
</insert>
```

测试

```java
@Test
public void test2() {
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    User user  = new User(3,"黑子","666");
    mapper.addUser(user);
    //增删改一定要提交事务
    sqlSession.commit();

    //关闭sqlSession
    sqlSession.close();
}
```

**注意：增删改查一定要提交事务**

```java
sqlSession.commit();
```

### 3. Insert

### 4. update

### 5. Delete

### 6. 万能Map

假设，我们的实体类，或者数据库中的表，字段或者参数过多，考虑使用Map

UserMapper接口

```java
//用万能Map插入用户
public void addUser2(Map<String,Object> map);
```

UserMapper.xml

```java
<!--对象中的属性可以直接取出来 传递map的key-->
<insert id="addUser2" parameterType="map">
    insert into user (id,name,password) values (#{userid},#{username},#{userpassword})
</insert>
```

测试

```java
    @Test
    public void test3(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        HashMap<String, Object> map = new HashMap<String, Object>();
        map.put("userid",4);
        map.put("username","王虎");
        map.put("userpassword",789);
        mapper.addUser2(map);
        //提交事务
        sqlSession.commit();
        //关闭资源
        sqlSession.close();
    }
```

Map传递参数，直接在sql中取出key即可！ 【parameter=“map”】

对象传递参数，直接在sql中取出对象的属性即可！ 【parameter=“Object”】

只有一个基本类型参数的情况下，可以直接在sql中取到

多个参数用Map , **或者注解！**

### 7. 模糊查询

模糊查询这么写

Java代码执行的时候，传递通配符% % 

```java
List<User> userList = mapper.getUserLike("%李%");
```

在sql拼接中使用通配符 

```sql
select * from user where name like "%"#{value}"%"
```

## 4、配置解析

### 1. 核心配置文件

-  mybatis-config.xml 
-  Mybatis的配置文件包含了会深深影响MyBatis行为的设置和属性信息。 

```xml
configuration（配置）
    properties（属性）
    settings（设置）
    typeAliases（类型别名）
    typeHandlers（类型处理器）
    objectFactory（对象工厂）
    plugins（插件）
    environments（环境配置）
    	environment（环境变量）
    		transactionManager（事务管理器）
    		dataSource（数据源）
    databaseIdProvider（数据库厂商标识）
    mappers（映射器）
```

### 2. 环境配置 environments

MyBatis 可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境**

学会使用配置多套运行环境！

MyBatis默认的事务管理器就是JDBC ，连接池：POOLED

### 3. 属性 properties

我们可以通过properties属性来实现引用配置文件

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。【db.poperties】

编写一个配置文件 db.properties 

```xml
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?userSSL=true&useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
username=root
password=root
```

在核心配置文件中引入

```xml
<!--引用外部配置文件-->
<properties resource="db.properties">
    <property name="username" value="root"/>
    <property name="password" value="root"/>
</properties>
```

- 可以直接引入外部文件
- 可以在其中增加一些属性配置
- 如果两个文件有同一个字段，优先使用外部配置文件的

### 4. 类型别名 typeAliases

-  类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置. 
-  意在降低冗余的全限定类名书写。

```xml
<!--可以给实体类起别名-->
<typeAliases>
    <typeAlias type="com.kuang.pojo.User" alias="User"/>
</typeAliases>
```

- 也可以指定一个包，每一个在包 `domain.blog` 中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。 若有注解，则别名为其注解值。见下面的例子：

```xml
<typeAliases>
    <package name="com.kuang.pojo"/>
</typeAliases>
```

在实体类比较少的时候，使用第一种方式。

如果实体类十分多，建议用第二种扫描包的方式。

第一种可以DIY别名，第二种不行，如果非要改，需要在实体上增加注解。

```java
@Alias("author")
public class Author {
    ...
}
```

### 5. 设置 Settings

这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865195895-0c39e91b-1752-4507-bdc8-782818ed32f2.png)

### 6. 其他配置

- [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)

- [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)

- plugins 插件    

  - mybatis-generator-core

  - mybatis-plus

- 通用mapper

### 7. 映射器 mappers

MapperRegistry：注册绑定我们的Mapper文件；

方式一：【推荐使用】

```xml
<!--每一个Mapper.xml都需要在MyBatis核心配置文件中注册-->
<mappers>
    <mapper resource="com/kuang/dao/UserMapper.xml"/>
</mappers>
```

方式二：使用class文件绑定注册

```xml
<!--每一个Mapper.xml都需要在MyBatis核心配置文件中注册-->
<mappers>
    <mapper class="com.kuang.dao.UserMapper"/>
</mappers>
```

**注意点：**

- 接口和他的Mapper配置文件必须同名
- 接口和他的Mapper配置文件必须在同一个包下

方式三：使用包扫描进行注入

```xml
<mappers>
    <package name="com.kuang.dao"/>
</mappers>
```

### 8. 作用域和生命周期

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865195957-00354f45-ee88-4d30-a863-0aa08f0fc652.png)

声明周期和作用域是至关重要的，因为错误的使用会导致非常严重的**并发问题**。

**SqlSessionFactoryBuilder:**

- 一旦创建了SqlSessionFactory，就不再需要它了
- 局部变量

**SqlSessionFactory:**

- 说白了就可以想象为：数据库连接池
- SqlSessionFactory一旦被创建就应该在应用的运行期间一直存在，**没有任何理由丢弃它或重新创建一个实例。**

- 因此SqlSessionFactory的最佳作用域是应用作用域（ApplocationContext）。
- 最简单的就是使用**单例模式**或静态单例模式。

**SqlSession：**

- 连接到连接池的一个请求
- SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。

- 用完之后需要赶紧关闭，否则资源被占用！

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865195983-4d720167-de02-4a8e-bb63-59aef92c66df.png)

## 5、解决属性名和字段名不一致的问题

### 1. 问题

数据库中的字段

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865195934-b88ab46e-dcf0-4afe-9d6b-0c92705ef0ec.png)

新建一个项目，拷贝之前的，测试实体类字段不一致的情况

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865195919-7998c245-e2fe-4eb8-bffc-8fee4035b866.png)

测试出现问题

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865195917-4c384dce-0d8b-45e9-9bf0-cfc81e4d458b.png)

```plain
// select * from user where id = #{id}
// 类型处理器
// select id,name,pwd from user where id = #{id}
```

解决方法：

- 起别名

```xml
<select id="getUserById" resultType="com.kuang.pojo.User">
    select id,name,pwd as password from USER where id = #{id}
</select>
```

### 2. resultMap

结果集映射

```xml
<!--结果集映射-->
<resultMap id="UserMap" type="User">
    <!--column数据库中的字段，property实体类中的属性-->
    <result column="id" property="id"></result>
    <result column="name" property="name"></result>
    <result column="pwd" property="password"></result>
</resultMap>

<select id="getUserList" resultMap="UserMap">
    select * from USER
</select>
```

- `resultMap` 元素是 MyBatis 中最重要最强大的元素。 
- ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。 
- `ResultMap` 的优秀之处——你完全可以不用显式地配置它们。 

## 6、日志

### 6.1 日志工厂

如果一个数据库操作，出现了异常，我们需要排错，日志就是最好的助手！

曾经：sout、debug

现在：日志工厂

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865196034-5b6a68cc-a5ab-41b9-8181-077f9fcdc234.png)

- SLF4J
- LOG4J 【掌握】

- LOG4J2
- JDK_LOGGING

- COMMONS_LOGGING
- STDOUT_LOGGING 【掌握】

- NO_LOGGING

在MyBatis中具体使用哪一个日志实现，在设置中设定

**STDOUT_LOGGING**

```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865195995-5bcf0cd6-a71d-46ba-a3c4-6328ab9a60e3.png)

### 6.2 Log4j

什么是Log4j？

-  Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件； 
-  我们也可以控制每一条日志的输出格式； 

-  通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程； 
-  最令人感兴趣的就是，这些可以通过一个[配置文件](https://baike.baidu.com/item/配置文件/286550)来灵活地进行配置，而不需要修改应用的代码。

先导入log4j的包 

```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

log4j.properties 

```xml
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file
#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/rzp.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n
#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sq1.PreparedStatement=DEBUG
```

1.  配置settings为log4j实现 
2.  测试运行 

**Log4j简单使用**

1.  在要使用Log4j的类中，导入包 import org.apache.log4j.Logger; 
2.  日志对象，参数为当前类的class对象

```java
Logger logger = Logger.getLogger(UserDaoTest.class);
```

- 日志级别

```java
logger.info("info: 测试log4j");
logger.debug("debug: 测试log4j");
logger.error("error:测试log4j");
```

- info

- debug

- error

## 7、分页

**思考：为什么分页？**

- 减少数据的处理量

### 7.1 **使用Limit分页**

```sql
SELECT * from user limit startIndex,pageSize 
```

**使用MyBatis实现分页，核心SQL**

接口

```java
//分页
List<User> getUserByLimit(Map<String,Integer> map);
```

Mapper.xml 

```xml
<!--分页查询-->
<select id="getUserByLimit" parameterType="map" resultMap="UserMap">
    select * from user limit #{startIndex},#{pageSize}
</select>
```

测试

```java
    @Test
    public void getUserByLimit(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        HashMap<String, Integer> map = new HashMap<String, Integer>();
        map.put("startIndex",1);
        map.put("pageSize",2);
        List<User> list = mapper.getUserByLimit(map);
        for (User user : list) {
            System.out.println(user);
        }
    }
```

### 7.2 RowBounds分页

不再使用SQL实现分页

接口

```java
//分页2
List<User> getUserByRowBounds();
```

mapper.xml 

```xml
<!--分页查询2-->
<select id="getUserByRowBounds">
    select * from user limit #{startIndex},#{pageSize}
</select>
```

测试

```java
    public void getUserByRowBounds(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //RowBounds实现
        RowBounds rowBounds = new RowBounds(1, 2);
        //通过Java代码层面实现分页
        List<User> userList = sqlSession.selectList("com.kaung.dao.UserMapper.getUserByRowBounds", null, rowBounds);
        for (User user : userList) {
            System.out.println(user);
        }
        sqlSession.close();
    }
```

### 7.3 分页插件

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865195932-3afdbc76-6f8c-44b0-8415-d2e150c9cc54.png)

## 8、使用注解开发

### 8.1 面向接口开发

**三个面向区别**

- 面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性和方法；
- 面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现；

- 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题，更多的体现就是对系统整体的架构；

### 8.2 使用注解开发

注解在接口上实现 

```java
@Select("select * from user")
List<User> getUsers();
```

需要在核心配置文件中绑定接口

```xml
<mappers>
    <mapper class="com.kuang.dao.UserMapper"/>
</mappers>
```

本质：反射机制实现

底层：动态代理

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865195970-d0282b15-abac-447a-b7ec-4ea8ec08d014.png)

**MyBatis详细执行流程**

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865196113-980254e3-eaaf-4976-9948-e393cf09ffe3.png)

### 8.3 注解CURD

```java
//方法存在多个参数，所有的参数前面必须加上@Param("id")注解
@Delete("delete from user where id = ${uid}")
int deleteUser(@Param("uid") int id);
```

**关于@Param( )注解**

- 基本类型的参数或者String类型，需要加上
- 引用类型不需要加

- 如果只有一个基本类型的话，可以忽略，但是建议大家都加上
- 我们在SQL中引用的就是我们这里的@Param()中设定的属性名

## 9、Lombok

Lombok项目是一个Java库，它会自动插入编辑器和构建工具中，Lombok提供了一组有用的注释，用来消除Java类中的大量样板代码。仅五个字符(@Data)就可以替换数百行代码从而产生干净，简洁且易于维护的Java类。

使用步骤：

1.  在IDEA中安装Lombok插件 
2.  在项目中导入lombok的jar包 

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.10</version>
    <scope>provided</scope>
</dependency>
```

1.  在程序上加注解 

```plain
@Getter and @Setter
@FieldNameConstants
@ToString
@EqualsAndHashCode
@AllArgsConstructor, @RequiredArgsConstructor and @NoArgsConstructor
@Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog
@Data
@Builder
@SuperBuilder
@Singular
@Delegate
@Value
@Accessors
@Wither
@With
@SneakyThrows
@val
```

说明：

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String password;
}
```



![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865195990-10a17e0f-a542-49c4-9946-1488c48c6090.png)

## 10、多对一处理

多个学生一个老师；

```sql
alter table student ADD CONSTRAINT fk_tid foreign key (tid) references teacher(id)
```

### **1. 测试环境搭建**

1. 导入lombok
2. 新建实体类Teacher,Student

1. 建立Mapper接口
2. 建立Mapper.xml文件

1. 在核心配置文件中绑定注册我们的Mapper接口或者文件 【方式很多，随心选】
2. 测试查询是否能够成功

### 2. 按照查询嵌套处理

```xml
<!--
     思路：
        1. 查询所有的学生信息
        2. 根据查询出来的学生的tid寻找特定的老师 (子查询)
    -->
<select id="getStudent" resultMap="StudentTeacher">
    select * from student
</select>
<resultMap id="StudentTeacher" type="student">
    <result property="id" column="id"/>
    <result property="name" column="name"/>
    <!--复杂的属性，我们需要单独出来 对象：association 集合：collection-->
    <collection property="teacher" column="tid" javaType="teacher" select="getTeacher"/>
</resultMap>
<select id="getTeacher" resultType="teacher">
    select * from teacher where id = #{id}
</select>
```

### 3.按照结果嵌套处理

```xml
    <!--按照结果进行查询-->
    <select id="getStudent2" resultMap="StudentTeacher2">
        select s.id sid , s.name sname, t.name tname
        from student s,teacher t
        where s.tid=t.id
    </select>
    <!--结果封装，将查询出来的列封装到对象属性中-->
    <resultMap id="StudentTeacher2" type="student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <association property="teacher" javaType="teacher">
            <result property="name" column="tname"></result>
        </association>
    </resultMap>
```

回顾Mysql多对一查询方式:

- 子查询 （按照查询嵌套）
- 联表查询 （按照结果嵌套）

## 11、一对多处理

一个老师多个学生；

对于老师而言，就是一对多的关系；

### 1. 环境搭建

**实体类**

```java
@Data
public class Student {
    private int id;
    private String name;
    private int tid;
}
```

```java
@Data
public class Teacher {
    private int id;
    private String name;

    //一个老师拥有多个学生
    private List<Student> students;
}
```

### 2. **按照结果嵌套嵌套处理**

```xml
<!--按结果嵌套查询-->
<select id="getTeacher" resultMap="StudentTeacher">
    SELECT s.id sid, s.name sname,t.name tname,t.id tid FROM student s, teacher t
    WHERE s.tid = t.id AND tid = #{tid}
</select>
<resultMap id="StudentTeacher" type="Teacher">
    <result property="id" column="tid"/>
    <result property="name" column="tname"/>
    <!--复杂的属性，我们需要单独处理 对象：association 集合：collection
    javaType=""指定属性的类型！
    集合中的泛型信息，我们使用ofType获取
    -->
    <collection property="students" ofType="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <result property="tid" column="tid"/>
    </collection>
</resultMap>
```

### 小结

1. 关联 - association 【多对一】
2. 集合 - collection 【一对多】

1. javaType & ofType    

1. 1. JavaType用来指定实体类中的类型
   2. ofType用来指定映射到List或者集合中的pojo类型，泛型中的约束类型

 **注意点：**

- 保证SQL的可读性，尽量保证通俗易懂
- 注意一对多和多对一，属性名和字段的问题

- 如果问题不好排查错误，可以使用日志，建议使用Log4j

面试高频

- Mysql引擎
- InnoDB底层原理

- 索引
- 索引优化

## 12、动态SQL

**什么是动态SQL：动态SQL就是根据不同的条件生成不同的SQL语句**

**所谓的动态SQL，本质上还是SQL语句，只是我们可以在SQL层面，去执行一个逻辑代码**

------

动态 SQL 是 MyBatis 的强大特性之一。如果你使用过 JDBC 或其它类似的框架，你应该能理解根据不同条件拼接 SQL 语句有多痛苦，例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号。利用动态 SQL，可以彻底摆脱这种痛苦。

### 搭建环境

```sql
CREATE TABLE `mybatis`.`blog`  (
  `id` int(10) NOT NULL AUTO_INCREMENT COMMENT '博客id',
  `title` varchar(30) NOT NULL COMMENT '博客标题',
  `author` varchar(30) NOT NULL COMMENT '博客作者',
  `create_time` datetime(0) NOT NULL COMMENT '创建时间',
  `views` int(30) NOT NULL COMMENT '浏览量',
  PRIMARY KEY (`id`)
)
```

创建一个基础工程

1.  导包 
2.  编写配置文件 

1.  编写实体类

```java
@Data
public class Blog {
    private int id;
    private String title;
    private String author;

    private Date createTime;// 属性名和字段名不一致
    private int views;
}
```

1.  编写实体类对应Mapper接口和Mapper.xml文件 

### IF

```xml
<select id="queryBlogIF" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <if test="title!=null">
            and title = #{title}
        </if>
        <if test="author!=null">
            and author = #{author}
        </if>
    </where>
</select>
```

### choose (when, otherwise)

### trim、where、set

### SQL片段

有的时候，我们可能会将一些功能的部分抽取出来，方便服用！

1.  使用SQL标签抽取公共部分可 

```xml
<sql id="if-title-author">
    <if test="title!=null">
        title = #{title}
    </if>
    <if test="author!=null">
        and author = #{author}
    </if>
</sql>
```

1.  在需要使用的地方使用Include标签引用即可

```xml
<select id="queryBlogIF" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <include refid="if-title-author"></include>
    </where>
</select>
```

注意事项：

-  最好基于单标来定义SQL片段 
-  不要存在where标签 

**动态SQL就是在拼接SQL语句，我们只要保证SQL的正确性，按照SQL的格式，去排列组合就可以了**

建议：

- 先在Mysql中写出完整的SQL，再对应的去修改成我们的动态SQL实现通用即可

## 13、缓存

### 13.1 简介

查询 ： 连接数据库，耗资源

 一次查询的结果，给他暂存一个可以直接取到的地方 --> 内存：缓存

我们再次查询的相同数据的时候，直接走缓存，不走数据库了

**什么是缓存[Cache]？   ** 

- 存在内存中的临时数据

- 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上（关系型数据库文件）查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题

**为什么使用缓存？**

- 减少和数据库的交互次数，减少系统开销，提高系统效率

**什么样的数据可以使用缓存？**

- 经常查询并且不经常改变的数据 【可以使用缓存】

### 13.2 MyBatis缓存

- MyBatis包含一个非常强大的查询缓存特性，它可以非常方便的定制和配置缓存，缓存可以极大的提高查询效率。

- MyBatis系统中默认定义了两级缓存：**一级缓存**和**二级缓存**    

  - 默认情况下，只有一级缓存开启（SqlSession级别的缓存，也称为本地缓存）

  - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
  - 为了提高可扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来定义二级缓存。

### 13.3 一级缓存

- 一级缓存也叫本地缓存：SqlSession    

  - 与数据库同一次会话期间查询到的数据会放在本地缓存中

  - 以后如果需要获取相同的数据，直接从缓存中拿，没必要再去查询数据库

测试步骤：

1.  开启日志 
2.  测试在一个Session中查询两次记录

```java
    @Test
    public void test1() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        User user = mapper.getUserById(1);
        System.out.println(user);

        System.out.println("=====================================");

        User user2 =  mapper.getUserById(1);
        System.out.println(user2 == user);
    }
```

查看日志输出 

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865195973-89007834-841e-4d18-8f85-84e2a9cf372b.png)

**缓存失效的情况：**

1.  查询不同的东西 
2.  增删改操作，可能会改变原来的数据，所以必定会刷新缓存 

1.  查询不同的Mapper.xml 
2.  手动清理缓存 .clearCache();

```plain
sqlSession.clearCache();
```

### 13.4 二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存

- 基于namespace级别的缓存，一个名称空间，对应一个二级缓存

- 工作机制    

  - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中

  - 如果会话关闭了，这个会员对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中

  - 新的会话查询信息，就可以从二级缓存中获取内容

  - 不同的mapper查询出的数据会放在自己对应的缓存（map）中

一级缓存开启（SqlSession级别的缓存，也称为本地缓存）

- 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
- 为了提高可扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来定义二级缓存。

步骤：

开启全局缓存 

```xml
<!--显示的开启全局缓存-->
<setting name="cacheEnabled" value="true"/>
```

在Mapper.xml中使用缓存

```xml
<!--在当前Mapper.xml中使用二级缓存-->
<cache
       eviction="FIFO"
       flushInterval="60000"
       size="512"
       readOnly="true"/>
```

我们需要将实体类序列化，否则就会报错

**小结：**

- 只要开启了二级缓存，在同一个Mapper下就有效
- 所有的数据都会放在一级缓存中

- 只有当前会话提交，或者关闭的时候，才会提交到二级缓存中

### 13.5 缓存原理

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1614865196194-77594747-f0a7-461a-9a79-8bbbacd4701b.png)

**注意：**

-  只有查询才有缓存，根据数据是否需要缓存（修改是否频繁选择是否开启）useCache=“true” 

```xml
    <select id="getUserById" resultType="user" useCache="true">
        select * from user where id = #{id}
    </select>
```

### 13.6 自定义缓存-ehcache

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200601132939.png)

Ehcache是一种广泛使用的开源Java分布式缓存。主要面向通用缓存

导包

```xml
<dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-ehcache</artifactId>
    <version>1.2.1</version>
</dependency>
```

在mapper中指定使用我们的ehcache缓存实现 

```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```

在classpath下配置ehcache.xml


```xml
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
	<diskStore path="F:\develop\ehcache" />
	<defaultCache 
		maxElementsInMemory="1000" 
		maxElementsOnDisk="10000000"
		eternal="false" 
		overflowToDisk="false" 
		timeToIdleSeconds="120"
		timeToLiveSeconds="120" 
		diskExpiryThreadIntervalSeconds="120"
		memoryStoreEvictionPolicy="LRU">
	</defaultCache>
</ehcache>
```

## 14、MyBatis Generator

### 配置Mybatis Generator Config

**引入外部配置文件**

MyBatis Generator config 是可以引入外部配置文件的，如下，路径为相对于当前配置文件的路径

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200724210250.png)

代码如下，注意是配置在 `<generatorConfiguration>` 下

配置文件中的内容如下

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200724210401.png)

之后可以通过 ${xxx} 来引用外部配置文件中的值

### 配置context

注意是配置在 `<generatorConfiguration>` 下

```xml
<context id="myContext" targetRuntime="MyBatis3" defaultModelType="flat">

</context>
```

* **id** : 随便填，保证多个 **context** id 不重复就行

* **defaultModelType** ： 可以不填，默认值 **conditional**，**flat**表示一张表对应一个po

* **targetRuntime** ：可以不填，默认值 **MyBatis3**，常用的还有 **MyBatis3Simple**，这个配置会影响生成的 dao 和 mapper.xml的内容

  **targetRuntime = MyBatis3Simple，生成的 dao 和 mapper.xml 如下**，接口会少很多，只包含最最常用的。

唯一需要注意的就是**targetRuntime**的值，该配置成什么看个人喜好。

**context的子元素**

上一节只是配置了 context 节点， context 里面还有子元素需要配置。

context的子元素必须按照以下给出的个数、顺序配置。(**是的，没错 MyBatis Generator 对配置的循序还有要求**)

1. **property** (0..N)
2. **plugin** (0..N)
3. **commentGenerator** (0 or 1)
4. **jdbcConnection** (需要connectionFactory 或 jdbcConnection)
5. **javaTypeResolver** (0 or 1)
6. **javaModelGenerator** (至少1个)
7. **sqlMapGenerator** (0 or 1)
8. **javaClientGenerator** (0 or 1)
9. **table** (1..N)

**plugin**

配置一个插件，例如

```xml
<plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin"/>
```

插件给生成的Java模型对象增加了equals和hashCode方法

**commentGenerator**

commentGenerator 用来配置生成的注释。默认是生成注释的，并且会生成时间戳。

如果你想要保留注释和时间戳，可以不配置 **commentGenerator**。如果你不想保留时间戳，需要如下配置

```xml
<commentGenerator>
    <!-- 不希望生成的注释中包含时间戳 -->
    <property name="suppressDate" value="true"/>
</commentGenerator>
```

默认生成的注释是不会有 db 表中字段的注释，如果你想知道每个字段在数据库中的含义(前提是数据库中对应表的字段你添加了注释)，可以如下配置

```xml
<commentGenerator>
    <!-- 添加 db 表中字段的注释 -->
    <property name="addRemarkComments" value="true"/>
</commentGenerator>
```

但MyBatis Generator 生成注释无用信息太多了。

```xml
<commentGenerator>
    <!-- 是否不生成注释 -->
    <property name="suppressAllComments" value="true"/>
</commentGenerator>
```

**jdbcConnection**

MyBatis Generator 需要链接数据库，所以需要配置 **jdbcConnection**，具体如下

```xml
<!-- jdbc连接 -->
<jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                connectionURL="jdbc:mysql://localhost:3306/wtushop?serverTimezone=Asia/Shanghai"
                userId="root"
                password="123456">
    <!--高版本的 mysql-connector-java 需要设置 nullCatalogMeansCurrent=true-->
    <property name="nullCatalogMeansCurrent" value="true"/>
</jdbcConnection>
```

这里面值得注意的是`<property name="nullCatalogMeansCurrent" value="true"/>`,因为我用的 **mysql-connector-java** 版本是 8.0.17,如果不配置这一项，会找不到对应的数据库，[官网](http://www.mybatis.org/generator/usage/mysql.html)对此的解释是

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200724211955.png)

**javaTypeResolver**

javaTypeResolver 是配置 JDBC 与 java 的类型转换规则，或者你也可以不用配置，使用它默认的转换规则。

就算配置也只能配置 bigDecimal 类型和时间类型的转换

```xml
<javaTypeResolver>
    <!--是否使用 bigDecimal，默认false。
        false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer
        true，把JDBC DECIMAL 和 NUMERIC 类型解析为java.math.BigDecimal-->
    <property name="forceBigDecimals" value="true"/>
    <!--默认false
        false，将所有 JDBC 的时间类型解析为 java.util.Date
        true，将 JDBC 的时间类型按如下规则解析
            DATE	                -> java.time.LocalDate
            TIME	                -> java.time.LocalTime
            TIMESTAMP                   -> java.time.LocalDateTime
            TIME_WITH_TIMEZONE  	-> java.time.OffsetTime
            TIMESTAMP_WITH_TIMEZONE	-> java.time.OffsetDateTime
        -->
    <property name="useJSR310Types" value="true"/>
</javaTypeResolver>
```

**javaModelGenerator**

配置 pojo 生成的包路径和项目路径,如下

```xml
<!-- 生成实体类地址 -->
<javaModelGenerator targetPackage="wtushop.pojo" targetProject="src/main/java">
    <!-- 是否让 schema 作为包的后缀，默认为false -->
    <!--<property name="enableSubPackages" value="false"/>-->
    <!-- 是否针对string类型的字段在set方法中进行修剪，默认false -->
    <property name="trimStrings" value="true"/>
</javaModelGenerator>
```

`<property name="trimStrings" value="true"/>` 生成出来的 set 方法如下

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200724212622.png)

`<property name="enableSubPackages" value="true"/>` 时，可能会在 po 目录下在创建一个 `“数据库名”` 的文件夹，生成的 po 会放在该文件夹下，也就是说会多一层目录，用的上的可以配置

**sqlMapGenerator**

配置 Mapper.xml 文件的生成目录

```xml
<sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
    <!--<property name="enableSubPackages" value="false"/>-->
</sqlMapGenerator>
```

**javaClientGenerator**

配置 XxxMapper.java 文件的生成目录

```xml
<!-- 生成 XxxMapper.java 接口-->
<javaClientGenerator targetPackage="wtushop.dao" targetProject="src/main/java" type="XMLMAPPER">
    <!--<property name="enableSubPackages" value="false"/>-->
</javaClientGenerator>
```

`type="XMLMAPPER"` 会将接口的实现放在 mapper.xml中，也推荐这样配置。

也可以设置 type 为其他值，比如 `type="ANNOTATEDMAPPER"`，接口的实现通过注解写在接口上面。如果采用这种方式，不会生成 mapper.xml 也不用配置 `<sqlMapGenerator>`,但是采用注解来实现接口应对简单查询还好，如果是复杂查询并不如xml方便，所以还是建议将`type`配置成`XMLMAPPER`

**table**

一个 table 对应一张表，如果想同时生成多张表，需要配置多个 table

```xml
<!-- schema为数据库名，oracle需要配置，mysql不需要配置。
     tableName为对应的数据库表名
     domainObjectName 是要生成的实体类名(可以不指定，默认按帕斯卡命名法将表名转换成类名)
     enableXXXByExample 默认为 true， 为 true 会生成一个对应Example帮助类，帮助你进行条件查询，不想要可以设为false -->
<table schema="" tableName="admin" domainObjectName="Admin"
       enableCountByExample="true" enableDeleteByExample="true" enableSelectByExample="true"
       enableUpdateByExample="true" selectByExampleQueryId="true">
    <!--是否使用实际列名,默认为false-->
    <!--<property name="useActualColumnNames" value="false" />-->
</table>
<table schema="" tableName="userinfo" domainObjectName="UserInfo"
       enableCountByExample="true" enableDeleteByExample="true" enableSelectByExample="true"
       enableUpdateByExample="true" selectByExampleQueryId="true">
    <!--是否使用实际列名,默认为false-->
    <!--<property name="useActualColumnNames" value="false" />-->
</table>
<table schema="" tableName="category" domainObjectName="Category"
       enableCountByExample="true" enableDeleteByExample="true" enableSelectByExample="true"
       enableUpdateByExample="true" selectByExampleQueryId="true">
    <!--是否使用实际列名,默认为false-->
    <!--<property name="useActualColumnNames" value="false" />-->
</table>
<table schema="" tableName="goods" domainObjectName="Goods"
       enableCountByExample="true" enableDeleteByExample="true" enableSelectByExample="true"
       enableUpdateByExample="true" selectByExampleQueryId="true">
    <!--是否使用实际列名,默认为false-->
    <!--<property name="useActualColumnNames" value="false" />-->
</table>
```

其中 **domainObjectName** 不配置时，它会按照帕斯卡命名法将表名转换成类名

**enableXXXByExample** 默认为true，但只有在`targetRuntime="MyBatis3"`时才生效

生效时，会在pojo下多生成一个 `XxxExample.java` 的文件，如下

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200724213822.png)

### 整体配置

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--mybatis的代码生成器相关配置-->
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!-- 引入配置文件 -->
    <!--<properties resource="application-dev.properties"/>-->

    <!-- 一个数据库一个context,context的子元素必须按照它给出的顺序
        property*,plugin*,commentGenerator?,jdbcConnection,javaTypeResolver?,
        javaModelGenerator,sqlMapGenerator?,javaClientGenerator?,table+
    -->
    <context id="myContext" targetRuntime="MyBatis3" defaultModelType="flat">

        <!-- 这个插件给生成的Java模型对象增加了equals和hashCode方法 -->
        <!--<plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin"/>-->

        <!-- 注释 -->
        <commentGenerator>
            <!-- 是否不生成注释 -->
            <property name="suppressAllComments" value="true"/>
            <!-- 不希望生成的注释中包含时间戳 -->
            <!--<property name="suppressDate" value="true"/>-->
            <!-- 添加 db 表中字段的注释，只有suppressAllComments为false时才生效-->
            <!--<property name="addRemarkComments" value="true"/>-->
        </commentGenerator>


        <!-- jdbc连接 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/wtushop?serverTimezone=Asia/Shanghai"
                        userId="root"
                        password="123456">
            <!--高版本的 mysql-connector-java 需要设置 nullCatalogMeansCurrent=true-->
            <property name="nullCatalogMeansCurrent" value="true"/>
        </jdbcConnection>

        <!-- 类型转换 -->
        <javaTypeResolver>
            <!--是否使用bigDecimal，默认false。
                false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer
                true，把JDBC DECIMAL 和 NUMERIC 类型解析为java.math.BigDecimal-->
            <property name="forceBigDecimals" value="true"/>
            <!--默认false
                false，将所有 JDBC 的时间类型解析为 java.util.Date
                true，将 JDBC 的时间类型按如下规则解析
                    DATE	                -> java.time.LocalDate
                    TIME	                -> java.time.LocalTime
                    TIMESTAMP               -> java.time.LocalDateTime
                    TIME_WITH_TIMEZONE  	-> java.time.OffsetTime
                    TIMESTAMP_WITH_TIMEZONE	-> java.time.OffsetDateTime
                -->
            <!--<property name="useJSR310Types" value="false"/>-->
        </javaTypeResolver>

        <!-- 生成实体类地址 -->
        <javaModelGenerator targetPackage="wtushop.pojo" targetProject="src/main/java">
            <!-- 是否让 schema 作为包的后缀，默认为false -->
            <!--<property name="enableSubPackages" value="false"/>-->
            <!-- 是否针对string类型的字段在set方法中进行修剪，默认false -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>


        <!-- 生成Mapper.xml文件 -->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
            <!--<property name="enableSubPackages" value="false"/>-->
        </sqlMapGenerator>

        <!-- 生成 XxxMapper.java 接口-->
        <javaClientGenerator targetPackage="wtushop.dao" targetProject="src/main/java" type="XMLMAPPER">
            <!--<property name="enableSubPackages" value="false"/>-->
        </javaClientGenerator>


        <!-- schema为数据库名，oracle需要配置，mysql不需要配置。
             tableName为对应的数据库表名
             domainObjectName 是要生成的实体类名(可以不指定，默认按帕斯卡命名法将表名转换成类名)
             enableXXXByExample 默认为 true， 为 true 会生成一个对应Example帮助类，帮助你进行条件查询，不想要可以设为false
             -->
        <table schema="" tableName="admin" domainObjectName="Admin"
               enableCountByExample="true" enableDeleteByExample="true" enableSelectByExample="true"
               enableUpdateByExample="true" selectByExampleQueryId="true">
            <!--是否使用实际列名,默认为false-->
            <!--<property name="useActualColumnNames" value="false" />-->
        </table>
        <table schema="" tableName="userinfo" domainObjectName="UserInfo"
               enableCountByExample="true" enableDeleteByExample="true" enableSelectByExample="true"
               enableUpdateByExample="true" selectByExampleQueryId="true">
            <!--是否使用实际列名,默认为false-->
            <!--<property name="useActualColumnNames" value="false" />-->
        </table>
        <table schema="" tableName="category" domainObjectName="Category"
               enableCountByExample="true" enableDeleteByExample="true" enableSelectByExample="true"
               enableUpdateByExample="true" selectByExampleQueryId="true">
            <!--是否使用实际列名,默认为false-->
            <!--<property name="useActualColumnNames" value="false" />-->
        </table>
        <table schema="" tableName="goods" domainObjectName="Goods"
               enableCountByExample="true" enableDeleteByExample="true" enableSelectByExample="true"
               enableUpdateByExample="true" selectByExampleQueryId="true">
            <!--是否使用实际列名,默认为false-->
            <!--<property name="useActualColumnNames" value="false" />-->
        </table>
    </context>
</generatorConfiguration>
```

### 使用方式

1.Mybatis Generator 插件方式

1.引入Mybatis Generator插件

在pom的根节点下添加一下配置

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.7</version>
        </plugin>
    <plugins>    
</build>
```

**2.配置 MyBatis Generator 插件**

光引入 MyBatis Generator 插件还不行，还得配置 MyBatis Generator插件

3. **配置 MyBatis Generator config 文件路径**

MyBatis Generator 插件需要根据一个 **MyBatis Generator config** 文件，来具体运行。配置如下，版本我用的是目前最新的版本 1.3.7

```xml
<build>
  <plugins>
      <plugin>
          <groupId>org.mybatis.generator</groupId>
          <artifactId>mybatis-generator-maven-plugin</artifactId>
          <version>1.3.7</version>
          <configuration>
              <!--mybatis的代码生成器的配置文件-->
              <configurationFile>src/main/resources/generatorConfig.xml</configurationFile>           		   </configuration>
     </plugin>
 <plugins>    
</build>

```

注意，这个路径是你的配置文件相对于该 pom 文件的路径

**允许覆盖生成的文件**

有时候我们的数据库表添加了新字段，需要重新生成对应的文件。常规做法是手动删除旧文件，然后在用 MyBatis Generator 生成新文件。当然你也可以选择让 MyBatis Generator 覆盖旧文件，省下手动删除的步骤。

**配置如下**

```xml
<build>
  <plugins>
      <plugin>
          <groupId>org.mybatis.generator</groupId>
          <artifactId>mybatis-generator-maven-plugin</artifactId>
          <version>1.3.7</version>
          <configuration>
              <!--mybatis的代码生成器的配置文件-->
              <configurationFile>src/main/resources/generatorConfig.xml</configurationFile>
          	  <!--允许覆盖生成的文件-->
              <overwrite>true</overwrite>
          </configuration>
     </plugin>
 <plugins>    
</build>
```

> 值得注意的是，MyBatis Generator 只会覆盖旧的 po、dao、而 *mapper.xml 不会覆盖，而是追加，这样做的目的是防止用户自己写的 sql 语句一不小心都被 MyBatis Generator 给覆盖了

#### 添加数据库驱动依赖

MyBatis Generator 需要链接数据库，肯定是需要对应数据库驱动的依赖的。

如下，给 MyBatis Generator 添加数据库驱动依赖

```xml
<build>
  <plugins>
      <plugin>
          <groupId>org.mybatis.generator</groupId>
          <artifactId>mybatis-generator-maven-plugin</artifactId>
          <version>1.3.7</version>
          <configuration>
              <!--mybatis的代码生成器的配置文件-->
              <configurationFile>src/main/resources/generatorConfig.xml</configurationFile>
          	  <!--允许覆盖生成的文件-->
              <overwrite>true</overwrite>
          </configuration>
          <dependencies>
                <!-- mysql的JDBC驱动 -->
                <dependency>
                    <groupId>mysql</groupId>
                    <artifactId>mysql-connector-java</artifactId>
                    <version>8.0.17</version>
                </dependency>
          </dependencies>
     </plugin>
 <plugins>    
</build>
```

我用的数据库是 mysql ，其他数据库同理。注意数据库驱动的版本号，不同的版本对应的 MyBatis Generator 配置有些许不同。

大部分情况下，我们的项目中已经配置过了对应数据库的JDBC驱动。现在在插件中又配置一次，感觉有些冗余，maven 提供了 **includeCompileDependencies** 属性，让我们在插件中引用 dependencies 的依赖，这样就不需要重复配置了。

**配置如下**

```xml
<build>
  <plugins>
      <plugin>
          <groupId>org.mybatis.generator</groupId>
          <artifactId>mybatis-generator-maven-plugin</artifactId>
          <version>1.3.7</version>
          <configuration>
              <!--mybatis的代码生成器的配置文件-->
              <configurationFile>src/main/resources/generatorConfig.xml</configurationFile>
          	  <!--允许覆盖生成的文件-->
              <overwrite>true</overwrite>
              <!--将当前pom的依赖项添加到生成器的类路径中-->
              <includeCompileDependencies>true</includeCompileDependencies>
          </configuration>
     </plugin>
 <plugins>    
</build>
```

**添加其他依赖**

一般配置了 **includeCompileDependencies** 后就不需要配置其他依赖了，因为 **includeCompileDependencies** 会将当前 pom 的 **dependencies** 中所以 **Compile** 期的依赖全部添加到生成器的类路径中。

但有的人不想配置 **includeCompileDependencies** ，或者想在MyBatis Generator插件中使用另一个版本的依赖，就可以配置 **dependencies**

另外，我看到网上大部分文章都会配置 **mybatis-generator-core** 这个依赖，但是 MyBatis Generator 官网的案例中都没有提到说要配置这个依赖，我没有配置，并且可以正常使用 MyBatis Generator。

### java代码方式

**1.引入Mybatis Generator Core的依赖**

```xml
<dependency>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-core</artifactId>
    <version>1.4.0</version>
</dependency>
```

**2.编写java代码**

```java
package wtushop;

import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

import java.io.File;
import java.util.ArrayList;
import java.util.List;

/**
 * @Package wtushop
 * @ClassName GeneratorCode
 * @Description TODO
 * @Date 20/7/24 17:41
 * @Author krislin
 * @Version V1.0
 */
public class GeneratorCode {
    public static void main(String[] args) {
        try {
            List<String> warnings = new ArrayList<String>();
            boolean overwrite = true;
            File configFile = new File("src/main/resources/generatorConfig.xml");
            ConfigurationParser cp = new ConfigurationParser(warnings);
            Configuration config = cp.parseConfiguration(configFile);
            DefaultShellCallback callback = new DefaultShellCallback(overwrite);
            MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
            myBatisGenerator.generate(null);
            System.out.println("Generator Code Success!");
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```



## 15、MyBatis Plus



### 1.简介

MyBatis-Plus 是一个 Mybatis 增强版工具，在 MyBatis 上扩充了其他功能没有改变其基本功能，为了简化开发提交效率而存在。

官网文档地址：
　　https://mp.baomidou.com/guide/

MyBatis-Plus 特性：
　　https://mp.baomidou.com/guide/#%E7%89%B9%E6%80%A7

**使用 SpringBoot 快速使用 MyBatis-Plus**

1. 创建一个 SpringBoot 项目。

   - 方式一：去官网 https://start.spring.io/ 初始化一个，然后导入 IDE 工具即可。
     
   - 方式二：直接使用 IDE 工具创建一个。 Spring Initializer。

2. 添加 MyBatis-Plus 依赖（mybatis-plus-boot-starter）

   ```xml
   <dependency>
       <groupId>com.baomidou</groupId>
       <artifactId>mybatis-plus-boot-starter</artifactId>
       <version>3.3.1.tmp</version>
   </dependency>
   ```

3. 为了测试开发，此处使用 mysql 8，需要引入 mysql 相关依赖。为了简化代码，引入 lombok 依赖（减少 getter、setter 等方法）。

   ```xml
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>8.0.18</version>
   </dependency>
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.10</version>
   </dependency>
   ```

4. 使用一个表进行测试。仅供参考，可以定义 创建时间、修改时间等字段。

   ```sql
   DROP DATABASE IF EXISTS testMyBatisPlus;
   
   CREATE DATABASE testMyBatisPlus;
   
   USE testMyBatisPlus;
   
   DROP TABLE IF EXISTS user;
   
   CREATE TABLE user
   (
       id BIGINT(20) NOT NULL COMMENT '主键ID',
       name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
       age INT(11) NULL DEFAULT NULL COMMENT '年龄',
       email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
       PRIMARY KEY (id)
   );
   
   INSERT INTO user (id, name, age, email) VALUES
   (1, 'Jone', 18, 'test1@baomidou.com'),
   (2, 'Jack', 20, 'test2@baomidou.com'),
   (3, 'Tom', 28, 'test3@baomidou.com'),
   (4, 'Sandy', 21, 'test4@baomidou.com'),
   (5, 'Billie', 24, 'test5@baomidou.com');
   ```

5. 在 application.yml 文件中配置 mysql 数据源信息。

   ```yaml
   spring:
     datasource:
       driver-class-name: com.mysql.cj.jdbc.Driver
       username: root
       password: 123456
       url: jdbc:mysql://localhost:3306/testMyBatisPlus?useUnicode=true&characterEncoding=utf8
   ```

6. 编写表对应的实体类。

   ```java
   package entity;
   
   import lombok.Data;
   
   @Data
   public class User {
       private Long id;
       private String name;
       private int age;
       private String email;
   }
   ```

7. 编写操作实体类的 Mapper 类。直接继承 BaseMapper，这是 mybatis-plus 封装好的类。

   ```java
   package mapper;
   
   import bean.User;
   import com.baomidou.mybatisplus.core.mapper.BaseMapper;
   
   public interface UserMapper extends BaseMapper<User> {
   }
   ```

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727172032.png)

8. 实体类、Mapper 类都写好了，就可以使用了。
   Step1：先得在启动类里扫描 Mapper 类，即添加 @MapperScan 注解

   ```java
   import org.mybatis.spring.annotation.MapperScan;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   @MapperScan("mapper")
   @SpringBootApplication
   public class TestMybatisPlusApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(TestMybatisPlusApplication.class, args);
       }
   
   }
   ```

   Step2：写一个测试类测试一下。

   ```java
   import bean.User;
   import mapper.UserMapper;
   import org.junit.jupiter.api.Test;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.test.context.SpringBootTest;
   
   import java.util.List;
   
   @SpringBootTest
   class TestMybatisPlusApplicationTests {
   
       @Autowired
       private UserMapper userMapper;
   
       @Test
       public void testSelect() {
           System.out.println(("----- selectAll method test ------"));
           List<User> userList = userMapper.selectList(null);
           for(User user:userList) {
               System.out.println(user);
           }
       }
   }
   ```

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727172310.png)

9. 总结：通过以上简单操作，就能对 user 表进行 CRUD 操作，不需要去编写 xml 文件。
   注：若遇到 @Autowired 标记的变量出现 红色下划线，但是不影响 正常运行。

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727172450.png)

   可以进入 Settings，找到 Inspection，并选择其中的 Spring Core -> Code -> Autowiring for Bean Class,将 Error 改为 Warning，即可。

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727172542.png)

### 2.Mybatis-Plus 常用操作

#### 2.1 配置日志(两种方式)

**1.注解方式，yml文件配置上以下就可以直接使用**

```yaml
mybatis-plus:
  mapper-locations: classpath:mapper/*.xml
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

**2.在sqlSessionFactory中配置**

```java
@Bean("sqlSessionFactory")
public SqlSessionFactory sqlSessionFactory() throws Exception {
    // 导入mybatissqlsession配置
    MybatisSqlSessionFactoryBean sessionFactory = new MybatisSqlSessionFactoryBean();
    // 指明数据源
    sessionFactory.setDataSource(multipleDataSource(dataSource0(), dataSource1(), dataSource2()));
    // 指明mapper.xml位置(配置文件中指明的xml位置会失效用此方式代替，具体原因未知)
    sessionFactory.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath*:/mapper/**Mapper.xml"));
    // 指明实体扫描(多个package用逗号或者分号分隔)
    sessionFactory.setTypeAliasesPackage("cn.krislin.entity");
    // 导入mybatis配置
    MybatisConfiguration configuration = new MybatisConfiguration();
    configuration.setJdbcTypeForNull(JdbcType.NULL);
    configuration.setMapUnderscoreToCamelCase(true);
    configuration.setCacheEnabled(false);
    // 配置打印sql语句
    configuration.setLogImpl(StdOutImpl.class);
    sessionFactory.setConfiguration(configuration);
    // 添加分页功能
    sessionFactory.setPlugins(new Interceptor[]{
        paginationInterceptor()
    });
    // 导入全局配置
    sessionFactory.setGlobalConfig(globalConfiguration());
    return sessionFactory.getObject();
}
```

主要就是这句

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727173327.png)

点击setLogImpl看源码，找到Configuration()构造方法，就可以看见了

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727173359.png)

#### 2.2 常用注解

```
【@TableName 】
    @TableName               用于定义表名
注：
    常用属性：
        value                用于定义表名

【@TableId】
    @TableId                 用于定义表的主键
注：
    常用属性：
        value           用于定义主键字段名
        type            用于定义主键类型（主键策略 IdType）

   主键策略：
      IdType.AUTO          主键自增，系统分配，不需要手动输入
      IdType.NONE          未设置主键
      IdType.INPUT         需要自己输入 主键值。
      IdType.ASSIGN_ID     系统分配 ID，用于数值型数据（Long，对应 mysql 中 BIGINT 类型）。
      IdType.ASSIGN_UUID   系统分配 UUID，用于字符串型数据（String，对应 mysql 中 varchar(32) 类型）。

【@TableField】  
    @TableField            用于定义表的非主键字段。
注：
    常用属性：
        value                用于定义非主键字段名
        exist                用于指明是否为数据表的字段， true 表示是，false 为不是。
        fill                 用于指定字段填充策略（FieldFill）。
        
    字段填充策略：（一般用于填充 创建时间、修改时间等字段）
        FieldFill.DEFAULT         默认不填充
        FieldFill.INSERT          插入时填充
        FieldFill.UPDATE          更新时填充
        FieldFill.INSERT_UPDATE   插入、更新时填充。

【@TableLogic】
    @TableLogic           用于定义表的字段进行逻辑删除（非物理删除）
注：
    常用属性：
        value            用于定义未删除时字段的值
        delval           用于定义删除时字段的值
        
【@Version】
    @Version             用于字段实现乐观锁
```

#### 2.3 代码生成器

1. AutoGenerator 简介

   AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。
   与 mybatis 中的 mybatis-generator-core 类似。

2. 添加依赖

   ```xml
   <dependency>
       <groupId>com.baomidou</groupId>
       <artifactId>mybatis-plus-generator</artifactId>
       <version>3.3.1.tmp</version>
   </dependency>
   <!-- 添加 模板引擎 依赖 -->
   <dependency>
       <groupId>org.apache.velocity</groupId>
       <artifactId>velocity-engine-core</artifactId>
       <version>2.2</version>
   </dependency>
   ```

3. 
   Step1：创建一个 代码生成器。用于生成代码。

   ```java
   // Step1：代码生成器
   AutoGenerator mpg = new AutoGenerator();
   ```

   Step2：配置全局信息。指定代码输出路径，以及包名、作者等信息。

   ```java
   // Step2：全局配置
   GlobalConfig gc = new GlobalConfig();
   // 填写代码生成的目录(需要修改)
   String projectPath = "E:\\myProject\\test\\test_mybatis_plus";
   // 拼接出代码最终输出的目录
   gc.setOutputDir(projectPath + "/src/main/java");
   // 配置开发者信息（可选）（需要修改）
   gc.setAuthor("krislin");
   // 配置是否打开目录，false 为不打开（可选）
   gc.setOpen(false);
   // 实体属性 Swagger2 注解，添加 Swagger 依赖，开启 Swagger2 模式（可选）
   //gc.setSwagger2(true);
   // 重新生成文件时是否覆盖，false 表示不覆盖（可选）
   gc.setFileOverride(false);
   // 配置主键生成策略，此处为 ASSIGN_ID（可选）
   gc.setIdType(IdType.ASSIGN_ID);
   // 配置日期类型，此处为 ONLY_DATE（可选）
   gc.setDateType(DateType.ONLY_DATE);
   // 默认生成的 service 会有 I 前缀
   gc.setServiceName("%sService");
   mpg.setGlobalConfig(gc);
   ```

   Step3：配置数据源信息。用于指定 需要生成代码的 数据仓库、数据表。
   

   ```java
   // Step3：数据源配置（需要修改）
   DataSourceConfig dsc = new DataSourceConfig();
   // 配置数据库 url 地址
   dsc.setUrl("jdbc:mysql://localhost:3306/testMyBatisPlus?useUnicode=true&characterEncoding=utf8");
   // dsc.setSchemaName("testMyBatisPlus"); // 可以直接在 url 中指定数据库名
   // 配置数据库驱动
   dsc.setDriverName("com.mysql.cj.jdbc.Driver");
   // 配置数据库连接用户名
   dsc.setUsername("root");
   // 配置数据库连接密码
   dsc.setPassword("123456");
   mpg.setDataSource(dsc);
   ```

   Step4：配置包信息。
   

   ```java
   // Step:4：包配置
   PackageConfig pc = new PackageConfig();
   // 配置父包名（需要修改）
   pc.setParent("com.krislin.test");
   // 配置模块名（需要修改）
   pc.setModuleName("test_mybatis_plus");
   // 配置 entity 包名
   pc.setEntity("entity");
   // 配置 mapper 包名
   pc.setMapper("mapper");
   // 配置 service 包名
   pc.setService("service");
   // 配置 controller 包名
   pc.setController("controller");
   mpg.setPackageInfo(pc);
   ```

   Step5：配置数据表映射信息。

   ```java
   // Step5：策略配置（数据库表配置）
   StrategyConfig strategy = new StrategyConfig();
   // 指定表名（可以同时操作多个表，使用 , 隔开）（需要修改）
   strategy.setInclude("test_mybatis_plus_user");
   // 配置数据表与实体类名之间映射的策略
   strategy.setNaming(NamingStrategy.underline_to_camel);
   // 配置数据表的字段与实体类的属性名之间映射的策略
   strategy.setColumnNaming(NamingStrategy.underline_to_camel);
   // 配置 lombok 模式
   strategy.setEntityLombokModel(true);
   // 配置 rest 风格的控制器（@RestController）
   strategy.setRestControllerStyle(true);
   // 配置驼峰转连字符
   strategy.setControllerMappingHyphenStyle(true);
   // 配置表前缀，生成实体时去除表前缀
   // 此处的表名为 test_mybatis_plus_user，模块名为 test_mybatis_plus，去除前缀后剩下为 user。
   strategy.setTablePrefix(pc.getModuleName() + "_");
   mpg.setStrategy(strategy);
   ```

   Step6：执行代码生成操作。

   ```java
   // Step6：执行代码生成操作
   mpg.execute();
   ```

完整配置如下：

```java
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.rules.DateType;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
import org.junit.jupiter.api.Test;

public class TestAutoGenerate {
    @Test
    public void autoGenerate() {
        // Step1：代码生成器
        AutoGenerator mpg = new AutoGenerator();

        // Step2：全局配置
        GlobalConfig gc = new GlobalConfig();
        // 填写代码生成的目录(需要修改)
        String projectPath = "E:\\myProject\\test\\test_mybatis_plus";
        // 拼接出代码最终输出的目录
        gc.setOutputDir(projectPath + "/src/main/java");
        // 配置开发者信息（可选）（需要修改）
        gc.setAuthor("krislin");
        // 配置是否打开目录，false 为不打开（可选）
        gc.setOpen(false);
        // 实体属性 Swagger2 注解，添加 Swagger 依赖，开启 Swagger2 模式（可选）
        //gc.setSwagger2(true);
        // 重新生成文件时是否覆盖，false 表示不覆盖（可选）
        gc.setFileOverride(false);
        // 配置主键生成策略，此处为 ASSIGN_ID（可选）
        gc.setIdType(IdType.ASSIGN_ID);
        // 配置日期类型，此处为 ONLY_DATE（可选）
        gc.setDateType(DateType.ONLY_DATE);
        // 默认生成的 service 会有 I 前缀
        gc.setServiceName("%sService");
        mpg.setGlobalConfig(gc);

        // Step3：数据源配置（需要修改）
        DataSourceConfig dsc = new DataSourceConfig();
        // 配置数据库 url 地址
        dsc.setUrl("jdbc:mysql://localhost:3306/testMyBatisPlus?useUnicode=true&characterEncoding=utf8");
        // dsc.setSchemaName("testMyBatisPlus"); // 可以直接在 url 中指定数据库名
        // 配置数据库驱动
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        // 配置数据库连接用户名
        dsc.setUsername("root");
        // 配置数据库连接密码
        dsc.setPassword("123456");
        mpg.setDataSource(dsc);

        // Step:4：包配置
        PackageConfig pc = new PackageConfig();
        // 配置父包名（需要修改）
        pc.setParent("com.krislin.test");
        // 配置模块名（需要修改）
        pc.setModuleName("test_mybatis_plus");
        // 配置 entity 包名
        pc.setEntity("entity");
        // 配置 mapper 包名
        pc.setMapper("mapper");
        // 配置 service 包名
        pc.setService("service");
        // 配置 controller 包名
        pc.setController("controller");
        mpg.setPackageInfo(pc);

        // Step5：策略配置（数据库表配置）
        StrategyConfig strategy = new StrategyConfig();
        // 指定表名（可以同时操作多个表，使用 , 隔开）（需要修改）
        strategy.setInclude("test_mybatis_plus_user");
        // 配置数据表与实体类名之间映射的策略
        strategy.setNaming(NamingStrategy.underline_to_camel);
        // 配置数据表的字段与实体类的属性名之间映射的策略
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        // 配置 lombok 模式
        strategy.setEntityLombokModel(true);
        // 配置 rest 风格的控制器（@RestController）
        strategy.setRestControllerStyle(true);
        // 配置驼峰转连字符
        strategy.setControllerMappingHyphenStyle(true);
        // 配置表前缀，生成实体时去除表前缀
        // 此处的表名为 test_mybatis_plus_user，模块名为 test_mybatis_plus，去除前缀后剩下为 user。
        strategy.setTablePrefix(pc.getModuleName() + "_");
        mpg.setStrategy(strategy);

        // Step6：执行代码生成操作
        mpg.execute();
    }
}
```

#### 2.4 自动填充数据功能

1. 添加、修改数据时，每次都会使用相同的方式进行填充。比如 数据的创建时间、修改时间等。Mybatis-plus 支持自动填充这些字段的数据。

   给之前的数据表新增两个字段：创建时间、修改时间。

   ```sql
   CREATE TABLE test_mybatis_plus_user
   (
       id BIGINT NOT NULL COMMENT '主键ID',
       name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
       age INT(11) NULL DEFAULT NULL COMMENT '年龄',
       email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
       create_time timestamp NULL DEFAULT NULL COMMENT '创建时间',
       update_time timestamp NULL DEFAULT NULL COMMENT '最后修改时间', 
       PRIMARY KEY (id)
   );
   ```

2. 未使用自动填充时

   未使用 自动填充时，每次添加、修改数据都可以手动对其进行添加。

   ```java
   @SpringBootTest
   class TestMybatisPlusApplicationTests {
   
       @Autowired
       private UserService userService;
   
       @Test
       public void testUpdate() {
           User user = new User();
           user.setName("tom").setAge(20).setEmail("tom@163.com");
           // 手动添加数据
           user.setCreateTime(new Date()).setUpdateTime(new Date());
           if (userService.save(user)) {
               userService.list().forEach(System.out::println);
           } else {
               System.out.println("添加数据失败");
           }
       }
   }
   ```

3. 使用自动填充功能。

   Step1：使用 @TableField 注解，标注需要进行填充的字段。

   ```java
   /**
    * 创建时间
    */
   @TableField(fill = FieldFill.INSERT)
   private Date createTime;
   
   /**
    * 最后修改时间
    */
   @TableField(fill = FieldFill.INSERT_UPDATE)
   private Date updateTime;
   ```

   Step2：自定义一个类，实现 MetaObjectHandler 接口，并重写方法。添加 @Component 注解，交给 Spring 去管理。

   ```java
   import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
   import org.apache.ibatis.reflection.MetaObject;
   import org.springframework.stereotype.Component;
   
   import java.util.Date;
   
   @Component
   public class MyMetaObjectHandler implements MetaObjectHandler {
       @Override
       public void insertFill(MetaObject metaObject) {
           this.strictInsertFill(metaObject, "createTime", Date.class, new Date());
           this.strictInsertFill(metaObject, "updateTime", Date.class, new Date());
       }
   
       @Override
       public void updateFill(MetaObject metaObject) {
           this.strictUpdateFill(metaObject, "updateTime", Date.class, new Date());
       }
   }
   ```

   Step3：简单测试一下。

   ```java
   @Test
   public void testAutoFill() {
       User user = new User();
       user.setName("tom").setAge(20).setEmail("tom@163.com");
       if (userService.save(user)) {
           userService.list().forEach(System.out::println);
       } else {
           System.out.println("添加数据失败");
       }
   }
   ```

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727174931.png)

#### 2.5 逻辑删除

1. 删除数据，可以通过物理删除，也可以通过逻辑删除。
      　

   - 物理删除指的是直接将数据从数据库中删除，不保留。
     
   - 逻辑删除指的是修改数据的某个字段，使其表示为已删除状态，而非删除数据，保留该数据在数据库中，但是查询时不显示该数据（查询时过滤掉该数据）。

      　给数据表增加一个字段：delete_flag，用于表示该数据是否被逻辑删除。

   ```sql
   CREATE TABLE test_mybatis_plus_user
   (
       id BIGINT NOT NULL COMMENT '主键ID',
       name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
       age INT(11) NULL DEFAULT NULL COMMENT '年龄',
       email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
       create_time timestamp NULL DEFAULT NULL COMMENT '创建时间',
       update_time timestamp NULL DEFAULT NULL COMMENT '最后修改时间', 
       delete_flag tinyint(1) NULL DEFAULT NULL COMMENT '逻辑删除（0 未删除、1 删除）',
       PRIMARY KEY (id)
   );
   ```

2. 使用逻辑删除。
   可以定义一个自动填充规则，初始值为 0。0 表示未删除， 1 表示删除。

   ```java
   /**
    * 逻辑删除（0 未删除、1 删除）
    */
   @TableLogic(value = "0", delval = "1")
   @TableField(fill = FieldFill.INSERT)
   private Integer deleteFlag;
   
   
   @Override
   public void insertFill(MetaObject metaObject) {
       this.strictInsertFill(metaObject, "deleteFlag", Integer.class, 0);
   }
   ```

3. 简单测试
   使用 mybatis-plus 封装好的方法时，会自动添加逻辑删除的功能。
   若是自定义的 sql 语句，需要手动添加逻辑。

   ```java
   @Test
   public void testDelete() {
       if (userService.removeById(1258924257048547329L)) {
           System.out.println("删除数据成功");
           userService.list().forEach(System.out::println);
       } else {
           System.out.println("删除数据失败");
       }
   }
   ```

   现有数据 

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727175344.png)

   执行 testDelete 进行逻辑删除。

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727175434.png)

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727175506.png)

   若去除 TableLogic 注解，再执行 testDelete 时进行物理删除，直接删除这条数据。

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727175605.png)

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727175636.png)

#### 2.6 分页插件的使用

1. 简介
   与 mybatis 的插件 pagehelper 用法类似。通过简单的配置即可使用。

2. 使用

   Step1：配置分页插件。编写一个 配置类，内部使用 @Bean 注解将 PaginationInterceptor 交给 Spring 容器管理。

   ```java
   import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
   import org.mybatis.spring.annotation.MapperScan;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   /**
    * 自定义一个配置类，mapper 扫描也可在此写上
    */
   @Configuration
   @MapperScan("com.krislin.test.test_mybatis_plus.mapper")
   public class Myconfig {
       /**
        * 分页插件
        * @return 分页插件的实例
        */
       @Bean
       public PaginationInterceptor paginationInterceptor() {
           return new PaginationInterceptor();
       }
   }
   ```

   Step2：编写分页代码。直接 new 一个 Page 对象，对象需要传递两个参数（当前页，每页显示的条数）。调用 mybatis-plus 提供的分页查询方法，其会将 分页查询的数据封装到 Page 对象中。

   ```java
   @Test
   public void testPage() {
       // Step1：创建一个 Page 对象
       Page<User> page = new Page<>();
       // Page<User> page = new Page<>(2, 5);
       // Step2：调用 mybatis-plus 提供的分页查询方法
       userService.page(page, null);
       // Step3：获取分页数据
       System.out.println(page.getCurrent()); // 获取当前页
       System.out.println(page.getTotal()); // 获取总记录数
       System.out.println(page.getSize()); // 获取每页的条数
       System.out.println(page.getRecords()); // 获取每页数据的集合
       System.out.println(page.getPages()); // 获取总页数
       System.out.println(page.hasNext()); // 是否存在下一页
       System.out.println(page.hasPrevious()); // 是否存在上一页
   }
   ```

#### 2.7 乐观锁的实现

1. 乐观锁两种实现方式

   - 方式一：通过版本号机制实现。
   - 方式二：通过 CAS 算法实现。

2. mybatis-plus 实现乐观锁（通过 version 机制）实现思路：
   　　

   - Step1：取出记录时，获取当前version
   - Step2：更新时，带上这个version
   - Step3：执行更新时，set version = new Version where version = oldVersion
     
   - Step4：如果version不对，就更新失败

3. mybatis-plus 代码实现乐观锁

   Step1：配置乐观锁插件。编写一个配置类（可以与上例的分页插件共用一个配置类），将 OptimisticLockerInterceptor 通过 @Bean 交给 Spring 管理。

   ```java
   /**
    * 乐观锁插件
    * @return 乐观锁插件的实例
    */
   @Bean
   public OptimisticLockerInterceptor optimisticLockerInterceptor() {
       return new OptimisticLockerInterceptor();
   }
   ```

   Step2：定义一个数据库字段 version。

   ```sql
   CREATE TABLE test_mybatis_plus_user
   (
       id BIGINT NOT NULL COMMENT '主键ID',
       name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
       age INT(11) NULL DEFAULT NULL COMMENT '年龄',
       email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
       create_time timestamp NULL DEFAULT NULL COMMENT '创建时间',
       update_time timestamp NULL DEFAULT NULL COMMENT '最后修改时间', 
       delete_flag tinyint(1) NULL DEFAULT NULL COMMENT '逻辑删除（0 未删除、1 删除）',
       version int NULL DEFAULT NULL COMMENT '版本号（用于乐观锁， 默认为 1）',
       PRIMARY KEY (id)
   );
   ```

   Step3：使用 @Version 注解标注对应的实体类。可以通过 @TableField 进行数据自动填充。

   ```java
   /**
    * 版本号（用于乐观锁， 默认为 1）
    */
   @Version
   @TableField(fill = FieldFill.INSERT)
   private Integer version;
   
   
   @Override
   public void insertFill(MetaObject metaObject) {
       this.strictInsertFill(metaObject, "version", Integer.class, 1);
   }
   ```

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727181316.png)

   Step4：简单测试一下，可以看一下 控制台 打印的 sql 语句。

   ```java
   @Test
   public void testVersion() {
       User user = new User();
       user.setName("tom").setAge(20).setEmail("tom@163.com");
       userService.save(user);
       userService.list().forEach(System.out::println);
       user.setName("jarry");
       userService.update(user, null);
       userService.list().forEach(System.out::println);
   }
   ```

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727181435.png)

### 3.Mybatis-Plus CRUD 操作

#### 3.1 Mapper 接口方法（CRUD）

1. 使用代码生成器生成的 mapper 接口中，其继承了 BaseMapper 接口。
   而 BaseMapper 接口中封装了一系列 CRUD 常用操作，可以直接使用，而不用自定义 xml 与 sql 语句进行 CRUD 操作（当然根据实际开发需要，自定义 sql 还是有必要的）。

   此处简单介绍一下 BaseMapper 接口中的常用方法。

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727181720.png)

2. 方法介绍

   ```
   【添加数据：（增）】
       int insert(T entity);              // 插入一条记录
   注：
       T         表示任意实体类型
       entity    表示实体对象
   
   【删除数据：（删）】
       int deleteById(Serializable id);    // 根据主键 ID 删除
       int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);  // 根据 map 定义字段的条件删除
       int delete(@Param(Constants.WRAPPER) Wrapper<T> wrapper); // 根据实体类定义的 条件删除对象
       int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList); // 进行批量删除
   注：
       id        表示 主键 ID
       columnMap 表示表字段的 map 对象
       wrapper   表示实体对象封装操作类，可以为 null。
       idList    表示 主键 ID 集合（列表、数组），不能为 null 或 empty
   
   【修改数据：（改）】
       int updateById(@Param(Constants.ENTITY) T entity); // 根据 ID 修改实体对象。
       int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T> updateWrapper); // 根据 updateWrapper 条件修改实体对象
   注：
       update 中的 entity 为 set 条件，可以为 null。
       updateWrapper 表示实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句）
   
   【查询数据：（查）】
       T selectById(Serializable id); // 根据 主键 ID 查询数据
       List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList); // 进行批量查询
       List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap); // 根据表字段条件查询
       T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 根据实体类封装对象 查询一条记录
       Integer selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 查询记录的总条数
       List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 查询所有记录（返回 entity 集合）
       List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 查询所有记录（返回 map 集合）
       List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 查询所有记录（但只保存第一个字段的值）
       <E extends IPage<T>> E selectPage(E page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 查询所有记录（返回 entity 集合），分页
       <E extends IPage<Map<String, Object>>> E selectMapsPage(E page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper); // 查询所有记录（返回 map 集合），分页
   注：
       queryWrapper 表示实体对象封装操作类（可以为 null）
       page 表示分页查询条件
   ```

#### 3.2 Service 接口方法（CRUD）

1. 使用 代码生成器 生成的 service 接口中，其继承了 IService 接口。
   IService 内部进一步封装了 BaseMapper 接口的方法（当然也提供了更详细的方法）。
   使用时，可以通过 生成的 mapper 类进行 CRUD 操作，也可以通过 生成的 service 的实现类进行 CRUD 操作。

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727182008.png)

2. 内部封装了 BaseMapper 的方法，也提供了新的方法。

   ```
   【添加数据：（增）】
       default boolean save(T entity); // 调用 BaseMapper 的 insert 方法，用于添加一条数据。
       boolean saveBatch(Collection<T> entityList, int batchSize); // 批量插入数据
   注：
       entityList 表示实体对象集合 
       batchSize 表示一次批量插入的数据量，默认为 1000
   
   【添加或修改数据：（增或改）】
       boolean saveOrUpdate(T entity); // id 若存在，则修改， id 不存在则新增数据
      default boolean saveOrUpdate(T entity, Wrapper<T> updateWrapper); // 先根据条件尝试更新，然后再执行 saveOrUpdate 操作
      boolean saveOrUpdateBatch(Collection<T> entityList, int batchSize); // 批量插入并修改数据 
   
   【删除数据：（删）】
       default boolean removeById(Serializable id); // 调用 BaseMapper 的 deleteById 方法，根据 id 删除数据。
       default boolean removeByMap(Map<String, Object> columnMap); // 调用 BaseMapper 的 deleteByMap 方法，根据 map 定义字段的条件删除
       default boolean remove(Wrapper<T> queryWrapper); // 调用 BaseMapper 的 delete 方法，根据实体类定义的 条件删除对象。
       default boolean removeByIds(Collection<? extends Serializable> idList); // 用 BaseMapper 的 deleteBatchIds 方法, 进行批量删除。
       
   【修改数据：（改）】
       default boolean updateById(T entity); // 调用 BaseMapper 的 updateById 方法，根据 ID 选择修改。
       default boolean update(T entity, Wrapper<T> updateWrapper); // 调用 BaseMapper 的 update 方法，根据 updateWrapper 条件修改实体对象。
       boolean updateBatchById(Collection<T> entityList, int batchSize); // 批量更新数据
   
   【查找数据：（查）】
       default T getById(Serializable id); // 调用 BaseMapper 的 selectById 方法，根据 主键 ID 返回数据。
       default List<T> listByIds(Collection<? extends Serializable> idList); // 调用 BaseMapper 的 selectBatchIds 方法，批量查询数据。
       default List<T> listByMap(Map<String, Object> columnMap); // 调用 BaseMapper 的 selectByMap 方法，根据表字段条件查询
       default T getOne(Wrapper<T> queryWrapper); // 返回一条记录（实体类保存）。
       Map<String, Object> getMap(Wrapper<T> queryWrapper); // 返回一条记录（map 保存）。
       default int count(Wrapper<T> queryWrapper); // 根据条件返回 记录数。
       default List<T> list(); // 返回所有数据。
       default List<T> list(Wrapper<T> queryWrapper); // 调用 BaseMapper 的 selectList 方法，查询所有记录（返回 entity 集合）。
       default List<Map<String, Object>> listMaps(Wrapper<T> queryWrapper); // 调用 BaseMapper 的 selectMaps 方法，查询所有记录（返回 map 集合）。
       default List<Object> listObjs(); // 返回全部记录，但只返回第一个字段的值。
       default <E extends IPage<T>> E page(E page, Wrapper<T> queryWrapper); // 调用 BaseMapper 的 selectPage 方法，分页查询
       default <E extends IPage<Map<String, Object>>> E pageMaps(E page, Wrapper<T> queryWrapper); // 调用 BaseMapper 的 selectMapsPage 方法，分页查询
   注：
       get 用于返回一条记录。
       list 用于返回多条记录。
       count 用于返回记录总数。
       page 用于分页查询。
       
   【链式调用：】
       default QueryChainWrapper<T> query(); // 普通链式查询
       default LambdaQueryChainWrapper<T> lambdaQuery(); // 支持 Lambda 表达式的修改
       default UpdateChainWrapper<T> update(); // 普通链式修改
       default LambdaUpdateChainWrapper<T> lambdaUpdate(); // 支持 Lambda 表达式的修改
   注：
       query 表示查询
       update 表示修改
       Lambda 表示内部支持 Lambda 写法。
   形如：
       query().eq("column", value).one();
       lambdaQuery().eq(Entity::getId, value).list();
       update().eq("column", value).remove();
       lambdaUpdate().eq(Entity::getId, value).update(entity);
   ```

#### 3.3 条件构造器（Wrapper，定义 where 条件）

1. 上面介绍的 接口方法的参数中，会出现各种 wrapper，比如 queryWrapper、updateWrapper 等。wrapper 的作用就是用于定义各种各样的查询条件（where）。

   ```
   Wrapper  条件构造抽象类
       -- AbstractWrapper 查询条件封装，用于生成 sql 中的 where 语句。
           -- QueryWrapper Entity 对象封装操作类，用于查询。
           -- UpdateWrapper Update 条件封装操作类，用于更新。
       -- AbstractLambdaWrapper 使用 Lambda 表达式封装 wrapper
           -- LambdaQueryWrapper 使用 Lambda 语法封装条件，用于查询。
           -- LambdaUpdateWrapper 使用 Lambda 语法封装条件，用于更新。
   ```

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727182500.png)

2. 常用条件

   ```
   【通用条件：】
   【比较大小： ( =, <>, >, >=, <, <= )】
       eq(R column, Object val); // 等价于 =，例: eq("name", "老王") ---> name = '老王'
       ne(R column, Object val); // 等价于 <>，例: ne("name", "老王") ---> name <> '老王'
       gt(R column, Object val); // 等价于 >，例: gt("name", "老王") ---> name > '老王'
       ge(R column, Object val); // 等价于 >=，例: ge("name", "老王") ---> name >= '老王'
       lt(R column, Object val); // 等价于 <，例: lt("name", "老王") ---> name < '老王'
       le(R column, Object val); // 等价于 <=，例: le("name", "老王") ---> name <= '老王'
       
   【范围：（between、not between、in、not in）】
      between(R column, Object val1, Object val2); // 等价于 between a and b, 例： between("age", 18, 30) ---> age between 18 and 30
      notBetween(R column, Object val1, Object val2); // 等价于 not between a and b, 例： notBetween("age", 18, 30) ---> age not between 18 and 30
      in(R column, Object... values); // 等价于 字段 IN (v0, v1, ...),例: in("age",{1,2,3}) ---> age in (1,2,3)
      notIn(R column, Object... values); // 等价于 字段 NOT IN (v0, v1, ...), 例: notIn("age",{1,2,3}) ---> age not in (1,2,3)
      inSql(R column, Object... values); // 等价于 字段 IN (sql 语句), 例: inSql("id", "select id from table where id < 3") ---> id in (select id from table where id < 3)
      notInSql(R column, Object... values); // 等价于 字段 NOT IN (sql 语句)
      
   【模糊匹配：（like）】
       like(R column, Object val); // 等价于 LIKE '%值%'，例: like("name", "王") ---> name like '%王%'
       notLike(R column, Object val); // 等价于 NOT LIKE '%值%'，例: notLike("name", "王") ---> name not like '%王%'
       likeLeft(R column, Object val); // 等价于 LIKE '%值'，例: likeLeft("name", "王") ---> name like '%王'
       likeRight(R column, Object val); // 等价于 LIKE '值%'，例: likeRight("name", "王") ---> name like '王%'
       
   【空值比较：（isNull、isNotNull）】
       isNull(R column); // 等价于 IS NULL，例: isNull("name") ---> name is null
       isNotNull(R column); // 等价于 IS NOT NULL，例: isNotNull("name") ---> name is not null
   
   【分组、排序：（group、having、order）】
       groupBy(R... columns); // 等价于 GROUP BY 字段, ...， 例: groupBy("id", "name") ---> group by id,name
       orderByAsc(R... columns); // 等价于 ORDER BY 字段, ... ASC， 例: orderByAsc("id", "name") ---> order by id ASC,name ASC
       orderByDesc(R... columns); // 等价于 ORDER BY 字段, ... DESC， 例: orderByDesc("id", "name") ---> order by id DESC,name DESC
       having(String sqlHaving, Object... params); // 等价于 HAVING ( sql语句 )， 例: having("sum(age) > {0}", 11) ---> having sum(age) > 11
   
   【拼接、嵌套 sql：（or、and、nested、apply）】
      or(); // 等价于 a or b， 例：eq("id",1).or().eq("name","老王") ---> id = 1 or name = '老王'
      or(Consumer<Param> consumer); // 等价于 or(a or/and b)，or 嵌套。例: or(i -> i.eq("name", "李白").ne("status", "活着")) ---> or (name = '李白' and status <> '活着')
      and(Consumer<Param> consumer); // 等价于 and(a or/and b)，and 嵌套。例: and(i -> i.eq("name", "李白").ne("status", "活着")) ---> and (name = '李白' and status <> '活着')
      nested(Consumer<Param> consumer); // 等价于 (a or/and b)，普通嵌套。例: nested(i -> i.eq("name", "李白").ne("status", "活着")) ---> (name = '李白' and status <> '活着')
      apply(String applySql, Object... params); // 拼接sql（若不使用 params 参数，可能存在 sql 注入），例: apply("date_format(dateColumn,'%Y-%m-%d') = {0}", "2008-08-08") ---> date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")
      last(String lastSql); // 无视优化规则直接拼接到 sql 的最后，可能存若在 sql 注入。
      exists(String existsSql); // 拼接 exists 语句。例: exists("select id from table where age = 1") ---> exists (select id from table where age = 1)
      
   【QueryWrapper 条件：】
       select(String... sqlSelect); // 用于定义需要返回的字段。例： select("id", "name", "age") ---> select id, name, age
       select(Predicate<TableFieldInfo> predicate); // Lambda 表达式，过滤需要的字段。
       lambda(); // 返回一个 LambdaQueryWrapper
       
   【UpdateWrapper 条件：】
       set(String column, Object val); // 用于设置 set 字段值。例: set("name", null) ---> set name = null
       etSql(String sql); // 用于设置 set 字段值。例: setSql("name = '老李头'") ---> set name = '老李头'
       lambda(); // 返回一个 LambdaUpdateWrapper
   ```

3. 简单使用，测试一下 QueryWrapper 

   ```java
   @Test
   public void testQueryWrapper() {
       // Step1：创建一个 QueryWrapper 对象
       QueryWrapper<User> queryWrapper = new QueryWrapper<>();
   
       // Step2： 构造查询条件
       queryWrapper
               .select("id", "name", "age")
               .eq("age", 20)
               .like("name", "j");
   
       // Step3：执行查询
       userService
               .list(queryWrapper)
               .forEach(System.out::println);
   }
   ```

   当前数据库数据如下：

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727182922.png)

   执行测试方法输出如下：

   ![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200727182954.png)

   

# 高级

## 1、工作原理

1. 读取核心配置文件并返回`InputStream`流对象。
2. 根据`InputStream`流对象解析出`Configuration`对象，生成SqlSessionFactoryBuilder对象，然后创建`SqlSessionFactory`工厂对象
3. 根据一系列属性从`SqlSessionFactory`工厂中创建`SqlSession`
4. 从`SqlSession`中调用`Executor`执行数据库操作&&生成具体SQL指令
5. 对执行结果进行二次封装
6. 提交与事务

## 2、执行sql流程

1. 由Spring管理SqlSession的创建、关闭、提交、回滚，创建**SqlSessionTemplate**和**DefaultSqlSession**

   ![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/12208532-5bd3621917679471.png)

2. **获取SQL**：即从Mapper文件中读取类似<select>等标签再结合Mapper文件中的**namespace**以及**标签中的id**可以唯一定位到**DAO文件的接口**，配合反射生成Mapperstatement对象

3. **判断走缓存还是数据库**：由**Cachingexecutor**根据缓存级别以及sql语句来判断是否走缓存，否则就委托给其他executor走数据库进行查询【缓存本质上是一个Map，并且如果涉及到相关sql数据更新，则会清空缓存】

4. **管理连接**：由数据库连接池打开相关连接对象

5. **解析执行**：解析相关参数并且由相关的Handler执行

## 3、Executor执行器及区别

```java
public interface Executor {

  ResultHandler NO_RESULT_HANDLER = null;

  // 执行update，delete，insert三种类型的sql语句
  int update(MappedStatement ms, Object parameter) throws SQLException;

  // 执行select类型的SQL语句，返回值分为结果对象列表和游标对象
  <E> List<E> query(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, CacheKey cacheKey, BoundSql boundSql) throws SQLException;
  <E> List<E> query(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler) throws SQLException;
  <E> Cursor<E> queryCursor(MappedStatement ms, Object parameter, RowBounds rowBounds) throws SQLException;

  // 批量执行SQL语句
  List<BatchResult> flushStatements() throws SQLException;

  // 提交事务
  void commit(boolean required) throws SQLException;

  // 事务回滚
  void rollback(boolean required) throws SQLException;

  // 创建缓存中用到的CacheKey对象
  CacheKey createCacheKey(MappedStatement ms, Object parameterObject, RowBounds rowBounds, BoundSql boundSql);

  boolean isCached(MappedStatement ms, CacheKey key);

  // 清空一级缓存
  void clearLocalCache();

  // 延迟加载一级缓存中的数据
  void deferLoad(MappedStatement ms, MetaObject resultObject, String property, CacheKey key, Class<?> targetType);

  // 获取事务对象
  Transaction getTransaction();

  // 关闭Executor对象
  void close(boolean forceRollback);

  // 检测Executor对象是否关闭
  boolean isClosed();

  void setExecutorWrapper(Executor executor);

}
```

![190628_1H4d_3737136.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/190628_1H4d_3737136.png)

1. SimpleExecutor：每执行一次update或select，就开启一个Statement对象，用完立刻关闭Statement对象。
2. ReuseExecutor：执行update或select，以sql作为key查找Statement对象，存在就使用，不存在就创建，用完后，不关闭Statement对象，而是放置于Map内，供下一次使用。简言之，就是重复使用Statement对象。
3. BatchExecutor：执行update（没有select，JDBC批处理不支持select），将所有sql都添加到批处理中（addBatch()），等待统一执行（executeBatch()），它缓存了多个Statement对象，每个Statement对象都是addBatch()完毕后，等待逐一执行executeBatch()批处理。与JDBC批处理相同。
4. **缓存执行器CachingExecutor**：装饰设计模式典范，先从缓存中获取查询结果，存在就返回，不存在，再委托给Executor delegate去数据库取，delegate可以是上面任一的SimpleExecutor、ReuseExecutor、BatchExecutor。

作用范围：Executor的这些特点，都严格限制在SqlSession生命周期范围内。默认是**SimpleExcutor**，需要配置在创建SqlSession对象的时候指定执行器的类型即可。

## 4、缓存

MyBatis 提供了查询缓存来缓存数据，以提高查询的性能。MyBatis 的缓存分为`一级缓存`和`二级缓存`。涉及到DML语句的时候并且执行commit操作，则缓存会清空

- 一级缓存是 SqlSession 级别的缓存
- 二级缓存是 mapper 级别的缓存，多个 SqlSession 共享

注意：MyBatis的缓存机制是基于id进行缓存，也就是说MyBatis在使用HashMap缓存数据时，是使用对象的id作为key，而对象作为value保存

