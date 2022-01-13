# JDBC

## 简介

JDBC代表Java数据库连接，用于Java编程语言和数据库之间的数据库无关连接的标准Java API。

JDBC库包括通常与数据库使用相关，如下面提到的每个任务的API -

- 连接到数据库
- 创建SQL或MySQL语句
- 在数据库中执行SQL或MySQL查询
- 查看和修改结果记录

**JDBC架构**

JDBC API支持用于数据库访问的两层和三层处理模型，但通常，JDBC体系结构由两层组成：

- JDBC API：提供应用程序到JDBC管理器连接。
- JDBC驱动程序API：支持JDBC管理器到驱动程序连接。

JDBC API使用驱动程序管理器并指定数据库的驱动程序来提供与异构数据库的透明连接。

JDBC驱动程序管理器确保使用正确的驱动程序来访问每个数据源。 驱动程序管理器能够支持连接到多个异构数据库的多个并发驱动程序。

**常见组件**

JDBC API提供以下接口和类 -

- `DriverManager`：此类管理数据库驱动程序列表。 使用通信子协议将来自java应用程序的连接请求与适当的数据库驱动程序进行匹配。在JDBC下识别某个子协议的第一个驱动程序将用于建立数据库连接。
- `Driver`：此接口处理与数据库服务器的通信。我们很少会直接与`Driver`对象进行交互。 但会使用`DriverManager`对象来管理这种类型的对象。 它还提取与使用`Driver`对象相关的信息。
- `Connection`：此接口具有用于联系数据库的所有方法。 连接(`Connection`)对象表示通信上下文，即，与数据库的所有通信仅通过连接对象。
- `Statement`：使用从此接口创建的对象将SQL语句提交到数据库。 除了执行存储过程之外，一些派生接口还接受参数。
- `ResultSet`：在使用`Statement`对象执行SQL查询后，这些对象保存从数据库检索的数据。 它作为一个迭代器并可移动`ResultSet`对象查询的数据。
- `SQLException`：此类处理数据库应用程序中发生的任何错误。

**JDBC包**

`java.sql`和`javax.sql`是JDBC 4.0的主要包。这是编写本教程时最新的JDBC版本。 它提供了与数据源进行交互的主要类。

这些包中的新功能包括以下更改(增强) -

- 自动数据库驱动程序加载
- 异常处理改进
- 增强的`BLOB/CLOB`功能
- `Connection` 和 `Statement`接口的增强
- 国家字符集支持
- SQL ROWID访问
- SQL 2003 XML数据类型的支持
- 注解支持

## 使用

步骤：
		

1. 导入驱动jar包，到项目的libs目录下，并添加为依赖	
2. 注册驱动，获取数据库连接对象 Connection
3. 定义sql，获取执行sql语句的对象 Statement
4. 执行sql，接受返回结果，处理结果

```java
        //1. 导入驱动jar包
        //2.注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //3.获取数据库连接对象
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db3", "root", "root");
        //4.定义sql语句
        String sql = "update account set balance = 500 where id = 1";
        //5.获取执行sql的对象 Statement
        Statement stmt = conn.createStatement();
        //6.执行sql
        int count = stmt.executeUpdate(sql);
        //7.处理结果
        System.out.println(count);
        //8.释放资源
        stmt.close();
        conn.close();
```

**各对象作用**

1. DriverManager：驱动管理对象
	* 功能：
		1. 注册驱动：告诉程序该使用哪一个数据库驱动jar
		
		2. static void registerDriver(Driver driver) :注册与给定的驱动程序 DriverManager 
		
		3. 写代码使用：  
		
		  ```java
		  Class.forName("com.mysql.jdbc.Driver");
		  通过查看源码发现：在com.mysql.jdbc.Driver类中存在静态代码块
		   static {
		          try {
		              java.sql.DriverManager.registerDriver(new Driver());
		          } catch (SQLException E) {
		              throw new RuntimeException("Can't register driver!");
		          }
		  	}
		  //注意：mysql5之后的驱动jar包可以省略注册驱动的步骤。
		  ```
		
		  获取数据库连接：
		
		* 方法：static Connection getConnection(String url, String user, String password) 
		* 参数：
			* url：指定连接的路径
				* 语法：jdbc:mysql://ip地址(域名):端口号/数据库名称
				* 例子：jdbc:mysql://localhost:3306/db3
				* 细节：如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql:///数据库名称
			* user：用户名
			* password：密码 
	
2. Connection：数据库连接对象
	
	- 功能：
	
	  1. 获取执行sql 的对象
	  	* Statement createStatement()
	  	* PreparedStatement prepareStatement(String sql)  
	
	  2. 管理事务：
	  	* 开启事务：setAutoCommit(boolean autoCommit) ：调用该方法设置参数为false，即开启事务
	  	  * 提交事务：commit() 
	  	  * 回滚事务：rollback() 
	
3. Statement：执行sql的对象
	1. 执行sql
		1. boolean execute(String sql) ：可以执行任意的sql
		2. int executeUpdate(String sql) ：执行DML（insert、update、delete）语句、DDL(create，alter、drop)语句
			* 返回值：影响的行数，可以通过这个影响的行数判断DML语句是否执行成功，返回值>0的则执行成功，反之，则失败。
		3. ResultSet executeQuery(String sql)  ：执行DQL（select)语句
		3. 示例：
		
		```java
						Statement stmt = null;
				        Connection conn = null;
				        try {
				            //1. 注册驱动
				            Class.forName("com.mysql.jdbc.Driver");
				            //2. 定义sql
				            String sql = "insert into account values(null,'王五',3000)";
				            //3.获取Connection对象
				            conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");
				            //4.获取执行sql的对象 Statement
				            stmt = conn.createStatement();
				            //5.执行sql
				            int count = stmt.executeUpdate(sql);//影响的行数
				            //6.处理结果
				            System.out.println(count);
				            if(count > 0){
				                System.out.println("添加成功！");
				            }else{
				                System.out.println("添加失败！");
				            }
				
				        } catch (ClassNotFoundException e) {
				            e.printStackTrace();
				        } catch (SQLException e) {
				            e.printStackTrace();
				        }finally {
				            //stmt.close();
				            //7. 释放资源
				            //避免空指针异常
				            if(stmt != null){
				                try {
				                    stmt.close();
				                } catch (SQLException e) {
				                    e.printStackTrace();
				                }
				            }
				
				            if(conn != null){
				                try {
				                    conn.close();
				                } catch (SQLException e) {
				                    e.printStackTrace();
				                }
				            }
				        }
		}
		```
	
4. ResultSet：结果集对象，封装查询结果
	* boolean next(): 游标向下移动一行，判断当前行是否是最后一行末尾(是否有数据)，如果是，则返回false，如果不是则返回true
	
	* getXxx(参数):获取数据
		* Xxx：代表数据类型   如： int getInt() ,	String getString()
		* 参数：
			1. int：代表列的编号,从1开始   如： getString(1)
			2. String：代表列名称。 如： getDouble("balance")
	
	* 注意：
		* 使用步骤：
			1. 游标向下移动一行
			2. 判断是否有数据
			3. 获取数据
   	  
           ```java
              //循环判断游标是否是最后一行末尾。
           	            while(rs.next()){
           	                //获取数据
           	                //6.2 获取数据
	        	                int id = rs.getInt(1);
           	                String name = rs.getString("name");
           	                double balance = rs.getDouble(3);
	        	
	        	                System.out.println(id + "---" + name + "---" + balance);
	        	            }
	        ```
	
5. PreparedStatement：执行sql的对象
	1. SQL注入问题：在拼接sql时，有一些sql的特殊关键字参与字符串的拼接。会造成安全性问题
		1. 输入用户随便，输入密码：a' or 'a' = 'a
		2. sql：select * from user where username = 'fhdsjkf' and password = 'a' or 'a' = 'a' 
	2. 解决sql注入问题：使用PreparedStatement对象来解决
	3. 预编译的SQL：参数使用?作为占位符
	4. 步骤：
		1. 导入驱动jar包 mysql-connector-java-5.1.37-bin.jar
		2. 注册驱动
		3. 获取数据库连接对象 Connection
		4. 定义sql
			* 注意：sql的参数使用？作为占位符。 如：select * from user where username = ? and password = ?;
		5. 获取执行sql语句的对象 PreparedStatement  Connection.prepareStatement(String sql) 
		6. 给？赋值：
			* 方法： setXxx(参数1,参数2)
				* 参数1：？的位置编号 从1 开始
				* 参数2：？的值
		7. 执行sql，接受返回结果，不需要传递sql语句
		8. 处理结果
		9. 释放资源
	5. 注意：后期都会使用PreparedStatement来完成增删改查的所有操作
		1. 可以防止SQL注入
		2. 效率更高

