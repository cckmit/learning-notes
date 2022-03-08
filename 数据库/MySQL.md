# MySQL 基础

## 入门

数据库（Database）是按照数据结构来组织、存储和管理数据的仓库，所谓的关系型数据库，是建立在关系模型基础上的数据库，借助于集合代数等数学概念和方法来处理数据库中的数据。

RDBMS即关系数据库管理系统(Relational Database Management System)的特点：

- 数据以表格的形式出现
- 每行为各种记录名称
- 每列为记录名称所对应的数据域
- 许多的行和列组成一张表单
- 若干的表单组成database

在我们开始学习MySQL 数据库前，让我们先了解下RDBMS的一些术语：

- **数据库:** 数据库是一些关联表的集合。.
- **数据表:** 表是数据的矩阵。在一个数据库中的表看起来像一个简单的电子表格。
- **列:** 一列(数据元素) 包含了相同的数据, 例如邮政编码的数据。
- **行：**一行（=元组，或记录）是一组相关的数据，例如一条用户订阅的数据。
- **冗余**：存储两倍数据，冗余可以使系统速度更快。
- **主键**：主键是唯一的。一个数据表中只能包含一个主键。你可以使用主键来查询数据。
- **外键：**外键用于关联两个表。
- **复合键**：复合键（组合键）将多个列作为一个索引键，一般用于复合索引。
- **索引：**使用索引可快速访问数据库表中的特定信息。索引是对数据库表中一列或多列的值进行排序的一种结构。类似于书籍的目录。
- **参照完整性:** 参照的完整性要求关系中不允许引用不存在的实体。与实体完整性是关系模型必须满足的完整性约束条件，目的是保证数据的一致性。

## 问题

```shell
# (1) 首先检查网络是否连通

  # 是否能ping通网络
  ping <ip>

  # 检测端口是否有开放，先要安装telnet工具
  telnet <ip> <port>

# (2) 从机器内部定位
  # 查看mysql服务是否开启
  ps -ef | grep mysqld
  # 查看tcp端口是否开启
  netstat -lnpt
  # 查看mysql是否指定了bind-address
  vim /etc/mysql/my.cnf
  #查看mysql的账号是否允许外部连接(需要重启mysql：/etc/init.d/mysqld restart)
    mysql -u root -p
    use mysql
    select user,host from user;
    update user set host=‘%’ where user=‘root’;
    或
    GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
  # 查看防火墙是否开启mysql端口
    iptables -S
    /sbin/iptables -I INPUT -p tcp --dport 8011 -j ACCEPT #开启8011端口
    /etc/init.d/iptables restart
```

## 管理

查询是否启动：`    ps -ef | grep mysqld`

登录mysql：` mysql -u root -p` 

选择数据库：` use xxdatabase` 

**常用管理命令**

- **SHOW DATABASES:** 列出 MySQL 数据库管理系统的数据库列表。
- **SHOW TABLES:** 显示指定数据库的所有表，使用该命令前需要使用 use 命令来选择要操作的数据库。
- **SHOW COLUMNS FROM 数据表:** 显示数据表的属性，属性类型，主键信息 ，是否为 NULL，默认值等其他信息。
- **SHOW INDEX FROM 数据表:** 显示数据表的详细索引信息，包括PRIMARY KEY（主键）。
- **SHOW TABLE STATUS LIKE 数据表:** 该命令将输出MySQL数据库管理系统的性能及统计信息。

## 数据类型

### 整型

| 整型类型  | 字节数 | 有符号范围        | 无符号范围  |
| :-------- | :----- | :---------------- | :---------- |
| TINYINT   | 1字节  | (-128，127)       | (0，255)    |
| SMALLINT  | 2字节  | (-32768，32767)   | (0，65 535) |
| MEDIUMINT | 3字节  | (-2^26，2 ^26-1)  | (0，2^27-1) |
| INT       | 4字节  | (-2^31，2 ^31-1)  | (0，2^32-1) |
| BIGINT    | 8字节  | (-2 ^ 63，2^63-1) | (0，2^64-1) |

可选参数：整型[(M)] [UNSIGNED] [ZEROFILL]

- UNSIGNED：无符号数，例如：age TINYINT UNSIGNED;
- ZEROFILL：其中参数M表示字节宽度，自动会转为无符号整型，一般适用于学号、编码等宽度无符号数字，可以用0填充，例如：sn TINYINT(6) ZEROFILL;

### 浮点型

注意：浮点数有精度损失的。

| 整型类型 | 字节数 | 有符号范围               | 无符号范围     |
| :------- | :----- | :----------------------- | :------------- |
| FLOAT    | 4字节  | (-3.40E+38, -1.17E-38)   | (0, 3.40E+38)  |
| DOUBLE   | 8字节  | (-1.80E+308, -2.23E-308) | (0, 2.23E+308) |

可选参数：浮点型[(M,D)] [UNSIGNED] [ZEROFILL]

其中M表示显示的值（包括小数位）最大位数，D表示小数位数。 例如：sara FLOAT(7,3) 规定显示的值不会超过7位数字，小数点后面带有 3位数字。

对于小数点后面的位数超过允许范围的值，MySQL 会自动将它四舍五入为最接近它的值。

注意：浮点数有精度损失的。

### 定点型

把整数和小数部分分开存储，比较精确。可选参数：DECIMAL[(M[,D])] [UNSIGNED] [ZEROFILL]

示例：sara DECIMAL(6,2);

### 字符串

| 字符串类型 | 字节数    | 描述及存储需求                  |
| :--------- | :-------- | :------------------------------ |
| CHAR       | 0～255    | 定长字符串                      |
| VARCHAR    | 0～255    | 变长字符串                      |
| TINYBLOB   | 0～255    | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0～255    | 短文本字符串                    |
| BLOB       | 0～65535  | 二进制形式的长文本数据          |
| TEXT       | 0～65535  | 长文本数据                      |
| MEDIUMBLOB | 0～2^24-1 | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0～2^24-1 | 中等长度文本数据                |
| LOGNGBLOB  | 0～2^32-1 | 二进制形式的极大文本数据        |
| LONGTEXT   | 0～2^32-1 | 极大文本数据                    |

注意：

- char型，如果不够M个字符，内部用空格补齐，取出时会把右侧空格删除，意味着本身有的空格会被丢弃。varchar类型则不会。
- 速度上定长比不定长的更快。
- blob是二进制类型，用来储存图像、音频等类型。可以防止字符集问题导致信息丢失。

### 枚举和集合

- 枚举：ENUM(‘value1’,‘value2’,…)，插入值时，只能在枚举范围中的一个。
- 集合：SET(‘value1’,‘value2’,…)，插入值时，值是在集合的一个或多个。

### 日期时间类型

| 类型      | 字节数 | 范围                                     | 格式                |
| :-------- | :----- | :--------------------------------------- | :------------------ |
| DATE      | 4      | 1000-01-01～9999-12-31                   | YYYY-MM-DD          |
| TIME      | 3      | ‘00:00:00’～’23:59:59’                   | HH:MM:SS            |
| YEAR      | 1      | 1901～2155                               | YYYY                |
| DATETIME  | 8      | 1000-01-01 00:00:00～9999-12-31 23:59:59 | YYYY-MM-DD HH:MM:SS |
| TIMESTAMP | 4      | 1970-01-01 00:00:00～2037年某时          | YYYY-MM-DD HH:MM:SS |

注意：TIMESTAMP是系统自动填充的时间戳，效率不高，不建议使用，通常用int unsigned来储存时间戳。

```sql
# 获取时间戳
SELECT UNIX_TIMESTAMP() ; #当前时间戳1451588688
SELECT UNIX_TIMESTAMP('2016-01-01 00:00:00');

# 把日期时间转为时间戳
SELECT FROM_UNIXTIME(1451588888, '%Y-%m-%d %h:%i:%s'); # 2016-01-01 03:08:08
```

## 库和数据库表

### 库

数据库就是存放数据的仓库，这个仓库是按照一定的数据结构来组织存储的。

```sql
# 显示数据库
show databases;

# 选择数据库
use dbname;

# 创建数据库
create database dbname charset utf8;

# 删除数据库
drop database dbname;
```

### 数据表

数据表是关系数据库中一个非常重要的对象，是其它对象的基础，也是一系列二维数组的集合，用来存储和操作数据的逻辑结构。

#### 表的列操作

##### **查看表字段信息**

```sql
# 语法
DESC 表名

# 示例
DESC student;
```

##### **查看表的所有信息**

```sql
# 语法
SHOW CREATE TABLE 表名

# 示例
SHOW CREATE TABLE student;
```

##### **查看当前库下所有表信息**

```sql
# 语法
SHOW TABLE status

# 示例
# 列形式输出
SHOW TABLE status \G;

#或过滤表输出
SHOW TABLE status WHERE name='goods' \G;
```

##### **创建表**

```sql
# 语法
CREATE TABLE 表名(列名 列类型 列属性 ......)

# 示例
CREATE TABLE student (
  sid TINYINT UNSIGNED,
  name VARCHAR(20),
  age INT
);
```

##### **在原表上添加新的列**

```sql
# 语法
ALTER TABLE 表名 ADD COLUMN 列名 列类型 列属性 ......

# 示例
# 在最后添加列
ALTER TABLE student ADD COLUMN sn TINYINT(6) ZEROFILL;

# 在指定位置添加列：
ALTER TABLE user ADD height TINYINT AFTER weight;
```

##### **删除表**

```sql
# 语法
DROP TABLE 表名

# 示例
DROP TABLE student;
```

##### **删除列**

```sql
# 语法
ALTER TABLE 表名 DROP 列名

# 示例
ALTER TABLE student DROP COLUMN sn;
```

##### **清空表内容**

TRUNCATE相当与drop和create表两个操作。

```sql
# 语法
TRUNCATE 表名

# 示例
TRUNCATE student;
```

##### **修改表名称**

```sql
# 语法
ALTER TABLE 旧表名 RENAME TO 新表名

# 示例
ALTER TABLE student RENAME TO stu;
```

##### **修改列属性**

```sql
# 语法
ALTER TABLE 表名 MODIFY  列名 列类型 列属性

# 示例
ALTER TABLE student MODIFY sid TINYINT UNSIGNED;
```

##### **修改列名称和属性**

```sql
# 语法
ALTER TABLE 表名 CHANGE 旧列名 新列名 列类型 列属性

# 示例
ALTER TABLE student CHANGE sid id INT UNSIGNED;
```

##### **添加主键约束**

```sql
# 语法
ALTER TABLE 表名 ADD CONSTRAINT PRIMARY KEY 表名(列名)

# 示例
ALTER TABLE student ADD CONSTRAINT PRIMARY KEY student(sid);
```

##### **删除主键约束**

```sql
# 语法
ALTER TABLE 表名 DROP PRIMARY KEY

# 示例
ALTER TABLE student DROP PRIMARY KEY;
```

##### **删除查询缓存**

```sql
# 语法
RESET QUERY CACHE
```

###  列的属性

#### **列的默认属性**

实际使用中避免列的默认值为null，创建表时给列一个初始值，在列的类型后面添加NOT NULL DEFAULT值，示例：id INT UNSIGNED NOT NULL DEFAULT 0

#### **主键与自增**

主键(primary key)，能够区分每一行，一般和auto_increment一起使用，有两种方式声明该列为主键：

- 在列的类型之后声明primary key，例如：id INT UNSIGNED PRIMARY KEY
- 声明完列之后，在最后声明那一列为主键primary key(列名)，PRIMARY KEY(id)

主键与自增搭配使用，例如：id INT PRIMARY KEY AUTO_INCREMENT

提高效率建表原则：定长与变长分离，常用和不常用分离。

### 表引擎

MySQL常用两种表引擎是Myisam和InnoDB，引擎不同，组织数据方式也不同，新版本MySQL可以统一使用InnoDB，性能比Myisam差别不是太大。

```sql
CREATE TABLE account(
  id INT UNSIGNED PRIMARY KEY,
  name CHAR(10) NOT NULL DEFAULT '',
  balance INT NOT NULL DEFAULT 0
)ENGINE innodb CHARSET utf8;
```

## 增删改查

###  insert 插入数据操作

语法：

- 往哪张表添加行：INSERT INTO 表名
- 给那几个列添加值：列名称(可省略)
- VALUES：列对应的值

插入操作时注意事项：列和值必须一一对应，并且符合类型要求。

```sql
# user表有id、name、age三列

INSERT INTO user (id, name, age) VALUES (1, '张三', 25);
INSERT INTO user (name, age) VALUES ('李四', 26);
INSERT INTO user (name) VALUES ('王五');

# 忽略列时需要写完所有对应列的值
INSERT INTO user VALUES (5, '刘备', 52);
# 列没有严格对应，执行错误
#INSERT INTO user  VALUES ('关羽',45);

# 一次插入多行数据
INSERT INTO user (name, age) VALUES ('关羽', 45),('张飞', 46);
```

### delete 删除数据操作

语法：

- 删除一张表的数据：DELETE FROM 表名
- 删掉表中的哪些行：WHERE 表达式

注意事项：删除必须写where约束条件，也不能写常量，如where 1，否则会删除整张表数据。

```sql
# user表有id、name、age三列

DELETE FROM user WHERE age=26;
DELETE FROM user WHERE uid>2;
```

### update 修改数据操作

语法：

- 改哪一张表：UPDATE 表名
- 改哪几列的值：SET 列名=值1，列名=值2 ……
- 在哪些行生效：WHERE 表达式

注意事项：一定要有where约束条件，即在哪些行生效。

```sql
# user表有id、name、age三列

UPDATE user SET age=27 WHERE name='王五';

# 修改多列用逗号隔开
UPDATE user SET name='赵六',age=28 WHERE uid=4;
```

### select 查询数据操作

语法：

- 查询哪些列数据：SELECT列名1 列名2 ……
- 从哪张表查询：FROM 表名
- 选择哪些行生效：WHERE 表达式

```sql
# user表有id、name、age三列

SELECT * FROM user; # 实际开发中很少使用
SELECT * FROM user WHERE name='关羽';
SELECT name,age FROM user WHERE age<30; # 查询符合条件的指定列
```

