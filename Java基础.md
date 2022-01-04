# 入门

## Java简介

Java编译器将Java源程序编译成与平台无关的字节码文件(class文件)，由Java虚拟机（JVM）对字节码文件解释执行。不同的操作系统下实现java程序的跨平台。

- JVM（Java Virtual Machine）：Java虚拟机
- JRE（Java Runtime Environment）：Java运行环境，包含了JVM和Java的核心类库（Java API）

- JDK（Java Development Kit）: 称为Java开发工具，包含了JRE和开发工具

| 目录名称 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| bin      | 该路径下存放了JDK的各种工具命令。javac和java就放在这个目录。 |
| conf     | 该路径下存放了JDK的相关配置文件。                            |
| include  | 该路径下存放了一些平台特定的头文件。                         |
| jmods    | 该路径下存放了JDK的各种模块。                                |
| legal    | 该路径下存放了JDK各模块的授权文档。                          |
| lib      | 该路径下存放了JDK工具的一些补充JAR包。                       |

## 基础

### 标识符

标识符是用户编程时使用的名字，用于给类、方法、变量、常量等命名。

Java中标识符的组成规则：

- 由字母、数字、下划线“_”、美元符号“$”组成，第一个字符不能是数字。
- 不能使用java中的关键字作为标识符。	
- 标识符对大小写敏感（区分大小写）。

Java中标识符的命名约定：

- 小驼峰式命名：变量名、方法名
  - 首字母小写，从第二个单词开始每个单词的首字母大写。

- 大驼峰式命名：类名
  - 每个单词的首字母都大写。

​	另外，标识符的命名最好可以做到见名知意

### 注释

注释是对代码的解释和说明文字，可提高程序的可读性。Java中的注释分为三种：

- 单行注释。
- 多行注释。
- 文档注释。

```java
//单行注释

/*
这是多行注释文字
这是多行注释文字
这是多行注释文字
*/

/**
*文档注释
*/
```

### 关键字

关键字是指被java语言赋予了特殊含义的单词。

关键字的特点：

- 关键字的字母全部小写，常用的代码编辑器对关键字都有高亮显示，比如现在我们能看到的public、class、static等。

​       ![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1591781912346-3c29035a-9531-4cee-af74-a2190de10c5c.png)

- private 表示私有，只有自己类能访问
- default表示没有修饰符修饰，只有同一个包的类能访问
- protected表示可以被同一个包的类以及其他包中的子类访问
- public表示可以被该项目的所有包中的所有类访问

### 基本结构

```java
    public class Hello {
        public static void main(String[] args) {
            System.out.println("Hello, world!");
        }
    }
```

一个Java源码只能定义一个`public`类型的class，并且class名称和文件名要完全一致；

使用`javac`可以将`.java`源码编译成`.class`字节码；

使用`java`可以运行一个已编译的Java程序，参数是类名。

类名要求：

- 类名必须以英文字母开头，后接字母，数字和下划线的组合
- 习惯以大写字母开头

### 数据类型、变量、常量

**Java中的数据类型**

Java是一个强类型语言，必须明确数据类型。在Java中的数据类型包括基本数据类型和引用数据类型两种。

Java中的基本数据类型：

| 数据类型 | 关键字       | 内存占用 | 取值范围                                                     |
| -------- | ------------ | -------- | ------------------------------------------------------------ |
| 整数类型 | byte         | 1        | -128~127                                                     |
|          | short        | 2        | -32768~32767                                                 |
|          | int(默认)    | 4        | -2的31次方到2的31次方-1                                      |
|          | long         | 8        | -2的63次方到2的63次方-1                                      |
| 浮点类型 | float        | 4        | 负数：-3.402823E+38到-1.401298E-45                                                             正数：   1.401298E-45到3.402823E+38 |
|          | double(默认) | 8        | 负数：-1.797693E+308到-4.9000000E-324                                              正数：4.9000000E-324   到1.797693E+308 |
| 字符类型 | char         | 2        | 0-65535                                                      |
| 布尔类型 | boolean      | 1        | true，false                                                  |

说明：

​	e+38表示是乘以10的38次方，同样，e-45表示乘以10的负45次方。

​	在java中整数默认是int类型，浮点数默认是double类型。

**变量**

变量：在程序运行过程中，其值可以发生改变的量。

从本质上讲，变量是内存中的一小块区域，其值可以在一定范围内变化。

变量的定义格式：

- 数据类型 变量名 = 初始化值; 

- 数据类型 变量名;

  变量名 = 初始化值;

还可以在同一行定义多个同一种数据类型的变量，中间使用逗号隔开。但不建议使用这种方式，降低程序的可读性。

变量的使用：通过变量名访问即可。

**使用变量时的注意事项**

- 在同一对花括号中，变量名不能重复。
- 变量在使用之前，必须初始化（赋值）。
- 定义long类型的变量时，需要在整数的后面加L（大小写均可，建议大写）。因为整数默认是int类型，整数太大可能超出int范围。
- 定义float类型的变量时，需要在小数的后面加F（大小写均可，建议大写）。因为浮点数的默认类型是double， double的取值范围是大于float的，类型不兼容。

**常量**

常量：在程序运行过程中，其值不可以发生改变的量。

Java中的常量分类：

1. 字符串常量：用双引号括起来的多个字符（可以包含0个、一个或多个）。
2. 整数常量：整数。
3. 小数常量：小数。
4. 字符常量：用单引号括起来的一个字符。
5. 布尔常量：布尔值，表示真假，只有两个值true和false
6. 空常量：一个特殊的值，空值，值为null

除空常量外，其他常量均可使用输出语句直接输出。

### 运算符

**运算符和表达式**

运算符：对常量或者变量进行操作的符号

表达式：用运算符把常量或者变量连接起来符合java语法的式子就可以称为表达式。

不同运算符连接的表达式体现的是不同类型的表达式。

#### **算术运算符**

| 符号 | 作用 | 说明                         |
| ---- | ---- | ---------------------------- |
| +    | 加   | 参看小学一年级               |
| -    | 减   | 参看小学一年级               |
| *    | 乘   | 参看小学二年级，与“×”相同    |
| /    | 除   | 参看小学二年级，与“÷”相同    |
| %    | 取余 | 获取的是两个数据做除法的余数 |

注意：

/和%的区别：两个数据做除法，/取结果的商，%取结果的余数。

整数操作只能得到整数，要想得到小数，必须有浮点数参与运算。

**字符的“+”操作**

char类型参与算术运算，使用的是计算机底层对应的十进制数值。需要我们记住三个字符对应的数值：

'a'  --  97		a-z是连续的，所以'b'对应的数值是98，'c'是99，依次递加

'A'  --  65		A-Z是连续的，所以'B'对应的数值是66，'C'是67，依次递加

'0'  --  48		0-9是连续的，所以'1'对应的数值是49，'2'是50，依次递加

```java
// 可以通过使用字符与整数做算术运算，得出字符对应的数值是多少
char ch1 = 'a';
System.out.println(ch1 + 1); // 输出98，97 + 1 = 98
```

算术表达式中包含不同的基本数据类型的值的时候，整个算术表达式的类型会自动进行提升。

```java
byte b1 = 10;
byte b2 = 20;
// byte b3 = b1 + b2; // 该行报错，因为byte类型参与算术运算会自动提示为int，int赋值给byte可能损失精度
int i3 = b1 + b2; // 应该使用int接收
byte b3 = (byte) (b1 + b2); // 或者将结果强制转换为byte类型
-------------------------------
int num1 = 10;
double num2 = 20.0;
double num3 = num1 + num2; // 使用double接收，因为num1会自动提升为double类型
```

tips：正是由于上述原因，所以在程序开发中我们很少使用byte或者short类型定义整数。也很少会使用char类型定义字符，而使用字符串类型，更不会使用char类型做算术运算。

 **字符串的“+”操作**

当“+”操作中出现字符串时，这个”+”是字符串连接符，而不是算术运算。

```java
System.out.println("itheima"+ 666); // 输出：itheima666
```

在”+”操作中，如果出现了字符串，就是连接运算符，否则就是算术运算。

**赋值运算符**

赋值运算符的作用是将一个表达式的值赋给左边，左边必须是可修改的，不能是常量。

| 符号 | 作用       | 说明                  |
| ---- | ---------- | --------------------- |
| =    | 赋值       | a=10，将10赋值给变量a |
| +=   | 加后赋值   | a+=b，将a+b的值给a    |
| -=   | 减后赋值   | a-=b，将a-b的值给a    |
| *=   | 乘后赋值   | a*=b，将a×b的值给a    |
| /=   | 除后赋值   | a/=b，将a÷b的商给a    |
| %=   | 取余后赋值 | a%=b，将a÷b的余数给a  |

注意：

扩展的赋值运算符隐含了强制类型转换。

```java
short s = 10;
s = s + 10; // 此行代码报出，因为运算中s提升为int类型，运算结果int赋值给short可能损失精度

s += 10; // 此行代码没有问题，隐含了强制类型转换，相当于 s = (short) (s + 10);
```

#### **自增自减运算符**

| 符号 | 作用 | 说明        |
| ---- | ---- | ----------- |
| ++   | 自增 | 变量的值加1 |
| --   | 自减 | 变量的值减1 |

注意事项：

​	++和-- 既可以放在变量的后边，也可以放在变量的前边。

​	单独使用的时候， ++和-- 无论是放在变量的前边还是后边，结果是一样的。

​	参与操作的时候，如果放在变量的后边，先拿变量参与操作，后拿变量做++或者--。

​	参与操作的时候，如果放在变量的前边，先拿变量做++或者--，后拿变量参与操作。

#### **关系运算符**

关系运算符有6种关系，分别为小于、小于等于、大于、等于、大于等于、不等于。

| 符号 | 说明                                                    |
| ---- | ------------------------------------------------------- |
| ==   | a==b，判断a和b的值是否相等，成立为true，不成立为false   |
| !=   | a!=b，判断a和b的值是否不相等，成立为true，不成立为false |
| >    | a>b，判断a是否大于b，成立为true，不成立为false          |
| >=   | a>=b，判断a是否大于等于b，成立为true，不成立为false     |
| <    | a<b，判断a是否小于b，成立为true，不成立为false          |
| <=   | a<=b，判断a是否小于等于b，成立为true，不成立为false     |

注意事项：

​	关系运算符的结果都是boolean类型，要么是true，要么是false。

​	千万不要把“”误写成“=”，""是判断是否相等的关系，"="是赋值。

#### **逻辑运算符**

逻辑运算符把各个运算的关系表达式连接起来组成一个复杂的逻辑表达式，以判断程序中的表达式是否成立，判断的结果是 true 或 false。

| 符号 | 作用     | 说明                                         |
| ---- | -------- | -------------------------------------------- |
| &    | 逻辑与   | a&b，a和b都是true，结果为true，否则为false   |
| \|   | 逻辑或   | a\|b，a和b都是false，结果为false，否则为true |
| ^    | 逻辑异或 | a^b，a和b结果不同为true，相同为false         |
| !    | 逻辑非   | !a，结果和a的结果正好相反                    |



| 符号 | 作用   | 说明                         |
| ---- | ------ | ---------------------------- |
| &&   | 短路与 | 作用和&相同，但是有短路效果  |
| \|\| | 短路或 | 作用和\|相同，但是有短路效果 |

- 逻辑与&，无论左边真假，右边都要执行。

- 短路与&&，如果左边为真，右边执行；如果左边为假，右边不执行。

- 逻辑或|，无论左边真假，右边都要执行。

- 短路或||，如果左边为假，右边执行；如果左边为真，右边不执行。

#### **三元运算符**

三元运算符语法格式：

```java
关系表达式 ? 表达式1 : 表达式2;
```

解释：问号前面的位置是判断的条件，判断结果为boolean型，为true时调用表达式1，为false时调用表达式2。

### 类型转换

在Java中，一些数据类型之间是可以相互转换的。分为两种情况：自动类型转换和强制类型转换。

**自动类型转换：**

把一个表示数据范围小的数值或者变量赋值给另一个表示数据范围大的变量。这种转换方式是自动的，直接书写即可。如：

```java
double num = 10; // 将int类型的10直接赋值给double类型
System.out.println(num); // 输出10.0
```

**强制类型转换：**

把一个表示数据范围大的数值或者变量赋值给另一个表示数据范围小的变量。

强制类型转换格式：目标数据类型 变量名 = (目标数据类型)值或者变量;

​	如：

```java
double num1 = 5.5;
int num2 = (int) num1; // 将double类型的num1强制转换为int类型
System.out.println(num2); // 输出5（小数位直接舍弃）
```

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1609770479591-15c4f64a-e1bf-4ec6-bf1c-e0758d0a5afb.png)

说明：

1. char类型的数据转换为int类型是按照码表中对应的int值进行计算的。比如在ASCII码表中，'a'对应97。

2. 整数默认是int类型，byte、short和char类型数据参与运算均会自动转换为int类型。
3. boolean类型不能与其他基本数据类型相互转换。

### 数组类型

Java的数组有几个特点：

- 数组所有元素初始化为默认值，整型都是`0`，浮点型是`0.0`，布尔型是`false`；
- 数组一旦创建后，大小就不可改变。

例如：

```java
int[] nums = new int[5];
String[] strs = new String[5];
int[] nums_1 = new int[]{1,2,3,4,5}
String[] strs_1 = {"A","B","C"};
```

- 数组是同一数据类型的集合，数组一旦创建后，大小就不可变；

- 可以通过索引访问数组元素，但索引超出范围将报错；

- 数组元素可以是值类型（如int）或引用类型（如String），但数组本身是引用类型；

## 流程控制

### 输入和输出

使用`System.out.println()`来向屏幕输出一些内容，`println`是print line的缩写，表示输出并换行。因此，如果输出后不想换行，可以用`print()`：

**格式化输出：**

Java的格式化功能提供了多种占位符，可以把各种数据类型“格式化”成指定的字符串：

| 占位符 | 说明                             |
| ------ | -------------------------------- |
| %d     | 格式化输出整数                   |
| %x     | 格式化输出十六进制整数           |
| %f     | 格式化输出浮点数                 |
| %e     | 格式化输出科学计数法表示的浮点数 |
| %s     | 格式化字符串                     |

注意，由于%表示占位符，因此，连续两个%%表示一个%字符本身。

**输入：**

1、导包。Scanner 类在java.util包下，所以需要将该类导入。导包的语句需要定义在类的上面。

```java
import java.util.Scanner;
```

2、创建Scanner对象。

```java
Scanner sc = new Scanner(System.in);// 创建Scanner对象，sc表示变量名，其他均不可变
```

3、接收数据

```java
int i = sc.nextInt(); // 表示将键盘录入的值作为int数返回。
```

- Java提供的输出包括：`System.out.println()` / `print()` / `printf()`，其中`printf()`可以格式化输出；

- Java提供Scanner对象来方便输入，读取对应的类型可以使用：`scanner.nextLine()` / `nextInt()` / `nextDouble()` / ...

### if判断

**格式一：**

```java
if (关系表达式) {
    语句体;	
}
```

执行流程：

①首先计算关系表达式的值

②如果关系表达式的值为true就执行语句体

③如果关系表达式的值为false就不执行语句体

④继续执行后面的语句内容



![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1609771072835-cd67aabd-89ae-4d2d-9c38-96cbc7e601e4.png)



**格式二：**

```java
if (关系表达式) {
    语句体1;	
} else {
    语句体2;	
}
```

执行流程：

①首先计算关系表达式的值

②如果关系表达式的值为true就执行语句体1

③如果关系表达式的值为false就执行语句体2

④继续执行后面的语句内容

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1609771114533-0b3d0953-4b91-48c5-8eb6-45b36073b3e9.png)

**格式三**

```java
if (关系表达式1) {
    语句体1;	
} else if (关系表达式2) {
    语句体2;	
} 
…
else {
    语句体n+1;
}
```

执行流程：

①首先计算关系表达式1的值

②如果值为true就执行语句体1；如果值为false就计算关系表达式2的值

③如果值为true就执行语句体2；如果值为false就计算关系表达式3的值

④…

⑤如果没有任何关系表达式为true，就执行语句体n+1。

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1609771145140-1405f26d-5008-48eb-ae40-9ab95ffd0a08.png)

### Switch

- `switch`语句可以做多重选择，然后执行匹配的`case`语句后续代码。

- `switch`的计算结果必须是整型、字符串或枚举类型；

- 如果switch中得case，没有对应break的话，则会出现case穿透的现象。注意千万不要漏写`break`，建议打开`fall-through`警告；

- 总是写上`default`，建议打开`missing default`警告；

- 从Java 14开始，`switch`语句正式升级为表达式，不再需要`break`，并且允许使用`yield`返回值。

```java
switch (表达式) {
	case 1:
		语句体1;
		break;
	case 2:
		语句体2;
		break;
	...
	default:
		语句体n+1;
		break;
}
```

### for循环

- for循环格式：

```java
for (初始化语句;条件判断语句;条件控制语句) {
	循环体语句;
}
```

- 执行流程：
  - 执行初始化语句
  - 执行条件判断语句，看其结果是true还是false
           如果是false，循环结束，如果是true，继续执行
  - 执行循环体语句
  - 执行条件控制语句
  - 回到②继续

### while循环

- while循环完整格式：

```java
while (条件判断语句) {
	循环体语句;
    条件控制语句;
}
```

- while循环执行流程：

  - 执行初始化语句

  - 执行条件判断语句，看其结果是true还是false
           

  - 如果是false，循环结束，若是true，继续执行

  - 执行循环体语句
  
- 执行条件控制语句
  
- 回到②继续

**do...while循环结构**

- 完整格式：

```java
do {
	循环体语句;
	条件控制语句;
}while(条件判断语句);
```

执行流程：

① 执行初始化语句

② 执行循环体语句

③ 执行条件控制语句

④ 执行条件判断语句，看其结果是true还是false，如果是false，循环结束；如果是true，继续执行

⑤ 回到②继续

### break和continue

- `break`语句可以跳出当前循环；
- `break`语句通常配合`if`，在满足条件时提前结束整个循环；
- `break`语句总是跳出最近的一层循环；
- `continue`语句可以提前结束本次循环；
- `continue`语句通常配合`if`，在满足条件时提前结束本次循环。

#  面向对象编程

## 面向对象基础

### 类与对象

- 类

- - 类的理解

- - - 类是对现实生活中一类具有共同属性和行为的事物的抽象

- - - 类是对象的数据类型，类是具有相同属性和行为的一组对象的集合

- - - 简单理解：类就是对现实事物的一种描述

- - 类的组成

- - - 属性：指事物的特征
    - 行为：指事物能执行的操作

- 类和对象的关系

- - 类：类是对现实生活中一类具有共同属性和行为的事物的抽象

- - 对象：是能够看得到摸的着的真实存在的实体

- - 简单理解：**类是对事物的一种描述，对象则为具体存在的事物**

- **成员变量与局部变量**

- - 类中位置不同：成员变量（类中方法外）局部变量（方法内部或方法声明上）

- - 内存中位置不同：成员变量（堆内存）局部变量（栈内存）

- - 生命周期不同：成员变量（随着对象的存在而存在，随着对象的消失而消失）局部变量（随着方法的调用而存在，醉着方法的调用完毕而消失）

- - 初始化值不同：成员变量（有默认初始化值）局部变量（没有默认初始化值，必须先定义，赋值才能使用）

- 

### 方法

​	方法（method）是将具有独立功能的代码块组织成为一个整体，使其具有特殊功能的代码集

- 注意：

- - 方法必须先创建才可以使用，该过程成为方法定义

- - 方法创建后并不是直接可以运行的，需要手动使用后，才执行，该过程成为方法调用

- 形参：方法定义中的参数，等同于变量定义格式

- 实参：方法调用中的参数，等同于使用变量或常量

**无返回值无参方法：**

```java
public static void 方法名 (   ) {
	// 方法体;
}
```

**无返回值有参方法：**

```java
public static void 方法名 (参数1, 参数2, 参数3...) {
	方法体;
}
```

**带返回值方法定义：**

```java
public static 数据类型 方法名 ( 参数 ) { 
	return 数据 ;
}
```

### 构造方法

- 实例在创建时通过`new`操作符会调用其对应的构造方法，构造方法用于初始化实例；

- 没有定义构造方法时，编译器会自动创建一个默认的无参数构造方法；

- 可以定义多个构造方法，编译器根据参数自动判断；

- 可以在一个构造方法内部调用另一个构造方法，便于代码复用。

### **方法重载**

- 方法重载概念
  方法重载指同一个类中定义的多个方法之间的关系，满足下列条件的多个方法相互构成重载

- - 多个方法在同一个类中

- - 多个方法具有相同的方法名

- - 多个方法的参数不相同，类型不同或者数量不同

- 注意：

- - 重载仅对应方法的定义，与方法的调用无关，调用方式参照标准格式

- - **重载仅针对同一个类中方法的名称与参数进行识别，与返回值无关**，换句话说不能通过返回值来判定两个方法是否相互构成重载

### 继承

- 继承是面向对象编程的一种强大的代码复用方式；
- Java只允许单继承，所有类最终的根类是`Object`；
- `protected`允许子类访问父类的字段和方法；
- 子类的构造方法可以通过`super()`调用父类的构造方法；
- 可以安全地向上转型为更抽象的类型；
- 可以强制向下转型，最好借助`instanceof`判断；
- 子类和父类的关系是is，has关系不能用继承。

### 多态

- 子类可以覆写父类的方法（Override），覆写在子类中改变了父类方法的行为；
- Java的方法调用总是作用于运行期对象的实际类型，这种行为称为多态；
- `final`修饰符有多种作用：
  - `final`修饰的方法可以阻止被覆写；
  - `final`修饰的class可以阻止被继承；
  - `final`修饰的field必须在创建对象时初始化，随后不可修改。

### 抽象类

- 通过`abstract`定义的方法是抽象方法，它只有定义，没有实现。抽象方法定义了子类必须实现的接口规范；
- 定义了抽象方法的class必须被定义为抽象类，从抽象类继承的子类必须实现抽象方法；
- 如果不实现抽象方法，则该子类仍是一个抽象类；
- 面向抽象编程使得调用者只关心抽象方法的定义，不关心子类的具体实现。

### 接口

Java的接口特指`interface`的定义，表示一个接口类型和一组方法签名，而编程接口泛指接口规范，如方法签名，数据格式，网络协议等。

抽象类和接口的对比如下：

|            | abstractclass        | interface                   |
| ---------- | -------------------- | --------------------------- |
| 继承       | 只能extends一个class | 可以implements多个interface |
| 字段       | 可以定义实例字段     | 不能定义实例字段            |
| 抽象方法   | 可以定义抽象方法     | 可以定义抽象方法            |
| 非抽象方法 | 可以定义非抽象方法   | 可以定义default方法         |

总结：

- Java的接口（interface）定义了纯抽象规范，一个类可以实现多个接口；

- 接口也是数据类型，适用于向上转型和向下转型；

- 接口的所有方法都是抽象方法，接口不能定义实例字段；

- 接口可以定义`default`方法（JDK>=1.8）。

### 静态字段和静态方法

- 静态字段属于所有实例“共享”的字段，实际上是属于`class`的字段；
- 调用静态方法不需要实例，无法访问`this`，但可以访问静态字段和其他静态方法；
- 静态方法常用于工具类和辅助方法。

