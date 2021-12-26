# 1、工作原理

1. 读取核心配置文件并返回`InputStream`流对象。
2. 根据`InputStream`流对象解析出`Configuration`对象，生成SqlSessionFactoryBuilder对象，然后创建`SqlSessionFactory`工厂对象
3. 根据一系列属性从`SqlSessionFactory`工厂中创建`SqlSession`
4. 从`SqlSession`中调用`Executor`执行数据库操作&&生成具体SQL指令
5. 对执行结果进行二次封装
6. 提交与事务

# 2、执行sql流程

1. 由Spring管理SqlSession的创建、关闭、提交、回滚，创建**SqlSessionTemplate**和**DefaultSqlSession**

   ![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/12208532-5bd3621917679471.png)

2. **获取SQL**：即从Mapper文件中读取类似<select>等标签再结合Mapper文件中的**namespace**以及**标签中的id**可以唯一定位到**DAO文件的接口**，配合反射生成Mapperstatement对象

3. **判断走缓存还是数据库**：由**Cachingexecutor**根据缓存级别以及sql语句来判断是否走缓存，否则就委托给其他executor走数据库进行查询【缓存本质上是一个Map，并且如果涉及到相关sql数据更新，则会清空缓存】

4. **管理连接**：由数据库连接池打开相关连接对象

5. **解析执行**：解析相关参数并且由相关的Handler执行

# 3、Executor执行器及区别

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

# 4、缓存

MyBatis 提供了查询缓存来缓存数据，以提高查询的性能。MyBatis 的缓存分为`一级缓存`和`二级缓存`。涉及到DML语句的时候并且执行commit操作，则缓存会清空

- 一级缓存是 SqlSession 级别的缓存
- 二级缓存是 mapper 级别的缓存，多个 SqlSession 共享

注意：MyBatis的缓存机制是基于id进行缓存，也就是说MyBatis在使用HashMap缓存数据时，是使用对象的id作为key，而对象作为value保存