## 综合查询

MySQL的比较运算和逻辑运算符：

| 比较运算符号    | 描述                                                         |
| :-------------- | :----------------------------------------------------------- |
| =               | 等于                                                         |
| <>, !=          | 不等于                                                       |
| >               | 大于                                                         |
| <               | 小于                                                         |
| <=              | 小于等于                                                     |
| >=              | 大于等于                                                     |
| BETWEEN         | 在两值之间 >=min&&<=max                                      |
| NOT BETWEEN     | 不在两值之间                                                 |
| IN              | 在集合中                                                     |
| NOT IN          | 不在集合中                                                   |
| <=, >=          | 严格比较两个NULL值是否相等 两个操作码均为NULL时，其所得值为1；而当一个操作码为NULL时，其所得值为0 |
| LIKE            | 模糊匹配                                                     |
| REGEXP 或 RLIKE | 正则式匹配                                                   |
| IS NULL         | 为空                                                         |
| IS NOT NULL     | 不为空                                                       |

| 逻辑运算符号 | 作用     |
| :----------- | :------- |
| NOT 或 !     | 逻辑非   |
| AND          | 逻辑与   |
| OR           | 逻辑或   |
| XOR          | 逻辑异或 |

注意事项：

- and比or的优先级高，当表达式比较复杂时，习惯使用括号，避免出现歧义。
- in用法：in的后面跟着枚举，例如in (3,11)
- between通常和 and连用，例如 between 100 and 500

示例表数据如下：

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200528090314.png)

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200528090343.png)

### where 条件约束

where是针对磁盘数据文件，后面一般都是列变量的表达式。模糊查询时，百分号%表示匹配所有字符，下划线_表示匹配一个字符。

```sql
# ⑴ 主键为32的商品
SELECT goods_id,goods_name,shop_price FROM goods WHERE goods_id=32;

# ⑵ 不属第3栏目的所有商品
SELECT cat_id,goods_name,shop_price FROM goods WHERE cat_id!=3;

# ⑶ 本店价格高于3000元的商品
SELECT goods_id,goods_name,shop_price FROM goods WHERE shop_price>3000;

# ⑷ 本店价格低于或等于100元的商品
SELECT goods_id,goods_name,shop_price FROM goods WHERE shop_price<=100;

# ⑸ 取出第4栏目或第11栏目的商品(不用or)
SELECT cat_id,goods_name,shop_price FROM goods WHERE cat_id IN (4,11);

# ⑹ 取出100<=价格<=500的商品(不许用and)
SELECT goods_id,goods_name,shop_price FROM goods WHERE shop_price BETWEEN 100 AND 500;

# ⑺ 不属于第3栏目且不属于第11栏目的商品(and,或not in分别实现)
SELECT cat_id,goods_name,shop_price FROM goods WHERE (cat_id!=3) && (cat_id!=11);
SELECT cat_id,goods_name,shop_price FROM goods WHERE cat_id NOT IN (3,11);

# ⑻ 取出价格大于100且小于300,或者大于3000小于4000商品
SELECT goods_id,goods_name,shop_price FROM goods WHERE (shop_price>100 && shop_price<300) || (shop_price>3000 && shop_price<4000);

# ⑼ 取出第3个栏目下面价格<1000或>3000,并且点击量>5的系列商品
SELECT cat_id,goods_name,shop_price,click_count FROM goods WHERE (cat_id=3) && (shop_price<1000 || shop_price>3000) && (click_count>=5);

# ⑽ 取出第1个栏目下面的商品(注意:第1栏目下面没商品,但其子栏目下有)
SELECT cat_id,goods_name,shop_price FROM goods WHERE cat_id IN (2,3,4,5);# 手动查看第1栏目的子栏目有2 3 4 5

# ⑾ 取出名字以"诺基亚"开头的商品
SELECT cat_id,goods_name,shop_price FROM goods WHERE goods_name LIKE '诺基亚%';

# ⑿ 取出诺基亚N系列的手机
SELECT goods_id,goods_name,shop_price FROM goods WHERE goods_name LIKE '诺基亚N__';

# ⒀ 取出名字不以"诺基亚"开头的商品
SELECT goods_id,goods_name,shop_price FROM goods WHERE goods_name NOT LIKE '诺基亚%';

# ⒁ 取出第3个栏目下面价格在1000到3000之间,并且点击量>5 的"诺基亚"开头的商品
SELECT cat_id,goods_name,shop_price,click_count FROM goods WHERE (cat_id=3) && (shop_price>1000 && shop_price<3000) && (click_count>5) && (goods_name LIKE '诺基亚%');

# ⒂ 把num值处于[20,29]之间,改为20，num值处于[30,39]之间的,改为30
UPDATE mian SET num=floor(num/10)*10 WHERE num BETWEEN 20 AND 39;

# ⒃ 把good表中商品名为'诺基亚xxxx'的商品,改为'HTCxxxx'
UPDATE goods SET goods_name=INSERT(goods_name,1,3,'HTC') WHERE goods_name LIKE '诺基亚%'; # 注：字符串位置从1开始
```

### group 数据分组

使用group时首先会对该列重新排序，再做统计，所以比较耗资源，尽量避免使用。

```sql
# ⑴ 查出最贵和最便宜的商品的价格
SELECT max(shop_price),min(shop_price) FROM goods;

# ⑵ 查出最新和最旧的商品编号
SELECT max(goods_id),min(goods_id) FROM goods;

# ⑶ 查询该店所有商品的库存总量
SELECT sum(goods_number) FROM goods;

# ⑷ 查询所有商品的平均价格
SELECT avg(shop_price) FROM goods;

# ⑸ 查询该店一共有多少种商品
SELECT count(*) FROM goods;

# ⑹ 查询每个栏目下面最贵商品价格、最便宜商品价格、商品平均价格、商品库存量、商品库存总价格、商品种类
SELECT max(shop_price),min(shop_price),avg(shop_price),sum(goods_number),count(*) FROM goods GROUP BY cat_id;
```

### having 筛选数据

having是针对where条件结果进行筛选，是对内存数据操作，所以having必须用在where之后。

```sql
# ⑴ 查询该店的商品比市场价所节省的价格
SELECT goods_name,market_price-shop_price FROM goods;

# ⑵ 查询每个商品所积压的货款
SELECT goods_name,shop_price*goods_number FROM goods;

# ⑶ 查询该店积压的总货款
SELECT sum(shop_price*goods.goods_number) FROM goods;

# ⑷ 查询该店每个栏目下面积压的货款
SELECT cat_id,sum(shop_price*goods_number) FROM goods GROUP BY cat_id;

# ⑸ 查询比市场价省钱200元以上的商品及该商品所省的钱
SELECT goods_id,goods_name,(market_price-goods.shop_price) AS save FROM goods HAVING save>200;

# ⑹ 查询积压货款超过2W元的栏目,以及该栏目积压的货款
SELECT cat_id,sum(shop_price*goods.goods_number) AS catPrice FROM goods GROUP BY cat_id HAVING catPrice>20000;

# ⑺ 查询出2门及2门以上不及格者的平均成绩
  成绩表result数据如下：
  +------+---------+-------+
  | name | subject | score |
  | 张三 |  数学   |  90    |
  | 张三 |  语文   |  50    |
  | 张三 |  地理   |  40    |
  | 李四 |  语文   |  55    |
  | 李四 |  政治   |  45    |
  | 王五 |  政治   |  30    |
  +------+---------+-------+
  SELECT name,avg(score),sum(score<60) AS fail FROM result GROUP BY name HAVING fail>=2;
```

### order by 排序和limit限制取出条目

当按某列排序(默认asc升序，desc降序)无法满足要求时，可以在列的内部再继续排序。实际使用中oder by经常和limit配合一起使用。 limit有两个参数，分别是limit 偏移量，取出条目，可以使用在分页上。

```sql
# (1)按价格由高到低排序
SELECT goods_id,goods_name,shop_price FROM goods ORDER BY shop_price ASC;

# (2)按发布时间由早到晚排序
SELECT goods_id,goods_name,shop_price FROM goods ORDER BY goods_id DESC;

# (3)接栏目由低到高排序,栏目内部按价格由高到低排序
SELECT cat_id,goods_name,shop_price FROM goods ORDER BY cat_id ASC, shop_price DESC;

# (4)取出价格最高的前三名商品
SELECT goods_id,goods_name,shop_price FROM goods ORDER BY shop_price DESC LIMIT 0,3;

# (5)取出点击量前三名到前5名的商品
SELECT goods_name,shop_price,click_count FROM goods ORDER BY click_count DESC LIMIT 0,5;
```

### 子查询

#### **where子查询**

内层select查询的结果充当外层select的where条件的查询。

```sql
# 取出每个栏目下最新的商品
SELECT cat_id,goods_id,goods_name,shop_price FROM goods WHERE goods_id IN (SELECT max(goods_id) FROM goods GROUP BY cat_id);
```

#### **from子查询**

内层select作为一张表(AS table)，外层select从内层表取出数据。

```sql
# 取出每个栏目下最新的商品
SELECT cat_id,goods_id,goods_name,shop_price FROM (SELECT * FROM goods ORDER BY cat_id ASC,goods_id DESC) AS tmp GROUP BY cat_id;
```

#### **exists子查询**

exists指定一个子查询，检测行的存在。该子查询实际上并不返回任何数据，而是返回值True或False。exists子查询能完成的where子查询 in也能完成。

```sql
# 取出没有商品的栏目
SELECT * FROM category WHERE NOT exists(SELECT * FROM goods WHERE category.cat_id=goods.cat_id);
```

### **子查询练习**

```sql
# 查询出最新一行商品
SELECT goods_id,goods_name,shop_price FROM goods WHERE goods_id=(SELECT max(goods_id) FROM goods);

# 查询出编号为19的商品的栏目名称(where和连接查询)
where子查询：
SELECT cat_id,cat_name FROM category WHERE cat_id=(SELECT cat_id FROM goods WHERE goods_id=19);
连接查询：
SELECT category.cat_id,category.cat_name FROM 
  category INNER JOIN goods ON category.cat_id=goods.cat_id 
WHERE goods.goods_id=19;

# 用where和from型子查询方式把每个栏目下面最新的商品取出来
where子查询：
SELECT goods_id,cat_id,goods_name FROM goods WHERE goods_id IN
(SELECT max(goods_id) FROM goods GROUP BY cat_id);
from子查询：
SELECT goods_id,cat_id,goods_name FROM 
(SELECT * FROM goods ORDER BY cat_id ASC,goods_id DESC) AS tmp GROUP BY tmp.cat_id;

# 用exists型子查询,查出所有有商品的栏目
SELECT * FROM category WHERE exists(SELECT * FROM goods WHERE category.cat_id=goods.cat_id);
```

## join连接查询

#### **INNER JOIN内连接查询**

把两张独立代表拼接成一张大表，拼接时指定匹配条件。

```sql
# 语法：
表1 INNER JOIN 表2 ON 匹配条件

# 把boy表和girl表同一组的人取出来
SELECT * FROM boy INNER JOIN girl ON boy.hid=girl.hid;
```

#### **LEFT JOIN左连接查询**

以左边的表为标准，有匹配条件取出值来，没有则填充默认值。

```sql
# 以boy表为标准，把boy表和girl表进行同组匹配
SELECT * FROM boy LEFT JOIN girl ON boy.hid=girl.hid;
```

#### **RIGHT JOIN右连接查询**

以右边的表为标准，有匹配条件取出值来，没有则填充默认值。

```sql
# 以girl表为标准，把boy表和girl表进行同组匹配
SELECT * FROM boy RIGHT JOIN girl ON boy.hid=girl.hid;
```

#### **连接查询练习**

```sql
# 取出所有商品的商品名,栏目名,价格
SELECT goods.goods_id,goods.goods_name,category.cat_name,goods.shop_price FROM goods LEFT JOIN category ON goods.cat_id=category.cat_id;

# 取出第4个栏目下的商品的商品名,栏目名,价格
SELECT goods.goods_id,goods.goods_name,category.cat_name,goods.shop_price FROM goods LEFT JOIN category ON goods.cat_id=category.cat_id WHERE goods.cat_id=4;

# 查出 2006-6-1 到2006-7-1之间举行的所有比赛，并且用以下形式列出：主队名  比分 客队 比赛时间
表名：m
+-----+------+------+------+------------+
| mid | hid  | gid  | mres | matime     |
+-----+------+------+------+------------+
|   1 |    1 |    2 | 2:0  | 2006-05-21 |
|   2 |    2 |    3 | 1:2  | 2006-06-21 |
|   3 |    3 |    1 | 2:5  | 2006-06-25 |
|   4 |    2 |    1 | 3:2  | 2006-07-21 |
+-----+------+------+------+------------+

队名：t
+------+----------+
| tid  | tname    |
+------+----------+
|    1 | 国安     |
|    2 | 申花     |
|    3 | 公益联队 |
+------+----------+

# 写法一：
SELECT ttb.tname,ttb.mres,t.tname,ttb.matime FROM (SELECT * FROM m LEFT JOIN t ON m.hid=t.tid)AS ttb LEFT JOIN t ON ttb.gid=t.tid WHERE ttb.matime BETWEEN '2006-06-01' AND '2006-07-01';

# 写法二：
SELECT t1.tname,mres,t2.tname,matime FROM m LEFT JOIN t AS t1 ON m.hid = t1.tid LEFT JOIN t AS t2 ON m.gid = t2.tid WHERE matime BETWEEN '2006-06-01' AND '2006-07-01';
```

## union查询

把两条或多条sql查询的结果合并成一个结果集。主要用途：两张不同的表中有相同的列时，把结果集合并，或者简化两个复杂的where条件， 使用条件：两个表的列必须相同，但是例外的是列名称可以不相同。

注意事项：

- 完全相等的行将会合并，合并是耗时的操作，一般不让union合并，使用union all则不合并。
- union子句中一般不用order by，在union合并后可以再order by。