### 包

- Java内建的`package`机制是为了避免`class`命名冲突；

- JDK的核心类使用`java.lang`包，编译器会自动导入；

- JDK的其它常用类定义在`java.util.*`，`java.math.*`，`java.text.*`，……；

- 包名推荐使用倒置的域名，例如`org.apache`。

### 作用域

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1591781912346-3c29035a-9531-4cee-af74-a2190de10c5c.png)

**总结：**

- Java内建的访问权限包括`public`、`protected`、`private`和`package`权限；

- Java在方法内部定义的变量是局部变量，局部变量的作用域从变量声明开始，到一个块结束；

- `final`修饰符不是访问权限，它可以修饰`class`、`field`和`method`；

- 一个`.java`文件只能包含一个`public`类，但可以包含多个非`public`类。

### 内部类

Java的内部类可分为Inner Class、Anonymous Class和Static Nested Class三种：

- Inner Class和Anonymous Class本质上是相同的，都必须依附于Outer Class的实例，即隐含地持有`Outer.this`实例，并拥有Outer Class的`private`访问权限；
- Static Nested Class是独立类，但拥有Outer Class的`private`访问权限。

### classpath和jar

JVM通过环境变量`classpath`决定搜索`class`的路径和顺序；

不推荐设置系统环境变量`classpath`，始终建议通过`-cp`命令传入；

jar包相当于目录，可以包含很多`.class`文件，方便下载和使用；

`MANIFEST.MF`文件可以提供jar包的信息，如`Main-Class`，这样可以直接运行jar包。

### 模块

Java 9引入的模块目的是为了管理依赖；

使用模块可以按需打包JRE；

使用模块对类的访问权限有了进一步限制。

## Java核心类

### 字符串和编码

**String**

在Java中，`String`是一个引用类型，它本身也是一个`class`。但是，Java编译器对`String`有特殊处理，即可以直接用`"..."`来表示一个字符串。

- 字符串比较：equals()

- 字符串去除首尾空包：trim()

- 字符串拼接：join()

- 字符串分割：spilt()

- 字符串替换：replace()

- 字符串格式化：format()

- 字符串装箱：valueof()

**以较新版本的JDK为例子**

```java
public final class String {
        private final byte[] value;
        private final byte coder; // 0 = LATIN1, 1 = UTF16
    }
```

Java字符串的一个重要特点就是**字符串不可变**。这种不可变性是通过内部的`private final byte[]`字段，以及没有任何修改`byte[]`的方法实现的。

**总结：**

- Java字符串`String`是不可变对象；
- 字符串操作不改变原字符串内容，而是返回新字符串；
- 常用的字符串操作：提取子串、查找、替换、大小写转换等；
- Java使用Unicode编码表示`String`和`char`；
- 转换编码就是将`String`和`byte[]`转换，需要指定编码；
- 转换为`byte[]`时，始终优先考虑`UTF-8`编码。

### StringBuilder

为了能高效拼接字符串，Java标准库提供了`StringBuilder`，它是一个可变对象，可以预分配缓冲区，这样，往`StringBuilder`中新增字符时，不会创建新的临时对象。

**总结：**

- `StringBuilder`是可变对象，用来高效拼接字符串；

- `StringBuilder`可以支持链式操作，实现链式操作的关键是返回实例本身；

- `StringBuffer`是`StringBuilder`的线程安全版本，现在很少使用。

### StringJoiner

用指定分隔符拼接字符串数组时，使用`StringJoiner`或者`String.join()`更方便；

用`StringJoiner`拼接字符串时，还可以额外附加一个“开头”和“结尾”。

### 包装类型

Java核心库为每种基本类型都提供了对应的包装类型：

| 基本类型 | 对应的引用类型      |
| -------- | ------------------- |
| boolean  | java.lang.Boolean   |
| byte     | java.lang.Byte      |
| short    | java.lang.Short     |
| int      | java.lang.Integer   |
| long     | java.lang.Long      |
| float    | java.lang.Float     |
| double   | java.lang.Double    |
| char     | java.lang.Character |

**所有的包装类型都是不变类。**

**总结：**

- Java核心库提供的包装类型可以把基本类型包装为`class`；

- 自动装箱和自动拆箱都是在编译期完成的（JDK>=1.5）；

- 装箱和拆箱会影响执行效率，且拆箱时可能发生`NullPointerException`；

- 包装类型的比较必须使用`equals()`；

- 整数和浮点数的包装类型都继承自`Number`；

- 包装类型提供了大量实用方法。

### JavaBean

在Java中，有很多`class`的定义都符合这样的规范：

- 若干`private`实例字段；
- 通过`public`方法来读写实例字段。

**总结：**

- JavaBean是一种符合命名规范的`class`，它通过`getter`和`setter`来定义属性；

- 属性是一种通用的叫法，并非Java语法规定；

- 可以利用IDE快速生成`getter`和`setter`；

- 使用`Introspector.getBeanInfo()`可以获取属性列表。

### 枚举类

`enum`类型的每个常量在JVM中只有一个唯一实例，所以可以直接用`==`比较。

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1591781912801-5f556354-bdcb-4002-9785-c05b9b945b3e.png)

**总结：**

- Java使用`enum`定义枚举类型，它被编译器编译为`final class Xxx extends Enum { … }`；

- 通过`name()`获取常量定义的字符串，注意不要使用`toString()`；

- 通过`ordinal()`返回常量定义的顺序（无实质意义）；

- 可以为`enum`编写构造方法、字段和方法

- `enum`的构造方法要声明为`private`，字段强烈建议声明为`final`；

- `enum`适合用在`switch`语句中。

### 记录类

```java
    public record Point(int x, int y) {}
```

**使用`record`关键字，可以一行写出一个不变类。**

从Java 14开始，提供新的`record`关键字，可以非常方便地定义Data Class：

- 使用`record`定义的是不变类；
- 可以编写Compact Constructor对参数进行验证；
- 可以定义静态方法。

### BigInteger

在Java中，由CPU原生提供的整型最大范围是64位`long`型整数。使用`long`型整数可以直接通过CPU指令进行计算，速度非常快。

**相关计算得调用此类的API方法。**

`BigInteger`和`Integer`、`Long`一样，也是不可变类，并且也继承自`Number`类。因为`Number`定义了转换为基本类型的几个方法：

```java
转换为`byte`：`byteValue()`
转换为`short`：`shortValue()`
转换为`int`：`intValue()`
转换为`long`：`longValue()`
转换为`float`：`floatValue()`
转换为`double`：`doubleValue()`
```

**总结：**

- `BigInteger`用于表示任意大小的整数；

- `BigInteger`是不变类，并且继承自`Number`；

- 将`BigInteger`转换成基本类型时可使用`longValueExact()`等方法保证结果准确。

### BigDecimal

`BigDecimal`用于表示精确的小数，常用于财务计算；

**相关计算得调用此类的API方法。**

比较`BigDecimal`的值是否相等，必须使用`compareTo()`而不能使用`equals()`。

### 常用工具类

Java提供的常用工具类有：

- Math：数学计算
- Random：生成伪随机数
- SecureRandom：生成安全的随机数

**Math：**

```java
    Math.abs(-100); // 100
    Math.max(100, 99); // 100
    Math.min(1.2, 2.3); // 1.2
    Math.pow(2, 10); // 2的10次方=1024
    Math.exp(2); // 7.389...
    Math.log(4); // 1.386...
    Math.log10(100); // 2
    Math.sin(3.14); // 0.00159...
    Math.cos(3.14); // -0.9999...
    Math.tan(3.14); // -0.0015...
    Math.asin(1.0); // 1.57079...
    Math.acos(1.0); // 0.0
    Math.random(); // 0.53907... 每次都不一样

```

**Random：**

```java
    Random r = new Random();
    r.nextInt(); // 2071575453,每次都不一样
    r.nextInt(10); // 5,生成一个[0,10)之间的int
    r.nextLong(); // 8811649292570369305,每次都不一样
    r.nextFloat(); // 0.54335...生成一个[0,1)之间的float
    r.nextDouble(); // 0.3716...生成一个[0,1)之间的double
```

**SecureRandom：**

有伪随机数，就有真随机数。实际上真正的真随机数只能通过量子力学原理来获取，而我们想要的是一个不可预测的安全的随机数，`SecureRandom`就是用来创建安全的随机数的：

```java
    SecureRandom sr = new SecureRandom();
    System.out.println(sr.nextInt(100));
```

# 异常处理

## Java异常

对于异常的处理：

- 方法一：约定返回错误码。
- 方法二：在语言层面上提供一个异常处理机制。Java内置了一套异常处理机制，总是使用异常来表示错误。异常是一种`class`，因此它本身带有类型信息。异常可以在任何地方抛出，但只需要在上层捕获，这样就和方法调用分离了。

从继承关系可知：`Throwable`是异常体系的根，它继承自`Object`。`Throwable`有两个体系：`Error`和`Exception`，`Error`表示严重的错误，程序对此一般无能为力，例如：

- `OutOfMemoryError`：内存耗尽
- `NoClassDefFoundError`：无法加载某个Class
- `StackOverflowError`：栈溢出

而`Exception`则是运行时的错误，它可以被捕获并处理。

某些异常是应用程序逻辑处理的一部分，应该捕获并处理。例如：

- `NumberFormatException`：数值类型的格式错误
- `FileNotFoundException`：未找到文件
- `SocketException`：读取网络失败

还有一些异常是程序逻辑编写不对造成的，应该修复程序本身。例如：

- `NullPointerException`：对某个`null`的对象调用方法或字段
- `IndexOutOfBoundsException`：数组索引越界

`Exception`又分为两大类：

1. `RuntimeException`以及它的子类；
2. 非`RuntimeException`（包括`IOException`、`ReflectiveOperationException`等等）

Java规定：

- 必须捕获的异常，包括`Exception`及其子类，但不包括`RuntimeException`及其子类，这种类型的异常称为Checked Exception。
- 不需要捕获的异常，包括`Error`及其子类，`RuntimeException`及其子类。

**总结：**

Java使用异常来表示错误，并通过`try ... catch`捕获异常；

Java的异常是`class`，并且从`Throwable`继承；

`Error`是无需捕获的严重错误，`Exception`是应该捕获的可处理的错误；

`RuntimeException`无需强制捕获，非`RuntimeException`（Checked Exception）需强制捕获，或者用`throws`声明；

不推荐捕获了异常但不进行任何处理。

## 捕获异常

在Java中，凡是可能抛出异常的语句，都可以用`try ... catch`捕获。把可能发生异常的语句放在`try { ... }`中，然后使用`catch`捕获对应的`Exception`及其子类。

**多Catch语句**

可以使用多个`catch`语句，每个`catch`分别捕获对应的`Exception`及其子类。JVM在捕获到异常后，会从上到下匹配`catch`语句，匹配到某个`catch`后，执行`catch`代码块，然后*不再*继续匹配。

存在多个`catch`的时候，`catch`的顺序非常重要：子类必须写在前面。

```java
    public static void main(String[] args) {
        try {
            process1();
            process2();
            process3();
        } catch (UnsupportedEncodingException e) {
            System.out.println("Bad encoding");
        } catch (IOException e) {
            System.out.println("IO error");
        }
    }
```

**finally语句**

`finally`有几个特点：

1. `finally`语句不是必须的，可写可不写；
2. `finally`总是最后执行。

如果没有发生异常，就正常执行`try { ... }`语句块，然后执行`finally`。如果发生了异常，就中断执行`try { ... }`语句块，然后跳转执行匹配的`catch`语句块，最后执行`finally`。

可见，`finally`是用来保证一些代码必须执行的。

**总结：**

使用`try ... catch ... finally`时：

- 多个`catch`语句的匹配顺序非常重要，子类必须放在前面；
- `finally`语句保证了有无异常都会执行，它是可选的；
- 一个`catch`语句也可以匹配多个非继承关系的异常。

## 抛出异常

当某个方法抛出了异常时，如果当前方法没有捕获异常，异常就会被抛到上层调用方法，直到遇到某个`try ... catch`被捕获为止。

抛出异常分两步：

1. 创建某个`Exception`的实例；
2. 用`throw`语句抛出。

**小结**

- 调用`printStackTrace()`可以打印异常的传播栈，对于调试非常有用；

- 捕获异常并再次抛出新的异常时，应该持有原始异常信息；

- 通常不要在`finally`中抛出异常。如果在`finally`中抛出异常，应该原始异常加入到原有异常中。调用方可通过`Throwable.getSuppressed()`获取所有添加的`Suppressed Exception`。

## 自定义异常

```java
public class BaseException extends RuntimeException {
        public BaseException() {
            super();
        }
    
        public BaseException(String message, Throwable cause) {
            super(message, cause);
        }
    
        public BaseException(String message) {
            super(message);
        }
    
        public BaseException(Throwable cause) {
            super(cause);
        }
    }
```

**总结：**

- 抛出异常时，尽量复用JDK已定义的异常类型；

- 自定义异常体系时，推荐从`RuntimeException`派生“根异常”，再派生出业务异常；

- 自定义异常时，应该提供多种构造方法。

## NullPointerException

`NullPointerException`即空指针异常，俗称NPE。如果一个对象为`null`，调用其方法或访问其字段就会产生`NullPointerException`，这个异常通常是由JVM抛出的。

**处理**

`NullPointerException`是一种代码逻辑错误，遇到`NullPointerException`，遵循原则是早暴露，早修复，严禁使用`catch`来隐藏这种编码错误：

```java
    try {
        transferMoney(from, to, amount);
    } catch (NullPointerException e) {
    }
```

**定位**

增强的`NullPointerException`详细信息是Java 14新增的功能，但默认是关闭的，我们可以给JVM添加一个`-XX:+ShowCodeDetailsInExceptionMessages`参数启用它：

```java
java -XX:+ShowCodeDetailsInExceptionMessages Main.java
```

**总结**

- `NullPointerException`是Java代码常见的逻辑错误，应当早暴露，早修复；

- 可以启用Java 14的增强异常信息来查看`NullPointerException`的详细错误信息。

## 使用断言

断言（Assertion）是一种调试程序的方式。在Java中，使用`assert`关键字来实现断言。

```java
    public static void main(String[] args) {
        double x = Math.abs(-123.45);
        assert x >= 0  : "x must >= 0";
        System.out.println(x);
    }
```

语句`assert x >= 0;`即为断言，断言条件`x >= 0`预期为`true`。如果计算结果为`false`，则断言失败，抛出`AssertionError`。

特点是：断言失败时会抛出`AssertionError`，导致程序结束退出。因此，断言不能用于可恢复的程序错误，只应该用于开发和测试阶段。

**总结：**

- 断言是一种调试方式，断言失败会抛出`AssertionError`，只能在开发和测试阶段启用断言；

- 对可恢复的错误不能使用断言，而应该抛出异常；

- 断言很少被使用，更好的方法是编写单元测试。

## 使用JDK Logging

日志就是Logging，它的目的是为了取代`System.out.println()`。

输出日志，而不是用`System.out.println()`，有以下几个好处：

1. 可以设置输出样式，避免自己每次都写`"ERROR: " + var`；
2. 可以设置输出级别，禁止某些级别输出。例如，只输出错误日志；
3. 可以被重定向到文件，这样可以在程序运行结束后查看日志；
4. 可以按包名控制日志级别，只输出某些包打的日志；
5. 可以……

Java标准库内置了日志包`java.util.logging`，我们可以直接用：

```java
    // logging
    import java.util.logging.Level;
    import java.util.logging.Logger;
    ----
    public class Hello {
        public static void main(String[] args) {
            Logger logger = Logger.getGlobal();
            logger.info("start process...");
            logger.warning("memory is running out...");
            logger.fine("ignored.");
            logger.severe("process will be terminated...");
        }
    }
```

使用日志最大的好处是：它自动打印了时间、调用类、调用方法等很多有用的信息。

日志的输出可以设定级别。JDK的Logging定义了7个日志级别，从严重到普通：

- SEVERE
- WARNING
- INFO
- CONFIG
- FINE
- FINER
- FINEST

因为默认级别是INFO，因此，INFO级别以下的日志，不会被打印出来。使用日志级别的好处在于，调整级别，就可以屏蔽掉很多调试相关的日志输出。

使用Java标准库内置的Logging有以下局限：

- Logging系统在JVM启动时读取配置文件并完成初始化，一旦开始运行`main()`方法，就无法修改配置；

- 配置不太方便，需要在JVM启动时传递参数`-Djava.util.logging.config.file=<config-file-name>`。

**总结：**

- 日志是为了替代`System.out.println()`，可以定义格式，重定向到文件等；

- 日志可以存档，便于追踪问题；

- 日志记录可以按级别分类，便于打开或关闭某些级别；

- 可以根据配置文件调整日志，无需修改代码；

- Java标准库提供了`java.util.logging`来实现日志功能。

## 使用CommonsLogging

和Java标准库提供的日志不同，Commons Logging是一个第三方日志库，它是由Apache创建的日志模块。

Commons Logging的特色是，可挂接不同的日志系统，并通过配置文件指定挂接的日志系统。默认情况下，Commons Loggin自动搜索并使用Log4j，如果没有找到Log4j，再使用JDK Logging。

```java
    import org.apache.commons.logging.Log;
    import org.apache.commons.logging.LogFactory;
    ----
    public class Main {
        public static void main(String[] args) {
            Log log = LogFactory.getLog(Main.class);
            log.info("start...");
            log.warn("end.");
        }
    }
```

Commons Logging定义了6个日志级别：

- FATAL
- ERROR
- WARNING
- INFO
- DEBUG
- TRACE

默认级别是`INFO`。

使用Commons Logging时，如果在静态方法中引用`Log`，通常直接定义一个静态类型变量：

```java
    // 在静态方法中引用Log:
    public class Main {
        static final Log log = LogFactory.getLog(Main.class);
    
        static void foo() {
            log.info("foo");
        }
    }
```

**总结：**

- Commons Logging是使用最广泛的日志模块；

- Commons Logging的API非常简单；

- Commons Logging可以自动检测并使用其他日志模块。

## 使用Log4j

真正的“日志实现”可以使用Log4j。

当我们使用Log4j输出一条日志时，Log4j自动通过不同的Appender把同一条日志输出到不同的目的地。

- console：输出到屏幕；
- file：输出到文件；
- socket：通过网络输出到远程计算机；
- jdbc：输出到数据库

```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <Configuration>
    	<Properties>
            <!-- 定义日志格式 -->
    		<Property name="log.pattern">%d{MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36}%n%msg%n%n</Property>
            <!-- 定义文件名变量 -->
    		<Property name="file.err.filename">log/err.log</Property>
    		<Property name="file.err.pattern">log/err.%i.log.gz</Property>
    	</Properties>
        <!-- 定义Appender，即目的地 -->
    	<Appenders>
            <!-- 定义输出到屏幕 -->
    		<Console name="console" target="SYSTEM_OUT">
                <!-- 日志格式引用上面定义的log.pattern -->
    			<PatternLayout pattern="${log.pattern}" />
    		</Console>
            <!-- 定义输出到文件,文件名引用上面定义的file.err.filename -->
    		<RollingFile name="err" bufferedIO="true" fileName="${file.err.filename}" filePattern="${file.err.pattern}">
    			<PatternLayout pattern="${log.pattern}" />
    			<Policies>
                    <!-- 根据文件大小自动切割日志 -->
    				<SizeBasedTriggeringPolicy size="1 MB" />
    			</Policies>
                <!-- 保留最近10份 -->
    			<DefaultRolloverStrategy max="10" />
    		</RollingFile>
    	</Appenders>
    	<Loggers>
    		<Root level="info">
                <!-- 对info级别的日志，输出到console -->
    			<AppenderRef ref="console" level="info" />
                <!-- 对error级别的日志，输出到err，即上面定义的RollingFile -->
    			<AppenderRef ref="err" level="error" />
    		</Root>
    	</Loggers>
    </Configuration>
```

**总结：**

- 通过Commons Logging实现日志，不需要修改代码即可使用Log4j；

- 使用Log4j只需要把log4j2.xml和相关jar放入classpath；

- 如果要更换Log4j，只需要移除log4j2.xml和相关jar；

- 只有扩展Log4j时，才需要引用Log4j的接口（例如，将日志加密写入数据库的功能，需要自己开发）

## 使用SLF4J和Logback

SLF4J的日志接口传入的是一个带占位符的字符串，用后面的变量自动替换占位符

```java
    int score = 99;
    p.setScore(score);
    logger.info("Set score {} for Person {} ok.", score, p.getName());
```

对比一下Commons Logging和SLF4J的接口：

| CommonsLogging                        | SLF4J                   |
| ------------------------------------- | ----------------------- |
| org.apache.commons.logging.Log        | org.slf4j.Logger        |
| org.apache.commons.logging.LogFactory | org.slf4j.LoggerFactory |

```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
    
    	<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
    		<encoder>
    			<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    		</encoder>
    	</appender>
    
    	<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    		<encoder>
    			<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    			<charset>utf-8</charset>
    		</encoder>
    		<file>log/output.log</file>
    		<rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
    			<fileNamePattern>log/output.log.%i</fileNamePattern>
    		</rollingPolicy>
    		<triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
    			<MaxFileSize>1MB</MaxFileSize>
    		</triggeringPolicy>
    	</appender>
    
    	<root level="INFO">
    		<appender-ref ref="CONSOLE" />
    		<appender-ref ref="FILE" />
    	</root>
    </configuration>
```

**总结：**

- SLF4J和Logback可以取代Commons Logging和Log4j；

- 始终使用SLF4J的接口写入日志，使用Logback只需要配置，不需要修改代码。

# 反射

## Class类

除了`int`等基本类型外，Java的其他类型全部都是`class`（包括`interface`）。例如：

- `String`
- `Object`
- `Runnable`
- `Exception`

`class`是由JVM在执行过程中动态加载的。JVM在第一次读取到一种`class`类型时，将其加载进内存。

每加载一种`class`，JVM就为其创建一个`Class`类型的实例，并关联起来。注意：这里的`Class`类型是一个名叫`Class`的`class`。

**通过`Class`实例获取`class`信息的方法称为反射（Reflection）,`Class`实例在JVM中是唯一的。**

```java
Class cls = String.class; //直接通过一个class的静态变量class获取
String s = "Hello";
Class cls = s.getClass(); //方通过该实例变量提供的getClass()方法获取：
Class cls = Class.forName("java.lang.String"); //class的完整类名，可以通过静态方法Class.forName()获取


```

**动态加载**：JVM在执行Java程序的时候，并不是一次性把所有用到的class全部加载到内存，而是第一次需要用到class时才加载。

**总结：**

- JVM为每个加载的`class`及`interface`创建了对应的`Class`实例来保存`class`及`interface`的所有信息；

- 获取一个`class`对应的`Class`实例后，就可以获取该`class`的所有信息；

- 通过Class实例获取`class`信息的方法称为反射（Reflection）；

- JVM总是动态加载`class`，可以在运行期根据条件来控制加载class。

## 访问字段

对任意的一个`Object`实例，只要我们获取了它的`Class`，就可以获取它的一切信息。

我们先看看如何通过`Class`实例获取字段信息。`Class`类提供了以下几个方法来获取字段：

