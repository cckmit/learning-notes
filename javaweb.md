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