```sql
# (1) 合并表a和表b，不合并相同的行
SELECT * FROM a UNION ALL SELECT * FROM b;

# (2) 对两个表相同id的行进行num列求和。
SELECT tmp.id,sum(tmp.num) FROM
(SELECT * FROM a UNION ALL SELECT * FROM b) AS tmp
GROUP BY tmp.id;
```

## 索引

### 索引长度

对于定长数据类型(char，int，datetime)，如果有是否为NULL的标记，这个标记需要占用1个字节。

对于变长数据类型(varchar)，除了是否为NULL的标记外，还需要有长度信息，需要占用2个字节，当字段定义为NOT NULL的时候，是否为NULL的标记将不占用字节。

不同的字符集： - latin1编码一个字符一个字节； - gbk编码的为一个字符2个字节； - utf8编码的一个字符3个字节。

创建索引的时候可以指定索引的长度，例如：

> alter table test add index uri(uri(30));

长度30指的是字符的个数，如果为utf8编码varchar(255)，key_length=30*3+2=92个字节。

对于多数名称的前10个字符通常不同的列，用前10个字符作为索引不会比使用列全名创建的索引速度慢很多。另外，使用列的一部分创建索引可以使索引文件大大减小，从而节省了大量的磁盘空间，有可能提高INSERT操作的速度。

### 单列索引

```sql
# 普通索引
key 列名(列名)
key cat_id(cat_id)

# 唯一索引
unique key
unique key email(email)

# 主键索引，一张表只能存在一个
primary key (列名)
primary key (id)

# 全文索引 fulltext
# 在中文环境下几乎无效。一般用第三方解决方案sphinx
```

索引长度：建索引时，可以取列的部分字符作为索引，节省空间， 例如：unique key email(email(10)); 取email列的前10个字符作为索引。

### 多列索引

把两列或多列的值作为整体后再建索引，左前缀规则,例如key xm(xing, ming)

```sql
CREATE TABLE name (
  xing CHAR(2),
  ming CHAR(10),
  KEY xm(xing,ming)
);

# 在select前面添加explain关键字，得到结果的possible_key可以查看是否使用到索引查询。
# 使用索引xm查询
EXPLAIN SELECT * FROM name WHERE xing='刘' AND ming='备';

# 使用索引xm查询
EXPLAIN SELECT * FROM name WHERE xing='刘';

# 没有使用索引xm查询
EXPLAIN SELECT * FROM name WHERE ming='备';
```

### 冗余索引

在某个列上可能存在多个索引,比如某个表：key xm(xing, ming)，key ming(ming)，对于列ming来说，索引xm和ming两个索引覆盖，叫做冗余索引。

### 索引操作

```sql
# 查看索引
SHOW INDEX FROM 表名;

# 删除索引
ALTER TABLE 表名 DROP INDEX 索引名;

# 添加索引
# 添加普通索引
ALTER TABLE 表名 ADD INDEX 索引名(列名...);
# 添加唯一索引
ALTER TABLE 表名 ADD UNIQUE 索引名(列名);
# 添加主键索引
ALTER TABLE 表名 ADD PRIMARY KEY(列名);
```

## 事务

### 事务特性(ACID)

- 原子性(Atomicity）：是指某几句sql的影响，要么都发生，要么都不发生。
- 一致性(Consistency)：事务前后的数据，保持业务上的合理一致。
- 隔离性(Isolation)：在事务进行过程中，其他事务看不到此事务的任何效果。
- 持久性(Durability)：事务一旦发生，不能取消，只能通过补偿性事务来抵消效果。

事务与引擎：myisam引擎不支持事务，innodb和BDB引擎支持。

一个完整的事务过程：

- (1) 启动事务：START TRANSACTION;
- (2) sql执行：增删改查，如果出错，回滚ROLLBACK
- (3) 结束事务：COMMIT(提交)或ROLLBACK(取消);

注意：commit一旦发生之后，rollback无法回滚。

```bash
# 示例
START TRANSACTION;
UPDATE account SET balance=balance+1000 WHERE name='曹操';
UPDATE account SET balance=balance-1000 WHERE name='刘备';
COMMIT;
```

### 设置事务级别

set session transaction isolation level [read uncommitted | read committed | repeatable read | serializable]

- read uncommitted：可以读未提交的事务内容，称为”脏读”，破坏了事务的隔离性，一般不用。
- read commited：在一个事务进行过程中，读不到另一个进行事务的操作，但是可以读到另一个结束事务的操作影响，一般不用。
- repeatable read：在一个事务过程中，所有信息都来自事务开始那一瞬间的信息，不受其他已提交事务的影响，大多数的系统，用此隔离级别，建议使用。
- serializeable：串行化，把所有事务进行编号，按顺序一个一个来执行，也就取消了事务冲突的可能，隔离级别最高，但事务相互等待的时间会比较长，在实际中使用也不是很多。

## 用户与权限管理

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200531082101.png)

### 用户连接mysql服务器

mysql认证用户依据有3个参数：

- 你从哪里来：host
- 你是谁：user
- 你的密码是多少：password

```bash
# 连接数据库
mysql -h192.168.8.102 -uroot -p123456;

# 查看当前登录用户
SELECT user();

# 通过库名mysql里的user表来查看有哪些用户可以登录
USE mysql;
DESC user;
SELECT host,user,password FROM user;

# 修改host域，指定IP能连接起来
UPDATE user SET host='192.168.8.101' WHERE host='::1';
FLUSH PRIVILEGES; # 冲刷权限

# 修改用户密码
UPDATE user SET password=password('123456') WHERE host='192.168.8.101';
FLUSH PRIVILEGES; # 冲刷权限
```

### 创建、授权和回收用户

用户常用权限all，creat，drop，insert，update，delete，select ……

```bash
# 创建一个新用户，注：如果是8.0版本以上，默认使用caching_sha2_password，有些客户端可能不支持
CREATE USER 'krislin'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
# 为新用户授权库.表
GRANT ALL ON *.* TO 'krislin'@'%';
FLUSH PRIVILEGES; # 冲刷权限

# 新建一个用户并授权
GRANT [权限1, 权限2......] ON 数据库名.该库下的表名 TO 用户名@主机名 IDENTIFIED BY 密码;

# 示例：
    # 授权给主机为192.168.8.n局域网内的用户root所有数据库权限
    GRANT ALL ON *.* TO root@'192.168.8.%' IDENTIFIED BY '123456';
    FLUSH PRIVILEGES; # 冲刷权限

    # 针对某个库做授权
    GRANT ALL ON test.* TO root@'192.168.8.%';
    FLUSH PRIVILEGES; # 冲刷权限

    # 针对某个表授权
    GRANT select,update,insert ON test.goods TO root@'192.168.8.%';
    FLUSH PRIVILEGES; # 冲刷权限


# 收回用户权限
REVOKE [权限1, 权限2 ......] ON 数据库名.该库下的表名 FROM 用户名@主机名;

# 示例：
    # 收回某用户的所有权限
    REVOKE ALL ON *.* FROM root@'192.168.8.%';
```

### 跳过mysql认证登录方法

有时候忘记mysql帐号密码或修改了主机ip使得无法登录mysql， 解决办法：

```bash
# (1) 关闭mysql服务
/etc/init.d/mysql stop

# (2) 启动mysql服务时添加跳过权限检测
/usr/local/mysql/bin/mysqld --skip-grant-tables

# (3) 在新的终端登录mysql修改数据库mysql下user表的host、user、name列
/usr/local/mysql/bin/mysql
  > use mysql;
  > select host,user,password from user;
  > update user set host='ip地址',user='用户名' password=password('你的密码') where 定位那一行;

# 最后重启mysql服务。
```

## 优化

**表的优化**

列选择原则：

**字段类型优先选择顺序**

> 整型 > date、time > char 、varchar > blob

因为整形、time类型运算快又节省空间；char、varchar要考虑字符集的转换与排序时的校对集(a B排序哪个优先)、速度慢；blob无法使用内存临时表，设计到排序必须在硬盘完成，速度慢。

**字段长度够用就行(tinyint、 varchar(N))**

因为大的字段浪费内存，影响速度，例如某字段varchar(30)能存下的内容，如果用varchar(100)的话，在联表查询时，varchar(100)要花跟多内存。

**避免使用null**

因为null不利于索引，要用多一个字节来特殊标注该字段值是否为null，在磁盘上占据多一个字节。一般声明字段时都带有not null define属性

**对于字段属性分类数量确定而且数量比较少时，优先选择枚举类型enum，例如性别、学历等**

enum列在内部是用整型来存储的

- 优点：当一个表enum列与另一个表的enum相关联时速度最快，当enum成员是char类型，并且字节比较多时，enum依然时整型存储，可以节省IO。
- 缺点：在碰到与char关联时，需要转化消耗点时间，速度要比enum<->enum和char<->char要慢。

索引优化

理想索引：

- 查询频繁
- 区分度高
- 长度小
- 尽量能覆盖常用查询字段

### B-tree索引

B-tree是种常见的数据结构。可以显著减少定位记录时所经历的中间过程，从而加快存取速度。一般用于数据库的索引，综合效率较高，常被用于对检索时间要求苛刻的场合。B-tree可以理解为已经排好序的快速查找数据结构。

btree的左前缀规则：

- 按f1、f2……建立的复合索引，在where条件中，按f1、f2……由左到右的顺序（当and时不用按顺序），索引才会发挥作用。
- 如果中间某列没有条件或like条件，导致后面的列，索引用不上。
- 索引也能用于排序和分组，因为分组要先排序后计算。所以我们的order by或group如果能针对有顺序的表进行，可以避免临时表和文件排序。也就是说我们的order by或group按顺序使用索引的列，则可发挥索引的作用。

B-tree索引的误区：

在表中的常用的列都独立加上索引就以为常用的查询都完全用到索引了。例如user表中的gender和age列都独立加上索引，查询出表中大于60岁的女性的语句。

> select * from user where gender=‘女’ and age>60;

注意：上面语句只能用上gender或age索引，因为是独立索引，同时只能用上一个，上面sql查询要完全使用索引，需要建立多列索引，例如：index(gender,age)

### hash索引

哈希索引包含以数组形式组织的Bucket集合。哈希函数将索引键映射到哈希索引中对应的Bucket，使用哈希索引必须要使用哈希集群。哈希索引可能是访问数据库中数据的最快方法(时间复杂度为O(1))，但它也有自身的缺点，只支持等值计算，不支持范围搜索或排序。

hash索引查找速度最快，为什么不用hash索引呢？

- 哈希函数计算的结果是随机的，如果在磁盘上随机放置数据，hash找到数据在磁盘位置很快，但是磁盘随机读取数据却很慢。
- hash无法对范围查询进行优化
- hash无法利用前缀索引，不能前缀优化。例如在mysql查找列值为zhangsan，B-tree索引支持’zhang’或’zhangsan’关键字索引，而hash必须为’zhangsan’全部搜索。
- 排序也无法优化

B-tree索引和hash索引区别：

- hash索引查找数据基本上能一次定位数据，当然有大量碰撞的话性能也会下降。而btree索引就得在节点上挨着查找了，很明显在数据精确查找方面hash索引的效率是要高于btree的。
- 那么不精确查找呢，也很明显，因为hash算法是基于等值计算的，所以对于“like”等范围查找hash索引无效，不支持。
- 对于btree支持的联合索引的最优前缀，hash也是无法支持的，联合索引中的字段要么全用要么全不用。
- hash不支持索引排序，索引值和计算出来的hash值大小并不一定一致。

### 聚簇索引

聚簇索引：索引和数据混合一起，如InnoDB的id和数据是混合在一起的。有个缺点，当数据文件比较大时，查询数据时不断翻越磁盘扇区的页，使用时间会比较长。

回表：从索引定位到获取磁盘数据的过程，索引查找是快的，回表取数据是慢的。如果能直接从索引中获取数据，将会省去回行过程，提高查询速度，因为索引在内存运行的。

索引覆盖：查询的列恰好时索引的一部分，只在索引就能获取想要的数据，不需要回表到磁盘取数据。如果查询的数据能用到索引覆盖，速度是最快的。用explain 查询语句，看extra项目是否有Using index，如果有则使用到索引覆盖。

MyISAM的索引特点：指向的是数据在磁盘上的位置。

InnoDB的索引特点：

- 主键作为索引，既储存索引值又储存行数据。
- 次级索引是先指向主键id，然后再从id获取行数据，也就是说InnoDB的索引是间接获取数据，中间多了从id获取数据的过程。
- 如果没有主键系统把unique key作为主键，如果也没有unique key，系统内部自动生成row id作为主键。

主键值为随机的插入行数据时，主键节点会分裂，对于MyISAM类型的表，影响不是很大，因为节点的包括的内容比较小(只有指向磁盘地址)，在内存里完成，转移数据时耗时小；对于InnoDB类型的表影响比较大，因为表使用的聚簇索引，每个节点都包括行内容，节点分裂时需要转移的数据比较多，耗时也比较多。

高性能索引是使用自动递增的整型，例如定义主键类型id INT UNSIGNED PRIMARY KEY AUTO_INCREMENT; 插入数据时不用指定主键值，让其有规律的自动增加，减少节点分裂的耗时。

聚簇索引排序慢原因分析

```bash
# 有一个表结构和引擎如下，有100000行数据。

# 创建表
CREATE TABLE ts(
    id CHAR(30) PRIMARY KEY AUTO_INCREMENT,
    val INT NOT NULL DEFAULT 0,
    str1 VARCHAR(2000),
    str2 VARCHAR(2000),
    KEY idval(id,val)
)ENGINE innodb CHARSET utf8;

# 查询速度慢
SELECT id FROM ts ORDER BY id;

# 查询速度快
SELECT id FROM ts ORDER BY id,val;
```

查询速度慢的原因：表驱动为InnoDB，主键索引id属于聚簇索引，每个节点包含一行数据内容，而且字段内容比较大，储存时超越磁盘最小块，对主键id排序时会跨很多磁盘的数据块，导致速度很慢。

对联合索引排序快的原因：因为联合索引不是聚簇索引，节点内容很小(只有指向主键id地址)，联合排序时使用了索引覆盖，不需要回行取数据，而且是在内存中完成，索引速度比较快。