- Field getField(name)：根据字段名获取某个public的field（包括父类）
- Field getDeclaredField(name)：根据字段名获取当前类的某个field（不包括父类）
- Field[] getFields()：获取所有public的field（包括父类）
- Field[] getDeclaredFields()：获取当前类的所有field（不包括父类）

**总结：**

- Java的反射API提供的`Field`类封装了字段的所有信息：

- 通过`Class`实例的方法可以获取`Field`实例：`getField()`，`getFields()`，`getDeclaredField()`，`getDeclaredFields()`；

- 通过Field实例可以获取字段信息：`getName()`，`getType()`，`getModifiers()`；

- 通过Field实例可以读取或设置某个对象的字段，如果存在访问限制，要首先调用`setAccessible(true)`来访问非`public`字段。

- 通过反射读写字段是一种非常规方法，它会破坏对象的封装。

## 调用方法

可以通过`Class`实例获取所有`Method`信息。`Class`类提供了以下几个方法来获取`Method`：

- `Method getMethod(name, Class...)`：获取某个`public`的`Method`（包括父类）
- `Method getDeclaredMethod(name, Class...)`：获取当前类的某个`Method`（不包括父类）
- `Method[] getMethods()`：获取所有`public`的`Method`（包括父类）
- `Method[] getDeclaredMethods()`：获取当前类的所有`Method`（不包括父类）

**总结：**

- Java的反射API提供的Method对象封装了方法的所有信息：

- 通过`Class`实例的方法可以获取`Method`实例：`getMethod()`，`getMethods()`，`getDeclaredMethod()`，`getDeclaredMethods()`；

- 通过`Method`实例可以获取方法信息：`getName()`，`getReturnType()`，`getParameterTypes()`，`getModifiers()`；

- 通过`Method`实例可以调用某个对象的方法：`Object invoke(Object instance, Object... parameters)`；

- 通过设置`setAccessible(true)`来访问非`public`方法；

- 通过反射调用方法时，仍然遵循多态原则。

## 调用构造方法

如果通过反射来创建新的实例，可以调用Class提供的newInstance()方法：

```java
Person p = Person.class.newInstance();    

// 获取构造方法Integer(int):
Constructor cons1 = Integer.class.getConstructor(int.class);
// 调用构造方法:
Integer n1 = (Integer) cons1.newInstance(123);
```

调用Class.newInstance()的局限是，它只能调用该类的public无参数构造方法。如果构造方法带有参数，或者不是public，就无法直接通过Class.newInstance()来调用。

**为了调用任意的构造方法，Java的反射API提供了Constructor对象，它包含一个构造方法的所有信息，可以创建一个实例。**

**总结：**

- `Constructor`对象封装了构造方法的所有信息；

- 通过`Class`实例的方法可以获取`Constructor`实例：`getConstructor()`，`getConstructors()`，`getDeclaredConstructor()`，`getDeclaredConstructors()`；

- 通过`Constructor`实例可以创建一个实例对象：`newInstance(Object... parameters)`； 通过设置`setAccessible(true)`来访问非`public`构造方法。



## 获取继承关系

通过`Class`对象可以获取继承关系：

- `Class getSuperclass()`：获取父类类型；
- `Class[] getInterfaces()`：获取当前类实现的所有接口。

通过`Class`对象的`isAssignableFrom()`方法可以判断一个向上转型是否可以实现。

## 动态代理

Java标准库提供了一种动态代理（Dynamic Proxy）的机制：可以在运行期动态创建某个`interface`的实例。

在运行期动态创建一个`interface`实例的方法如下：

1. 定义一个`InvocationHandler`实例，它负责实现接口的方法调用；
2. 通过`Proxy.newProxyInstance()`创建`interface`实例，它需要3个参数：
   1. 使用的`ClassLoader`，通常就是接口类的`ClassLoader`；
   2. 需要实现的接口数组，至少需要传入一个接口进去；
   3. 用来处理接口方法调用的`InvocationHandler`实例。
3. 将返回的`Object`强制转型为接口。

```java
    import java.lang.reflect.InvocationHandler;
    import java.lang.reflect.Method;
    import java.lang.reflect.Proxy;
    ----
    public class Main {
        public static void main(String[] args) {
            InvocationHandler handler = new InvocationHandler() {
                @Override
                public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                    System.out.println(method);
                    if (method.getName().equals("morning")) {
                        System.out.println("Good morning, " + args[0]);
                    }
                    return null;
                }
            };
            Hello hello = (Hello) Proxy.newProxyInstance(
                Hello.class.getClassLoader(), // 传入ClassLoader
                new Class[] { Hello.class }, // 传入要实现的接口
                handler); // 传入处理调用方法的InvocationHandler
            hello.morning("Bob");
        }
    }
    
    interface Hello {
        void morning(String name);
    }
```

**总结：**

- Java标准库提供了动态代理功能，允许在运行期动态创建一个接口的实例；

- 动态代理是通过`Proxy`创建代理对象，然后将接口方法“代理”给`InvocationHandler`完成的。

# 注解

注释会被编译器直接忽略，注解则可以被编译器打包进入class文件，因此，注解是一种用作标注的“元数据”。

**作用：**

从JVM的角度看，注解本身对代码逻辑没有任何影响，如何使用注解完全由工具决定。

## 使用注解

Java的注解可以分为三类：

第一类是由编译器使用的注解，例如：

- `@Override`：让编译器检查该方法是否正确地实现了覆写；
- `@SuppressWarnings`：告诉编译器忽略此处代码产生的警告。

这类注解不会被编译进入`.class`文件，它们在编译后就被编译器扔掉了。

第二类是由工具处理`.class`文件使用的注解，在加载class的时候，对class做动态修改，实现一些特殊的功能。这类注解会被编译进入`.class`文件，但加载结束后并不会存在于内存中。这类注解只被一些底层库使用，一般我们不必自己处理。

第三类是在程序运行期能够读取的注解，它们在加载后一直存在于JVM中，这也是最常用的注解。例如，一个配置了`@PostConstruct`的方法会在调用构造方法后自动被调用（这是Java代码读取该注解实现的功能，JVM并不会识别该注解）。

定义一个注解时，还可以定义配置参数。配置参数可以包括：

- 所有基本类型；
- String；
- 枚举类型；
- 基本类型、String、Class以及枚举的数组。

因为配置参数必须是常量，所以，上述限制保证了注解在定义时就已经确定了每个参数的值。

注解的配置参数可以有默认值，缺少某个配置参数时将使用默认值。

此外，大部分注解会有一个名为`value`的配置参数，对此参数赋值，可以只写常量，相当于省略了value参数。

如果只写注解，相当于全部使用默认值。

**总结：**

- 注解（Annotation）是Java语言用于工具处理的标注：

- 注解可以配置参数，没有指定配置的参数使用默认值；

- 如果参数名称是`value`，且只有一个参数，那么可以省略参数名称。

## 定义注解

Java语言使用`@interface`语法来定义注解（`Annotation`），它的格式如下：

```java
    public @interface Report {
        int type() default 0;
        String level() default "info";
        String value() default "";
    }
```

注解的参数类似无参数方法，可以用`default`设定一个默认值（强烈推荐）。最常用的参数应当命名为`value`。

**元注解：**

注解可修饰其他注解，称为元注解（meta annotation）。Java标准库已经定义了一些元注解。

#### @Target

最常用的元注解是`@Target`。使用`@Target`可以定义`Annotation`能够被应用于源码的哪些位置：

- 类或接口：`ElementType.TYPE`；
- 字段：`ElementType.FIELD`；
- 方法：`ElementType.METHOD`；
- 构造方法：`ElementType.CONSTRUCTOR`；
- 方法参数：`ElementType.PARAMETER`。

#### @Retention

另一个重要的元注解`@Retention`定义了`Annotation`的生命周期：

- 仅编译期：`RetentionPolicy.SOURCE`；
- 仅class文件：`RetentionPolicy.CLASS`；
- 运行期：`RetentionPolicy.RUNTIME`。

#### @Repeatable

使用`@Repeatable`这个元注解可以定义`Annotation`是否可重复。

#### @Inherited

使用`@Inherited`定义子类是否可继承父类定义的`Annotation`。`@Inherited`仅针对`@Target(ElementType.TYPE)`类型的`annotation`有效，并且仅针对`class`的继承，对`interface`的继承无效：

```JAVA
    @Target(ElementType.TYPE)
    @Retention(RetentionPolicy.RUNTIME)
    public @interface Report {
        int type() default 0;
        String level() default "info";
        String value() default "";
    }
```

**总结：**

- Java使用`@interface`定义注解：

- 可定义多个参数和默认值，核心参数使用`value`名称；

- 必须设置`@Target`来指定`Annotation`可以应用的范围；

- 应当设置`@Retention(RetentionPolicy.RUNTIME)`便于运行期读取该`Annotation`。

## 处理注解

Java的注解本身对代码逻辑没有任何影响。根据`@Retention`的配置：

- `SOURCE`类型的注解在编译期就被丢掉了；
- `CLASS`类型的注解仅保存在class文件中，它们不会被加载进JVM；
- `RUNTIME`类型的注解会被加载进JVM，并且在运行期可以被程序读取。

注解定义后也是一种`class`，所有的注解都继承自`java.lang.annotation.Annotation`，因此，读取注解，需要使用反射API。

Java提供的使用反射API读取`Annotation`的方法包括：

判断某个注解是否存在于`Class`、`Field`、`Method`或`Constructor`：

- `Class.isAnnotationPresent(Class)`
- `Field.isAnnotationPresent(Class)`
- `Method.isAnnotationPresent(Class)`
- `Constructor.isAnnotationPresent(Class)`

例如：

```java
    Person.class.isAnnotationPresent(Report.class);
```

使用反射API读取Annotation：

- `Class.getAnnotation(Class)`
- `Field.getAnnotation(Class)`
- `Method.getAnnotation(Class)`
- `Constructor.getAnnotation(Class)`

例如：

```java
    Report report = Person.class.getAnnotation(Report.class);
    int type = report.type();
    String level = report.level();
```

**总结:**

可以在运行期通过反射读取`RUNTIME`类型的注解，注意千万不要漏写`@Retention(RetentionPolicy.RUNTIME)`，否则运行期无法读取到该注解。

可以通过程序处理注解来实现相应的功能：

- 对JavaBean的属性值按规则进行检查；
- Junit会自动运行`@Test`标记的测试方法。

# 泛型

泛型就是编写模板代码来适应任意类型，使用泛型时，把泛型参数`<T>`替换为需要的class类型，可以省略编译器能自动推断出的类型

不指定泛型参数类型时，编译器会给出警告，且只能将`<T>`视为`Object`类型；可以在接口中定义泛型类型，实现此接口的类必须实现正确的泛型类型。

注意泛型的继承关系：可以把`ArrayList<Integer>`向上转型为`List<Integer>`（`T`不能变！），但不能把`ArrayList<Integer>`向上转型为`ArrayList<Number>`（`T`不能变成父类）。

```java
    public class Pair<T> {
        private T first;
        private T last;
        public Pair(T first, T last) {
            this.first = first;
            this.last = last;
        }
        public T getFirst() {
            return first;
        }
        public T getLast() {
            return last;
        }
    }
```

**编写泛型类时，要特别注意，泛型类型`<T>`不能用于静态方法；泛型还可以定义多种类型。**

Java语言的泛型实现方式是擦拭法（Type Erasure），虚拟机对泛型其实一无所知，所有的工作都是编译器做的。故有以下几点

- `<T>`不能是基本类型
- 无法取得带泛型的`Class`
- 无法判断带泛型的类型
- 不能实例化`T`类型

**通配符：**

extends要传入T的子类，而super要传入其父类。

作为方法参数，`<? extends T>`类型和`<? super T>`类型的区别在于：

- `<? extends T>`允许调用读方法`T get()`获取`T`的引用，但不允许调用写方法`set(T)`传入`T`的引用（传入`null`除外）；
- `<? super T>`允许调用写方法`set(T)`传入`T`的引用，但不允许调用读方法`T get()`获取`T`的引用（获取`Object`除外）。

**常见错误：**

```java
    // compile warning:
    Class clazz = String.class;
    String str = (String) clazz.newInstance();
    
    // no warning:
    Class<String> clazz = String.class;
    String str = clazz.newInstance();

    // 调用Class的getSuperclass()方法返回的Class类型是Class<? super T>：
    Class<? super String> sup = String.class.getSuperclass();
    
```

- 部分反射API是泛型，例如：`Class<T>`，`Constructor<T>`；

- 可以声明带泛型的数组，但不能直接创建带泛型的数组，必须强制转型；

- 可以通过`Array.newInstance(Class<T>, int)`创建`T[]`数组，需要强制转型；

- 同时使用泛型和可变参数时需要特别小心。

# 集合

## Collection

Java标准库自带的`java.util`包提供了集合类：`Collection`，它是除`Map`外所有其他集合类的根接口，Java集合使用统一的`Iterator`遍历。提供了以下三种类型的集合：

- `List`：一种有序列表的集合，例如，按索引排列的`Student`的`List`；
- `Set`：一种保证没有重复元素的集合，例如，所有无重复名称的`Student`的`Set`；
- `Map`：一种通过键值（key-value）查找的映射表集合，例如，根据`Student`的`name`查找对应`Student`的`Map`。

小部分集合类是遗留类，不应该继续使用：

- `Hashtable`：一种线程安全的`Map`实现；
- `Vector`：一种线程安全的`List`实现；
- `Stack`：基于`Vector`实现的`LIFO`的栈。

还有一小部分接口是遗留接口，也不应该继续使用：

- `Enumeration<E>`：已被`Iterator<E>`取代。

## List

`List<E>`接口，可以看到几个主要的接口方法：

- 在末尾添加一个元素：`boolean add(E e)`
- 在指定索引添加一个元素：`boolean add(int index, E e)`
- 删除指定索引的元素：`E remove(int index)`
- 删除某个元素：`boolean remove(Object e)`
- 获取指定索引的元素：`E get(int index)`
- 获取链表大小（包含元素的个数）：`int size()`

比较一下`ArrayList`和`LinkedList`：

|                     | ArrayList    | LinkedList           |
| ------------------- | ------------ | -------------------- |
| 获取指定元素        | 速度很快     | 需要从头开始查找元素 |
| 添加元素到末尾      | 速度很快     | 速度很快             |
| 在指定位置添加/删除 | 需要移动元素 | 不需要移动元素       |
| 内存占用            | 少           | 较大                 |

```java
//可添加null

//可用of创建
List<Integer> list = List.of(1,2,3);

//遍历推荐通过迭代器
List<String> list = List.of("apple", "pear", "banana");
for (Iterator<String> it = list.iterator(); it.hasNext(); ) {
     String s = it.next();
     System.out.println(s);
            }


```

对于JDK 11之前的版本，可以使用`Arrays.asList(T...)`方法把数组转换成`List`。

## equals方法

`equals()`方法要求我们必须满足以下条件：

- 自反性（Reflexive）：对于非`null`的`x`来说，`x.equals(x)`必须返回`true`；
- 对称性（Symmetric）：对于非`null`的`x`和`y`来说，如果`x.equals(y)`为`true`，则`y.equals(x)`也必须为`true`；
- 传递性（Transitive）：对于非`null`的`x`、`y`和`z`来说，如果`x.equals(y)`为`true`，`y.equals(z)`也为`true`，那么`x.equals(z)`也必须为`true`；
- 一致性（Consistent）：对于非`null`的`x`和`y`来说，只要`x`和`y`状态不变，则`x.equals(y)`总是一致地返回`true`或者`false`；
- 对`null`的比较：即`x.equals(null)`永远返回`false`。

```java
    public boolean equals(Object o) {
        if (o instanceof Person) {
            Person p = (Person) o;
            return this.name.equals(p.name) && this.age == p.age;
        }
        return false;
    }
```

如果不在`List`中查找元素，就不必覆写`equals()`方法。

## Map

`Map`是一种映射表，可以通过`key`快速查找`value`。

可以通过`for each`遍历`keySet()`，也可以通过`for each`遍历`entrySet()`，直接获取`key-value`。

最常用的一种`Map`实现是`HashMap`。

要正确使用`HashMap`，作为`key`的类必须正确覆写`equals()`和`hashCode()`方法；

一个类如果覆写了`equals()`，就必须覆写`hashCode()`，并且覆写规则是：

- 如果`equals()`返回`true`，则`hashCode()`返回值必须相等；
- 如果`equals()`返回`false`，则`hashCode()`返回值尽量不要相等。

实现`hashCode()`方法可以通过`Objects.hashCode()`辅助方法实现。

## EnumMap

如果作为key的对象是`enum`类型，那么，还可以使用Java集合库提供的一种`EnumMap`，它在内部以一个非常紧凑的数组存储value，并且根据`enum`类型的key直接定位到内部数组的索引，并不需要计算`hashCode()`，不但效率最高，而且没有额外的空间浪费。

```java
    import java.time.DayOfWeek;
    import java.util.*;
    ----
    public class Main {
        public static void main(String[] args) {
            Map<DayOfWeek, String> map = new EnumMap<>(DayOfWeek.class);
            map.put(DayOfWeek.MONDAY, "星期一");
            map.put(DayOfWeek.TUESDAY, "星期二");
            map.put(DayOfWeek.WEDNESDAY, "星期三");
            map.put(DayOfWeek.THURSDAY, "星期四");
            map.put(DayOfWeek.FRIDAY, "星期五");
            map.put(DayOfWeek.SATURDAY, "星期六");
            map.put(DayOfWeek.SUNDAY, "星期日");
            System.out.println(map);
            System.out.println(map.get(DayOfWeek.MONDAY));
        }
    }
```

**总结：**

- 如果`Map`的key是`enum`类型，推荐使用`EnumMap`，既保证速度，也不浪费空间。

- 使用`EnumMap`的时候，根据面向抽象编程的原则，应持有`Map`接口。

## TreeMap

内部会对Key进行排序，这种`Map`就是`SortedMap`。注意到`SortedMap`是接口，它的实现类是`TreeMap`。

使用`TreeMap`时，放入的Key必须实现`Comparable`接口。`String`、`Integer`这些类已经实现了`Comparable`接口，因此可以直接作为Key使用。作为Value的对象则没有任何要求。

如果作为Key的class没有实现`Comparable`接口，那么，必须在创建`TreeMap`时同时指定一个自定义排序算法：

```java
  import java.util.*;
    ----
    public class Main {
        public static void main(String[] args) {
            Map<Person, Integer> map = new TreeMap<>(new Comparator<Person>() {
                public int compare(Person p1, Person p2) {
                    return p1.name.compareTo(p2.name);
                }
            });
            map.put(new Person("Tom"), 1);
            map.put(new Person("Bob"), 2);
            map.put(new Person("Lily"), 3);
            for (Person key : map.keySet()) {
                System.out.println(key);
            }
            // {Person: Bob}, {Person: Lily}, {Person: Tom}
            System.out.println(map.get(new Person("Bob"))); // 2
        }
    }
    
    class Person {
        public String name;
        Person(String name) {
            this.name = name;
        }
        public String toString() {
            return "{Person: " + name + "}";
        }
    }
```

**总结：**

- `SortedMap`在遍历时严格按照Key的顺序遍历，最常用的实现类是`TreeMap`；

- 作为`SortedMap`的Key必须实现`Comparable`接口，或者传入`Comparator`；

- 要严格按照`compare()`规范实现比较逻辑，否则，`TreeMap`将不能正常工作。

## Properties

用`Properties`读取配置文件非常简单。Java默认配置文件以`.properties`为扩展名，每行以`key=value`表示，以`#`课开头的是注释。以下是一个典型的配置文件：

```properties
# setting.properties
last_open_file=/data/hello.txt
auto_save_interval=60
```

可以从文件系统读取这个`.properties`文件：

```java
    String f = "setting.properties";
    Properties props = new Properties();
    props.load(new java.io.FileInputStream(f));
    
    String filepath = props.getProperty("last_open_file");
    String interval = props.getProperty("auto_save_interval", "120");
```

可见，用`Properties`读取配置文件，一共有三步：

1. 创建`Properties`实例；
2. 调用`load()`读取文件；
3. 调用`getProperty()`获取配置。

调用`getProperty()`获取配置时，如果key不存在，将返回`null`。我们还可以提供一个默认值，这样，当key不存在的时候，就返回默认值。

也可以从classpath读取`.properties`文件，因为`load(InputStream)`方法接收一个`InputStream`实例，表示一个字节流，它不一定是文件流，也可以是从jar包中读取的资源流：

```java
Properties props = new Properties(); props.load(getClass().getResourceAsStream("/common/setting.properties"));
```

**写入配置文件**

如果通过`setProperty()`修改了`Properties`实例，可以把配置写入文件，以便下次启动时获得最新配置。写入配置文件使用`store()`方法：

```java
Properties props = new Properties();    props.setProperty("url", "http://www.liaoxuefeng.com");    props.setProperty("language", "Java");    props.store(new FileOutputStream("C:\\conf\\setting.properties"), "这是写入的properties注释");
```

**总结：**

- Java集合库提供的`Properties`用于读写配置文件`.properties`。`.properties`文件可以使用UTF-8编码。

- 可以从文件系统、classpath或其他任何地方读取`.properties`文件。

- 读写`Properties`时，注意仅使用`getProperty()`和`setProperty()`方法，不要调用继承而来的`get()`和`put()`等方法。

## Set

只需要存储不重复的key，并不需要存储映射的value，那么就可以使用`Set`。

`Set`用于存储不重复的元素集合，它主要提供以下几个方法：

- 将元素添加进`Set<E>`：`boolean add(E e)`
- 将元素从`Set<E>`删除：`boolean remove(Object e)`
- 判断是否包含元素：`boolean contains(Object e)`

最常用的`Set`实现类是`HashSet`，实际上，`HashSet`仅仅是对`HashMap`的一个简单封装，它的核心代码如下：

```java
    public class HashSet<E> implements Set<E> {
        // 持有一个HashMap:
        private HashMap<E, Object> map = new HashMap<>();
    
        // 放入HashMap的value:
        private static final Object PRESENT = new Object();
    
        public boolean add(E e) {
            return map.put(e, PRESENT) == null;
        }
    
        public boolean contains(Object o) {
            return map.containsKey(o);
        }
    
        public boolean remove(Object o) {
            return map.remove(o) == PRESENT;
        }
    }
```

`Set`接口并不保证有序，而`SortedSet`接口则保证元素是有序的：

- `HashSet`是无序的，因为它实现了`Set`接口，并没有实现`SortedSet`接口；
- `TreeSet`是有序的，因为它实现了`SortedSet`接口。

**总结：**

`Set`用于存储不重复的元素集合：

- 放入`HashSet`的元素与作为`HashMap`的key要求相同；
- 放入`TreeSet`的元素与作为`TreeMap`的Key要求相同；

利用`Set`可以去除重复元素；

遍历`SortedSet`按照元素的排序顺序遍历，也可以自定义排序算法。

## Queue

队列（`Queue`）是一种经常使用的集合。`Queue`实际上是实现了一个先进先出（FIFO：First In First Out）的有序表。它和`List`的区别在于，`List`可以在任意位置添加和删除元素，而`Queue`只有两个操作：

- 把元素添加到队列末尾；
- 从队列头部取出元素。