## JDBC Utils

目的：简化书写，具有抽取注册驱动、封装获取连接对象的方法等优点，方便用户更好地编程。

```java
	public class JDBCUtils {
    private static String url;
    private static String user;
    private static String password;
    private static String driver;
    /**
     * 文件的读取，只需要读取一次即可拿到这些值。使用静态代码块
     */
    static{
        //读取资源文件，获取值。
        try {
            //1. 创建Properties集合类。
            Properties pro = new Properties();
            //获取src路径下的文件的方式--->ClassLoader 类加载器
            ClassLoader classLoader = JDBCUtils.class.getClassLoader();
            URL res  = classLoader.getResource("jdbc.properties");
            String path = res.getPath();
            System.out.println(path);///D:/IdeaProjects/itcast/out/production/day04_jdbc/jdbc.properties
            //2. 加载文件
           // pro.load(new FileReader("D:\\IdeaProjects\\itcast\\day04_jdbc\\src\\jdbc.properties"));
            pro.load(new FileReader(path));
            //3. 获取数据，赋值
            url = pro.getProperty("url");
            user = pro.getProperty("user");
            password = pro.getProperty("password");
            driver = pro.getProperty("driver");
            //4. 注册驱动
            Class.forName(driver);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
    /**
     * 获取连接
     * @return 连接对象
     */
    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url, user, password);
    }
    /**
     * 释放资源
     * @param stmt
     * @param conn
     */
    public static void close(Statement stmt,Connection conn){
        if( stmt != null){
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if( conn != null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
    /**
     * 释放资源
     * @param stmt
     * @param conn
     */
    public static void close(ResultSet rs,Statement stmt, Connection conn){
        if( rs != null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if( stmt != null){
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if( conn != null){
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## JDBC事务

1. 事务：一个包含多个步骤的业务操作。如果这个业务操作被事务管理，则这多个步骤要么同时成功，要么同时失败。
2. 操作：
	1. 开启事务
	2. 提交事务
	3. 回滚事务
3. 使用Connection对象来管理事务
	* 开启事务：setAutoCommit(boolean autoCommit) ：调用该方法设置参数为false，即开启事务
		* 在执行sql之前开启事务
	* 提交事务：commit() 
		* 当所有sql都执行完提交事务
	* 回滚事务：rollback() 
		* 在catch中回滚事务

```java
	public class JDBCDemo10 {
	    public static void main(String[] args) {
	        Connection conn = null;
	        PreparedStatement pstmt1 = null;
	        PreparedStatement pstmt2 = null;
	
	        try {
	            //1.获取连接
	            conn = JDBCUtils.getConnection();
	            //开启事务
	            conn.setAutoCommit(false);
	
	            //2.定义sql
	            //2.1 张三 - 500
	            String sql1 = "update account set balance = balance - ? where id = ?";
	            //2.2 李四 + 500
	            String sql2 = "update account set balance = balance + ? where id = ?";
	            //3.获取执行sql对象
	            pstmt1 = conn.prepareStatement(sql1);
	            pstmt2 = conn.prepareStatement(sql2);
	            //4. 设置参数
	            pstmt1.setDouble(1,500);
	            pstmt1.setInt(2,1);
	
	            pstmt2.setDouble(1,500);
	            pstmt2.setInt(2,2);
	            //5.执行sql
	            pstmt1.executeUpdate();
	            // 手动制造异常
	            int i = 3/0;
	
	            pstmt2.executeUpdate();
	            //提交事务
	            conn.commit();
	        } catch (Exception e) {
	            //事务回滚
	            try {
	                if(conn != null) {
	                    conn.rollback();
	                }
	            } catch (SQLException e1) {
	                e1.printStackTrace();
	            }
	            e.printStackTrace();
	        }finally {
	            JDBCUtils.close(pstmt1,conn);
	            JDBCUtils.close(pstmt2,null);
	        }
	    }
	}