当把str1和str2内容比较大的列删除后，两条语句执行速度差别不大。 如果表类驱动型为MyISAM两条语句执行速度差别不大。

### 多列索引

在建立多列索引后，必须满足左前缀要求(从左到右按顺序，中间不能断开)，索引才能发挥作用，例如多列索引index(a,b,c)

| sql的where语句                    | 索引是否发挥作用  |
| :-------------------------------- | :---------------- |
| where a=1                         | √ 使用a列         |
| where a=1 and b=2                 | √ 使用a、b列      |
| where a=1 and b=2 and c=3         | √ a、b、c列都使用 |
| where b=2                         | ×                 |
| where c=3                         | ×                 |
| where a=1 and c=3                 | ×                 |
| where b=2 and c=3                 | ×                 |
| where a=1 and b>2 and c=3         | √使用a、b列       |
| where a=1 and b like ‘%2’ and c=3 | √ 使用a、b列      |

使用多列索引误区：查询哪个列索引都会发挥作用。

注意：多列索引一定要结合业务逻辑进行优化，例如查100~200元的男装商品，这条查询涉及到价格和商品栏目两列，可以把这两列作为一个多列索引。

### 伪哈希索引

为了避免hash整个字段值，可hash字段中的部分具有区分度的长度，从而大大减少索引的长度，提高查询速度。

### 延时索引

用大量数据分页优化说明延时索引技巧。 例如显示搜索结果有100000条，分页显示每页20条，sql语句为 SELECT filed FROM table LIMIT (N-1)*20,20;其中N表示第几页。

- 优先从业务逻辑优化，条件是限制分页数量，也就是说搜到100000条结果，给用户显示最多是40页就以及满足客户需求了，查询时间是ms级别。

为什么要限制N的大小呢？limit offset,num的工作机制是先查询，然后再跳过offset获取num条数据。

- 如果不允许从业务逻辑优化，还有个办法就是利用主键id作为查找条件再筛选结果，大量数据查询时间是ms级别。例如sql语句：SELECT filed FROM baidu WHERE id>100000 LIMIT 0,20;

这种优化查询用到索引所以速度也很快，但是条件是要求数据完整性，不允许删除数据。否则会造成每次查询结果不一致，解决办法时不进行物理删除，用逻辑标记删除，最终在页面上显示时，逻辑删除的条目不显示结果即可。

- 延时索引优化，这种方法没有条件限制，大量数据查询时间为秒级别，例如语句SELECT filed FROM table WHERE id IN (SELECT id FROM table WHERE id>200000 LIMIT (N-1)*20,20);

首先查找取出主键id，然后再通过id来取数据，这种通过索引中间过程再从表中取数据过程叫延时索引，延时索引的好处时，节省了大量的回行时间(sending data时间)，提高查询速度。

### 冗余索引

在某个列上可能存在多个索引，例如：

```sql
CREATE TABLE user(
  id INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  first_name CHAR(5) NOT NULL DEFAULT '',
  last_name CHAR(10) NOT NULL DEFAULT '',
  KEY first_name(first_name),
  KEY last_name(last_name),
  KEY full_name(first_name,last_name)
)ENGINE myisam CHARSET utf8;
```

表中的列first_name既有独立作为索引，也有联合索引full_name，这叫冗余索引，实际中也会用到，单列查询时可以用到索引，多列查询时也可以用到索引。

注意index AB(A,B)和index BA(B,A)是不同的索引，对于列A或B来说，AB和BA属于冗余索引。

### 索引的长度和区分度

对于一般系统，区别度能达到10%时，索引性能就可以接受。 针对列中的值，从左到右截取来建立索引：

- 截得越短，重复度越高，区分度越低，索引效果越不好。
- 截得越长，重复度越低，区分度越高，索引效果越好，但是带来负面影响越大，占有空间越大，也会减慢查询速度，增删改操作变慢。

### 索引与排序

对于带有排序的sql语句，例如下面语句，可能发生2种情况。

> SELECT * FROM goods WHERE cat_id=3 ORDER BY shop_price DESC;

- 跳过排序过程，直接取出最终结果，查询速度快。要达到这个结果，必须事先建立索引，利用索引本身有序的特点，取出来就是有顺序的，所以省略了排序过程，达到查询速度快的效果。
- 没有索引，先从表中取出数据作为临时表，然后再对临时做排序(在内存或磁盘排序)，排序是比较耗时的，因尽量避免，所以查询速度会比较慢。

在有排序的查询时，排序的列尽量作为索引(或联合索引)，目的是取出来的数据本身就是有顺序的，避免排序这个耗时的过程，从而提高查询速度。

## 优化分析

**获取查询和连接数量**

```bash
# 在mysql命令行查看服务器状态
SHOW STATUS;

# 在linux命令行查看服务器状态
mysqladmin -u root -p 123456 ext

# 在linux命令行获取上面三个参数值：
mysqladmin -u root -p 123456 ext | awk '/Queries/{q=$4} /Threads_connected/{c=$4} /Threads_running/{r=$4} END{printf "%d %d %d\n", q, c, r}'
```

获得mysql服务器状态数据，主要看3个参数：

- Queries: 查询次数
- Threads_connected: 线程连接数
- Threads_running: 线程运行数

一般使用脚本循环获取mysql状态，脚本内容如下：

```bash
#!/bin/bash

while true
do
   mysqladmin -uroot ext | awk '/Queries/{q=$4} /Threads_connected/{c=$4} /Threads_running/{r=$4} END{printf "%d  %d  %d\n", q,c,r}' >> status.txt
   sleep 1
done
```

**mysql进程状态分布**

```bash
# 在mysql命令行查看服务器状态
SHOW PROCESSLIST;

# 在linux命令行查看服务器状态：
mysql -uroot -p123456 -e 'show processlist \G'

# 如果只关心State状态这行，在linux下执行命令获取State
mysql -uroot -p123456 -e 'show processlist \G' | grep State
```

一般使用脚本循环获取mysql进程状态，脚本内容如下：

```bash
#!/bin/bash

while true
do
    mysql -uroot -e 'show processlist \G' | grep State >> process.txt
    usleep 100000
done
```

**使用profile记录各个sql执行时间**

```bash
# (1) 打开profile功能，打开profile功能后，执行的每一条sql语句都会被记录下来。
SET PROFILING =1;

# (2) 查看执行所有sql耗时时间列表(在phpstorm可以按使用时间排序)，可以找到有问题的语句(耗时比较大的)
SHOW PROFILES;

# (3) 查看sql语句详细执行过程，可以分析该语句耗时都用在哪个状态下，其中数字72是在SHOW PROFILES命令显示的查询id。
SHOW PROFILE for query 72;

# (4) 关闭profile功能：
SET PROFILING =0;
```

**使用explain分析sql执行效果**

explain分析结果字段说明：

| 属性名        | 说明                                                         |
| :------------ | :----------------------------------------------------------- |
| id            | 查询的编号，简单查询通常为1，如果有子查询会增加子查询行编号  |
| select_type   | (1) simple：不含子查询的简单的查询； (2) primary：含有子查询或派生查询， (subquery表示非from子查询)、(derived表示from型子查询)、 (union)、(union result) |
| table         | 查询针对的表名，如果原表名加了别名，就显示表的别名，也有可能是null |
| type          | (1) All: 全表扫描，在磁盘全表扫描是非常慢的，尽量避免出现全表扫描这样的查询； (2) index: 扫描索引的所有节点，因为在内存扫描，性能比All要好，虽然说尽量使用索引，但不希望从头到尾扫描一遍； (3) range: 根据索引做范围扫描，比index性能好； (4) ref: 通过索引列直接引用到某些数据行，比range性能好； (5) eq_ref: 从表中读取一行，在联表查询使用到索引时经常出现，比ref性能好； (6) const |
| possible_keys | 可能用到的索引                                               |
| key           | 最终使用的索引                                               |
| key_len       | 索引的长度，长度越短越好。                                   |
| ref           | 引用了哪索引或列                                             |
| rows          | 估计扫描的行                                                 |
| Extra         | (1) using index：使用索引覆盖； (2) using where：使用了条件查询； (3) using temporary：使用临时表，尽量不出现； (4) using filesort：使用了排序，尽量不出现； (5) range checked for each record：使用了在范围检查每个记录，不要出现扫描的行 |

使用：

explain命令+耗时比较多sql查询语句可以查看该语句是否使用了索引，使用索引说明已经优化过，没有使用索引则对表进行优化。

```bash
# 查看有索引的查询
EXPLAIN SELECT * FROM plan_task WHERE id=100000;
# 从结果可以看出已经使用了索引，type为const，extra为null。

# 查看没有索引的查询
EXPLAIN SELECT * FROM plan_task WHERE background_color=100000;
# 从结果看出没有使用到索引，type为ALL，extra为using where。
```

**开启慢查询日志**

(1) 开启慢查询日志有两种，一是全局变量设置，二是设置配置文件。

全局变量设置：

```bash
# 将slow_query_log全局变量设置为“ON”状态
mysql> set global slow_query_log='ON';

# 设置慢查询日志存放的位置
mysql> set global slow_query_log_file='/usr/local/mysql/data/slow.log';

# 查询超过1秒就记录
mysql> set global long_query_time=1;
```

修改配置文件my.cnf，在[mysqld]下的下方加入下面内容：

```bash
[mysqld]
slow_query_log = ON
slow_query_log_file = /usr/local/mysql/data/slow.log
long_query_time = 1
```

重启mysql服务：service mysqld restart

(2) 查看设置后的参数

```bash
show variables like 'slow_query%';
+---------------------+--------------------------------+
| Variable_name       | Value                          |
+---------------------+--------------------------------+
| slow_query_log      | ON                             |
| slow_query_log_file | /usr/local/mysql/data/slow.log |
+---------------------+--------------------------------+

show variables like 'long_query_time';
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| long_query_time | 1.000000 |
+-----------------+----------+
```

(3) 验证慢查询查日志是否开启成功

```bash
# 执行一条慢查询SQL语句
select sleep(2);

# 查看是否生成慢查询日志
cat /usr/local/mysql/data/slow.log
```

**索引碎片和维护**

索引碎片的形成原因：在长期的数据更改中，索引文件和数据文件都将会产生空洞，形成碎片，索引碎片会对查询速度有影响。

维护索引碎片有两种方法：

```bash
# 修改表的nop操作，这个操作不影响表数据
ALTER TABLE 表名 ENGINE 驱动名;

# 释放表空间
OPTIMIZE TABLE 表名

# 注意：当数据比较大时，维护是很耗时间的，通常在访问量比较少的夜里维护，当数据修改不频繁可以按年来做维护，如果数据修改比较多可以按月来修复。
```

## 表分区

表分区是把原来一张表按一定方式拆分为多个相同类型的表，同时原来一张表的数据文件也会拆分为多个数据文件，拆分方式可以按范围、散列点方式来分区。

表分区的好处：拆分后可以打开的线程数更多了，因为数据在不同的文件上，文件被锁的可能性会降低，一定程度上提高了读写速度。 

对用户使用来说，表有没有分区是无影响的，增删改查操作还是一样。

**按范围来分区**

```bash
# 例如有一张用户表，按主键id的范围对用户表分区
CREATE TABLE user(
  id INT UNSIGNED PRIMARY KEY AUTO_INCREMENT, # 主键
  name VARCHAR(20) NOT NULL DEFAULT '',   # 名称
  aid TINYINT UNSIGNED NOT NULL DEFAULT 0 # 地区代号
)ENGINE myisam CHARSET utf8
  PARTITION BY RANGE (id)(    # 按主键id范围对表进行分区
  PARTITION u0 VALUES LESS THAN (10), # 第1个分区范围
  PARTITION u1 VALUES LESS THAN (20), # 第2个分区范围
  PARTITION u2 VALUES LESS THAN (MAXVALUE)    # 最后一个分区范围
);
# 也可以按时间按范围来分表，例如年、月
```

**按散列点对表进行分区**

```bash
# 假如有一张地区表和一张用户表，按地区代号对用户表进行分区
# 地区表：
CREATE TABLE area(
  aid INT UNSIGNED PRIMARY KEY AUTO_INCREMENT, # 主键
  addr VARCHAR(10) NOT NULL DEFAULT '' # 地区名称，不要为null
)ENGINE myisam CHARSET utf8;

# 用户表：
CREATE TABLE user(
  id INT UNSIGNED PRIMARY KEY AUTO_INCREMENT, # 主键
  name VARCHAR(20) NOT NULL DEFAULT '',   # 名称
  aid TINYINT UNSIGNED NOT NULL DEFAULT 0 # 地区代号
)ENGINE myisam CHARSET utf8
  PARTITION BY LIST (aid)(    # 按散列点(地区代号)对表进行分区
  PARTITION a1 VALUES IN (1), # 按地区代号分区
  PARTITION a2 VALUES IN (2), # 按地区代号分区
  PARTITION a3 VALUES IN (3), # 按地区代号分区
  PARTITION a4 VALUES IN (4)  # 按地区代号分区
);
```

## 集群

**主从复制**

MySQL数据库自身提供的主从复制功能，可以方便的实现数据的多处自动备份，实现数据库的拓展。多个数据备份不仅可以加强数据的安全性，通过实现读写分离还能进一步提升数据库的负载性能。

在一主多从的数据库体系中，多个从服务器采用异步的方式更新主数据库的变化，业务服务器在执行写或者相关修改数据库的操作是在主服务器上进行的，读操作则是在各从服务器上进行。

MySQL之间数据复制的基础是二进制日志文件bin log。主数据库启用二进制日志后作为master，所有操作都会以“事件”的方式记录在二进制日志中。

slave数据库通过一个I/O线程与主服务器保持通信，并监控master的二进制日志文件的变化，会把变化复制到自己的中继日志中，然后slave的一个SQL线程会把相关的“事件”执行到自己的数据库中，以此实现从数据库和主数据库的一致性，也就实现了主从复制。

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200531091948.png)

配置主从复制流程：

- 主mysql配置binlog
- 从mysql器配置relaylog
- 主mysql授权给从mysql器一个帐号
- 从mysql连接主mysql