在Java的标准库中，队列接口`Queue`定义了以下几个方法：

- `int size()`：获取队列长度；
- `boolean add(E)`/`boolean offer(E)`：添加元素到队尾；
- `E remove()`/`E poll()`：获取队首元素并从队列中删除；
- `E element()`/`E peek()`：获取队首元素但并不从队列中删除。

对于具体的实现类，有的Queue有最大队列长度限制，有的Queue没有。注意到添加、删除和获取队列元素总是有两个方法，这是因为在添加或获取元素失败时，这两个方法的行为是不同的。我们用一个表格总结如下：

|                    | throwException | 返回false或null    |
| ------------------ | -------------- | ------------------ |
| 添加元素到队尾     | add(E e)       | boolean offer(E e) |
| 取队首元素并删除   | E remove()     | E poll()           |
| 取队首元素但不删除 | E element()    | E peek()           |

```java
    // 这是一个List:
    List<String> list = new LinkedList<>();
    // 这是一个Queue:
    Queue<String> queue = new LinkedList<>();
    queue.offer("apple");
```

**总结：**

队列`Queue`实现了一个先进先出（FIFO）的数据结构：

- 通过`add()`/`offer()`方法将元素添加到队尾；
- 通过`remove()`/`poll()`从队首获取元素并删除；
- 通过`element()`/`peek()`从队首获取元素但不删除。

要避免把`null`添加到队列。

## PriorityQueue

`PriorityQueue`和`Queue`的区别在于，它的出队顺序与元素的优先级有关，对`PriorityQueue`调用`remove()`或`poll()`方法，返回的总是优先级最高的元素。

放入`PriorityQueue`的元素，必须实现`Comparable`接口，`PriorityQueue`会根据元素的排序顺序决定出队的优先级。

```java
    import java.util.Comparator;
    import java.util.PriorityQueue;
    import java.util.Queue;
    ----
    public class Main {
        public static void main(String[] args) {
            Queue<User> q = new PriorityQueue<>(new UserComparator());
            // 添加3个元素到队列:
            q.offer(new User("Bob", "A1"));
            q.offer(new User("Alice", "A2"));
            q.offer(new User("Boss", "V1"));
            System.out.println(q.poll()); // Boss/V1
            System.out.println(q.poll()); // Bob/A1
            System.out.println(q.poll()); // Alice/A2
            System.out.println(q.poll()); // null,因为队列为空
        }
    }
    
    class UserComparator implements Comparator<User> {
        public int compare(User u1, User u2) {
            if (u1.number.charAt(0) == u2.number.charAt(0)) {
                // 如果两人的号都是A开头或者都是V开头,比较号的大小:
                return u1.number.compareTo(u2.number);
            }
            if (u1.number.charAt(0) == 'V') {
                // u1的号码是V开头,优先级高:
                return -1;
            } else {
                return 1;
            }
        }
    }
    
    class User {
        public final String name;
        public final String number;
    
        public User(String name, String number) {
            this.name = name;
            this.number = number;
        }
    
        public String toString() {
            return name + "/" + number;
        }
    }
    
```

**总结：**

- `PriorityQueue`实现了一个优先队列：从队首获取元素时，总是获取优先级最高的元素。

- `PriorityQueue`默认按元素比较的顺序排序（必须实现`Comparable`接口），也可以通过`Comparator`自定义排序算法（元素就不必实现`Comparable`接口）。

## Deque

允许两头都进，两头都出，这种队列叫双端队列（Double Ended Queue），学名`Deque`。

Java集合提供了接口`Deque`来实现一个双端队列，它的功能是：

- 既可以添加到队尾，也可以添加到队首；
- 既可以从队首获取，又可以从队尾获取。

我们来比较一下`Queue`和`Deque`出队和入队的方法：

|                    | Queue                | Deque                         |
| ------------------ | -------------------- | ----------------------------- |
| 添加元素到队尾     | add(E e)/offer(E e)  | addLast(E e)/offerLast(E e)   |
| 取队首元素并删除   | E remove()/E poll()  | E removeFirst()/E pollFirst() |
| 取队首元素但不删除 | E element()/E peek() | E getFirst()/E peekFirst()    |
| 添加元素到队首     | 无                   | addFirst(E e)/offerFirst(E e) |
| 取队尾元素并删除   | 无                   | E removeLast()/E  pollLast()  |
| 取队尾元素但不删除 | 无                   | E getLast()/E peekLast()      |

**`Deque`是一个接口，它的实现类有`ArrayDeque`和`LinkedList`。**

**总结：**

`Deque`实现了一个双端队列（Double Ended Queue），它可以：

- 将元素添加到队尾或队首：`addLast()`/`offerLast()`/`addFirst()`/`offerFirst()`；
- 从队首／队尾获取元素并删除：`removeFirst()`/`pollFirst()`/`removeLast()`/`pollLast()`；
- 从队首／队尾获取元素但不删除：`getFirst()`/`peekFirst()`/`getLast()`/`peekLast()`；
- 总是调用`xxxFirst()`/`xxxLast()`以便与`Queue`的方法区分开；
- 避免把`null`添加到队列。

## Stack

`Stack`是这样一种数据结构：只能不断地往`Stack`中压入（push）元素，最后进去的必须最早弹出（pop）来。

`Stack`只有入栈和出栈的操作：

- 把元素压栈：`push(E)`；
- 把栈顶的元素“弹出”：`pop()`；
- 取栈顶元素但不弹出：`peek()`。

在Java中，我们用`Deque`可以实现`Stack`的功能：

- 把元素压栈：`push(E)`/`addFirst(E)`；
- 把栈顶的元素“弹出”：`pop()`/`removeFirst()`；
- 取栈顶元素但不弹出：`peek()`/`peekFirst()`。

**在Java中，我们用`Deque`可以实现`Stack`的功能，注意只调用`push()`/`pop()`/`peek()`方法，避免调用`Deque`的其他方法。不要使用遗留类`Stack`。**

## Iterator

通过`Iterator`对象遍历集合的模式称为迭代器，使用迭代器的好处在于，调用方总是以统一的方式遍历各种集合类型，而不必关系它们内部的存储结构。

如果我们自己编写了一个集合类，想要使用`for each`循环，只需满足以下条件：

- 集合类实现`Iterable`接口，该接口要求返回一个`Iterator`对象；
- 用`Iterator`对象迭代集合内部数据。

**总结：**

`Iterator`是一种抽象的数据访问模型。使用`Iterator`模式进行迭代的好处有：

- 对任何集合都采用同一种访问模型；
- 调用者对集合内部结构一无所知；
- 集合类返回的`Iterator`对象知道如何迭代。

Java提供了标准的迭代器模型，即集合类实现`java.util.Iterable`接口，返回`java.util.Iterator`实例。

## Collections

`Collections`是JDK提供的工具类，同样位于`java.util`包中。它提供了一系列静态方法，能更方便地操作各种集合。

**创建空集合**

`Collections`提供了一系列方法来创建空集合：

- 创建空List：`List<T> emptyList()`
- 创建空Map：`Map<K, V> emptyMap()`
- 创建空Set：`Set<T> emptySet()`
- 也可用集合接口提供的`of(T...)`方法创建空集合

**要注意到返回的空集合是不可变集合，无法向其中添加或删除元素。**

**创建单元素集合**

`Collections`提供了一系列方法来创建一个单元素集合：

- 创建一个元素的List：`List<T> singletonList(T o)`
- 创建一个元素的Map：`Map<K, V> singletonMap(K key, V value)`
- 创建一个元素的Set：`Set<T> singleton(T o)`

要注意到返回的单元素集合也是不可变集合，无法向其中添加或删除元素。

**不可变集合**

`Collections`还提供了一组方法把可变集合封装成不可变集合：

- 封装成不可变List：`List<T> unmodifiableList(List<? extends T> list)`
- 封装成不可变Set：`Set<T> unmodifiableSet(Set<? extends T> set)`
- 封装成不可变Map：`Map<K, V> unmodifiableMap(Map<? extends K, ? extends V> m)`

这种封装实际上是通过创建一个代理对象，拦截掉所有修改方法实现的。

**线程安全集合**

`Collections`还提供了一组方法，可以把线程不安全的集合变为线程安全的集合：

- 变为线程安全的List：`List<T> synchronizedList(List<T> list)`
- 变为线程安全的Set：`Set<T> synchronizedSet(Set<T> s)`
- 变为线程安全的Map：`Map<K,V> synchronizedMap(Map<K,V> m)`

# IO

## File

Java的标准库`java.io`提供了`File`对象来操作文件和目录。构造File对象时，既可传入绝对路径，也可传入相对路径。绝对路径是以根目录开头的完整路径，例如：

```java
// 注意Windows平台使用\作为路径分隔符，在Java字符串中需要用\\表示一个\。Linux平台使用/作为路径分隔符：
File f = new File("C:\\Windows\\notepad.exe");

```

可以用`.`表示当前目录，`..`表示上级目录。

File对象有3种形式表示的路径，一种是`getPath()`，返回构造方法传入的路径，一种是`getAbsolutePath()`，返回绝对路径，一种是`getCanonicalPath`，它和绝对路径类似，但是返回的是规范路径。

**文件和目录**

调用`isFile()`，判断该`File`对象是否是一个已存在的文件，调用`isDirectory()`，判断该`File`对象是否是一个已存在的目录

用`File`对象获取到一个文件时，还可以进一步判断文件的权限和大小：

- `boolean canRead()`：是否可读；
- `boolean canWrite()`：是否可写；
- `boolean canExecute()`：是否可执行；
- `long length()`：文件字节大小。

**创建和删除文件**

当File对象表示一个文件时，可以通过`createNewFile()`创建一个新文件，用`delete()`删除该文件：

```java
    File file = new File("/path/to/file");
    if (file.createNewFile()) {
        // 文件创建成功:
        // TODO:
        if (file.delete()) {
            // 删除文件成功:
        }
    }
```

有些时候，程序需要读写一些临时文件，File对象提供了`createTempFile()`来创建一个临时文件，以及`deleteOnExit()`在JVM退出时自动删除该文件。

**遍历文件和目录**

当File对象表示一个目录时，可以使用`list()`和`listFiles()`列出目录下的文件和子目录名。`listFiles()`提供了一系列重载方法，可以过滤不想要的文件和目录：

```java
    import java.io.*;
    ----
    public class Main {
        public static void main(String[] args) throws IOException {
            File f = new File("C:\\Windows");
            File[] fs1 = f.listFiles(); // 列出所有文件和子目录
            printFiles(fs1);
            File[] fs2 = f.listFiles(new FilenameFilter() { // 仅列出.exe文件
                public boolean accept(File dir, String name) {
                    return name.endsWith(".exe"); // 返回true表示接受该文件
                }
            });
            printFiles(fs2);
        }
    
        static void printFiles(File[] files) {
            System.out.println("==========");
            if (files != null) {
                for (File f : files) {
                    System.out.println(f);
                }
            }
            System.out.println("==========");
        }
    }
    
```

File对象如果表示一个目录，可以通过以下方法创建和删除目录：

- `boolean mkdir()`：创建当前File对象表示的目录；
- `boolean mkdirs()`：创建当前File对象表示的目录，并在必要时将不存在的父目录也创建出来；
- `boolean delete()`：删除当前File对象表示的目录，当前目录必须为空才能删除成功。

**Path类**

位于`java.nio.file`包。`Path`对象和`File`对象类似，但操作更加简单：

```java
    import java.io.*;
    import java.nio.file.*;
    ----
    public class Main {
        public static void main(String[] args) throws IOException {
            Path p1 = Paths.get(".", "project", "study"); // 构造一个Path对象
            System.out.println(p1);
            Path p2 = p1.toAbsolutePath(); // 转换为绝对路径
            System.out.println(p2);
            Path p3 = p2.normalize(); // 转换为规范路径
            System.out.println(p3);
            File f = p3.toFile(); // 转换为File对象
            System.out.println(f);
            for (Path p : Paths.get("..").toAbsolutePath()) { // 可以直接遍历Path
                System.out.println("  " + p);
            }
        }
    }
```

**总结：**

Java标准库的`java.io.File`对象表示一个文件或者目录：

- 创建`File`对象本身不涉及IO操作；
- 可以获取路径／绝对路径／规范路径：`getPath()`/`getAbsolutePath()`/`getCanonicalPath()`；
- 可以获取目录的文件和子目录：`list()`/`listFiles()`；
- 可以创建或删除文件和目录。

## InputStream

`InputStream`就是Java标准库提供的最基本的输入流。它位于`java.io`这个包里。`java.io`包提供了所有同步IO的功能。

要特别注意的一点是，`InputStream`并不是一个接口，而是一个抽象类，它是所有输入流的超类。这个抽象类定义的一个最重要的方法就是`int read()`，签名如下：

```java
    public abstract int read() throws IOException;

```

`FileInputStream`是`InputStream`的一个子类。顾名思义，`FileInputStream`就是从文件流中读取数据。下面的代码演示了如何完整地读取一个`FileInputStream`的所有字节：

```java
    public void readFile() throws IOException {
        // 创建一个FileInputStream对象:
        InputStream input = new FileInputStream("src/readme.txt");
        for (;;) {
            int n = input.read(); // 反复调用read()方法，直到返回-1
            if (n == -1) {
                break;
            }
            System.out.println(n); // 打印byte的值
        }
        input.close(); // 关闭流
    }
```

**`InputStream`和`OutputStream`都是通过`close()`方法来关闭流。关闭流就会释放对应的底层资源。**

编译器不会特别地为`InputStream`加上自动关闭。编译器只看`try(resource = ...)`中的对象是否实现了`java.lang.AutoCloseable`接口，如果实现了，就自动加上`finally`语句并调用`close()`方法。`InputStream`和`OutputStream`都实现了这个接口，因此，都可以用在`try(resource)`中。

**缓冲**

`InputStream`提供了两个重载方法来支持读取多个字节：

- `int read(byte[] b)`：读取若干字节并填充到`byte[]`数组，返回读取的字节数
- `int read(byte[] b, int off, int len)`：指定`byte[]`数组的偏移量和最大填充数

```java
    public void readFile() throws IOException {
        try (InputStream input = new FileInputStream("src/readme.txt")) {
            // 定义1000个字节大小的缓冲区:
            byte[] buffer = new byte[1000];
            int n;
            while ((n = input.read(buffer)) != -1) { // 读取到缓冲区
                System.out.println("read " + n + " bytes.");
            }
        }
    }
```

**阻塞**

在调用`InputStream`的`read()`方法读取数据时，我们说`read()`方法是阻塞（Blocking）的。它的意思是，对于下面的代码：

```java
    int n;
    n = input.read(); // 必须等待read()方法返回才能执行下一行代码
    int m = n;
```

执行到第二行代码时，必须等`read()`方法返回后才能继续。因为读取IO流相比执行普通代码，速度会慢很多，因此，无法确定`read()`方法调用到底要花费多长时间。

**总结**

Java标准库的`java.io.InputStream`定义了所有输入流的超类：

- `FileInputStream`实现了文件流输入；
- `ByteArrayInputStream`在内存中模拟一个字节流输入。

总是使用`try(resource)`来保证`InputStream`正确关闭。

## OutputStream

和`InputStream`相反，`OutputStream`是Java标准库提供的最基本的输出流。

和`InputStream`类似，`OutputStream`也是抽象类，它是所有输出流的超类。这个抽象类定义的一个最重要的方法就是`void write(int b)`，签名如下：

```java
public abstract void write(int b) throws IOException; 
```

**`OutputStream`还提供了一个`flush()`方法，它的目的是将缓冲区的内容真正输出到目的地。**

**阻塞**

和`InputStream`一样，`OutputStream`的`write()`方法也是阻塞的。

**总结**

Java标准库的`java.io.OutputStream`定义了所有输出流的超类：

- `FileOutputStream`实现了文件流输出；
- `ByteArrayOutputStream`在内存中模拟一个字节流输出。

某些情况下需要手动调用`OutputStream`的`flush()`方法来强制输出缓冲区。

总是使用`try(resource)`来保证`OutputStream`正确关闭。

## Filter

如果要给`FileInputStream`添加缓冲和签名的功能，那么我们还需要派生`BufferedDigestFileInputStream`。

如果要给`FileInputStream`添加缓冲和加解密的功能，则需要派生`BufferedCipherFileInputStream`。

为了解决依赖继承会导致子类数量失控的问题，JDK首先将`InputStream`分为两大类：

一类是直接提供数据的基础`InputStream`，例如：

- FileInputStream
- ByteArrayInputStream
- ServletInputStream

一类是提供额外附加功能的`InputStream`，例如：

- BufferedInputStream
- DigestInputStream
- CipherInputStream

```java
    InputStream file = new FileInputStream("test.gz");
    InputStream buffered = new BufferedInputStream(file);
    InputStream gzip = new GZIPInputStream(buffered);

```

Java的IO标准库使用Filter模式为`InputStream`和`OutputStream`增加功能：

- 可以把一个`InputStream`和任意个`FilterInputStream`组合；
- 可以把一个`OutputStream`和任意个`FilterOutputStream`组合。

Filter模式可以在运行期动态增加功能（又称Decorator模式）。

## Zip

`ZipInputStream`是一种`FilterInputStream`，它可以直接读取zip包的内容。

一个`ZipEntry`表示一个压缩文件或目录，如果是压缩文件，我们就用`read()`方法不断读取，直到返回`-1`：

```java
    try (ZipInputStream zip = new ZipInputStream(new FileInputStream(...))) {
        ZipEntry entry = null;
        while ((entry = zip.getNextEntry()) != null) {
            String name = entry.getName();
            if (!entry.isDirectory()) {
                int n;
                while ((n = zip.read()) != -1) {
                    ...
                }
            }
        }
    }
```

**写入Zip**

`ZipOutputStream`是一种`FilterOutputStream`，它可以直接写入内容到zip包。我们要先创建一个`ZipOutputStream`，通常是包装一个`FileOutputStream`，然后，每写入一个文件前，先调用`putNextEntry()`，然后用`write()`写入`byte[]`数据，写入完毕后调用`closeEntry()`结束这个文件的打包。

```java
    try (ZipOutputStream zip = new ZipOutputStream(new FileOutputStream(...))) {
        File[] files = ...
        for (File file : files) {
            zip.putNextEntry(new ZipEntry(file.getName()));
            zip.write(getFileDataAsBytes(file));
            zip.closeEntry();
        }
    }
```

**总结：**

- `ZipInputStream`可以读取zip格式的流，`ZipOutputStream`可以把多份数据写入zip包；

- 配合`FileInputStream`和`FileOutputStream`就可以读写zip文件。

## Classpath

从classpath读取文件就可以避免不同环境下文件路径不一致的问题：如果我们把`default.properties`文件放到classpath中，就不用关心它的实际存放路径。

在classpath中的资源文件，路径总是以`／`开头，我们先获取当前的`Class`对象，然后调用`getResourceAsStream()`就可以直接从classpath读取任意的资源文件：

## 序列化

一个Java对象要能序列化，必须实现一个特殊的`java.io.Serializable`接口，它的定义如下：

```java
    public interface Serializable {
    }
```

`Serializable`接口没有定义任何方法，它是一个空接口。

**序列化**

把一个Java对象变为`byte[]`数组，需要使用`ObjectOutputStream`。它负责把一个Java对象写入一个字节流：

```java
    import java.io.*;
    import java.util.Arrays;
    ----
    public class Main {
        public static void main(String[] args) throws IOException {
            ByteArrayOutputStream buffer = new ByteArrayOutputStream();
            try (ObjectOutputStream output = new ObjectOutputStream(buffer)) {
                // 写入int:
                output.writeInt(12345);
                // 写入String:
                output.writeUTF("Hello");
                // 写入Object:
                output.writeObject(Double.valueOf(123.456));
            }
            System.out.println(Arrays.toString(buffer.toByteArray()));
        }
    }
```

`ObjectOutputStream`既可以写入基本类型，如`int`，`boolean`，也可以写入`String`（以UTF-8编码），还可以写入实现了`Serializable`接口的`Object`。

因为写入`Object`时需要大量的类型信息，所以写入的内容很大。

**反序列化**

和`ObjectOutputStream`相反，`ObjectInputStream`负责从一个字节流读取Java对象：

```java
    try (ObjectInputStream input = new ObjectInputStream(...)) {
        int n = input.readInt();
        String s = input.readUTF();
        Double d = (Double) input.readObject();
    }
```

除了能读取基本类型和`String`类型外，调用`readObject()`可以直接返回一个`Object`对象。要把它变成一个特定类型，必须强制转型。

`readObject()`可能抛出的异常有：

- `ClassNotFoundException`：没有找到对应的Class；
- `InvalidClassException`：Class不匹配。

**安全性**

因为Java的序列化机制可以导致一个实例能直接从`byte[]`数组创建，而不经过构造方法。一个精心构造的`byte[]`数组被反序列化后可以执行特定的Java代码，从而导致严重的安全漏洞。

实际上，Java本身提供的基于对象的序列化和反序列化机制既存在安全性问题，也存在兼容性问题。更好的序列化方法是通过JSON这样的通用数据结构来实现，只输出基本类型（包括String）的内容，而不存储任何与代码相关的信息。

**总结**

- 可序列化的Java对象必须实现`java.io.Serializable`接口，类似`Serializable`这样的空接口被称为“标记接口”（Marker Interface）；

- 反序列化时不调用构造方法，可设置`serialVersionUID`作为版本号（非必需）；

- Java的序列化机制仅适用于Java，如果需要与其它语言交换数据，必须使用通用的序列化方法，例如JSON。

## Reader

`Reader`是Java的IO库提供的另一个输入流接口。和`InputStream`的区别是，`InputStream`是一个字节流，即以`byte`为单位读取，而`Reader`是一个字符流，即以`char`为单位读取：

| InputStream                      | Reader                             |
| -------------------------------- | ---------------------------------- |
| 字节流，以byte为单位             | 字符流，以char为单位               |
| 读取字节（-1，0~255）：intread() | 读取字符（-1，0~65535）：intread() |
| 读到字节数组：intread(byte[]b)   | 读到字符数组：intread(char[]c)     |

`java.io.Reader`是所有字符输入流的超类，它最主要的方法是：

```java
public int read() throws IOException;
```

**总结：**

`Reader`定义了所有字符输入流的超类：

- `FileReader`实现了文件字符流输入，使用时需要指定编码；
- `CharArrayReader`和`StringReader`可以在内存中模拟一个字符流输入。

`Reader`是基于`InputStream`构造的：可以通过`InputStreamReader`在指定编码的同时将任何`InputStream`转换为`Reader`。

总是使用`try (resource)`保证`Reader`正确关闭。

## Writer

`Reader`是带编码转换器的`InputStream`，它把`byte`转换为`char`，而`Writer`就是带编码转换器的`OutputStream`，它把`char`转换为`byte`并输出。

`Writer`和`OutputStream`的区别如下：

| OutputStream                       | Writer                               |
| ---------------------------------- | ------------------------------------ |
| 字节流，以byte为单位               | 字符流，以char为单位                 |
| 写入字节（0~255）：voidwrite(intb) | 写入字符（0~65535）：voidwrite(intc) |
| 写入字节数组：voidwrite(byte[]b)   | 写入字符数组：voidwrite(char[]c)     |
| 无对应方法                         | 写入String：voidwrite(Strings)       |