```

## JDBC连接池

当系统初始化好后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完之后，会将连接对象归还给容器。

**优点：节约了资源，使得用户访问更高效。**

标准接口：DataSource   javax.sql包下的

- 方法：

  * 获取连接：getConnection()

  * 归还连接：Connection.close()。如果连接对象Connection是从连接池中获取的，那么调用Connection.close()方法，则不会再关闭连接了。而是归还连接

**C3P0连接池：**

* 步骤：
	1. 导入jar包 (两个) c3p0-0.9.5.2.jar mchange-commons-java-0.2.12.jar ，
		* 不要忘记导入数据库驱动jar包
	2. 定义配置文件：
		* 名称： c3p0.properties 或者 c3p0-config.xml
		* 路径：直接将文件放在src目录下即可。
	3. 创建核心对象 数据库连接池对象 ComboPooledDataSource
	4. 获取连接： getConnection
	
	```java
			//1.创建数据库连接池对象
	        DataSource ds  = new ComboPooledDataSource();
	        //2. 获取连接对象
	        Connection conn = ds.getConnection();
	```
	
	**Druid连接池：**
	
	- 步骤：
	
	  1. 导入Druid的jar包
	     2. 定义配置文件：
	     	* 是properties形式的
	     	* 可以叫任意名称，可以放在任意目录下
	
	  2. 加载配置文件。Properties
	
	  3. 获取数据库连接池对象：通过工厂来来获取  DruidDataSourceFactory
	
	  4. 获取连接：getConnection
	
	  ```java
	  		 //3.加载配置文件
	          Properties pro = new Properties();
	          InputStream is = DruidDemo.class.getClassLoader().getResourceAsStream("druid.properties");
	          pro.load(is);
	          //4.获取连接池对象
	          DataSource ds = DruidDataSourceFactory.createDataSource(pro);
	          //5.获取连接
	          Connection conn = ds.getConnection();
	  ```
	
	  5. 定义工具类
	
	  ```java
	  		public class JDBCUtils {
	  
	  		    //1.定义成员变量 DataSource
	  		    private static DataSource ds ;
	  		
	  		    static{
	  		        try {
	  		            //1.加载配置文件
	  		            Properties pro = new Properties();
	  		            pro.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
	  		            //2.获取DataSource
	  		            ds = DruidDataSourceFactory.createDataSource(pro);
	  		        } catch (IOException e) {
	  		            e.printStackTrace();
	  		        } catch (Exception e) {
	  		            e.printStackTrace();
	  		        }
	  		    }
	  		
	  		    /**
	  		     * 获取连接
	  		     */
	  		    public static Connection getConnection() throws SQLException {
	  		        return ds.getConnection();
	  		    }
	  		
	  		    /**
	  		     * 释放资源
	  		     */
	  		    public static void close(Statement stmt,Connection conn){
	  		       close(null,stmt,conn);
	  		    }
	  		
	  		
	  		    public static void close(ResultSet rs , Statement stmt, Connection conn){
	  		
	  		
	  		        if(rs != null){
	  		            try {
	  		                rs.close();
	  		            } catch (SQLException e) {
	  		                e.printStackTrace();
	  		            }
	  		        }
	  		
	  		
	  		        if(stmt != null){
	  		            try {
	  		                stmt.close();
	  		            } catch (SQLException e) {
	  		                e.printStackTrace();
	  		            }
	  		        }
	  		
	  		        if(conn != null){
	  		            try {
	  		                conn.close();//归还连接
	  		            } catch (SQLException e) {
	  		                e.printStackTrace();
	  		            }
	  		        }
	  		    }
	  		
	  		    /**
	  		     * 获取连接池方法
	  		     */
	  		    public static DataSource getDataSource(){
	  		        return  ds;
	  		    }
	  		}
	  ```
	
## Spring JDBC

* Spring框架对JDBC的简单封装。提供了一个JDBCTemplate对象简化JDBC的开发
* 步骤：
	1. 导入jar包
	2. 创建JdbcTemplate对象。依赖于数据源DataSource
		* JdbcTemplate template = new JdbcTemplate(ds);

	3. 调用JdbcTemplate的方法来完成CRUD的操作
		* update():执行DML语句。增、删、改语句
		* queryForMap():查询结果将结果集封装为map集合，将列名作为key，将值作为value 将这条记录封装为一个map集合
			* 注意：这个方法查询的结果集长度只能是1
		* queryForList():查询结果将结果集封装为list集合
			* 注意：将每一条记录封装为一个Map集合，再将Map集合装载到List集合中
		* query():查询结果，将结果封装为JavaBean对象
			* query的参数：RowMapper
				* 一般我们使用BeanPropertyRowMapper实现类。可以完成数据到JavaBean的自动封装
				* new BeanPropertyRowMapper<类型>(类型.class)
		* queryForObject：查询结果，将结果封装为对象
			* 一般用于聚合函数的查询

​	  **示例**

```java
			import cn.itcast.domain.Emp;
			import cn.itcast.utils.JDBCUtils;
			import org.junit.Test;
			import org.springframework.jdbc.core.BeanPropertyRowMapper;
			import org.springframework.jdbc.core.JdbcTemplate;
			import org.springframework.jdbc.core.RowMapper;
			
			import java.sql.Date;
			import java.sql.ResultSet;
			import java.sql.SQLException;
			import java.util.List;
			import java.util.Map;
			
			public class JdbcTemplateDemo2 {
			
			    //Junit单元测试，可以让方法独立执行
			
			
			    //1. 获取JDBCTemplate对象
			    private JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
			    /**
			     * 1. 修改1号数据的 salary 为 10000
			     */
			    @Test
			    public void test1(){
			
			        //2. 定义sql
			        String sql = "update emp set salary = 10000 where id = 1001";
			        //3. 执行sql
			        int count = template.update(sql);
			        System.out.println(count);
			    }
			
			    /**
			     * 2. 添加一条记录
			     */
			    @Test
			    public void test2(){
			        String sql = "insert into emp(id,ename,dept_id) values(?,?,?)";
			        int count = template.update(sql, 1015, "郭靖", 10);
			        System.out.println(count);
			
			    }
			
			    /**
			     * 3.删除刚才添加的记录
			     */
			    @Test
			    public void test3(){
			        String sql = "delete from emp where id = ?";
			        int count = template.update(sql, 1015);
			        System.out.println(count);
			    }
			
			    /**
			     * 4.查询id为1001的记录，将其封装为Map集合
			     * 注意：这个方法查询的结果集长度只能是1
			     */
			    @Test
			    public void test4(){
			        String sql = "select * from emp where id = ? or id = ?";
			        Map<String, Object> map = template.queryForMap(sql, 1001,1002);
			        System.out.println(map);
			        //{id=1001, ename=孙悟空, job_id=4, mgr=1004, joindate=2000-12-17, salary=10000.00, bonus=null, dept_id=20}
			
			    }
			
			    /**
			     * 5. 查询所有记录，将其封装为List
			     */
			    @Test
			    public void test5(){
			        String sql = "select * from emp";
			        List<Map<String, Object>> list = template.queryForList(sql);
			
			        for (Map<String, Object> stringObjectMap : list) {
			            System.out.println(stringObjectMap);
			        }
			    }
			
			    /**
			     * 6. 查询所有记录，将其封装为Emp对象的List集合
			     */
			
			    @Test
			    public void test6(){
			        String sql = "select * from emp";
			        List<Emp> list = template.query(sql, new RowMapper<Emp>() {
			
			            @Override
			            public Emp mapRow(ResultSet rs, int i) throws SQLException {
			                Emp emp = new Emp();
			                int id = rs.getInt("id");
			                String ename = rs.getString("ename");
			                int job_id = rs.getInt("job_id");
			                int mgr = rs.getInt("mgr");
			                Date joindate = rs.getDate("joindate");
			                double salary = rs.getDouble("salary");
			                double bonus = rs.getDouble("bonus");
			                int dept_id = rs.getInt("dept_id");
			
			                emp.setId(id);
			                emp.setEname(ename);
			                emp.setJob_id(job_id);
			                emp.setMgr(mgr);
			                emp.setJoindate(joindate);
			                emp.setSalary(salary);
			                emp.setBonus(bonus);
			                emp.setDept_id(dept_id);
			
			                return emp;
			            }
			        });
			
			
			        for (Emp emp : list) {
			            System.out.println(emp);
			        }
			    }
			
			    /**
			     * 6. 查询所有记录，将其封装为Emp对象的List集合
			     */
			
			    @Test
			    public void test6_2(){
			        String sql = "select * from emp";
			        List<Emp> list = template.query(sql, new BeanPropertyRowMapper<Emp>(Emp.class));
			        for (Emp emp : list) {
			            System.out.println(emp);
			        }
			    }
			
			    /**
			     * 7. 查询总记录数
			     */
			
			    @Test
			    public void test7(){
			        String sql = "select count(id) from emp";
			        Long total = template.queryForObject(sql, Long.class);
			        System.out.println(total);
			    }
			
			}