```bash
# (1) 修改主mysql配置文件
  # 打开配置文件
  sudo vim /etc/mysql/my.cnf
  #在[mysqld]字段下
    # ① 给主mysql起一个名字，在局域网内一般为ip最后一个数字
    server-id = 101
    # ② 开启logbin二进制日志，并给logbin起个名字
    log-bin = mysql_bin
    # ③ 指定日志格式，分别有mixed(由系统决定row还是statement)、row(记录磁盘变化)、statement(记录执行语句)，各有各的应用场景。
    binlog-format = mixed
    # ④ 重启mysql服务器
    sudo service mysql restart

# (2) 给从mysql添加一个授权帐号
GRANT REPLICATION CLIENT,REPLICATION SLAVE ON *.* TO 'repl'@'192.168.8.%' IDENTIFIED BY '123456';
FLUSH PRIVILEGES; # 冲刷权限

# (3) 查看mysql是否具备充当主mysql条件
SHOW MASTER STATUS;

# (4) 从mysql器也开启binlog
  # 打开配置文件
  vim /usr/local/mysql/my.cnf
  # 在[mysqld]字段下
    # ① 给从mysql起一个名字，防止多个主从混乱
    server-id = 102
    # ② 开启relaylog日志，并给relaylog起个名字
    relay-log=mysql_relay
    # ③ 重启mysql服务器：
    sudo service mysql restart

# (5) 在从mysql通过sql命令指定要复制的主mysql (可以一主多从，不能多主)
CHANGE MASTER TO
  MASTER_HOST='192.168.8.101',
  MASTER_USER = 'repl',
  MASTER_PASSWORD = '123456',
  MASTER_LOG_FILE = 'mysql_bin.000003', # 用语句SHOW MASTER STATUS;查看
  MASTER_LOG_POS = 120; # 用语句SHOW MASTER STATUS;查看

  # 配置连接信息后启动从mysql器功能
  START SLAVE;

  # 查看从mysql是否连接到主mysql
  SHOW SLAVE STATUS;

  # 如果有连接错误，ping下能否有网络连接，telnet 判断是否连接上，查看是否开启防火墙service iptables stop

  # 停止主从服务
  STOP SLAVE

# 注：主从复制的时间间隔一般为毫秒级别，达到秒级别的使用时风险比较大。
```

**主主复制**

所谓双主备份，其实也就是互做主从复制，每台master既是master，又是另一台服务器的slave。这样，任何一方所做的变更，都会通过复制应用到另外一方的数据库中。

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200531092528.png)

配置主主复制流程：

- 两台mysql都设置二进制和relay日志
- 两台mysql都设置replication帐号
- 两台mysql都设置对方为自己的master

修改两台主mysql配置文件

```bash
# (1) 编辑配置文件：sudo vim /etc/mysql/my.cnf
  # 在[mysqld]字段下
    # ① 给主mysql起一个名字，在局域网内一般为ip最后一个数字，注意两台主mysql名字不能一样。
    server-id = 101
    # ② 开启logbin二进制日志，并给logbin起个名字
    log-bin = mysql_bin
    # ③ 指定日志格式，分别有mixed（由系统决定row还是statement）、row（记录磁盘变化）、statement（记录执行语句），各有各的应用场景。
    binlog-format = mixed
    # ④ 开启relaylog日志，并给relaylog起个名字
    relay-log=mysql_relay
    # ⑤ 重启mysql服务器
    sudo service mysql restart

# (2) 给从mysql添加一个授权帐号，两个主mysql授权帐号可以相同。
GRANT REPLICATION CLIENT,REPLICATION SLAVE ON *.* TO 'repl'@'192.168.8.%' IDENTIFIED BY '123456';
FLUSH PRIVILEGES; # 冲刷权限

# (3) 查看两个mysql是否具备充当主mysql条件
SHOW MASTER STATUS;

# (4) 互相连接对方的mysql
CHANGE MASTER TO
  MASTER_HOST='192.168.8.101',
  MASTER_USER = 'repl',
  MASTER_PASSWORD = '123456',
  MASTER_LOG_FILE = 'mysql_bin.000002', # 从SHOW MASTER STATUS查看对方的状态得到
  MASTER_LOG_POS = 560; # 从SHOW MASTER STATUS查看对方的状态得到

CHANGE MASTER TO
  MASTER_HOST='192.168.8.102',
  MASTER_USER = 'repl',
  MASTER_PASSWORD = '123456',
  MASTER_LOG_FILE = 'mysql_bin.000005', # 从SHOW MASTER STATUS查看对方的状态得到
  MASTER_LOG_POS = 1212; # 从SHOW MASTER STATUS查看对方的状态得到

# (5) 配置连接信息后都启动从mysql器功能
START SLAVE;

# (6) 查看能否互相连接到mysql
SHOW SLAVE STATUS;
```

这样设置后会有个主键冲突问题，即(表里有主键，并且是自动增长的，当分别同时插入两台主mysql时，同步的时候就出现主键冲突问题)，解决办法：

```bash
# (1) 在mysql表设置自动增长步长为2，一个mysql从1开始自动增长1 3 5......，另一个从2开始自动增长2 4 6......
# 第一台mysql：
set global auto_increment_increment = 2;
set global auto_increment_offset = 1;
set session auto_increment_increment = 2;
set session auto_increment_offset = 1;

# 第二台mysql：
set global auto_increment_increment = 2;
set global auto_increment_offset = 2;
set session auto_increment_increment=2;
set session auto_increment_offset = 2;

#注：auto-increment-increment和auto-increment-offset要写到配置文件中，防止下次重启后失效．这种方式也有缺陷，当新增一台主机作为主mysql时，又要从新修改自动增长步长。

# (2) 从业务上统一主键id
# 比如使用redis的incr命令(自动加1操作)，每插入一行数据前先执行redis的incr命令获取主机id值，然后在插入数据到mysql。
```

**被动模式下主主复制**

被动模式下主主复制和普通主主复制区别就是设置一台主服务器为只读状态，也就是说把只读的主mysql作为另一个可读写主mysql的备份，当可读写的主mysql主机坏了，只需设置只读的主mysql配置为可读写就可以了，实现mysql无缝切换。

在主主配置的基础上，只需配置一台主mysql设置为只读。

```bash
# 打开一台主mysql配置文件
sudo vim /etc/mysql/my.cnf
# 在[mysqld]字段下添加字段
read-only=on

# 重启mysql
sudo service mysql restart
```

**mysql集群负载均衡**

通过mysql-proxy中间件来管理mysql集群，用户不需要知道集群中有多少个mysql服务器和地址，只需要连接mysql-proxy中间件，用户增删改查操作都是通过mysql-proxy中间件完成，mysql-proxy既可以mysql进行读写分离，也可以负载均衡，如下图所示：

![](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20200531093124.png)

```bash
# 安装mysql-proxy中间件
# 下载源码
wget http://ftp.ntu.edu.tw/pub/MySQL/Downloads/MySQL-Proxy/mysql-proxy-0.8.4-linux-glibc2.3-x86-64bit.tar.gz

# 解压
tar zxvf mysql-proxy-0.8.4-linux-glibc2.3-x86-64bit.tar.gz

# 把解压文件夹移动到常用的安装软件目录下并修改名字，
mv mysql-proxy-0.8.4-linux-glibc2.3-x86-64bit /usr/local/mysql-proxy
```

 **(1) mysql-proxy说明**

mysql-proxy启动参数思路： - 代理了哪个端口? - 代理了哪写mysql服务? - 对mysql是否进行读写分离?

mysql-proxy是自动负载均衡的，这里的均衡并不是sql语句上的均衡，而是mysql-proxy和用户连接上的均衡，例如当前有20台mysql服务器，有1000个用户连接过来，此时mysql-proxy会把1000个连接数平均分给20台mysql服务器，一旦用户连接上了一台mysql服务器，在用户没有断开连接之前，用户的增删改查或事务操作都是该mysql服务器操作的。

**(2) 启动mysql-proxy**

```bash
# 查看帮助
/usr/local/mysql-proxy/bin/mysql-proxy --help-all

# 普通启动
/usr/local/mysql-proxy/bin/mysql-proxy -P 192.168.8.102:4040 -b 192.168.8.101:3306 -b 192.168.8.102:3306
# -P 参数指定mysql-proxy运行的ip地址和端口
# -b 参数指定mysql服务器的ip地址和端口
# --daemon 表示在后台启动
```

**(3) 设置读写分离mysql启动方式**

```bash
# mysql-proxy通过一个脚本文件判断用户输入的sql是写入还是读取，脚本位置在/usr/local/mysql-proxy/share/doc/mysql-proxy/rw-splitting.lua

# 读写分离启动mysql-proxy
/usr/local/mysql-proxy/bin/mysql-proxy -P 192.168.8.102:4040 -b 192.168.8.101:3306 -r 192.168.8.102:3306 -s /usr/local/mysql-proxy/share/doc/mysql-proxy/rw-splitting.lua
# -P参数指定mysql-proxy运行的ip地址和端口
# -b参数指定mysql服务器的ip地址和端口
# -r参数指定只读mysql服务器的ip地址和端口
# -s参数指定脚本文件的路径
# --daemon表示在后台启动

# 注：因为-s指定了脚本，脚本有默认最小和最大连接空闲连接数，当连接数超过默认值时，mysql-proxy才会做负载均衡。
```

## 不常见功能

### 触发器

触发器是一类特殊的事务，可以监视某种数据操作(insert|update|delete)，并触发相关操作(insert|update|delete)。

使用场合：有时碰到表中某些数据改变，希望同时引起改变其他数据改变的需求，利用触发器可以满足这样的需求。例如商城中的有客户下订单后，库存量、购买人数等这些数据需要跟着改变。

作用：在表中某些特定数据变化时自动完成某些查询，运用触发器不仅可以简化程序，并且可以增加程序的灵活性。

创建触发器语法的四要素：

- 监视地点(table表)
- 监视事件(insert | update | delete)
- 触发时间(before | after)
- 触发事件(insert | update | delete)

```bash
# 创建触发器
CREATE TRIGGER 触发器名
  BEFORE 或 AFTER # 触发时间
  INSERT 或UPDATE 或 DELETE # 监视事件
  ON 表名 # 监视地点
  FOR EACH ROW #在mysql中必须写，行级触发器，在oracle可以不写，表示语句级触发器
  BEGIN # 开始触发
    sql语句1 sql语句2 ......
  END # 结束触发


# 查看触发器
SHOW TRIGGERS;

# 删除触发器
DROP TRIGGER 触发器名;
```

监控insert行为时，引用变量用new，监控delete行为时，引用变量用old

```bash
# (1)订单表插入数据时，触发商品表对应的数据修改的触发器。
CREATE TRIGGER tr1
AFTER INSERT
ON orders
FOR EACH ROW
BEGIN
  UPDATE goods SET num=num-new.much WHERE gid=new.gid;
END;

# 改进版，当购买数量大于库存数量是，默认为库存数量,防止爆仓。
CREATE TRIGGER tr4
BEFORE INSERT
ON orders
FOR EACH ROW
BEGIN
  DECLARE rnum SMALLINT UNSIGNED DEFAULT 0;
  SELECT num INTO rnum FROM goods WHERE gid=new.gid;
  IF new.much>rnum THEN
    SET new.much = rnum;
  END IF;

  UPDATE goods SET num=num-new.much WHERE gid=new.gid;
END;


# (2)订单表删除数据时，触发商品表对应的数据修改的触发器（实际中订单只能失效，不能删除）。
CREATE TRIGGER tr2
AFTER DELETE
ON orders
FOR EACH ROW
BEGIN
  UPDATE goods SET num=num+old.much WHERE gid=old.gid;
END;
```

监控update行为时，引用变量update前用old，update后用new。

```bash
# (3)修改订单表数据时，触发商品表对应的数据修改的触发器。
CREATE TRIGGER tr3
BEFORE UPDATE
ON orders
FOR EACH ROW
BEGIN
  UPDATE goods SET num=num+old.much-new.much WHERE gid=new.gid;
END;
```

### 存储过程

把若干条sql语句封装起来并起个名字，在过程中把数据存储到数据库中。

存储过程使用：

```bash
# 创建存储过程
CREATE PROCEDURE 名称()
  BEGIN
    # sql语句
  END;

# 调用存储过程
CALL 存储过程名字();

# 查看存储过程
SHOW PROCEDURE STATUS;

# 删除存储过程
DROP PROCEDURE 存储过程名字;
```

存储过程是可以编程的，意味着可以使用变量、表达式、控制结构来完成复杂的功能。

**声明变量**

```bash
# 语法
DECLARE 变量名 变量类型 [default 默认值]
  # 注意：声明变量必须在begin和end之间声明。
  # 变量可以参与sql语句的运算，SET 变量名 := 表达式

# 示例
CREATE PROCEDURE test1()
  BEGIN
    DECLARE leng INT DEFAULT 0;
    DECLARE widch INT DEFAULT 0;
    SET leng := 5;
    SET widch := 6;
    SELECT leng*widch;
  END;
```

**参数**

参数分为in、 out、 inout类型。in表示输入类型，out表示输出类型，inout表示输入输出类型。

```bash
# (1)in和out类型
CREATE PROCEDURE cuArea(in r INT, OUT area INT)
  BEGIN
    SET area:=0;# 如果输出area参与运算时必须设置area的初始值，因为null参与运算的值都为null
    SET area := 3.14*r*r;
  END;

# 调用：
CALL cuArea(10,@area);
SELECT @area;# 结果：314

# (2)inout类型
CREATE PROCEDURE add_1(INOUT v INT)
  BEGIN
    SET v := v + 1;
  END;

# 定义变量和调用
SET @v := 1;
CALL add_1(@v);
SELECT @v;  # 结果：2
```

### **if条件控制结构**

```bash
# 语法
IF 条件1 THEN
ELSEIF条件2 THEN
......
ELSE
END IF;
# 其中ELSEIF和ELSE可以没有。

# 使用示例
CREATE PROCEDURE compare(v1 INT,v2 INT)
  BEGIN
    IF v1>v2 THEN
      SELECT concat(v1,'大于',v2);
    ELSEIF v1<v2 THEN
      SELECT concat(v1,'小于',v2);
    ELSE
      SELECT concat(v1,'等于',v2);
    END IF;
  END;
```