`Writer`是所有字符输出流的超类，它提供的方法主要有：

- 写入一个字符（0~65535）：`void write(int c)`；
- 写入字符数组的所有字符：`void write(char[] c)`；
- 写入String表示的所有字符：`void write(String s)`。

**总结：**

`Writer`定义了所有字符输出流的超类：

- `FileWriter`实现了文件字符流输出；
- `CharArrayWriter`和`StringWriter`在内存中模拟一个字符流输出。

使用`try (resource)`保证`Writer`正确关闭。

`Writer`是基于`OutputStream`构造的，可以通过`OutputStreamWriter`将`OutputStream`转换为`Writer`，转换时需要指定编码。

## PrintStream / PrintWriter

**PrintStream**

`PrintStream`是一种`FilterOutputStream`，它在`OutputStream`的接口上，额外提供了一些写入各种数据类型的方法：

- 写入`int`：`print(int)`
- 写入`boolean`：`print(boolean)`
- 写入`String`：`print(String)`
- 写入`Object`：`print(Object)`，实际上相当于`print(object.toString())`

以及对应的一组`println()`方法，它会自动加上换行符。

我们经常使用的`System.out.println()`实际上就是使用`PrintStream`打印各种数据。其中，`System.out`是系统默认提供的`PrintStream`，表示标准输出：

```java
    System.out.print(12345); // 输出12345
    System.out.print(new Object()); // 输出类似java.lang.Object@3c7a835a
    System.out.println("Hello"); // 输出Hello并换行
```

`System.err`是系统默认提供的标准错误输出。

`PrintStream`和`OutputStream`相比，除了添加了一组`print()`/`println()`方法，可以打印各种数据类型，比较方便外，它还有一个额外的优点，就是不会抛出`IOException`，这样我们在编写代码的时候，就不必捕获`IOException`。

**PrintWriter**

`PrintStream`最终输出的总是byte数据，而`PrintWriter`则是扩展了`Writer`接口，它的`print()`/`println()`方法最终输出的是`char`数据。两者的使用方法几乎是一模一样的：

```java
    import java.io.*;
    ----
    public class Main {
        public static void main(String[] args)     {
            StringWriter buffer = new StringWriter();
            try (PrintWriter pw = new PrintWriter(buffer)) {
                pw.println("Hello");
                pw.println(12345);
                pw.println(true);
            }
            System.out.println(buffer.toString());
        }
    }
```

**总结：**

`PrintStream`是一种能接收各种数据类型的输出，打印数据时比较方便：

- `System.out`是标准输出；
- `System.err`是标准错误输出。

`PrintWriter`是基于`Writer`的输出。

## Files

虽然`Files`和`Paths`是`java.nio`包里面的类，但他俩封装了很多读写文件的简单方法，例如，我们要把一个文件的全部内容读取为一个`byte[]`，可以这么写：

```java
    byte[] data = Files.readAllBytes(Paths.get("/path/to/file.txt"));

    // 默认使用UTF-8编码读取:
    String content1 = Files.readString(Paths.get("/path/to/file.txt"));
    // 可指定编码:
    String content2 = Files.readString(Paths.get("/path/to/file.txt"), StandardCharsets.ISO_8859_1);
    // 按行读取并返回每行内容:
    List<String> lines = Files.readAllLines(Paths.get("/path/to/file.txt"));

    // 写入二进制文件:
    byte[] data = ...
    Files.write(Paths.get("/path/to/file.txt"), data);
    // 写入文本并指定编码:
    Files.writeString(Paths.get("/path/to/file.txt"), "文本内容...", StandardCharsets.ISO_8859_1);
    // 按行写入文本:
    List<String> lines = ...
    Files.write(Paths.get("/path/to/file.txt"), lines);
```

`Files`工具类还有`copy()`、`delete()`、`exists()`、`move()`等快捷方法操作文件和目录。

最后需要特别注意的是，`Files`提供的读写方法，受内存限制，只能读写小文件，例如配置文件等，不可一次读入几个G的大文件。读写大型文件仍然要使用文件流，每次只读写一部分文件内容。

# 日期和时间

计算机通过`Locale`来针对当地用户习惯格式化日期、时间、数字、货币等。

## Date 、Calendar

在计算机中，只需要存储一个整数`1574208900`表示某一时刻。当需要显示为某一地区的当地时间时，我们就把它格式化为一个字符串：

```java
    String displayDateTime(int n, String timezone) { ... }

```

`Epoch Time`又称为时间戳，在不同的编程语言中，会有几种存储方式：

- 以秒为单位的整数：1574208900，缺点是精度只能到秒；
- 以毫秒为单位的整数：1574208900123，最后3位表示毫秒数；
- 以秒为单位的浮点数：1574208900.123，小数点后面表示零点几秒。

**标准库API**

Java标准库有两套处理日期和时间的API：

- 一套定义在`java.util`这个包里面，主要包括`Date`、`Calendar`和`TimeZone`这几个类；
- 一套新的API是在Java 8引入的，定义在`java.time`这个包里面，主要包括`LocalDateTime`、`ZonedDateTime`、`ZoneId`等。

为什么会有新旧两套API呢？因为历史遗留原因，旧的API存在很多问题，所以引入了新的API。

**Date**

`java.util.Date`是用于表示一个日期和时间的对象，注意与`java.sql.Date`区分，后者用在数据库中。如果观察Date的源码，可以发现它实际上存储了一个long类型的以毫秒表示的时间戳：

```java
    public class Date implements Serializable, Cloneable, Comparable<Date> {
    
        private transient long fastTime;
    
        ...
    }
```

**Date基本用法**

```java
    import java.util.*;
    ----
    public class Main {
        public static void main(String[] args) {
            // 获取当前时间:
            Date date = new Date();
            System.out.println(date.getYear() + 1900); // 必须加上1900
            System.out.println(date.getMonth() + 1); // 0~11，必须加上1
            System.out.println(date.getDate()); // 1~31，不能加1
            // 转换为String:
            System.out.println(date.toString());
            // 转换为GMT时区:
            System.out.println(date.toGMTString());
            // 转换为本地时区:
            System.out.println(date.toLocaleString());
        }
    }
    
```

注意`getYear()`返回的年份必须加上`1900`，`getMonth()`返回的月份是0-11，所以要加1，而`getDate()`返回的日期范围是`1`~`31`，又不能加1。

如果我们想要针对用户的偏好精确地控制日期和时间的格式，就可以使用`SimpleDateFormat`对一个`Date`进行转换。它用预定义的字符串表示格式化：

- yyyy：年
- MM：月
- dd: 日
- HH: 小时
- mm: 分钟
- ss: 秒

```java
    import java.text.*;
    import java.util.*;
    ----
    public class Main {
        public static void main(String[] args) {
            // 获取当前时间:
            Date date = new Date();
            var sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
            System.out.println(sdf.format(date));
        }
    }
```

**Calendar**

`Calendar`可以用于获取并设置年、月、日、时、分、秒，它和`Date`比，主要多了一个可以做简单的日期和时间运算的功能。

我们来看`Calendar`的基本用法：

```java
    import java.util.*;
    ----
    public class Main {
        public static void main(String[] args) {
            // 获取当前时间:
            Calendar c = Calendar.getInstance();
            int y = c.get(Calendar.YEAR);
            int m = 1 + c.get(Calendar.MONTH);
            int d = c.get(Calendar.DAY_OF_MONTH);
            int w = c.get(Calendar.DAY_OF_WEEK);
            int hh = c.get(Calendar.HOUR_OF_DAY);
            int mm = c.get(Calendar.MINUTE);
            int ss = c.get(Calendar.SECOND);
            int ms = c.get(Calendar.MILLISECOND);
            System.out.println(y + "-" + m + "-" + d + " " + w + " " + hh + ":" + mm + ":" + ss + "." + ms);
        }
    }
```

注意到`Calendar`获取年月日这些信息变成了`get(int field)`，返回的年份不必转换，返回的月份仍然要加1，返回的星期要特别注意，`1`~`7`分别表示周日，周一，……，周六。

`Calendar`只有一种方式获取，即`Calendar.getInstance()`，而且一获取到就是当前时间。如果我们想给它设置成特定的一个日期和时间，就必须先清除所有字段：

```java
    import java.text.*;
    import java.util.*;
    ----
    public class Main {
        public static void main(String[] args) {
            // 当前时间:
            Calendar c = Calendar.getInstance();
            // 清除所有:
            c.clear();
            // 设置2019年:
            c.set(Calendar.YEAR, 2019);
            // 设置9月:注意8表示9月:
            c.set(Calendar.MONTH, 8);
            // 设置2日:
            c.set(Calendar.DATE, 2);
            // 设置时间:
            c.set(Calendar.HOUR_OF_DAY, 21);
            c.set(Calendar.MINUTE, 22);
            c.set(Calendar.SECOND, 23);
            System.out.println(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(c.getTime()));
            // 2019-09-02 21:22:23
        }
    }
```

利用`Calendar.getTime()`可以将一个`Calendar`对象转换成`Date`对象，然后就可以用`SimpleDateFormat`进行格式化了。

**TimeZone**

`Calendar`和`Date`相比，它提供了时区转换的功能。时区用`TimeZone`对象表示：

```java
    import java.util.*;
    ----
    public class Main {
        public static void main(String[] args) {
            TimeZone tzDefault = TimeZone.getDefault(); // 当前时区
            TimeZone tzGMT9 = TimeZone.getTimeZone("GMT+09:00"); // GMT+9:00时区
            TimeZone tzNY = TimeZone.getTimeZone("America/New_York"); // 纽约时区
            System.out.println(tzDefault.getID()); // Asia/Shanghai
            System.out.println(tzGMT9.getID()); // GMT+09:00
            System.out.println(tzNY.getID()); // America/New_York
        }
    }
    
```

时区的唯一标识是以字符串表示的ID，我们获取指定`TimeZone`对象也是以这个ID为参数获取，`GMT+09:00`、`Asia/Shanghai`都是有效的时区ID。要列出系统支持的所有ID，请使用`TimeZone.getAvailableIDs()`。

有了时区，我们就可以对指定时间进行转换。例如，下面的例子演示了如何将北京时间`2019-11-20 8:15:00`转换为纽约时间：

```java
    import java.text.*;
    import java.util.*;
    ----
    public class Main {
        public static void main(String[] args) {
            // 当前时间:
            Calendar c = Calendar.getInstance();
            // 清除所有:
            c.clear();
            // 设置为北京时区:
            c.setTimeZone(TimeZone.getTimeZone("Asia/Shanghai"));
            // 设置年月日时分秒:
            c.set(2019, 10 /* 11月 */, 20, 8, 15, 0);
            // 显示时间:
            var sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
            sdf.setTimeZone(TimeZone.getTimeZone("America/New_York"));
            System.out.println(sdf.format(c.getTime()));
            // 2019-11-19 19:15:00
        }
    }
```

可见，利用`Calendar`进行时区转换的步骤是：

1. 清除所有字段；
2. 设定指定时区；
3. 设定日期和时间；
4. 创建`SimpleDateFormat`并设定目标时区；
5. 格式化获取的`Date`对象（注意`Date`对象无时区信息，时区信息存储在`SimpleDateFormat`中）。

因此，本质上时区转换只能通过`SimpleDateFormat`在显示的时候完成。

`Calendar`也可以对日期和时间进行简单的加减：

```java
    import java.text.*;
    import java.util.*;
    ----
    public class Main {
        public static void main(String[] args) {
            // 当前时间:
            Calendar c = Calendar.getInstance();
            // 清除所有:
            c.clear();
            // 设置年月日时分秒:
            c.set(2019, 10 /* 11月 */, 20, 8, 15, 0);
            // 加5天并减去2小时:
            c.add(Calendar.DAY_OF_MONTH, 5);
            c.add(Calendar.HOUR_OF_DAY, -2);
            // 显示时间:
            var sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
            Date d = c.getTime();
            System.out.println(sdf.format(d));
            // 2019-11-25 6:15:00
        }
    }
```

**总结**

计算机表示的时间是以整数表示的时间戳存储的，即Epoch Time，Java使用`long`型来表示以毫秒为单位的时间戳，通过`System.currentTimeMillis()`获取当前时间戳。

Java有两套日期和时间的API：

- 旧的Date、Calendar和TimeZone；
- 新的LocalDateTime、ZonedDateTime、ZoneId等。

分别位于`java.util`和`java.time`包中。

## LocalDateTime

从Java 8开始，`java.time`包提供了新的日期和时间API，主要涉及的类型有：

- 本地日期和时间：`LocalDateTime`，`LocalDate`，`LocalTime`；
- 带时区的日期和时间：`ZonedDateTime`；
- 时刻：`Instant`；
- 时区：`ZoneId`，`ZoneOffset`；
- 时间间隔：`Duration`。

以及一套新的用于取代`SimpleDateFormat`的格式化类型`DateTimeFormatter`。

和旧的API相比，新API严格区分了时刻、本地日期、本地时间和带时区的日期时间，并且，对日期和时间进行运算更加方便。

此外，新API修正了旧API不合理的常量设计：

- Month的范围用1~12表示1月到12月；
- Week的范围用1~7表示周一到周日。

最后，新API的类型几乎全部是不变类型（和String类似），可以放心使用不必担心被修改。

**LocalDateTime**

表示一个本地日期和时间：

```java
    import java.time.*;
    ----
    public class Main {
        public static void main(String[] args) {
            LocalDate d = LocalDate.now(); // 当前日期
            LocalTime t = LocalTime.now(); // 当前时间
            LocalDateTime dt = LocalDateTime.now(); // 当前日期和时间
            System.out.println(d); // 严格按照ISO 8601格式打印
            System.out.println(t); // 严格按照ISO 8601格式打印
            System.out.println(dt); // 严格按照ISO 8601格式打印
        }
    }
```

本地日期和时间通过now()获取到的总是以当前默认时区返回的，和旧API不同，`LocalDateTime`、`LocalDate`和`LocalTime`默认严格按照[ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html)规定的日期和时间格式进行打印。

反过来，通过指定的日期和时间创建`LocalDateTime`可以通过`of()`方法：

```java
    LocalDate d2 = LocalDate.of(2019, 11, 30); // 2019-11-30, 注意11=11月
    LocalTime t2 = LocalTime.of(15, 16, 17); // 15:16:17
    LocalDateTime dt2 = LocalDateTime.of(2019, 11, 30, 15, 16, 17);
    LocalDateTime dt3 = LocalDateTime.of(d2, t2);
```

因为严格按照ISO 8601的格式，因此，将字符串转换为`LocalDateTime`就可以传入标准格式：

```java
LocalDateTime dt = LocalDateTime.parse("2019-11-19T15:16:17");    
LocalDate d = LocalDate.parse("2019-11-19");    LocalTime t = LocalTime.parse("15:16:17");
```

注意ISO 8601规定的日期和时间分隔符是`T`。标准格式如下：

- 日期：yyyy-MM-dd
- 时间：HH:mm:ss
- 带毫秒的时间：HH:mm:ss.SSS
- 日期和时间：yyyy-MM-dd'T'HH:mm:ss
- 带毫秒的日期和时间：yyyy-MM-dd'T'HH:mm:ss.SSS

**DateTimeFormatter**

如果要自定义输出的格式，或者要把一个非ISO 8601格式的字符串解析成`LocalDateTime`，可以使用新的`DateTimeFormatter`：

```java
    import java.time.*;
    import java.time.format.*;
    ----
    public class Main {
        public static void main(String[] args) {
            // 自定义格式化:
            DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy/MM/dd HH:mm:ss");
            System.out.println(dtf.format(LocalDateTime.now()));
    
            // 用自定义格式解析:
            LocalDateTime dt2 = LocalDateTime.parse("2019/11/30 15:16:17", dtf);
            System.out.println(dt2);
        }
    }
```

`LocalDateTime`提供了对日期和时间进行加减的非常简单的链式调用：

```java
    import java.time.*;
    ----
    public class Main {
        public static void main(String[] args) {
            LocalDateTime dt = LocalDateTime.of(2019, 10, 26, 20, 30, 59);
            System.out.println(dt);
            // 加5天减3小时:
            LocalDateTime dt2 = dt.plusDays(5).minusHours(3);
            System.out.println(dt2); // 2019-10-31T17:30:59
            // 减1月:
            LocalDateTime dt3 = dt2.minusMonths(1);
            System.out.println(dt3); // 2019-09-30T17:30:59
        }
    }
```

注意到月份加减会自动调整日期，例如从`2019-10-31`减去1个月得到的结果是`2019-09-30`，因为9月没有31日。

对日期和时间进行调整则使用`withXxx()`方法，例如：`withHour(15)`会把`10:11:12`变为`15:11:12`：

- 调整年：withYear()
- 调整月：withMonth()
- 调整日：withDayOfMonth()
- 调整时：withHour()
- 调整分：withMinute()
- 调整秒：withSecond()

要判断两个`LocalDateTime`的先后，可以使用`isBefore()`、`isAfter()`方法，对于`LocalDate`和`LocalTime`类似：

```java
    import java.time.*;
    ----
    public class Main {
        public static void main(String[] args) {
            LocalDateTime now = LocalDateTime.now();
            LocalDateTime target = LocalDateTime.of(2019, 11, 19, 8, 15, 0);
            System.out.println(now.isBefore(target));
            System.out.println(LocalDate.now().isBefore(LocalDate.of(2019, 11, 19)));
            System.out.println(LocalTime.now().isAfter(LocalTime.parse("08:15:00")));
        }
    }
```

注意到`LocalDateTime`无法与时间戳进行转换，因为`LocalDateTime`没有时区，无法确定某一时刻。后面我们要介绍的`ZonedDateTime`相当于`LocalDateTime`加时区的组合，它具有时区，可以与`long`表示的时间戳进行转换。

**Duration / Period**

`Duration`表示两个时刻之间的时间间隔。另一个类似的`Period`表示两个日期之间的天数：

```java
    import java.time.*;
    ----
    public class Main {
        public static void main(String[] args) {
            LocalDateTime start = LocalDateTime.of(2019, 11, 19, 8, 15, 0);
            LocalDateTime end = LocalDateTime.of(2020, 1, 9, 19, 25, 30);
            Duration d = Duration.between(start, end);
            System.out.println(d); // PT1235H10M30S
    
            Period p = LocalDate.of(2019, 11, 19).until(LocalDate.of(2020, 1, 9));
            System.out.println(p); // P1M21D
        }
    }
```

注意到两个`LocalDateTime`之间的差值使用`Duration`表示，类似`PT1235H10M30S`，表示1235小时10分钟30秒。而两个`LocalDate`之间的差值用`Period`表示，类似`P1M21D`，表示1个月21天。

`Duration`和`Period`的表示方法也符合ISO 8601的格式，它以`P...T...`的形式表示，`P...T`之间表示日期间隔，`T`后面表示时间间隔。如果是`PT...`的格式表示仅有时间间隔。利用`ofXxx()`或者`parse()`方法也可以直接创建`Duration`：

```java
   Duration d1 = Duration.ofHours(10); // 10 hours
   Duration d2 = Duration.parse("P1DT2H3M"); // 1 day, 2 hours, 3 minutes
```

**总结：**

- Java 8引入了新的日期和时间API，它们是不变类，默认按ISO 8601标准格式化和解析；

- 使用`LocalDateTime`可以非常方便地对日期和时间进行加减，或者调整日期和时间，它总是返回新对象；

- 使用`isBefore()`和`isAfter()`可以判断日期和时间的先后；

- 使用`Duration`和`Period`可以表示两个日期和时间的“区间间隔”。

## ZonedDateTime

`LocalDateTime`总是表示本地日期和时间，要表示一个带时区的日期和时间，我们就需要`ZonedDateTime`。

可以简单地把`ZonedDateTime`理解成`LocalDateTime`加`ZoneId`。`ZoneId`是`java.time`引入的新的时区类，注意和旧的`java.util.TimeZone`区别。

要创建一个`ZonedDateTime`对象，有以下几种方法，一种是通过`now()`方法返回当前时间：

```java
    import java.time.*;
    ----
    public class Main {
        public static void main(String[] args) {
            ZonedDateTime zbj = ZonedDateTime.now(); // 默认时区
            ZonedDateTime zny = ZonedDateTime.now(ZoneId.of("America/New_York")); // 用指定时区获取当前时间
            System.out.println(zbj);
            System.out.println(zny);
        }
    }
```

另一种方式是通过给一个`LocalDateTime`附加一个`ZoneId`，就可以变成`ZonedDateTime`：

```java
    import java.time.*;
    ----
    public class Main {
        public static void main(String[] args) {
            LocalDateTime ldt = LocalDateTime.of(2019, 9, 15, 15, 16, 17);
            ZonedDateTime zbj = ldt.atZone(ZoneId.systemDefault());
            ZonedDateTime zny = ldt.atZone(ZoneId.of("America/New_York"));
            System.out.println(zbj);
            System.out.println(zny);
        }
    }
```

**时区转换**

要转换时区，首先我们需要有一个`ZonedDateTime`对象，然后，通过`withZoneSameInstant()`将关联时区转换到另一个时区，转换后日期和时间都会相应调整。

下面的代码演示了如何将北京时间转换为纽约时间：

```java
    import java.time.*;
    ----
    public class Main {
        public static void main(String[] args) {
            // 以中国时区获取当前时间:
            ZonedDateTime zbj = ZonedDateTime.now(ZoneId.of("Asia/Shanghai"));
            // 转换为纽约时间:
            ZonedDateTime zny = zbj.withZoneSameInstant(ZoneId.of("America/New_York"));
            System.out.println(zbj);
            System.out.println(zny);
        }
    }
    
```

有了`ZonedDateTime`，将其转换为本地时间就非常简单：

```java
ZonedDateTime zdt = ...    
LocalDateTime ldt = zdt.toLocalDateTime();    
```

转换为`LocalDateTime`时，直接丢弃了时区信息。

**总结**

- `ZonedDateTime`是带时区的日期和时间，可用于时区转换；

- `ZonedDateTime`和`LocalDateTime`可以相互转换

## DateTimeFormatter

使用旧的`Date`对象时，我们用`SimpleDateFormat`进行格式化显示。使用新的`LocalDateTime`或`ZonedLocalDateTime`时，我们要进行格式化显示，就要使用`DateTimeFormatter`。

和`SimpleDateFormat`不同的是，`DateTimeFormatter`不但是不变对象，它还是线程安全的。因为`SimpleDateFormat`不是线程安全的，使用的时候，只能在方法内部创建新的局部变量。而`DateTimeFormatter`可以只创建一个实例，到处引用。

创建`DateTimeFormatter`时，我们仍然通过传入格式化字符串实现：

```java
    DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
```

格式化字符串的使用方式与`SimpleDateFormat`完全一致。

另一种创建`DateTimeFormatter`的方法是，传入格式化字符串时，同时指定`Locale`：

```java
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("E, yyyy-MMMM-dd HH:mm", Locale.US);
```

当直接调用`System.out.println()`对一个`ZonedDateTime`或者`LocalDateTime`实例进行打印时，实际调用的是它们的`toString()`方法，默认的`toString()`方法显示的字符串就是按照`ISO 8601`格式显示的，我们可以通过`DateTimeFormatter`预定义的几个静态变量来引用：

