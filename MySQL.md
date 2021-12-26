# MySQL优化

## MySQL 逻辑架构

大体来说，MySQL 可以分为 Server 层和存储引擎层两部分。

<img src="https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211225203024945.png" alt="image-20211225203024945" style="zoom: 67%;" />

Server 层包括连接器、查询缓存、分析器、优化器、执行器等，涵盖 MySQL 的大多数核心服务功能，以及所有的内置函数（如日期、时间、数学和加密函数等），所有跨存储引擎的功能都在这一层实现，比如存储过程、触发器、视图等。

而存储引擎层负责数据的存储和提取。其架构模式是插件式的，支持 InnoDB、MyISAM、Memory 等多个存储引擎。现在最常用的存储引擎是 InnoDB，它从 MySQL 5.5.5 版本开始成为了默认存储引擎。

以一条SQL执行流程来分析整个执行过程。

### 连接器

连接器负责跟客户端建立连接、获取权限、维持和管理连接。连接命令一般是这么写的：

```shell
mysql -h$ip -P$port -u$user -p
```

输完命令后，需在交互对话里输入密码。虽然密码也可直接跟在 -p 后面，但可能会导致密码泄露。强烈建议在生产服务器不要这么做。

连接命令中的 mysql 是客户端工具，用来跟服务端建立连接。在完成经典的 TCP 握手后，连接器就要开始认证你的身份，这个时候用的就是你输入的用户名和密码。

- 如果用户名或密码不对，你就会收到一个"Access denied for user"的错误，然后客户端程序结束执行。
- 如果用户名密码认证通过，连接器会到权限表里面查出你拥有的权限。之后，这个连接里面的权限判断逻辑，都将依赖于此时读到的权限。

可以在 show processlist 命令中看到连接进程，“Sleep”标识空闲连接。

![img](data:image/png;base64,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)

客户端长时间没动静，连接器就会自动将它断开，断开后需重连。时间是由参数 wait_timeout 控制的，默认值是 8 小时。

数据库里，长连接指连接成功后，如果客户端持续有请求，则一直使用同一个连接。短连接则是指每次执行完很少的几次查询就断开连接，下次查询再重新建立一个。

但 MySQL 在执行过程中临时使用的内存是管理在连接对象里面的。这些资源会在连接断开的时候才释放。所以如果长连接累积下来，可能导致内存占用太大，被系统强行杀掉（OOM），从现象看就是 MySQL 异常重启了。

怎么解决这个问题呢？你可以考虑以下两种方案。

1. 定期断开长连接。使用一段时间，或者程序里面判断执行过一个占用内存的大查询后，断开连接，之后要查询再重连。
2. 如果你用的是 MySQL 5.7 或更新版本，可以在每次执行一个比较大的操作后，通过执行 mysql_reset_connection 来重新初始化连接资源。这个过程不需要重连和重新做权限验证，但是会将连接恢复到刚刚创建完时的状态。

## 查询缓存

连接建立完成后，你就可以执行 select 语句了。

MySQL 拿到一个查询请求后，会先到查询缓存看看，之前是不是执行过这条语句。之前执行过的语句及其结果可能会以 key-value 对的形式，被直接缓存在内存中。key 是查询的语句，value 是查询的结果。如果你的查询能够直接在这个缓存中找到 key，那么这个 value 就会被直接返回给客户端。

如果语句不在查询缓存中，就会继续后面的执行阶段。执行完成后，执行结果会被存入查询缓存中。你可以看到，如果查询命中缓存，MySQL 不需要执行后面的复杂操作，就可以直接返回结果，这个效率会很高。

**但是大多数情况下我会建议你不要使用查询缓存，为什么呢？因为查询缓存往往弊大于利。**

查询缓存的失效非常频繁，只要有对一个表的更新，这个表上所有的查询缓存都会被清空。因此很可能你费劲地把结果存起来，还没使用呢，就被一个更新全清空了。对于更新压力大的数据库来说，查询缓存的命中率会非常低。除非你的业务就是有一张静态表，很长时间才会更新一次。比如，一个系统配置表，那这张表上的查询才适合使用查询缓存。

好在 MySQL 也提供了这种“按需使用”的方式。你可以将参数 query_cache_type 设置成 DEMAND，这样对于默认的 SQL 语句都不使用查询缓存。而对于你确定要使用查询缓存的语句，可以用 SQL_CACHE 显式指定，像下面这个语句一样：

```
mysql> select SQL_CACHE * from T where ID=10；
复制代码
```