### **case选择控制结构**

```bash
# 语法
CASE 变量
  WHEN 值 THEN 表达式;
  ......
  ELSE 不满足条件最后的默认结果;
END CASE;
# 注：else 可以省略。

# 使用示例
CREATE PROCEDURE cs()
  BEGIN
    DECLARE v INT;
    SET v := floor(rand()*10);
    CASE v
      WHEN 0 THEN SELECT '星期日';
      WHEN 1 THEN SELECT '星期一';
      WHEN 2 THEN SELECT '星期二';
      WHEN 3 THEN SELECT '星期三';
      WHEN 4 THEN SELECT '星期四';
      WHEN 5 THEN SELECT '星期五';
      WHEN 6 THEN SELECT '星期六';
      ELSE SELECT 'unknown day';
    END CASE;
  END;
```

### **while循环结构**

```bash
# 语法
WHILE 条件 DO
  执行语句
END WHILE;
# 注：避免死循环

# 使用示例
CREATE PROCEDURE cusum (v INT)
  BEGIN
    DECLARE s INT DEFAULT 0;
    WHILE v>0 DO
      SET s := s + v;
      SET v := v-1;
    END WHILE;
    SELECT s;
  END;

# 调用：
CALL cusum(100);  # 结果：5050
```

### **repeat循环结构**

```bash
# 语法
REPEAT
  执行语句......
UNTIL 条件 END REPEAT;

# 使用示例
CREATE PROCEDURE cuSum2(v INT)
  BEGIN
    DECLARE sum INT DEFAULT 0;
    REPEAT
      SET sum := sum+v;
      SET v:=v-1;
    UNTIL v<=0 END REPEAT;
    SELECT sum;
  END;

# 调用
CALL cuSum2(100); # 结果：5050
```

### 游标

一条sql的select语句取出对应的n条资源，取出资源的接口(句柄)就是游标，沿着游标，每次只取出一行，取出的行可以任意的逻辑控制了，而select没有这种功能。

```bash
# 声明游标
DECLARE 游标名 CURSOR FOR select语句;
# 设置触发边界标志
DECLARE EXIT HANDLER FOR NOT FOUND 表达式;
# 打开游标
OPEN 游标名;
# 取值
FETCH游标名INTO 变量1, 变量2 ......;
# 关闭游标
CLOSE 游标名;

# 用循环读取游标数据，结束条件是判断是否去到最后一条数据(事先计算出来的总数)。

# 使用示例
CREATE PROCEDURE cursor1()
  BEGIN
    DECLARE tmp_name VARCHAR(20);
    DECLARE tme_num INT;
    DECLARE cnt INT;
    DECLARE i INT DEFAULT 0;

    DECLARE get_goods CURSOR FOR SELECT name,num FROM goods;
    OPEN get_goods;
    SELECT count(*) INTO cnt FROM goods;
    WHILE i<cnt DO
      SET i:=i+1;
      FETCH get_goods INTO tmp_name,tme_num;# 取一条数据
      SELECT tmp_name,tme_num;
    END WHILE;

    CLOSE get_goods;
  END;

# 调用
CALL cursor1();
```

在mysql的cursor中可以用declare exit或continue handler for not fond来操作越界标志。类似于js中的事件，当读取游标完毕则触发该事件。其中exit和continue的区别是是否执行后面的sql语句。

```bash
# (1)触发越界后执行exit，不执行后面的sql语句
CREATE PROCEDURE cursor2()
  BEGIN
    DECLARE tmp_name VARCHAR(20);
    DECLARE tme_num INT;
    DECLARE isEnd BOOL DEFAULT FALSE;

    DECLARE get_goods CURSOR FOR SELECT name,num FROM goods;
    DECLARE EXIT HANDLER FOR NOT FOUND SET isEnd:=TRUE ;# 设置触发边界标志
    OPEN get_goods;

    WHILE !isEnd DO
      FETCH get_goods INTO tmp_name,tme_num;# 取一条数据
      SELECT tmp_name,tme_num;
    END WHILE;

    CLOSE get_goods;
  END;

# 调用
CALL cursor2();

# (2)触发越界后执行continue，继续后面的sql语句。
CREATE PROCEDURE cursor3()
  BEGIN
    DECLARE tmp_name VARCHAR(20);
    DECLARE tme_num INT;
    DECLARE isEnd BOOL DEFAULT FALSE;

    DECLARE get_goods CURSOR FOR SELECT name,num FROM goods;
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET isEnd:=TRUE ;# 设置触发边界标志
    OPEN get_goods;

    FETCH get_goods INTO tmp_name,tme_num;# 进入循环前先取一条数据
    WHILE !isEnd DO
      SELECT tmp_name,tme_num;
      FETCH get_goods INTO tmp_name,tme_num;# 取一条数据
    END WHILE;

    CLOSE get_goods;
  END;

# 调用
CALL cursor3();
```

## 导入和导出

**导出数据库**

(1) 导出全库备份到本地的目录

> ```bash
> mysqldump -u$USER -p$PASSWD -h127.0.0.1 -P3306 –routines –default-character-set=utf8 –lock-all-tables –add-drop-database -A > db.all.sql
> ```

(2) 导出指定库到本地的目录

> ```bash
> mysqldump -uroot -p123456 -h192.168.0.54 -P3306 –routines –default-character-set=utf8 –databases taobao > db.sql
> ```

(3) 导出某个库的表到本地的目录

> ```bash
> mysqldump -u$USER -p$PASSWD -h127.0.0.1 -P3306 –routines –default-character-set=utf8 –tables mysql user> db.table.sql
> ```

(4) 导出指定库的表(仅数据)到本地的目录(例如mysql库的user表,带过滤条件)

> ```bash
> mysqldump -u$USER -p$PASSWD -h127.0.0.1 -P3306 –routines –default-character-set=utf8 –no-create-db –no-create-info –tables mysql user –where=“host=‘localhost’”> db.table.sql
> ```

(5) 导出某个库的所有表结构

> ```bash
> mysqldump -u$USER -p$PASSWD -h127.0.0.1 -P3306 –routines –default-character-set=utf8 –no-data –databases mysql > db.nodata.sql
> ```

(6) 导出某个查询sql的数据为txt格式文件到本地的目录(各数据值之间用”制表符”分隔)，例如sql为’select user,host,password from mysql.user;’

> ```bash
> mysql -u$USER -p$PASSWD -h127.0.0.1 -P3306 –default-character-set=utf8 –skip-column-names -B -e ‘select user,host,password from mysql.user;’ > mysql_user.txt
> ```

(7) 导出某个查询sql的数据为txt格式文件到MySQL服务器，登录MySQL,将默认的制表符换成逗号(适应csv格式文件)。指定的路径，mysql要有写的权限。

> ```bash
> SELECT user,host,password FROM mysql.user INTO OUTFILE ‘/tmp/mysql_user.csv’ FIELDS TERMINATED BY ‘,’;
> ```

**导入数据库**

(1) 恢复全库数据到MySQL

> ```bash
> mysql -uroot -p123456 -h192.168.0.54 -P3306 –default-character-set=utf8 < db.sql
> ```

(2) 恢复某个库的数据(mysql库的user表)

> ```bash
> mysql -u$USER -p$PASSWD -h127.0.0.1 -P3306 –default-character-set=utf8 mysql < db.table.sql
> ```

(3) 恢复MySQL服务器上面的txt格式文件(需要FILE权限,各数据值之间用”制表符”分隔)

```bash
mysql -u$USER -p$PASSWD -h127.0.0.1 -P3306 --default-character-set=utf8
......
mysql> use mysql;
mysql> LOAD DATA INFILE '/tmp/mysql_user.txt' INTO TABLE user ;
```

(4) 恢复MySQL服务器上面的csv格式文件(需要FILE权限,各数据值之间用”逗号”分隔)

```bash
mysql -u$USER -p$PASSWD -h127.0.0.1 -P3306 --default-character-set=utf8
......
mysql> use mysql;
mysql> LOAD DATA INFILE '/tmp/mysql_user.csv' INTO TABLE user FIELDS TERMINATED BY ',';
```

(5) 恢复本地的txt或csv文件到MySQL

```bash
mysql -u$USER -p$PASSWD -h127.0.0.1 -P3306 --default-character-set=utf8
......
mysql> use mysql;
# txt
mysql> LOAD DATA LOCAL INFILE '/tmp/mysql_user.csv' INTO TABLE user;

# csv
mysql> LOAD DATA LOCAL INFILE '/tmp/mysql_user.csv' INTO TABLE user FIELDS TERMINATED BY ',';
```

(5) 文件导入

```bash
use o2o;
set names utf8;
source D:/o2o.sql
```

## 安全

**为什么需要安全连接**

SSL(Secure Socket Layer) ：安全套接字层，利用数据加密、身份验证和消息完整性验证机制，为基于TCP等可靠连接的应用层协议提供安全性保证，SSL协议提供的功能主要有：

- 数据传输的机密性：利用对称密钥算法对传输的数据进行加密。
- 身份验证机制：基于证书利用数字签名方法对服务器和客户端进行身份验证，其中客户端的身份验证是可选的。
- 消息完整性验证：消息传输过程中使用MAC算法来检验消息的完整性。

数据传输如果不是通过SSL的方式，那么其在网络中数据都是以明文进行传输的，如果传输的是敏感信息(账号、密码)，通过抓包就可以获取到敏感信息。所以在数据库方面，如果客户端连接服务器获取数据需要使用SSL连接，那么在传输过程中，经过加密的数据很难被窃取。

**客户端安全连接**

从mysql5.7之后的版本，在其data目录下有很多后缀为.pem类型的文件，这些.pem文件是用来SSL加密连接的，也就是说mysql5.7之后默认是开启了ssl安全连接功能。

```bash
# 查看是否开启了SSL/TLS安全连接功能
SHOW VARIABLES LIKE '%ssl%';

# 如果未开启，使用命令mysql_ssl_rsa_setup生成*.pem文件，默认存放在目录/var/lib/mysql下
```

从mysql安装目录/var/lib/mysql下复制出来3个客户端安全连接时需要的文件：**client-cert.pem、client-key.pem、ca.pem**。

**没有强制使用ssl安全连接情况**

默认情况下，mysql配置文件my.cnf的[mysqld]下require_secure_transport = OFF，说明不强制要求ssl连接，但mysql客户端连接时添加选项–ssl-mode也可以使用ssl安全连接，–ssl-mode选项值有以下5种类型：

- DISABLED：不使用安全连接，如果require_secure_transport = ON时则无效。
- PREFERRED, REQUIRED：使用SSL加密，加密算法没差别。
- VERIFY_CA, VERIFY_IDENTITY：客户端连接时需附加–ssl-ca等选项。

设置指定用户必须使用ssl传输数据示例：

```bash
# 使用x509连接
mysql -h 192.168.101.88 -u zhuyasen -P 33060 --ssl-cert=./client-cert.pem --ssl-key=./client-key.pem --ssl-mode=REQUIRED -p

# 使用x509连接和mysql服务颁发证书ca.pem连接
mysql -h 192.168.101.88 -u zhuyasen -P 33060 --ssl-mode=VERIFY_CA --ssl-cert=./client-cert.pem --ssl-key=./client-key.pem --ssl-ca=./ca.pem -p
```

在require_secure_transport = OFF(不需要ssl连接)，可以设置指定用户必须使用ssl传输数据。

```bash
# 创建新用户指定该用户使用连接方式：ssl或x509
CREATE USER 'krislin'@'%' IDENTIFIED WITH mysql_native_password BY '123456' REQUIRE ssl;
# 授权数据库和表的权限
GRANT ALL ON *.* TO 'krislin'@'%';
# 刷新
FLUSH PRIVILEGES;

# 对于已存在用户也可以是修改连接方式：ssl或x509
ALTER USER 'zhuyasen'@'%' REQUIRE x509;

# 查看用户表信息
select host,user,ssl_type,ssl_cipher,x509_issuer,x509_subject,plugin from mysql.user;

# 查看当前用户密文状态
show status like 'ssl_cipher';

# 如果用户不使用证书连接，返回连接失败(非本机连接)
mysql -h 192.168.101.88 -u vison -p

# 指定证书连接，连接成功
mysql -h 192.168.101.88 -u vison --ssl-cert=./client-cert.pem --ssl-key=./client-key.pem -p

# 查看上一条命令状态详情
\s
```

**强制所有用户使用ssl安全连接**

在配置文件my.cnf的[mysqld]下添加参数，强制所有用户传输使用ssl安全传输，

> require_secure_transport = ON

重启mysql后，远程登陆必须使用证书

```bash
mysql -h 192.168.101.88 -u root -P 33060 --ssl-cert=./client-cert.pem --ssl-key=./client-key.pem -p

mysql -h 192.168.101.88 -u vison -P 33060 --ssl-cert=./client-cert.pem --ssl-key=./client-key.pem --ssl-ca=./ca.pem --ssl-mode=VERIFY_CA -p
```

**未使用SS和使用SSL安全性对比**

- 未使用SSL情况下，在数据库服务器端可以通过抓包的方式获取数据，安全性不高。
- 没有抓到该语句，采用SSL加密后，tshark抓不到数据，安全性高。

**使用SSL前后性能对比**

- 虽然SSL方式使得安全性提高了，但是相对地使得QPS也降低23%左右，使用连接池或者长连接可能会好许多。
- 对于非常敏感核心的数据，或者QPS本来就不高的核心数据，可以采用SSL方式保障数据安全性；
- 对于采用短链接、要求高性能的应用，或者不产生核心敏感数据的应用，性能和可用性才是首要，建议不要采用SSL方式，除非使用云服务商数据强制要求除外。



**注：如果用户是采用本地localhost或者sock连接数据库，不会使用SSL方式的。**

# MySQL 高级

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

### 查询缓存

连接建立完成后，你就可以执行 select 语句了。MySQL 拿到一个查询请求后，在内存中查询缓存是否存在以 key-value 对的语句及结果，若存在则返回缓存。