```java
var ldt = LocalDateTime.now(); 
System.out.println(DateTimeFormatter.ISO_DATE.format(ldt));   
System.out.println(DateTimeFormatter.ISO_DATE_TIME.format(ldt));
```

**总结：**

- 对`ZonedDateTime`或`LocalDateTime`进行格式化，需要使用`DateTimeFormatter`类；

- `DateTimeFormatter`可以通过格式化字符串和`Locale`对日期和时间进行定制输出。

## Instant

Java提供的`System.currentTimeMillis()`返回的就是以毫秒表示的当前时间戳。

这个当前时间戳在`java.time`中以`Instant`类型表示，我们用`Instant.now()`获取当前时间戳，效果和`System.currentTimeMillis()`类似：

```java
    import java.time.*;
    ----
    public class Main {
        public static void main(String[] args) {
            Instant now = Instant.now();
            System.out.println(now.getEpochSecond()); // 秒
            System.out.println(now.toEpochMilli()); // 毫秒
        }
    }
```

**Instant**

```java
    public final class Instant implements ... {
        private final long seconds;
        private final int nanos;
    }
```

一个是以秒为单位的时间戳，一个是更精确的纳秒精度。它和`System.currentTimeMillis()`返回的`long`相比，只是多了更高精度的纳秒。

既然`Instant`就是时间戳，那么，给它附加上一个时区，就可以创建出`ZonedDateTime`：

```java
    Instant ins = Instant.ofEpochSecond(1568568760);
    ZonedDateTime zdt = ins.atZone(ZoneId.systemDefault());
    System.out.println(zdt); // 2019-09-16T01:32:40+08:00[Asia/Shanghai]
```

**总结**

`Instant`表示高精度时间戳，它可以和`ZonedDateTime`以及`long`互相转换。

## 最佳实践

**旧API转新API**

```java
    // Date -> Instant:
    Instant ins1 = new Date().toInstant();
    
    // Calendar -> Instant -> ZonedDateTime:
    Calendar calendar = Calendar.getInstance();
    Instant ins2 = calendar.toInstant();
    ZonedDateTime zdt = ins2.atZone(calendar.getTimeZone().toZoneId());
```

**新API转旧API**

```java
    // ZonedDateTime -> long:
    ZonedDateTime zdt = ZonedDateTime.now();
    long ts = zdt.toEpochSecond() * 1000;
    
    // long -> Date:
    Date date = new Date(ts);
    
    // long -> Calendar:
    Calendar calendar = Calendar.getInstance();
    calendar.clear();
    calendar.setTimeZone(TimeZone.getTimeZone(zdt.getZone().getId()));
    calendar.setTimeInMillis(zdt.toEpochSecond() * 1000);
```

**数据库**

除了旧式的`java.util.Date`，我们还可以找到另一个`java.sql.Date`，它继承自`java.util.Date`，但会自动忽略所有时间相关信息。这个奇葩的设计原因要追溯到数据库的日期与时间类型。

在数据库中，也存在几种日期和时间类型：

- `DATETIME`：表示日期和时间；
- `DATE`：仅表示日期；
- `TIME`：仅表示时间；
- `TIMESTAMP`：和`DATETIME`类似，但是数据库会在创建或者更新记录的时候同时修改`TIMESTAMP`。

在使用Java程序操作数据库时，我们需要把数据库类型与Java类型映射起来。下表是数据库类型与Java新旧API的映射关系：

| 数据库    | 对应Java类（旧）   | 对应Java类（新） |
| --------- | ------------------ | ---------------- |
| DATETIME  | java.util.Date     | LocalDateTime    |
| DATE      | java.sql.Date      | LocalDate        |
| TIME      | java.sql.Time      | LocalTime        |
| TIMESTAMP | java.sql.Timestamp | LocalDateTime    |

实际上，在数据库中，我们需要存储的最常用的是时刻（`Instant`），因为有了时刻信息，就可以根据用户自己选择的时区，显示出正确的本地时间。所以，最好的方法是直接用长整数`long`表示，在数据库中存储为`BIGINT`类型。

```java
    import java.time.*;
    import java.time.format.*;
    import java.util.Locale;
    ----
    public class Main {
        public static void main(String[] args) {
            long ts = 1574208900000L;
            System.out.println(timestampToString(ts, Locale.CHINA, "Asia/Shanghai"));
            System.out.println(timestampToString(ts, Locale.US, "America/New_York"));
        }
    
        static String timestampToString(long epochMilli, Locale lo, String zoneId) {
            Instant ins = Instant.ofEpochMilli(epochMilli);
            DateTimeFormatter f = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM, FormatStyle.SHORT);
            return f.withLocale(lo).format(ZonedDateTime.ofInstant(ins, ZoneId.of(zoneId)));
        }
    }
```

**总结：**

- 处理日期和时间时，尽量使用新的`java.time`包；

- 在数据库中存储时间戳时，尽量使用`long`型时间戳，它具有省空间，效率高，不依赖数据库的优点。

# 多线程

## 线程基础

### **创建线程**

方法一：从`Thread`派生一个自定义类，然后覆写`run()`方法：

```java
    public class Main {
        public static void main(String[] args) {
            Thread t = new MyThread();
            t.start(); // 启动新线程
        }
    }
    
    class MyThread extends Thread {
        @Override
        public void run() {
            System.out.println("start new thread!");
        }
    }
```

方法二：创建`Thread`实例时，传入一个`Runnable`实例（可用lambda）：

```java
    public class Main {
        public static void main(String[] args) {
            Thread t = new Thread(new MyRunnable());
            t.start(); // 启动新线程
        }
    }
    
    class MyRunnable implements Runnable {
        @Override
        public void run() {
            System.out.println("start new thread!");
        }
    }
```

**线程优先级**

```java
    Thread.setPriority(int n) // 1~10, 默认值5
```

**总结：**

- Java用`Thread`对象表示一个线程，通过调用`start()`启动一个新线程；

- 一个线程对象只能调用一次`start()`方法；

- 线程的执行代码写在`run()`方法中；

- 线程调度由操作系统决定，程序本身无法决定调度顺序；

- `Thread.sleep()`可以把当前线程暂停一段时间。

### 线程状态

在Java程序中，一个线程对象只能调用一次`start()`方法启动新线程，并在新线程中执行`run()`方法。一旦`run()`方法执行完毕，线程就结束了。因此，Java线程的状态有以下几种：

- New：新创建的线程，尚未执行；
- Runnable：运行中的线程，正在执行`run()`方法的Java代码；
- Blocked：运行中的线程，因为某些操作被阻塞而挂起；
- Waiting：运行中的线程，因为某些操作在等待中；
- Timed Waiting：运行中的线程，因为执行`sleep()`方法正在计时等待；
- Terminated：线程已终止，因为`run()`方法执行完毕。

线程终止的原因有：

- 线程正常终止：`run()`方法执行到`return`语句返回；
- 线程意外终止：`run()`方法因为未捕获的异常导致线程终止；
- 对某个线程的`Thread`实例调用`stop()`方法强制终止（强烈不推荐使用）。

**总结：**

- Java线程对象`Thread`的状态包括：`New`、`Runnable`、`Blocked`、`Waiting`、`Timed Waiting`和`Terminated`；

- 通过对另一个线程对象调用`join()`方法可以等待其执行结束；

- 可以指定等待时间，超过等待时间线程仍然没有结束就不再等待；

- 对已经运行结束的线程调用`join()`方法会立刻返回。

### 中断线程

中断一个线程非常简单，只需要在其他线程中对目标线程调用`interrupt()`方法，目标线程需要反复检测自身状态是否是interrupted状态，如果是，就立刻结束运行。

```java
    public class Main {
        public static void main(String[] args) throws InterruptedException {
            Thread t = new MyThread();
            t.start();
            Thread.sleep(1); // 暂停1毫秒
            t.interrupt(); // 中断t线程
            t.join(); // 等待t线程结束
            System.out.println("end");
        }
    }
    
    class MyThread extends Thread {
        public void run() {
            int n = 0;
            while (! isInterrupted()) {
                n ++;
                System.out.println(n + " hello!");
            }
        }
    }
```

另一个常用的中断线程的方法是设置标志位。我们通常会用一个`running`标志位来标识线程是否应该继续运行，在外部线程中，通过把`HelloThread.running`置为`false`，就可以让线程结束：

```java
    public class Main {
        public static void main(String[] args)  throws InterruptedException {
            HelloThread t = new HelloThread();
            t.start();
            Thread.sleep(1);
            t.running = false; // 标志位置为false
        }
    }
    
    class HelloThread extends Thread {
        public volatile boolean running = true;
        public void run() {
            int n = 0;
            while (running) {
                n ++;
                System.out.println(n + " hello!");
            }
            System.out.println("end!");
        }
    }
```

`volatile`关键字的目的是告诉虚拟机：

- 每次访问变量时，总是获取主内存的最新值；
- 每次修改变量后，立刻回写到主内存。

`volatile`关键字解决的是可见性问题：当一个线程修改了某个共享变量的值，其他线程能够立刻看到修改后的值。

**总结：**

- 对目标线程调用`interrupt()`方法可以请求中断一个线程，目标线程通过检测`isInterrupted()`标志获取自身是否已中断。如果目标线程处于等待状态，该线程会捕获到`InterruptedException`；

- 目标线程检测到`isInterrupted()`为`true`或者捕获了`InterruptedException`都应该立刻结束自身线程；

- 通过标志位判断需要正确使用`volatile`关键字；

- `volatile`关键字解决了共享变量在线程间的可见性问题。

### 守护线程

守护线程是指为其他线程服务的线程。在JVM中，所有非守护线程都执行完毕后，无论有没有守护线程，虚拟机都会自动退出。

因此，JVM退出时，不必关心守护线程是否已结束。

```java
    Thread t = new MyThread();
    t.setDaemon(true);
    t.start();
```

### 线程同步

保证一段代码的原子性就是通过加锁和解锁实现的。Java程序使用`synchronized`关键字对一个对象进行加锁：

```java
    synchronized(lock) {
        n = n + 1;
    }
```

使用`synchronized`解决了多线程同步访问共享变量的正确性问题。缺点是带来了性能下降。因为`synchronized`代码块无法并发执行。此外，加锁和解锁需要消耗一定的时间，所以，`synchronized`会降低程序的执行效率。**无论是否抛出异常，均会正常释放锁。**

**无需synchronized的操作**

JVM规范定义了几种原子操作：

- 基本类型（`long`和`double`除外）赋值，例如：`int n = m`；
- 引用类型赋值，例如：`List<String> list = anotherList`。

`long`和`double`是64位数据，JVM没有明确规定64位赋值操作是不是一个原子操作，不过在x64平台的JVM是把`long`和`double`的赋值作为原子操作实现的。

用`synchronized`修饰方法可以把整个方法变为同步代码块，`synchronized`方法加锁对象是`this`；

多线程能否安全访问某个非线程安全的实例，需要具体问题具体分析。

**死锁：**

Java的`synchronized`锁是可重入锁；

死锁产生的条件是多线程各自持有不同的锁，并互相试图获取对方已持有的锁，导致无限等待；

避免死锁的方法是多线程获取锁的顺序要一致。

**总结：**

- 多线程同时读写共享变量时，会造成逻辑错误，因此需要通过`synchronized`同步；

- 同步的本质就是给指定对象加锁，加锁后才能继续执行后续代码；

- 注意加锁对象必须是同一个实例；对JVM定义的单个原子操作不需要同步。

## wait 和 notify

`wait`和`notify`用于多线程协调运行：

- 在`synchronized`内部可以调用`wait()`使线程进入等待状态；
- 必须在已获得的锁对象上调用`wait()`方法；
- 在`synchronized`内部可以调用`notify()`或`notifyAll()`唤醒其他等待线程；
- 必须在已获得的锁对象上调用`notify()`或`notifyAll()`方法；
- 已唤醒的线程还需要重新获得锁后才能继续执行。

## ReentrantLock

`java.util.concurrent.locks`包提供的`ReentrantLock`用于替代`synchronized`加锁，是可重入锁：

```java
    public class Counter {
        private final Lock lock = new ReentrantLock();
        private int count;
    
        public void add(int n) {
            lock.lock();
            try {
                count += n;
            } finally {
                lock.unlock();
            }
        }
    }
```

和`synchronized`不同的是，`ReentrantLock`可以尝试获取锁：

```java
    if (lock.tryLock(1, TimeUnit.SECONDS)) {
        try {
            ...
        } finally {
            lock.unlock();
        }
    }
```

所以，使用`ReentrantLock`比直接使用`synchronized`更安全，线程在`tryLock()`失败的时候不会导致死锁。

**总结：**

- `ReentrantLock`可以替代`synchronized`进行同步；`ReentrantLock`获取锁更安全；

- 必须先获取到锁，再进入`try {...}`代码块，最后使用`finally`保证释放锁；

- 可以使用`tryLock()`尝试获取锁。

## Condition

`Condition`对象来实现`wait`和`notify`的功能。

我们仍然以`TaskQueue`为例，把前面用`synchronized`实现的功能通过`ReentrantLock`和`Condition`来实现：

```java
    class TaskQueue {
        private final Lock lock = new ReentrantLock();
        private final Condition condition = lock.newCondition();
        private Queue<String> queue = new LinkedList<>();
    
        public void addTask(String s) {
            lock.lock();
            try {
                queue.add(s);
                condition.signalAll();
            } finally {
                lock.unlock();
            }
        }
    
        public String getTask() {
            lock.lock();
            try {
                while (queue.isEmpty()) {
                    condition.await();
                }
                return queue.remove();
            } finally {
                lock.unlock();
            }
        }
    }
```

可见，使用`Condition`时，引用的`Condition`对象必须从`Lock`实例的`newCondition()`返回，这样才能获得一个绑定了`Lock`实例的`Condition`实例。

`Condition`提供的`await()`、`signal()`、`signalAll()`原理和`synchronized`锁对象的`wait()`、`notify()`、`notifyAll()`是一致的，并且其行为也是一样的：

- `await()`会释放当前锁，进入等待状态；
- `signal()`会唤醒某个等待线程；
- `signalAll()`会唤醒所有等待线程；
- 唤醒线程从`await()`返回后需要重新获得锁。

**总结：**

- `Condition`可以替代`wait`和`notify`；

- `Condition`对象必须从`Lock`对象获取。

## ReadWriteLock

使用`ReadWriteLock`可以解决这个问题，它保证：

- 只允许一个线程写入（其他线程既不能写入也不能读取）；
- 没有写入时，多个线程允许同时读（提高性能）。

用`ReadWriteLock`实现这个功能十分容易。我们需要创建一个`ReadWriteLock`实例，然后分别获取读锁和写锁：

```java
    public class Counter {
        private final ReadWriteLock rwlock = new ReentrantReadWriteLock();
        private final Lock rlock = rwlock.readLock();
        private final Lock wlock = rwlock.writeLock();
        private int[] counts = new int[10];
    
        public void inc(int index) {
            wlock.lock(); // 加写锁
            try {
                counts[index] += 1;
            } finally {
                wlock.unlock(); // 释放写锁
            }
        }
    
        public int[] get() {
            rlock.lock(); // 加读锁
            try {
                return Arrays.copyOf(counts, counts.length);
            } finally {
                rlock.unlock(); // 释放读锁
            }
        }
    }
```

**总结：**

使用`ReadWriteLock`可以提高读取效率：

- `ReadWriteLock`只允许一个线程写入；
- `ReadWriteLock`允许多个线程在没有写入时同时读取；
- `ReadWriteLock`适合读多写少的场景。

## StampedLock

要进一步提升并发执行效率，Java 8引入了新的读写锁：`StampedLock`。

`StampedLock`和`ReadWriteLock`相比，改进之处在于：读的过程中也允许获取写锁后写入！这样一来，我们读的数据就可能不一致，所以，需要一点额外的代码来判断读的过程中是否有写入，这种读锁是一种乐观锁。

```java
    public class Point {
        private final StampedLock stampedLock = new StampedLock();
    
        private double x;
        private double y;
    
        public void move(double deltaX, double deltaY) {
            long stamp = stampedLock.writeLock(); // 获取写锁
            try {
                x += deltaX;
                y += deltaY;
            } finally {
                stampedLock.unlockWrite(stamp); // 释放写锁
            }
        }
    
        public double distanceFromOrigin() {
            long stamp = stampedLock.tryOptimisticRead(); // 获得一个乐观读锁
            // 注意下面两行代码不是原子操作
            // 假设x,y = (100,200)
            double currentX = x;
            // 此处已读取到x=100，但x,y可能被写线程修改为(300,400)
            double currentY = y;
            // 此处已读取到y，如果没有写入，读取是正确的(100,200)
            // 如果有写入，读取是错误的(100,400)
            if (!stampedLock.validate(stamp)) { // 检查乐观读锁后是否有其他写锁发生
                stamp = stampedLock.readLock(); // 获取一个悲观读锁
                try {
                    currentX = x;
                    currentY = y;
                } finally {
                    stampedLock.unlockRead(stamp); // 释放悲观读锁
                }
            }
            return Math.sqrt(currentX * currentX + currentY * currentY);
        }
    }
```

可见，`StampedLock`把读锁细分为乐观读和悲观读，能进一步提升并发效率。但这也是有代价的：一是代码更加复杂，二是`StampedLock`是不可重入锁，不能在一个线程中反复获取同一个锁。

`StampedLock`还提供了更复杂的将悲观读锁升级为写锁的功能，它主要使用在if-then-update的场景：即先读，如果读的数据满足条件，就返回，如果读的数据不满足条件，再尝试写。

**总结：**

- `StampedLock`提供了乐观读锁，可取代`ReadWriteLock`以进一步提升并发性能；

- `StampedLock`是不可重入锁。

## Concurrent

除了`BlockingQueue`外，针对`List`、`Map`、`Set`、`Deque`等，`java.util.concurrent`包也提供了对应的并发集合类。我们归纳一下：

| interface | non-thread-safe       | thread-safe                            |
| --------- | --------------------- | -------------------------------------- |
| List      | ArrayList             | CopyOnWriteArrayList                   |
| Map       | HashMap               | ConcurrentHashMap                      |
| Set       | HashSet/TreeSet       | CopyOnWriteArraySet                    |
| Queue     | ArrayDeque/LinkedList | ArrayBlockingQueue/LinkedBlockingQueue |
| Deque     | ArrayDeque/LinkedList | LinkedBlockingDeque                    |

```java
    Map<String, String> map = new ConcurrentHashMap<>();
    // 在不同的线程读写:
    map.put("A", "1");
    map.put("B", "2");
    map.get("A", "1");
```

**总结：**

- 使用`java.util.concurrent`包提供的线程安全的并发集合可以大大简化多线程编程：

- 多线程同时读写并发集合是安全的；

- 尽量使用Java标准库提供的并发集合，避免自己编写同步代码。

## 使用Atomic

Java的`java.util.concurrent`包除了提供底层锁、并发集合外，还提供了一组原子操作的封装类，它们位于`java.util.concurrent.atomic`包。

我们以`AtomicInteger`为例，它提供的主要操作有：

- 增加值并返回新值：`int addAndGet(int delta)`
- 加1后返回新值：`int incrementAndGet()`
- 获取当前值：`int get()`
- 用CAS方式设置：`int compareAndSet(int expect, int update)`

Atomic类是通过无锁（lock-free）的方式实现的线程安全（thread-safe）访问。它的主要原理是利用了CAS：Compare and Set。

```java
    public int incrementAndGet(AtomicInteger var) {
        int prev, next;
        do {
            prev = var.get();
            next = prev + 1;
        } while ( ! var.compareAndSet(prev, next));
        return next;
    }
```

**总结：**

使用`java.util.concurrent.atomic`提供的原子操作可以简化多线程编程：

- 原子操作实现了无锁的线程安全；
- 适用于计数器，累加器等。

## 线程池

线程池内部维护了若干个线程，没有任务的时候，这些线程都处于等待状态。如果有新任务，就分配一个空闲线程执行。如果所有线程都处于忙碌状态，新任务要么放入队列等待，要么增加一个新线程进行处理。

因为`ExecutorService`只是接口，Java标准库提供的几个常用实现类有：

- FixedThreadPool：线程数固定的线程池；
- CachedThreadPool：线程数根据任务动态调整的线程池；
- SingleThreadExecutor：仅单线程执行的线程池。

```java
    import java.util.concurrent.*;
    
    public class Main {
        public static void main(String[] args) {
            // 创建一个固定大小的线程池:
            ExecutorService es = Executors.newFixedThreadPool(4);
            for (int i = 0; i < 6; i++) {
                es.submit(new Task("" + i));
            }
            // 关闭线程池:
            es.shutdown();
        }
    }
    
    class Task implements Runnable {
        private final String name;
    
        public Task(String name) {
            this.name = name;
        }
    
        @Override
        public void run() {
            System.out.println("start task " + name);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
            }
            System.out.println("end task " + name);
        }
    }
```

**总结：**

JDK提供了`ExecutorService`实现了线程池功能：

- 线程池内部维护一组线程，可以高效执行大量小任务；
- `Executors`提供了静态方法创建不同类型的`ExecutorService`；
- 必须调用`shutdown()`关闭`ExecutorService`；
- `ScheduledThreadPool`可以定期调度多个任务。

## Future

当我们提交一个`Callable`任务后，我们会同时获得一个`Future`对象，然后，我们在主线程某个时刻调用`Future`对象的`get()`方法，就可以获得异步执行的结果。在调用`get()`时，如果异步任务已经完成，我们就直接获得结果。如果异步任务还没有完成，那么`get()`会阻塞，直到任务完成后才返回结果。

一个`Future<V>`接口表示一个未来可能会返回的结果，它定义的方法有：

- `get()`：获取结果（可能会等待）
- `get(long timeout, TimeUnit unit)`：获取结果，但只等待指定的时间；
- `cancel(boolean mayInterruptIfRunning)`：取消当前任务；
- `isDone()`：判断任务是否已完成。

## CompletableFuture

从Java 8开始引入了`CompletableFuture`，它针对`Future`做了改进，可以传入回调对象，当异步任务完成或者发生异常时，自动调用回调对象的回调方法。

```java
    import java.util.concurrent.CompletableFuture;
    ----
    public class Main {
        public static void main(String[] args) throws Exception {
            // 创建异步执行任务:
            CompletableFuture<Double> cf = CompletableFuture.supplyAsync(Main::fetchPrice);
            // 如果执行成功:
            cf.thenAccept((result) -> {
                System.out.println("price: " + result);
            });
            // 如果执行异常:
            cf.exceptionally((e) -> {
                e.printStackTrace();
                return null;
            });
            // 主线程不要立刻结束，否则CompletableFuture默认使用的线程池会立刻关闭:
            Thread.sleep(200);
        }
    
        static Double fetchPrice() {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
            }
            if (Math.random() < 0.3) {
                throw new RuntimeException("fetch price failed!");
            }
            return 5 + Math.random() * 20;
        }
    }
```

创建一个`CompletableFuture`是通过`CompletableFuture.supplyAsync()`实现的，它需要一个实现了`Supplier`接口的对象：

```java
public interface Supplier<T> {        T get();    }   
```

这里我们用lambda语法简化了一下，直接传入`Main::fetchPrice`，因为`Main.fetchPrice()`静态方法的签名符合`Supplier`接口的定义（除了方法名外）。