需要注意的是，MySQL 8.0 版本直接将查询缓存的整块功能删掉了，也就是说 8.0 开始彻底没有这个功能了。

# 分析器

如果没有命中查询缓存，就要开始真正执行语句了。首先，MySQL 需要知道你要做什么，因此需要对 SQL 语句做解析。

分析器先会做“词法分析”。你输入的是由多个字符串和空格组成的一条 SQL 语句，MySQL 需要识别出里面的字符串分别是什么，代表什么。

MySQL 从你输入的"select"这个关键字识别出来，这是一个查询语句。它也要把字符串“T”识别成“表名 T”，把字符串“ID”识别成“列 ID”。

做完了这些识别以后，就要做“语法分析”。根据词法分析的结果，语法分析器会根据语法规则，判断你输入的这个 SQL 语句是否满足 MySQL 语法。

如果你的语句不对，就会收到“You have an error in your SQL syntax”的错误提醒，比如下面这个语句 select 少打了开头的字母“s”。

```
mysql> elect * from t where ID=1;
 
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'elect * from t where ID=1' at line 1
```

一般语法错误会提示第一个出现错误的位置，所以你要关注的是紧接“use near”的内容。

# 优化器

经过了分析器，MySQL 就知道你要做什么了。在开始执行之前，还要先经过优化器的处理。

优化器是在表里面有多个索引的时候，决定使用哪个索引；或者在一个语句有多表关联（join）的时候，决定各个表的连接顺序。比如你执行下面这样的语句，这个语句是执行两个表的 join：

```
mysql> select * from t1 join t2 using(ID)  where t1.c=10 and t2.d=20;
复制代码
```

- 既可以先从表 t1 里面取出 c=10 的记录的 ID 值，再根据 ID 值关联到表 t2，再判断 t2 里面 d 的值是否等于 20。
- 也可以先从表 t2 里面取出 d=20 的记录的 ID 值，再根据 ID 值关联到 t1，再判断 t1 里面 c 的值是否等于 10。

这两种执行方法的逻辑结果是一样的，但是执行的效率会有不同，而优化器的作用就是决定选择使用哪一个方案。

优化器阶段完成后，这个语句的执行方案就确定下来了，然后进入执行器阶段。如果你还有一些疑问，比如优化器是怎么选择索引的，有没有可能选择错等等，没关系，我会在后面的文章中单独展开说明优化器的内容。

# 执行器

MySQL 通过分析器知道了你要做什么，通过优化器知道了该怎么做，于是就进入了执行器阶段，开始执行语句。

开始执行的时候，要先判断一下你对这个表 T 有没有执行查询的权限，如果没有，就会返回没有权限的错误，如下所示 (在工程实现上，如果命中查询缓存，会在查询缓存返回结果的时候，做权限验证。查询也会在优化器之前调用 precheck 验证权限)。

```
mysql> select * from T where ID=10;
 
ERROR 1142 (42000): SELECT command denied to user 'b'@'localhost' for table 'T'
```

如果有权限，就打开表继续执行。打开表的时候，执行器就会根据表的引擎定义，去使用这个引擎提供的接口。

比如我们这个例子中的表 T 中，ID 字段没有索引，那么执行器的执行流程是这样的：

1. 调用 InnoDB 引擎接口取这个表的第一行，判断 ID 值是不是 10，如果不是则跳过，如果是则将这行存在结果集中；
2. 调用引擎接口取“下一行”，重复相同的判断逻辑，直到取到这个表的最后一行。
3. 执行器将上述遍历过程中所有满足条件的行组成的记录集作为结果集返回给客户端。

至此，这个语句就执行完成了。

对于有索引的表，执行的逻辑也差不多。第一次调用的是“取满足条件的第一行”这个接口，之后循环取“满足条件的下一行”这个接口，这些接口都是引擎中已经定义好的。

你会在数据库的慢查询日志中看到一个 rows_examined 的字段，表示这个语句执行过程中扫描了多少行。这个值就是在执行器每次调用引擎获取数据行的时候累加的。

在有些场景下，执行器调用一次，在引擎内部则扫描了多行，因此**引擎扫描行数跟 rows_examined 并不是完全相同的。**我们后面会专门有一篇文章来讲存储引擎的内部机制，里面会有详细的说明。

1、存储过程

2、循环结构

3、变量

4、函数

5、事务和视图

6、explain各字段

7、索引优化

单表优化：建索引、覆盖索引等

多表优化：左连接加索引在右表，右连接加索引在左表，理解数据量不同【小表驱动大表】