若语句不在查询缓存中，就会继续后面的执行。执行结果则会被存入查询缓存中。若查询命中缓存，MySQL 不需要执行后面的复杂操作，可直接返回结果，提示效率。

**但大多数情况下查询缓存不适用，因为查询缓存往往弊大于利。**

查询缓存的失效非常频繁，若表有更新，则此表上所有的查询缓存都会被清空。

 MySQL 提供了“按需使用”的方式。将参数 query_cache_type 设置成 DEMAND，这样对于默认的 SQL 语句都不使用查询缓存。而对于你确定要使用查询缓存的语句，可以用 SQL_CACHE 显式指定，像下面这个语句一样：

```shell
mysql> select SQL_CACHE * from T where ID=10；
```

**MySQL 8.0 版本移除了查询缓存。**

### 分析器

若无命中查询缓存，则开始真正执行语句了，对 SQL 语句做解析。

分析器先会做“词法分析”，MySQL 需要识别出里面的字符串分别是什么，代表什么，例如“select”对应查询语句；之后做“语法分析”，根据词法分析的结果，语法分析器会根据语法规则，判断你输入的这个 SQL 语句是否满足 MySQL 语法。

若语句不对，则会收到“You have an error in your SQL syntax”的错误提醒。

```shell
mysql> elect * from t where ID=1;
 
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'elect * from t where ID=1' at line 1
```

### 优化器

经过了分析器，识别分析完SQL语句，在开始执行语句之前，还要经过优化器的处理。

优化器是在表里面有多个索引的时候，决定使用哪个索引；或者在一个语句有多表关联（join）的时候，决定各个表的连接顺序。比如你执行下面这样的语句，这个语句是执行两个表的 join：

```shell
mysql> select * from t1 join t2 using(ID)  where t1.c=10 and t2.d=20;
```

- 既可以先从表 t1 里面取出 c=10 的记录的 ID 值，再根据 ID 值关联到表 t2，再判断 t2 里面 d 的值是否等于 20。
- 也可以先从表 t2 里面取出 d=20 的记录的 ID 值，再根据 ID 值关联到 t1，再判断 t1 里面 c 的值是否等于 10。

这两种执行方法的逻辑结果是一样的，但是效率不同，而优化器的作用就是决定选择使用哪一个方案。

优化器阶段完成后，执行方案便确定下来了，然后进入执行器阶段。

### 执行器

MySQL 通过分析器识别语句，通过优化器确定执行方案，便进入了执行器阶段，开始执行语句。

开始执行时，先判断对这个表 T 有没有执行查询的权限，如果没有，就会返回没有权限的错误，如下所示 (在工程实现上，如果命中查询缓存，会在查询缓存返回结果的时候，做权限验证。查询也会在优化器之前调用 precheck 验证权限)。

```java
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

数据库的慢查询日志中存在一个 rows_examined 的字段，表示这个语句执行过程中扫描了多少行。这个值就是在执行器每次调用引擎获取数据行的时候累加的。

在有些场景下，执行器调用一次，在引擎内部则扫描了多行，因此**引擎扫描行数跟 rows_examined 并不是完全相同的。**

## 日志系统

### redo log

为了解决 MySQL 更新操作都需要写进磁盘，然后磁盘也要找到对应的那条记录，然后再更新，整个过程 IO 成本、查找成本都很高。MySQL 的设计者采用了 WAL 技术来提升更新效率，WAL 的全称是 Write-Ahead Logging。它的关键点就是先写日志，再写磁盘。

而采用写redo log是**顺序写**，无需像更新数据需要找位置，具体来说，当有一条记录需要更新的时候，InnoDB 引擎就会先把记录写到 redo log 里，并更新内存，在适当的时候，将这个操作记录更新到磁盘里面。

![下载](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/%E4%B8%8B%E8%BD%BD.png)

InnoDB 中的 redo log 大小是固定的，write pos 是当前记录的位置指针。checkpoint 是当前要擦除的位置指针，擦除记录前要把记录更新到数据文件。若两者相遇则表明无空闲空间，需把记录更新到磁盘中腾出空间再继续记录。

有了 redo log，InnoDB 就可以保证即使数据库发生异常重启，之前提交的记录都不会丢失，这个能力称为**crash-safe**。

### bin log

redo log 是 InnoDB 引擎特有的日志，而 Server 层也有自己的日志，称为 binlog（归档日志）。

Binlog有两种模式，statement 格式的话是记sql语句， row格式会记录行的内容，记录更新前和更新后的内容。

**只记录update/delete等语句，不记录select等不发生实际修改的语句，并且主从架构是依靠binlog来同步数据的。**

### 两者区别

这两种日志有以下三点不同。

1. redo log 是 InnoDB 引擎特有的；binlog 是 MySQL 的 Server 层实现的，所有引擎都可以使用。
2. redo log 是物理日志，记录的是“在某个数据页上做了什么修改”；binlog 是逻辑日志，记录的是这个语句的原始逻辑，比如“给 ID=2 这一行的 c 字段加 1 ”。
3. redo log 是循环写的，空间固定会用完；binlog 是可以追加写入的。“追加写”是指 binlog 文件写到一定大小后会切换到下一个，并不会覆盖以前的日志。

### 两阶段提交

以 InnoDB 执行更新语句为例。

![下载 (1)](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/%E4%B8%8B%E8%BD%BD%20(1).png)

1. 执行器先找引擎取 ID=2 这一行。ID 是主键，引擎直接用树搜索找到这一行。若此行所在的数据页在内存中，则直接返回给执行器；否则，需要先从磁盘读入内存，然后再返回。
2. 执行器拿到引擎给的行数据，把这个值加上 1，比如原来是 N，现在就是 N+1，得到新的一行数据，再调用引擎接口写入这行新数据。
3. 引擎将这行新数据更新到内存中，同时将这个更新操作记录到 redo log 里面，此时 redo log 处于 prepare 状态。然后告知执行器执行完成了，随时可以提交事务。
4. 执行器生成这个操作的 binlog，并把 binlog 写入磁盘。
5. 执行器调用引擎的提交事务接口，引擎把刚刚写入的 redo log 改成提交（commit）状态，更新完成。

redo log 和 binlog 都可以用于表示事务的提交状态，而两阶段提交就是让这两个状态保持逻辑上的一致。

- **先写 redo log 后写 binlog**：若写完 redo log后 MySQL 异常重启，此时数据虽然会恢复，但 binlog 里无此条语句更新逻辑，采用 binlog 恢复会造成数据不一致。
- **先写 binlog 后写 redo log**：若写完 binlog 后 MySQL 异常重启，由于 redo log 还没写，崩溃恢复以后这个事务无效。故采用 binlog 恢复库的时候会造成多出事务。

redo log 用于保证 crash-safe 能力。innodb_flush_log_at_trx_commit 参数设成 1 时，表示每次事务的 redo log 都直接持久化到磁盘，保证 MySQL 异常重启之后数据不丢失。

sync_binlog 参数设成 1 时，表示每次事务的 binlog 都持久化到磁盘，保证 MySQL 异常重启之后 binlog 不丢失。

### undo log

主要作用：回滚和多版本控制（MVCC）

如果因为某些原因导致事务失败或回滚了，可以用`undo log`进行回滚。

`undo log`主要存储的也是逻辑日志，比如我们要`insert`一条数据了，那`undo log`会记录的一条对应的`delete`日志。我们要`update`一条记录时，它会记录一条对应**相反**的update记录。

## 事务隔离

MySQL 是一个支持多引擎的系统，但并不是所有的引擎都支持事务。比如 MySQL 原生的 MyISAM 引擎就不支持事务，这也是 MyISAM 被 InnoDB 取代的重要原因之一。

1. 事务的特性：原子性、一致性、隔离性、持久性
2. 多事务同时执行的时候，可能会出现的问题：脏读、不可重复读、幻读
3. 事务隔离级别：读未提交、读提交、可重复读、串行化
   - 读未提交是指，一个事务还没提交时，它做的变更就能被别的事务看到。
   - 读提交是指，一个事务提交之后，它做的变更才会被其他事务看到。
   - 可重复读是指，一个事务执行过程中看到的数据，总是跟这个事务在启动时看到的数据是一致的。当然在可重复读隔离级别下，未提交变更对其他事务也是不可见的。
   - 串行化，顾名思义是对于同一行记录，“写”会加“写锁”，“读”会加“读锁”。当出现读写锁冲突的时候，后访问的事务必须等前一个事务执行完成，才能继续执行。

在数据库中访问的实际结果以视图为准

- “可重复读”的隔离级别下，视图是在启动时创建的，整个事务期间都存在此视图。
- “读已提交”隔离级别下，视图是在每个SQL语句开始执行时创建的
- “读未提交”隔离级别下，直接返回记录上的最新值，没有视图概念
- “串行化”隔离级别下，直接用加锁的方式来避免并行访问。

**事务的启动方式**

MySQL 的事务启动方式有以下几种：

1. 显式启动事务语句， begin 或 start transaction。配套的提交语句是 commit，回滚语句是 rollback。
2. set autocommit=0，这个命令会将这个线程的自动提交关掉。意味着如果你只执行一个 select 语句，这个事务就启动了，而且并不会自动提交。这个事务持续存在直到你主动执行 commit 或 rollback 语句，或者断开连接。

 set autocommit=0 的命令，会导致接下来的查询都在事务中，如果是长连接，就导致了意外的长事务。长事务会造成较大的回滚段，占用锁资源等性能消耗。

## 索引

索引的出现其实就是为了提高数据查询的效率。在 MySQL 中，索引是在存储引擎层实现的，并没有统一的索引标准，即不同存储引擎的索引的工作方式并不一样。而即使多个存储引擎支持同一种类型的索引，其底层的实现也可能不同。

**InnoDB 索引模型**

在 InnoDB 中，表都是根据主键顺序以索引的形式存放的，这种存储方式的表称为索引组织表。InnoDB 使用了 B+ 树索引模型，所以数据都是存储在 B+ 树中的。每一个索引在 InnoDB 里面对应一棵 B+ 树。



1、存储过程

2、循环结构

3、变量

4、函数

5、事务和视图

6、explain各字段

7、索引优化

单表优化：建索引、覆盖索引等

多表优化：左连接加索引在右表，右连接加索引在左表，理解数据量不同【小表驱动大表】

# MySQL 面试

## 1、B+树索引和哈希索引的区别

B+树是一个平衡的多叉树，从根节点到每个叶子节点的高度差值不超过1，而且同层级的节点间有指针相互链接。

哈希索引就是采用一定的哈希算法，把键值换算成新的哈希值，检索时无需B+树那样从根节点到叶子节点逐级查找，只需一次哈希算法即可定位到相应的位置，速度快。

B+树索引和哈希索引的明显区别是：

- **如果是等值查询，那么哈希索引明显有绝对优势**，只需要经过一次算法即可找到相应的键值；前提是键值都是唯一的。如果键值不是唯一的，就需要先找到该键所在位置，然后再根据链表往后扫描，直到找到相应的数据；
- **如果是范围查询检索，这时候哈希索引就毫无用武之地了**，因为原先是有序的键值，经过哈希算法后，有可能变成不连续的了，就没办法再利用索引完成范围查询检索；
- **哈希索引也没办法利用索引完成排序**，以及like ‘xxx%’ 这样的部分模糊查询（这种部分模糊查询，其实本质上也是范围查询）；
- **哈希索引也不支持多列联合索引的最左匹配规则**；
- B+树索引的关键字检索效率比较平均，不像B树那样波动幅度大，**在有大量重复键值情况下，哈希索引的效率也是极低的，因为存在所谓的哈希碰撞问题**。

## 2、MVCC原理

MVCC 是一种并发控制的方法，实现对数据库的并发访问；在编程语言中实现事务内存。它使得大部分支持行锁的事务引擎，不再单纯的使用行锁来进行数据库的并发控制，取而代之的是把数据库的行锁与行的多个版本结合起来，只需要很小的开销，就可以实现非锁定读，从而大大提高数据库系统的并发性能。

一句话讲，MVCC就是用 同一份数据临时保留多版本的方式 的方式，实现并发控制。

1. 系统版本号：每开启一个事务，系统版本号就会自增
2. 事务版本号：事务创建时的系统版本号
3. 创建版本号：创建一个数据行快照时的系统版本号
4. 删除版本号：如果快照的删除版本号大于事务版本号，说明该快照有效；否则表示该快照已经被删除。
5. innodb存储的最基本row中包含一些额外的存储信息 DATA_TRX_ID，DATA_ROLL_PTR，DB_ROW_ID，DELETE BIT
6. 6字节的DATA_TRX_ID 标记了最新更新这条行记录的transaction id，每处理一个事务，其值自动+1
7. 7字节的DATA_ROLL_PTR 指向当前记录项的rollback segment的undo log记录，找之前版本的数据就是通过这个指针
8. 6字节的DB_ROW_ID，当由innodb自动产生聚集索引时，聚集索引包括这个DB_ROW_ID的值，否则聚集索引中不包括这个值.，这个用于索引当中
9. DELETE BIT位用于标识该记录是否被删除，这里的不是真正的删除数据，而是标志出来的删除。真正意义的删除是在commit的时候

MVCC使用的快照保存在日志中，该日志通过回滚指针把一个数据行的所有快照连接起来。

与redo log相反，undo log是为了回滚事务而写的日志，具体内容就是copy事务开始前的数据（行）到undo buffer。

与redo buffer一样，undo buffer也是环形缓冲，当缓冲满的时候buffer内容会被刷新到磁盘。

与redo log不同的是，undo log没有独立的磁盘文件，所有的undo log均被存在主ibd数据文件中（表空间）。

**快照读和当前读**

快照读：读取的是快照版本，也就是历史版本,MVCC读取的是快照中的数据，可以减少加锁带来的开销。

当前读：读取的是最新版本,读的是最新数据，需要加锁。

普通的SELECT就是快照读，而UPDATE、DELETE、INSERT、SELECT ...  LOCK IN SHARE MODE、SELECT ... FOR UPDATE是当前读。

**实现过程**

innodb MVCC主要是为Repeatable-Read事务隔离级别做的。在此隔离级别下，A、B客户端所示的数据相互隔离，互相更新不可见

了解innodb的行结构、Read-View的结构对于理解innodb mvcc的实现由重要意义具体的执行过程

begin->用排他锁锁定该行->记录redo log->记录undo log->修改当前行的值，写事务编号，回滚指针指向undo log中的修改前的行

上述过程确切地说是描述了UPDATE的事务过程，其实undo log分insert和update undo log，因为insert时，原始的数据并不存在，所以回滚时把insert undo log丢弃即可，而update undo log则必须遵守上述过程

下面分别以select、delete、 insert、 update语句来说明

关键：开启一个事务时，该事务版本号肯定大于当前所有数据行的创建版本号

SELECT

Innodb检查每行数据，确保他们符合两个标准：

1、InnoDB只查找版本早于当前事务版本的数据行(也就是数据行的版本必须小于等于事务的版本)，这确保当前事务读取的行都是事务之前已经存在的，或者是由当前事务创建或修改的行

2、行的删除操作的版本一定是未定义的或者大于当前事务的版本号，确定了当前事务开始之前，行没有被删除

符合了以上两点则返回查询结果。

INSERT

InnoDB为每个新增行记录当前系统版本号作为创建ID。

DELETE

InnoDB为每个删除行的记录当前系统版本号作为行的删除ID。

UPDATE

新插入一行（复制了要删除的记录），并以当前事务的版本号作为新行的创建版本号，同时将原记录行的删除版本号设置为当前事务版本号

说明

insert操作时 “创建时间”=DB_ROW_ID，这时，“删除时间 ”是未定义的；

update时，复制新增行的“创建时间”=DB_ROW_ID，删除时间未定义，旧数据行“创建时间”不变，删除时间=该事务的DB_ROW_ID；

delete操作，相应数据行的“创建时间”不变，删除时间=该事务的DB_ROW_ID；

select操作对两者都不修改，只读相应的数据

六、对于MVCC的总结

上述更新前建立undo log，根据各种策略读取时非阻塞就是MVCC，undo log中的行就是MVCC中的多版本，这个可能与我们所理解的MVCC有较大的出入，一般我们认为MVCC有下面几个特点：

1. 每行数据都存在一个版本，每次数据更新时都更新该版本
2. 修改时Copy出当前版本随意修改，各个事务之间无干扰
3. 保存时比较版本号，如果成功（commit），则覆盖原记录；失败则放弃copy（rollback）
4. 事务以排他锁的形式修改原始数据
5. 把修改前的数据存放于undo log，通过回滚指针与主数据关联
6. 修改成功（commit）啥都不做，失败则恢复undo log中的数据（rollback）

## 3、了解count(1)、count(*)、count(列名)区别吗

1. 当 count() 统计某一列时，比如 count(a)，a 表示列名，是不统计 null 的。
2. count(*) 无论是否包含空值，都会统计。
3. count(1)中的 1 是恒真表达式，因此也会统计所有结果。

在 MySQL 5.6之后的版本里，count(*)与count(1)都会选择非聚簇索进行优化执行，因为由于主键索引树的特性，其大小会比聚簇索引大得多。

## 4、分页查询如何优化

- 通常在MySQL中通过limit #{limit},#{offset}来进行分页查询。
- 当表中记录较多且页数（#{limit}）较大时，分页查询效率变慢。
- 变慢的原因时，分页查询时，会先查询出limit + offset条记录，然后截取后面的offset记录。

**原始SQL**

```mysql
-- 原始分页查询，耗时：4500ms
select * 
from big_table
limit 2000000,10;
```

慢的原因：

- 1.查询条件为`*`
- 2.`limit = 2000000`太大

**优化一（推荐）：用具体字段2代替*（1600ms）***

```mysql
-- 用明确字段代替*，耗时：1600ms
select id,uid,media_id,media_name,news_id,comment 
from big_table
limit 2000000,10;
```

**优化二：先查询索引（450ms）**

```mysql
-- 方法1：先对索引进行分页查询，耗时：450ms
select * from big_table AS h inner join
 (select id from big_table
  limit 2000000,10) AS ss 
 on h.id = ss.id;