紧接着，`CompletableFuture`已经被提交给默认的线程池执行了，我们需要定义的是`CompletableFuture`完成时和异常时需要回调的实例。完成时，`CompletableFuture`会调用`Consumer`对象：

```java
public interface Consumer<T> {  void accept(T t);    } 
```

异常时，`CompletableFuture`会调用`Function`对象：

```java
public interface Function<T, R> {  R apply(T t);    }   
```

这里我们都用lambda语法简化了代码。

可见`CompletableFuture`的优点是：

- 异步任务结束时，会自动回调某个对象的方法；
- 异步任务出错时，会自动回调某个对象的方法；
- 主线程设置好回调后，不再关心异步任务的执行。

如果只是实现了异步回调机制，我们还看不出`CompletableFuture`相比`Future`的优势。`CompletableFuture`更强大的功能是，多个`CompletableFuture`可以串行执行，例如，定义两个`CompletableFuture`，第一个`CompletableFuture`根据证券名称查询证券代码，第二个`CompletableFuture`根据证券代码查询证券价格，这两个`CompletableFuture`实现串行操作如下：

```java
    import java.util.concurrent.CompletableFuture;
    ----
    public class Main {
        public static void main(String[] args) throws Exception {
            // 第一个任务:
            CompletableFuture<String> cfQuery = CompletableFuture.supplyAsync(() -> {
                return queryCode("中国石油");
            });
            // cfQuery成功后继续执行下一个任务:
            CompletableFuture<Double> cfFetch = cfQuery.thenApplyAsync((code) -> {
                return fetchPrice(code);
            });
            // cfFetch成功后打印结果:
            cfFetch.thenAccept((result) -> {
                System.out.println("price: " + result);
            });
            // 主线程不要立刻结束，否则CompletableFuture默认使用的线程池会立刻关闭:
            Thread.sleep(2000);
        }
    
        static String queryCode(String name) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
            }
            return "601857";
        }
    
        static Double fetchPrice(String code) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
            }
            return 5 + Math.random() * 20;
        }
    }
```

除了串行执行外，多个`CompletableFuture`还可以并行执行。

```java
   import java.util.concurrent.CompletableFuture;
    ----
    public class Main {
        public static void main(String[] args) throws Exception {
            // 两个CompletableFuture执行异步查询:
            CompletableFuture<String> cfQueryFromSina = CompletableFuture.supplyAsync(() -> {
                return queryCode("中国石油", "https://finance.sina.com.cn/code/");
            });
            CompletableFuture<String> cfQueryFrom163 = CompletableFuture.supplyAsync(() -> {
                return queryCode("中国石油", "https://money.163.com/code/");
            });
    
            // 用anyOf合并为一个新的CompletableFuture:
            CompletableFuture<Object> cfQuery = CompletableFuture.anyOf(cfQueryFromSina, cfQueryFrom163);
    
            // 两个CompletableFuture执行异步查询:
            CompletableFuture<Double> cfFetchFromSina = cfQuery.thenApplyAsync((code) -> {
                return fetchPrice((String) code, "https://finance.sina.com.cn/price/");
            });
            CompletableFuture<Double> cfFetchFrom163 = cfQuery.thenApplyAsync((code) -> {
                return fetchPrice((String) code, "https://money.163.com/price/");
            });
    
            // 用anyOf合并为一个新的CompletableFuture:
            CompletableFuture<Object> cfFetch = CompletableFuture.anyOf(cfFetchFromSina, cfFetchFrom163);
    
            // 最终结果:
            cfFetch.thenAccept((result) -> {
                System.out.println("price: " + result);
            });
            // 主线程不要立刻结束，否则CompletableFuture默认使用的线程池会立刻关闭:
            Thread.sleep(200);
        }
    
        static String queryCode(String name, String url) {
            System.out.println("query code from " + url + "...");
            try {
                Thread.sleep((long) (Math.random() * 100));
            } catch (InterruptedException e) {
            }
            return "601857";
        }
    
        static Double fetchPrice(String code, String url) {
            System.out.println("query price from " + url + "...");
            try {
                Thread.sleep((long) (Math.random() * 100));
            } catch (InterruptedException e) {
            }
            return 5 + Math.random() * 20;
        }
    }
```

除了`anyOf()`可以实现“任意个`CompletableFuture`只要一个成功”，`allOf()`可以实现“所有`CompletableFuture`都必须成功”，这些组合操作可以实现非常复杂的异步流程控制。

最后我们注意`CompletableFuture`的命名规则：

- `xxx()`：表示该方法将继续在已有的线程中执行；
- `xxxAsync()`：表示将异步在线程池中执行。

**总结：**

`CompletableFuture`可以指定异步处理流程：

- `thenAccept()`处理正常结果；
- `exceptional()`处理异常结果；
- `thenApplyAsync()`用于串行化另一个`CompletableFuture`；
- `anyOf()`和`allOf()`用于并行化多个`CompletableFuture`。

## ForkJoin

Java 7开始引入了一种新的Fork/Join线程池，它可以执行一种特殊的任务：把一个大任务拆成多个小任务并行执行。

```java
    import java.util.Random;
    import java.util.concurrent.*;
    ----
    public class Main {
        public static void main(String[] args) throws Exception {
            // 创建2000个随机数组成的数组:
            long[] array = new long[2000];
            long expectedSum = 0;
            for (int i = 0; i < array.length; i++) {
                array[i] = random();
                expectedSum += array[i];
            }
            System.out.println("Expected sum: " + expectedSum);
            // fork/join:
            ForkJoinTask<Long> task = new SumTask(array, 0, array.length);
            long startTime = System.currentTimeMillis();
            Long result = ForkJoinPool.commonPool().invoke(task);
            long endTime = System.currentTimeMillis();
            System.out.println("Fork/join sum: " + result + " in " + (endTime - startTime) + " ms.");
        }
    
        static Random random = new Random(0);
    
        static long random() {
            return random.nextInt(10000);
        }
    }
    
    class SumTask extends RecursiveTask<Long> {
        static final int THRESHOLD = 500;
        long[] array;
        int start;
        int end;
    
        SumTask(long[] array, int start, int end) {
            this.array = array;
            this.start = start;
            this.end = end;
        }
    
        @Override
        protected Long compute() {
            if (end - start <= THRESHOLD) {
                // 如果任务足够小,直接计算:
                long sum = 0;
                for (int i = start; i < end; i++) {
                    sum += this.array[i];
                    // 故意放慢计算速度:
                    try {
                        Thread.sleep(1);
                    } catch (InterruptedException e) {
                    }
                }
                return sum;
            }
            // 任务太大,一分为二:
            int middle = (end + start) / 2;
            System.out.println(String.format("split %d~%d ==> %d~%d, %d~%d", start, end, start, middle, middle, end));
            SumTask subtask1 = new SumTask(this.array, start, middle);
            SumTask subtask2 = new SumTask(this.array, middle, end);
            invokeAll(subtask1, subtask2);
            Long subresult1 = subtask1.join();
            Long subresult2 = subtask2.join();
            Long result = subresult1 + subresult2;
            System.out.println("result = " + subresult1 + " + " + subresult2 + " ==> " + result);
            return result;
        }
    }
```

核心代码`SumTask`继承自`RecursiveTask`，在`compute()`方法中，关键是如何“分裂”出子任务并且提交子任务：

```java
    class SumTask extends RecursiveTask<Long> {
        protected Long compute() {
            // “分裂”子任务:
            SumTask subtask1 = new SumTask(...);
            SumTask subtask2 = new SumTask(...);
            // invokeAll会并行运行两个子任务:
            invokeAll(subtask1, subtask2);
            // 获得子任务的结果:
            Long subresult1 = subtask1.join();
            Long subresult2 = subtask2.join();
            // 汇总结果:
            return subresult1 + subresult2;
        }
    }
```

Fork/Join线程池在Java标准库中就有应用。Java标准库提供的`java.util.Arrays.parallelSort(array)`可以进行并行排序，它的原理就是内部通过Fork/Join对大数组分拆进行并行排序，在多核CPU上就可以大大提高排序的速度。

**总结：**

- Fork/Join是一种基于“分治”的算法：通过分解任务，并行执行，最后合并结果得到最终结果。

- `ForkJoinPool`线程池可以把一个大任务分拆成小任务并行执行，任务类必须继承自`RecursiveTask`或`RecursiveAction`。

- 使用Fork/Join模式可以进行并行计算以提高效率。

## ThreadLocal

多线程是Java实现多任务的基础，`Thread`对象代表一个线程，我们可以在代码中调用`Thread.currentThread()`获取当前线程。

可以把`ThreadLocal`看成一个全局`Map<Thread, Object>`：每个线程获取`ThreadLocal`变量时，总是使用`Thread`自身作为key：

```java
    Object threadLocalValue = threadLocalMap.get(Thread.currentThread());
```

因此，`ThreadLocal`相当于给每个线程都开辟了一个独立的存储空间，各个线程的`ThreadLocal`关联的实例互不干扰。

最后，特别注意`ThreadLocal`一定要在`finally`中清除：

```java
    try {
        threadLocalUser.set(user);
        ...
    } finally {
        threadLocalUser.remove();
    }
```

这是因为当前线程执行完相关代码后，很可能会被重新放入线程池中，如果`ThreadLocal`没有被清除，该线程执行其他代码时，会把上一次的状态带进去。

为了保证能释放`ThreadLocal`关联的实例，我们可以通过`AutoCloseable`接口配合`try (resource) {...}`结构，让编译器自动为我们关闭。例如，一个保存了当前用户名的`ThreadLocal`可以封装为一个`UserContext`对象：

```java
    public class UserContext implements AutoCloseable {
    
        static final ThreadLocal<String> ctx = new ThreadLocal<>();
    
        public UserContext(String user) {
            ctx.set(user);
        }
    
        public static String currentUser() {
            return ctx.get();
        }
    
        @Override
        public void close() {
            ctx.remove();
        }
    }
```

使用的时候，我们借助`try (resource) {...}`结构，可以这么写：

```java
    try (var ctx = new UserContext("Bob")) {
        // 可任意调用UserContext.currentUser():
        String currentUser = UserContext.currentUser();
    } // 在此自动调用UserContext.close()方法释放ThreadLocal关联对象
```

这样就在`UserContext`中完全封装了`ThreadLocal`，外部代码在`try (resource) {...}`内部可以随时调用`UserContext.currentUser()`获取当前线程绑定的用户名。

**总结：**

- `ThreadLocal`表示线程的“局部变量”，它确保每个线程的`ThreadLocal`变量都是各自独立的；

- `ThreadLocal`适合在一个线程的处理流程中保持上下文（避免了同一参数在所有方法中传递）；

- 使用`ThreadLocal`要用`try ... finally`结构，并在`finally`中清除。

# 正则表达式

元字符是在Java正则表达式中具有特殊含义的字符。

Java中的正则表达式支持的元字符如下:

```
 ( ) [ ] { { \ ^ $ | ? * + . < > - = !
```

**字符类**

元字符`[`和`]`指定正则表达式中的字符类。

字符类是一组字符。正则表达式引擎将尝试匹配集合中的一个字符。

字符类“[ABC]"将匹配字符A，B或C。

范围使用连字符`-`字符表示。

- `[A-Z]`表示任何大写英文字母;“[0-9]"表示0和9之间的任何数字。
- `[^ ABC]`表示除A，B和C以外的任何字符。
- `[^ A-Z]`表示除大写字母之外的任何字符。

**预定义字符类**

|      | 含义                                                         |
| ---- | ------------------------------------------------------------ |
| .    | 任何字符                                                     |
| \d   | 数字。与[0-9]相同                                            |
| \D   | 非数字。与[ ^ 0-9 ]相同                                      |
| \s   | 空格字符。包括与[\t\n\x0B\f\r]相同。空格标签换行符垂直标签表单Feed回车字符 |
| \S   | 非空白字符。与[ ^ \s]相同                                    |
| \w   | 一个字符。与[a-zA-Z_0-9]相同。                               |
| \W   | 非字字符。与[ ^ \w]相同。                                    |

```java
    import java.util.regex.Matcher;
    import java.util.regex.Pattern;
    
    public class Main {  
      public static void main(String args[]) {    
        String regex = "\\w+";    
        Pattern pattern = Pattern.compile(regex);    
    
        String candidate = "asdf Java2s.com";    
        Matcher matcher = pattern.matcher(candidate);    
        if (matcher.find()) {      
         System.out.println("GROUP 0:" + matcher.group(0));    
        }  
     }
    }
```

## 模式

包`java.util.regex`包含三个类，以支持正则表达式的完整版本。

- 模式
- 匹配
- PatternSyntaxException

`Pattern`保存正则表达式的编译形式。

`Matcher`将要匹配的字符串与模式相关联，并执行实际匹配。

`PatternSyntaxException`表示格式错误的正则表达式中的错误。

没有公共构造函数的模式是不可变的，可以共享。

`Pattern`类包含一个静态compile()方法，它返回一个`Pattern`对象。

`compile()`方法是重载的。

```java
    static Pattern  compile(String regex)
    static Pattern compile(String regex, int flags)
```

以下代码将正则表达式编译为Pattern对象:

```java
    import java.util.regex.Pattern;
    
    public class Main {
      public static void main(String[] args) {
        // Prepare a regular expression
        String regex = "[a-z]@.";
    
        // Compile the regular expression into a Pattern object
        Pattern p = Pattern.compile(regex);
      }
    }
```

compile()方法的第二个版本设置修改模式匹配方式的标志。

flags参数是一个位掩码，并在Pattern类中定义为int常量。

| Flag                            | 描述                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| Pattern.CANON_EQ                | 启用规范等效。                                               |
| Pattern.CASE_INSENSITIVE        | 启用不区分大小写的匹配。                                     |
| Pattern.COMMENTS                | 启用不区分大小写的匹配。...                                  |
| Pattern.DOTALL                  | 允许在模式中的空格和注释。忽略以＃开头的空格和嵌入的注释，直到行的结尾。 |
| Pattern.LITERAL                 | 启用模式的文字解析。这个标志使元字符和转义序列作为正常字符。 |
| Pattern.MULTILINE               | 启用多行模式。默认情况下，^和$匹配输入序列的开始和结束。此标志使模式仅逐行匹配或输入序列的末尾。 |
| Pattern.UNICODE_CASE            | 启用支持Unicode的大小写。与CASE_INSENSITIVE标志一起，可以根据Unicode标准执行不区分大小写的匹配。 |
| Pattern.UNICODE_CHARACTER_CLASS | 启用预定义字符类和POSIX字符类的Unicode版本。设置此标志时，预定义字符类和POSIX字符类符合Unicode技术标准。 |
| Pattern.UNIX_LINES              | 启用Unix行模式。设置此标志时，只有\n字符被识别为行终止符。   |

## 匹配

`Matcher`类对字符序列执行匹配通过解释在`Pattern`对象中定义的编译模式。

`Pattern`类的`matcher()`方法创建一个实例的`Matcher`类。

```java
   public class Main {
      public static void main(String[] args) {
        String regex   = "[a-z]@.";
        Pattern p  = Pattern.compile(regex);
        String str = "abc@yahoo.com,123@cnn.com,abc@google.com";
        Matcher  m   = p.matcher(str);
      }
    }
```

匹配器的以下方法执行匹配。

- find() method
- start() method
- end() method
- group() method

**find**

`find()`方法找到输入中的模式的匹配。

如果find成功，它返回true。否则，它返回false。

第一次调用`find()`在输入的开始处开始搜索。下一个调用将在上一次匹配后开始搜索。

我们可以使用while循环与`find()`方法来查找所有的匹配。

`find()`方法是一个重载的方法。find()方法的另一个版本接受一个整数参数，这是开始查找匹配的偏移量。

**start**

`find()`方法返回上一次匹配的开始索引。 它在成功的find()方法调用之后使用。

**end**

`end()`方法返回匹配字符串中最后一个字符的索引加一。

匹配后，`str.substring(m.start()，m.end())`给出匹配的字符串。

**group**

group()方法通过前一个成功的find()方法调用返回找到的字符串。

## 量词

我们可以指定正则表达式中的字符的次数可以匹配字符序列。

为了使用正则表达式表达一个数字或更多的模式，我们可以使用量词。

下表列出了量词及其含义。

| 量词   | 含义               |
| ------ | ------------------ |
| *      | 零次或更多次       |
| +      | 一次或多次         |
| ?      | 一次或根本不       |
| {m}    | 正好m次            |
| {m,}   | 至少m次            |
| {m，n} | 至少m，但不超过n次 |

量词必须遵循字符或字符类。

## 边界

要匹配一行的开头，或匹配整个单词，不是任何单词的一部分，我们必须为匹配器设置边界。

下表列出了正则表达式中的边界匹配器

| 边界匹配 | 含义                                       |
| -------- | ------------------------------------------ |
| ^        | 一行的开始                                 |
| $        | 一行的结束                                 |
| \b       | 字边界                                     |
| \B       | 非字边界                                   |
| \A       | 输入的开始                                 |
| \G       | 上一次匹配的结束                           |
| \Z       | 输入的结束，但是对于最终终止符，如果有的话 |
| \z       | 输入的结束                                 |

## 组

我们可以通过括号将多个字符组合为一个单位。例如，`(ab)`。

正则表达式中的每个组都有一个组号，从1开始。

Matcher类中的方法groupCount()返回与Matcher实例相关联的模式中的组数。

组0引用整个正则表达式和不由groupCount()方法报告。

正则表达式中的每个左括号标记新组的开始。

我们可以在正则表达式中返回引用组号。

假设我们要匹配以“abc"开头，后跟“xyz"的文本，后跟“abc"。

我们可以写一个正则表达式为“abcxyzabc"。

我们可以使用反向引用将正则表达式重写为“(abc)xyz \\ 1"。`\1`指第1组，即`(abc)`。

`\2`引用组2，`\3`引用组3，依此类推。

**命名组**

我们可以在正则表达式中使用命名组。

我们可以命名一个组，然后使用他们的名字来引用参考组。

我们可以在替换文本中引用组名称，并使用组名称获取匹配的文本。

定义命名组的格式为

```
 (?<groupName>pattern)    
```

一对括号标记一个组。开始括号后面跟着一个`?`和放在尖括号中的组名称。

组名称只能包含字母和数字，且只能以字母开头。

**组边界**

我们可以使用`start()`和`end()`方法来获取组的匹配边界。 这些方法重载:

```java
    int start()
    int start(int groupNumber)
    int start(String groupName)
    int end()
    int end(int groupNumber)
    int  end(String groupName)
```

## 查找替换

我们可以找到一个模式，并用一些文本替换，替换的文本取决于匹配的文本。

在Java中，我们可以在Matcher类中使用以下两个方法来完成这个任务。

```java
Matcher appendReplacement(StringBuffer sb, String replacement)    
StringBuffer appendTail(StringBuffer sb)    
```

# 网络编程

## TCP

**服务器**

`ServerSocket`类的一个对象表示Java中的TCP服务器套接字。

`ServerSocket`对象可以接受来自远程客户端的连接请求。

我们可以使用no-args构造函数创建一个未绑定的服务器套接字，并使用其bind()方法将其绑定到本地端口和本地IP地址。

- 要接受远程连接请求，请调用服务器套接字上的`accept()`方法。accept()方法调用阻塞执行，直到来自远程客户端的请求到达其等待队列。
- Socket类包含两个方法`getInputStream()`和`getOutputStream()`用于读取和写入连接的套接字。
- 使用套接字的close()方法关闭连接。关闭套接字还会关闭其输入和输出流。

```java
    import java.io.BufferedReader;
    import java.io.BufferedWriter;
    import java.io.InputStreamReader;
    import java.io.OutputStreamWriter;
    import java.net.InetAddress;
    import java.net.ServerSocket;
    import java.net.Socket;
    
    public class Main {
      public static void main(String[] args) throws Exception {
        ServerSocket serverSocket = new ServerSocket(12900, 100,
            InetAddress.getByName("localhost"));
        System.out.println("Server started  at:  " + serverSocket);
    
        while (true) {
          System.out.println("Waiting for a  connection...");
    
          final Socket activeSocket = serverSocket.accept();
    
          System.out.println("Received a  connection from  " + activeSocket);
          Runnable runnable = () -> handleClientRequest(activeSocket);
          new Thread(runnable).start(); // start a new thread
        }
      }
    
      public static void handleClientRequest(Socket socket) {
        try{
          BufferedReader socketReader = null;
          BufferedWriter socketWriter = null;
          socketReader = new BufferedReader(new InputStreamReader(
              socket.getInputStream()));
          socketWriter = new BufferedWriter(new OutputStreamWriter(
              socket.getOutputStream()));
    
          String inMsg = null;
          while ((inMsg = socketReader.readLine()) != null) {
            System.out.println("Received from  client: " + inMsg);
    
            String outMsg = inMsg;
            socketWriter.write(outMsg);
            socketWriter.write("\n");
            socketWriter.flush();
          }
          socket.close();
        }catch(Exception e){
          e.printStackTrace();
        }
    
      }
    }
    
```

**客户端**

`Socket`类表示一个TCP客户端套接字。

- 创建TCP客户端套接字
- 使用getInputStream()和getOutputStream()方法使用其输入和输出流
- 关闭

```java
    import java.io.BufferedReader;
    import java.io.BufferedWriter;
    import java.io.InputStreamReader;
    import java.io.OutputStreamWriter;
    import java.net.Socket;
    
    public class Main {
      public static void main(String[] args) throws Exception {
        Socket socket = new Socket("localhost", 12900);
        System.out.println("Started client  socket at "
            + socket.getLocalSocketAddress());
        BufferedReader socketReader = new BufferedReader(new InputStreamReader(
            socket.getInputStream()));
        BufferedWriter socketWriter = new BufferedWriter(new OutputStreamWriter(
            socket.getOutputStream()));
        BufferedReader consoleReader = new BufferedReader(
            new InputStreamReader(System.in));
    
        String promptMsg = "Please enter a  message  (Bye  to quit):";
        String outMsg = null;
    
        System.out.print(promptMsg);
        while ((outMsg = consoleReader.readLine()) != null) {
          if (outMsg.equalsIgnoreCase("bye")) {
            break;
          }
          // Add a new line to the message to the server,
          // because the server reads one line at a time.
          socketWriter.write(outMsg);
          socketWriter.write("\n");
          socketWriter.flush();
    
          // Read and display the message from the server
          String inMsg = socketReader.readLine();
          System.out.println("Server: " + inMsg);
          System.out.println(); // Print a blank line
          System.out.print(promptMsg);
        }
        socket.close();
      }
    }
    
```

## UDP

当编码UDP连接时，使用以下两个类。

- `DatagramPacket`类表示UDP数据报。
- `DatagramSocket`类表示用于发送或接收数据报包的UDP套接字。

```java
    DatagramSocket udpSocket = new DatagramSocket(12345, "localhost");
//DatagramSocket类提供了一个bind()方法，它允许您将套接字绑定到本地IP地址和本地端口号。
```

**DatagramPacket**

DatagramPacket包含三个东西：

- 目的IP地址
- 目的端口号
- 数据

DatagramPacket类的构造函数创建一个数据包来接收数据如下：