```

# tomcat

* 服务器：安装了服务器软件的计算机
* 服务器软件：接收用户的请求，处理请求，做出响应
* web服务器软件：接收用户的请求，处理请求，做出响应。
	* 在web服务器软件中，可以部署web项目，让用户通过浏览器来访问这些项目
	* web容器
* 常见的java相关的web服务器软件：
	* webLogic：oracle公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的。
	* webSphere：IBM公司，大型的JavaEE服务器，支持所有的JavaEE规范，收费的。
	* JBOSS：JBOSS公司的，大型的JavaEE服务器，支持所有的JavaEE规范，收费的。
	* Tomcat：Apache基金组织，中小型的JavaEE服务器，仅仅支持少量的JavaEE规范servlet/jsp。开源的，免费的。
* JavaEE：Java语言在企业级开发中使用的技术规范的总和，一共规定了13项大的规范
* Tomcat：web服务器软件
	1. 下载：http://tomcat.apache.org/
	2. 安装：解压压缩包即可。
		* 注意：安装目录建议不要有中文和空格
	3. 卸载：删除目录就行了
	4. 启动：
		* bin/startup.bat ,双击运行该文件即可
		* 访问：浏览器输入：http://localhost:8080 回车访问自己
						  http://别人的ip:8080 访问别人
		
		* 可能遇到的问题：
			1. 黑窗口一闪而过：
				* 原因： 没有正确配置JAVA_HOME环境变量
				* 解决方案：正确配置JAVA_HOME环境变量
			2. 启动报错：
				1. 暴力：找到占用的端口号，并且找到对应的进程，杀死该进程
					* netstat -ano
				2. 温柔：修改自身的端口号
					* conf/server.xml
					* <Connector port="8888" protocol="HTTP/1.1"
		               connectionTimeout="20000"
		               redirectPort="8445" />
					* 一般会将tomcat的默认端口号修改为80。80端口号是http协议的默认端口号。
						* 好处：在访问时，就不用输入端口号
	5. 关闭：
		1. 正常关闭：
			* bin/shutdown.bat
			* ctrl+c
		2. 强制关闭：
			* 点击启动窗口的×
	6. 配置:
		* 部署项目的方式：
			1. 直接将项目放到webapps目录下即可。
				* /hello：项目的访问路径-->虚拟目录
				* 简化部署：将项目打成一个war包，再将war包放置到webapps目录下。
					* war包会自动解压缩
			2. 配置conf/server.xml文件
				在<Host>标签体中配置
				<Context docBase="D:\hello" path="/hehe" />
				* docBase:项目存放的路径
				* path：虚拟目录
			3. 在conf\Catalina\localhost创建任意名称的xml文件。在文件中编写
				<Context docBase="D:\hello" />
				* 虚拟目录：xml文件的名称
		
		* 静态项目和动态项目：
			* 目录结构
				* java动态项目的目录结构：
					-- 项目的根目录
						-- WEB-INF目录：
							-- web.xml：web项目的核心配置文件
							-- classes目录：放置字节码文件的目录
							-- lib目录：放置依赖的jar包
		* 将Tomcat集成到IDEA中，并且创建JavaEE的项目，部署项目。

# Servlet

* 概念：运行在服务器端的小程序
	* Servlet就是一个接口，定义了Java类被浏览器访问到(tomcat识别)的规则。
	* 将来我们自定义一个类，实现Servlet接口，复写方法。
	
* 快速入门：
	1. 创建JavaEE项目
	2. 定义一个类，实现Servlet接口
		* public class ServletDemo1 implements Servlet
	3. 实现接口中的抽象方法
	4. 配置Servlet
		 在web.xml中配置
	
	```xml
	<!--配置Servlet -->
	<servlet>
	    <servlet-name>demo1</servlet-name>
	    <servlet-class>cn.itcast.web.servlet.ServletDemo1</servlet-class>
	</servlet>
	
	<servlet-mapping>
	    <servlet-name>demo1</servlet-name>
	    <url-pattern>/demo1</url-pattern>
	</servlet-mapping>
	```
	
	
	
* 执行原理：
	1. 当服务器接受到客户端浏览器的请求后，会解析请求URL路径，获取访问的Servlet的资源路径
	2. 查找web.xml文件，是否有对应的<url-pattern>标签体内容。
	3. 如果有，则在找到对应的<servlet-class>全类名
	4. tomcat会将字节码文件加载进内存，并且创建其对象
	5. 调用其方法
	
* **Servlet中的生命周期方法：**
	
	1. 被创建：执行init方法，只执行一次
		* Servlet什么时候被创建？
			* 默认情况下，第一次被访问时，Servlet被创建
			* 可以配置执行Servlet的创建时机。
				* 在<servlet>标签下配置
  				1. 第一次被访问时，创建
	              	：<load-on-startup>的值为负数
		      2. 在服务器启动时，创建：<load-on-startup>的值为0或正整数
		* Servlet的init方法，只执行一次，说明一个Servlet在内存中只存在一个对象，Servlet是单例的
			* 多个用户同时访问时，可能存在线程安全问题。
			* 解决：尽量不要在Servlet中定义成员变量。即使定义了成员变量，也不要对修改值
	2. 提供服务：执行service方法，执行多次
		* 每次访问Servlet时，Service方法都会被调用一次。
	3. 被销毁：执行destroy方法，只执行一次
		* Servlet被销毁时执行。服务器关闭时，Servlet被销毁
		* 只有服务器正常关闭时，才会执行destroy方法。
		* destroy方法在Servlet被销毁之前执行，一般用于释放资源
	
* **Servlet3.0：**
	
	* 好处：
		* 支持注解配置。可以不需要web.xml了。
		
	* 步骤：
		1. 创建JavaEE项目，选择Servlet的版本3.0以上，可以不创建web.xml
		
		2. 定义一个类，实现Servlet接口
		
		3. 复写方法
		
		4. 在类上使用@WebServlet注解，进行配置
			
			```java
			    @WebServlet("资源路径")
			    @Target({ElementType.TYPE})
			    @Retention(RetentionPolicy.RUNTIME)
			    @Documented
			    public @interface WebServlet {
			        String name() default "";//相当于<Servlet-name>
			
			        String[] value() default {};//代表urlPatterns()属性配置
			
			        String[] urlPatterns() default {};//相当于<url-pattern>
			
			        int loadOnStartup() default -1;//相当于<load-on-startup>
			
			        WebInitParam[] initParams() default {};
			
			        boolean asyncSupported() default false;
			
			        String smallIcon() default "";
			
			        String largeIcon() default "";
			
			        String description() default "";
			
			        String displayName() default "";
			    }
			```

# Request、Response

## Request

1. request对象和response对象的原理
	1. request和response对象是由服务器创建的。我们来使用它们
	2. request对象是来获取请求消息，response对象是来设置响应消息
	
2. request对象继承体系结构：

  - ```
    ServletRequest		--	接口
    	|	继承
    HttpServletRequest	-- 接口
    	|	实现
    org.apache.catalina.connector.RequestFacade 类(tomcat)
    ```

    

3. request功能：
	1. 获取请求消息数据
		1. 获取请求行数据
			* GET /day14/demo1?name=zhangsan HTTP/1.1
			* 方法：
				1. 获取请求方式 ：GET
					* String getMethod()  
				2. (*)获取虚拟目录：/day14
					* String getContextPath()
				3. 获取Servlet路径: /demo1
					* String getServletPath()
				4. 获取get方式请求参数：name=zhangsan
					* String getQueryString()
				5. (*)获取请求URI：/day14/demo1
					* String getRequestURI():		/day14/demo1
					* StringBuffer getRequestURL()  :http://localhost/day14/demo1
					* URL:统一资源定位符 ： http://localhost/day14/demo1	中华人民共和国
					* URI：统一资源标识符 : /day14/demo1					共和国
				
				6. 获取协议及版本：HTTP/1.1
					* String getProtocol()
				7. 获取客户机的IP地址：
					* String getRemoteAddr()
			
		2. 获取请求头数据
			* 方法：
				* (*)String getHeader(String name):通过请求头的名称获取请求头的值
				* Enumeration<String> getHeaderNames():获取所有的请求头名称
			
		3. 获取请求体数据:
			* 请求体：只有POST请求方式，才有请求体，在请求体中封装了POST请求的请求参数
			* 步骤：
				1. 获取流对象
					*  BufferedReader getReader()：获取字符输入流，只能操作字符数据
					*  ServletInputStream getInputStream()：获取字节输入流，可以操作所有类型数据
						* 在文件上传知识点后讲解
				2. 再从流对象中拿数据
			
		
	2. 其他功能：
		1. 获取请求参数通用方式：不论get还是post请求方式都可以使用下列方法来获取请求参数
			1. String getParameter(String name):根据参数名称获取参数值    username=zs&password=123
			2. String[] getParameterValues(String name):根据参数名称获取参数值的数组  hobby=xx&hobby=game
			3. Enumeration<String> getParameterNames():获取所有请求的参数名称
			4. Map<String,String[]> getParameterMap():获取所有参数的map集合
			* 中文乱码问题：
				* get方式：tomcat 8 已经将get方式乱码问题解决了
				* post方式：会乱码
					* 解决：在获取参数前，设置request的编码request.setCharacterEncoding("utf-8");
		
		2. 请求转发：一种在服务器内部的资源跳转方式
			1. 步骤：
				1. 通过request对象获取请求转发器对象：RequestDispatcher getRequestDispatcher(String path)
				2. 使用RequestDispatcher对象来进行转发：forward(ServletRequest request, ServletResponse response) 
			2. 特点：
				1. 浏览器地址栏路径不发生变化
				2. 只能转发到当前服务器内部资源中。
				3. 转发是一次请求
		3. 共享数据：
			* 域对象：一个有作用范围的对象，可以在范围内共享数据
			* request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
			* 方法：
				1. void setAttribute(String name,Object obj):存储数据
				2. Object getAttitude(String name):通过键获取值
				3. void removeAttribute(String name):通过键移除键值对
		4. 获取ServletContext：
			* ServletContext getServletContext()

## Response

* 功能：设置响应消息
	1. 设置响应行
		1. 格式：HTTP/1.1 200 ok
		2. 设置状态码：setStatus(int sc) 
	2. 设置响应头：setHeader(String name, String value) 
		
	3. 设置响应体：
		* 使用步骤：
			1. 获取输出流
				* 字符输出流：PrintWriter getWriter()
				* 字节输出流：ServletOutputStream getOutputStream()
			2. 使用输出流，将数据输出到客户端浏览器
* 案例：
	1. 完成重定向
		* 重定向：资源跳转的方式
		
		* 代码实现：
	   
	     ```java
	        //1. 设置状态码为302
	        response.setStatus(302);
	        //2.设置响应头location
		     response.setHeader("location","/day15/responseDemo2");
		     //简单的重定向方法
		     response.sendRedirect("/day15/responseDemo2");
		  ```
		
		  
		
		* 重定向的特点:redirect
			1. 地址栏发生变化
			2. 重定向可以访问其他站点(服务器)的资源
			3. 重定向是两次请求。不能使用request对象来共享数据
			
		* 转发的特点：forward
			1. 转发地址栏路径不变
			2. 转发只能访问当前服务器下的资源
			3. 转发是一次请求，可以使用request对象来共享数据
		
		* forward 和  redirect 区别
			
		* 路径写法：
			1. 路径分类
				1. 相对路径：通过相对路径不可以确定唯一资源
					* 如：./index.html
					* 不以/开头，以.开头路径
					* 规则：找到当前资源和目标资源之间的相对位置关系
						* ./：当前目录
						* ../:后退一级目录
				2. 绝对路径：通过绝对路径可以确定唯一资源
					* 如：http://localhost/day15/responseDemo2		/day15/responseDemo2
					* 以/开头的路径
					* 规则：判断定义的路径是给谁用的？判断请求将来从哪儿发出
						* 给客户端浏览器使用：需要加虚拟目录(项目的访问路径)
							* 建议虚拟目录动态获取：request.getContextPath()
							* <a> , <form> 重定向...
						* 给服务器使用：不需要加虚拟目录
							* 转发路径
							
					
		
	2. 服务器输出字符数据到浏览器
		* 步骤：
  		1. 获取字符输出流
			2. 输出数据
		* 注意：
			* 乱码问题：
				1. PrintWriter pw = response.getWriter();获取的流的默认编码是ISO-8859-1
				2. 设置该流的默认编码
				3. 告诉浏览器响应体使用的编码
				//简单的形式，设置编码，是在获取流之前设置
	
	```java
			response.setContentType("text/html;charset=utf-8");
	```
	3. 服务器输出字节数据到浏览器
		* 步骤：
			1. 获取字节输出流
			2. 输出数据
	4. 验证码
		1. 本质：图片
		2. 目的：防止恶意表单注册

# ServletContext

1. 概念：代表整个web应用，可以和程序的容器(服务器)来通信
2. 获取：
	1. 通过request对象获取
		request.getServletContext();
	2. 通过HttpServlet获取
		this.getServletContext();
3. 功能：
	1. 获取MIME类型：
		* MIME类型:在互联网通信过程中定义的一种文件数据类型
			* 格式： 大类型/小类型   text/html		image/jpeg
		* 获取：String getMimeType(String file)  
	2. 域对象：共享数据
		1. setAttribute(String name,Object value)
		2. getAttribute(String name)
		3. removeAttribute(String name)
		* ServletContext对象范围：所有用户所有请求的数据
	3. 获取文件的真实(服务器)路径
		1. 方法：String getRealPath(String path)  
			 String b = context.getRealPath("/b.txt");//web目录下资源访问
	         System.out.println(b);
	
	        String c = context.getRealPath("/WEB-INF/c.txt");//WEB-INF目录下的资源访问
	        System.out.println(c);
	
	        String a = context.getRealPath("/WEB-INF/classes/a.txt");//src目录下的资源访问
	        System.out.println(a);

# Cookie&Session

两者区别：

- session 在服务器端，cookie 在客户端（浏览器）
- session 默认被存在在服务器的一个文件里（不是内存）
- session 的运行依赖 session id，而 session id 是存在 cookie 中的，也就是说，如果浏览器禁用了 cookie ，同时 session 也会失效（但是可以通过其它方式实现，比如在 url 中传递 session_id）
- session 可以放在 文件、数据库、或内存中都可以。
- 用户验证这种场合一般会用 session

## **Cookie**

1. 概念：客户端会话技术，将数据保存到客户端

2. 快速入门：
	* 使用步骤：
		1. 创建Cookie对象，绑定数据
			* new Cookie(String name, String value) 
		2. 发送Cookie对象
			* response.addCookie(Cookie cookie) 
		3. 获取Cookie，拿到数据
			* Cookie[]  request.getCookies()  
	
3. 实现原理
	* 基于响应头set-cookie和请求头cookie实现
	
4. cookie的细节
	1. 一次可不可以发送多个cookie?
		* 可以
		* 可以创建多个Cookie对象，使用response调用多次addCookie方法发送cookie即可。
	2. cookie在浏览器中保存多长时间？
		1. 默认情况下，当浏览器关闭后，Cookie数据被销毁
		2. 持久化存储：
			* setMaxAge(int seconds)
				1. 正数：将Cookie数据写到硬盘的文件中。持久化存储。并指定cookie存活时间，时间到后，cookie文件自动失效
				2. 负数：默认值
				3. 零：删除cookie信息
	3. cookie能不能存中文？
		* 在tomcat 8 之前 cookie中不能直接存储中文数据。
			* 需要将中文数据转码---一般采用URL编码(%E3)
		* 在tomcat 8 之后，cookie支持中文数据。特殊字符还是不支持，建议使用URL编码存储，URL解码解析
	4. cookie共享问题？
		1. 假设在一个tomcat服务器中，部署了多个web项目，那么在这些web项目中cookie能不能共享？
			* 默认情况下cookie不能共享
			* setPath(String path):设置cookie的获取范围。默认情况下，设置当前的虚拟目录
				* 如果要共享，则可以将path设置为"/"
		
		2. 不同的tomcat服务器间cookie共享问题？
			* setDomain(String path):如果设置一级域名相同，那么多个服务器之间cookie可以共享
				* setDomain(".baidu.com"),那么tieba.baidu.com和news.baidu.com中cookie可以共享
	
5. Cookie的特点和作用
	1. cookie存储数据在客户端浏览器
	2. 浏览器对于单个cookie 的大小有限制(4kb) 以及 对同一个域名下的总cookie数量也有限制(20个)
	* 作用：
		1. cookie一般用于存出少量的不太敏感的数据
		2. 在不登录的情况下，完成服务器对客户端的身份识别

## Session

1. 概念：服务器端会话技术，在一次会话的多次请求间共享数据，将数据保存在服务器端的对象中。HttpSession

2. 快速入门：
	1. 获取HttpSession对象：
		HttpSession session = request.getSession();
	2. 使用HttpSession对象：
		Object getAttribute(String name)  
		void setAttribute(String name, Object value)
		void removeAttribute(String name)  
	
3. 原理
	* Session的实现是依赖于Cookie的。
	
4. 细节：
	1. 当客户端关闭后，服务器不关闭，两次获取session是否为同一个？
		* 默认情况下。不是。
		* 如果需要相同，则可以创建Cookie,键为JSESSIONID，设置最大存活时间，让cookie持久化保存。
			 Cookie c = new Cookie("JSESSIONID",session.getId());
	         c.setMaxAge(60*60);
	         response.addCookie(c);
	2. 客户端不关闭，服务器关闭后，两次获取的session是同一个吗？
		* 不是同一个，但是要确保数据不丢失。tomcat自动完成以下工作
			* session的钝化：
				* 在服务器正常关闭之前，将session对象系列化到硬盘上
			* session的活化：
				* 在服务器启动后，将session文件转化为内存中的session对象即可。
		
	3. session什么时候被销毁？
		1. 服务器关闭
		2. session对象调用invalidate() 。
		3. session默认失效时间 30分钟
			选择性配置修改	
			<session-config>
		        <session-timeout>30</session-timeout>
		    </session-config>
	
 5. session的特点
	 1. session用于存储一次会话的多次请求的数据，存在服务器端
	 2. session可以存储任意类型，任意大小的数据
	* session与Cookie的区别：
		1. session存储数据在服务器端，Cookie在客户端
		2. session没有数据大小限制，Cookie有
		3. session数据安全，Cookie相对于不安全

# MVC

1. jsp演变历史
	1. 早期只有servlet，只能使用response输出标签数据，非常麻烦
	2. 后来又jsp，简化了Servlet的开发，如果过度使用jsp，在jsp中即写大量的java代码，有写html表，造成难于维护，难于分工协作
	3. 再后来，java的web开发，借鉴mvc开发模式，使得程序的设计更加合理性

2. MVC：
	1. M：Model，模型。JavaBean
		* 完成具体的业务操作，如：查询数据库，封装对象
	2. V：View，视图。JSP
		* 展示数据
	3. C：Controller，控制器。Servlet
		* 获取用户的输入
		* 调用模型
		* 将数据交给视图进行展示

3. 优缺点：

   	1. 优点：
   		1. 耦合性低，方便维护，可以利于分工协作
   		2. 重用性高

   2. 缺点：
   	1. 使得项目架构变得复杂，对开发人员要求高

# JSP

1. 概念：
	* Java Server Pages： java服务器端页面
		* 可以理解为：一个特殊的页面，其中既可以指定定义html标签，又可以定义java代码
		* 原理：JSP本质上就是一个Servlet
	
3. JSP的脚本：JSP定义Java代码的方式
	1. <%  代码 %>：定义的java代码，在service方法中。service方法中可以定义什么，该脚本中就可以定义什么。
	2. <%! 代码 %>：定义的java代码，在jsp转换后的java类的成员位置。
	3. <%= 代码 %>：定义的java代码，会输出到页面上。输出语句中可以定义什么，该脚本中就可以定义什么。
	
4. JSP的内置对象：
	* 在jsp页面中不需要获取和创建，可以直接使用的对象
	* jsp一共有9个内置对象。
	
	  - 		 变量名					真实类型						作用
	  - 		 pageContext			PageContext					当前页面共享数据，还可以获取其他八个内置对象
	  - 		 request					HttpServletRequest			一次请求访问的多个资源(转发)
	  - 		 session					HttpSession					一次会话的多个请求间
	  - 		 application			   ServletContext				所有用户间共享数据
	  - 		 response				 HttpServletResponse			响应对象
	  - 		 page					   Object						当前页面(Servlet)的对象  this
	  - 		 out						  JspWriter					输出对象，数据输出到页面上
	  - 		 config					  ServletConfig				Servlet的配置对象
	  - 		 exception				 Throwable					异常对象
	* 常见：
		* request
		* response
		* out：字符输出流对象。可以将数据输出到页面上。和response.getWriter()类似
			* response.getWriter()和out.write()的区别：
				* 在tomcat服务器真正给客户端做出响应之前，会先找response缓冲区数据，再找out缓冲区数据。
				* response.getWriter()数据输出永远在out.write()之前
	
4. 使用
   1. 指令
   2. 作用：用于配置JSP页面，导入资源文件
   3. 格式：
       <%@ 指令名称 属性名1=属性值1 属性名2=属性值2 ... %>
   4. 分类：
     1. page： 配置JSP页面的
   
     	* contentType：等同于response.setContentType()
     		1. 设置响应体的mime类型以及字符集
     		2. 设置当前jsp页面的编码（只能是高级的IDE才能生效，如果使用低级工具，则需要设置pageEncoding属性设置当前页面的字符集）
     	* import：导包
     	* errorPage：当前页面发生异常后，会自动跳转到指定的错误页面
     	* isErrorPage：标识当前也是是否是错误页面。
     		* true：是，可以使用内置对象exception
     		* false：否。默认值。不可以使用内置对象exception
   
5. 注释:

   - html注释：
     <!-- -->:只能注释html代码片段

   - jsp注释：推荐使用
     <%-- --%>：可以注释所有

## EL表达式

1. 概念：Expression Language 表达式语言
2. 作用：替换和简化jsp页面中java代码的编写
3. 语法：${表达式}
4. 注意：
	* jsp默认支持el表达式的。如果要忽略el表达式
		1. 设置jsp中page指令中：isELIgnored="true" 忽略当前jsp页面中所有的el表达式
		2.  \ ${表达式} ：忽略当前这个el表达式


5. 使用：
	1. 运算：
		* 运算符：
			1. 算数运算符： + - * /(div) %(mod)
			2. 比较运算符： > < >= <= == !=
			3. 逻辑运算符： &&(and) ||(or) !(not)
			4. 空运算符： empty
				* 功能：用于判断字符串、集合、数组对象是否为null或者长度是否为0
				* ${empty list}:判断字符串、集合、数组对象是否为null或者长度为0
				* ${not empty str}:表示判断字符串、集合、数组对象是否不为null 并且 长度>0
	2. 获取值
		1. el表达式只能从域对象中获取值
		2. 语法：
			1. ${域名称.键名}：从指定域中获取指定键的值
				* 域名称：
					1. pageScope		--> pageContext
					2. requestScope 	--> request
					3. sessionScope 	--> session
					4. applicationScope --> application（ServletContext）
				* 举例：在request域中存储了name=张三
				* 获取：${requestScope.name}

			2. ${键名}：表示依次从最小的域中查找是否有该键对应的值，直到找到为止。

			
			
			3. 获取对象、List集合、Map集合的值
				1. 对象：${域名称.键名.属性名}
					* 本质上会去调用对象的getter方法

				2. List集合：${域名称.键名[索引]}

				3. Map集合：
					* ${域名称.键名.key名称}
					* ${域名称.键名["key名称"]}
	
	3. 隐式对象：
		* el表达式中有11个隐式对象
		* pageContext：
			* 获取jsp其他八个内置对象
				* ${pageContext.request.contextPath}：动态获取虚拟目录

## JSTL

1. 概念：JavaServer Pages Tag Library  JSP标准标签库
	* 是由Apache组织提供的开源的免费的jsp标签		<标签>

2. 作用：用于简化和替换jsp页面上的java代码		

3. 使用步骤：
	1. 导入jstl相关jar包
	2. 引入标签库：taglib指令：  <%@ taglib %>
	3. 使用标签

4. 常用的JSTL标签
	1. if:相当于java代码的if语句
		1. 属性：
            * test 必须属性，接受boolean表达式
                * 如果表达式为true，则显示if标签体内容，如果为false，则不显示标签体内容
                * 一般情况下，test属性值会结合el表达式一起使用
   		    
      	    注意： c:if标签没有else情况，想要else情况，则可以在定义一个c:if标签
	   
	2. choose:相当于java代码的switch语句
   	1. 使用choose标签声明         			相当于switch声明
        2. 使用when标签做判断         			相当于case
	     3. 使用otherwise标签做其他情况的声明    	相当于default
	
	3. foreach:相当于java代码的for语句

# Filter

1. 概念：
	* 生活中的过滤器：净水器、空气净化、土匪
	* web中的过滤器：当访问服务器的资源时，过滤器可以将请求拦截下来，完成一些特殊的功能。
	* 过滤器的作用：
		* 一般用于完成通用的操作。如：登录验证、统一编码处理、敏感字符过滤...

2. 快速入门：
	1. 步骤：
		1. 定义一个类，实现接口Filter
		2. 复写方法
		3. 配置拦截路径
			1. web.xml
			2. 注解
	2. 代码：
		
		```java
				@WebFilter("/*")//访问所有资源之前，都会执行该过滤器
				public class FilterDemo1 implements Filter {
				    @Override
				    public void init(FilterConfig filterConfig) throws ServletException {
				
				    }
				
				    @Override
				    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
				        System.out.println("filterDemo1被执行了....");
				
				
				        //放行
				        filterChain.doFilter(servletRequest,servletResponse);
				
				    }
				
				    @Override
				    public void destroy() {
				
				    }
				}
		```
		


3. 过滤器细节：
	1. web.xml配置
	
	  ```xml
	  	<filter>
	  	        <filter-name>demo1</filter-name>
	  	        <filter-class>cn.itcast.web.filter.FilterDemo1</filter-class>
	  	    </filter>
	  	    <filter-mapping>
	  	        <filter-name>demo1</filter-name>
	  			<!-- 拦截路径 -->
	  	        <url-pattern>/*</url-pattern>
	  	    </filter-mapping>
	  ```
	
	  
	
	2. 过滤器执行流程
		1. 执行过滤器
		2. 执行放行后的资源
		3. 回来执行过滤器放行代码下边的代码
		
	3. 过滤器生命周期方法
		1. init:在服务器启动后，会创建Filter对象，然后调用init方法。只执行一次。用于加载资源
		2. doFilter:每一次请求被拦截资源时，会执行。执行多次
		3. destroy:在服务器关闭后，Filter对象被销毁。如果服务器是正常关闭，则会执行destroy方法。只执行一次。用于释放资源
		
	4. 过滤器配置详解
		* 拦截路径配置：
			1. 具体资源路径： /index.jsp   只有访问index.jsp资源时，过滤器才会被执行
			2. 拦截目录： /user/*	访问/user下的所有资源时，过滤器都会被执行
			3. 后缀名拦截： *.jsp		访问所有后缀名为jsp资源时，过滤器都会被执行
			4. 拦截所有资源：/*		访问所有资源时，过滤器都会被执行
		* 拦截方式配置：资源被访问的方式
			* 注解配置：
				* 设置dispatcherTypes属性
					1. REQUEST：默认值。浏览器直接请求资源
					2. FORWARD：转发访问资源
					3. INCLUDE：包含访问资源
					4. ERROR：错误跳转资源
					5. ASYNC：异步访问资源
			* web.xml配置
				* 设置<dispatcher></dispatcher>标签即可
		
	5. 过滤器链(配置多个过滤器)
		* 执行顺序：如果有两个过滤器：过滤器1和过滤器2
			1. 过滤器1
			2. 过滤器2
			3. 资源执行
			4. 过滤器2
			5. 过滤器1 
	
		* 过滤器先后顺序问题：
			1. 注解配置：按照类名的字符串比较规则比较，值小的先执行
				* 如： AFilter 和 BFilter，AFilter就先执行了。
			2. web.xml配置： <filter-mapping>谁定义在上边，谁先执行
	
	# Listener
	
	* 概念：web的三大组件之一。
		* 事件监听机制
			* 事件	：一件事情
			* 事件源 ：事件发生的地方
			* 监听器 ：一个对象
			* 注册监听：将事件、事件源、监听器绑定在一起。 当事件源上发生某个事件后，执行监听器代码
	
	
	* ServletContextListener:监听ServletContext对象的创建和销毁
		* 方法：
			* void contextDestroyed(ServletContextEvent sce) ：ServletContext对象被销毁之前会调用该方法
			* void contextInitialized(ServletContextEvent sce) ：ServletContext对象创建后会调用该方法
			
		* 步骤：
			1. 定义一个类，实现ServletContextListener接口
			
			2. 复写方法
			
			3. 配置
				1. web.xml
				
				   ```xml
				   					<listener>
				    					 <listener-class>cn.itcast.web.listener.ContextLoaderListener</listener-class>
				   					</listener>
				   
				   ```
				
				2. 注解：@WebListener

# log4j配置详解

## 一.参数意义说明

输出级别的种类

```properties
ERROR、WARN、INFO、DEBUG
ERROR 为严重错误 主要是程序的错误
WARN 为一般警告，比如session丢失
INFO 为一般要显示的信息，比如登录登出
DEBUG 为程序的调试信息
```

配置日志信息输出目的地

```properties
log4j.appender.appenderName = fully.qualified.name.of.appender.class
1.org.apache.log4j.ConsoleAppender（控制台）
2.org.apache.log4j.FileAppender（文件）
3.org.apache.log4j.DailyRollingFileAppender（每天产生一个日志文件）
4.org.apache.log4j.RollingFileAppender（文件大小到达指定尺寸的时候产生一个新的文件）
5.org.apache.log4j.WriterAppender（将日志信息以流格式发送到任意指定的地方）
```

配置日志信息的格式

```properties
log4j.appender.appenderName.layout = fully.qualified.name.of.layout.class
1.org.apache.log4j.HTMLLayout（以HTML表格形式布局），
2.org.apache.log4j.PatternLayout（可以灵活地指定布局模式），
3.org.apache.log4j.SimpleLayout（包含日志信息的级别和信息字符串），
4.org.apache.log4j.TTCCLayout（包含日志产生的时间、线程、类别等等信息）
```

控制台选项

```properties
Threshold=DEBUG:指定日志消息的输出最低层次。
ImmediateFlush=true:默认值是true,意谓着所有的消息都会被立即输出。
Target=System.err：默认情况下是：System.out,指定输出控制台
FileAppender 选项
Threshold=DEBUF:指定日志消息的输出最低层次。
ImmediateFlush=true:默认值是true,意谓着所有的消息都会被立即输出。
File=mylog.txt:指定消息输出到mylog.txt文件。
Append=false:默认值是true,即将消息增加到指定文件中，false指将消息覆盖指定的文件内容。
RollingFileAppender 选项
Threshold=DEBUG:指定日志消息的输出最低层次。
ImmediateFlush=true:默认值是true,意谓着所有的消息都会被立即输出。
File=mylog.txt:指定消息输出到mylog.txt文件。
Append=false:默认值是true,即将消息增加到指定文件中，false指将消息覆盖指定的文件内容。
MaxFileSize=100KB: 后缀可以是KB, MB 或者是 GB. 在日志文件到达该大小时，将会自动滚动，即将原来的内容移到mylog.log.1文件。
MaxBackupIndex=2:指定可以产生的滚动文件的最大数。
log4j.appender.A1.layout.ConversionPattern=%-4r %-5p %d{yyyy-MM-dd HH:mm:ssS} %c %m%n
```

日志信息格式中几个符号所代表的含义：

```properties
 -X号: X信息输出时左对齐；
 %p: 输出日志信息优先级，即DEBUG，INFO，WARN，ERROR，FATAL,
 %d: 输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，比如：%d{yyy MMM dd HH:mm:ss,SSS}，输出类似：2002年10月18日 22：10：28，921
 %r: 输出自应用启动到输出该log信息耗费的毫秒数
 %c: 输出日志信息所属的类目，通常就是所在类的全名
 %t: 输出产生该日志事件的线程名
 %l: 输出日志事件的发生位置，相当于%C.%M(%F:%L)的组合,包括类目名、发生的线程，以及在代码中的行数。举例：Testlog4.main (TestLog4.java:10)
 %x: 输出和当前线程相关联的NDC(嵌套诊断环境),尤其用到像java servlets这样的多客户多线程的应用中。
 %%: 输出一个"%"字符
 %F: 输出日志消息产生时所在的文件名称
 %L: 输出代码中的行号
 %m: 输出代码中指定的消息,产生的日志具体信息
 %n: 输出一个回车换行符，Windows平台为"/r/n"，Unix平台为"/n"输出日志信息换行
 可以在%与模式字符之间加上修饰符来控制其最小宽度、最大宽度、和文本的对齐方式。如：
 1)%20c：指定输出category的名称，最小的宽度是20，如果category的名称小于20的话，默认的情况下右对齐。
 2)%-20c:指定输出category的名称，最小的宽度是20，如果category的名称小于20的话，"-"号指定左对齐。
 3)%.30c:指定输出category的名称，最大的宽度是30，如果category的名称大于30的话，就会将左边多出的字符截掉，但小于30的话也不会有空格。
 4)%20.30c:如果category的名称小于20就补空格，并且右对齐，如果其名称长于30字符，就从左边较远输出的字符截掉。
```

## 二.文件配置

Sample1

```properties
log4j.rootLogger=DEBUG,A1,R
#log4j.rootLogger=INFO,A1,R
# ConsoleAppender 输出
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%-d{yyyy-MM-dd HH:mm:ss,SSS} [%c]-[%p] %m%n
# File 输出 一天一个文件,输出路径可以定制,一般在根路径下
log4j.appender.R=org.apache.log4j.DailyRollingFileAppender
log4j.appender.R.File=blog_log.txt
log4j.appender.R.MaxFileSize=500KB
log4j.appender.R.MaxBackupIndex=10
log4j.appender.R.layout=org.apache.log4j.PatternLayout
log4j.appender.R.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS} [%t] [%c] [%p] - %m%n
```

Sample2

下面给出的Log4J配置文件实现了输出到控制台，文件，回滚文件，发送日志邮件，输出到数据库日志表，自定义标签等全套功能。

~~~properties
    log4j.rootLogger=DEBUG,CONSOLE,A1,im
    #DEBUG,CONSOLE,FILE,ROLLING_FILE,MAIL,DATABASE
    log4j.addivity.org.apache=true
    ###################
    # Console Appender
    ###################
    log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
    log4j.appender.Threshold=DEBUG
    log4j.appender.CONSOLE.Target=System.out
    log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
    log4j.appender.CONSOLE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n
    #log4j.appender.CONSOLE.layout.ConversionPattern=[start]%d{DATE}[DATE]%n%p[PRIORITY]%n%x[NDC]%n%t[THREAD] n%c[CATEGORY]%n%m[MESSAGE]%n%n
    #####################
    # File Appender
    #####################
    log4j.appender.FILE=org.apache.log4j.FileAppender
    log4j.appender.FILE.File=file.log
    log4j.appender.FILE.Append=false
    log4j.appender.FILE.layout=org.apache.log4j.PatternLayout
    log4j.appender.FILE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n
    # Use this layout for LogFactor 5 analysis
    ########################
    # Rolling File
    ########################
    log4j.appender.ROLLING_FILE=org.apache.log4j.RollingFileAppender
    log4j.appender.ROLLING_FILE.Threshold=ERROR
    log4j.appender.ROLLING_FILE.File=rolling.log
    log4j.appender.ROLLING_FILE.Append=true
    log4j.appender.ROLLING_FILE.MaxFileSize=10KB
    log4j.appender.ROLLING_FILE.MaxBackupIndex=1
    log4j.appender.ROLLING_FILE.layout=org.apache.log4j.PatternLayout
    log4j.appender.ROLLING_FILE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n
    ####################
    # Socket Appender
    ####################
    log4j.appender.SOCKET=org.apache.log4j.RollingFileAppender
    log4j.appender.SOCKET.RemoteHost=localhost
    log4j.appender.SOCKET.Port=5001
    log4j.appender.SOCKET.LocationInfo=true
    # Set up for Log Facter 5
    log4j.appender.SOCKET.layout=org.apache.log4j.PatternLayout
    log4j.appender.SOCET.layout.ConversionPattern=[start]%d{DATE}[DATE]%n%p[PRIORITY]%n%x[NDC]%n%t[THREAD]%n%c[CATEGORY]%n%m[MESSAGE]%n%n
    ########################
    # Log Factor 5 Appender
    ########################
    log4j.appender.LF5_APPENDER=org.apache.log4j.lf5.LF5Appender
    log4j.appender.LF5_APPENDER.MaxNumberOfRecords=2000
    ########################
    # SMTP Appender
    #######################
    log4j.appender.MAIL=org.apache.log4j.net.SMTPAppender
    log4j.appender.MAIL.Threshold=FATAL
    log4j.appender.MAIL.BufferSize=10
    log4j.appender.MAIL.From=chenyl@yeqiangwei.com
    log4j.appender.MAIL.SMTPHost=mail.hollycrm.com
    log4j.appender.MAIL.Subject=Log4J Message
    log4j.appender.MAIL.To=chenyl@yeqiangwei.com
    log4j.appender.MAIL.layout=org.apache.log4j.PatternLayout
    log4j.appender.MAIL.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n
    ########################
    # JDBC Appender
    #######################
    log4j.appender.DATABASE=org.apache.log4j.jdbc.JDBCAppender
    log4j.appender.DATABASE.URL=jdbc:mysql://localhost:3306/test
    log4j.appender.DATABASE.driver=com.mysql.jdbc.Driver
    log4j.appender.DATABASE.user=root
    log4j.appender.DATABASE.password=
    log4j.appender.DATABASE.sql=INSERT INTO LOG4J (Message) VALUES ('[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n')
    log4j.appender.DATABASE.layout=org.apache.log4j.PatternLayout
    log4j.appender.DATABASE.layout.ConversionPattern=[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n
    log4j.appender.A1=org.apache.log4j.DailyRollingFileAppender
    log4j.appender.A1.File=SampleMessages.log4j
    log4j.appender.A1.DatePattern=yyyyMMdd-HH'.log4j'
    log4j.appender.A1.layout=org.apache.log4j.xml.XMLLayout
    ###################
    #自定义Appender
    ###################
    log4j.appender.im = net.cybercorlin.util.logger.appender.IMAppender
    log4j.appender.im.host = mail.cybercorlin.net
    log4j.appender.im.username = username
    log4j.appender.im.password = password
    log4j.appender.im.recipient = corlin@yeqiangwei.com
    log4j.appender.im.layout=org.apache.log4j.PatternLayout
    log4j.appender.im.layout.ConversionPattern =[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n
~~~

## 三.高级使用：

  1. 把FATAL级错误写入2000NT日志
  2. WARN，ERROR，FATAL级错误发送email通知管理员
  3. 其他级别的错误直接在后台输出

输出到2000NT日志

1.把Log4j压缩包里的NTEventLogAppender.dll拷到WINNT/SYSTEM32目录下

2.写配置文件log4j.properties

```properties
# 在2000系统日志输出
log4j.logger.NTlog=FATAL, A8
#APPENDER A8
log4j.appender.A8=org.apache.log4j.nt.NTEventLogAppender
log4j.appender.A8.Source=JavaTest
log4j.appender.A8.layout=org.apache.log4j.PatternLayout
log4j.appender.A8.layout.ConversionPattern=%-4r %-5p [%t] %37c %3x - %m%n
```

3.调用代码：

```java
 Logger logger2 = Logger.getLogger("NTlog"); //要和配置文件中设置的名字相同
 logger2.debug("debug!!!");
 logger2.info("info!!!");
 logger2.warn("warn!!!");
 logger2.error("error!!!");
 //只有这个错误才会写入2000日志
 logger2.fatal("fatal!!!");
```

发送email通知管理员：

1.首先下载JavaMail和JAF

  http://java.sun.com/j2ee/ja/javamail/index.html
  http://java.sun.com/beans/glasgow/jaf.html

 在项目中引用mail.jar和activation.jar。

2.写配置文件

```properties
# 将日志发送到email
log4j.logger.MailLog=WARN,A5
# APPENDER A5
log4j.appender.A5=org.apache.log4j.net.SMTPAppender
log4j.appender.A5.BufferSize=5
log4j.appender.A5.To=chunjie@yeqiangwei.com
log4j.appender.A5.From=error@yeqiangwei.com
log4j.appender.A5.Subject=ErrorLog
log4j.appender.A5.SMTPHost=smtp.263.net
log4j.appender.A5.layout=org.apache.log4j.PatternLayout
log4j.appender.A5.layout.ConversionPattern=%-4r %-5p [%t] %37c %3x - %m%n
```

3.调用代码：

```java
 //把日志发送到mail
 Logger logger3 = Logger.getLogger("MailLog");
 logger3.warn("warn!!!");
 logger3.error("error!!!");
 logger3.fatal("fatal!!!");
```

在后台输出所有类别的错误

1. 写配置文件

```properties
# 在后台输出
log4j.logger.console=DEBUG, A1
#APPENDER A1
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%-4r %-5p [%t] %37c %3x - %m%n
```

2．调用代码

```java
 Logger logger1 = Logger.getLogger("console");
 logger1.debug("debug!!!");
 logger1.info("info!!!");
 logger1.warn("warn!!!");
 logger1.error("error!!!");
 logger1.fatal("fatal!!!");
```

全部配置文件：log4j.properties

```properties
 #在后台输出
 log4j.logger.console=DEBUG, A1
 # APPENDER A1
 log4j.appender.A1=org.apache.log4j.ConsoleAppender
 log4j.appender.A1.layout=org.apache.log4j.PatternLayout
 log4j.appender.A1.layout.ConversionPattern=%-4r %-5p [%t] %37c %3x - %m%n
# 在2000系统日志输出
 log4j.logger.NTlog=FATAL, A8
 # APPENDER A8
 log4j.appender.A8=org.apache.log4j.nt.NTEventLogAppender
 log4j.appender.A8.Source=JavaTest
 log4j.appender.A8.layout=org.apache.log4j.PatternLayout
 log4j.appender.A8.layout.ConversionPattern=%-4r %-5p [%t] %37c %3x - %m%n
# 将日志发送到email
 log4j.logger.MailLog=WARN,A5
 #  APPENDER A5
 log4j.appender.A5=org.apache.log4j.net.SMTPAppender
 log4j.appender.A5.BufferSize=5
 log4j.appender.A5.To=chunjie@yeqiangwei.com
 log4j.appender.A5.From=error@yeqiangwei.com
 log4j.appender.A5.Subject=ErrorLog
 log4j.appender.A5.SMTPHost=smtp.263.net
 log4j.appender.A5.layout=org.apache.log4j.PatternLayout
 log4j.appender.A5.layout.ConversionPattern=%-4r %-5p [%t] %37c %3x - %m%n
```

全部代码：Log4jTest.java

```java
import org.apache.log4j.*;
//import org.apache.log4j.nt.*;
//import org.apache.log4j.net.*;

public class Log4jTest
{
    public static void main(String args[])
    {
        PropertyConfigurator.configure("log4j.properties");
        //在后台输出
        Logger logger1 = Logger.getLogger("console");
        logger1.debug("debug!!!");
        logger1.info("info!!!");
        logger1.warn("warn!!!");
        logger1.error("error!!!");
        logger1.fatal("fatal!!!");
        //在NT系统日志输出
        Logger logger2 = Logger.getLogger("NTlog");
        //NTEventLogAppender nla = new NTEventLogAppender();
        logger2.debug("debug!!!");
        logger2.info("info!!!");
        logger2.warn("warn!!!");
        logger2.error("error!!!");
        //只有这个错误才会写入2000日志
        logger2.fatal("fatal!!!");
        //把日志发送到mail
        Logger logger3 = Logger.getLogger("MailLog");
        //SMTPAppender sa = new SMTPAppender();
        logger3.warn("warn!!!");
        logger3.error("error!!!");
        logger3.fatal("fatal!!!");
    }
}
```