-- 方法2：先查询出起始位置的索引，耗时：450ms
select * from big_table
where id > (
	select id from big_table limit 2000000,1
)
limit 10;

```

**优化三：between … and （5ms）**

```mysql
-- 上一页保留最后一条记录所在id，耗时：5ms
select * from big_table
where id between 4882489 and 4882489 + 10 ;
```

**优化四：保留上一页所在的id（5ms）**

```mysql
-- 上一页需要保留最后一条记录所在id，耗时：5ms
select * from big_table
where id > 4882488
limit 10;
```

## 5、常见的SQL预防手段

SQL注入攻击是输入参数未经过滤，然后直接拼接到SQL语句当中解析，执行达到预想之外的一种行为，称之为SQL注入攻击。

1）严格检查输入变量的类型和格式

对于整数参数，加判断条件：不能为空、参数类型必须为数字

对于字符串参数，可以使用正则表达式进行过滤：如：必须为[0-9a-zA-Z]范围内的字符串

2）过滤和转义特殊字符

在username这个变量前进行转义，对'、"、\等特殊字符进行转义，如：php中的addslashes()函数对username参数进行转义

3）利用mysql的预编译机制

把sql语句的模板（变量采用占位符进行占位）发送给mysql服务器，mysql服务器对sql语句的模板进行编译，编译之后根据语句的优化分析对相应的索引进行优化，在最终绑定参数时把相应的参数传送给mysql服务器，直接进行执行，节省了sql查询时间，以及mysql服务器的资源，达到一次编译、多次执行的目的，除此之外，还可以防止SQL注入。

## 6、为什么说B+树比B树更适合数据库索引？

1、 B+树的磁盘读写代价更低：B+树的内部节点并没有指向关键字具体信息的指针，因此其内部节点相对B树更小，如果把所有同一内部节点的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多，一次性读入内存的需要查找的关键字也就越多，相对IO读写次数就降低了。

2、B+树的查询效率更加稳定：由于非终结点并不是最终指向文件内容的结点，而只是叶子结点中关键字的索引。所以任何关键字的查找必须走一条从根结点到叶子结点的路。所有关键字查询的路径长度相同，导致每一个数据的查询效率相当。

3、由于B+树的数据都存储在叶子结点中，分支结点均为索引，方便扫库，只需要扫一遍叶子结点即可，但是B树因为其分支结点同样存储着数据，我们要找到具体的数据，需要进行一次中序遍历按序来扫，所以B+树更加适合在区间查询的情况，所以通常B+树用于数据库索引。

## 7、聚簇索引与非聚簇索引之间的区别

聚集索引与非聚集索引的区别是：叶节点是否存放一整行记录

InnoDB 主键使用的是聚簇索引，MyISAM 不管是主键索引，还是二级索引使用的都是非聚簇索引

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/5/16/16ac10253b8748df~tplv-t2oaga2asx-watermark.awebp)

1.对于**非聚簇索引表**来说（右图），表数据和索引是分成两部分存储的，主键索引和二级索引存储上没有任何区别。使用的是B+树作为索引的存储结构，所有的节点都是索引，叶子节点存储的是索引+索引对应的记录的数据。

2.对于**聚簇索引表**来说（左图），表数据是和主键一起存储的，主键索引的叶结点存储行数据(包含了主键值)，二级索引的叶结点存储行的主键值。使用的是B+树作为索引的存储结构，非叶子节点都是索引关键字，但非叶子节点中的关键字中不存储对应记录的具体内容或内容地址。叶子节点上的数据是主键与具体记录(数据内容)。

**聚簇索引的优点**

​	1.当你需要取出一定范围内的数据时，用聚簇索引也比用非聚簇索引好。

​	2.当通过聚簇索引查找目标数据时理论上比非聚簇索引要快，因为非聚簇索引定位到对应主键时还要多一次目标记录寻址,即多一次I/O。

​	3.使用覆盖索引扫描的查询可以直接使用页节点中的主键值。

**聚簇索引的缺点**

​	1.**插入速度严重依赖于插入顺序**，按照主键的顺序插入是最快的方式，否则将会出现页分裂，严重影响性能。因此，对于InnoDB表，我们一般都会定义一个自增的ID列为主键。

​	2.**更新主键的代价很高，因为将会导致被更新的行移动**。因此，对于InnoDB表，我们一般定义主键为不可更新。

​	3.**二级索引访问需要两次索引查找，第一次找到主键值，第二次根据主键值找到行数据。**

​	二级索引的叶节点存储的是主键值，而不是行指针（非聚簇索引存储的是指针或者说是地址），这是为了减少当出现行移动或数据页分裂时二级索引的维护工作，但会让二级索引占用更多的空间。

​	4.**采用聚簇索引插入新值比采用非聚簇索引插入新值的速度要慢很多**，因为插入要保证主键不能重复，判断主键不能重复，采用的方式在不同的索引下面会有很大的性能差距，聚簇索引遍历所有的叶子节点，非聚簇索引也判断所有的叶子节点，但是聚簇索引的叶子节点除了带有主键还有记录值，记录的大小往往比主键要大的多。这样就会导致聚簇索引在判定新记录携带的主键是否重复时进行昂贵的I/O代价。

## 8、自增主键的优缺点

这种方式是使用数据库提供的自增数值型字段作为自增主键，它的优点是：

（1）数据库自动编号，速度快，而且是增量增长，按顺序存放，对于检索非常有利；

（2）数字型，占用空间小，易排序，在程序中传递也方便；

（3）如果通过非系统增加记录时，可以不用指定该字段，不用担心主键重复问题。

其实它的缺点也就是来自其优点，缺点如下：

（1）因为自动增长，在手动要插入指定ID的记录时会显得麻烦，尤其是当系统与其它系统集成时，需要数据导入时，很难保证原系统的ID不发生主键冲突（前提是老系统也是数字型的）。特别是在新系统上线时，新旧系统并行存在，并且是异库异构的数据库的情况下，需要双向同步时，自增主键将是你的噩梦；

（2）在系统集成或割接时，如果新旧系统主键不同是数字型就会导致修改主键数据类型，这也会导致其它有外键关联的表的修改，后果同样很严重；

（3）若系统也是数字型的，在导入时，为了区分新老数据，可能想在老数据主键前统一加一个字符标识（例如“o”，old）来表示这是老数据，那么自动增长的数字型又面临一个挑战。

## 9、MySQL、Oracle区别

- 事务提交区别：MySQL默认是自动提交，而Oracle默认不自动提交，需要用户手动提交，需要在写commit;指令或者点击commit按钮
- 分页查询区别：MySQL是直接在SQL语句中写"select... from ...where...limit  x, y",有limit就可以实现分页;而Oracle则是需要用到伪列ROWNUM和嵌套查询
- 事务隔离级别：MySQL是read commited的隔离级别，而Oracle是repeatable read的隔离级别，同时二者都支持serializable串行化事务隔离级别，可以实现最高级别的
  读一致性。每个session提交后其他session才能看到提交的更改。Oracle通过在undo表空间中构造多版本数据块来实现读一致性，每个session
  查询时，如果对应的数据块发生变化，Oracle会在undo表空间中为这个session构造它查询时的旧的数据块
  MySQL没有类似Oracle的构造多版本数据块的机制，只支持read commited的隔离级别。一个session读取数据时，其他session不能更改数据，但
  可以在表最后插入数据。session更新数据时，要加上排它锁，其他session无法访问数据
- 对事务的支持：MySQL在innodb存储引擎的行级锁的情况下才可支持事务，而Oracle则完全支持事务
- 保存数据的持久性：MySQL是在数据库更新或者重启，则会丢失数据，Oracle把提交的sql操作线写入了在线联机日志文件中，保持到了磁盘上，可以随时恢复
- 并发性：MySQL以表级锁为主，对资源锁定的粒度很大，如果一个session对一个表加锁时间过长，会让其他session无法更新此表中的数据。虽然InnoDB引擎的表可以用行级锁，但这个行级锁的机制依赖于表的索引，如果表没有索引，或者sql语句没有使用索引，那么仍然使用表级锁。 Oracle使用行级锁，对资源锁定的粒度要小很多，只是锁定sql需要的资源，并且加锁是在数据库中的数据行上，不依赖与索引。所以Oracle对并 发性的支持要好很多。
- 逻辑备份：MySQL逻辑备份时要锁定数据，才能保证备份的数据是一致的，影响业务正常的dml使用,Oracle逻辑备份时不锁定数据，且备份的数据是一致
- 复制：MySQL:复制服务器配置简单，但主库出问题时，丛库有可能丢失一定的数据。且需要手工切换丛库到主库。
  Oracle:既有推或拉式的传统数据复制，也有dataguard的双机或多机容灾机制，主库出现问题是，可以自动切换备库到主库，但配置管理较复杂。
- 性能诊断：MySQL的诊断调优方法较少，主要有慢查询日志。
  Oracle有各种成熟的性能诊断调优工具，能实现很多自动分析、诊断功能。比如awr、addm、sqltrace、tkproof等    
- 权限与安全：MySQL的用户与主机有关，感觉没有什么意义，另外更容易被仿冒主机及ip有可乘之机。
  Oracle的权限与安全概念比较传统，中规中矩。
- 分区表和分区索引：MySQL的分区表还不太成熟稳定。
  Oracle的分区表和分区索引功能很成熟，可以提高用户访问db的体验。
- 管理工具：MySQL管理工具较少，在linux下的管理工具的安装有时要安装额外的包（phpmyadmin， etc)，有一定复杂性。
  Oracle有多种成熟的命令行、图形界面、web管理工具，还有很多第三方的管理工具，管理极其方便高效。
- 最重要的区别：MySQL是轻量型数据库，并且免费，没有服务恢复数据。
  Oracle是重量型数据库，收费，Oracle公司对Oracle数据库有任何服务。

## 9、数据库四个范式区别

1. 第一范式：无重复的列
2. 第二范式：属性完全依赖于主键
3. 第三范式：属性不依赖于其他非主属性
4. 第四范式：禁止主键列和非主键列一对多关系不受约束