```java
    DatagramPacket(byte[] buf,  int  length)
    DatagramPacket(byte[] buf,  int offset, int length)
```

DatagramPacket类的构造函数创建一个数据包来发送数据如下：

```java
    DatagramPacket(byte[] buf,  int  length,  InetAddress address, int port)
    DatagramPacket(byte[] buf,  int offset, int length,  InetAddress address, int port)
    DatagramPacket(byte[] buf,  int length, SocketAddress address)
    DatagramPacket(byte[] buf,  int offset, int length, SocketAddress address)
```

数据包中的数据总是指定偏移量和长度。我们需要使用offset和length来读取数据包中的数据。

**服务器**

```java
    import java.net.DatagramPacket;
    import java.net.DatagramSocket;
    import java.net.InetAddress;
    
    public class Main {
      public static void main(String[] args) throws Exception {
        final int LOCAL_PORT = 12345;
        final String SERVER_NAME = "localhost";
        DatagramSocket udpSocket = new DatagramSocket(LOCAL_PORT,
            InetAddress.getByName(SERVER_NAME));
    
        System.out.println("Created UDP  server socket at "
            + udpSocket.getLocalSocketAddress() + "...");
        while (true) {
          System.out.println("Waiting for a  UDP  packet...");
          DatagramPacket packet = new DatagramPacket(new byte[1024], 1024);
          udpSocket.receive(packet);
          displayPacketDetails(packet);
          udpSocket.send(packet);
        }
      }
      public static void displayPacketDetails(DatagramPacket packet) {
        byte[] msgBuffer = packet.getData();
        int length = packet.getLength();
        int offset = packet.getOffset();
    
        int remotePort = packet.getPort();
        InetAddress remoteAddr = packet.getAddress();
        String msg = new String(msgBuffer, offset, length);
    
        System.out.println("Received a  packet:[IP Address=" + remoteAddr
            + ", port=" + remotePort + ", message=" + msg + "]");
      }
    }
    
```

**客户端**

```java
    import java.io.BufferedReader;
    import java.io.InputStreamReader;
    import java.net.DatagramPacket;
    import java.net.DatagramSocket;
    import java.net.InetAddress;
    import java.net.UnknownHostException;
    
    public class Main {
      public static void main(String[] args) throws Exception {
        DatagramSocket udpSocket = new DatagramSocket();
        String msg = null;
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        System.out.print("Please enter a  message  (Bye  to quit):");
        while ((msg = br.readLine()) != null) {
          if (msg.equalsIgnoreCase("bye")) {
            break;
          }
          DatagramPacket packet = Main.getPacket(msg);
          udpSocket.send(packet);
          udpSocket.receive(packet);
          displayPacketDetails(packet);
          System.out.print("Please enter a  message  (Bye  to quit):");
        }
        udpSocket.close();
      }
      public static void displayPacketDetails(DatagramPacket packet) {
        byte[] msgBuffer = packet.getData();
        int length = packet.getLength();
        int offset = packet.getOffset();
        int remotePort = packet.getPort();
        InetAddress remoteAddr = packet.getAddress();
        String msg = new String(msgBuffer, offset, length);
        System.out.println("[Server at IP  Address=" + remoteAddr + ", port="
            + remotePort + "]: " + msg);
      }
      public static DatagramPacket getPacket(String msg)
          throws UnknownHostException {
        int PACKET_MAX_LENGTH = 1024;
        byte[] msgBuffer = msg.getBytes();
    
        int length = msgBuffer.length;
        if (length > PACKET_MAX_LENGTH) {
          length = PACKET_MAX_LENGTH;
        }
        DatagramPacket packet = new DatagramPacket(msgBuffer, length);
        InetAddress serverIPAddress = InetAddress.getByName("localhost");
        packet.setAddress(serverIPAddress);
        packet.setPort(15900);
        return packet;
      }
    }
```

**多播**

Java使用MulticastSocket类来创建UDP多播套接字，以接收发送到多播IP地址的数据包。

组播套接字基于组成员资格。创建并绑定组播套接字后，调用其joinGroup（InetAddress multiCastIPAddress）方法加入组播组，发送到该组的任何数据包数据包将被传递到此套接字。

要离开组，请调用leaveGroup（InetAddress multiCastIPAddress）方法。

**多播客户端**

```java
    import java.net.DatagramPacket;
    import java.net.InetAddress;
    import java.net.MulticastSocket;
    
    public class Main {
      public static void main(String[] args) throws Exception {
        int mcPort = 12345;
        String mcIPStr = "230.1.1.1";
        MulticastSocket mcSocket = null;
        InetAddress mcIPAddress = null;
        mcIPAddress = InetAddress.getByName(mcIPStr);
        mcSocket = new MulticastSocket(mcPort);
        System.out.println("Multicast Receiver running at:"
            + mcSocket.getLocalSocketAddress());
        mcSocket.joinGroup(mcIPAddress);
    
        DatagramPacket packet = new DatagramPacket(new byte[1024], 1024);
    
        System.out.println("Waiting for a  multicast message...");
        mcSocket.receive(packet);
        String msg = new String(packet.getData(), packet.getOffset(),
            packet.getLength());
        System.out.println("[Multicast  Receiver] Received:" + msg);
    
        mcSocket.leaveGroup(mcIPAddress);
        mcSocket.close();
      }
    }
```

**多播服务端**

```java
    import java.net.DatagramPacket;
    import java.net.DatagramSocket;
    import java.net.InetAddress;
    
    public class Main {
      public static void main(String[] args) throws Exception {
        int mcPort = 12345;
        String mcIPStr = "230.1.1.1";
        DatagramSocket udpSocket = new DatagramSocket();
    
        InetAddress mcIPAddress = InetAddress.getByName(mcIPStr);
        byte[] msg = "Hello".getBytes();
        DatagramPacket packet = new DatagramPacket(msg, msg.length);
        packet.setAddress(mcIPAddress);
        packet.setPort(mcPort);
        udpSocket.send(packet);
    
        System.out.println("Sent a  multicast message.");
        System.out.println("Exiting application");
        udpSocket.close();
      }
    }
```

## URL

绝对URI具有以下通用格式：

```
scheme:scheme-specific-part
```

`scheme-specific-part`取决于`scheme`。

例如，`http`scheme使用一种格式，并且`mailto`scheme使用另一种格式。

URI的另一个通用形式如下，它表示一个URL。

```
scheme://<authority><path>?<query>#<fragment>    
```

`scheme`访问资源的方法。它是协议名称，如http，ftp等。

URI中的`scheme`和`path`部分是必需的。所有其他部分均为可选。

`path`部分可以是空字符串。权限部分指示服务器名称或IP地址。

如果权限部分表示服务器名称，则它可以是`userinfo @ host:port`的形式。

例如，标识本地文件系统中的文件的URL使用文件方案作为`file:///c:/documents/java.doc`。

**概念**

URI语法在其`path`部分中使用分层语法。

`path`的多个部分由正斜杠(/)分隔。

`query`部分指示通过执行指定的查询获得资源。

它由名称 - 值对组成，用`&`符号分隔。

名称和值由等号`=`分隔。

```jav
    URI:        //www.w3cschool.cn/java/a.html?id=123#abc
    Scheme:     http 
    Authority:  www.w3cschool.cn 
    Path:       /java/a.html 
    Query:      id=123
    Fragment:   abc
```

Java表示一个URI和一个URL作为对象。

它提供以下四个类，您可以使用它们将URI和URL用作Java程序中的对象：

```
java.net.URI    
java.net.URL    
java.net.URLEncoder    
java.net.URLDecoder    
```

以下代码创建一个URI对象。

```java
URI baseURI  = new URI("http://www.www.w3cschool.cn"); 
```

要创建具有相对URI字符串的URI，并使用baseURI解析它

```java
URI baseURI  = new URI("http://www.www.w3cschool.cn"); URI relativeURI = new URI("welcome.html");    
URI resolvedRelativeURI = baseURI.resolve(relativeURI);
```

**URLEncoder和URLDecoder**

URLEncoder和URLDecoder类分别用于对字符串进行编码和解码。

```java
    import java.net.URLDecoder;
    import java.net.URLEncoder;
    
    public class Main {
      public static void main(String[] args) throws Exception {
        String source = "index&^%*a test for 2.5% and  &";
        String encoded = URLEncoder.encode(source, "utf-8");
        String decoded = URLDecoder.decode(encoded, "utf-8");
        System.out.println("Source: " + source);
        System.out.println("Encoded: " + encoded);
        System.out.println("Decoded: " + decoded);
    
      }
    }
```

## 非阻塞套接字

**服务器套接字通道**

要创建选择器对象，请调用其open()静态方法。

```java
Selector selector = Selector.open();    
```

ServerSocketChannel用于监听来自客户端的新连接请求。

调用其open()静态方法来创建一个ServerSocketChannel。

```java
ServerSocketChannel ssChannel = ServerSocketChannel.open();    
```

默认情况下，服务器套接字通道或套接字通道是阻塞通道。要使其成为非阻塞通道，请调用以下方法。

```java
ssChannel.configureBlocking(false);    
```

**选择器**

服务器套接字必须向选择器注册才能执行某些操作。

有四种操作，我们可以用选择器注册一个通道。

- 使用SelectionKey.OP_CONNECT连接操作，可以在客户端为SocketChannel注册。选择器将通知有关连接操作进度。
- 使用SelectionKey.OP_ACCEPT接受操作，可以在服务器上为ServerSocketChannel注册。当客户端请求新连接到达时，选择器将通知。
- 使用SelectionKey.OP_READ读取操作，可以在客户端和服务器上为SocketChannel注册。选择器将在通道准备好读取某些数据时通知。
- 使用SelectionKey.OP_WRITE进行写操作，可以在客户端和服务器上为SocketChannel注册。选择器将在通道准备好写入某些数据时通知。

**服务端**

```java
    import java.net.InetAddress;
    import java.net.InetSocketAddress;
    import java.nio.ByteBuffer;
    import java.nio.channels.SelectionKey;
    import java.nio.channels.Selector;
    import java.nio.channels.ServerSocketChannel;
    import java.nio.channels.SocketChannel;
    import java.util.Iterator;
    import java.util.Set;
    
    public class Main {
      public static void main(String[] args) throws Exception {
        InetAddress hostIPAddress = InetAddress.getByName("localhost");
        int port = 19000;
        Selector selector = Selector.open();
        ServerSocketChannel ssChannel = ServerSocketChannel.open();
        ssChannel.configureBlocking(false);
        ssChannel.socket().bind(new InetSocketAddress(hostIPAddress, port));
        ssChannel.register(selector, SelectionKey.OP_ACCEPT);
        while (true) {
          if (selector.select() <= 0) {
            continue;
          }
          processReadySet(selector.selectedKeys());
        }
      }
      public static void processReadySet(Set readySet) throws Exception {
        Iterator iterator = readySet.iterator();
        while (iterator.hasNext()) {
          SelectionKey key = (SelectionKey) iterator.next();
          iterator.remove();
          if (key.isAcceptable()) {
            ServerSocketChannel ssChannel = (ServerSocketChannel) key.channel();
            SocketChannel sChannel = (SocketChannel) ssChannel.accept();
            sChannel.configureBlocking(false);
            sChannel.register(key.selector(), SelectionKey.OP_READ);
          }
          if (key.isReadable()) {
            String msg = processRead(key);
            if (msg.length() > 0) {
              SocketChannel sChannel = (SocketChannel) key.channel();
              ByteBuffer buffer = ByteBuffer.wrap(msg.getBytes());
              sChannel.write(buffer);
            }
          }
        }
      }
      public static String processRead(SelectionKey key) throws Exception {
        SocketChannel sChannel = (SocketChannel) key.channel();
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        int bytesCount = sChannel.read(buffer);
        if (bytesCount > 0) {
          buffer.flip();
          return new String(buffer.array());
        }
        return "NoMessage";
      }
    }
    
```

**客户端**

```java
    import java.io.BufferedReader;
    import java.io.InputStreamReader;
    import java.net.InetAddress;
    import java.net.InetSocketAddress;
    import java.nio.ByteBuffer;
    import java.nio.CharBuffer;
    import java.nio.channels.SelectionKey;
    import java.nio.channels.Selector;
    import java.nio.channels.SocketChannel;
    import java.nio.charset.Charset;
    import java.nio.charset.CharsetDecoder;
    import java.util.Iterator;
    import java.util.Set;
    /*from www.w3cschool.cn*/
    public class Main {
      static BufferedReader userInputReader = null;
    
      public static boolean processReadySet(Set readySet) throws Exception {
        Iterator iterator = readySet.iterator();
        while (iterator.hasNext()) {
          SelectionKey key = (SelectionKey)
           iterator.next();
          iterator.remove();
          if (key.isConnectable()) {
            boolean connected = processConnect(key);
            if (!connected) {
              return true; // Exit
            }
          }
          if (key.isReadable()) {
            String msg = processRead(key);
            System.out.println("[Server]: " + msg);
          }
          if (key.isWritable()) {
            System.out.print("Please enter a message(Bye to quit):");
            String msg = userInputReader.readLine();
            
            if (msg.equalsIgnoreCase("bye")) {
              return true; // Exit
            }
            SocketChannel sChannel = (SocketChannel) key.channel();
            ByteBuffer buffer = ByteBuffer.wrap(msg.getBytes());
            sChannel.write(buffer);
          }
        }
        return false; // Not done yet
      }
      public static boolean processConnect(SelectionKey key) throws Exception{
        SocketChannel channel = (SocketChannel) key.channel();
        while (channel.isConnectionPending()) {
          channel.finishConnect();
        }
        return true;
      }
      public static String processRead(SelectionKey key) throws Exception {
        SocketChannel sChannel = (SocketChannel) key.channel();
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        sChannel.read(buffer);
        buffer.flip();
        Charset charset = Charset.forName("UTF-8");
        CharsetDecoder decoder = charset.newDecoder();
        CharBuffer charBuffer = decoder.decode(buffer);
        String msg = charBuffer.toString();
        return msg;
      }
      public static void main(String[] args) throws Exception {
        InetAddress serverIPAddress = InetAddress.getByName("localhost");
        int port = 19000;
        InetSocketAddress serverAddress = new InetSocketAddress(
            serverIPAddress, port);
        Selector selector = Selector.open();
        SocketChannel channel = SocketChannel.open();
        channel.configureBlocking(false);
        channel.connect(serverAddress);
        int operations = SelectionKey.OP_CONNECT | SelectionKey.OP_READ
            | SelectionKey.OP_WRITE;
        channel.register(selector, operations);
    
        userInputReader = new BufferedReader(new InputStreamReader(System.in));
        while (true) {
          if (selector.select() > 0) {
            boolean doneStatus = processReadySet(selector.selectedKeys());
            if (doneStatus) {
              break;
            }
          }
        }
        channel.close();
      }
    }
    
```

## 异步套接字

使用以下两个套接字通道类来执行异步套接字操作：

```java
java.nio.channels.AsynchronousServerSocketChannel    java.nio.channels.AsynchronousSocketChannel
```

**服务端**

```java
    import java.io.IOException;
    import java.net.InetSocketAddress;
    import java.net.SocketAddress;
    import java.nio.ByteBuffer;
    import java.nio.channels.AsynchronousServerSocketChannel;
    import java.nio.channels.AsynchronousSocketChannel;
    import java.nio.channels.CompletionHandler;
    import java.nio.charset.Charset;
    public class Main {
      public static void main(String[] args) throws Exception {
        AsynchronousServerSocketChannel server = AsynchronousServerSocketChannel
            .open();
        String host = "localhost";
        int port = 8989;
        InetSocketAddress sAddr = new InetSocketAddress(host, port);
        server.bind(sAddr);
        System.out.format("Server is listening at %s%n", sAddr);
        Attachment attach = new Attachment();
        attach.server = server;
        server.accept(attach, new ConnectionHandler());
        Thread.currentThread().join();
      }
    }
    class Attachment {
      AsynchronousServerSocketChannel server;
      AsynchronousSocketChannel client;
      ByteBuffer buffer;
      SocketAddress clientAddr;
      boolean isRead;
    }
    
    class ConnectionHandler implements
        CompletionHandler<AsynchronousSocketChannel, Attachment> {
      @Override
      public void completed(AsynchronousSocketChannel client, Attachment attach) {
        try {
          SocketAddress clientAddr = client.getRemoteAddress();
          System.out.format("Accepted a  connection from  %s%n", clientAddr);
          attach.server.accept(attach, this);
          ReadWriteHandler rwHandler = new ReadWriteHandler();
          Attachment newAttach = new Attachment();
          newAttach.server = attach.server;
          newAttach.client = client;
          newAttach.buffer = ByteBuffer.allocate(2048);
          newAttach.isRead = true;
          newAttach.clientAddr = clientAddr;
          client.read(newAttach.buffer, newAttach, rwHandler);
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
    
      @Override
      public void failed(Throwable e, Attachment attach) {
        System.out.println("Failed to accept a  connection.");
        e.printStackTrace();
      }
    }
    
    class ReadWriteHandler implements CompletionHandler<Integer, Attachment> {
      @Override
      public void completed(Integer result, Attachment attach) {
        if (result == -1) {
          try {
            attach.client.close();
            System.out.format("Stopped   listening to the   client %s%n",
                attach.clientAddr);
          } catch (IOException ex) {
            ex.printStackTrace();
          }
          return;
        }
    
        if (attach.isRead) {
          attach.buffer.flip();
          int limits = attach.buffer.limit();
          byte bytes[] = new byte[limits];
          attach.buffer.get(bytes, 0, limits);
          Charset cs = Charset.forName("UTF-8");
          String msg = new String(bytes, cs);
          System.out.format("Client at  %s  says: %s%n", attach.clientAddr,
              msg);
          attach.isRead = false; // It is a write
          attach.buffer.rewind();
    
        } else {
          // Write to the client
          attach.client.write(attach.buffer, attach, this);
          attach.isRead = true;
          attach.buffer.clear();
          attach.client.read(attach.buffer, attach, this);
        }
      }
    
      @Override
      public void failed(Throwable e, Attachment attach) {
        e.printStackTrace();
      }
    }
```

**客户端**

`AsynchronousSocketChannel`类用作客户端应用程序中的异步客户端套接字通道。

```java
    import java.io.BufferedReader;
    import java.io.InputStreamReader;
    import java.net.InetSocketAddress;
    import java.net.SocketAddress;
    import java.nio.ByteBuffer;
    import java.nio.channels.AsynchronousSocketChannel;
    import java.nio.channels.CompletionHandler;
    import java.nio.charset.Charset;
    import java.util.concurrent.Future;
    
    public class Main {
      public static void main(String[] args) throws Exception {
        AsynchronousSocketChannel channel = AsynchronousSocketChannel.open();
        SocketAddress serverAddr = new InetSocketAddress("localhost", 8989);
        Future<Void> result = channel.connect(serverAddr);
        result.get();
        System.out.println("Connected");
        Attachment attach = new Attachment();
        attach.channel = channel;
        attach.buffer = ByteBuffer.allocate(2048);
        attach.isRead = false;
        attach.mainThread = Thread.currentThread();
    
        Charset cs = Charset.forName("UTF-8");
        String msg = "Hello";
        byte[] data = msg.getBytes(cs);
        attach.buffer.put(data);
        attach.buffer.flip();
    
        ReadWriteHandler readWriteHandler = new ReadWriteHandler();
        channel.write(attach.buffer, attach, readWriteHandler);
        attach.mainThread.join();
      }
    }
    class Attachment {
      AsynchronousSocketChannel channel;
      ByteBuffer buffer;
      Thread mainThread;
      boolean isRead;
    }
    class ReadWriteHandler implements CompletionHandler<Integer, Attachment> {
      @Override
      public void completed(Integer result, Attachment attach) {
        if (attach.isRead) {
          attach.buffer.flip();
          Charset cs = Charset.forName("UTF-8");
          int limits = attach.buffer.limit();
          byte bytes[] = new byte[limits];
          attach.buffer.get(bytes, 0, limits);
          String msg = new String(bytes, cs);
          System.out.format("Server Responded: "+ msg);
          try {
            msg = this.getTextFromUser();
          } catch (Exception e) {
            e.printStackTrace();
          }
          if (msg.equalsIgnoreCase("bye")) {
            attach.mainThread.interrupt();
            return;
          }
          attach.buffer.clear();
          byte[] data = msg.getBytes(cs);
          attach.buffer.put(data);
          attach.buffer.flip();
          attach.isRead = false; // It is a write
          attach.channel.write(attach.buffer, attach, this);
        }else {
          attach.isRead = true;
          attach.buffer.clear();
          attach.channel.read(attach.buffer, attach, this);
        }
      }
      @Override
      public void failed(Throwable e, Attachment attach) {
        e.printStackTrace();
      }
      private String getTextFromUser() throws Exception{
        System.out.print("Please enter a  message  (Bye  to quit):");
        BufferedReader consoleReader = new BufferedReader(
            new InputStreamReader(System.in));
        String msg = consoleReader.readLine();
        return msg;
      }
    }
```

## 数据报套接字

java.nio.channels.DatagramChannel类表示数据报通道。默认情况下，它是阻塞。要使其无阻塞，请使用configureBlocking(false)方法。

要创建`DatagramChannel`，请调用其`open()`静态方法之一。

要将其用于IP多播，请将多播组的地址类型指定为其`open()`方法的参数。

`open()`方法创建一个没有连接的DatagramChannel对象。

**服务端**

```java
    import java.net.InetSocketAddress;
    import java.net.SocketAddress;
    import java.nio.ByteBuffer;
    import java.nio.channels.DatagramChannel;
    
    public class Main {
      public static void main(String[] args) throws Exception {
        DatagramChannel server = null;
        server = DatagramChannel.open();
        InetSocketAddress sAddr = new InetSocketAddress("localhost", 8989);
        server.bind(sAddr);
        ByteBuffer buffer = ByteBuffer.allocate(1024);
    
        while (true) {
          System.out.println("Waiting for a  message  from"
              + "  a  remote  host at " + sAddr);
          SocketAddress remoteAddr = server.receive(buffer);
          buffer.flip();
          int limits = buffer.limit();
          byte bytes[] = new byte[limits];
          buffer.get(bytes, 0, limits);
          String msg = new String(bytes);
    
          System.out.println("Client at " + remoteAddr + "  says: " + msg);
          buffer.rewind();
          server.send(buffer, remoteAddr);
          buffer.clear();
        }
        //server.close();
      }
    }
```

**客户端**

```java
    import java.net.InetSocketAddress;
    import java.nio.ByteBuffer;
    import java.nio.channels.DatagramChannel;
    
    public class Main {
      public static void main(String[] args) throws Exception {
        DatagramChannel client = null;
        client = DatagramChannel.open();
    
        client.bind(null);
    
        String msg = "Hello";
        ByteBuffer buffer = ByteBuffer.wrap(msg.getBytes());
        InetSocketAddress serverAddress = new InetSocketAddress("localhost",
            8989);
    
        client.send(buffer, serverAddress);
        buffer.clear();
        client.receive(buffer);
        buffer.flip();
        int limits = buffer.limit();
        byte bytes[] = new byte[limits];
        buffer.get(bytes, 0, limits);
        String response = new String(bytes);
        System.out.println("Server  responded: " + response);
        client.close();
      }
    }
```

