# 一、Java基础

## 1、Sleep 和 Wait 的区别

- 1、sleep是Thread的静态方法，wait是Object的方法，任何对象实例都能调用。但sleep可以在任意地方进行使用，而wait只能在同步方法或者同步块中使用。

- 2、sleep不会释放锁，它也不需要占用锁，到时间后会自动由操作系统选择执行；wait会释放锁，需要notify/notifyAll后重新获取到对象锁资源后才能继续执行。

- 3、它们都可以被interrupted方法中断。

## 2、强软弱虚引用

Java中是JVM负责内存的分配和回收，故将对象的引用分为：**强引用**、**软引用**、**弱引用**、**虚引用**。

- 强引用：**如果一个对象具有强引用，垃圾回收器就不会回收它，**当内存空间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。

- 软引用：如果内存空间足够，垃圾回收器就不会回收它，如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。**软引用可用来实现内存敏感的高速缓存。**

  - 软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收，JAVA虚拟机就会把这个软引用加入到与之关联的引用队列中。

- 弱引用：只具有弱引用的对象拥有更短暂的生命周期，当GC线程发现了只具有弱引用的对象后，不管内存足够与否，都会回收它的内存。 

  - 弱引用可以和一个引用队列（ReferenceQueue）联合使用，如果弱引用所引用的对象被垃圾回收，Java虚拟机就会把这个弱引用加入到与之关联的引用队列中。

- 虚引用：虚引用并不决定对象的生命周期，如果一个对象仅拥有虚引用，则**和没有任何引用一样，在任何时候都可能被垃圾回收。虚引用主要用来跟踪对象被垃圾回收的活动**。**虚引用必须和引用队列（ReferenceQueue）联合使用**。

  ![image-20211221121323046](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221121323046.png)

## 3、Arrays.sort()方法原理

  原理：在数据量小的时候，各个排序方法的算法时间复杂度在常数项的影响下，性能不同。

- 元素数量<47用插入排序
- 47<=元素数量<=286用快速排序
- 元素数量>=286用归并排序
- <img src="https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1701765-20191126153931611-191817306.png" style="zoom: 67%;" />

## 4、Java创建对象的方式

- 使用new关键字

  ```java
  User user = new User();
  ```

- 使用反射机制

  - 使用Class类的newInstance方法（已过期）

    ```java
    //创建方法1
    User user = (User)Class.forName("根路径.User").newInstance();　
    //创建方法2（用这个最好）
    User user = User.class.newInstance();
    ```

    

  - 使用Constructor类的newInstance方法 

    ```java
    Constructor<User> constructor = User.class.getConstructor();
    User user = constructor.newInstance();
    ```

- 使用clone方法

  - 需要实现Cloneable接口，且属于浅克隆，除非重写clone方法

  ```java
  public class CloneTest implements Cloneable{
      private String name;  
      private int age; 
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public int getAge() {
          return age;
      }
  
      public void setAge(int age) {
          this.age = age;
      }
  
      public CloneTest(String name, int age) {
          super();
          this.name = name;
          this.age = age;
      }
  
      public static void main(String[] args) {
          try {
              CloneTest cloneTest = new CloneTest("wangql",18);
              CloneTest copyClone = (CloneTest) cloneTest.clone();
              System.out.println("newclone:"+cloneTest.getName());
              System.out.println("copyClone:"+copyClone.getName());
          } catch (CloneNotSupportedException e) {
              e.printStackTrace();
          }
      }
  
  }
  ```

- 使用反序列化

  当我们序列化和反序列化一个对象，jvm会给我们创建一个单独的对象。在反序列化时，jvm创建对象并不会调用任何构造函数。

  前提：**类实现Serializable接口**

## 5、解决哈希冲突的方法

- **拉链法**：如HashMap、HashSet都是采用拉链法，即采用链表的数据结构去存取哈希冲突的值，此时插入、查询、删除操作时间复杂度均为O（1），拉链法处理冲突简单、节点动态申请、易删除等。

- **开放定址法**：插入一个元素的时候，先通过哈希函数进行判断，若是发生哈希冲突，就以当前地址为基准，根据再寻址的方法（探查序列），去寻找下一个地址，若发生冲突再去寻找，直至找到一个为空的地址为止。所以这种方法又称为再散列法。
  - 1、线性探查

  - 2、二次探查

  - 3、伪随机探测

- **再哈希法**:使用哈希函数去散列一个输入的时候，输出是同一个位置就再次哈希，直至不发生冲突位置

## 6、Java中的引用传递和值传递

- 基本数据类型传值，对形参的修改不会影响实参；
- 引用类型传引用，形参和实参指向同一个内存地址（同一个对象），所以对参数的修改会影响到实际的对象；
- String, Integer, Double等immutable的类型特殊处理，可以理解为传值，**由于是final类，且私有属性也用final修饰，未开放方法访问，故任何操作均不会修改实参对象**

## 7、== 与 equals的区别

- 对于基本类型来说，==比较的是值，而对于引用类型来说， ==比较的是地址
- equals是Object类的默认方法，不重写前相当于==，并且要求调用方不为null

## 8、重写hashCode与equals

- equals默认是==，满足自反性、对称性、一致性等特性
- hashcode是一个本地方法，返回一个整型数值
- 对于具有相同hashcode的对象来说，其对象不一定相同
- 对于相同的对象来说，其hashcode一定相同
- 对于equals返回true的对象来说，其hashcode一定相同
- 故我们常重写equals与hashcode方法来确保对象的唯一性，其中重写hashcode使得hash过程更效率

## 9、Java中代理模式

### 一、代理模式

代理模式是一种设计模式，提供了对目标对象额外的访问方式，即通过代理对象访问目标对象，从而做到不修改原目标对象的前提下，提供额外的功能操作，扩展目标对象的功能。

### 二、静态代理

需代理对象和目标对象实现一样的接口。
**优点**：可以在不修改目标对象的前提下扩展目标对象的功能。
**缺点**：冗余、不易维护

- 接口类IUserDao

  ```java
  package com.proxy;
  
  public interface IUserDao {
      public void save();
  }
  ```

- 目标对象UserDao

  ```java
  package com.proxy;
  
  public class UserDao implements IUserDao{
  
    @Override
    public void save() {
        System.out.println("保存数据");
    }
  }
  ```

- 静态代理对象UserDapProxy

```java
package com.proxy;

public class UserDaoProxy implements IUserDao{

    private IUserDao target;
    public UserDaoProxy(IUserDao target) {
        this.target = target;
    }
    
    @Override
    public void save() {
        System.out.println("开启事务");//扩展了额外功能
        target.save();
        System.out.println("提交事务");
    }
}

```

### 三、动态代理

动态代理在内存中构建代理对象，从而实现对目标对象的代理功能。动态代理又被称为JDK代理或接口代理。【底层是实现接口的代理类，体现了接口实现的概念】

静态代理与动态代理的区别主要在：

- 静态代理在编译时就已经实现，编译完成后代理类是一个实际的class文件
- 动态代理是在运行时动态生成的，即编译完成后没有实际的class文件，而是在运行时动态生成类字节码，并加载到JVM中

**特点：**动态代理对象不需要实现接口，但是要求目标对象必须实现接口，否则不能使用动态代理。主要涉及的类有**Proxy**、**InvocationHandler**

- 接口类： IUserDao

```java
package com.proxy;

public interface IUserDao {
    public void save();
}
```



- 目标对象：UserDao

```java
package com.proxy;

public class UserDao implements IUserDao{

    @Override
    public void save() {
        System.out.println("保存数据");
    }
}
```

- 动态代理对象：UserProxyFactory

```java
package com.proxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyFactory {

    private Object target;// 维护一个目标对象

    public ProxyFactory(Object target) {
        this.target = target;
    }

    // 为目标对象生成代理对象
    public Object getProxyInstance() {
        return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(),
                new InvocationHandler() {

                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        System.out.println("开启事务");

                        // 执行目标对象方法
                        Object returnValue = method.invoke(target, args);

                        System.out.println("提交事务");
                        return null;
                    }
                });
    }
}
```

### 四、CGLIB代理

CGLIB是一个第三方代码生成类库，运行时在内存中动态生成一个子类对象从而实现对目标对象功能的扩展。常用于许多AOP的框架，是一个强大的高性能代码生成包，底层使用一个小而快的字节码处理框架ASM，来转换字节码并生成新的类。涉及**Enhancer**、**MethodInterceptor**类【底层是目标对象的子类代理，体现了子类继承的概念】

cglib与动态代理最大的**区别**就是

- 使用动态代理的对象必须实现一个或多个接口
- 使用cglib代理的对象则无需实现接口，达到代理类无侵入。

- 目标对象：UserDao

```java
package com.cglib;

public class UserDao{

    public void save() {
        System.out.println("保存数据");
    }
}
```

- 代理对象：ProxyFactory

```java
package com.cglib;

import java.lang.reflect.Method;

import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

public class ProxyFactory implements MethodInterceptor{

    private Object target;//维护一个目标对象
    public ProxyFactory(Object target) {
        this.target = target;
    }
    
    //为目标对象生成代理对象
    public Object getProxyInstance() {
        //工具类
        Enhancer en = new Enhancer();
        //设置父类
        en.setSuperclass(target.getClass());
        //设置回调函数
        en.setCallback(this);
        //创建子类对象代理
        return en.create();
    }

    @Override
    public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
        System.out.println("开启事务");
        // 执行目标对象的方法
        Object returnValue = method.invoke(target, args);
        System.out.println("关闭事务");
        return null;
    }
}
```

## 10、Java序列化

- 序列化含义：将对象写入IO流中
- 反序列化：从IO流中恢复对象
- 意义：序列化机制允许将实现序列化的Java对象转换为字节流，字节流可以存在于磁盘或者网络传输过程中。**所有在网络上传输、需要保存至磁盘的对象都必须是可序列化的，通常建议程序创建的JavaBean对象都需要实现serializeable接口**。
- 对象的类名、实例变量（包括基本类型，数组，对其他对象的引用）都会被序列化；方法、类变量、transient实例变量都不会被序列化。

### 一、实现方式

这个类应该实现**Serializable**接口或者**Externalizable**接口之一。

#### 1、Serializable

Serializable接口是一个标记接口，不用实现任何方法。一旦实现了此接口，该类的对象就是可序列化的。

- 序列化步骤：

- **步骤一：创建一个ObjectOutputStream输出流；**
- **步骤二：调用ObjectOutputStream对象的writeObject输出可序列化对象。**

```java
import java.io.*;

class Person implements Serializable {
    private String name;
    private int age;
    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

public class test{
    public static void main(String[] args) throws IOException {
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("object.txt"));
        Person person = new Person("ryan",10);
        oos.writeObject(person);
    }
}
```

- 反序列化步骤：
- **步骤一：创建一个ObjectInputStream输入流；**
- **步骤二：调用ObjectInputStream对象的readObject()得到序列化的对象。**
- **反序列化并不会调用构造方法。反序列的对象是由JVM自己生成的对象，不通过构造方法生成。**

```java
import java.io.*;

class Person implements Serializable {
    private String name;
    private int age;
    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

public class test{
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        ObjectInputStream oin = new ObjectInputStream(new FileInputStream("object.txt"));
        Person o = (Person) oin.readObject();
    }
}
```

#### 2、成员是引用的序列化

**如果一个可序列化的类的成员不是基本类型，也不是String类型，那这个引用类型也必须是可序列化的；否则，会导致此类不能序列化。**

#### 3、同一对象序列化多次的机制

- **同一对象序列化多次，会将这个对象序列化多次吗？**答案是**否定**的。
- **注意：反序列化的顺序与序列化时的顺序一致**。

#### 4、Java序列化算法

1. 所有保存到磁盘的对象都有一个序列化编码号
2. 当程序试图序列化一个对象时，会先检查此对象是否已经序列化过，只有此对象从未（在此虚拟机）被序列化过，才会将此对象序列化为字节序列输出。
3. 如果此对象已经序列化过，则直接输出编号即可。
4. 如果序列化一个可变对象（对象内的内容可更改）后，更改了对象内容，再次序列化，并不会再次将此对象转换为字节序列，而只是保存序列化编号。

#### 5、可选的序列化

- 使用transient关键字选择不需要序列化的字段。
- 使用transient修饰的属性，java序列化时，会忽略掉此字段，所以反序列化出的对象，被transient修饰的属性是默认值。对于引用类型，值是null；基本类型，值是0；boolean类型，值是false。

#### 6、自定义序列化

java提供了**可选的自定义序列化。**可以进行控制序列化的方式，或者对序列化数据进行编码加密等。

```java
    private void writeObject(java.io.ObjectOutputStream out) throws IOException;
    private void readObject(java.io.ObjectIutputStream in) throws IOException,ClassNotFoundException;
    private void readObjectNoData() throws ObjectStreamException;
```

#### 7、Externalizable：强制自定义序列化

通过实现Externalizable接口，必须实现writeExternal、readExternal方法。

```java
public interface Externalizable extends java.io.Serializable {     
   void writeExternal(ObjectOutput out) throws IOException;     
   void readExternal(ObjectInput in) throws IOException, ClassNotFoundException;
}
```

**注意：Externalizable接口不同于Serializable接口，实现此接口必须实现接口中的两个方法实现自定义序列化，这是强制性的；特别之处是必须提供pulic的无参构造器，因为在反序列化的时候需要反射创建对象。**

| 实现Serializable接口                                         | 实现Externalizable接口   |
| ------------------------------------------------------------ | ------------------------ |
| 系统自动存储必要的信息                                       | 程序员决定存储哪些信息   |
| Java内建支持，易于实现，只需要实现该接口即可，无需任何代码支持 | 必须实现接口内的两个方法 |
| 性能略差                                                     | 性能略好                 |

### 二、序列化版本号serialVersionUID

**反序列化必须拥有class文件**，java序列化提供了一个**private static final long serialVersionUID** 的序列化版本号，只有版本号相同，即使更改了序列化属性，对象也可以正确被反序列化回来。

如果反序列化使用的**class的版本号**与序列化时使用的**不一致**，反序列化会**报InvalidClassException异常。**

序列化版本号可自由指定，如果不指定，JVM会根据类信息自己计算一个版本号，这样随着class的升级，就无法正确反序列化；不指定版本号另一个明显隐患是，不利于jvm间的移植，可能class文件没有更改，但不同jvm可能计算的规则不一样，这样也会导致无法反序列化。

什么情况下需要修改serialVersionUID呢？分三种情况。

- 如果只是修改了方法，反序列化不容影响，则无需修改版本号；
- 如果只是修改了静态变量，瞬态变量（transient修饰的变量），反序列化不受影响，无需修改版本号；
- 如果修改了非瞬态变量，则可能导致反序列化失败。**如果新类中实例变量的类型与序列化时类的类型不一致，则会反序列化失败，这时候需要更改serialVersionUID。**如果只是新增了实例变量，则反序列化回来新增的是默认值；如果减少了实例变量，反序列化时会忽略掉减少的实例变量。

## 11、Java反射

**编译期**：编译期将源代码翻译成机器码

**运行期**：将可执行文件交给操作系统去执行，**Java反射机制就是运行在运行期的；对于任意一个类，都能调用它的任意方法和属性，这种动态获取对象信息和动态调用对象方法的功能称为Java语言的反射机制**

- 当一个类被类加载器加载的时候，JVM便产生了一个Class对象，而Class类对象拥有很多方法。**日常业务开发少用反射，反射需要运行期jvm去重新解析，性能不好**

```java
获取公共构造器 getConstructors()
获取所有构造器 getDeclaredConstructors()
获取包含的方法 getMethod()
获取包含的属性 getField(String name)
获取内部类 getDeclaredClasses()
获取外部类 getDeclaringClass()
获取所实现的接口 getInterfaces()
获取修饰符 getModifiers()
获取所在包 getPackage()
获取类名包含包路径  getName()
类名不包含包路径  getSimpleName()
```

Java反射应用场景：

- 模块化开发（利用反射获取对象）
- 动态代理设计模式
- 框架开发（注解和XML注入）

### JDK动态代理

JDK动态代理是由java内部的反射机制来实现的。**针对于有实现接口的对象。**

**核心类是InvocationHandler接口、Proxy类，其中在invoke()方法中方法增强，在Proxy类的newProxyInstance方法中得到增强后的对象。（需要传入三个参数：类加载器、类实现的接口、代理的目标对象）**

例子：

**接口类与实现类**

```java
public interface OrderService {
    public void save(UUID orderId, String name);

    public void update(UUID orderId, String name);

    public String getByName(String name);
}
```

```java
public class OrderServiceImpl implements OrderService {

    private String user = null;

    public OrderServiceImpl() {
    }

    public OrderServiceImpl(String user) {
        this.setUser(user);
    }

	//...
	
    @Override
    public void save(UUID orderId, String name) {
        System.out.println("call save()方法,save:" + name);
    }

    @Override
    public void update(UUID orderId, String name) {
        System.out.println("call update()方法");
    }

    @Override
    public String getByName(String name) {
        System.out.println("call getByName()方法");
        return "aoho";
    }
}

```

**JDK动态代理类**

```java
public class JDKProxy implements InvocationHandler {
	//需要代理的目标对象
    private Object targetObject;
    
    public Object createProxyInstance(Object targetObject) {
        this.targetObject = targetObject;
        return Proxy.newProxyInstance(this.targetObject.getClass().getClassLoader(),
                this.targetObject.getClass().getInterfaces(), this);
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    	//被代理对象
        OrderServiceImpl bean = (OrderServiceImpl) this.targetObject;
        Object result = null;
        //切面逻辑（advise），此处是在目标类代码执行之前
        System.out.println("---before invoke----");
        if (bean.getUser() != null) {
            result = method.invoke(targetObject, args);
        }
        System.out.println("---after invoke----");
        return result;
    }

	//...

}

```

### CGLIB动态代理

CGLIB既可以对接口的类生成代理，也可以针对类生成代理。

**核心类是MethodInterceptor接口、Enhancer类，其中主要方法是intercept及Enhancer创建增强后的对象，intercept是对方法作增强。而Enhancer需要通过setSuperclass方法设置目标对象父类、回调方法setCallback参数为代理类对象，最后调用create得到增强后对象。**

**目标类**

```java
public class OrderManager {
    private String user = null;

    public OrderManager() {
    }

    public OrderManager(String user) {
        this.setUser(user);
    }

	//...

    public void save(UUID orderId, String name) {
        System.out.println("call save()方法,save:" + name);
    }

    public void update(UUID orderId, String name) {
        System.out.println("call update()方法");
    }

    public String getByName(String name) {
        System.out.println("call getByName()方法");
        return "aoho";
    }
}

```

**CGLIB动态代理类**

```java
public class CGLibProxy implements MethodInterceptor {
    	// CGLib需要代理的目标对象
       private Object targetObject;

       public Object createProxyObject(Object obj) {
        this.targetObject = obj;
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(obj.getClass());
        //回调方法的参数为代理类对象CglibProxy，最后增强目标类调用的是代理类对象CglibProxy中的intercept方法 
        enhancer.setCallback(this);
        //增强后的目标类
        Object proxyObj = enhancer.create();
        // 返回代理对象
        return proxyObj;
    }

    @Override
    public Object intercept(Object proxy, Method method, Object[] args,
                            MethodProxy methodProxy) throws Throwable {
        Object obj = null;
        //切面逻辑（advise），此处是在目标类代码执行之前
        System.out.println("---before intercept----");
        obj = method.invoke(targetObject, args);
        System.out.println("---after intercept----");
        return obj;
    }
}

```



## 12、Object中具有的方法

- Object是所有的Java类继承根源。
- 具有的方法：hashCode()、equals()、clone()、toString()、getClass()、wait()、notify()等。其中getClass()返回一个**Class 对象**

## 13、Java IO中涉及的设计模式

- 装饰者模式：动态地将责任附加到对象上，若要扩展功能，装饰者模提供了比继承更有弹性的替代方案。

- 适配器模式：将一个类的接口，转换成客户期望的另一个接口。适配器让原本接口不兼容的类可以合作无间。

- ```java
  FileInputStream fileInput = new FileInputStream(file); 
  InputStreamReader inputStreamReader = new InputStreamReader(fileInput);
  //将FileInputStream读取一个字节流的方法扩展转换为InputStreamReader读取一个字符流的功能。
  
  ```

  ```java
  BufferedReader bufferedReader=new BufferedReader(inputStreamReader);
  //包装为inputStreamReader后，就有了读取一个字符的功能，在包装为BufferedReader后，就拥有了read一行字符的功能。
  ```

## 14、Socket是用什么网络协议

- Socket代表着网络套接字编程，常用协议即为传输层所使用的协议：TCP/UDP，常用TCP作为可靠连接，如im聊天等；常用UDP作为流传输的实现协议，如视频等；

## 15、Java IO读写

- 无论是Socket的读写还是文件的读写，在Java层面应用开发或者Linux系统底层开发，都属于IO读写
- IO读写两大系统调用：read & write，两者都不负责内核缓冲区和磁盘之间的交换，底层的读写交换由**操作系统kernel**内核负责完成
- **read**：将数据从内核缓存区复制到进程缓存区
- **write**：将数据从进程缓存区复制到内核缓存区
- 在linux系统中，系统内核也有个缓冲区叫做内核缓冲区。每个进程有自己独立的缓冲区，叫做进程缓冲区。

![在这里插入图片描述](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20190105163657587.png)

### 一、四种主要的IO模型

阻塞IO：指的是需要内核IO操作彻底完成后，才返回到用户空间，执行用户的操作。传统的IO模型都是同步阻塞IO。在java中，默认创建的socket都是阻塞的。

非阻塞IO：指的是用户程序不需要等待内核IO操作完成后，内核立即返回给用户一个状态值，用户空间无需等到内核的IO操作彻底完成，可以立即返回用户空间，执行用户的操作，处于非阻塞的状态。

同步IO：**同步与异步区别是一种用户空间与内核空间的调用发起方式。**同步IO是指用户空间线程是主动发起IO请求的一方，内核空间是被动接受方。

异步IO：是指内核kernel是主动发起IO请求的一方，用户线程是被动接受方。



#### 1、同步阻塞IO

![在这里插入图片描述](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20190105163801795.jpg)

- 优点：在阻塞等待期间，用户线程挂起，CPU资源空闲
- 缺点：高并发情况下需要大量的线程来维护连接，并且线程切换开销大

#### 2、**同步非阻塞NIO**

![在这里插入图片描述](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20190105163821398.jpg)

- 优点：发起IO的时候，用户线程不会阻塞，实时性较好
- 缺点：需要不断重复发起IO调用，不断轮询内核消耗大量的CPU时间片，利用率较低，不适用于高并发场景
- **Java中的NIO（New IO）并不是这种**，而是叫做多路复用IO

#### 3、**IO多路复用模型**

![在这里插入图片描述](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20190105163846560.jpg)

- IO多路复用模型：通过一种新的系统调用，一个进程可以监视多个文件描述符，一旦某个描述符就绪（一般是内核缓冲区可读/可写），内核kernel能够通知程序进行相应的IO系统调用。
- 支持IO多路复用的系统调用，有 select、poll、epoll等，将目标提前注册到select/epoll的socket查询列表中，再开启整个模型的读流程。
- 优点：利用一条线程同时监测多个文件描述符，大大减小了系统的开销。
- 缺点：本质上，select/epoll系统调用，属于同步IO，也是阻塞IO。都需要在读写事件就绪后，自己负责进行读写，也就是说这个读写过程是阻塞的。

#### 4、异步IO

![在这里插入图片描述](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20190105163914730.jpg)

- 在内核kernel的等待数据和复制数据的两个阶段，用户线程都不是block(阻塞)的。用户线程需要接受kernel的IO操作完成的事件，或者说注册IO操作完成的回调函数，到操作系统的内核。所以说，异步IO有的时候，也叫做信号驱动 IO 。

### 二、Java NIO

- NIO是基于Channel和Buffer进行操作的，数据总是从通道读取到缓冲区，或者从缓冲区写入到通道中，Selector（选择区）用于监听多个通道的事件，如（连接打开、数据到达），因此单个线程可以监听多个通道。并且NIO是面向缓冲区的，故可以移动读取缓冲区里面的数据。

- Java NIO 比 Java IO快的原因：
  - IO是基于流来读取的，而NIO则是基于块读取的。
  - 非阻塞IO和异步IO以及IO多路复用的支持，减少线程占有的栈空间以及上下文切换。
  - Buffer支持，所有读写操作都是基于缓冲操作的。
  - NIO支持Direct Memory，可以减少一次数据拷贝，并且拥有Netty 零拷贝的支持

#### 1、Channel（通道）

- 不同于IO流，IO流是单向的。而Channel是双向的，既可以用来读数据，也可以用来写数据。NIO里面的Channel里面有：FileChannel、DatagramChannel、SocketChannel、SeverSocketChannel，分别对应于文件IO、UDP、TCP（Sever和Client）



#### 2、Buffer（缓冲区）

- NIO里面的关键Buffer有：ByteBuffer、CharBuffer等，分别对应基本类型：byte、char等，其中也有MappedByteBuffer, HeapByteBuffer, DirectByteBuffer等

- Buffer主要的字段有：mark、limit、position、capacity，方法则有：flip（切换读写模式）、get、compact（拷贝未读数据至起始处）等

  - Buffer的使用：

    - 分配空间：ByteBuffer buf = ByteBuffer.allocate(1024)

    - 写数据：int bytesRead = fileChannel.read(buf)
    - 调用flip()方法
    - 读数据:System.out.print((char)buf.get())
    - 调用clear()或者comapact()方法

  - Buffer中写数据：

    - 从Channel写到Buffer (fileChannel.read(buf))
    - 通过Buffer的put()方法 （buf.put(…)）

  - Buffer中读数据：
    - 从Buffer读取到Channel (channel.write(buf))
    - 使用get()方法从Buffer中读取数据 （buf.get()）

#### 3、Selector（选择器）

- Selector运行单线程可以监听多个Channel，监听多个Selection.KEY事件，

- 一旦向Selector注册了一或多个通道，就可以调用几个重载的select()方法。

  ```java
  int select()  //阻塞，每次返回一个通道就绪
  int select(long timeout) //阻塞特定事件
  int selectNow() //非阻塞，立刻返回
      
  //要获取多个就绪通道
  Set selectedKeys = selector.selectedKeys();
  //注意每次迭代末尾的keyIterator.remove()调用。Selector不会自己从已选择键集中移除SelectionKey实例。必须在处理完通道时自己移除。
  ```

#### 4、内存映射

处理大文件，一般用BufferedReader、BufferedInputStream这种带缓冲的IO类，如果过大，则采用MappedByteBuffer。

MappedByteBuffer是NIO引入的文件内存映射方案，读写性能极高。SocketChannel的读写是通过一个类叫ByteBuffer来操作的。

ByteBuffer有两种模式：直接、间接。间接即是HeapByteBuffer，即操作堆内存 (byte[])。如果文件过大，这时就必须使用"直接"模式，即 MappedByteBuffer，文件映射。

MappedByteBuffer 将文件直接映射到内存（虚拟内存）。通常，可以映射整个文件，如果文件比较大的话可以分段进行映射，只要指定文件的那个部分就可以。

```java
//FileChannel提供了map方法来把文件影射为内存映像文件
MappedByteBuffer map(int mode,long position,long size)
//从position开始的size大小的区域映射为内存映像文件，mode指出了可访问该内存映像文件的方式：

//READ_ONLY,（只读）：试图修改得到的缓冲区将导致抛出 ReadOnlyBufferException.(MapMode.READ_ONLY)

//READ_WRITE（读/写）：对得到的缓冲区的更改最终将传播到文件；该更改对映射到同一文件的其他程序不一定是可见的。 (MapMode.READ_WRITE)

//PRIVATE（专用）： 对得到的缓冲区的更改不会传播到文件，并且该更改对映射到同一文件的其他程序也不是可见的；相反，会创建缓冲区已修改部分的专用副本。(MapMode.PRIVATE)

MappedByteBuffer是ByteBuffer的子类，其扩充了三个方法：

force()：缓冲区是READ_WRITE模式下，此方法对缓冲区内容的修改强行写入文件；

load()：将缓冲区的内容载入内存，并返回该缓冲区的引用；

isLoaded()：如果缓冲区的内容在物理内存中，则返回真，否则返回假；
```

#### 5、其余部分

- **scatter**：Channel将从Channel中读取的数据“分散（scatter）”到多个Buffer中。
- **gather**：Channel 将多个Buffer中的数据“聚集（gather）”后发送到Channel。
- 管道间数据传输：transferFrom & transferTo
- **Pipe**：Java NIO 管道是2个线程之间的单向数据连接。Pipe有一个source通道和一个sink通道。数据会被写到sink通道，从source通道读取。
- **DatagramChannel**：UDP通道，故不像其他通道一样读取和写入，发送和接受的为数据包

#### 6、示例代码

- 客户端：

```java
    public static void client(){
        //创建缓冲区
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        SocketChannel socketChannel = null;
        try
        {
            //打开管道
           socketChannel = SocketChannel.open();
           //设置非阻塞
           socketChannel.configureBlocking(false);
           // 打开TCP连接
           socketChannel.connect(new InetSocketAddress("10.10.195.115",8080));
            if(socketChannel.finishConnect())
            {
                int i=0;
                while(true)
                {
                    TimeUnit.SECONDS.sleep(1);
                    String info = "I'm "+i+++"-th information from client";
                    //清空缓冲区
                    buffer.clear();
                    //写入缓冲区
                    buffer.put(info.getBytes());
                    //切换读模式
                    buffer.flip();
                    while(buffer.hasRemaining()){
                      System.out.println(buffer);
                      //数据写入管道
                      socketChannel.write(buffer);
                    }
                }
            }
        }
        catch (IOException | InterruptedException e)
        {
            e.printStackTrace();
        }
        finally{
            try{
                if(socketChannel!=null){
                    socketChannel.close();
                }
            }catch(IOException e){
                e.printStackTrace();
            }
        }
    }
```

- 服务端：

```java
public class ServerConnect
{
    private static final int BUF_SIZE=1024;
    private static final int PORT = 8080;
    private static final int TIMEOUT = 3000;
    public static void main(String[] args)
    {
        selector();
    }
    //处理连接
    public static void handleAccept(SelectionKey key) throws IOException{
        //打开主管道
        ServerSocketChannel ssChannel = (ServerSocketChannel)key.channel();
        //得到新进来的管道连接
        SocketChannel sc = ssChannel.accept();
        //设置非阻塞
        sc.configureBlocking(false);
        //注册到选择器
        sc.register(key.selector(), SelectionKey.OP_READ,ByteBuffer.allocateDirect(BUF_SIZE));
    }
    //处理可读数据
    public static void handleRead(SelectionKey key) throws IOException{
        //得到相关管道
        SocketChannel sc = (SocketChannel)key.channel();
        ByteBuffer buf = (ByteBuffer)key.attachment();
        long bytesRead = sc.read(buf);
        while(bytesRead>0){
            buf.flip();
            while(buf.hasRemaining()){
                System.out.print((char)buf.get());
            }
            System.out.println();
            buf.clear();
            bytesRead = sc.read(buf);
        }
        if(bytesRead == -1){
            sc.close();
        }
    }
    //处理写数据
    public static void handleWrite(SelectionKey key) throws IOException{
        ByteBuffer buf = (ByteBuffer)key.attachment();
        buf.flip();
        SocketChannel sc = (SocketChannel) key.channel();
        while(buf.hasRemaining()){
            sc.write(buf);
        }
        buf.compact();
    }
    public static void selector() {
        Selector selector = null;
        ServerSocketChannel ssc = null;
        try{
            //打开选择器
            selector = Selector.open();
            ssc= ServerSocketChannel.open();
            ssc.socket().bind(new InetSocketAddress(PORT));
            //设置非阻塞
            ssc.configureBlocking(false);
            //注册选择器
            ssc.register(selector, SelectionKey.OP_ACCEPT);
            while(true){
                if(selector.select(TIMEOUT) == 0){
                    System.out.println("==");
                    continue;
                }
                Iterator<SelectionKey> iter = selector.selectedKeys().iterator();
                while(iter.hasNext()){
                    SelectionKey key = iter.next();
                    if(key.isAcceptable()){
                        handleAccept(key);
                    }
                    if(key.isReadable()){
                        handleRead(key);
                    }
                    if(key.isWritable() && key.isValid()){
                        handleWrite(key);
                    }
                    if(key.isConnectable()){
                        System.out.println("isConnectable = true");
                    }
                    iter.remove();
                }
            }
        }catch(IOException e){
            e.printStackTrace();
        }finally{
            try{
                if(selector!=null){
                    selector.close();
                }
                if(ssc!=null){
                    ssc.close();
                }
            }catch(IOException e){
                e.printStackTrace();
            }
        }
    }
}
```

- Selector可以监听Channel中的四种事件

```java
  Connect
  Accept
  Read
  Write
  
  //分别对应于
  SelectionKey.OP_CONNECT
  SelectionKey.OP_ACCEPT
  SelectionKey.OP_READ
  SelectionKey.OP_WRITE
      
  //通道已经准备就绪的操作集合
  int readySet = selectionKey.readyOps();

  //从SelectionKey访问Channel和Selector很简单
  Channel  channel  = selectionKey.channel();
  Selector selector = selectionKey.selector();

  //可以将对象附着到SelectionKey中，类似于Buffer
  selectionKey.attach(theObject);
  Object attachedObj = selectionKey.attachment();
  SelectionKey key = channel.register(selector, SelectionKey.OP_READ, theObject);

```

## 16、Java常见异常

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/src=http___javaquan.com_file_image_20191114_2019111417000780633931.PNG&refer=http___javaquan.jpg)

- Throwable分为两个重要的子类，即Error和Exception，其中Exception表示可以由程序处理的异常，而Error则是程序无法处理的错误，如Virtual MachineError（虚拟机运行错误）、NoClassDefFoundError（类定义错误）、OutOfMemoryError。
- Exception又可分为受检异常、非受检异常。

### 1、捕获异常

#### 一、try-catch-finally

- try代码块：用于捕获异常。其后可以接零个或者多个catch块。如果没有catch块，后必须跟finally块，来完成资源释放等操作。

- catch代码块：用于捕获异常，并在其中处理异常。

- finally代码块：无论是否捕获异常，finally代码总会被执行。若前面有多个return，最终finally的返回值则会覆盖前面的返回值。

- finally代码块将在方法返回前被执行。注意以下几种情况，finally代码块不会被执行：

  1、 在前边的代码中使用System.exit()退出应用

  2、 程序所在的线程死亡或者cpu关闭

  3、 如果在finally代码块中的操作又产生异常，则该finally代码块不能完全执行结束，同时该异常会覆盖前边抛出的异常

#### 二、throw和throws

- throws：在方法声明中抛出异常，由上层方法进行处理
- throw：方法内抛出异常，常结合try-catch进行异常处理

#### 三、Throwable常用方法

- e.getCause():返回抛出异常的原因。
- e.getMessage():返回异常信息。
- e.printStackTrace():发生异常时，跟踪堆栈信息并输出。

### 2、常见内存异常分析

#### 一、OOM for Heap

分析：heap不满足JVM的需求，可以优化代码对象进行改进，同时也可以调大**-Xmx**参数

#### 二、OOM for StackOverflowError

分析：检查是否存在深度递归，同时使用并发量较大业务的时候，需要优化**-Xss**参数，来优化每个线程所申请的空间

#### 三、OOM for Perm

分析：较多Class或者较多jar包等情况会导致，调高Perm的最大值，即**-XX:MaxPermSize**的值调大

#### 四、OOM for GC

分析：JVM在GC的时候，对象过多，导致内存溢出，可以调整GC的策略，例如**-XX：SurvivorRatio**和**-XX：NewRatio**

#### 五、OOM for native thread created

分析：创建线程过多，在Java里面，虚拟机在JVM内存中创建Thread对象的同时创建操作系统线程，而操作系统线程所用的内存是系统中剩下的内存。（Java里面线程映射是n 对 n 模型）

## 17、JDBC连接过程

- 加载驱动：加载 Driver 类，会加载static初始块，方便后续代码调动方法；
- 创建连接：使用getConnection 方法实现与数据库的连接；
- 创建命令语句：创建命令语句对象，方便下文通过这个对象，调用，命令语句进行执行；
- statement 的 executeupdate 方法是执行修改操作（增删改）的方法；
- 释放资源：将已打开的资源进行释放，因为系统不会自动释放资源，需要自行释放；注意：释放时要按时间逆顺序进行释放。

## 18、statement 和 prestatement 的区别

1、Statement用于执行静态SQL语句，在执行时，必须指定一个事先准备好的SQL语句。 

2、PrepareStatement是预编译的SQL语句对象，sql语句被预编译并保存在对象中。被封装的sql语句代表某一类操作，语句中可以包含动态参数“?”，在执行时可以为“?”动态设置参数值。 

3、使用PrepareStatement对象执行sql时，sql被数据库进行解析和编译，然后被放到命令缓冲区，每当执行同一个PrepareStatement对象时，它就会被解析一次，但不会被再次编译。在缓冲区可以发现预编译的命令，并且可以重用。 

4、PrepareStatement可以减少编译次数提高数据库性能。

## 19、Class.forName和classloader的区别

- java中class.forName()和classLoader都可用来对类进行加载。
- class.forName()除了将类的.class文件加载到jvm中之外，还会对类进行解释，执行类中的static块。
- classLoader将.class文件加载到jvm中，不会执行static中的内容,只有在newInstance才会去执行static块。
- Class.forName(name, initialize, loader)带参函数也可控制是否加载static块。并且只有调用了newInstance()方法采用调用构造函数，创建类的对象。

## 20、ReentrantLock 和 synchronized对比

1. 相比 synchronized ，ReentrantLock可以实现与 synchronized 相同的语义，而且支持以非阻塞方式获取锁，也可以想用中断，限时阻塞，更为灵活。 但是 synchronized 的使用更为简单，代码量也更少。
2. ReentrantLock位于JUC包下，而synchronized是由JVM支持，会进行相关的锁优化。

# 二、Java集合

**使用java中的集合时要自己指定集合的大小，减小后续扩容等操作代价。**

**List(列表)**：

- List的元素以线性方式存储，可以存放重复对象，List主要有以下两个实现类：
  - ArrayList : 长度可变的数组，可随机访问元素，插入与删除速度慢。 JDK8 中ArrayList扩容的实现是通过grow()方法中的语句**newCapacity = oldCapacity + (oldCapacity >> 1)**，然后调用Arrays.copyof()方法进行对原数组进行复制。
  - LinkedList: 采用链表数据结构，插入和删除速度快，但访问速度慢。

**Set(集合)**：

- Set中的对象不按特定(HashCode)的方式排序，且无重复对象，Set主要有以下两个实现类：
  - HashSet： HashSet按照哈希算法来存取集合中的对象，存取速度比较快。当HashSet中的元素个数超过数组大小loadFactor（默认值为0.75）时，就会进行近似两倍扩容（newCapacity = (oldCapacity << 1) + 1）。
  - TreeSet ：TreeSet实现了SortedSet接口，能够对集合中的对象进行排序。


**Map(映射)**：

- Map是一种把键对象和值对象映射的集合，它的每一个元素都包含一个键对象和值对象。 Map主要有以下两个实现类：

  - HashMap：HashMap基于散列表实现，其插入和查询<K,V>的开销是固定的，可以通过构造器设置容量和负载因子来调整容器的性能。
  - LinkedHashMap：类似于HashMap，但是迭代遍历它时，取得<K,V>的顺序是其插入次序，或者是最近最少使用(LRU)的次序。
  - TreeMap：TreeMap基于红黑树实现。查看<K,V>时，它们会被排序。TreeMap是唯一的带有subMap()方法的Map，subMap()可以返回一个子树。

| 比较       | List                                                      | Set                                                      | Map                                                          |
| ---------- | --------------------------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------------ |
| 继承接口   | Collection                                                | Collection                                               |                                                              |
| 常见实现类 | AbstractList(其常用子类有ArrayList、LinkedList、Vector)   | AbstractSet(其常用子类有HashSet、LinkedHashSet、TreeSet) | HashMap、HashTable                                           |
| 常见方法   | add( )、remove( )、clear( )、get( )、contains( )、size( ) | add( )、remove( )、clear( )、contains( )、size( )        | put( )、get( )、remove( )、clear( )、containsKey( )、containsValue( )、keySet( )、values( )、size( ) |
| 元素       | 可重复                                                    | 不可重复(用`equals()`判断)                               | 不可重复                                                     |
| 顺序       | 有序                                                      | 无序(实际上由HashCode决定)，底层由Map实现                | 无序(实际上由HashCode决定)                                   |
| 并发       | Vector线程安全                                            |                                                          | Hashtable线程安全                                            |



## HashMap

### 1、简介

```java
public class HashMap<K,V>
         extends AbstractMap<K,V> 
         implements Map<K,V>, Cloneable, Serializable
```

![image-20211221122259421](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122259421.png)

### 2、数据结构

#### 2.1、主要介绍

![image-20211221122322906](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122322906.png)

![image-20211221122339736](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122339736.png)

#### 2.2、存储过程

![image-20211221122358290](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122358290.png)

#### 2.3、源码

- Node节点

```java
/** 
  * Node  = HashMap的内部类
  **/  

  static class Node<K,V> implements Map.Entry<K,V> {

        final int hash; // 哈希值，HashMap根据该值确定记录的位置
        final K key; // key
        V value; // value
        Node<K,V> next;// 链表下一个节点
  }
```

- 红黑树节点

```java
 /**
  * 红黑树节点 实现类：继承自LinkedHashMap.Entry<K,V>类
  */
  static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {  

  	// 属性 = 父节点、左子树、右子树、删除辅助节点 + 颜色
    TreeNode<K,V> parent;  
    TreeNode<K,V> left;   
    TreeNode<K,V> right;
    TreeNode<K,V> prev;   
    boolean red;   

    // 构造函数
    TreeNode(int hash, K key, V val, Node<K,V> next) {  
        super(hash, key, val, next);  
    }  
  
    // 返回当前节点的根节点  
    final TreeNode<K,V> root() {  
        for (TreeNode<K,V> r = this, p;;) {  
            if ((p = r.parent) == null)  
                return r;  
            r = p;  
        }  
    } 

```

#### 2.4、主要API

```java
V get(Object key); // 获得指定键的值
V put(K key, V value);  // 添加键值对
void putAll(Map<? extends K, ? extends V> m);  // 将指定Map中的键值对 复制到 此Map中
V remove(Object key);  // 删除该键值对

boolean containsKey(Object key); // 判断是否存在该键的键值对；是 则返回true
boolean containsValue(Object value);  // 判断是否存在该值的键值对；是 则返回true
 
Set<K> keySet();  // 单独抽取key序列，将所有key生成一个Set
Collection<V> values();  // 单独value序列，将所有value生成一个Collection

void clear(); // 清除哈希表中的所有键值对
int size();  // 返回哈希表中所有 键值对的数量 = 数组中的键值对 + 链表中的键值对
boolean isEmpty(); // 判断HashMap是否为空；size == 0时 表示为 空 

// 获得key-value的Set集合 再遍历
Set<Map.Entry<String, Integer>> entrySet = map.entrySet();

//获得key的Set集合 再遍历
Set<String> keySet = map.keySet();

//获得value的Set集合
Collection valueSet = map.values();
```

### 3、重要参数

```java
 /** 
   * 主要参数 同  JDK 1.7 
   * 即：容量、加载因子、扩容阈值（要求、范围均相同）
   */

  // 容量（capacity）： 必须是2的幂 & <最大容量（2的30次方）
  static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // 默认容量
  static final int MAXIMUM_CAPACITY = 1 << 30; // 最大容量


  final float loadFactor; // 实际加载因子
  static final float DEFAULT_LOAD_FACTOR = 0.75f; // 默认加载因子 = 0.75

  //当哈希表的大小 ≥ 扩容阈值时， 扩容阈值 = 容量 x 加载因子
  int threshold;


  transient Node<K,V>[] table;  // 存储数据的Node类型 数组，长度 = 2的幂；数组的每个元素 = 1个单链表
  transient int size; // HashMap中存储的键值对的数量
 


   // 链表转成红黑树的阈值，在存储数据时，当链表长度 > 该值时，则将链表转换成红黑树
   static final int TREEIFY_THRESHOLD = 8; 
   //红黑树转为链表的阈值，当在扩容时原有的红黑树内数量 < 6时，则将 红黑树转换成链表
   static final int UNTREEIFY_THRESHOLD = 6;
   //最小树形化容量阈值：即 当哈希表中的容量 > 该值时，才允许树形化链表 （即 将链表 转换成红黑树）
   // 否则，若桶内元素太多时，则直接扩容，而不是树形化
   // 为了避免进行扩容、树形化选择的冲突，这个值不能小于 4 * TREEIFY_THRESHOLD
   static final int MIN_TREEIFY_CAPACITY = 64;
  

```

- **JDK 1.7 和 JDK 1.8 主要区别**

![image-20211221122421329](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122421329.png)



![image-20211221122437786](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122437786.png)

- JDK 1.8的时候hash取值过程，相比于JDK1.7少了多次扰动，仅用1次位运算和1次异或，取了hashcode的高16位和低16位做异或，扰动处理，大大降低了hash冲突，充分利用了hashcode的32位。

![image-20211221122453436](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122453436.png)

- 关于JDK 1.8的时候hash细节，注意**允许key为null，在hash()得到数据位置的时候，null默认数组位置为0**

![image-20211221122509197](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122509197.png)

- **JDK 1.7 和 JDK 1.8扩容区别**

![16217d71025ade97~tplv-t2oaga2asx-watermark](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/16217d71025ade97~tplv-t2oaga2asx-watermark.png)

- **此处主要讲解： `JDK 1.8`扩容时，数据存储位置重新计算的方式**，主要看新增的长度高位和原始hash值对应的位取值确定位置。

![image-20211221122623085](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122623085.png)

- 总的数据插入流程

![16217d711bfb9402~tplv-t2oaga2asx-watermark](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/16217d711bfb9402~tplv-t2oaga2asx-watermark.png)

### 4、JDK 1.7 和 JDK 1.8区别总结

![image-20211221122732399](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122732399.png)

### 5、包装类作为key的原因

![image-20211221122748961](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122748961.png)

### 6、HashMap 中的 `key`若 `Object`类型， 则需实现哪些方法？

![image-20211221122805909](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122805909.png)

### 7、如何保证HashMap线程安全

- 使用Collections.synchronizedMap(Map)创建线程安全的map集合：内部维护了一个**mutex**锁对象，在需要线程安全的方法都有相应的同步代码块保证安全】
- Hashtable：内部操作元素方法用了synchronized修饰
- ConcurrentHashMap：采用了分段锁，即CAS+synchronized保证线程安全

## Hashtable

### **核心成员变量**

```java
public class Hashtable<K,V>
    extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable {
        private transient Entry<?,?>[] table;

    private transient int count;

    private int threshold;

    private float loadFactor;

    private transient int modCount = 0;

    private static final long serialVersionUID = 1421746759512286392L;
}
```

### **构造方法**

若不指定，则默认容量为11，加载因子为0.75

```java
    public Hashtable(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal Load: "+loadFactor);

        if (initialCapacity==0)
            initialCapacity = 1;
        this.loadFactor = loadFactor;
        table = new Entry<?,?>[initialCapacity];
        threshold = (int)Math.min(initialCapacity * loadFactor, MAX_ARRAY_SIZE + 1);
    }


    public Hashtable(int initialCapacity) {
        this(initialCapacity, 0.75f);
    }

    public Hashtable() {
        this(11, 0.75f);
    }
```

- 其余结构基本与HashMap差不多，只不过其没有采用红黑树结构。
- Hashtable 是不允许键或值为 null 的，如果使用null值，就会使得其无法判断对应的key是不存在还是为空，ConcurrentHashMap同理。
- **扩容机制**：当现有容量大于总容量 * 负载因子时，Hashtable 扩容规则为 (oldCapacity << 1) + 1；即2n + 1。
- **迭代器**：Hashtable 的 Enumerator 不是 fail-fast 的，而其 Iterator 则是fail-fast的。

**为何Hashtable的key-value不能为null？**

![image-20211215141842690](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211215141842690.png)

## ConcurrentHashMap

### JDK 1.7

- 主要是数组+链表，由Segment数组、HashEntry组成。

![image-20211221122858416](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122858416.png)

#### **核心成员变量**

```java
      /**
       * Segment 数组，存放数据时首先需要定位到具体的 Segment 中。
       */
      final Segment<K,V>[] segments;
  
      transient Set<K> keySet;
      transient Set<Map.Entry<K,V>> entrySet;
```

- **HashEntry的源代码：**关键属性加了volatile修饰可见性

![image-20211221122920246](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122920246.png)

- **Segment的源代码：继承于ReentrantLock，决定了并发度**

```java
    static final class Segment<K,V> extends ReentrantLock implements Serializable {

        private static final long serialVersionUID = 2249069246763182397L;
        
        // 和 HashMap 中的 HashEntry 作用一样，真正存放数据的桶
        transient volatile HashEntry<K,V>[] table;
        transient int count;
        transient int modCount;
        transient int threshold;
        final float loadFactor;
	}
```

#### **put**

```java
    public V put(K key, V value) {
        Segment<K,V> s;
        if (value == null)
            throw new NullPointerException();
        int hash = hash(key);
        int j = (hash >>> segmentShift) & segmentMask;
        if ((s = (Segment<K,V>)UNSAFE.getObject          // nonvolatile; recheck
             (segments, (j << SSHIFT) + SBASE)) == null) //  in ensureSegment
            s = ensureSegment(j);
        return s.put(key, hash, value, false);
    }

    final V put(K key, int hash, V value, boolean onlyIfAbsent) {
         //尝试自旋获取锁，重试次数过多则改为阻塞锁获取，保证能得到锁
            HashEntry<K,V> node = tryLock() ? null :
                scanAndLockForPut(key, hash, value);
            V oldValue;
            try {
                HashEntry<K,V>[] tab = table;
                //定位到相应的HashEntry插入位置
                int index = (tab.length - 1) & hash;
                HashEntry<K,V> first = entryAt(tab, index);
                //从定位到的位置开始判断，若key相同则覆盖，否则以链表形式插入
                for (HashEntry<K,V> e = first;;) {
                    if (e != null) {
                        K k;
                        if ((k = e.key) == key ||
                            (e.hash == hash && key.equals(k))) {
                            oldValue = e.value;
                            if (!onlyIfAbsent) {
                                e.value = value;
                                ++modCount;
                            }
                            break;
                        }
                        e = e.next;
                    }
                    else {
                        if (node != null)
                            node.setNext(first);
                        else
                            node = new HashEntry<K,V>(hash, key, value, first);
                        int c = count + 1;
                        //插入后判断是否扩容
                        if (c > threshold && tab.length < MAXIMUM_CAPACITY)
                            rehash(node);
                        else
                            setEntryAt(tab, index, node);
                        ++modCount;
                        count = c;
                        oldValue = null;
                        break;
                    }
                }
            } finally {
                //解除阻塞锁
                unlock();
            }
            return oldValue;
        }

```

1. 先通过key定位到 Segment，尝试自旋获取锁，尝试多次失败后改为阻塞锁获取
2. 通过 key 的 hashcode 定位到 HashEntry，若HashEnrty为空，则新建节点插入，否则以链表形式遍历插入，key相等则覆盖

3. 插入后判断是否需要扩容，解除阻塞锁




#### get

只需要将 Key 通过 Hash 之后定位到具体的 Segment ，再通过一次 Hash 定位到具体的元素上。

由于 HashEntry 中的 value 属性是用 volatile 关键词修饰的，保证了内存可见性，所以每次获取时都是最新值。

ConcurrentHashMap 的 get 方法是非常高效的，**因为整个过程都不需要加锁**。




### JDK 1.8

- JDK 1.8的时候抛弃了Segment的分段锁，而采用了**CAS+synchronized**保证并发安全性，并且将HashEntry改为了Node，类似于HashMap，引入了红黑树，链表大于8的时候会转换为红黑树，仍采用**volatile**来保证可见性

#### 核心成员变量

- 扩容的加载因子为**常量0.75**
- hash值的状态：**MOVED（正在扩容）、TREEBIN（树的根节点）、RESERVED（正在序列化）、HASH_BITS（正常状态）**
- **sizeCtl状态值**：
  - -1：表示正有线程进行初始化操作    
  - -(1 + nThreads)：表示有n个线程在扩容
  - 0：默认值，后续真正初始化采用默认容量 
  - 大于0：初始化或扩容完成后的下一次扩容门槛

```java
    private static final int DEFAULT_CAPACITY = 16; // 默认容量
    private static final float LOAD_FACTOR = 0.75f; // 默认加载因子
    static final int TREEIFY_THRESHOLD = 8; // 树化长度阈值
    static final int UNTREEIFY_THRESHOLD = 6; // 非树化长度阈值
    static final int MIN_TREEIFY_CAPACITY = 64; // 最小树化容量阈值

    static final int MOVED     = -1; // hash for forwarding nodes
    static final int TREEBIN   = -2; // hash for roots of trees
    static final int RESERVED  = -3; // hash for transient reservations
    static final int HASH_BITS = 0x7fffffff; // usable bits of normal node hash

    // 状态值： -1：表示正有线程进行初始化操作    -(1 + nThreads)：表示有n个线程在扩容
    //     0：默认值，后续真正初始化采用默认容量  >0：初始化或扩容完成后的下一次扩容门槛
    private transient volatile int sizeCtl;
    transient volatile Node<K,V>[] table;
    // 当扩容时候非空
    private transient volatile Node<K,V>[] nextTable;

```

#### **构造方法**

- 可以手动设置并发度及初始容量

```java
    public ConcurrentHashMap() {
    }

    public ConcurrentHashMap(int initialCapacity) {
        this(initialCapacity, LOAD_FACTOR, 1);
    }

    public ConcurrentHashMap(Map<? extends K, ? extends V> m) {
        this.sizeCtl = DEFAULT_CAPACITY;
        putAll(m);
    }

    public ConcurrentHashMap(int initialCapacity, float loadFactor) {
        this(initialCapacity, loadFactor, 1);
    }

    //此构造方法运算后sizeCtl状态值为扩容阈值
    public ConcurrentHashMap(int initialCapacity,
                             float loadFactor, int concurrencyLevel) {
        if (!(loadFactor > 0.0f) || initialCapacity < 0 || concurrencyLevel <= 0)
            throw new IllegalArgumentException();
        if (initialCapacity < concurrencyLevel)   // Use at least as many bins
            initialCapacity = concurrencyLevel;   // as estimated threads
        //运算出size后，结果为大于size的最小n的2次方
        long size = (long)(1.0 + (long)initialCapacity / loadFactor);
        int cap = (size >= (long)MAXIMUM_CAPACITY) ?
            MAXIMUM_CAPACITY : tableSizeFor((int)size);
        this.sizeCtl = cap;
    }
```

#### **put**

```java
    public V put(K key, V value) {
        return putVal(key, value, false);
    }

    /** Implementation for put and putIfAbsent */
    final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) throw new NullPointerException();
        //三次扰动，包括了hashcode右移16位后与本身异或，以及与HASH_BITS位与
        int hash = spread(key.hashCode());
        // 要插入的元素所在桶的元素个数
        int binCount = 0;
        // 死循环，结合CAS使用（如果CAS失败，则会重新取整个桶进行下面的流程）
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh; K fk; V fv;
            if (tab == null || (n = tab.length) == 0)
                // 如果桶未初始化或者桶个数为0，则初始化桶
                tab = initTable();
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                // 如果桶初始化后无元素，则用CAS插入元素，成功则跳出循环
                if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value)))
                    break;                 	
            }
           // 若要插入的元素所在的桶的第一个元素的hash是MOVED，则当前线程帮忙一起迁移元素
            else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            // 查找第一个节点是否等于要插入的元素
            else if (onlyIfAbsent // check first node without acquiring lock
                     && fh == hash
                     && ((fk = f.key) == key || (fk != null && key.equals(fk)))
                     && (fv = f.val) != null)
                return fv;
                // 如果这个桶不为空且不在迁移元素，则锁住这个桶（分段锁）
                // 并查找要插入的元素是否在这个桶中
                // 存在，则替换值（onlyIfAbsent=false）
                // 不存在，则插入到链表结尾或插入树中
            else {
                V oldVal = null;
                synchronized (f) {
                    // 再次检测第一个元素是否有变化，如有则进入下一次循环，从头来过
                    if (tabAt(tab, i) == f) {
                        // 如果第一个元素的hash值大于等于0（说明不是在迁移，也不是树）
                        // 那就是桶中的元素使用的是链表方式存储
                        if (fh >= 0) {
                            // 桶中元素个数赋值为1
                            binCount = 1;
                            for (Node<K,V> e = f;; ++binCount) {
                                K ek;
                                if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    if (!onlyIfAbsent)
                                        e.val = value;
                                    break;
                                }
                                Node<K,V> pred = e;
                                if ((e = e.next) == null) {
                                    pred.next = new Node<K,V>(hash, key, value);
                                    break;
                                }
                            }
                        }
                        else if (f instanceof TreeBin) {
                            // 如果第一个元素是树节点
                            Node<K,V> p;
                            // 桶中元素个数赋值为2
                            binCount = 2;
                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                           value)) != null) {
                                oldVal = p.val;
                                if (!onlyIfAbsent)
                                    p.val = value;
                            }
                        }
                        else if (f instanceof ReservationNode)
                            throw new IllegalStateException("Recursive update");
                    }
                }
                // 如果binCount不为0，说明成功插入了元素或者寻找到了元素
                if (binCount != 0) {
                    // 如果链表元素个数达到了8，则尝试树化
                    if (binCount >= TREEIFY_THRESHOLD)
                        treeifyBin(tab, i);
                    if (oldVal != null)
                        return oldVal;
                    break;
                }
            }
        }
        // 成功插入元素，元素个数加1（是否要扩容在这个里面）
        addCount(1L, binCount);
        return null;
    }


    
```

整体流程：

1. 判断桶数组是否未初始化，未则初始化
2. 判断桶数组是否为空，则尝试CAS写入元素
3. 判断桶数组的第一个元素hash值，若为MOVED，则表明正扩容，线程进入协助扩容
4. 判断桶的第一个元素桶的第一个节点是否为要插入的节点，是则返回其节点
5. 若所在桶不为空且不在扩容状态，用synchronized锁住当前桶
6. 如果当前桶中元素以链表方式存储，则在链表中寻找该元素或者插入元素
7. 如果当前桶中元素以红黑树方式存储，则在红黑树中寻找该元素或者插入元素
8. 在上述过程中，元素存在则返回旧值
9. 如果元素不存在，以链表结构的需判断是否树化，最终元素个数加1，再判断是否扩容

#### **initTable**

扩容阈值在此处计算。

```java
    //初始化桶数组
    private final Node<K,V>[] initTable() {
        Node<K,V>[] tab; int sc;
        while ((tab = table) == null || tab.length == 0) {
            if ((sc = sizeCtl) < 0)
                // 如果sizeCtl<0说明正在初始化或者扩容，让出CPU
                Thread.yield(); // lost initialization race; just spin
            else if (U.compareAndSwapInt(this, SIZECTL, sc, -1)) {
                // 如果CAS把sizeCtl原子更新为-1成功，则当前线程进入初始化
                // 如果原子更新失败则说明有其它线程先一步进入初始化了，则进入下一次循环
                try {
                    // 再次检查table是否为空，防止ABA问题
                    if ((tab = table) == null || tab.length == 0) {
                        // 如果sc为0则使用默认值16
                        int n = (sc > 0) ? sc : DEFAULT_CAPACITY;
                        // 新建数组
                        @SuppressWarnings("unchecked")
                        Node<K,V>[] nt = (Node<K,V>[])new Node<?,?>[n];
                        // 赋值给table桶数组
                        table = tab = nt;
                        // 设置sc为数组长度的0.75倍（扩容阈值写死）
                        // n - (n >>> 2) = n - n/4 = 0.75n
                        sc = n - (n >>> 2);
                    }
                } finally {
                    // 把sc赋值给sizeCtl，这时存储的是扩容门槛
                    sizeCtl = sc;
                }
                break;
            }
        }
        return tab;
    }
```

#### **remove**

```java
   public V remove(Object key) {
        // 调用替换节点方法
        return replaceNode(key, null, null);
    }
    
    final V replaceNode(Object key, V value, Object cv) {
        // 计算hash
        int hash = spread(key.hashCode());
        // 自旋
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh;
            if (tab == null || (n = tab.length) == 0 ||
                    (f = tabAt(tab, i = (n - 1) & hash)) == null)
                // 如果目标key所在的桶不存在，跳出循环返回null
                break;
            else if ((fh = f.hash) == MOVED)
                // 如果正在扩容中，协助扩容
                tab = helpTransfer(tab, f);
            else {
                V oldVal = null;
                // 标记是否处理过
                boolean validated = false;
                synchronized (f) {
                    // 再次验证当前桶第一个元素是否被修改过
                    if (tabAt(tab, i) == f) {
                        if (fh >= 0) {
                            // fh>=0表示是链表节点
                            validated = true;
                            // 遍历链表寻找目标节点
                            for (Node<K,V> e = f, pred = null;;) {
                                K ek;
                                if (e.hash == hash &&
                                        ((ek = e.key) == key ||
                                                (ek != null && key.equals(ek)))) {
                                    // 找到了目标节点
                                    V ev = e.val;
                                    // 检查目标节点旧value是否等于cv
                                    if (cv == null || cv == ev ||
                                            (ev != null && cv.equals(ev))) {
                                        oldVal = ev;
                                        if (value != null)
                                            // 如果value不为空则替换旧值
                                            e.val = value;
                                        else if (pred != null)
                                            // 如果前置节点不为空
                                            // 删除当前节点
                                            pred.next = e.next;
                                        else
                                            // 如果前置节点为空
                                            // 说明是桶中第一个元素，删除之
                                            setTabAt(tab, i, e.next);
                                    }
                                    break;
                                }
                                pred = e;
                                // 遍历到链表尾部还没找到元素，跳出循环
                                if ((e = e.next) == null)
                                    break;
                            }
                        }
                        else if (f instanceof TreeBin) {
                            // 如果是树节点
                            validated = true;
                            TreeBin<K,V> t = (TreeBin<K,V>)f;
                            TreeNode<K,V> r, p;
                            // 遍历树找到了目标节点
                            if ((r = t.root) != null &&
                                    (p = r.findTreeNode(hash, key, null)) != null) {
                                V pv = p.val;
                                // 检查目标节点旧value是否等于cv
                                if (cv == null || cv == pv ||
                                        (pv != null && cv.equals(pv))) {
                                    oldVal = pv;
                                    if (value != null)
                                        // 如果value不为空则替换旧值
                                        p.val = value;
                                    else if (t.removeTreeNode(p))
                                        // 如果value为空则删除元素
                                        // 如果删除后树的元素个数较少则退化成链表
                                        // t.removeTreeNode(p)这个方法返回true表示删除节点后树的元素个数较少
                                        setTabAt(tab, i, untreeify(t.first));
                                }
                            }
                        }
                    }
                }
                // 如果处理过，不管有没有找到元素都返回
                if (validated) {
                    // 如果找到了元素，返回其旧值
                    if (oldVal != null) {
                        // 如果要替换的值为空，元素个数减1
                        if (value == null)
                            addCount(-1L, -1);
                        return oldVal;
                    }
                    break;
                }
            }
        }
        // 没找到元素返回空
        return null;
    }
    
```

#### get

获得hash值，然后判断存在与否，遍历即可，注意get没有任何锁操作

```java
    public V get(Object key) {
        Node<K,V>[] tab; Node<K,V> e, p; int n, eh; K ek;
        int h = spread(key.hashCode());
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (e = tabAt(tab, (n - 1) & h)) != null) {
            if ((eh = e.hash) == h) {
                if ((ek = e.key) == key || (ek != null && key.equals(ek)))
                    return e.val;
            }
            else if (eh < 0)   // 是个TreeBin hash = -2 
                return (p = e.find(h, key)) != null ? p.val : null;
            while ((e = e.next) != null) { // 对于结点hash值大于0的情况链表
                if (e.hash == h &&
                    ((ek = e.key) == key || (ek != null && key.equals(ek))))
                    return e.val;
            }
        }
        return null;
    }
```

**addCount方法**

主要就2件事：一是更新 baseCount，二是判断是否需要扩容。

```java
    private final void addCount(long x, int check) {
        CounterCell[] as; long b, s;
        // 思想跟LongAdder类是一样
        // 把数组的大小存储根据不同的线程存储到不同的段上（分段锁的思想）
        // 并且有一个baseCount，优先更新baseCount，如果失败了再更新不同线程对应的段
        // 这样可以保证尽量小的减少冲突
    
        // 先尝试把数量加到baseCount上，如果失败再加到分段的CounterCell上
        if ((as = counterCells) != null ||
                !U.compareAndSwapLong(this, BASECOUNT, b = baseCount, s = b + x)) {
            CounterCell a; long v; int m;
            boolean uncontended = true;
            // 如果as为空
            // 或者长度为0
            // 或者当前线程所在的段为null
            // 或者在当前线程的段上加数量失败
            if (as == null || (m = as.length - 1) < 0 ||
                    (a = as[ThreadLocalRandom.getProbe() & m]) == null ||
                    !(uncontended =
                            U.compareAndSwapLong(a, CELLVALUE, v = a.value, v + x))) {
                // 强制增加数量（无论如何数量是一定要加上的，并不是简单地自旋）
                // 不同线程对应不同的段都更新失败了
                // 说明已经发生冲突了，那么就对counterCells进行扩容
                // 以减少多个线程hash到同一个段的概率
                fullAddCount(x, uncontended);
                return;
            }
            if (check <= 1)
                return;
            // 计算元素个数
            s = sumCount();
        }
        if (check >= 0) {
            Node<K,V>[] tab, nt; int n, sc;
            // 如果元素个数达到了扩容门槛，则进行扩容
            // 注意，正常情况下sizeCtl存储的是扩容门槛，即容量的0.75倍
            while (s >= (long)(sc = sizeCtl) && (tab = table) != null &&
                    (n = tab.length) < MAXIMUM_CAPACITY) {
                // rs是扩容时的一个邮戳标识
                int rs = resizeStamp(n);
                if (sc < 0) {
                    // sc<0说明正在扩容中
                    if ((sc >>> RESIZE_STAMP_SHIFT) != rs || sc == rs + 1 ||
                            sc == rs + MAX_RESIZERS || (nt = nextTable) == null ||
                            transferIndex <= 0)
                        // 扩容已经完成了，退出循环
                        // 正常应该只会触发nextTable==null这个条件，其它条件没看出来何时触发
                        break;
    
                    // 扩容未完成，则当前线程加入迁移元素中
                    // 并把扩容线程数加1
                    if (U.compareAndSwapInt(this, SIZECTL, sc, sc + 1))
                        transfer(tab, nt);
                }
                else if (U.compareAndSwapInt(this, SIZECTL, sc,
                        (rs << RESIZE_STAMP_SHIFT) + 2))
                    // 这里是触发扩容的那个线程进入的地方
                    // sizeCtl的高16位存储着rs这个扩容邮戳
                    // sizeCtl的低16位存储着扩容线程数加1，即(1+nThreads)
    
                    // 进入迁移元素
                    transfer(tab, null);
                // 重新计算元素个数
                s = sumCount();
            }
        }
    }
    
```

1. 元素个数的存储方式类似于LongAdder类，存储在不同的段上，减少不同线程同时更新size时的冲突；

2. 计算元素个数时把这些段的值及baseCount相加算出总的元素个数；

3. 正常情况下sizeCtl存储着扩容门槛，扩容门槛为容量的0.75倍；

4. 扩容时sizeCtl高位存储扩容邮戳(resizeStamp)，低位存储扩容线程数加1（1+nThreads）；

5. 其它线程添加元素后如果发现存在扩容，也会加入的扩容行列中来；

#### helpTransfer

线程添加元素时发现正在扩容且当前元素所在的桶元素已经迁移完成了，则协助迁移其它桶的元素。

```java
    final Node<K,V>[] helpTransfer(Node<K,V>[] tab, Node<K,V> f) {
        Node<K,V>[] nextTab; int sc;
        // 如果桶数组不为空，并且当前桶第一个元素为ForwardingNode类型，并且nextTab不为空
        // 说明当前桶已经迁移完毕了，才去帮忙迁移其它桶的元素
        // 扩容时会把旧桶的第一个元素置为ForwardingNode，并让其nextTab指向新桶数组
        if (tab != null && (f instanceof ForwardingNode) &&
                (nextTab = ((ForwardingNode<K,V>)f).nextTable) != null) {
            int rs = resizeStamp(tab.length);
            // sizeCtl<0，说明正在扩容
            while (nextTab == nextTable && table == tab &&
                    (sc = sizeCtl) < 0) {
                if ((sc >>> RESIZE_STAMP_SHIFT) != rs || sc == rs + 1 ||
                        sc == rs + MAX_RESIZERS || transferIndex <= 0)
                    break;
                // 扩容线程数加1
                if (U.compareAndSwapInt(this, SIZECTL, sc, sc + 1)) {
                    // 当前线程帮忙迁移元素
                    transfer(tab, nextTab);
                    break;
                }
            }
            return nextTab;
        }
        return table;
    }
```

#### transfer

1. 新桶数组大小是旧桶数组的两倍；迁移元素先从靠后的桶开始；

2. 迁移完成的桶在里面放置一ForwardingNode类型的元素，标记该桶迁移完成；
3. 遇到已完成迁移的桶则跳过，继续下一个桶，迁移元素时会锁住当前桶，也是分段锁的思想；

4. 每个协助扩容的线程均有自己的一个任务区间
5. 扩容完成后则将新表赋值给旧表，完成扩容

```java
    private final void transfer(Node<K,V>[] tab, Node<K,V>[] nextTab) {
        int n = tab.length, stride;
        if ((stride = (NCPU > 1) ? (n >>> 3) / NCPU : n) < MIN_TRANSFER_STRIDE)
            stride = MIN_TRANSFER_STRIDE; // subdivide range
        if (nextTab == null) {            // nextTab为空，说明还未迁移，新建数组
            try {
                @SuppressWarnings("unchecked")
                Node<K,V>[] nt = (Node<K,V>[])new Node<?,?>[n << 1]; //大小为原先两倍
                nextTab = nt;
            } catch (Throwable ex) {      // try to cope with OOME
                sizeCtl = Integer.MAX_VALUE;
                return;
            }
            nextTable = nextTab;
            transferIndex = n;
        }
        int nextn = nextTab.length;
        // 新建一个ForwardingNode类型的节点，并把新桶数组存储在里面
        ForwardingNode<K,V> fwd = new ForwardingNode<K,V>(nextTab);
        boolean advance = true;
        boolean finishing = false; // to ensure sweep before committing nextTab
        for (int i = 0, bound = 0;;) {
            //领取任务区间
            Node<K,V> f; int fh;
            while (advance) {
                int nextIndex, nextBound;
                if (--i >= bound || finishing)
                    advance = false;
                else if ((nextIndex = transferIndex) <= 0) {
                    i = -1;
                    advance = false;
                }
                //任务取键确认
                else if (U.compareAndSetInt
                         (this, TRANSFERINDEX, nextIndex,
                          nextBound = (nextIndex > stride ?
                                       nextIndex - stride : 0))) {
                    bound = nextBound;
                    i = nextIndex - 1;
                    advance = false;
                }
            }
            //所有线程任务完成
            if (i < 0 || i >= n || i + n >= nextn) {
                int sc;
                if (finishing) {   //直接赋值操作
                    nextTable = null;
                    table = nextTab;
                    sizeCtl = (n << 1) - (n >>> 1); //设置新的扩容量
                    return;
                }
                if (U.compareAndSetInt(this, SIZECTL, sc = sizeCtl, sc - 1)) {
                    if ((sc - 2) != resizeStamp(n) << RESIZE_STAMP_SHIFT)
                        //判断当前完成的线程是不是最后一个扩容的线程
                        return;
                    finishing = advance = true;
                    i = n; // recheck before commit
                }
            }
            else if ((f = tabAt(tab, i)) == null)
                advance = casTabAt(tab, i, null, fwd);
            else if ((fh = f.hash) == MOVED)
                advance = true; // already processed
            else {
                //加锁扩容操作
                synchronized (f) {
                    if (tabAt(tab, i) == f) {
                        Node<K,V> ln, hn;
                        if (fh >= 0) {
                            int runBit = fh & n;
                            Node<K,V> lastRun = f;
                            for (Node<K,V> p = f.next; p != null; p = p.next) {
                                int b = p.hash & n;
                                if (b != runBit) {
                                    runBit = b;
                                    lastRun = p;
                                }
                            }
                            if (runBit == 0) {
                                ln = lastRun;
                                hn = null;
                            }
                            else {
                                hn = lastRun;
                                ln = null;
                            }
                            for (Node<K,V> p = f; p != lastRun; p = p.next) {
                                int ph = p.hash; K pk = p.key; V pv = p.val;
                                if ((ph & n) == 0)
                                    ln = new Node<K,V>(ph, pk, pv, ln);
                                else
                                    hn = new Node<K,V>(ph, pk, pv, hn);
                            }
                            setTabAt(nextTab, i, ln);
                            setTabAt(nextTab, i + n, hn);
                            setTabAt(tab, i, fwd);
                            advance = true;
                        }
                        else if (f instanceof TreeBin) {
                            TreeBin<K,V> t = (TreeBin<K,V>)f;
                            TreeNode<K,V> lo = null, loTail = null;
                            TreeNode<K,V> hi = null, hiTail = null;
                            int lc = 0, hc = 0;
                            for (Node<K,V> e = t.first; e != null; e = e.next) {
                                int h = e.hash;
                                TreeNode<K,V> p = new TreeNode<K,V>
                                    (h, e.key, e.val, null, null);
                                if ((h & n) == 0) {
                                    if ((p.prev = loTail) == null)
                                        lo = p;
                                    else
                                        loTail.next = p;
                                    loTail = p;
                                    ++lc;
                                }
                                else {
                                    if ((p.prev = hiTail) == null)
                                        hi = p;
                                    else
                                        hiTail.next = p;
                                    hiTail = p;
                                    ++hc;
                                }
                            }
                            ln = (lc <= UNTREEIFY_THRESHOLD) ? untreeify(lo) :
                                (hc != 0) ? new TreeBin<K,V>(lo) : t;
                            hn = (hc <= UNTREEIFY_THRESHOLD) ? untreeify(hi) :
                                (lc != 0) ? new TreeBin<K,V>(hi) : t;
                            setTabAt(nextTab, i, ln);
                            setTabAt(nextTab, i + n, hn);
                            setTabAt(tab, i, fwd);
                            advance = true;
                        }
                    }
                }
            }
        }
    }

```

#### sumCount

元素个数的存储也是采用分段的思想，获取元素个数时需要把所有段加起来。

```java
    public int size() {
        // 调用sumCount()计算元素个数
        long n = sumCount();
        return ((n < 0L) ? 0 :
                (n > (long)Integer.MAX_VALUE) ? Integer.MAX_VALUE :
                        (int)n);
    }
    
    final long sumCount() {
        // 计算CounterCell所有段及baseCount的数量之和
        CounterCell[] as = counterCells; CounterCell a;
        long sum = baseCount;
        if (as != null) {
            for (int i = 0; i < as.length; ++i) {
                if ((a = as[i]) != null)
                    sum += a.value;
            }
        }
        return sum;
    }
    
```

**其他API**

```java
    static final int spread(int h) {
        return (h ^ (h >>> 16)) & HASH_BITS;
    }
```

ConcurrentHashMap总结

1. CAS + 自旋，乐观锁的思想，减少线程上下文切换的时间；查询不加锁，添加元素加锁，提升了效率；

2. 分段锁的思想，减少同一把锁争用带来的低效问题；

3. CounterCell，分段存储元素个数，减少多线程同时更新一个字段带来的低效；

4. 多线程协同进行扩容，采用CAS控制sizeCtl状态位进行扩容标志以及扩容阈值，对于桶数组的Hash值有不同的标志含义，便于高效扩容及插入；
5. 是HashMap的线程安全版本，采用了数组 + 链表 + 红黑树的结构，相关关键字用了volatile进行修饰，达到线程可见性；

## LinkedHashMap

LinkedHashMap内部维护了一个双向链表，能保证元素按插入的顺序访问，也能以访问顺序访问，可以用来实现LRU缓存策略。

操作元素的时候需要同时维护在HashMap中的存储，也要维护在LinkedList中的存储，所以性能上来说会比HashMap稍慢。![202105091520406812.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091520406812.png)

### 核心成员变量

```java
public class LinkedHashMap<K,V>
    extends HashMap<K,V>
    implements Map<K,V>{
    
    // 位于LinkedHashMap中的内部类
    static class Entry<K,V> extends HashMap.Node<K,V> {
        Entry<K,V> before, after;
        Entry(int hash, K key, V value, Node<K,V> next) {
            super(hash, key, value, next);
        }
    }
    
    // 双向链表的头节点，旧数据存在头节点
    transient LinkedHashMap.Entry<K,V> head;
    // 双向链表的尾节点，新数据存在尾节点
    transient LinkedHashMap.Entry<K,V> tail;   
    // 是否按访问顺序排序
    final boolean accessOrder;
}
```

### 构造方法

```java
    public LinkedHashMap(int initialCapacity, float loadFactor) {
        super(initialCapacity, loadFactor);
        accessOrder = false;
    }
    
    public LinkedHashMap(int initialCapacity) {
        super(initialCapacity);
        accessOrder = false;
    }
    
    public LinkedHashMap() {
        super();
        accessOrder = false;
    }
    
    public LinkedHashMap(Map<? extends K, ? extends V> m) {
        super();
        accessOrder = false;
        putMapEntries(m, false);
    }
    
    //accessOrder自定义传入，为true则是按访问顺序存储，实现LRU Cache的关键
    public LinkedHashMap(int initialCapacity,
                         float loadFactor,
                         boolean accessOrder) {
        super(initialCapacity, loadFactor);
        this.accessOrder = accessOrder;
    }
```

### afterNodeInsertion

在节点插入后行为表现，在HashMap中的putVal()方法中被调用，但HashMap中这个方法的实现为空。

```java
    void afterNodeInsertion(boolean evict) { // possibly remove eldest
        LinkedHashMap.Entry<K,V> first;
        if (evict && (first = head) != null && removeEldestEntry(first)) {
            K key = first.key;
            removeNode(hash(key), key, null, false, true);
        }
    }
    
    protected boolean removeEldestEntry(Map.Entry<K,V> eldest) {
        return false;
    }
```

evict代表驱逐：

- 若为true，且头节点不为空，且确定移除最老的元素，那么就调用HashMap.removeNode()把头节点移除（指双向链表的头节点）
- 从HashMap中移除头节点后，会调用afterNodeRemoval()方法
- afterNodeRemoval()方法在LinkedHashMap中也有实现，用来在移除元素后修改双向链表
- 默认removeEldestEntry()方法返回false，也就是不删除元素

### afterNodeAccess

在节点访问之后被调用，主要在put()或get()时被调用，如果accessOrder为true，调用这个方法把访问到的节点移动到双向链表的末尾。

```java
    void afterNodeAccess(Node<K,V> e) { // move node to last
        LinkedHashMap.Entry<K,V> last;
        // 如果accessOrder为true，并且访问的节点不是尾节点
        if (accessOrder && (last = tail) != e) {
            LinkedHashMap.Entry<K,V> p =
                    (LinkedHashMap.Entry<K,V>)e, b = p.before, a = p.after;
            // 把p节点从双向链表中移除
            p.after = null;
            if (b == null)
                head = a;
            else
                b.after = a;
    
            if (a != null)
                a.before = b;
            else
                last = b;
    
            // 把p节点放到双向链表的末尾
            if (last == null)
                head = p;
            else {
                p.before = last;
                last.after = p;
            }
            // 尾节点等于p
            tail = p;
            ++modCount;
        }
    }
```

1. 如果accessOrder为true，并且访问的节点不是尾节点
2. 从双向链表中移除访问的节点；
3. 把访问的节点加到双向链表的末尾

### afterNodeRemoval

在HashMap中节点删除后，在双向链表中移除删除节点。

```java
    void afterNodeRemoval(Node<K,V> e) { // unlink
        LinkedHashMap.Entry<K,V> p =
                (LinkedHashMap.Entry<K,V>)e, b = p.before, a = p.after;
        // 把节点p从双向链表中删除。
        p.before = p.after = null;
        if (b == null)
            head = a;
        else
            b.after = a;
        if (a == null)
            tail = b;
        else
            a.before = b;
    }
    
```

### get

得到节点，如果accessOrder为true，则访问后会将节点更新到末尾。

```java
    public V get(Object key) {
        Node<K,V> e;
        if ((e = getNode(hash(key), key)) == null)
            return null;
        if (accessOrder)
            afterNodeAccess(e);
        return e.value;
    }
```

------

总结：

1. 继承自HashMap，具有HashMap的所有特性，重写了部分HashMap保留方法，内部维护了一个双向链表存储所有的元素。
2. 通过accessOrder的值来确定是否按照**访问元素**或**插入元素**顺序遍历元素
3. 默认的LinkedHashMap并不会移除旧元素，如果需要移除旧元素，则需要重写removeEldestEntry()方法设定移除策略；**可实现LRU缓存**

## WeakHashMap

WeakHashMap是一种弱引用map，内部的key会存储为弱引用，当GC时，若key没有强引用存在的话，则会被回收并删除相应的entry，基于这种特性，WeakHashMap特别适用于缓存处理。

### 核心成员变量

WeakHashMap在GC时把没有强引用的key回收掉，所以注定了它里面的元素不会太多，因此，WeakHashMap的存储结构只有（数组 + 链表）。

```java
public class WeakHashMap<K,V>
    extends AbstractMap<K,V>
    implements Map<K,V> {
    
    private static final int DEFAULT_INITIAL_CAPACITY = 16;  // 默认容量
    private static final int MAXIMUM_CAPACITY = 1 << 30; // 最大容量
    private static final float DEFAULT_LOAD_FACTOR = 0.75f; // 加载因子
    Entry<K,V>[] table;  // 存储对象
    private int size;  // 元素个数
    private int threshold; // 扩容阈值
    private final float loadFactor;  // 加载因子
    private final ReferenceQueue<Object> queue = new ReferenceQueue<>(); // 引用队列
}
```

**Entry内部类**

WeakHashMap内部的存储节点, 没有key属性。

```java
    private static class Entry<K,V> extends WeakReference<Object> implements Map.Entry<K,V> {
        // 可以发现没有key, 因为key是作为弱引用存到Referen类中
        V value;
        final int hash;
        Entry<K,V> next;
    
        Entry(Object key, V value,
              ReferenceQueue<Object> queue,
              int hash, Entry<K,V> next) {
            // 调用WeakReference的构造方法初始化key和引用队列
            super(key, queue);
            this.value = value;
            this.hash  = hash;
            this.next  = next;
        }
    }
    
    public class WeakReference<T> extends Reference<T> {
        public WeakReference(T referent, ReferenceQueue<? super T> q) {
            // 调用Reference的构造方法初始化key和引用队列
            super(referent, q);
        }
    }
    
    public abstract class Reference<T> {
        // 实际存储key的地方
        private T referent;         /* Treated specially by GC */
        // 引用队列
        volatile ReferenceQueue<? super T> queue;
    
        Reference(T referent, ReferenceQueue<? super T> queue) {
            this.referent = referent;
            this.queue = (queue == null) ? ReferenceQueue.NULL : queue;
        }
    }
    
```

从Entry的构造方法可知，key和queue最终会传到到Reference的构造方法中，即key会被包装为弱引用，它会被gc特殊对待，即当没有强引用存在时，当下一次gc的时候会被清除。

### 构造方法

```java
   public WeakHashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal Initial Capacity: "+
                    initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
    
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal Load factor: "+
                    loadFactor);
        int capacity = 1;
        while (capacity < initialCapacity)
            capacity <<= 1;
        table = newTable(capacity);
        this.loadFactor = loadFactor;
        threshold = (int)(capacity * loadFactor);
    }
    
    public WeakHashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }
    
    public WeakHashMap() {
        this(DEFAULT_INITIAL_CAPACITY, DEFAULT_LOAD_FACTOR);
    }
    
    public WeakHashMap(Map<? extends K, ? extends V> m) {
        this(Math.max((int) (m.size() / DEFAULT_LOAD_FACTOR) + 1,
                DEFAULT_INITIAL_CAPACITY),
                DEFAULT_LOAD_FACTOR);
        putAll(m);
    }
    
```

### put

```java
    public V put(K key, V value) {
        // 如果key为空，用空对象代替
        Object k = maskNull(key);
        // 计算key的hash值
        int h = hash(k);
        // 获取桶
        Entry<K,V>[] tab = getTable();
        // 计算元素在哪个桶中，h & (length-1)
        int i = indexFor(h, tab.length);
    
        // 遍历桶对应的链表
        for (Entry<K,V> e = tab[i]; e != null; e = e.next) {
            if (h == e.hash && eq(k, e.get())) {
                // 如果找到了元素就使用新值替换旧值，并返回旧值
                V oldValue = e.value;
                if (value != oldValue)
                    e.value = value;
                return oldValue;
            }
        }
    
        modCount++;
        // 如果没找到就把新值插入到链表的头部
        Entry<K,V> e = tab[i];
        tab[i] = new Entry<>(k, value, queue, h, e);
        // 如果插入元素后数量达到了扩容门槛就把桶的数量扩容为2倍大小
        if (++size >= threshold)
            resize(tab.length * 2);
        return null;
    }
    
```

流程：计算hash — 定位到桶—遍历桶相应的链表 —- 若旧值则替换，否则插入到尾部 —- 判断扩容

### resize

```java
    void resize(int newCapacity) {
        // 获取旧桶，getTable()的时候会剔除失效的Entry
        Entry<K,V>[] oldTable = getTable();
        // 旧容量
        int oldCapacity = oldTable.length;
        if (oldCapacity == MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return;
        }
    
        // 新桶
        Entry<K,V>[] newTable = newTable(newCapacity);
        // 把元素从旧桶转移到新桶
        transfer(oldTable, newTable);
        // 把新桶赋值桶变量
        table = newTable;
    
        /*
         * If ignoring null elements and processing ref queue caused massive
         * shrinkage, then restore old table.  This should be rare, but avoids
         * unbounded expansion of garbage-filled tables.
         */
        // 如果元素个数大于扩容门槛的一半，则使用新桶和新容量，并计算新的扩容门槛
        if (size >= threshold / 2) {
            threshold = (int)(newCapacity * loadFactor);
        } else {
            // 否则把元素再转移回旧桶，还是使用旧桶
            // 因为在transfer的时候会清除失效的Entry，所以元素个数可能没有那么大了，就不需要扩容了
            expungeStaleEntries();
            transfer(newTable, oldTable);
            table = oldTable;
        }
    }
    
    private void transfer(Entry<K,V>[] src, Entry<K,V>[] dest) {
        // 遍历旧桶
        for (int j = 0; j < src.length; ++j) {
            Entry<K,V> e = src[j];
            src[j] = null;
            while (e != null) {
                Entry<K,V> next = e.next;
                Object key = e.get();
                // 如果key等于了null就清除，说明key被gc清理掉了，则把整个Entry清除
                if (key == null) {
                    e.next = null;  // Help GC
                    e.value = null; //  "   "
                    size--;
                } else {
                    // 否则就计算在新桶中的位置并把这个元素放在新桶对应链表的头部
                    int i = indexFor(e.hash, dest.length);
                    e.next = dest[i];
                    dest[i] = e;
                }
                e = next;
            }
        }
    }
    
```

流程：判断旧容量是否达到最大容量 — 转移元素到新建的桶 — 转移后达不到一半容量，则转移回旧桶，否则使用新桶，并计算新的扩容门槛 （转移过程中会将key为null的元素清理掉）

### get

```java
    void resize(int newCapacity) {
        // 获取旧桶，getTable()的时候会剔除失效的Entry
        Entry<K,V>[] oldTable = getTable();
        // 旧容量
        int oldCapacity = oldTable.length;
        if (oldCapacity == MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return;
        }
    
        // 新桶
        Entry<K,V>[] newTable = newTable(newCapacity);
        // 把元素从旧桶转移到新桶
        transfer(oldTable, newTable);
        // 把新桶赋值桶变量
        table = newTable;
    

        // 如果元素个数大于扩容门槛的一半，则使用新桶和新容量，并计算新的扩容门槛
        if (size >= threshold / 2) {
            threshold = (int)(newCapacity * loadFactor);
        } else {
            // 否则把元素再转移回旧桶，还是使用旧桶
            // 因为在transfer的时候会清除失效的Entry，所以元素个数可能没有那么大了，就不需要扩容了
            expungeStaleEntries();
            transfer(newTable, oldTable);
            table = oldTable;
        }
    }
    
    private void transfer(Entry<K,V>[] src, Entry<K,V>[] dest) {
        // 遍历旧桶
        for (int j = 0; j < src.length; ++j) {
            Entry<K,V> e = src[j];
            src[j] = null;
            while (e != null) {
                Entry<K,V> next = e.next;
                Object key = e.get();
                // 如果key等于了null就清除，说明key被gc清理掉了，则把整个Entry清除
                if (key == null) {
                    e.next = null;  // Help GC
                    e.value = null; //  "   "
                    size--;
                } else {
                    // 否则就计算在新桶中的位置并把这个元素放在新桶对应链表的头部
                    int i = indexFor(e.hash, dest.length);
                    e.next = dest[i];
                    dest[i] = e;
                }
                e = next;
            }
        }
    }
```

流程：计算hash值 — 遍历桶所在的链表 — 查找元素的value，不存在则返回空

### remove

```java
   public V remove(Object key) {
        Object k = maskNull(key);
        // 计算hash
        int h = hash(k);
        Entry<K,V>[] tab = getTable();
        int i = indexFor(h, tab.length);
        // 元素所在的桶的第一个元素
        Entry<K,V> prev = tab[i];
        Entry<K,V> e = prev;
    
        // 遍历链表
        while (e != null) {
            Entry<K,V> next = e.next;
            if (h == e.hash && eq(k, e.get())) {
                // 如果找到了就删除元素
                modCount++;
                size--;
    
                if (prev == e)
                    // 如果是头节点，就把头节点指向下一个节点
                    tab[i] = next;
                else
                    // 如果不是头节点，删除该节点
                    prev.next = next;
                return e.value;
            }
            prev = e;
            e = next;
        }
    
        return null;
    }
    
```

流程： 流程：计算hash值 — 遍历桶所在的链表 — 查找并删除该节点，返回value，不存在则返回空

### expungeStaleEntries

```java
   private void expungeStaleEntries() {
        // 遍历引用队列
        for (Object x; (x = queue.poll()) != null; ) {
            synchronized (queue) {
                @SuppressWarnings("unchecked")
                Entry<K,V> e = (Entry<K,V>) x;
                int i = indexFor(e.hash, table.length);
                // 找到所在的桶
                Entry<K,V> prev = table[i];
                Entry<K,V> p = prev;
                // 遍历链表
                while (p != null) {
                    Entry<K,V> next = p.next;
                    // 找到该元素
                    if (p == e) {
                        // 删除该元素
                        if (prev == e)
                            table[i] = next;
                        else
                            prev.next = next;
                        // Must not null out e.next;
                        // stale entries may be in use by a HashIterator
                        e.value = null; // Help GC
                        size--;
                        break;
                    }
                    prev = p;
                    p = next;
                }
            }
        }
    }
    
```

- 当key失效的时候gc会自动把对应的Entry添加到这个引用队列中；
- 所有对map的操作都会直接或间接地调用到这个方法先移除失效的Entry，比如getTable()、size()、resize()；
- 这个方法的目的就是遍历引用队列，并把其中保存的Entry从map中移除掉，具体的过程请看类注释；
- 从这里可以看到移除Entry的同时把value也一并置为null帮助gc清理元素，防御性编程。

------

总结：

1. WeakHashMap使用（数组 + 链表）存储结构；

2. WeakHashMap中的key是弱引用，gc的时候会被清除，常用来作为缓存使用；

3. 每次对map的操作都会剔除失效key对应的Entry；

4. 使用包装类作为key的时候，注意要采用new对象形式，避免常量/缓存池造成的强引用

## TreeMap

TreeMap具有存储结构只有一颗红黑树、元素有序，按key的顺序排列、无扩容、效率比HashMap慢、非传统递归式遍历、可按范围查找元素等特性。

![202105091520432302.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091520432302.png)

**红黑树特性**：

- 每个节点非黑即红、根节点是黑色
- 每个叶子节点（NIL）是黑色
- 如果一个节点是红色的，则它的子节点必须是黑色的
- 从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑节点

**右旋**

![202105091520436904.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091520436904.png)

**左旋**

![202105091520433963.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091520433963.png)

### 核心成员变量

```java
public class TreeMap<K,V>
    extends AbstractMap<K,V>
    implements NavigableMap<K,V>, Cloneable, java.io.Serializable
{
   
    private final Comparator<? super K> comparator; // 比较器
    private transient Entry<K,V> root; // 根节点
    private transient int size = 0; // 元素个数
    private transient int modCount = 0; // 结构修改次数
}
```

SortedMap规定了元素可以按key的大小来遍历，它定义了一些返回部分map的方法。

```java
   public interface SortedMap<K,V> extends Map<K,V> {
        // key的比较器
        Comparator<? super K> comparator();
        // 返回fromKey（包含）到toKey（不包含）之间的元素组成的子map
        SortedMap<K,V> subMap(K fromKey, K toKey);
        // 返回小于toKey（不包含）的子map
        SortedMap<K,V> headMap(K toKey);
        // 返回大于等于fromKey（包含）的子map
        SortedMap<K,V> tailMap(K fromKey);
        // 返回最小的key
        K firstKey();
        // 返回最大的key
        K lastKey();
        // 返回key集合
        Set<K> keySet();
        // 返回value集合
        Collection<V> values();
        // 返回节点集合
        Set<Map.Entry<K, V>> entrySet();
    }
```

NavigableMap是对SortedMap的增强，定义了一些返回离目标key最近的元素的方法。

```java
   public interface NavigableMap<K,V> extends SortedMap<K,V> {
        // 小于给定key的最大节点
        Map.Entry<K,V> lowerEntry(K key);
        // 小于给定key的最大key
        K lowerKey(K key);
        // 小于等于给定key的最大节点
        Map.Entry<K,V> floorEntry(K key);
        // 小于等于给定key的最大key
        K floorKey(K key);
        // 大于等于给定key的最小节点
        Map.Entry<K,V> ceilingEntry(K key);
        // 大于等于给定key的最小key
        K ceilingKey(K key);
        // 大于给定key的最小节点
        Map.Entry<K,V> higherEntry(K key);
        // 大于给定key的最小key
        K higherKey(K key);
        // 最小的节点
        Map.Entry<K,V> firstEntry();
        // 最大的节点
        Map.Entry<K,V> lastEntry();
        // 弹出最小的节点
        Map.Entry<K,V> pollFirstEntry();
        // 弹出最大的节点
        Map.Entry<K,V> pollLastEntry();
        // 返回倒序的map
        NavigableMap<K,V> descendingMap();
        // 返回有序的key集合
        NavigableSet<K> navigableKeySet();
        // 返回倒序的key集合
        NavigableSet<K> descendingKeySet();
        // 返回从fromKey到toKey的子map，是否包含起止元素可以自己决定
        NavigableMap<K,V> subMap(K fromKey, boolean fromInclusive,
                                 K toKey,   boolean toInclusive);
        // 返回小于toKey的子map，是否包含toKey自己决定
        NavigableMap<K,V> headMap(K toKey, boolean inclusive);
        // 返回大于fromKey的子map，是否包含fromKey自己决定
        NavigableMap<K,V> tailMap(K fromKey, boolean inclusive);
        // 等价于subMap(fromKey, true, toKey, false)
        SortedMap<K,V> subMap(K fromKey, K toKey);
        // 等价于headMap(toKey, false)
        SortedMap<K,V> headMap(K toKey);
        // 等价于tailMap(fromKey, true)
        SortedMap<K,V> tailMap(K fromKey);
    }
   
```

**Entry内部类**

典型的红黑树结构。

```java
    static final class Entry<K,V> implements Map.Entry<K,V> {
        K key;
        V value;
        Entry<K,V> left;
        Entry<K,V> right;
        Entry<K,V> parent;
        boolean color = BLACK;
    }
    
```

### 构造方法

```java
   /**
     * 默认构造方法，key必须实现Comparable接口
     */
    public TreeMap() {
        comparator = null;
    }
    
    /**
     * 使用传入的comparator比较两个key的大小
     */
    public TreeMap(Comparator<? super K> comparator) {
        this.comparator = comparator;
    }
    
    /**
     * key必须实现Comparable接口，把传入map中的所有元素保存到新的TreeMap中
     */
    public TreeMap(Map<? extends K, ? extends V> m) {
        comparator = null;
        putAll(m);
    }
    
    /**
     * 使用传入map的比较器，并把传入map中的所有元素保存到新的TreeMap中
     */
    public TreeMap(SortedMap<K, ? extends V> m) {
        comparator = m.comparator();
        try {
            buildFromSorted(m.size(), m.entrySet().iterator(), null, null);
        } catch (java.io.IOException cannotHappen) {
        } catch (ClassNotFoundException cannotHappen) {
        }
    }
```

构造方法主要分成两类，一类是使用comparator比较器，一类是key必须实现Comparable接口。

### get

本质上为*二叉*排序树的经典查找方法。

```java
    public V get(Object key) {
        // 根据key查找元素
        Entry<K,V> p = getEntry(key);
        // 找到了返回value值，没找到返回null
        return (p==null ? null : p.value);
    }
    
    final Entry<K,V> getEntry(Object key) {
        // 如果comparator不为空，使用comparator的版本获取元素
        if (comparator != null)
            return getEntryUsingComparator(key);
        // 如果key为空返回空指针异常
        if (key == null)
            throw new NullPointerException();
        // 将key强转为Comparable
        @SuppressWarnings("unchecked")
        Comparable<? super K> k = (Comparable<? super K>) key;
        // 从根元素开始遍历
        Entry<K,V> p = root;
        while (p != null) {
            int cmp = k.compareTo(p.key);
            if (cmp < 0)
                // 如果小于0从左子树查找
                p = p.left;
            else if (cmp > 0)
                // 如果大于0从右子树查找
                p = p.right;
            else
                // 如果相等说明找到了直接返回
                return p;
        }
        // 没找到返回null
        return null;
    }
    
    final Entry<K,V> getEntryUsingComparator(Object key) {
        @SuppressWarnings("unchecked")
        K k = (K) key;
        Comparator<? super K> cpr = comparator;
        if (cpr != null) {
            // 从根元素开始遍历
            Entry<K,V> p = root;
            while (p != null) {
                int cmp = cpr.compare(k, p.key);
                if (cmp < 0)
                    // 如果小于0从左子树查找
                    p = p.left;
                else if (cmp > 0)
                    // 如果大于0从右子树查找
                    p = p.right;
                else
                    // 如果相等说明找到了直接返回
                    return p;
            }
        }
        // 没找到返回null
        return null;
    }
    
```

### put

插入元素，如果元素在树中存在，则替换value；如果元素不存在，则插入到对应的位置，再平衡树。

```java
    public V put(K key, V value) {
        Entry<K,V> t = root;
        if (t == null) {
            // 如果没有根节点，直接插入到根节点
            compare(key, key); // type (and possibly null) check
            root = new Entry<>(key, value, null);
            size = 1;
            modCount++;
            return null;
        }
        // key比较的结果
        int cmp;
        // 用来寻找待插入节点的父节点
        Entry<K,V> parent;
        // 根据是否有comparator使用不同的分支
        Comparator<? super K> cpr = comparator;
        if (cpr != null) {
            // 如果使用的是comparator方式，key值可以为null，只要在comparator.compare()中允许即可
            // 从根节点开始遍历寻找
            do {
                parent = t;
                cmp = cpr.compare(key, t.key);
                if (cmp < 0)
                    // 如果小于0从左子树寻找
                    t = t.left;
                else if (cmp > 0)
                    // 如果大于0从右子树寻找
                    t = t.right;
                else
                    // 如果等于0，说明插入的节点已经存在了，直接更换其value值并返回旧值
                    return t.setValue(value);
            } while (t != null);
        }
        else {
            // 如果使用的是Comparable方式，key不能为null
            if (key == null)
                throw new NullPointerException();
            @SuppressWarnings("unchecked")
            Comparable<? super K> k = (Comparable<? super K>) key;
            // 从根节点开始遍历寻找
            do {
                parent = t;
                cmp = k.compareTo(t.key);
                if (cmp < 0)
                    // 如果小于0从左子树寻找
                    t = t.left;
                else if (cmp > 0)
                    // 如果大于0从右子树寻找
                    t = t.right;
                else
                    // 如果等于0，说明插入的节点已经存在了，直接更换其value值并返回旧值
                    return t.setValue(value);
            } while (t != null);
        }
        // 如果没找到，那么新建一个节点，并插入到树中
        Entry<K,V> e = new Entry<>(key, value, parent);
        if (cmp < 0)
            // 如果小于0插入到左子节点
            parent.left = e;
        else
            // 如果大于0插入到右子节点
            parent.right = e;
    
        // 插入之后的平衡
        fixAfterInsertion(e);
        // 元素个数加1（不需要扩容）
        size++;
        // 修改次数加1
        modCount++;
        // 如果插入了新节点返回空
        return null;
    }
```

采用插入再判断平衡的方式，若需要平衡则按照红黑树特性调整。

### remove

```java
    public V remove(Object key) {
        // 获取节点
        Entry<K,V> p = getEntry(key);
        if (p == null)
            return null;
    
        V oldValue = p.value;
        // 删除节点
        deleteEntry(p);
        // 返回删除的value
        return oldValue;
    }
    
    private void deleteEntry(Entry<K,V> p) {
        // 修改次数加1
        modCount++;
        // 元素个数减1
        size--;
    
        if (p.left != null && p.right != null) {
            // 如果当前节点既有左子节点，又有右子节点
            // 取其右子树中最小的节点
            Entry<K,V> s = successor(p);
            // 用右子树中最小节点的值替换当前节点的值
            p.key = s.key;
            p.value = s.value;
            // 把右子树中最小节点设为当前节点
            p = s;
            // 这种情况实际上并没有删除p节点，而是把p节点的值改了，实际删除的是p的后继节点
        }
    
        // 如果原来的当前节点（p）有2个子节点，则当前节点已经变成原来p的右子树中的最小节点了，也就是说其没有左子节点了
        // 到这一步，p肯定只有一个子节点了
        // 如果当前节点有子节点，则用子节点替换当前节点
        Entry<K,V> replacement = (p.left != null ? p.left : p.right);
    
        if (replacement != null) {
            // 把替换节点直接放到当前节点的位置上（相当于删除了p，并把替换节点移动过来了）
            replacement.parent = p.parent;
            if (p.parent == null)
                root = replacement;
            else if (p == p.parent.left)
                p.parent.left  = replacement;
            else
                p.parent.right = replacement;
    
            // 将p的各项属性都设为空
            p.left = p.right = p.parent = null;
    
            // 如果p是黑节点，则需要再平衡
            if (p.color == BLACK)
                fixAfterDeletion(replacement);
        } else if (p.parent == null) {
            // 如果当前节点就是根节点，则直接将根节点设为空即可
            root = null;
        } else {
            // 如果当前节点没有子节点且其为黑节点，则把自己当作虚拟的替换节点进行再平衡
            if (p.color == BLACK)
                fixAfterDeletion(p);
    
            // 平衡完成后删除当前节点（与父节点断绝关系）
            if (p.parent != null) {
                if (p == p.parent.left)
                    p.parent.left = null;
                else if (p == p.parent.right)
                    p.parent.right = null;
                p.parent = null;
            }
        }
    }
```

采用删除再判断平衡的方式，若有替代元素则替代，再进行平衡，否则以删除位置的元素作为当前节点进行平衡。

### 遍历

```java
    @Override
    public void forEach(BiConsumer<? super K, ? super V> action) {
        Objects.requireNonNull(action);
        // 遍历前的修改次数
        int expectedModCount = modCount;
        // 执行遍历，先获取第一个元素的位置，再循环遍历后继节点
        for (Entry<K, V> e = getFirstEntry(); e != null; e = successor(e)) {
            // 执行动作
            action.accept(e.key, e.value);
    
            // 如果发现修改次数变了，则抛出异常
            if (expectedModCount != modCount) {
                throw new ConcurrentModificationException();
            }
        }
    }

    final Entry<K,V> getFirstEntry() {
            Entry<K,V> p = root;
            // 从根节点开始找最左边的节点，即最小的元素
            if (p != null)
                while (p.left != null)
                    p = p.left;
            return p;
        }

    static <K,V> TreeMap.Entry<K,V> successor(Entry<K,V> t) {
        if (t == null)
            // 如果当前节点为空，返回空
            return null;
        else if (t.right != null) {
            // 如果当前节点有右子树，取右子树中最小的节点
            Entry<K,V> p = t.right;
            while (p.left != null)
                p = p.left;
            return p;
        } else {
            // 如果当前节点没有右子树
            // 如果当前节点是父节点的左子节点，直接返回父节点
            // 如果当前节点是父节点的右子节点，一直往上找，直到找到一个祖先节点是其父节点的左子节点为止，返回这个祖先节点的父节点
            Entry<K,V> p = t.parent;
            Entry<K,V> ch = t;
            while (p != null && ch == p.right) {
                ch = p;
                p = p.parent;
            }
            return p;
        }
    }
```

由源代码可知，其遍历无需消耗空间，本质上因其比传统二叉树多了几个指针，且遍历时间复杂度为O(n)。

## ConcurrentSkipListMap

### 简介

跳表是一个随机化的数据结构，实质就是一种可以进行**二分**查找的**有序链表**。

跳表在原有的有序链表上面增加了多级索引，通过索引来实现快速查找。

跳表不仅能提高搜索性能，同时也可以提高插入和删除操作的性能。

### 存储结构

跳表在原有的有序链表上面增加了多级索引，通过索引来实现快速查找。

![202105091520532201.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091520532201.png)

### 核心成员变量

```java
    final Comparator<? super K> comparator;

    /** Lazily initialized topmost index of the skiplist. */
    private transient Index<K,V> head;
    /** Lazily initialized element count */
    private transient LongAdder adder;
    /** Lazily initialized key set */
    private transient KeySet<K,V> keySet;
    /** Lazily initialized values collection */
    private transient Values<K,V> values;
    /** Lazily initialized entry set */
    private transient EntrySet<K,V> entrySet;
    /** Lazily initialized descending map */
    private transient SubMap<K,V> descendingMap;

    // 数据节点，典型的单链表结构
    static final class Node<K,V> {
        final K key;
        // 注意：这里value的类型是Object，而不是V
        // 在删除元素的时候value会指向当前元素本身
        volatile Object value;
        volatile Node<K,V> next;
    
        Node(K key, Object value, Node<K,V> next) {
            this.key = key;
            this.value = value;
            this.next = next;
        }
    
        Node(Node<K,V> next) {
            this.key = null;
            this.value = this; // 当前元素本身(marker)
            this.next = next;
        }
    }
    
    // 索引节点，存储着对应的node值，及向下和向右的索引指针
    static class Index<K,V> {
        final Node<K,V> node;
        final Index<K,V> down;
        volatile Index<K,V> right;
    
        Index(Node<K,V> node, Index<K,V> down, Index<K,V> right) {
            this.node = node;
            this.down = down;
            this.right = right;
        }
    }
    
    // 头索引节点，继承自Index，并扩展一个level字段，用于记录索引的层级
    static final class HeadIndex<K,V> extends Index<K,V> {
        final int level;
    
        HeadIndex(Node<K,V> node, Index<K,V> down, Index<K,V> right, int level) {
            super(node, down, right);
            this.level = level;
        }
    }
    
```

1. Node，数据节点，存储数据的节点，典型的单链表结构；

2. Index，索引节点，存储着对应的node值，及向下和向右的索引指针；

3. HeadIndex，头索引节点，继承自Index，并扩展一个level字段，用于记录索引的层级；

### 构造方法

```java
  
    public ConcurrentSkipListMap() {
        this.comparator = null;
        initialize();
    }
    
    public ConcurrentSkipListMap(Comparator<? super K> comparator) {
        this.comparator = comparator;
        initialize();
    }
    
    public ConcurrentSkipListMap(Map<? extends K, ? extends V> m) {
        this.comparator = null;
        initialize();
        putAll(m);
    }
    
    public ConcurrentSkipListMap(SortedMap<K, ? extends V> m) {
        this.comparator = m.comparator();
        initialize();
        buildFromSorted(m);
    }
```

四个构造方法里面都调用了initialize()。

```java
    private static final Object BASE_HEADER = new Object();
    
    private void initialize() {
        keySet = null;
        entrySet = null;
        values = null;
        descendingMap = null;
        // Node(K key, Object value, Node<K,V> next)
        // HeadIndex(Node<K,V> node, Index<K,V> down, Index<K,V> right, int level)
        head = new HeadIndex<K,V>(new Node<K,V>(null, BASE_HEADER, null),
                                  null, null, 1);
    }
```

初始化了一些属性，并创建了一个头索引节点，里面存储着一个数据节点，这个数据节点的值是空对象，且它的层级是1。

所以，初始化的时候，跳表中只有一个头索引节点，层级是1，数据节点是一个空对象，down和right都是null。

![202105091520536202.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091520536202.png)

通过内部类的结构我们知道，一个头索引指针包含node, down, right三个指针，为了便于理解，我们把指向node的指针用虚线表示，其它两个用实线表示，也就是虚线不是表明方向的。

### put

```java
  public V put(K key, V value) {
        // 不能存储value为null的元素，否则会被删除
        if (value == null)
            throw new NullPointerException();
    
        // 调用doPut()方法添加元素
        return doPut(key, value, false);
    }
    
    private V doPut(K key, V value, boolean onlyIfAbsent) {
        // 添加元素后存储在z中
        Node<K,V> z;             // added node
        // key也不能为null
        if (key == null)
            throw new NullPointerException();
        Comparator<? super K> cmp = comparator;
    
        // Part I：在数据链路(最底层链表)找到目标节点的位置并插入（本质上为通过二分快速确定插入位置）
        // 自旋直至插入成功
        outer: for (;;) {
            // 通过findPredecessor寻找到邻近的数据节点b，称为当前节点b，且n为下个节点
            for (Node<K,V> b = findPredecessor(key, cmp), n = b.next;;) {
                // 若n非空，比较其与目标节点key，找到目标节点应该插入的位置
                if (n != null) {
                    // v=value，存储节点value值，c=compare，存储两个节点比较的大小结果
                    Object v; int c;
                    // n的下一个数据节点 称为f
                    Node<K,V> f = n.next;
                    // 若n不为b的下一个节点，说明有其它线程修改了数据，则跳出内层循环
                    // 回到外层循环自旋的位置，从头来过
                    if (n != b.next)               // inconsistent read
                        break;
                    // 若n的value值为空，说明该节点已删除，协助删除节点
                    if ((v = n.value) == null) {   // n is deleted
                        // 协助删除
                        n.helpDelete(b, f);
                        break;
                    }
                    // 若b的值为空或者v等于n，说明b已被删除，n就是marker节点，重新循环
                    if (b.value == null || v == n) // b is deleted
                        break;
                    // 若目标key比下一个节点大，则整体后移，再继续循环查找插入位置
                    if ((c = cpr(cmp, key, n.key)) > 0) {
                        b = n;
                        n = f;
                        continue;
                    }
                    // 若存在下一个节点的key与目标key相同
                    if (c == 0) {
                        // 则用新值替换旧值，并返回旧值（onlyIfAbsent=false）
                        if (onlyIfAbsent || n.casValue(v, value)) {
                            @SuppressWarnings("unchecked") V vv = (V)v;
                            return vv;
                        }
                        // 如果替换旧值时失败，说明其它线程先一步修改了值，从头来过
                        break; // restart if lost race to replace value
                    }
                }
    
                // 有两种情况会到这里
                // 一是到链表尾部了，即n为null
                // 二是找到了目标节点的位置，即c<0
    
                // 新建目标节点，并赋值给z，把n作为新节点的next
                z = new Node<K,V>(key, value, n);
                // 原子更新b的下一个节点为目标节点z
                if (!b.casNext(n, z))
                    // 如果更新失败，说明其它线程先一步修改了值，从头来过
                    break;        
                // 如果更新成功，跳出自旋状态
                break outer;
            }
        }
    
        // 经过Part I，目标节点已经插入到有序链表中了
    
        // Part II：随机决定是否需要建立索引及其层次，如果需要则建立自上而下的索引
    
        // 取个随机数
        int rnd = ThreadLocalRandom.nextSecondarySeed();
        if ((rnd & 0x80000001) == 0) { 
            // 默认level为1，也就是只要到这里了就会至少建立一层索引
            int level = 1, max;
            // 随机数从最低位的第二位开始，有几个连续的1则level就加几
            while (((rnd >>>= 1) & 1) != 0)
                ++level;
    
            // 用于记录目标节点建立的最高的那层索引节点
            Index<K,V> idx = null;
            // 取头索引节点（这是最高层的头索引节点）
            HeadIndex<K,V> h = head;
            // 如果生成的层数小于等于当前最高层的层级，即跳表的高度不会超过现有高度
            if (level <= (max = h.level)) {
                // 从第一层开始建立一条竖直的索引链表
                for (int i = 1; i <= level; ++i)
                    idx = new Index<K,V>(z, idx, null);
            }
            else { 
                // 如果新的层数超过了现有跳表的高度，则最多只增加一层
                level = max + 1; 
                // idxs用于存储目标节点建立的竖起索引的所有索引节点
                @SuppressWarnings("unchecked")Index<K,V>[] idxs =
                        (Index<K,V>[])new Index<?,?>[level+1];
                // 从第一层开始建立一条竖的索引链表（跟上面一样，只是这里顺便把索引节点放到数组里面了）
                for (int i = 1; i <= level; ++i)
                    idxs[i] = idx = new Index<K,V>(z, idx, null);
    
                // 自旋
                for (;;) {
                    // 旧的最高层头索引节点
                    h = head;
                    // 旧的最高层级
                    int oldLevel = h.level;
                    // 再次检查，若旧的最高层级已经不比新层级矮了，说明有其它线程先一步修改了值，从头来过
                    if (level <= oldLevel) 
                        break;
                    // 新的最高层头索引节点
                    HeadIndex<K,V> newh = h;
                    // 头节点指向的数据节点
                    Node<K,V> oldbase = h.node;
                    // 超出的部分建立新的头索引节点
                    for (int j = oldLevel+1; j <= level; ++j)
                        newh = new HeadIndex<K,V>(oldbase, newh, idxs[j], j);
                    // 原子更新头索引节点
                    if (casHead(h, newh)) {
                        // h指向新的最高层头索引节点
                        h = newh;
                        idx = idxs[level = oldLevel];
                        break;
                    }
                }
            }
    
            // 经过上面的步骤，有两种情况
            // 一是没有超出高度，新建一条目标节点的索引节点链
            // 二是超出了高度，新建一条目标节点的索引节点链，同时最高层头索引节点同样往上长
    
            // Part III：将新建的索引节点（包含头索引节点）与其它索引节点通过右指针连接在一起
    
            // 这时level是等于旧的最高层级的，自旋
            splice: for (int insertionLevel = level;;) {
                // h为最高头索引节点
                int j = h.level;
    
                // 从头索引节点开始遍历
                // 为了方便，这里叫q为当前节点，r为右节点，d为下节点，t为目标节点相应层级的索引
                for (Index<K,V> q = h, r = q.right, t = idx;;) {
                    // 如果遍历到了最右边，或者最下边，
                    // 也就是遍历到头了，则退出外层循环
                    if (q == null || t == null)
                        break splice;
                    // 如果右节点不为空
                    if (r != null) {
                        // n是右节点的数据节点，为了方便，这里直接叫右节点的值
                        Node<K,V> n = r.node;
                        // 比较目标key与右节点的值
                        int c = cpr(cmp, key, n.key);
                        // 如果右节点的值为空了，则表示此节点已删除
                        if (n.value == null) {
                            // 则把右节点删除
                            if (!q.unlink(r))
                                // 如果删除失败，说明有其它线程先一步修改了，从头来过
                                break;
                            // 删除成功后重新取右节点
                            r = q.right;
                            continue;
                        }
                        // 如果比较c>0，表示目标节点还要往右
                        if (c > 0) {
                            // 则把当前节点和右节点分别右移
                            q = r;
                            r = r.right;
                            continue;
                        }
                    }
    
                    // 到这里说明已经到当前层级的最右边了
                    // 这里实际是会先走第二个if
    
                    // 第一个if
                    // j与insertionLevel相等了
                    // 实际是先走的第二个if，j自减后应该与insertionLevel相等
                    if (j == insertionLevel) {
                        // 这里是真正连右指针的地方
                        if (!q.link(r, t))
                            // 连接失败，从头来过
                            break; // restart
                        // t节点的值为空，可能是其它线程删除了这个元素
                        if (t.node.value == null) {
                            // 这里会去协助删除元素
                            findNode(key);
                            break splice;
                        }
                        // 当前层级右指针连接完毕，向下移一层继续连接
                        // 如果移到了最下面一层，则说明都连接完成了，退出外层循环
                        if (--insertionLevel == 0)
                            break splice;
                    }
    
                    // 第二个if
                    // j先自减1，再与两个level比较
                    // j、insertionLevel和t(idx)三者是对应的，都是还未把右指针连好的那个层级
                    if (--j >= insertionLevel && j < level)
                        // t往下移
                        t = t.down;
    
                    // 当前层级到最右边了
                    // 那只能往下一层级去走了
                    // 当前节点下移
                    // 再取相应的右节点
                    q = q.down;
                    r = q.right;
                }
            }
        }
        return null;
    }
    
    // 寻找目标节点之前最近的一个索引对应的数据节点
    private Node<K,V> findPredecessor(Object key, Comparator<? super K> cmp) {
        // key不能为空
        if (key == null)
            throw new NullPointerException(); // don't postpone errors
        // 自旋
        for (;;) {
            // 从最高层头索引节点开始查找，先向右，再向下
            // 直到找到目标位置之前的那个索引
            for (Index<K,V> q = head, r = q.right, d;;) {
                // 如果右节点不为空
                if (r != null) {
                    // 右节点对应的数据节点，为了方便，我们叫右节点的值
                    Node<K,V> n = r.node;
                    K k = n.key;
                    // 如果右节点的value为空
                    // 说明其它线程把这个节点标记为删除了
                    // 则协助删除
                    if (n.value == null) {
                        if (!q.unlink(r))
                            // 如果删除失败
                            // 说明其它线程先删除了，从头来过
                            break;           // restart
                        // 删除之后重新读取右节点
                        r = q.right;         // reread r
                        continue;
                    }
                    // 如果目标key比右节点还大，继续向右寻找
                    if (cpr(cmp, key, k) > 0) {
                        // 往右移
                        q = r;
                        // 重新取右节点
                        r = r.right;
                        continue;
                    }
                    // 如果c<0，说明不能再往右了
                }
                // 到这里说明当前层级已经到最右了
                // 两种情况：一是r==null，二是c<0
                // 再从下一级开始找
    
                // 如果没有下一级了，就返回这个索引对应的数据节点
                if ((d = q.down) == null)
                    return q.node;
    
                // 往下移
                q = d;
                // 重新取右节点
                r = d.right;
            }
        }
    }
    
    // Node.class中的方法，协助删除元素
    void helpDelete(Node<K,V> b, Node<K,V> f) {
        // 这里的调用者this==n，三者关系是b->n->f
        if (f == next && this == b.next) {
            // 将n的值设置为null后，会先把n的下个节点设置为marker节点
            // 这个marker节点的值是它自己
            // 这里如果不是它自己说明marker失败了，重新marker
            if (f == null || f.value != f) // not already marked
                casNext(f, new Node<K,V>(f));
            else
                // marker过了，就把b的下个节点指向marker的下个节点
                b.casNext(this, f.next);
        }
    }
    
    // Index.class中的方法，删除succ节点
    final boolean unlink(Index<K,V> succ) {
        // 原子更新当前节点指向下一个节点的下一个节点
        // 也就是删除下一个节点
        return node.value != null && casRight(succ, succ.right);
    }
    
    // Index.class中的方法，在当前节点与succ之间插入newSucc节点
    final boolean link(Index<K,V> succ, Index<K,V> newSucc) {
        // 在当前节点与下一个节点中间插入一个节点
        Node<K,V> n = node;
        // 新节点指向当前节点的下一个节点
        newSucc.right = succ;
        // 原子更新当前节点的下一个节点指向新节点
        return n.value != null && casRight(succ, newSucc);
    }
```

总结：

1. 插入目标节点到数据节点链表中，通过findPredecessor从头索引结点开始寻找位置，通过CAS插入到数据节点

2. 建立目标节点竖直的down链表，根据随机数，确定是否要新建层级，若需要新建层级，则同时需要建立新层级的头索引节点

3. 建立横向的right链表，将前一步的down链表与之前的联系起来

![202105091520543685.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091520543685.png)

<img src="https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091520543685.png" alt="202105091520543685.png"  />

![202105091520547156.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091520547156.png)

### remove

```java
   public V remove(Object key) {
        return doRemove(key, null);
    }
    
    final V doRemove(Object key, Object value) {
        // key不为空
        if (key == null)
            throw new NullPointerException();
        Comparator<? super K> cmp = comparator;
        // 自旋
        outer: for (;;) {
            // 寻找目标节点之前的最近的索引节点对应的数据节点
            // 为了方便，这里叫b为当前节点，n为下一个节点，f为下下个节点
            for (Node<K,V> b = findPredecessor(key, cmp), n = b.next;;) {
                Object v; int c;
                // 整个链表都遍历完了也没找到目标节点，退出外层循环
                if (n == null)
                    break outer;
                // 下下个节点
                Node<K,V> f = n.next;
                // 再次检查，若n不是b的下一个节点了，说明有其它线程先一步修改了，从头来过
                if (n != b.next)                   
                    break;
                // 如果下个节点的值奕为null了，说明有其它线程标记该元素为删除状态了
                if ((v = n.value) == null) {    
                    // 协助删除
                    n.helpDelete(b, f);
                    break;
                }
                // 如果b的值为空或者v等于n，说明b已被删除，这时候n就是marker节点
                if (b.value == null || v == n)    
                    break;
                // 如果c<0，说明没找到元素，退出外层循环
                if ((c = cpr(cmp, key, n.key)) < 0)
                    break outer;
                // 如果c>0，说明还没找到，继续向右找
                if (c > 0) {
                    // 当前节点往后移
                    b = n;
                    // 下一个节点往后移
                    n = f;
                    continue;
                }
                // c=0，说明n就是要找的元素
                // 如果value不为空且不等于找到元素的value，不需要删除，退出外层循环
                if (value != null && !value.equals(v))
                    break outer;
                // 如果value为空，或者相等
                // 原子标记n的value值为空
                if (!n.casValue(v, null))
                    // 如果删除失败，说明其它线程先一步修改了，从头来过
                    break;
    
                // P.S.到了这里n的值肯定是设置成null了
    
               ：
                // 一是如果标记market成功，再尝试将b的下个节点指向下下个节点，如果第二步失败了，进入条件，如果成功了就不用进入条件了
                // 二是如果标记market失败了，直接进入条件
                if (!n.appendMarker(f) || !b.casNext(n, f))
                    // 通过findNode()重试删除（里面有个helpDelete()方法）
                    findNode(key);                  // retry via findNode
                else {
                    // 上面两步操作都成功了，才会进入这里，不太好理解，上面两个条件都有非"!"操作
                    // 说明节点已经删除了，通过findPredecessor()方法删除索引节点
                    // findPredecessor()里面有unlink()操作
                    findPredecessor(key, cmp);      // clean index
                    // 如果最高层头索引节点没有右节点，则跳表的高度降级
                    if (head.right == null)
                        tryReduceLevel();
                }
                // 返回删除的元素值
                @SuppressWarnings("unchecked") V vv = (V)v;
                return vv;
            }
        }
        return null;
    }
```

总结：

- 通过findPredecessor从头索引结点开始寻找位置，在数据节点后开始遍历直到找到目标节点位置
- 若没有目标元素，直接返回null，否则先通过`n.casValue(v, null)`原子更新把其value设置为null
- 通过`n.appendMarker(f)`在当前元素添加一个marker元素标记此元素需要删除 || 通过`b.casNext(n, f)`尝试删除元素
- 若两个方法都失败了都通过`findNode(key)`中的`n.helpDelete(b, f)`再去不断尝试删除
- 若两个方法都成功了再通过`findPredecessor(key, cmp)`中的`q.unlink(r)`删除索引节点
- 如果head的right指针指向了null，则跳表高度降级；

![202105091520550757.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091520550757.png)

![202105091520554158.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091520554158.png)

### findPredecessor

```java
 
    // 寻找目标节点之前最近的一个索引对应的数据节点
    private Node<K,V> findPredecessor(Object key, Comparator<? super K> cmp) {
        // key不能为空
        if (key == null)
            throw new NullPointerException(); // don't postpone errors
        // 自旋
        for (;;) {
            // 从最高层头索引节点开始查找，先向右，再向下
            // 直到找到目标位置之前的那个索引
            for (Index<K,V> q = head, r = q.right, d;;) {
                // 如果右节点不为空
                if (r != null) {
                    // 右节点对应的数据节点，为了方便，我们叫右节点的值
                    Node<K,V> n = r.node;
                    K k = n.key;
                    // 如果右节点的value为空
                    // 说明其它线程把这个节点标记为删除了
                    // 则协助删除
                    if (n.value == null) {
                        if (!q.unlink(r))
                            // 如果删除失败
                            // 说明其它线程先删除了，从头来过
                            break;           // restart
                        // 删除之后重新读取右节点
                        r = q.right;         // reread r
                        continue;
                    }
                    // 如果目标key比右节点还大，继续向右寻找
                    if (cpr(cmp, key, k) > 0) {
                        // 往右移
                        q = r;
                        // 重新取右节点
                        r = r.right;
                        continue;
                    }
                    // 如果c<0，说明不能再往右了
                }
                // 到这里说明当前层级已经到最右了
                // 两种情况：一是r==null，二是c<0
                // 再从下一级开始找
    
                // 如果没有下一级了，就返回这个索引对应的数据节点
                if ((d = q.down) == null)
                    return q.node;
    
                // 往下移
                q = d;
                // 重新取右节点
                r = d.right;
            }
        }
    }
```

![202105091520559529.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091520559529.png)

![2021050915205628610.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/2021050915205628610.png)



## HashSet

HashSet不包含重复元素，是Set一种实现方式，底层采用HashMap实现。

#### 核心成员变量

```java
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
{
    // 底层采用HashMap
    private transient HashMap<E,Object> map;

    // 虚拟对象，用来作为map中的value
    private static final Object PRESENT = new Object();
```

#### 构造方法

```java
    public HashSet() {
        map = new HashMap<>();
    }

    public HashSet(Collection<? extends E> c) {
        map = new HashMap<>(Math.max((int) (c.size()/.75f) + 1, 16));
        addAll(c);
    }

    public HashSet(int initialCapacity, float loadFactor) {
        map = new HashMap<>(initialCapacity, loadFactor);
    }

    public HashSet(int initialCapacity) {
        map = new HashMap<>(initialCapacity);
    }

    // 非public，主要是给LinkedHashSet使用的
    HashSet(int initialCapacity, float loadFactor, boolean dummy) {
        map = new LinkedHashMap<>(initialCapacity, loadFactor);
    }
```

#### add

```java
    public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }
```

#### remove

```java
    public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }
```

#### contains

```java
    public boolean contains(Object o) {
        return map.containsKey(o);
    }
```

#### iterator

```java
    public Iterator<E> iterator() {
        return map.keySet().iterator();
    }
```

------

总结：

- HashSet内部使用HashMap的key存储元素，以此来保证元素不重复；
- HashSet是无序的，因为HashMap的key是无序的；
- HashSet中允许有一个null元素，因为HashMap允许key为null；
- HashSet是非线程安全的；
- HashSet是没有get()方法的；

## LinkedHashSet

LinkedHashSet继承自HashSet。

```java
    package java.util;
    
    // LinkedHashSet继承自HashSet
    public class LinkedHashSet<E>
        extends HashSet<E>
        implements Set<E>, Cloneable, java.io.Serializable {
    
        private static final long serialVersionUID = -2851667679971038690L;
    
        // 传入容量和装载因子
        public LinkedHashSet(int initialCapacity, float loadFactor) {
            super(initialCapacity, loadFactor, true);
        }
    
        // 只传入容量, 装载因子默认为0.75
        public LinkedHashSet(int initialCapacity) {
            super(initialCapacity, .75f, true);
        }
    
        // 使用默认容量16, 默认装载因子0.75
        public LinkedHashSet() {
            super(16, .75f, true);
        }
    
        
        public LinkedHashSet(Collection<? extends E> c) {
            super(Math.max(2*c.size(), 11), .75f, true);
            addAll(c);
        }
    
        // 可分割的迭代器, 主要用于多线程并行迭代处理时使用
        @Override
        public Spliterator<E> spliterator() {
            return Spliterators.spliterator(this, Spliterator.DISTINCT | Spliterator.ORDERED);
        }
    }
```

------

总结：

- LinkedHashSet继承自HashSet，而HashSet内部有个构造方法使用了LinkedHashMap，而LinkedHashSet使用了其构造方法，故底层使用LinkedHashMap存储元素
- LinkedHashSet是有序的，由于HashSet构造方法中含有LinkedHashMap的accessOrder固定为false，它是按照插入的顺序排序的。

## TreeSet

TreeSet底层是采用TreeMap实现的一种Set，所以它是有序的，同样也是非线程安全的。

**API**

```java
    package java.util;
    
    // TreeSet实现了NavigableSet接口，所以它是有序的
    public class TreeSet<E> extends AbstractSet<E>
        implements NavigableSet<E>, Cloneable, java.io.Serializable
    {
        // 元素存储在NavigableMap中，TreeMap实现了这个接口，实例化后底层使用的为TreeMap
        private transient NavigableMap<E,Object> m;
    
        // 虚拟元素, 用来作为value存储在map中
        private static final Object PRESENT = new Object();
    
        // 直接使用传进来的NavigableMap存储元素，浅拷贝
        TreeSet(NavigableMap<E,Object> m) {
            this.m = m;
        }
    
        // 使用TreeMap初始化
        public TreeSet() {
            this(new TreeMap<E,Object>());
        }
    
        // 使用带comparator的TreeMap初始化
        public TreeSet(Comparator<? super E> comparator) {
            this(new TreeMap<>(comparator));
        }
    
        // 将集合c中的所有元素添加的TreeSet中
        public TreeSet(Collection<? extends E> c) {
            this();
            addAll(c);
        }
    
        // 将SortedSet中的所有元素添加到TreeSet中
        public TreeSet(SortedSet<E> s) {
            this(s.comparator());
            addAll(s);
        }
    
        // 迭代器
        public Iterator<E> iterator() {
            return m.navigableKeySet().iterator();
        }
    
        // 逆序迭代器
        public Iterator<E> descendingIterator() {
            return m.descendingKeySet().iterator();
        }
    
        // 以逆序返回一个新的TreeSet
        public NavigableSet<E> descendingSet() {
            return new TreeSet<>(m.descendingMap());
        }
    
        // 元素个数
        public int size() {
            return m.size();
        }
    
        // 判断是否为空
        public boolean isEmpty() {
            return m.isEmpty();
        }
    
        // 判断是否包含某元素
        public boolean contains(Object o) {
            return m.containsKey(o);
        }
    
        // 添加元素, 调用map的put()方法, value为PRESENT
        public boolean add(E e) {
            return m.put(e, PRESENT)==null;
        }
    
        // 删除元素
        public boolean remove(Object o) {
            return m.remove(o)==PRESENT;
        }
    
        // 清空所有元素
        public void clear() {
            m.clear();
        }
    
        // 添加集合c中的所有元素
        public  boolean addAll(Collection<? extends E> c) {
            // 满足一定条件时直接调用TreeMap的addAllForTreeSet()方法添加元素
            if (m.size()==0 && c.size() > 0 &&
                c instanceof SortedSet &&
                m instanceof TreeMap) {
                SortedSet<? extends E> set = (SortedSet<? extends E>) c;
                TreeMap<E,Object> map = (TreeMap<E, Object>) m;
                Comparator<?> cc = set.comparator();
                Comparator<? super E> mc = map.comparator();
                if (cc==mc || (cc != null && cc.equals(mc))) {
                    map.addAllForTreeSet(set, PRESENT);
                    return true;
                }
            }
            // 不满足上述条件, 调用父类的addAll()通过遍历的方式一个一个地添加元素
            return super.addAll(c);
        }
    
        // 子set（NavigableSet中的方法）
        public NavigableSet<E> subSet(E fromElement, boolean fromInclusive,
                                      E toElement,   boolean toInclusive) {
            return new TreeSet<>(m.subMap(fromElement, fromInclusive,
                                           toElement,   toInclusive));
        }
    
        // 头set（NavigableSet中的方法）
        public NavigableSet<E> headSet(E toElement, boolean inclusive) {
            return new TreeSet<>(m.headMap(toElement, inclusive));
        }
    
        // 尾set（NavigableSet中的方法）
        public NavigableSet<E> tailSet(E fromElement, boolean inclusive) {
            return new TreeSet<>(m.tailMap(fromElement, inclusive));
        }
    
        // 子set（SortedSet接口中的方法）
        public SortedSet<E> subSet(E fromElement, E toElement) {
            return subSet(fromElement, true, toElement, false);
        }
    
        // 头set（SortedSet接口中的方法）
        public SortedSet<E> headSet(E toElement) {
            return headSet(toElement, false);
        }
    
        // 尾set（SortedSet接口中的方法）
        public SortedSet<E> tailSet(E fromElement) {
            return tailSet(fromElement, true);
        }
    
        // 比较器
        public Comparator<? super E> comparator() {
            return m.comparator();
        }
    
        // 返回最小的元素
        public E first() {
            return m.firstKey();
        }
    
        // 返回最大的元素
        public E last() {
            return m.lastKey();
        }
    
        // 返回小于e的最大的元素
        public E lower(E e) {
            return m.lowerKey(e);
        }
    
        // 返回小于等于e的最大的元素
        public E floor(E e) {
            return m.floorKey(e);
        }
    
        // 返回大于等于e的最小的元素
        public E ceiling(E e) {
            return m.ceilingKey(e);
        }
    
        // 返回大于e的最小的元素
        public E higher(E e) {
            return m.higherKey(e);
        }
    
        // 弹出最小的元素
        public E pollFirst() {
            Map.Entry<E,?> e = m.pollFirstEntry();
            return (e == null) ? null : e.getKey();
        }
    
        public E pollLast() {
            Map.Entry<E,?> e = m.pollLastEntry();
            return (e == null) ? null : e.getKey();
        }
    
        // 克隆方法
        @SuppressWarnings("unchecked")
        public Object clone() {
            TreeSet<E> clone;
            try {
                clone = (TreeSet<E>) super.clone();
            } catch (CloneNotSupportedException e) {
                throw new InternalError(e);
            }
    
            clone.m = new TreeMap<>(m);
            return clone;
        }
    
        // 序列化写出方法
        private void writeObject(java.io.ObjectOutputStream s)
            throws java.io.IOException {
            // Write out any hidden stuff
            s.defaultWriteObject();
    
            // Write out Comparator
            s.writeObject(m.comparator());
    
            // Write out size
            s.writeInt(m.size());
    
            // Write out all elements in the proper order.
            for (E e : m.keySet())
                s.writeObject(e);
        }
    
        // 序列化写入方法
        private void readObject(java.io.ObjectInputStream s)
            throws java.io.IOException, ClassNotFoundException {
            // Read in any hidden stuff
            s.defaultReadObject();
    
            // Read in Comparator
            @SuppressWarnings("unchecked")
                Comparator<? super E> c = (Comparator<? super E>) s.readObject();
    
            // Create backing TreeMap
            TreeMap<E,Object> tm = new TreeMap<>(c);
            m = tm;
    
            // Read in size
            int size = s.readInt();
    
            tm.readTreeSet(size, s, PRESENT);
        }
    
        // 可分割的迭代器
        public Spliterator<E> spliterator() {
            return TreeMap.keySpliteratorFor(m);
        }
    
        // 序列化id
        private static final long serialVersionUID = -2479143000061671589L;
    }
    
```

总结：

1. TreeSet底层使用NavigableMap（实现类为TreeMap）存储元素；

2. TreeSet是有序的、非线程安全的；

3. TreeSet实现了NavigableSet接口，而NavigableSet继承自SortedSet接口；

## ConcurrentSkipListSet

ConcurrentSkipListSet底层是通过ConcurrentNavigableMap来实现的，它是一个有序的线程安全的集合。

```java
    // 实现了NavigableSet接口，并没有所谓的ConcurrentNavigableSet接口
    public class ConcurrentSkipListSet<E>
        extends AbstractSet<E>
        implements NavigableSet<E>, Cloneable, java.io.Serializable {
    
        private static final long serialVersionUID = -2479143111061671589L;
    
        // 存储使用的map
        private final ConcurrentNavigableMap<E,Object> m;
    
        // 初始化
        public ConcurrentSkipListSet() {
            m = new ConcurrentSkipListMap<E,Object>();
        }
    
        // 传入比较器
        public ConcurrentSkipListSet(Comparator<? super E> comparator) {
            m = new ConcurrentSkipListMap<E,Object>(comparator);
        }
    
        // 使用ConcurrentSkipListMap初始化map
        // 并将集合c中所有元素放入到map中
        public ConcurrentSkipListSet(Collection<? extends E> c) {
            m = new ConcurrentSkipListMap<E,Object>();
            addAll(c);
        }
    
        // 使用ConcurrentSkipListMap初始化map
        // 并将有序Set中所有元素放入到map中
        public ConcurrentSkipListSet(SortedSet<E> s) {
            m = new ConcurrentSkipListMap<E,Object>(s.comparator());
            addAll(s);
        }
    
        // ConcurrentSkipListSet类内部返回子set时使用的
        ConcurrentSkipListSet(ConcurrentNavigableMap<E,Object> m) {
            this.m = m;
        }
    
        // 克隆方法
        public ConcurrentSkipListSet<E> clone() {
            try {
                @SuppressWarnings("unchecked")
                ConcurrentSkipListSet<E> clone =
                    (ConcurrentSkipListSet<E>) super.clone();
                clone.setMap(new ConcurrentSkipListMap<E,Object>(m));
                return clone;
            } catch (CloneNotSupportedException e) {
                throw new InternalError();
            }
        }
    
        /* ---------------- Set operations -------------- */
        // 返回元素个数
        public int size() {
            return m.size();
        }
    
        // 检查是否为空
        public boolean isEmpty() {
            return m.isEmpty();
        }
    
        // 检查是否包含某个元素
        public boolean contains(Object o) {
            return m.containsKey(o);
        }
    
        // 添加一个元素
        // 调用map的putIfAbsent()方法
        public boolean add(E e) {
            return m.putIfAbsent(e, Boolean.TRUE) == null;
        }
    
        // 移除一个元素
        public boolean remove(Object o) {
            return m.remove(o, Boolean.TRUE);
        }
    
        // 清空所有元素
        public void clear() {
            m.clear();
        }
    
        // 迭代器
        public Iterator<E> iterator() {
            return m.navigableKeySet().iterator();
        }
    
        // 降序迭代器
        public Iterator<E> descendingIterator() {
            return m.descendingKeySet().iterator();
        }
    
    
        /* ---------------- AbstractSet Overrides -------------- */
        // 比较相等方法
        public boolean equals(Object o) {
            // Override AbstractSet version to avoid calling size()
            if (o == this)
                return true;
            if (!(o instanceof Set))
                return false;
            Collection<?> c = (Collection<?>) o;
            try {
                // 这里是通过两次两层for循环来比较
                // 这里是有很大优化空间的，参考上篇文章CopyOnWriteArraySet中的彩蛋
                return containsAll(c) && c.containsAll(this);
            } catch (ClassCastException unused) {
                return false;
            } catch (NullPointerException unused) {
                return false;
            }
        }
    
        // 移除集合c中所有元素
        public boolean removeAll(Collection<?> c) {
            // Override AbstractSet version to avoid unnecessary call to size()
            boolean modified = false;
            for (Object e : c)
                if (remove(e))
                    modified = true;
            return modified;
        }
    
        /* ---------------- Relational operations -------------- */
    
        // 小于e的最大元素
        public E lower(E e) {
            return m.lowerKey(e);
        }
    
        // 小于等于e的最大元素
        public E floor(E e) {
            return m.floorKey(e);
        }
    
        // 大于等于e的最小元素
        public E ceiling(E e) {
            return m.ceilingKey(e);
        }
    
        // 大于e的最小元素
        public E higher(E e) {
            return m.higherKey(e);
        }
    
        // 弹出最小的元素
        public E pollFirst() {
            Map.Entry<E,Object> e = m.pollFirstEntry();
            return (e == null) ? null : e.getKey();
        }
    
        // 弹出最大的元素
        public E pollLast() {
            Map.Entry<E,Object> e = m.pollLastEntry();
            return (e == null) ? null : e.getKey();
        }
    
    
        /* ---------------- SortedSet operations -------------- */
    
        // 取比较器
        public Comparator<? super E> comparator() {
            return m.comparator();
        }
    
        // 最小的元素
        public E first() {
            return m.firstKey();
        }
    
        // 最大的元素
        public E last() {
            return m.lastKey();
        }
    
        // 取两个元素之间的子set
        public NavigableSet<E> subSet(E fromElement,
                                      boolean fromInclusive,
                                      E toElement,
                                      boolean toInclusive) {
            return new ConcurrentSkipListSet<E>
                (m.subMap(fromElement, fromInclusive,
                          toElement,   toInclusive));
        }
    
        // 取头子set
        public NavigableSet<E> headSet(E toElement, boolean inclusive) {
            return new ConcurrentSkipListSet<E>(m.headMap(toElement, inclusive));
        }
    
        // 取尾子set
        public NavigableSet<E> tailSet(E fromElement, boolean inclusive) {
            return new ConcurrentSkipListSet<E>(m.tailMap(fromElement, inclusive));
        }
    
        // 取子set，包含from，不包含to
        public NavigableSet<E> subSet(E fromElement, E toElement) {
            return subSet(fromElement, true, toElement, false);
        }
    
        // 取头子set，不包含to
        public NavigableSet<E> headSet(E toElement) {
            return headSet(toElement, false);
        }
    
        // 取尾子set，包含from
        public NavigableSet<E> tailSet(E fromElement) {
            return tailSet(fromElement, true);
        }
    
        // 降序set
        public NavigableSet<E> descendingSet() {
            return new ConcurrentSkipListSet<E>(m.descendingMap());
        }
    
        // 可分割的迭代器
        @SuppressWarnings("unchecked")
        public Spliterator<E> spliterator() {
            if (m instanceof ConcurrentSkipListMap)
                return ((ConcurrentSkipListMap<E,?>)m).keySpliterator();
            else
                return (Spliterator<E>)((ConcurrentSkipListMap.SubMap<E,?>)m).keyIterator();
        }
    
        // 原子更新map，给clone方法使用
        private void setMap(ConcurrentNavigableMap<E,Object> map) {
            UNSAFE.putObjectVolatile(this, mapOffset, map);
        }
    
        // 原子操作相关内容
        private static final sun.misc.Unsafe UNSAFE;
        private static final long mapOffset;
        static {
            try {
                UNSAFE = sun.misc.Unsafe.getUnsafe();
                Class<?> k = ConcurrentSkipListSet.class;
                mapOffset = UNSAFE.objectFieldOffset
                    (k.getDeclaredField("m"));
            } catch (Exception e) {
                throw new Error(e);
            }
        }
    }
```

ConcurrentSkipListSet基本上都是使用ConcurrentSkipListMap实现的，虽然取子set部分是使用ConcurrentSkipListMap中的内部类，但是这些内部类其实也是和ConcurrentSkipListMap相关的，它们返回ConcurrentSkipListMap的一部分数据。

### 总结

- ConcurrentSkipListSet底层是使用ConcurrentNavigableMap实现的；
- ConcurrentSkipListSet有序的，基于元素的自然排序或者通过比较器确定的顺序；
- ConcurrentSkipListSet是线程安全的；

## ArrayList

- ArrayList 本质上是一个可变大小的数组。具有数组的特性，默认初始容量为10，且动态扩容近似1.5倍，非线程安全。
- 实现了RandomAcess接口，具有随机访问特性，但插入和删除效率较慢
- **modCount是AbstractList的属性，记录了集合结构修改次数，采用迭代器迭代的时候会将值同步给迭代器中的属性expectedmodCount，即支持fail-fast**

### **核心成员变量**

```java
    /**
     * 默认容量
     */
    private static final int DEFAULT_CAPACITY = 10;
    
    /**
     * 空数组，如果传入的容量为0时使用
     */
    private static final Object[] EMPTY_ELEMENTDATA = {};
    
    /**
     * 空数组，当传入容量时使用，添加第一个元素的时候会重新初始为默认容量大小
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
    
    /**
     * 存储元素的数组，使用transient是为了不序列化这个字段
     */
    transient Object[] elementData; // non-private to simplify nested class access
    
    /**
     * 集合中真正存储元素的个数，非数组长度
     */
    private int size;
    
```

### **构造函数**

```java
    /**
    * 默认构造函数为空参，即懒加载
    */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    
    /**
     * 构造一个具有指定初始容量的空列表。
     */
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
    
    /**
      *  构造一个包含指定 collection 的元素的列表，这些元素是按照该 collection 的迭代器返回它们的顺序排列的。
      */
    public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // defend against c.toArray (incorrectly) not returning Object[]
            // (see e.g. https://bugs.openjdk.java.net/browse/JDK-6260652)
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // replace with empty array.
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }
    
```

### add

```java
     /**
     * modCount是AbstractList的属性，记录了集合结构修改次数
     */
    public boolean add(E e) {
        modCount++;
        add(e, elementData, size);
        return true;
    }
     /**
     * 集合长度达到容量后，进行grow扩容
     */
    private void add(E e, Object[] elementData, int s) {
        if (s == elementData.length)
            elementData = grow();
        elementData[s] = e;
        size = s + 1;
    }

    /**
     * 往指定位置添加元素
     */
    public void add(int index, E element) {
        rangeCheckForAdd(index);
        modCount++;
        final int s;
        Object[] elementData;
        if ((s = size) == (elementData = this.elementData).length)
            elementData = grow();
        System.arraycopy(elementData, index,
                         elementData, index + 1,
                         s - index);
        elementData[index] = element;
        size = s + 1;
    }
```

### **remove**

```java
    public E remove(int index) {
        Objects.checkIndex(index, size);
        final Object[] es = elementData;
   
        @SuppressWarnings("unchecked") E oldValue = (E) es[index];
        fastRemove(es, index);

        return oldValue;
    }
    //运用arraycopy复制前后元素达到删除的目的
    private void fastRemove(Object[] es, int i) {
        modCount++;
        final int newSize;
        if ((newSize = size - 1) > i)
            System.arraycopy(es, i + 1, es, i, newSize - i);
        es[size = newSize] = null;
    }

    //清空元素
    public void clear() {
        modCount++;
        final Object[] es = elementData;
        for (int to = size, i = size = 0; i < to; i++)
            es[i] = null;
    }
```

### **get**

```java
   public E get(int index) {
        //检查是否越界
        Objects.checkIndex(index, size);
        return elementData(index);
    }
```

### **grow**

```java
    //add元素当真实长度达到数组长度，会进行grow()调用
    private Object[] grow() {
        return grow(size + 1);
    }

    //扩容容量正确后，则进行copyof扩容
    private Object[] grow(int minCapacity) {
        return elementData = Arrays.copyOf(elementData,
                                           newCapacity(minCapacity));
    }

    //计算新的扩容量，若容量不越界，则返回true
    private int newCapacity(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity <= 0) {
            if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
                return Math.max(DEFAULT_CAPACITY, minCapacity);
            if (minCapacity < 0) // overflow
                throw new OutOfMemoryError();
            return minCapacity;
        }
        return (newCapacity - MAX_ARRAY_SIZE <= 0)
            ? newCapacity
            : hugeCapacity(minCapacity);
    }
    
    //判断容量是否越界
    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE)
            ? Integer.MAX_VALUE
            : MAX_ARRAY_SIZE;
    }
```

### **trimToSize**

```java
    public void trimToSize() {
        modCount++;
        //运用copyof将真实长度的数组拷贝
        if (size < elementData.length) {
            elementData = (size == 0)
              ? EMPTY_ELEMENTDATA
              : Arrays.copyOf(elementData, size);
        }
    }
```

```java
//其他API
    public boolean isEmpty() {
        return size == 0;
    }
    public int size() {
        return size;
    }
    public boolean contains(Object o) {
        return indexOf(o) >= 0;
    }
```

### **序列化与反序列化**

elementData设置为了transient后，ArrayList自定义实现了readObject与writeObject方法，优势：根据size序列化真实的元素，而不是数组的长度序列化元素，减少了空间占用。

```java
    private void readObject(java.io.ObjectInputStream s)
        throws java.io.IOException, ClassNotFoundException {

        // Read in size, and any hidden stuff
        s.defaultReadObject();

        // Read in capacity
        s.readInt(); // ignored

        if (size > 0) {
            // like clone(), allocate array based upon size not capacity
            SharedSecrets.getJavaObjectInputStreamAccess().checkArray(s, Object[].class, size);
            Object[] elements = new Object[size];

            // Read in all elements in the proper order.
            for (int i = 0; i < size; i++) {
                elements[i] = s.readObject();
            }

            elementData = elements;
        } else if (size == 0) {
            elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new java.io.InvalidObjectException("Invalid size: " + size);
        }
    }

    private void writeObject(java.io.ObjectOutputStream s)
        throws java.io.IOException {
        // Write out element count, and any hidden stuff
        int expectedModCount = modCount;
        s.defaultWriteObject();

        // Write out size as capacity for behavioral compatibility with clone()
        s.writeInt(size);

        // Write out all elements in the proper order.
        for (int i=0; i<size; i++) {
            s.writeObject(elementData[i]);
        }

        if (modCount != expectedModCount) {
            throw new ConcurrentModificationException();
        }
    }

```

## Vector

是一个线程安全的集合类，方法声明都用了synchronized关键字进行修饰，且动态扩容2倍。

### 核心成员变量

```java
public class Vector<E>
    extends AbstractList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable{
    // 存储元素的对象数组
    protected Object[] elementData;
    // 元素个数
    protected int elementCount;
    // 扩容增长量
    protected int capacityIncrement;

}
```

### 构造方法

```java
    public Vector(int initialCapacity, int capacityIncrement) {
        super();
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        this.elementData = new Object[initialCapacity];
        this.capacityIncrement = capacityIncrement;
    }

    public Vector(int initialCapacity) {
        this(initialCapacity, 0);
    }

    // 默认容量为10
    public Vector() {
        this(10);
    }
```

### add

```java
    private void add(E e, Object[] elementData, int s) {
        if (s == elementData.length)
            elementData = grow();
        elementData[s] = e;
        elementCount = s + 1;
    }

    public synchronized boolean add(E e) {
        modCount++;
        add(e, elementData, elementCount);
        return true;
    }
```

### remove

```java
  public boolean remove(Object o) {
        return removeElement(o);
  }

  public synchronized boolean removeElement(Object obj) {
        modCount++;
        int i = indexOf(obj);
        if (i >= 0) {
            removeElementAt(i);
            return true;
        }
        return false;
    }
```

### get

```java
    public synchronized E get(int index) {
        if (index >= elementCount)
            throw new ArrayIndexOutOfBoundsException(index);

        return elementData(index);
    }
```

### grow

扩容本质跟ArrayList方法大致相同，只是可以自定义扩容增长量。

```java
private Object[] grow() {
    return grow(elementCount + 1);
}

private Object[] grow(int minCapacity) {
    return elementData = Arrays.copyOf(elementData,
                                       newCapacity(minCapacity));
}


/**
 *若自定义的capacityIncrement <= 0 ，则按原容量扩容，即两倍扩容
 */
private int newCapacity(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                     capacityIncrement : oldCapacity);
    if (newCapacity - minCapacity <= 0) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return minCapacity;
    }
    return (newCapacity - MAX_ARRAY_SIZE <= 0)
        ? newCapacity
        : hugeCapacity(minCapacity);
}

private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) // overflow
        throw new OutOfMemoryError();
    return (minCapacity > MAX_ARRAY_SIZE) ?
        Integer.MAX_VALUE :
        MAX_ARRAY_SIZE;
}
```

### trimToSize

```java
    public synchronized void trimToSize() {
        modCount++;
        int oldCapacity = elementData.length;
        if (elementCount < oldCapacity) {
            elementData = Arrays.copyOf(elementData, elementCount);
        }
    }
```

------

总结：

- 与ArrayList大致相同，只是添加、修改、删除元素的方法加了synchronized关键字进行修饰
- 可自定义扩容量，若无定义其扩容量，则默认按原容量扩容，即为2倍扩容

## LinkedList

- 是一个双链表，具有链表特性，使得LinkedList在插入和删除时更优于ArrayList，而随机访问则比ArrayList逊色些，非线程安全。
- 继承于AbstractSequentialList，具有链表特性，实现了Deque接口，能够实现队列操作。
- **modCount是AbstractList的属性，记录了集合结构修改次数，采用迭代器迭代的时候会将值同步给迭代器中的属性expectedmodCount，即支持fail-fast**

**定义**

```java
     public class LinkedList<E>
        extends AbstractSequentialList<E>
        implements List<E>, Deque<E>, Cloneable, java.io.Serializable
```

### **核心成员变量**

```java
    transient int size = 0;

    //Pointer to first node
    transient Node<E> first;

    //Pointer to last node.
    transient Node<E> last;

    private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
    
```

### **构造方法**

```java
    // 默认无参构造
    public LinkedList() {
    }

    // 添加Collection的所有元素
    public LinkedList(Collection<? extends E> c) {
        this();
        addAll(c);
    }
```

### **添加元素**

```java
    // Links e as first element.
    private void linkFirst(E e) {
        final Node<E> f = first;
        final Node<E> newNode = new Node<>(null, e, f);
        first = newNode;
        if (f == null)
            last = newNode;
        else
            f.prev = newNode;
        size++;
        modCount++;
    }

    // Links e as last element.
    void linkLast(E e) {
        final Node<E> l = last;
        final Node<E> newNode = new Node<>(l, e, null);
        last = newNode;
        if (l == null)
            first = newNode;
        else
            l.next = newNode;
        size++;
        modCount++;
    } 

    // Inserts element e before non-null Node succ.
    void linkBefore(E e, Node<E> succ) {
        // assert succ != null;
        final Node<E> pred = succ.prev;
        final Node<E> newNode = new Node<>(pred, e, succ);
        succ.prev = newNode;
        if (pred == null)
            first = newNode;
        else
            pred.next = newNode;
        size++;
        modCount++;
    }
```

### **删除元素**

```java
    public E removeFirst() {
        final Node<E> f = first;
        if (f == null)
            throw new NoSuchElementException();
        return unlinkFirst(f);
    }

    public E removeLast() {
        final Node<E> l = last;
        if (l == null)
            throw new NoSuchElementException();
        return unlinkLast(l);
    }

    // Unlinks non-null first node f.
    private E unlinkFirst(Node<E> f) {
        // assert f == first && f != null;
        final E element = f.item;
        final Node<E> next = f.next;
        f.item = null;
        f.next = null; // help GC
        first = next;
        if (next == null)
            last = null;
        else
            next.prev = null;
        size--;
        modCount++;
        return element;
    }

    // Unlinks non-null last node l.
    private E unlinkLast(Node<E> l) {
        // assert l == last && l != null;
        final E element = l.item;
        final Node<E> prev = l.prev;
        l.item = null;
        l.prev = null; // help GC
        last = prev;
        if (prev == null)
            first = null;
        else
            prev.next = null;
        size--;
        modCount++;
        return element;
    }
```

### **其他特性**

```java
//具有队列特性，实现Deque方法，如poll()、pollFirst()、pollLast()、peekFirst()、peekLast()、offer(E)、offerFirst(E)、offerLast(E)等方法

//具有栈特性，实现了Deque方法，如pop()、push(E)等方法

//本质上是调用了unlinkLast、unlinkFirst、linkFirst等方法，只是对于空值处理不同。
```

## CopyOnWriteArrayList

CopyOnWriteArrayList是ArrayList的线程安全版本，内部通过数组实现，每次对数组的修改都拷贝一份新的数组来修改，完成后替换旧数组，保证了只阻塞写操作，不阻塞读操作，实现读写分离。

#### 核心成员变量

```java
public class CopyOnWriteArrayList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable {
    // 阻塞锁，用来对写操作加锁
    final transient Object lock = new Object();
    
    // 用volatile保证线程可见性
    private transient volatile Object[] array;

}
```

#### 构造方法

```java
    // 默认无参构造数组为空
    public CopyOnWriteArrayList() {
        setArray(new Object[0]);
    }

    public CopyOnWriteArrayList(Collection<? extends E> c) {
        Object[] es;
        if (c.getClass() == CopyOnWriteArrayList.class)
            es = ((CopyOnWriteArrayList<?>)c).getArray();
        else {
            es = c.toArray();
            //toArray判断返回类型
            if (es.getClass() != Object[].class)
                es = Arrays.copyOf(es, es.length, Object[].class);
        }
        setArray(es);
    }

    public CopyOnWriteArrayList(E[] toCopyIn) {
        setArray(Arrays.copyOf(toCopyIn, toCopyIn.length, Object[].class));
    }

```

#### add

修改通过copyof复制一份副本后修改，再进行替换，无需扩容操作。

```java
    // 此处加锁，说明并发度本质不高，加锁完成后替换
    public boolean add(E e) {
        synchronized (lock) {
            Object[] es = getArray();
            int len = es.length;
            es = Arrays.copyOf(es, len + 1);
            es[len] = e;
            setArray(es);
            return true;
        }
    }

    // 判断添加位置，不同位置不同逻辑
    public void add(int index, E element) {
        synchronized (lock) {
            Object[] es = getArray();
            int len = es.length;
            if (index > len || index < 0)
                throw new IndexOutOfBoundsException(outOfBounds(index, len));
            Object[] newElements;
            int numMoved = len - index;
            if (numMoved == 0)
                newElements = Arrays.copyOf(es, len + 1);
            else {
                newElements = new Object[len + 1];
                System.arraycopy(es, 0, newElements, 0, index);
                System.arraycopy(es, index, newElements, index + 1,
                                 numMoved);
            }
            newElements[index] = element;
            setArray(newElements);
        }
    }
```

#### get

```java
    public E get(int index) {
        // 获取元素不需要加锁
        // 直接返回index位置的元素
        // 这里是没有做越界检查的, 因为数组本身会做越界检查
        return get(getArray(), index);
    }
    
    final Object[] getArray() {
        return array;
    }
    
    private E get(Object[] a, int index) {
        return (E) a[index];
    }
    
```

#### remove

本质还是通过判断删除位置，加锁后用不同逻辑处理copyof复制成一个新数组并替换。

```java
   public boolean remove(Object o) {
        Object[] snapshot = getArray();
        int index = indexOfRange(o, snapshot, 0, snapshot.length);
        return index >= 0 && remove(o, snapshot, index);
    }

    /**
     * recent snapshot contains o at the given index.
     */
    private boolean remove(Object o, Object[] snapshot, int index) {
        synchronized (lock) {
            Object[] current = getArray();
            int len = current.length;
            if (snapshot != current) findIndex: {
                int prefix = Math.min(index, len);
                for (int i = 0; i < prefix; i++) {
                    if (current[i] != snapshot[i]
                        && Objects.equals(o, current[i])) {
                        index = i;
                        break findIndex;
                    }
                }
                if (index >= len)
                    return false;
                if (current[index] == o)
                    break findIndex;
                index = indexOfRange(o, current, index, len);
                if (index < 0)
                    return false;
            }
            Object[] newElements = new Object[len - 1];
            System.arraycopy(current, 0, newElements, 0, index);
            System.arraycopy(current, index + 1,
                             newElements, index,
                             len - index - 1);
            setArray(newElements);
            return true;
        }
    }
```

#### 其他API

```java
    public int size() {
        // 获取元素个数不需要加锁
        // 直接返回数组的长度
        return getArray().length;
    }

    final void setArray(Object[] a) {
        array = a;
    }
```

------

总结：

- CopyOnWriteArrayList采用内置synchronized加锁，保证线程安全；
- CopyOnWriteArrayList的写操作要先拷贝一份数组，在新数组中做修改，再替换旧数组，所以空间复杂度是O(n)，性能比较低下； 
- CopyOnWriteArrayList的读操作支持随机访问，时间复杂度为O(1)； 
- CopyOnWriteArrayList采用读写分离的思想，读操作不加锁，写操作加锁，且写操作占用较大内存空间，所以适用于读多写少的场合； 
- CopyOnWriteArrayList只保证最终一致性，不保证实时一致性；

## CopyOnWriteArraySet

CopyOnWriteArraySet底层是使用CopyOnWriteArrayList存储元素的，所以它并不是使用Map来存储元素的。

**主要API**

```java
   public class CopyOnWriteArraySet<E> extends AbstractSet<E>
            implements java.io.Serializable {
        private static final long serialVersionUID = 5457747651344034263L;
    
        // 内部使用CopyOnWriteArrayList存储元素
        private final CopyOnWriteArrayList<E> al;
    
        // 构造方法
        public CopyOnWriteArraySet() {
            al = new CopyOnWriteArrayList<E>();
        }
    
        // 将集合c中的元素初始化到CopyOnWriteArraySet中
        public CopyOnWriteArraySet(Collection<? extends E> c) {
            if (c.getClass() == CopyOnWriteArraySet.class) {
                // 如果c是CopyOnWriteArraySet类型，说明没有重复元素，
                // 直接调用CopyOnWriteArrayList的构造方法初始化
                @SuppressWarnings("unchecked") CopyOnWriteArraySet<E> cc =
                    (CopyOnWriteArraySet<E>)c;
                al = new CopyOnWriteArrayList<E>(cc.al);
            }
            else {
                // 如果c不是CopyOnWriteArraySet类型，说明有重复元素
                // 调用CopyOnWriteArrayList的addAllAbsent()方法初始化
                // 它会把重复元素排除掉
                al = new CopyOnWriteArrayList<E>();
                al.addAllAbsent(c);
            }
        }
    
        // 获取元素个数
        public int size() {
            return al.size();
        }
    
        // 检查集合是否为空
        public boolean isEmpty() {
            return al.isEmpty();
        }
    
        // 检查是否包含某个元素
        public boolean contains(Object o) {
            return al.contains(o);
        }
    
        // 集合转数组
        public Object[] toArray() {
            return al.toArray();
        }
    
        // 集合转数组，这里是可能有bug的，详情见ArrayList中分析
        public <T> T[] toArray(T[] a) {
            return al.toArray(a);
        }
    
        // 清空所有元素
        public void clear() {
            al.clear();
        }
    
        // 删除元素
        public boolean remove(Object o) {
            return al.remove(o);
        }
    
        // 添加元素
        // 这里是调用CopyOnWriteArrayList的addIfAbsent()方法
        // 它会检测元素不存在的时候才添加
        // 还记得这个方法吗？当时有分析过的，建议把CopyOnWriteArrayList拿出来再看看
        public boolean add(E e) {
            return al.addIfAbsent(e);
        }
    
        // 是否包含c中的所有元素
        public boolean containsAll(Collection<?> c) {
            return al.containsAll(c);
        }
    
        // 并集
        public boolean addAll(Collection<? extends E> c) {
            return al.addAllAbsent(c) > 0;
        }
    
        // 单方向差集
        public boolean removeAll(Collection<?> c) {
            return al.removeAll(c);
        }
    
        // 交集
        public boolean retainAll(Collection<?> c) {
            return al.retainAll(c);
        }
    
        // 迭代器
        public Iterator<E> iterator() {
            return al.iterator();
        }
    
        // equals()方法
        public boolean equals(Object o) {
            // 如果两者是同一个对象，返回true
            if (o == this)
                return true;
            // 如果o不是Set对象，返回false
            if (!(o instanceof Set))
                return false;
            Set<?> set = (Set<?>)(o);
            Iterator<?> it = set.iterator();
    
            // 集合元素数组的快照
            Object[] elements = al.getArray();
            int len = elements.length;
    

            boolean[] matched = new boolean[len];
            int k = 0;
            // 从o这个集合开始遍历
            outer: while (it.hasNext()) {
                // 如果k>len了，说明o中元素多了
                if (++k > len)
                    return false;
                // 取值
                Object x = it.next();
                // 遍历检查是否在当前集合中
                for (int i = 0; i < len; ++i) {
                    if (!matched[i] && eq(x, elements[i])) {
                        matched[i] = true;
                        continue outer;
                    }
                }
                // 如果不在当前集合中，返回false
                return false;
            }
            return k == len;
        }
    
        // 移除满足过滤条件的元素
        public boolean removeIf(Predicate<? super E> filter) {
            return al.removeIf(filter);
        }
    
        // 遍历元素
        public void forEach(Consumer<? super E> action) {
            al.forEach(action);
        }
    
        // 分割的迭代器
        public Spliterator<E> spliterator() {
            return Spliterators.spliterator
                (al.getArray(), Spliterator.IMMUTABLE | Spliterator.DISTINCT);
        }
    
        // 比较两个元素是否相等
        private static boolean eq(Object o1, Object o2) {
            return (o1 == null) ? o2 == null : o1.equals(o2);
        }
    }
    
```

总结：底层采用CopyOnWriteArrayList实现，并且去重采用CopyOnWriteArrayList的addIfAbsent()方法，当元素不存在的时候才添加，本质上效率低。

## PriorityQueue

一般来说，优先级队列使用堆来实现。

### 核心成员变量

```java
        // 默认容量
        private static final int DEFAULT_INITIAL_CAPACITY = 11;
        // 存储元素的地方
        transient Object[] queue; // non-private to simplify nested class access
        // 元素个数
        private int size = 0;
        // 比较器
        private final Comparator<? super E> comparator;
        // 修改次数
        transient int modCount = 0; // non-private to simplify nested class access
```

### 入队

- 入队有两个方法，add(E e)和offer(E e)，两者是一致的，add(E e)也是调用的offer(E e)

- PriorityQueue是一个小顶堆

```java
   public boolean add(E e) {
        return offer(e);
    }
    
    public boolean offer(E e) {
        // 不支持null元素
        if (e == null)
            throw new NullPointerException();
        modCount++;
        // 取size
        int i = size;
        // 元素个数达到最大容量了，扩容
        if (i >= queue.length)
            grow(i + 1);
        // 元素个数加1
        size = i + 1;
        // 如果还没有元素
        // 直接插入到数组第一个位置
        if (i == 0)
            queue[0] = e;
        else
            // 否则，插入元素到数组size的位置，也就是最后一个元素的下一位
            // 注意这里的size不是数组大小，而是元素个数
            // 然后，再做自下而上的堆化
            siftUp(i, e);
        return true;
    }
    
    private void siftUp(int k, E x) {
        // 根据是否有比较器，使用不同的方法
        if (comparator != null)
            siftUpUsingComparator(k, x);
        else
            siftUpComparable(k, x);
    }
    
    @SuppressWarnings("unchecked")
    private void siftUpComparable(int k, E x) {
        Comparable<? super E> key = (Comparable<? super E>) x;
        while (k > 0) {
            // 找到父节点的位置
            // 因为元素是从0开始的，所以减1之后再除以2
            int parent = (k - 1) >>> 1;
            // 父节点的值
            Object e = queue[parent];
            // 比较插入的元素与父节点的值
            // 如果比父节点大，则跳出循环
            // 否则交换位置
            if (key.compareTo((E) e) >= 0)
                break;
            // 与父节点交换位置
            queue[k] = e;
            // 现在插入的元素位置移到了父节点的位置
            // 继续与父节点再比较
            k = parent;
        }
        // 最后找到应该插入的位置，放入元素
        queue[k] = key;
    }
    
```

### 扩容

```java
    private void grow(int minCapacity) {
        // 旧容量
        int oldCapacity = queue.length;
        // Double size if small; else grow by 50%
        // 旧容量小于64时，容量翻倍
        // 旧容量大于等于64，容量只增加旧容量的一半
        int newCapacity = oldCapacity + ((oldCapacity < 64) ?
                                         (oldCapacity + 2) :
                                         (oldCapacity >> 1));
        // overflow-conscious code
        // 检查是否溢出
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
    
        // 创建出一个新容量大小的新数组并把旧数组元素拷贝过去
        queue = Arrays.copyOf(queue, newCapacity);
    }
    
    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
```

1. 当数组比较小（小于64）的时候每次扩容容量翻倍；

2. 当数组比较大的时候每次扩容只增加一半的容量；

### 出队

出队有两个方法，remove()和poll()，remove()也是调用的poll()，只是没有元素的时候抛出异常。

```java
    public E remove() {
        // 调用poll弹出队首元素
        E x = poll();
        if (x != null)
            // 有元素就返回弹出的元素
            return x;
        else
            // 没有元素就抛出异常
            throw new NoSuchElementException();
    }
    
    @SuppressWarnings("unchecked")
    public E poll() {
        // 如果size为0，说明没有元素
        if (size == 0)
            return null;
        // 弹出元素，元素个数减1
        int s = --size;
        modCount++;
        // 队列首元素
        E result = (E) queue[0];
        // 队列末元素
        E x = (E) queue[s];
        // 将队列末元素删除
        queue[s] = null;
        // 如果弹出元素后还有元素
        if (s != 0)
            // 将队列末元素移到队列首
            // 再做自上而下的堆化
            siftDown(0, x);
        // 返回弹出的元素
        return result;
    }
    
    private void siftDown(int k, E x) {
        // 根据是否有比较器，选择不同的方法
        if (comparator != null)
            siftDownUsingComparator(k, x);
        else
            siftDownComparable(k, x);
    }
    
    @SuppressWarnings("unchecked")
    private void siftDownComparable(int k, E x) {
        Comparable<? super E> key = (Comparable<? super E>)x;
        // 只需要比较一半就行了，因为叶子节点占了一半的元素
        int half = size >>> 1;        // loop while a non-leaf
        while (k < half) {
            // 寻找子节点的位置，这里加1是因为元素从0号位置开始
            int child = (k << 1) + 1; // assume left child is least
            // 左子节点的值
            Object c = queue[child];
            // 右子节点的位置
            int right = child + 1;
            if (right < size &&
                ((Comparable<? super E>) c).compareTo((E) queue[right]) > 0)
                // 左右节点取其小者
                c = queue[child = right];
            // 如果比子节点都小，则结束
            if (key.compareTo((E) c) <= 0)
                break;
            // 如果比最小的子节点大，则交换位置
            queue[k] = c;
            // 指针移到最小子节点的位置继续往下比较
            k = child;
        }
        // 找到正确的位置，放入元素
        queue[k] = key;
    }
```

将队列首元素弹出，自上而下堆化。

### 查看

取队首元素有两个方法，element()和peek()，element()也是调用的peek()，只是没取到元素时抛出异常。

```java
    public E element() {
        E x = peek();
        if (x != null)
            return x;
        else
            throw new NoSuchElementException();
    }
    public E peek() {
        return (size == 0) ? null : (E) queue[0];
    }
```

总结：

- PriorityQueue是一个非线程安全的小顶堆
- PriorityQueue不是有序的，只有堆顶存储着最小的元素
- 入队对应堆的插入元素，出队对应堆的删除元素

| 操作 | 抛出异常  | 返回特定值      |
| ---- | --------- | --------------- |
| 入队 | add(e)    | offer(e)——false |
| 出队 | remove()  | poll()——null    |
| 检查 | element() | peek()——null    |

## ArrayDeque

双端队列是一种特殊的队列，它的两端都可以进出元素，故而得名双端队列。

ArrayDeque是一种以数组方式实现的双端队列，它是非线程安全的。

ArrayDeque实现了Deque接口，Deque接口继承自Queue接口，它是对Queue的一种增强。

**Deque**

```java
   public interface Deque<E> extends Queue<E> {
        // 添加元素到队列头
        void addFirst(E e);
        // 添加元素到队列尾
        void addLast(E e);
        // 添加元素到队列头
        boolean offerFirst(E e);
        // 添加元素到队列尾
        boolean offerLast(E e);
        // 从队列头移除元素
        E removeFirst();
        // 从队列尾移除元素
        E removeLast();
        // 从队列头移除元素
        E pollFirst();
        // 从队列尾移除元素
        E pollLast();
        // 查看队列头元素
        E getFirst();
        // 查看队列尾元素
        E getLast();
        // 查看队列头元素
        E peekFirst();
        // 查看队列尾元素
        E peekLast();
        // 从队列头向后遍历移除指定元素
        boolean removeFirstOccurrence(Object o);
        // 从队列尾向前遍历移除指定元素
        boolean removeLastOccurrence(Object o);
    
        // *** 队列中的方法 ***
    
        // 添加元素，等于addLast(e)
        boolean add(E e);
         // 添加元素，等于offerLast(e)
        boolean offer(E e);
        // 移除元素，等于removeFirst()
        E remove();
        // 移除元素，等于pollFirst()
        E poll();
        // 查看元素，等于getFirst()
        E element();
        // 查看元素，等于peekFirst()
        E peek();
    
        // *** 栈方法 ***
    
        // 入栈，等于addFirst(e)
        void push(E e);
        // 出栈，等于removeFirst()
        E pop();
    
        // *** Collection中的方法 ***
    
        // 删除指定元素，等于removeFirstOccurrence(o)
        boolean remove(Object o);
        // 检查是否包含某个元素
        boolean contains(Object o);
        // 元素个数
        public int size();
        // 迭代器
        Iterator<E> iterator();
        // 反向迭代器
        Iterator<E> descendingIterator();
    }
```

### 核心成员变量

```java
public class ArrayDeque<E> extends AbstractCollection<E>
                           implements Deque<E>, Cloneable, Serializable
{
    // 存储元素的数组
    transient Object[] elements; // non-private to simplify nested class access
    // 队列头位置
    transient int head;
    // 队列尾位置
    transient int tail;
    // 最小初始容量
    private static final int MIN_INITIAL_CAPACITY = 8;
}
```

### 构造方法

```java
   // 默认构造方法，初始容量为16
    public ArrayDeque() {
        elements = new Object[16];
    }
    // 指定元素个数初始化
    public ArrayDeque(int numElements) {
        allocateElements(numElements);
    }
    // 将集合c中的元素初始化到数组中
    public ArrayDeque(Collection<? extends E> c) {
        allocateElements(c.size());
        addAll(c);
    }
    // 初始化数组
    private void allocateElements(int numElements) {
        elements = new Object[calculateSize(numElements)];
    }
    // 计算容量，这段代码的逻辑是算出大于numElements的最接近的2的n次方且不小于8
    // 比如，3算出来是8，9算出来是16，33算出来是64
    private static int calculateSize(int numElements) {
        int initialCapacity = MIN_INITIAL_CAPACITY;
        // Find the best power of two to hold elements.
        // Tests "<=" because arrays aren't kept full.
        if (numElements >= initialCapacity) {
            initialCapacity = numElements;
            initialCapacity |= (initialCapacity >>>  1);
            initialCapacity |= (initialCapacity >>>  2);
            initialCapacity |= (initialCapacity >>>  4);
            initialCapacity |= (initialCapacity >>>  8);
            initialCapacity |= (initialCapacity >>> 16);
            initialCapacity++;
    
            if (initialCapacity < 0)   // Too many elements, must back off
                initialCapacity >>>= 1;// Good luck allocating 2 ^ 30 elements
        }
        return initialCapacity;
    }
    
```

### 入队

主要分析两个，addFirst(e)和addLast(e)，分别从队列头或者从队列尾，若容量不足，则扩大两倍，通过取模确保在数组范围内。

```java
    // 从队列头入队
    public void addFirst(E e) {
        // 不允许null元素
        if (e == null)
            throw new NullPointerException();
        // 将head指针减1并与数组长度减1取模
        // 这是为了防止数组到头了边界溢出
        elements[head = (head - 1) & (elements.length - 1)] = e;
        // 如果头尾相遇，就扩容两倍
        if (head == tail)
            doubleCapacity();
    }
    // 从队列尾入队
    public void addLast(E e) {
        // 不允许null元素
        if (e == null)
            throw new NullPointerException();
        // 在尾指针的位置放入元素
        // 可以看到tail指针指向的是队列最后一个元素的下一个位置
        elements[tail] = e;
        // tail指针加1，如果到数组尾了就从头开始
        if ( (tail = (tail + 1) & (elements.length - 1)) == head)
            doubleCapacity();
    }
```

### 扩容

```java
    private void doubleCapacity() {
        assert head == tail;
        // 头指针的位置
        int p = head;
        // 旧数组长度
        int n = elements.length;
        // 头指针离数组尾的距离
        int r = n - p; // number of elements to the right of p
        // 新长度为旧长度的两倍
        int newCapacity = n << 1;
        // 判断是否溢出
        if (newCapacity < 0)
            throw new IllegalStateException("Sorry, deque too big");
        // 新建新数组
        Object[] a = new Object[newCapacity];
        // 将旧数组head之后的元素拷贝到新数组中
        System.arraycopy(elements, p, a, 0, r);
        // 将旧数组下标0到head之间的元素拷贝到新数组中
        System.arraycopy(elements, 0, a, r, p);
        // 赋值为新数组
        elements = a;
        // head指向0，tail指向旧数组长度表示的位置
        head = 0;
        tail = n;
    }
    
```

![202105091521088532.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091521088532.png)

### 出队

1. 出队有两种方式，从队列头或者从队列尾，无缩容；

2. 通过取模的方式让头尾指针在数组范围内循环；

```java
  // 从队列头出队
    public E pollFirst() {
        int h = head;
        @SuppressWarnings("unchecked")
        // 取队列头元素
        E result = (E) elements[h];
        // 如果队列为空，就返回null
        if (result == null)
            return null;
        // 将队列头置为空
        elements[h] = null;     // Must null out slot
        // 队列头指针右移一位
        head = (h + 1) & (elements.length - 1);
        // 返回取得的元素
        return result;
    }
    // 从队列尾出队
    public E pollLast() {
        // 尾指针左移一位
        int t = (tail - 1) & (elements.length - 1);
        @SuppressWarnings("unchecked")
        // 取当前尾指针处元素
        E result = (E) elements[t];
        // 如果队列为空返回null
        if (result == null)
            return null;
        // 将当前尾指针处置为空
        elements[t] = null;
        // tail指向新的尾指针处
        tail = t;
        // 返回取得的元素
        return result;
    }
    
```

总结：

（1）ArrayDeque是采用数组方式实现的双端队列；

（2）ArrayDeque的出队入队是通过头尾指针循环利用数组实现的；

（3）ArrayDeque容量不足时是会扩容的，每次扩容容量增加一倍；

（4）ArrayDeque可以直接作为栈使用；

## ArrayBlockingQueue

1、ArrayBlockingQueue不需要扩容，因为是初始化时指定容量，利用takeIndex和putIndex循环利用数组； 

2、入队和出队各定义了四组方法为满足不同的用途；

| 操作 | 抛出异常  | 返回特定值      | 阻塞   | 超时                  |
| ---- | --------- | --------------- | ------ | --------------------- |
| 入队 | add(e)    | offer(e)——false | put(e) | offer(e,timeout,unit) |
| 出队 | remove()  | poll()——null    | take() | poll(timeout,unit)    |
| 检查 | element() | peek()——null    | -      | -                     |

3、利用重入锁和两个条件保证并发安全：lock、notEmpty、notFull

### 核心成员变量

```java
public class ArrayBlockingQueue<E> extends AbstractQueue<E>
        implements BlockingQueue<E>, java.io.Serializable {
    // 使用数组存储元素
    final Object[] items;
    
    // 取元素的指针
    int takeIndex;
    
    // 放元素的指针
    int putIndex;
    
    // 元素数量
    int count;
    
    // 保证并发访问的锁
    final ReentrantLock lock;
    
    // 非空条件
    private final Condition notEmpty;
    
    // 非满条件
    private final Condition notFull;
}
```

### 构造方法

```java
    public ArrayBlockingQueue(int capacity) {
        this(capacity, false);
    }
    
    public ArrayBlockingQueue(int capacity, boolean fair) {
        if (capacity <= 0)
            throw new IllegalArgumentException();
        // 初始化数组
        this.items = new Object[capacity];
        // 创建重入锁及两个条件
        lock = new ReentrantLock(fair);
        notEmpty = lock.newCondition();
        notFull =  lock.newCondition();
    }
```

### 入队

入队有四个方法，它们分别是add(E e)、offer(E e)、put(E e)、offer(E e, long timeout, TimeUnit unit)。

```java
    public boolean add(E e) {
        // 调用父类的add(e)方法
        return super.add(e);
    }
    
    // super.add(e)
    public boolean add(E e) {
        // 调用offer(e)如果成功返回true，如果失败抛出异常
        if (offer(e))
            return true;
        else
            throw new IllegalStateException("Queue full");
    }
    
    public boolean offer(E e) {
        // 元素不可为空
        checkNotNull(e);
        final ReentrantLock lock = this.lock;
        // 加锁
        lock.lock();
        try {
            if (count == items.length)
                // 如果数组满了就返回false
                return false;
            else {
                // 如果数组没满就调用入队方法并返回true
                enqueue(e);
                return true;
            }
        } finally {
            // 解锁
            lock.unlock();
        }
    }
    
    public void put(E e) throws InterruptedException {
        checkNotNull(e);
        final ReentrantLock lock = this.lock;
        // 加锁，如果线程中断了抛出异常
        lock.lockInterruptibly();
        try {
            // 如果数组满了，使用notFull等待
            while (count == items.length)
                notFull.await();
            // 入队
            enqueue(e);
        } finally {
            // 解锁
            lock.unlock();
        }
    }
    
    public boolean offer(E e, long timeout, TimeUnit unit)
        throws InterruptedException {
        checkNotNull(e);
        long nanos = unit.toNanos(timeout);
        final ReentrantLock lock = this.lock;
        // 加锁
        lock.lockInterruptibly();
        try {
            // 如果数组满了，就阻塞nanos纳秒
            // 如果唤醒这个线程时依然没有空间且时间到了就返回false
            while (count == items.length) {
                if (nanos <= 0)
                    return false;
                nanos = notFull.awaitNanos(nanos);
            }
            // 入队
            enqueue(e);
            return true;
        } finally {
            // 解锁
            lock.unlock();
        }
    }
    
    private void enqueue(E x) {
        final Object[] items = this.items;
        // 把元素直接放在放指针的位置上
        items[putIndex] = x;
        // 如果放指针到数组尽头了，就返回头部
        if (++putIndex == items.length)
            putIndex = 0;
        // 数量加1
        count++;
        // 唤醒notEmpty，因为入队了一个元素，所以肯定不为空了
        notEmpty.signal();
    }
```

### 出队

出队有四个方法，它们分别是remove()、poll()、take()、poll(long timeout, TimeUnit unit)。

```java
    public E remove() {
        // 调用poll()方法出队
        E x = poll();
        if (x != null)
            // 如果有元素出队就返回这个元素
            return x;
        else
            // 如果没有元素出队就抛出异常
            throw new NoSuchElementException();
    }
    
    public E poll() {
        final ReentrantLock lock = this.lock;
        // 加锁
        lock.lock();
        try {
            // 如果队列没有元素则返回null，否则出队
            return (count == 0) ? null : dequeue();
        } finally {
            lock.unlock();
        }
    }
    
    public E take() throws InterruptedException {
        final ReentrantLock lock = this.lock;
        // 加锁
        lock.lockInterruptibly();
        try {
            // 如果队列无元素，则阻塞等待在条件notEmpty上
            while (count == 0)
                notEmpty.await();
            // 有元素了再出队
            return dequeue();
        } finally {
            // 解锁
            lock.unlock();
        }
    }
    
    public E poll(long timeout, TimeUnit unit) throws InterruptedException {
        long nanos = unit.toNanos(timeout);
        final ReentrantLock lock = this.lock;
        // 加锁
        lock.lockInterruptibly();
        try {
            // 如果队列无元素，则阻塞等待nanos纳秒
            // 如果下一次这个线程获得了锁但队列依然无元素且已超时就返回null
            while (count == 0) {
                if (nanos <= 0)
                    return null;
                nanos = notEmpty.awaitNanos(nanos);
            }
            return dequeue();
        } finally {
            lock.unlock();
        }
    }
    
    private E dequeue() {
        final Object[] items = this.items;
        @SuppressWarnings("unchecked")
        // 取取指针位置的元素
        E x = (E) items[takeIndex];
        // 把取指针位置设为null
        items[takeIndex] = null;
        // 取指针前移，如果数组到头了就返回数组前端循环利用
        if (++takeIndex == items.length)
            takeIndex = 0;
        // 元素数量减1
        count--;
        if (itrs != null)
            itrs.elementDequeued();
        // 唤醒notFull条件
        notFull.signal();
        return x;
    }
```

总结：

1. 队列长度固定且必须在初始化时指定，所以使用之前一定要慎重考虑好容量；

2. 如果消费速度跟不上入队速度，则会导致提供者线程一直阻塞，且越阻塞越多，非常危险；

3. 只使用了一个锁来控制入队出队，效率较低

## LinkedBlockingQueue

LinkedBlockingQueue是java并发包下一个以单链表实现的阻塞队列，它是线程安全的。

### 核心成员变量

```java
public class LinkedBlockingQueue<E> extends AbstractQueue<E>
        implements BlockingQueue<E>, java.io.Serializable {
    // 容量
    private final int capacity;
    
    // 元素数量
    private final AtomicInteger count = new AtomicInteger();
    
    // 链表头,Node是典型的单链表结构
    transient Node<E> head;
    
    // 链表尾
    private transient Node<E> last;
    
    // take锁
    private final ReentrantLock takeLock = new ReentrantLock();
    
    // notEmpty条件
    // 当队列无元素时，take锁会阻塞在notEmpty条件上，等待其它线程唤醒
    private final Condition notEmpty = takeLock.newCondition();
    
    // 放锁
    private final ReentrantLock putLock = new ReentrantLock();
    
    // notFull条件
    // 当队列满了时，put锁会会阻塞在notFull上，等待其它线程唤醒
    private final Condition notFull = putLock.newCondition();
}
```

- capacity，有容量，可以理解为LinkedBlockingQueue是有界队列
- head, last，链表头、链表尾指针
- takeLock，notEmpty，take锁及其对应的条件
- putLock, notFull，put锁及其对应的条件
- 入队、出队使用两个不同的锁控制，锁分离，提高效率

### 构造方法

```java
    public LinkedBlockingQueue() {
        // 如果没传容量，就使用最大int值初始化其容量
        this(Integer.MAX_VALUE);
    }
    
    public LinkedBlockingQueue(int capacity) {
        if (capacity <= 0) throw new IllegalArgumentException();
        this.capacity = capacity;
        // 初始化head和last指针为空值节点
        last = head = new Node<E>(null);
    }
```

### 入队

```java
    public void put(E e) throws InterruptedException {
        // 不允许null元素
        if (e == null) throw new NullPointerException();
        int c = -1;
        // 新建一个节点
        Node<E> node = new Node<E>(e);
        final ReentrantLock putLock = this.putLock;
        final AtomicInteger count = this.count;
        // 使用put锁加锁
        putLock.lockInterruptibly();
        try {
            // 如果队列满了，就阻塞在notFull条件上
            // 等待被其它线程唤醒
            while (count.get() == capacity) {
                notFull.await();
            }
            // 队列不满了，就入队
            enqueue(node);
            // 队列长度加1
            c = count.getAndIncrement();
            // 如果现队列长度如果小于容量
            // 就再唤醒一个阻塞在notFull条件上的线程
            if (c + 1 < capacity)
                notFull.signal();
        } finally {
            // 释放锁
            putLock.unlock();
        }
        // 如果原队列长度为0，现在加了一个元素后立即唤醒notEmpty条件
        if (c == 0)
            signalNotEmpty();
    }
    
    private void enqueue(Node<E> node) {
        // 直接加到last后面
        last = last.next = node;
    }    
    
    private void signalNotEmpty() {
        final ReentrantLock takeLock = this.takeLock;
        // 加take锁
        takeLock.lock();
        try {
            // 唤醒notEmpty条件
            notEmpty.signal();
        } finally {
            // 解锁
            takeLock.unlock();
        }
    }
    
```

- 使用putLock加锁；

- 如果队列满了就阻塞在notFull条件上，否则就入队；

- 如果入队后元素数量小于容量，唤醒其它阻塞在notFull条件上的线程；

- 释放锁，如果放元素之前队列长度为0，就唤醒notEmpty条件；

### 出队

```java
    public E take() throws InterruptedException {
        E x;
        int c = -1;
        final AtomicInteger count = this.count;
        final ReentrantLock takeLock = this.takeLock;
        // 使用takeLock加锁
        takeLock.lockInterruptibly();
        try {
            // 如果队列无元素，则阻塞在notEmpty条件上
            while (count.get() == 0) {
                notEmpty.await();
            }
            // 否则，出队
            x = dequeue();
            // 获取出队前队列的长度
            c = count.getAndDecrement();
            // 如果取之前队列长度大于1，则唤醒notEmpty
            if (c > 1)
                notEmpty.signal();
        } finally {
            // 释放锁
            takeLock.unlock();
        }
        // 如果取之前队列长度等于容量
        // 则唤醒notFull
        if (c == capacity)
            signalNotFull();
        return x;
    }
    
    private E dequeue() {
        // head节点本身是不存储任何元素的
        // 这里把head删除，并把head下一个节点作为新的值
        // 并把其值置空，返回原来的值
        Node<E> h = head;
        Node<E> first = h.next;
        h.next = h; // help GC
        head = first;
        E x = first.item;
        first.item = null;
        return x;
    }
    
    private void signalNotFull() {
        final ReentrantLock putLock = this.putLock;
        putLock.lock();
        try {
            // 唤醒notFull
            notFull.signal();
        } finally {
            putLock.unlock();
        }
    }
```

- 使用takeLock加锁；

- 如果队列空了就阻塞在notEmpty条件上，否则就出队；

- 如果出队前元素数量大于1，唤醒其它阻塞在notEmpty条件上的线程；

- 释放锁，如果取元素之前队列长度等于容量，就唤醒notFull条件；

总结：

（1）LinkedBlockingQueue采用单链表的形式实现；

（2）LinkedBlockingQueue采用两把锁的锁分离技术实现入队出队互不阻塞；

（3）LinkedBlockingQueue是有界队列，不传入容量时默认为最大int值；

------

**LinkedBlockingQueue与ArrayBlockingQueue对比？**

a）后者入队出队采用一把锁，导致入队出队相互阻塞，效率低下；

b）前者入队出队采用两把锁，入队出队互不干扰，效率较高；

c）二者都是有界队列，如果长度相等且出队速度跟不上入队速度，都会导致大量线程阻塞；

d）前者如果初始化不传入初始容量，则使用最大int值，如果出队速度跟不上入队速度，会导致队列特别长，占用大量内存；

## SynchronousQueue

SynchronousQueue是java并发包下无缓冲阻塞队列，它用来在两个线程之间移交元素。

### 核心成员变量

```java
public class SynchronousQueue<E> extends AbstractQueue<E>
    implements BlockingQueue<E>, java.io.Serializable {
        // CPU的数量
    static final int NCPUS = Runtime.getRuntime().availableProcessors();
    // 有超时的情况自旋多少次，当CPU数量小于2的时候不自旋
    static final int maxTimedSpins = (NCPUS < 2) ? 0 : 32;
    // 没有超时的情况自旋多少次
    static final int maxUntimedSpins = maxTimedSpins * 16;
    // 针对有超时的情况，自旋了多少次后，如果剩余时间大于1000纳秒就使用带时间的LockSupport.parkNanos()这个方法
    static final long spinForTimeoutThreshold = 1000L;
    // 传输器，即两个线程交换元素使用的东西
    private transient volatile Transferer<E> transferer;
    }
```

### 主要内部类

```java
   // Transferer抽象类，主要定义了一个transfer方法用来传输元素
    abstract static class Transferer<E> {
        abstract E transfer(E e, boolean timed, long nanos);
    }
    // 以栈方式实现的Transferer
    static final class TransferStack<E> extends Transferer<E> {
        // 栈中节点的几种类型：
        // 1. 消费者（请求数据的）
        static final int REQUEST    = 0;
        // 2. 生产者（提供数据的）
        static final int DATA       = 1;
        // 3. 二者正在撮合中
        static final int FULFILLING = 2;
    
        // 栈中的节点
        static final class SNode {
            // 下一个节点
            volatile SNode next;        // next node in stack
            // 匹配者
            volatile SNode match;       // the node matched to this
            // 等待着的线程
            volatile Thread waiter;     // to control park/unpark
            // 元素
            Object item;                // data; or null for REQUESTs
            // 模式，也就是节点的类型，是消费者，是生产者，还是正在撮合中
            int mode;
        }
        // 栈的头节点
        volatile SNode head;
    }
    // 以队列方式实现的Transferer
    static final class TransferQueue<E> extends Transferer<E> {
        // 队列中的节点
        static final class QNode {
            // 下一个节点
            volatile QNode next;          // next node in queue
            // 存储的元素
            volatile Object item;         // CAS'ed to or from null
            // 等待着的线程
            volatile Thread waiter;       // to control park/unpark
            // 是否是数据节点
            final boolean isData;
        }
    
        // 队列的头节点
        transient volatile QNode head;
        // 队列的尾节点
        transient volatile QNode tail;
    }
```

### 构造方法

```java
    public SynchronousQueue() {
        // 默认非公平模式
        this(false);
    }
    
    public SynchronousQueue(boolean fair) {
        // 如果是公平模式就使用队列，如果是非公平模式就使用栈
        transferer = fair ? new TransferQueue<E>() : new TransferStack<E>();
    }
    
```

### 入队

以put为例子，调用transferer的transfer()方法，传入元素e，说明是生产者

```java
    public void put(E e) throws InterruptedException {
        // 元素不可为空
        if (e == null) throw new NullPointerException();
        // 直接调用传输器的transfer()方法
        // 三个参数分别是：传输的元素，是否需要超时，超时的时间
        if (transferer.transfer(e, false, 0) == null) {
            // 如果传输失败，直接让线程中断并抛出中断异常
            Thread.interrupted();
            throw new InterruptedException();
        }
    }
```

### 出队

以take为例子，调用transferer的transfer()方法，传入null，说明是消费者。

```java
    public E take() throws InterruptedException {
        // 直接调用传输器的transfer()方法
        // 三个参数分别是：null，是否需要超时，超时的时间
        // 第一个参数为null表示是消费者，要取元素
        E e = transferer.transfer(null, false, 0);
        // 如果取到了元素就返回
        if (e != null)
            return e;
        // 否则让线程中断并抛出中断异常
        Thread.interrupted();
        throw new InterruptedException();
    }
```

### transfer

transfer()方法同时实现了取元素和放元素的功能。**以栈为例子。**

```java
    // TransferStack.transfer()方法
    E transfer(E e, boolean timed, long nanos) {
        SNode s = null; // constructed/reused as needed
        // 根据e是否为null决定是生产者还是消费者
        int mode = (e == null) ? REQUEST : DATA;
        // 自旋+CAS
        for (;;) {
            // 栈顶元素
            SNode h = head;
            // 栈顶没有元素，或者栈顶元素跟当前元素是一个模式，均为生产者节点或消费者节点
            if (h == null || h.mode == mode) {  // empty or same-mode
                // 如果有超时而且已到期
                if (timed && nanos <= 0) {      // can't wait
                    // 如果头节点不为空且是取消状态
                    if (h != null && h.isCancelled())
                        // 就把头节点弹出，并进入下一次循环
                        casHead(h, h.next);     // pop cancelled node
                    else
                        // 否则，直接返回null（超时返回null）
                        return null;
                } else if (casHead(h, s = snode(s, e, h, mode))) {
                    // 入栈成功（因为是模式相同的，所以只能入栈）
                    // 调用awaitFulfill()方法自旋+阻塞当前入栈的线程并等待被匹配到
                    SNode m = awaitFulfill(s, timed, nanos);
                    // 如果m等于s，说明取消了，那么就把它清除掉，并返回null
                    if (m == s) {               // wait was cancelled
                        clean(s);
                        // 被取消了返回null
                        return null;
                    }
    
                    // 到这里说明匹配到元素了
                    // 因为从awaitFulfill()里面出来要不被取消了要不就匹配到了
    
                    // 如果头节点不为空，并且头节点的下一个节点是s，把栈顶两个元素都弹出了
                    if ((h = head) != null && h.next == s)
                        casHead(h, s.next);     // help s's fulfiller
                    // 根据当前节点的模式判断返回m还是s中的值
                    return (E) ((mode == REQUEST) ? m.item : s.item);
                }
            } else if (!isFulfilling(h.mode)) { // try to fulfill
                // 到这里说明头节点和当前节点模式不一样
                // 如果头节点不是正在撮合中
    
                // 如果头节点已经取消了，就把它弹出栈
                if (h.isCancelled())            // already cancelled
                    casHead(h, h.next);         // pop and retry
                else if (casHead(h, s=snode(s, e, h, FULFILLING|mode))) {
                    // 头节点没有在撮合中，就让当前节点先入队，再让他们尝试匹配
                    // 且s成为了新的头节点，它的状态是正在撮合中
                    for (;;) { // loop until matched or waiters disappear
                        SNode m = s.next;       // m is s's match
                        // 如果m为null，说明除了s节点外的节点都被其它线程先一步撮合掉了
                        // 就清空栈并跳出内部循环，到外部循环再重新入栈判断
                        if (m == null) {        // all waiters are gone
                            casHead(s, null);   // pop fulfill node
                            s = null;           // use new node next time
                            break;              // restart main loop
                        }
                        SNode mn = m.next;
                        // 如果m和s尝试撮合成功，就弹出栈顶的两个元素m和s
                        if (m.tryMatch(s)) {
                            casHead(s, mn);     // pop both s and m
                            // 返回撮合结果
                            return (E) ((mode == REQUEST) ? m.item : s.item);
                        } else                  // lost match
                            // 尝试撮合失败，说明m已经先一步被其它线程撮合了
                            // 就协助清除它
                            s.casNext(m, mn);   // help unlink
                    }
                }
            } else {                            // help a fulfiller
                // 到这里说明当前节点和头节点模式不一样
                // 且头节点是正在撮合中
    
                SNode m = h.next;               // m is h's match
                if (m == null)                  // waiter is gone
                    // 如果m为null，说明m已经被其它线程先一步撮合了
                    casHead(h, null);           // pop fulfilling node
                else {
                    SNode mn = m.next;
                    // 协助匹配，如果m和s尝试撮合成功，就弹出栈顶的两个元素m和s
                    if (m.tryMatch(h))          // help match
                        // 将栈顶的两个元素弹出后，再让s重新入栈
                        casHead(h, mn);         // pop both h and m
                    else                        // lost match
                        // 尝试撮合失败，说明m已经先一步被其它线程撮合了
                        // 就协助清除它
                        h.casNext(m, mn);       // help unlink
                }
            }
        }
    }
    
    // 三个参数：需要等待的节点，是否需要超时，超时时间
    SNode awaitFulfill(SNode s, boolean timed, long nanos) {
        // 到期时间
        final long deadline = timed ? System.nanoTime() + nanos : 0L;
        // 当前线程
        Thread w = Thread.currentThread();
        // 自旋次数
        int spins = (shouldSpin(s) ?
                     (timed ? maxTimedSpins : maxUntimedSpins) : 0);
        for (;;) {
            // 当前线程中断了，尝试清除s
            if (w.isInterrupted())
                s.tryCancel();
    
            // 检查s是否匹配到了元素m（有可能是其它线程的m匹配到当前线程的s）
            SNode m = s.match;
            // 如果匹配到了，直接返回m
            if (m != null)
                return m;
    
            // 如果需要超时
            if (timed) {
                // 检查超时时间如果小于0了，尝试清除s
                nanos = deadline - System.nanoTime();
                if (nanos <= 0L) {
                    s.tryCancel();
                    continue;
                }
            }
            if (spins > 0)
                // 如果还有自旋次数，自旋次数减一，并进入下一次自旋
                spins = shouldSpin(s) ? (spins-1) : 0;
    
            // 后面的elseif都是自旋次数没有了
            else if (s.waiter == null)
                // 如果s的waiter为null，把当前线程注入进去，并进入下一次自旋
                s.waiter = w; // establish waiter so can park next iter
            else if (!timed)
                // 如果不允许超时，直接阻塞，并等待被其它线程唤醒，唤醒后继续自旋并查看是否匹配到了元素
                LockSupport.park(this);
            else if (nanos > spinForTimeoutThreshold)
                // 如果允许超时且还有剩余时间，就阻塞相应时间
                LockSupport.parkNanos(this, nanos);
        }
    }
    
        // SNode里面的方向，调用者m是s的下一个节点
        // 这时候m节点的线程应该是阻塞状态的
        boolean tryMatch(SNode s) {
            // 如果m还没有匹配者，就把s作为它的匹配者
            if (match == null &&
                UNSAFE.compareAndSwapObject(this, matchOffset, null, s)) {
                Thread w = waiter;
                if (w != null) {    // waiters need at most one unpark
                    waiter = null;
                    // 唤醒m中的线程，两者匹配完毕
                    LockSupport.unpark(w);
                }
                // 匹配到了返回true
                return true;
            }
            // 可能其它线程先一步匹配了m，返回其是否是s
            return match == s;
        }
    
```

整个逻辑比较复杂，这里为了简单起见，屏蔽掉多线程处理的细节，只描述正常业务场景下的逻辑：

（1）如果栈中没有元素，或者栈顶元素跟将要入栈的元素模式一样，就入栈；

（2）入栈后自旋等待一会看有没有其它线程匹配到它，自旋完了还没匹配到元素就阻塞等待；

（3）阻塞等待被唤醒了说明其它线程匹配到了当前的元素，就返回匹配到的元素；

（4）如果两者模式不一样，且头节点没有在匹配中，就拿当前节点跟它匹配，匹配成功了就返回匹配到的元素；

（5）如果两者模式不一样，且头节点正在匹配中，当前线程就协助去匹配，匹配完成了再让当前节点重新入栈重新匹配；

**总结**

（1）SynchronousQueue是java里的无缓冲队列，用于在两个线程之间直接移交元素；

（2）SynchronousQueue有两种实现方式，一种是公平（队列）方式，一种是非公平（栈）方式；

（3）栈方式中的节点有三种模式：生产者、消费者、正在匹配中；

（4）栈方式的大致思路是如果栈顶元素跟自己一样的模式就入栈并等待被匹配，否则就匹配，匹配到了就返回；

------

通过源码分析，我们可以发现其实SynchronousQueue内部或者使用栈或者使用队列来存储包含线程和元素值的节点，如果同一个模式的节点过多的话，它们都会存储进来，且都会阻塞着，所以，严格上来说，SynchronousQueue并不能算是一个无缓冲队列。

## PriorityBlockingQueue

PriorityBlockingQueue是java并发包下的优先级阻塞队列，它是线程安全的。

### 核心成员变量

```java
    // 默认容量为11
    private static final int DEFAULT_INITIAL_CAPACITY = 11;
    // 最大数组大小
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
    // 存储元素的地方
    private transient Object[] queue;
    // 元素个数
    private transient int size;
    // 比较器
    private transient Comparator<? super E> comparator;
    // 重入锁
    private final ReentrantLock lock;
    // 非空条件
    private final Condition notEmpty;
    // 扩容的时候使用的控制变量，CAS更新这个值，谁更新成功了谁扩容，其它线程让出CPU
    private transient volatile int allocationSpinLock;
    // 不阻塞的优先级队列，非存储元素的地方，仅用于序列化/反序列化时
    private PriorityQueue<E> q;
```

### 构造方法

```java
   // 默认容量为11
    public PriorityBlockingQueue() {
        this(DEFAULT_INITIAL_CAPACITY, null);
    }
    // 传入初始容量
    public PriorityBlockingQueue(int initialCapacity) {
        this(initialCapacity, null);
    }
    // 传入初始容量和比较器
    // 初始化各变量
    public PriorityBlockingQueue(int initialCapacity,
                                 Comparator<? super E> comparator) {
        if (initialCapacity < 1)
            throw new IllegalArgumentException();
        this.lock = new ReentrantLock();
        this.notEmpty = lock.newCondition();
        this.comparator = comparator;
        this.queue = new Object[initialCapacity];
    }
```

### 入队

以offer(E e)为例子。

```java
   public boolean offer(E e) {
        // 元素不能为空
        if (e == null)
            throw new NullPointerException();
        final ReentrantLock lock = this.lock;
        // 加锁
        lock.lock();
        int n, cap;
        Object[] array;
        // 判断是否需要扩容，即元素个数达到了数组容量
        while ((n = size) >= (cap = (array = queue).length))
            tryGrow(array, cap);
        try {
            Comparator<? super E> cmp = comparator;
            // 根据是否有比较器选择不同的方法
            if (cmp == null)
                siftUpComparable(n, e, array);
            else
                siftUpUsingComparator(n, e, array, cmp);
            // 插入元素完毕，元素个数加1            
            size = n + 1;
            // 唤醒notEmpty条件
            notEmpty.signal();
        } finally {
            // 解锁
            lock.unlock();
        }
        return true;
    }
    
    private static <T> void siftUpComparable(int k, T x, Object[] array) {
        Comparable<? super T> key = (Comparable<? super T>) x;
        while (k > 0) {
            // 取父节点
            int parent = (k - 1) >>> 1;
            // 父节点的元素值
            Object e = array[parent];
            // 如果key大于父节点，堆化结束
            if (key.compareTo((T) e) >= 0)
                break;
            // 否则，交换二者的位置，继续下一轮比较
            array[k] = e;
            k = parent;
        }
        // 找到了应该放的位置，放入元素
        array[k] = key;
    }
    
```

1. 加锁，判断是否需要扩容；

2. 添加元素并做自下而上的堆化；

3. 元素个数加1并唤醒notEmpty条件，唤醒取元素的线程，解锁；

### 扩容

```java
    private void tryGrow(Object[] array, int oldCap) {
        // 先释放锁，因为是从offer()方法的锁内部过来的
        // 这里先释放锁，使用allocationSpinLock变量控制扩容的过程
        // 防止阻塞的线程过多
        lock.unlock(); // must release and then re-acquire main lock
        Object[] newArray = null;
        // CAS更新allocationSpinLock变量为1的线程获得扩容资格
        if (allocationSpinLock == 0 &&
            UNSAFE.compareAndSwapInt(this, allocationSpinLockOffset,
                                     0, 1)) {
            try {
                // 旧容量小于64则翻倍，旧容量大于64则增加一半
                int newCap = oldCap + ((oldCap < 64) ?
                                       (oldCap + 2) : // grow faster if small
                                       (oldCap >> 1));
                // 判断新容量是否溢出
                if (newCap - MAX_ARRAY_SIZE > 0) {    // possible overflow
                    int minCap = oldCap + 1;
                    if (minCap < 0 || minCap > MAX_ARRAY_SIZE)
                        throw new OutOfMemoryError();
                    newCap = MAX_ARRAY_SIZE;
                }
                // 创建新数组
                if (newCap > oldCap && queue == array)
                    newArray = new Object[newCap];
            } finally {
                // 相当于解锁
                allocationSpinLock = 0;
            }
        }
        // 只有进入了上面条件的才会满足这个条件
        // 意思是让其它线程让出CPU
        if (newArray == null) // back off if another thread is allocating
            Thread.yield();
        // 再次加锁
        lock.lock();
        // 判断新数组创建成功并且旧数组没有被替换过
        if (newArray != null && queue == array) {
            // 队列赋值为新数组
            queue = newArray;
            // 并拷贝旧数组元素到新数组中
            System.arraycopy(array, 0, newArray, 0, oldCap);
        }
    }
    
```

（1）解锁，解除offer()方法中加的锁；

（2）使用allocationSpinLock变量的CAS操作来控制扩容的过程；

（3）旧容量小于64则翻倍，旧容量大于64则增加一半，创建新数组；

（4）修改allocationSpinLock为0，相当于解锁；其它线程在扩容的过程中要让出CPU；

（5）再次加锁；新数组创建成功，把旧数组元素拷贝过来，并返回到offer()方法中继续添加元素操作；

### 出队

分析take()方法。

```java
   public E take() throws InterruptedException {
        final ReentrantLock lock = this.lock;
        // 加锁
        lock.lockInterruptibly();
        E result;
        try {
            // 队列没有元素，就阻塞在notEmpty条件上
            // 出队成功，就跳出这个循环
            while ( (result = dequeue()) == null)
                notEmpty.await();
        } finally {
            // 解锁
            lock.unlock();
        }
        // 返回出队的元素
        return result;
    }
    
    private E dequeue() {
        // 元素个数减1
        int n = size - 1;
        if (n < 0)
            // 数组元素不足，返回null
            return null;
        else {
            Object[] array = queue;
            // 弹出堆顶元素
            E result = (E) array[0];
            // 把堆尾元素拿到堆顶
            E x = (E) array[n];
            array[n] = null;
            Comparator<? super E> cmp = comparator;
            // 并做自上而下的堆化
            if (cmp == null)
                siftDownComparable(0, x, array, n);
            else
                siftDownUsingComparator(0, x, array, n, cmp);
            // 修改size
            size = n;
            // 返回出队的元素
            return result;
        }
    }
    
    private static <T> void siftDownComparable(int k, T x, Object[] array,
                                               int n) {
        if (n > 0) {
            Comparable<? super T> key = (Comparable<? super T>)x;
            int half = n >>> 1;           // loop while a non-leaf
            // 只需要遍历到叶子节点就够了
            while (k < half) {
                // 左子节点
                int child = (k << 1) + 1; // assume left child is least
                // 左子节点的值
                Object c = array[child];
                // 右子节点
                int right = child + 1;
                // 取左右子节点中最小的值
                if (right < n &&
                    ((Comparable<? super T>) c).compareTo((T) array[right]) > 0)
                    c = array[child = right];
                // key如果比左右子节点都小，则堆化结束
                if (key.compareTo((T) c) <= 0)
                    break;
                // 否则，交换key与左右子节点中最小的节点的位置
                array[k] = c;
                k = child;
            }
            // 找到了放元素的位置，放置元素
            array[k] = key;
        }
    }
    
```

（1）加锁；判断是否出队成功，未成功就阻塞在notEmpty条件上；

（2）出队时弹出堆顶元素，并把堆尾元素拿到堆顶；

（3）再做自上而下的堆化；解锁；

**总结**

（1）PriorityBlockingQueue整个入队出队的过程与PriorityQueue基本是保持一致的；

（2）PriorityBlockingQueue使用一个锁+一个notEmpty条件控制并发安全；

（3）PriorityBlockingQueue扩容时使用一个单独变量的CAS操作来控制只有一个线程进行扩容；

（4）入队使用自下而上的堆化；出队使用自上而下的堆化；

## LinkedTransferQueue

LinkedTransferQueue是LinkedBlockingQueue、SynchronousQueue（公平模式）、ConcurrentLinkedQueue三者的集合体，它综合了这三者的方法，并且提供了更加高效的实现方式。

**TransferQueue接口中定义了以下几个方法：**

```java
    // 尝试移交元素
    boolean tryTransfer(E e);
    // 移交元素
    void transfer(E e) throws InterruptedException;
    // 尝试移交元素（有超时时间）
    boolean tryTransfer(E e, long timeout, TimeUnit unit)
        throws InterruptedException;
    // 判断是否有消费者
    boolean hasWaitingConsumer();
    // 查看消费者的数量
    int getWaitingConsumerCount();
```

主要是定义了三个移交元素的方法，有阻塞的，有不阻塞的，有超时的。

### 存储结构

LinkedTransferQueue使用了一个叫做`dual data structure`的数据结构，或者叫做`dual queue`，译为双重数据结构或者双重队列。

放取元素使用同一个队列，队列中的节点具有两种模式，一种是数据节点，一种是非数据节点。

放元素时先跟队列头节点对比，如果头节点是非数据节点，就让他们匹配，如果头节点是数据节点，就生成一个数据节点放在队列尾端（入队）。

取元素时也是先跟队列头节点对比，如果头节点是数据节点，就让他们匹配，如果头节点是非数据节点，就生成一个非数据节点放在队列尾端（入队）。

**不管是放元素还是取元素，都先跟头节点对比，如果二者模式不一样就匹配它们，如果二者模式一样，就入队。**

![202105091521058242.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091521058242.png)

### 核心成员变量

```java
    // 头节点
    transient volatile Node head;
    // 尾节点
    private transient volatile Node tail;
    // 放取元素的几种方式：
    // 立即返回，用于非超时的poll()和tryTransfer()方法中
    private static final int NOW   = 0; // for untimed poll, tryTransfer
    // 异步，不会阻塞，用于放元素时，因为内部使用无界单链表存储元素，不会阻塞放元素的过程
    private static final int ASYNC = 1; // for offer, put, add
    // 同步，调用的时候如果没有匹配到会阻塞直到匹配到为止
    private static final int SYNC  = 2; // for transfer, take
    // 超时，用于有超时的poll()和tryTransfer()方法中
    private static final int TIMED = 3; // for timed poll, tryTransfer
```

**主要内部类**

```java
    static final class Node {
        // 是否是数据节点（也就标识了是生产者还是消费者）
        final boolean isData;   // false if this is a request node
        // 元素的值
        volatile Object item;   // initially non-null if isData; CASed to match
        // 下一个节点
        volatile Node next;
        // 持有元素的线程
        volatile Thread waiter; // null until waiting
    }
```

### 构造方法

```java
    public LinkedTransferQueue() {
    }
    
    public LinkedTransferQueue(Collection<? extends E> c) {
        this();
        addAll(c);
    }
    
```

### 入队

四个方法都是一样的，使用异步的方式调用xfer()方法，传入的参数都一模一样。

```java
    public void put(E e) {
        // 异步模式，不会阻塞，不会超时
        // 因为是放元素，单链表存储，会一直往后加
        xfer(e, true, ASYNC, 0);
    }
    
    public boolean offer(E e, long timeout, TimeUnit unit) {
        xfer(e, true, ASYNC, 0);
        return true;
    }
    
    public boolean offer(E e) {
        xfer(e, true, ASYNC, 0);
        return true;
    }
    
    public boolean add(E e) {
        xfer(e, true, ASYNC, 0);
        return true;
    }
```

xfer(E e, boolean haveData, int how, long nanos)的参数分别是：

（1）e表示元素；

（2）haveData表示是否是数据节点，

（3）how表示放取元素的方式，上面提到的四种，NOW、ASYNC、SYNC、TIMED；

（4）nanos表示超时时间；

### 出队

出队的四个方法也是直接或间接的调用xfer()方法，放取元素的方式和超时规则略微不同，本质没有大的区别。

```java
    public E remove() {
        E x = poll();
        if (x != null)
            return x;
        else
            throw new NoSuchElementException();
    }
    public E take() throws InterruptedException {
        // 同步模式，会阻塞直到取到元素
        E e = xfer(null, false, SYNC, 0);
        if (e != null)
            return e;
        Thread.interrupted();
        throw new InterruptedException();
    }
    
    public E poll(long timeout, TimeUnit unit) throws InterruptedException {
        // 有超时时间
        E e = xfer(null, false, TIMED, unit.toNanos(timeout));
        if (e != null || !Thread.interrupted())
            return e;
        throw new InterruptedException();
    }
    
    public E poll() {
        // 立即返回，没取到元素返回null
        return xfer(null, false, NOW, 0);
    }
```

取元素就各有各的玩法了，有同步的，有超时的，有立即返回的。

### 移交元素

```java
    public boolean tryTransfer(E e) {
        // 立即返回
        return xfer(e, true, NOW, 0) == null;
    }
    
    public void transfer(E e) throws InterruptedException {
        // 同步模式
        if (xfer(e, true, SYNC, 0) != null) {
            Thread.interrupted(); // failure possible only due to interrupt
            throw new InterruptedException();
        }
    }
    
    public boolean tryTransfer(E e, long timeout, TimeUnit unit)
        throws InterruptedException {
        // 有超时时间
        if (xfer(e, true, TIMED, unit.toNanos(timeout)) == null)
            return true;
        if (!Thread.interrupted())
            return false;
        throw new InterruptedException();
    }
```

请注意第二个参数，都是true，也就是这三个方法其实也是放元素的方法。

### xfer()方法

```java
    private E xfer(E e, boolean haveData, int how, long nanos) {
        // 不允许放入空元素
        if (haveData && (e == null))
            throw new NullPointerException();
        Node s = null;                        // the node to append, if needed
        // 外层循环，自旋，失败就重试
        retry:
        for (;;) {                            // restart on append race
    
            // 下面这个for循环用于控制匹配的过程
            // 同一时刻队列中只会存储一种类型的节点
            // 从头节点开始尝试匹配，如果头节点被其它线程先一步匹配了
            // 就再尝试其下一个，直到匹配到为止，或者到队列中没有元素为止
    
            for (Node h = head, p = h; p != null;) { // find & match first node
                // p节点的模式
                boolean isData = p.isData;
                // p节点的值
                Object item = p.item;
                // p没有被匹配到
                if (item != p && (item != null) == isData) { // unmatched
                    // 如果两者模式一样，则不能匹配，跳出循环后尝试入队
                    if (isData == haveData)   // can't match
                        break;
                    // 如果两者模式不一样，则尝试匹配
                    // 把p的值设置为e（如果是取元素则e是null，如果是放元素则e是元素值）
                    if (p.casItem(item, e)) { // match
                        // 匹配成功
                        // for里面的逻辑比较复杂，用于控制多线程同时放取元素时出现竞争的情况的
                        // 看不懂可以直接跳过
                        for (Node q = p; q != h;) {
                            // 进入到这里可能是头节点已经被匹配，然后p会变成h的下一个节点
                            Node n = q.next;  // update by 2 unless singleton
                            // 如果head还没变，就把它更新成新的节点
                            // 并把它删除（forgetNext()会把它的next设为自己，也就是从单链表中删除了）
                            // 这时为什么要把head设为n呢？因为到这里了，肯定head本身已经被匹配掉了
                            // 而上面的p.casItem()又成功了，说明p也被当前这个元素给匹配掉了
                            // 所以需要把它们俩都出队列，让其它线程可以从真正的头开始，不用重复检查了
                            if (head == h && casHead(h, n == null ? q : n)) {
                                h.forgetNext();
                                break;
                            }                 // advance and retry
                            // 如果新的头节点为空，或者其next为空，或者其next未匹配，就重试
                            if ((h = head)   == null ||
                                (q = h.next) == null || !q.isMatched())
                                break;        // unless slack < 2
                        }
                        // 唤醒p中等待的线程
                        LockSupport.unpark(p.waiter);
                        // 并返回匹配到的元素
                        return LinkedTransferQueue.<E>cast(item);
                    }
                }
                // p已经被匹配了或者尝试匹配的时候失败了
                // 也就是其它线程先一步匹配了p
                // 这时候又分两种情况，p的next还没来得及修改，p的next指向了自己
                // 如果p的next已经指向了自己，就重新取head重试，否则就取其next重试
                Node n = p.next;
                p = (p != n) ? n : (h = head); // Use head if p offlist
            }
    
            // 到这里肯定是队列中存储的节点类型和自己一样
            // 或者队列中没有元素了
            // 就入队（不管放元素还是取元素都得入队）
            // 入队又分成四种情况：
            // NOW，立即返回，没有匹配到立即返回，不做入队操作
            // ASYNC，异步，元素入队但当前线程不会阻塞（相当于无界LinkedBlockingQueue的元素入队）
            // SYNC，同步，元素入队后当前线程阻塞，等待被匹配到
            // TIMED，有超时，元素入队后等待一段时间被匹配，时间到了还没匹配到就返回元素本身
    
            // 如果不是立即返回
            if (how != NOW) {                 // No matches available
                // 新建s节点
                if (s == null)
                    s = new Node(e, haveData);
                // 尝试入队
                Node pred = tryAppend(s, haveData);
                // 入队失败，重试
                if (pred == null)
                    continue retry;           // lost race vs opposite mode
                // 如果不是异步（同步或者有超时）
                // 就等待被匹配
                if (how != ASYNC)
                    return awaitMatch(s, pred, e, (how == TIMED), nanos);
            }
            return e; // not waiting
        }
    }
    
    private Node tryAppend(Node s, boolean haveData) {
        // 从tail开始遍历，把s放到链表尾端
        for (Node t = tail, p = t;;) {        // move p to last node and append
            Node n, u;                        // temps for reads of next & tail
            // 如果首尾都是null，说明链表中还没有元素
            if (p == null && (p = head) == null) {
                // 就让首节点指向s
                // 注意，这里插入第一个元素的时候tail指针并没有指向s
                if (casHead(null, s))
                    return s;                 // initialize
            }
            else if (p.cannotPrecede(haveData))
                // 如果p无法处理，则返回null
                // 这里无法处理的意思是，p和s节点的类型不一样，不允许s入队
                // 比如，其它线程先入队了一个数据节点，这时候要入队一个非数据节点，就不允许，
                // 队列中所有的元素都要保证是同一种类型的节点
                // 返回null后外面的方法会重新尝试匹配重新入队等
                return null;                  // lost race vs opposite mode
            else if ((n = p.next) != null)    // not last; keep traversing
                // 如果p的next不为空，说明不是最后一个节点
                // 则让p重新指向最后一个节点
                p = p != t && t != (u = tail) ? (t = u) : // stale tail
                    (p != n) ? n : null;      // restart if off list
            else if (!p.casNext(null, s))
                // 如果CAS更新s为p的next失败
                // 则说明有其它线程先一步更新到p的next了
                // 就让p指向p的next，重新尝试让s入队
                p = p.next;                   // re-read on CAS failure
            else {
                // 到这里说明s成功入队了
                // 如果p不等于t，就更新tail指针
                // 还记得上面插入第一个元素时tail指针并没有指向新元素吗？
                // 这里就是用来更新tail指针的
                if (p != t) {                 // update if slack now >= 2
                    while ((tail != t || !casTail(t, s)) &&
                           (t = tail)   != null &&
                           (s = t.next) != null && // advance and retry
                           (s = s.next) != null && s != t);
                }
                // 返回p，即s的前一个元素
                return p;
            }
        }
    }
    
    private E awaitMatch(Node s, Node pred, E e, boolean timed, long nanos) {
        // 如果是有超时的，计算其超时时间
        final long deadline = timed ? System.nanoTime() + nanos : 0L;
        // 当前线程
        Thread w = Thread.currentThread();
        // 自旋次数
        int spins = -1; // initialized after first item and cancel checks
        // 随机数，随机让一些自旋的线程让出CPU
        ThreadLocalRandom randomYields = null; // bound if needed
    
        for (;;) {
            Object item = s.item;
            // 如果s元素的值不等于e，说明它被匹配到了
            if (item != e) {                  // matched
                // assert item != s;
                // 把s的item更新为s本身
                // 并把s中的waiter置为空
                s.forgetContents();           // avoid garbage
                // 返回匹配到的元素
                return LinkedTransferQueue.<E>cast(item);
            }
            // 如果当前线程中断了，或者有超时的到期了
            // 就更新s的元素值指向s本身
            if ((w.isInterrupted() || (timed && nanos <= 0)) &&
                    s.casItem(e, s)) {        // cancel
                // 尝试解除s与其前一个节点的关系
                // 也就是删除s节点
                unsplice(pred, s);
                // 返回元素的值本身，说明没匹配到
                return e;
            }
    
            // 如果自旋次数小于0，就计算自旋次数
            if (spins < 0) {                  // establish spins at/near front
                // spinsFor()计算自旋次数
                // 如果前面有节点未被匹配就返回0
                // 如果前面有节点且正在匹配中就返回一定的次数，等待
                if ((spins = spinsFor(pred, s.isData)) > 0)
                    // 初始化随机数
                    randomYields = ThreadLocalRandom.current();
            }
            else if (spins > 0) {             // spin
                // 还有自旋次数就减1
                --spins;
                // 并随机让出CPU
                if (randomYields.nextInt(CHAINED_SPINS) == 0)
                    Thread.yield();           // occasionally yield
            }
            else if (s.waiter == null) {
                // 更新s的waiter为当前线程
                s.waiter = w;                 // request unpark then recheck
            }
            else if (timed) {
                // 如果有超时，计算超时时间，并阻塞一定时间
                nanos = deadline - System.nanoTime();
                if (nanos > 0L)
                    LockSupport.parkNanos(this, nanos);
            }
            else {
                // 不是超时的，直接阻塞，等待被唤醒
                // 唤醒后进入下一次循环，走第一个if的逻辑就返回匹配的元素了
                LockSupport.park(this);
            }
        }
    }
```

这三个方法里的内容特别复杂，很大一部分代码都是在控制线程安全，各种CAS，我们这里简单描述一下大致的逻辑：

（1）来了一个元素，我们先查看队列头的节点，是否与这个元素的模式一样；

（2）如果模式不一样，就尝试让他们匹配，如果头节点被别的线程先匹配走了，就尝试与头节点的下一个节点匹配，如此一直往后，直到匹配到或到链表尾为止；

（3）如果模式一样，或者到链表尾了，就尝试入队；

（4）入队的时候有可能链表尾修改了，那就尾指针后移，再重新尝试入队，依此往复；

（5）入队成功了，就自旋或阻塞，阻塞了就等待被其它线程匹配到并唤醒；

（6）唤醒之后进入下一次循环就匹配到元素了，返回匹配到的元素；

（7）是否需要入队及阻塞有四种情况：

```java
   a）NOW，立即返回，没有匹配到立即返回，不做入队操作
    
        对应的方法有：poll()、tryTransfer(e)
    
    b）ASYNC，异步，元素入队但当前线程不会阻塞（相当于无界LinkedBlockingQueue的元素入队）
    
        对应的方法有：add(e)、offer(e)、put(e)、offer(e, timeout, unit)
    
    c）SYNC，同步，元素入队后当前线程阻塞，等待被匹配到
    
        对应的方法有：take()、transfer(e)
    
    d）TIMED，有超时，元素入队后等待一段时间被匹配，时间到了还没匹配到就返回元素本身
    
        对应的方法有：poll(timeout, unit)、tryTransfer(e, timeout, unit)
```

### 总结

（1）LinkedTransferQueue可以看作LinkedBlockingQueue、SynchronousQueue（公平模式）、ConcurrentLinkedQueue三者的集合体；

（2）LinkedTransferQueue的实现方式是使用一种叫做`双重队列`的数据结构；

（3）不管是取元素还是放元素都会入队；

（4）先尝试跟头节点比较，如果二者模式不一样，就匹配它们，组成CP，然后返回对方的值；

（5）如果二者模式一样，就入队，并自旋或阻塞等待被唤醒；

（6）至于是否入队及阻塞有四种模式，NOW、ASYNC、SYNC、TIMED；

（7）LinkedTransferQueue全程都没有使用synchronized、重入锁等比较重的锁，基本是通过 自旋+CAS 实现；

（8）对于入队之后，先自旋一定次数后再调用LockSupport.park()或LockSupport.parkNanos阻塞；

## ConcurrentLinkedQueue

ConcurrentLinkedQueue只实现了Queue接口，并没有实现BlockingQueue接口，所以它不是阻塞队列，也不能用于线程池中，但是它是线程安全的，可用于多线程环境中。

### 核心成员变量

```java
    // 链表头节点
    private transient volatile Node<E> head;
    // 链表尾节点
    private transient volatile Node<E> tail;
```

### 主要内部类

```java
    private static class Node<E> {
        volatile E item;
        volatile Node<E> next;
    }
```

### 构造方法

```java
      public ConcurrentLinkedQueue() {
        // 初始化头尾节点
        head = tail = new Node<E>(null);
    }
    
    public ConcurrentLinkedQueue(Collection<? extends E> c) {
        Node<E> h = null, t = null;
        // 遍历c，并把它元素全部添加到单链表中
        for (E e : c) {
            checkNotNull(e);
            Node<E> newNode = new Node<E>(e);
            if (h == null)
                h = t = newNode;
            else {
                t.lazySetNext(newNode);
                t = newNode;
            }
        }
        if (h == null)
            h = t = new Node<E>(null);
        head = h;
        tail = t;
    }
    
```

可看出这是一个无界的单链表实现的队列。

### 入队

因为它不是阻塞队列，所以只有两个入队的方法，add(e)和offer(e)。

因为是无界队列，所以add(e)方法也不用抛出异常了。

```java
   public boolean add(E e) {
        return offer(e);
    }
    
    public boolean offer(E e) {
        // 不能添加空元素
        checkNotNull(e);
        // 新节点
        final Node<E> newNode = new Node<E>(e);
    
        // 入队到链表尾
        for (Node<E> t = tail, p = t;;) {
            Node<E> q = p.next;
            // 如果没有next，说明到链表尾部了，就入队
            if (q == null) {
                // CAS更新p的next为新节点
                // 如果成功了，就返回true
                // 如果不成功就重新取next重新尝试
                if (p.casNext(null, newNode)) {
                    // 如果p不等于t，说明有其它线程先一步更新tail
                    // 也就不会走到q==null这个分支了
                    // p取到的可能是t后面的值
                    // 把tail原子更新为新节点
                    if (p != t) // hop two nodes at a time
                        casTail(t, newNode);  // Failure is OK.
                    // 返回入队成功
                    return true;
                }
            }
            else if (p == q)
                // 如果p的next等于p，说明p已经被删除了（已经出队了）
                // 重新设置p的值
                p = (t != (t = tail)) ? t : head;
            else
                // t后面还有值，重新设置p的值
                p = (p != t && t != (t = tail)) ? t : q;
        }
    }
```

（1）定位到链表尾部，尝试把新节点到后面；

（2）如果尾部变化了，则重新获取尾部，再重试；

### 出队

因为它不是阻塞队列，所以只有两个出队的方法，remove()和poll()。

```java
   public E remove() {
        E x = poll();
        if (x != null)
            return x;
        else
            throw new NoSuchElementException();
    }
    
    public E poll() {
        restartFromHead:
        for (;;) {
            // 尝试弹出链表的头节点
            for (Node<E> h = head, p = h, q;;) {
                E item = p.item;
                // 如果节点的值不为空，并且将其更新为null成功了
                if (item != null && p.casItem(item, null)) {
                    // 如果头节点变了，则不会走到这个分支
                    // 会先走下面的分支拿到新的头节点
                    // 这时候p就不等于h了，就更新头节点
                    // 在updateHead()中会把head更新为新节点
                    // 并让head的next指向其自己
                    if (p != h) // hop two nodes at a time
                        updateHead(h, ((q = p.next) != null) ? q : p);
                    // 上面的casItem()成功，就可以返回出队的元素了
                    return item;
                }
                // 下面三个分支说明头节点变了
                // 且p的item肯定为null
                else if ((q = p.next) == null) {
                    // 如果p的next为空，说明队列中没有元素了
                    // 更新h为p，也就是空元素的节点
                    updateHead(h, p);
                    // 返回null
                    return null;
                }
                else if (p == q)
                    // 如果p等于p的next，说明p已经出队了，重试
                    continue restartFromHead;
                else
                    // 将p设置为p的next
                    p = q;
            }
        }
    }
    // 更新头节点的方法
    final void updateHead(Node<E> h, Node<E> p) {
        // 原子更新h为p成功后，延迟更新h的next为它自己
        // 这里用延迟更新是安全的，因为head节点已经变了
        // 只要入队出队的时候检查head有没有变化就行了，跟它的next关系不大
        if (h != p && casHead(h, p))
            h.lazySetNext(h);
    }
    
```

出队的整个逻辑也是比较清晰的：

（1）定位到头节点，尝试更新其值为null；

（2）若成功了则出队；若失败或者头节点变化了，就重新寻找头节点，并重试；

（3）整个出队过程没有一点阻塞相关的代码，所以出队的时候不会阻塞线程，没找到元素就返回null；

**总结**

（1）ConcurrentLinkedQueue不是阻塞队列；

（2）ConcurrentLinkedQueue不能用在线程池中；

（3）ConcurrentLinkedQueue使用（CAS+自旋）更新头尾节点控制出队入队操作；

**ConcurrentLinkedQueue与LinkedBlockingQueue对比？**

（1）两者都是线程安全的队列；

（2）两者都可以实现取元素时队列为空直接返回null，后者的poll()方法可以实现此功能；

（3）前者全程无锁，后者全部都是使用重入锁控制的；

（4）前者效率较高，后者效率较低；

（5）前者无法实现如果队列为空等待元素到来的操作；

（6）前者是非阻塞队列，后者是阻塞队列；

（7）前者无法用在线程池中，后者可以；

## DelayQueue

DelayQueue是java并发包下的延时阻塞队列，常用于实现定时任务。

### 核心成员变量

```java
public class DelayQueue<E extends Delayed> extends AbstractQueue<E>
    implements BlockingQueue<E> {
   
    private final transient ReentrantLock lock = new ReentrantLock(); // 并发控制锁
    private final PriorityQueue<E> q = new PriorityQueue<E>(); // 优先级队列
 
    private Thread leader;  // 在取元素时，用于标记是否有线程在排队

    private final Condition available = lock.newCondition(); // 条件锁，用于可取条件
}

// Delayed接口
    public interface Delayed extends Comparable<Delayed> {
    
        long getDelay(TimeUnit unit);
    }
```

### 构造方法

```java
    public DelayQueue() {}
    
    public DelayQueue(Collection<? extends E> c) {
        this.addAll(c);
    }
```

### 入队

因为DelayQueue是阻塞队列，且优先级队列是无界的，所以入队不会阻塞不会超时，因此它的四个入队方法是一样的。

```java
    public boolean add(E e) {
        return offer(e);
    }
    
    public void put(E e) {
        offer(e);
    }
    
    public boolean offer(E e, long timeout, TimeUnit unit) {
        return offer(e);
    }
    
    public boolean offer(E e) {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            q.offer(e);
            if (q.peek() == e) {
                leader = null;
                available.signal();
            }
            return true;
        } finally {
            lock.unlock();
        }
    }
    
```

- 加锁，添加元素到优先级队列中；

- 若添加为堆顶元素，则把leader置为空，并唤醒等待在条件available上的线程，解锁；

### 出队

因为DelayQueue是阻塞队列，所以它的出队有四个不同的方法，有抛出异常的，有阻塞的，有不阻塞的，有超时的。主要分析poll()和take()。

**poll**

```java
    public E poll() {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            E first = q.peek();
            if (first == null || first.getDelay(NANOSECONDS) > 0)
                return null;
            else
                return q.poll();
        } finally {
            lock.unlock();
        }
    }
```

- 加锁，检查第一个元素，如果为空或者还没到期，就返回null；

- 如果第一个元素到期了就调用poll()弹出第一个元素，解锁。

**take**

```java
  public E take() throws InterruptedException {
        final ReentrantLock lock = this.lock;
        lock.lockInterruptibly();
        try {
            for (;;) {
                // 堆顶元素
                E first = q.peek();
                // 如果堆顶元素为空，说明队列中还没有元素，直接阻塞等待
                if (first == null)
                    available.await();
                else {
                    // 堆顶元素的到期时间
                    long delay = first.getDelay(NANOSECONDS);
                    // 如果小于0说明已到期，直接调用poll()方法弹出堆顶元素
                    if (delay <= 0)
                        return q.poll();
    
                    // 如果delay大于0 ，则下面要阻塞了
    
                    // 将first置为空方便gc，因为有可能其它元素弹出了这个元素
                    // 这里还持有着引用不会被清理
                    first = null; // don't retain ref while waiting
                    // 如果前面有其它线程在等待，直接进入等待
                    if (leader != null)
                        available.await();
                    else {
                        // 如果leader为null，把当前线程赋值给它
                        Thread thisThread = Thread.currentThread();
                        leader = thisThread;
                        try {
                            // 等待delay时间后自动醒过来，醒过来后把leader置空并重新进入循环判断堆顶元素是否到期
                            // 这里即使醒过来后也不一定能获取到元素
                            // 因为有可能其它线程先一步获取了锁并弹出了堆顶元素
                            // 在AQS的队列中，当其它线程调用LockSupport.unpark(t)的时候才会真正唤醒
                            available.awaitNanos(delay);
                        } finally {
                            // 如果leader还是当前线程就把它置为空，让其它线程有机会获取元素
                            if (leader == thisThread)
                                leader = null;
                        }
                    }
                }
            }
        } finally {
            // 成功出队后，如果leader为空且堆顶还有元素，就唤醒下一个等待的线程
            if (leader == null && q.peek() != null)
                // signal()只是把等待的线程放到AQS的队列里面，并不是真正的唤醒
                available.signal();
            // 解锁，这才是真正的唤醒
            lock.unlock();
        }
    }
```

- 加锁，判断堆顶元素是否为空，为空的话直接阻塞等待；

- 判断堆顶元素是否到期，到期了直接poll()出元素；没到期，再判断前面是否有其它线程在等待，有则直接等待；
- 前面没有其它线程在等待，则把自己当作第一个线程等待delay时间后唤醒，再尝试获取元素；
- 获取到元素之后再唤醒下一个等待的线程，解锁；

总结：

（1）DelayQueue是阻塞队列；

（2）DelayQueue内部存储结构使用优先级队列；

（3）DelayQueue使用重入锁和条件来控制并发安全；

（4）DelayQueue常用于定时任务；

**例子**

```java
    public class DelayQueueTest {
        public static void main(String[] args) {
            DelayQueue<Message> queue = new DelayQueue<>();
    
            long now = System.currentTimeMillis();
    
            // 启动一个线程从队列中取元素
            new Thread(()->{
                while (true) {
                    try {
                        // 将依次打印1000，2000，5000，7000，8000
                        System.out.println(queue.take().deadline - now);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }).start();
    
            // 添加5个元素到队列中
            queue.add(new Message(now + 5000));
            queue.add(new Message(now + 8000));
            queue.add(new Message(now + 2000));
            queue.add(new Message(now + 1000));
            queue.add(new Message(now + 7000));
        }
    }
    
    class Message implements Delayed {
        long deadline;
    
        public Message(long deadline) {
            this.deadline = deadline;
        }
    
        @Override
        public long getDelay(TimeUnit unit) {
            return deadline - System.currentTimeMillis();
        }
    
        @Override
        public int compareTo(Delayed o) {
            return (int) (getDelay(TimeUnit.MILLISECONDS) - o.getDelay(TimeUnit.MILLISECONDS));
        }
    
        @Override
        public String toString() {
            return String.valueOf(deadline);
        }
    }
    
```



# 三、Java并发

## synchronized

> synchronized可以保证方法或者代码块在运行时，同一时刻只有一个方法可以进入到临界区，同时它还可以保证共享变量的内存可见性

Java对象头和monitor是实现synchronized的基础，Java中每一个对象都可以作为锁：

1. 普通同步方法，锁是当前实例对象
2. 静态同步方法，锁是当前类的class对象
3. 同步方法块，锁是括号里面的对象

同步代码块是使用monitorenter和monitorexit指令实现的，同步方法、依靠的是方法修饰符上的ACC_SYNCHRONIZED实现。

### Java对象头和Monitor

synchronized用的锁是存在Java对象头里的，otspot虚拟机的对象头主要包括两部分数据：Mark Word（标记字段）、Klass Pointer（类型指针）。其中Klass Point是是对象指向它的类元数据的指针，虚拟机通过这个指针来确定这个对象是哪个类的实例，Mark Word用于存储对象自身的运行时数据，它是实现轻量级锁和偏向锁的关键。

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/279759ee3d6d55fb47b0b6593cb0b54f21a4ddf0.jpeg)

每个Java对象都是Monitor，每一个被锁住的对象都会和一个monitor关联，同时monitor中有一个Owner字段存放拥有该锁的线程的唯一标识，表示该锁被这个线程占用。

### 锁优化

jdk1.6对锁的实现引入了大量的优化，如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销。 锁主要存在四中状态，依次是：无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态，他们会随着竞争的激烈而逐渐升级。注意锁可以升级不可降级，这种策略是为了提高获得锁和释放锁的效率。

#### 自旋锁

所谓自旋锁，即让该线程不被挂起，自旋固定次数获得锁。

优点：若持有锁的线程释放锁时间短，则提升了效率，减小了线程切换的开销。

缺点：若持有锁的线程释放锁时间长，则降低了效率和性能。

### 适应性自旋锁

自适应就意味着自旋的次数不再是固定的，它是由前一次在同一个锁上的自旋时间及锁的拥有者的状态来决定。有了自适应自旋锁，随着程序运行和性能监控信息的不断完善，虚拟机对程序锁的状况预测会越来越准确，虚拟机会变得越来越聪明。

### 锁消除

JVM检测到不可能存在共享数据竞争，这是JVM会对这些同步锁进行锁消除。锁消除的依据是逃逸分析的数据支持。 

### 锁粗化

JVM检测到对同一个对象连续加锁、解锁操作，会合并一个更大范围的加锁、解锁操作。

### 偏向锁

为了在无多线程竞争的情况下尽量减少不必要的轻量级锁执行路径。引入偏向锁来减小不必要的CAS操作。

**获取锁流程**：

1. 检测Mark Word是否为可偏向状态，即是否为偏向锁1，锁标识位为01；
2. 若为可偏向状态，则测试线程ID是否为当前线程ID，如果是，则执行步骤（5），否则执行步骤（3）；
3. 如果线程ID不为当前线程ID，则通过CAS操作竞争锁，竞争成功，则将Mark Word的线程ID替换为当前线程ID，否则执行线程（4）；
4. 通过CAS竞争锁失败，证明当前存在多线程竞争情况，当到达**全局安全点**，获得偏向锁的线程被挂起，偏向锁升级为轻量级锁，然后被阻塞在安全点的线程继续往下执行同步代码块；
5. 执行同步代码块

**释放锁流程**：不会主动释放，需其他线程竞争，撤销需要达到全局安全点。

1. 暂停拥有偏向锁的线程，判断锁对象石是否还处于被锁定状态；
2. 撤销偏向锁，恢复到无锁状态（01）或者轻量级锁的状态；

### 轻量级锁

引入轻量级锁的主要目的是在多没有多线程竞争的前提下，减少传统的重量级锁使用操作系统互斥量产生的性能消耗。**若偏向锁存在多线程竞争后，则会升级为轻量级锁。**

**轻量级锁获取锁或者释放锁本质上都通过CAS修改Mark Word中的锁指针，若多次CAS失败，则升级为重量级锁。**

### 重量级锁

重量级锁通过对象内部的监视器（monitor）实现，其中monitor的本质是依赖于底层操作系统的Mutex Lock实现，操作系统实现线程之间的切换需要从用户态到内核态的切换，切换成本非常高。



### 线程池

- **管理线程，避免增加创建线程和销毁线程的资源损耗**。因为线程其实也是一个对象，创建一个对象，需要经过类加载过程，销毁一个对象，需要走GC垃圾回收流程，都是需要资源开销的。

- **提高响应速度。** 如果任务到达了，相对于从线程池拿线程，重新去创建一条线程执行，速度肯定慢很多。

- **重复利用。** 线程用完，再放回池子，可以达到重复利用的效果，节省资源。

  

#### 内部状态

线程五种状态：新建、就绪、运行、阻塞、死亡，线程池同样具有五种状态：Running, SHUTDOWN, STOP, TIDYING, TERMINATED。

```
RUNNING：处于RUNNING状态的线程池能够接受新任务，以及对新添加的任务进行处理。

SHUTDOWN：处于SHUTDOWN状态的线程池不可以接受新任务，但是可以对已添加的任务进行处理。

STOP：处于STOP状态的线程池不接收新任务，不处理已添加的任务，并且会中断正在处理的任务。

TIDYING：当所有的任务已终止，ctl记录的"任务数量"为0，线程池会变为TIDYING状态。当线程池变为TIDYING状态时，会执行钩子函数terminated()。terminated()在ThreadPoolExecutor类中是空的，若用户想在线程池变为TIDYING时，进行相应的处理；可以通过重载terminated()函数来实现。

TERMINATED：线程池彻底终止的状态。
```

​	![202105091544329011.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091544329011.png)

#### 创建线程池

```java
        public ThreadPoolExecutor(int corePoolSize,
                                  int maximumPoolSize,
                                  long keepAliveTime,
                                  TimeUnit unit,
                                  BlockingQueue<Runnable> workQueue,
                                  ThreadFactory threadFactory,
                                  RejectedExecutionHandler handler) {
            if (corePoolSize < 0 ||
                maximumPoolSize <= 0 ||
                maximumPoolSize < corePoolSize ||
                keepAliveTime < 0)
                throw new IllegalArgumentException();
            if (workQueue == null || threadFactory == null || handler == null)
                throw new NullPointerException();
            this.corePoolSize = corePoolSize;
            this.maximumPoolSize = maximumPoolSize;
            this.workQueue = workQueue;
            this.keepAliveTime = unit.toNanos(keepAliveTime);
            this.threadFactory = threadFactory;
            this.handler = handler;
        }
```

![image-20211221122959345](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122959345.png)

**corePoolSize**

线程池中核心线程的数量。当提交一个任务时，线程池会新建一个线程来执行任务，直到当前线程数等于corePoolSize。如果调用了线程池的prestartAllCoreThreads()方法，线程池会提前创建并启动所有基本线程。

**maximumPoolSize**

线程池中允许的最大线程数。线程池的阻塞队列满了之后，如果还有任务提交，如果当前的线程数小于maximumPoolSize，则会新建线程来执行任务。注意，如果使用的是无界队列，该参数也就没有什么效果了。

**keepAliveTime**

线程空闲的时间。线程的创建和销毁是需要代价的。线程执行完任务后不会立即销毁，而是继续存活一段时间：keepAliveTime。默认情况下，该参数只有在线程数大于corePoolSize时才会生效。

**unit**

keepAliveTime的单位。TimeUnit

**workQueue**

用来保存等待执行的任务的阻塞队列，等待的任务必须实现Runnable接口。我们可以选择如下几种：

- ArrayBlockingQueue：基于数组结构的有界阻塞队列，FIFO。
- LinkedBlockingQueue：基于链表结构的有界阻塞队列，FIFO。
- SynchronousQueue：不存储元素的阻塞队列，每个插入操作都必须等待一个移出操作，反之亦然。
- PriorityBlockingQueue：具有优先界别的阻塞队列。

**threadFactory**

创建线程所使用的线程工厂

**handler**

RejectedExecutionHandler，线程池的拒绝策略。所谓拒绝策略，是指将任务添加到线程池中时，线程池拒绝该任务所采取的相应策略。当向线程池中提交任务时，如果此时线程池中的线程已经饱和了，而且阻塞队列也已经满了，则线程池会选择一种拒绝策略来处理该任务。

线程池提供了四种拒绝策略：

1. AbortPolicy：直接抛出异常，默认策略；
2. CallerRunsPolicy：用调用者所在的线程来执行任务；
3. DiscardOldestPolicy：丢弃阻塞队列中靠最前的任务，并执行当前任务；
4. DiscardPolicy：直接丢弃任务；

当然我们也可以实现自己的拒绝策略，例如记录日志等等，实现RejectedExecutionHandler接口即可。

#### 常见线程池的分析

**FixedThreadPool**（固定数目线程的线程池）：易导致OOM

```java
        public static ExecutorService newFixedThreadPool(int nThreads) {
            return new ThreadPoolExecutor(nThreads, nThreads,
                                          0L, TimeUnit.MILLISECONDS,
                                          new LinkedBlockingQueue<Runnable>());
        }
```

**SingleThreadExecutor**（单线程的线程池）：易导致OOM

```java
        public static ExecutorService newSingleThreadExecutor() {
            return new FinalizableDelegatedExecutorService
                (new ThreadPoolExecutor(1, 1,
                                        0L, TimeUnit.MILLISECONDS,
                                        new LinkedBlockingQueue<Runnable>()));
        }
```



**CachedThreadPool**（可缓存线程的线程池）：易导致OOM且消耗较多资源

```java
        public static ExecutorService newCachedThreadPool() {
            return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                          60L, TimeUnit.SECONDS,
                                          new SynchronousQueue<Runnable>());
        }
```

#### 任务提交

线程池根据业务不同的需求提供了两种方式提交任务：Executor.execute()、ExecutorService.submit()。其中ExecutorService.submit()可以获取该任务执行的Future。以execute为例

1. 如果线程池当前线程数小于corePoolSize，则调用addWorker创建新线程执行任务，成功返回true，失败执行步骤2。
2. 如果线程池处于RUNNING状态，则尝试加入阻塞队列，如果加入阻塞队列成功，则尝试进行Double Check，如果加入失败，则执行步骤3。
3. 如果线程池不是RUNNING状态或者加入阻塞队列失败，则尝试创建新线程直到maxPoolSize，如果失败，则调用reject()方法运行相应的拒绝策略。

#### 线程终止

线程池ThreadPoolExecutor提供了shutdown()和shutDownNow()用于关闭线程池。

shutdown()：按过去执行已提交任务的顺序发起一个有序的关闭，但是不接受新任务。

shutdownNow() :尝试停止所有的活动执行任务、暂停等待任务的处理，并返回等待执行的任务列表。

#### 线程池异常处理方案

- 在任务代码try/catch捕获异常
- 通过Future对象的get方法接收抛出的异常，再处理
- 为工作者线程设置UncaughtExceptionHandler，在uncaughtException方法中处理异常
- 重写ThreadPoolExecutor的afterExecute方法，处理传递的异常引用

## JMM内存模型

操作系统中处理器上的寄存器的读写的速度比内存快几个数量级，为了解决这种速度矛盾，在它们之间加入了高速缓存。

加入高速缓存带来了一个新的问题：缓存一致性。

![image-20211221122019510](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122019510.png)

**操作系统中解决缓存一致性的方案：**

1. 通过在总线加LOCK#锁的方式
2. 通过缓存一致性协议（某个CPU操作共享变量时，会通知其他CPU缓存无效，需直接从主存获取）

------

**JMM可以简单的理解为线程访问共享变量的方式。**JMM规定所有的变量都**存储在主内存中，每个线程还有自己的工作内存**，工作内存存储在高速缓存或者寄存器中，保存了该线程使用的变量的主内存副本拷贝。

线程只能直接操作工作内存中的变量，不同线程之间的变量值传递需要通过主内存来完成。

![image-20211221122051235](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122051235.png)

JMM提供了八大原子操作来解决这个问题

![image-20211221122107240](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221122107240.png)

1. **lock(锁定)**: 作用于主内存的变量，把该变量标记为线程独占的状态
2. **read(读取)**: 读取主内存的变量，以便随后的load操作
3. **load(加载)**:把read的变量放到工作内存中
4. **use(使用)**: 作用于工作内存的变量，把工作内存的变量传递给CPU的执行引擎
5. **assign(赋值)**: 作用于工作内存的变量，把执行引擎处理之后的值传递到工作内存
6. **store(存储)**: 作用于工作内存的变量，把该变量传递到主内存中，以便随后的write操作
7. **write(写入)**: 作用于工作内存的变量，把store操作的变量从工作内存传递到主内存
8. **unlock(解锁)**: 作用于主内存的变量，把该变量从锁定的状态释放出来，释放之后才能被其他线程访问

**并发编程特性**

- 原子性：操作不可中断，即时在多线程环境下，操作一旦开始就不会被其他线程影响。
- 可见性： 一个线程对共享变量的操作，能够被其他线程立马感知到。
- 有序性： 不管是单线程还是多线程，代码的执行顺序都是依次执行的（指令重排后的就不是依次执行）

**那么JMM模型怎么解决并发编程的原子性、可见性、有序性问题呢？**

**原子性问题**：除了JVM自身提供的`Atomic`操作基本数据类型以外，还可以通过 `synchronized`和`Lock`实现原子性。

**可见性问题**： `volatile`关键字保证可见性，当一个共享变量被volatile修饰后，它能保证某一个线程修改之后的值能够及时被其他线程看到。`synchronized`和`Lock`也可以保证可见性，因为它们保证同一时间只能有一个线程操作共享资源。

**有序性问题**： `volatile`关键字保证有序性，`synchronized`和`Lock`也可以保证。

**Happens-Before原则**

除了靠`synchronized`和`volatile`来保证原子性、可见性、有序性，JDK提供了一套**happens-before原则**来辅助保证程序执行的原子性、可见性、有序性，具体原则如下：

1. **程序执行顺序规则**：在一个线程内，必须保证按照代码的顺序执行。
2. **锁规则**：同一个锁的情况下，解锁操作必须发生在后一个加锁操作之前。
3. **volatile变量规则**：`volatile`变量的写操作必须发生在对这个变量的读操作之前，这保证了`volatile`的及时可见性。
4. **传递性规则**：如果操作A发生在操作B之前，而操作B又发生在操作C之前，则可以得出操作A发生在操作C之前
5. **线程启动规则**： 线程的start方法发生在此线程任意操作之前。
6. **线程中断规则**：线程的interrupt方法发生在线程中断之前。
7. **线程终止规则**： 线程中的任意操作都发生在线程终止之前。
8. **对象终结规则**：一个对象的初始化完成（构造函数执行结束）发生在它的finalize（）方法开始之前。

**了解一下指令重排**

java语言规定，只要程序的最终结果与它有序执行的结果相等，那么指令的执行顺序可以与代码不一致，这个过程叫做**指令重排**。

**指令重排有什么意义**？

JVM根据CPU的特性（多级缓存、多核处理器）适当的对机器指令进行指令重排，使机器指令更符合CPU的执行特征，最大限度的发挥CPU的作用

**指令重排的原则：as-if-serial**

意思是不管怎样重排序，**单线程**的执行结果不能被改变。











## Volatile

> volatile的内存语义:当写一个volatile变量时，JMM会把该线程对应的本地内存中的共享变量值立即刷新到主内存中。 当读一个volatile变量时，JMM会把该线程对应的本地内存设置为无效，直接从主内存中读取共享变量

Volatile 变量具有 `synchronized` 的可见性特性，还能**防止指令重排序**，但是**不具备原子性**。这就是说线程能够自动发现 volatile 变量的最新值。

volatile的底层实现是通过插入内存屏障，但是对于编译器来说，发现一个最优布置来最小化插入内存屏障的总数几乎是不可能的，所以，JMM采用了保守策略。如下：

- 在每一个volatile写操作前面插入一个StoreStore屏障
- 在每一个volatile写操作后面插入一个StoreLoad屏障
- 在每一个volatile读操作后面插入一个LoadLoad屏障
- 在每一个volatile读操作后面插入一个LoadStore屏障

![202105091543383051.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091543383051.png)

## CAS

CAS即比较交换，执行函数CAS(V,E,N)，即比较内存值是否等于预期值，若相等则更改为新值。其实现方式是基于硬件平台的汇编指令，也就是CAS是基于硬件方面实现的，**JUC里面的实现类都是基于CAS和volatile实现的**。

![202105091543542891.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091543542891.png)

底层采用了**Unsafe类，其里面是与硬件交互的native方法**。valueOffset为变量值在内存中的偏移地址，unsafe就是通过偏移地址来得到数据的原值的。

**CAS缺陷：**

1. **ABA问题**：指多个线程在CAS的过程中，其他线程感知不到相关变更过程。**解决方法**：**采用AtomicStampedReference（版本时间戳），本质上有个int值的版本号，更改前会获取，更改时会比较版本号是否相同，从而保持一致性**
2. **循环时间太长**
3. **只能保证一个共享变量原子操作**

## AQS

AQS是AbstractQueuedSynchronizer的简称。AQS提供了一种实现阻塞锁和一系列依赖FIFO等待队列的同步器的框架。AQS使用一个int类型的成员变量state来表示同步状态，子类们必须定义操作state变量的protected方法，且为了保证同步，必须原子地操作这些变量。

### **核心API**

```java
常见方法：
setState(int newState)：设置当前同步状态；
getState()：返回同步状态的当前值
compareAndSetState(int expect, int update)：使用CAS设置当前状态，该方法能够保证状态设置的原子性；
tryAcquire(int arg)：独占式获取同步状态，获取同步状态成功后，其他线程需要等待该线程释放同步状态才能获取同步状态；
tryRelease(int arg)：独占式释放同步状态；
tryAcquireShared(int arg)：共享式获取同步状态，返回值大于等于0则表示获取成功，否则获取失败；
tryReleaseShared(int arg)：共享式释放同步状态；
isHeldExclusively()：当前同步器是否在独占式模式下被线程占用，一般该方法表示是否被当前线程所独占；
acquire(int arg)：独占式获取同步状态，成功则返回，否则，进入同步队列等待，该方法将会调用可重写的tryAcquire(int arg)方法；
acquireInterruptibly(int arg)：独享式获取同步状态，响应中断；
tryAcquireNanos(int arg,long nanos)：超时获取同步状态，若超时没有获取到同步状态，那么将会返回false，已经获取则返回true；
acquireShared(int arg)：共享式获取同步状态，若未获取到同步状态，则会进入同步队列等待，与独占式的主要区别是在同一时刻可以有多个线程获取到同步状态；
acquireSharedInterruptibly(int arg)：共享式获取同步状态，响应中断；
tryAcquireSharedNanos(int arg, long nanosTimeout)：共享式获取同步状态，增加超时限制；
release(int arg)：独占式释放同步状态，该方法会在释放同步状态之后，将同步队列中第一个节点包含的线程唤醒；
releaseShared(int arg)：共享式释放同步状态；
```

### CLH队列

AQS内部维护着一个FIFO双向队列，该队列就是CLH同步队列。当前线程如果获取同步状态失败时，AQS则会将当前线程已经等待状态等信息构造成一个节点（Node）并将其加入到CLH同步队列，同时会阻塞当前线程，当同步状态释放时，会把首节点唤醒，使其再次尝试获取同步状态。 

![202105091543434881.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091543434881.png)

在CLH同步队列中，一个节点表示一个线程，它保存着线程的引用（thread）、状态（waitStatus）、前驱节点（prev）、后继节点（next），其定义如下：

```java
   static final class Node {
        /** 共享 */
        static final Node SHARED = new Node();
    
        /** 独占 */
        static final Node EXCLUSIVE = null;
    
        // 线程等待状态为取消
        static final int CANCELLED =  1;
    
        // 指示后继节点取消停放，即unpark
        static final int SIGNAL    = -1;
    
        // 线程等待条件
        static final int CONDITION = -2;
    
        // 表示下一次共享式同步状态获取将会无条件地传播下去
        static final int PROPAGATE = -3;
    
        // 等待状态
        volatile int waitStatus;
    
        /** 前驱节点 */
        volatile Node prev;
    
        /** 后继节点 */
        volatile Node next;
    
        /** 获取同步状态的线程 */
        volatile Thread thread;
    
        Node nextWaiter;
    
        final boolean isShared() {
            return nextWaiter == SHARED;
        }
    
        final Node predecessor() throws NullPointerException {
            Node p = prev;
            if (p == null)
                throw new NullPointerException();
            else
                return p;
        }
    
        Node() {
        }
    
        Node(Thread thread, Node mode) {
            this.nextWaiter = mode;
            this.thread = thread;
        }
    
        Node(Thread thread, int waitStatus) {
            this.waitStatus = waitStatus;
            this.thread = thread;
        }
    }
    
```

### 入队

- addWaiter(Node node)先通过快速尝试设置尾节点，如果失败，则调用enq(Node node)方法设置尾节点
- enq(Node node)一直CAS直至成功添加后，当前线程才会从该方法返回，否则会一直执行下去

```java
        private Node addWaiter(Node mode) {
            //新建Node
            Node node = new Node(Thread.currentThread(), mode);
            //快速尝试添加尾节点
            Node pred = tail;
            if (pred != null) {
                node.prev = pred;
                //CAS设置尾节点
                if (compareAndSetTail(pred, node)) {
                    pred.next = node;
                    return node;
                }
            }
            //多次尝试
            enq(node);
            return node;
        }

        private Node enq(final Node node) {
            //多次尝试，直到成功为止
            for (;;) {
                Node t = tail;
                //tail不存在，设置为首节点
                if (t == null) {
                    if (compareAndSetHead(new Node()))
                        tail = head;
                } else {
                    //设置为尾节点
                    node.prev = t;
                    if (compareAndSetTail(t, node)) {
                        t.next = node;
                        return t;
                    }
                }
            }
        }
    
```

![202105091543440112.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091543440112.png)

### 出队

首节点的线程释放同步状态后，将会唤醒它的后继节点（next），而后继节点将会在获取同步状态成功时将自己设置为首节点。过程：head执行该节点并断开原首节点的next和当前节点的prev即可，无需CAS来保证，因为只有一个线程能够成功获取到同步状态。

![202105091543446343.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091543446343.png)

### 独占式

独占式，同一时刻仅有一个线程持有同步状态。

#### 独占式同步状态获取

acquire(int arg)方法为AQS提供的模板方法，该方法为独占式获取同步状态。

```java
        public final void acquire(int arg) {
            if (!tryAcquire(arg) &&
                acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
                selfInterrupt();
        }
```

各个方法定义如下：

1. tryAcquire：尝试获取锁，获取成功则设置锁状态并返回true，否则返回false。**该方法自定义同步组件自己实现**，该方法必须要保证线程安全地获取同步状态。
2. addWaiter：如果tryAcquire返回false，则调用该方法将当前线程加入到CLH同步队列尾部。
3. acquireQueued：当前线程会根据公平性原则来进行阻塞等待（自旋），直到获取锁为止；并且返回当前线程在等待过程中有没有中断过。
4. selfInterrupt：产生一个中断。

acquireQueued方法为一个自旋的过程，也就是说当前线程（Node）进入同步队列后，就会进入一个自旋的过程，每个节点都会自省地观察，当条件满足，获取到同步状态后，就可以从这个自旋过程中退出，否则会一直执行下去。如下：

```java
        final boolean acquireQueued(final Node node, int arg) {
            boolean failed = true;
            try {
                //中断标志
                boolean interrupted = false;
                /*
                 * 自旋过程，其实就是一个死循环而已
                 */
                for (;;) {
                    //当前线程的前驱节点
                    final Node p = node.predecessor();
                    //当前线程的前驱节点是头结点，且同步状态成功
                    if (p == head && tryAcquire(arg)) {
                        setHead(node);
                        p.next = null; // help GC
                        failed = false;
                        return interrupted;
                    }
                    //获取失败，线程等待--具体后面介绍
                    if (shouldParkAfterFailedAcquire(p, node) &&
                            parkAndCheckInterrupt())
                        interrupted = true;
                }
            } finally {
                if (failed)
                    cancelAcquire(node);
            }
        }
```

从上面代码中可以看到，当前线程会一直尝试获取同步状态，当然前提是只有其前驱节点为头结点才能够尝试获取同步状态，理由：

1. 保持FIFO同步队列原则。
2. 头节点释放同步状态后，将会唤醒其后继节点，后继节点被唤醒后需要检查自己是否为头节点。

acquire(int arg)方法流程图如下：

![202105091543473341.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091543473341.png)

#### 独占式获取响应中断

为了响应中断，AQS提供了acquireInterruptibly(int arg)方法，该方法在等待获取同步状态时，如果当前线程被中断了，会立刻响应中断抛出异常InterruptedException。

```java
        public final void acquireInterruptibly(int arg)
                throws InterruptedException {
            if (Thread.interrupted())
                throw new InterruptedException();
            if (!tryAcquire(arg))
                doAcquireInterruptibly(arg);
        }
```

首先校验该线程是否已经中断了，如果是则抛出InterruptedException，否则执行tryAcquire(int arg)方法获取同步状态，如果获取成功，则直接返回，否则执行doAcquireInterruptibly(int arg)。doAcquireInterruptibly(int arg)定义如下：

```java
    private void doAcquireInterruptibly(int arg)
            throws InterruptedException {
            final Node node = addWaiter(Node.EXCLUSIVE);
            boolean failed = true;
            try {
                for (;;) {
                    final Node p = node.predecessor();
                    if (p == head && tryAcquire(arg)) {
                        setHead(node);
                        p.next = null; // help GC
                        failed = false;
                        return;
                    }
                    if (shouldParkAfterFailedAcquire(p, node) &&
                        parkAndCheckInterrupt())
                        throw new InterruptedException();
                }
            } finally {
                if (failed)
                    cancelAcquire(node);
            }
        }
    
```

doAcquireInterruptibly(int arg)方法与acquire(int arg)方法仅有两个差别。

1. 方法声明抛出InterruptedException异常
2. 在中断方法处不再是使用interrupted标志，而是直接抛出InterruptedException异常

#### 独占式超时获取

tryAcquireNanos(int arg,long nanos)。该方法为acquireInterruptibly方法的进一步增强，它除了响应中断外，还有超时控制。即如果当前线程没有在指定时间内获取同步状态，则会返回false，否则返回true。

```java
       public final boolean tryAcquireNanos(int arg, long nanosTimeout)
                throws InterruptedException {
            if (Thread.interrupted())
                throw new InterruptedException();
            return tryAcquire(arg) ||
                doAcquireNanos(arg, nanosTimeout);
        }
```

tryAcquireNanos(int arg, long nanosTimeout)方法超时获取最终是在doAcquireNanos(int arg, long nanosTimeout)中实现的，如下：

```java
        private boolean doAcquireNanos(int arg, long nanosTimeout)
                throws InterruptedException {
            //nanosTimeout <= 0
            if (nanosTimeout <= 0L)
                return false;
            //超时时间
            final long deadline = System.nanoTime() + nanosTimeout;
            //新增Node节点
            final Node node = addWaiter(Node.EXCLUSIVE);
            boolean failed = true;
            try {
                //自旋
                for (;;) {
                    final Node p = node.predecessor();
                    //获取同步状态成功
                    if (p == head && tryAcquire(arg)) {
                        setHead(node);
                        p.next = null; // help GC
                        failed = false;
                        return true;
                    }
                    /*
                     * 获取失败，做超时、中断判断
                     */
                    //重新计算需要休眠的时间
                    nanosTimeout = deadline - System.nanoTime();
                    //已经超时，返回false
                    if (nanosTimeout <= 0L)
                        return false;
                    //如果没有超时，则等待nanosTimeout纳秒
                    //注：该线程会直接从LockSupport.parkNanos中返回，
                    //LockSupport为JUC提供的一个阻塞和唤醒的工具类，后面做详细介绍
                    if (shouldParkAfterFailedAcquire(p, node) &&
                            nanosTimeout > spinForTimeoutThreshold)
                        LockSupport.parkNanos(this, nanosTimeout);
                    //线程是否已经中断了
                    if (Thread.interrupted())
                        throw new InterruptedException();
                }
            } finally {
                if (failed)
                    cancelAcquire(node);
            }
        }
```

![202105091543476712.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091543476712.png)

#### 独占式同步状态释放

当线程获取同步状态后，执行完相应逻辑后就需要释放同步状态。

```java
       public final boolean release(int arg) {
            // 调用tryRelease尝试释放
            if (tryRelease(arg)) {
                Node h = head;
                if (h != null && h.waitStatus != 0)
                    // 唤醒后继节点
                    unparkSuccessor(h);
                return true;
            }
            return false;
        }
```

> 在AQS中维护着一个FIFO的同步队列，当线程获取同步状态失败后，则会加入到CLH同步队列的队尾并一直保持着自旋。在CLH同步队列中的线程在自旋时会判断其前驱节点是否为首节点，如果为首节点则不断尝试获取同步状态，获取成功则退出CLH同步队列，若非首节点则进入等待队列。当线程执行完逻辑后，会释放同步状态，释放后会唤醒其后继节点。

### 共享式

共享式在同一时刻可以有多个线程获取同步状态。

#### 共享式同步状态获取

```java
        public final void acquireShared(int arg) {
            if (tryAcquireShared(arg) < 0)
                //获取失败，自旋获取同步状态
                doAcquireShared(arg);
        }
```

先调用tryAcquireShared(int arg)方法尝试获取同步状态，如果获取失败则调用doAcquireShared(int arg)自旋方式获取同步状态，共享式获取同步状态的标志是返回 >= 0 的值表示获取成功。自选式获取同步状态如下：

```java
        private void doAcquireShared(int arg) {
            /共享式节点
            final Node node = addWaiter(Node.SHARED);
            boolean failed = true;
            try {
                boolean interrupted = false;
                for (;;) {
                    //前驱节点
                    final Node p = node.predecessor();
                    //如果其前驱节点，获取同步状态
                    if (p == head) {
                        //尝试获取同步
                        int r = tryAcquireShared(arg);
                        if (r >= 0) {
                            setHeadAndPropagate(node, r);
                            p.next = null; // help GC
                            if (interrupted)
                                selfInterrupt();
                            failed = false;
                            return;
                        }
                    }
                    if (shouldParkAfterFailedAcquire(p, node) &&
                            parkAndCheckInterrupt())
                        interrupted = true;
                }
            } finally {
                if (failed)
                    cancelAcquire(node);
            }
        }
    
```

tryAcquireShared(int arg)方法尝试获取同步状态，返回值为int，当 >= 0 时，表示能够获取到同步状态，此时退出自旋。 

acquireShared(int arg)方法不响应中断，与独占式相似，AQS也提供了响应中断、超时的方法，分别是：acquireSharedInterruptibly(int arg)、tryAcquireSharedNanos(int arg,long nanos)。（此处不做介绍）

#### 共享式同步状态释放

```java
        public final boolean releaseShared(int arg) {
            if (tryReleaseShared(arg)) {
                doReleaseShared();
                return true;
            }
            return false;
        }
```

因为可能会存在多个线程同时进行释放同步状态资源，所以需要确保同步状态安全地成功释放，一般都是通过CAS和循环来完成的。

### 阻塞和唤醒线程

在线程获取同步状态时如果获取失败，则加入CLH同步队列，通过通过自旋的方式不断获取同步状态，但是在自旋的过程中则需要判断当前线程是否需要阻塞，其主要方法在acquireQueued()：

```java
    if (shouldParkAfterFailedAcquire(p, node) &&
                        parkAndCheckInterrupt())
                        interrupted = true;
```

可看出，在获取同步状态失败后，线程并不是立马阻塞，需检查该线程状态，其方法为 shouldParkAfterFailedAcquire(Node pred, Node node) ，主要靠前驱节点判断当前线程是否应该被阻塞，代码如下：

```java
        private static boolean shouldParkAfterFailedAcquire(Node pred, Node node) {
            //前驱节点
            int ws = pred.waitStatus;
            //状态为signal，表示当前线程处于等待状态，直接放回true
            if (ws == Node.SIGNAL)
                return true;
            //前驱节点状态 > 0 ，则为Cancelled,表明该节点已经超时或者被中断了，需要从同步队列中取消
            if (ws > 0) {
                do {
                    node.prev = pred = pred.prev;
                } while (pred.waitStatus > 0);
                pred.next = node;
            }
            //前驱节点状态为Condition、propagate
            else {
                compareAndSetWaitStatus(pred, ws, Node.SIGNAL);
            }
            return false;
        }
    
```

这段代码主要检查当前线程是否需要被阻塞，具体规则如下：

1. 如果当前线程的前驱节点状态为SINNAL，则表明当前线程需要被阻塞,直接返回true，当前线程阻塞
2. 如果当前线程的前驱节点状态为CANCELLED（ws > 0），则表明该线程的前驱节点已经等待超时或者被中断了，则需要从CLH队列中将该前驱节点删除掉，直到回溯到前驱节点状态 <= 0 ，返回false
3. 如果前驱节点非SINNAL，非CANCELLED，则通过CAS的方式将其前驱节点设置为SINNAL，返回false

如果 shouldParkAfterFailedAcquire(Node pred, Node node) 方法返回true，则调用parkAndCheckInterrupt()方法阻塞当前线程：

```java
        private final boolean parkAndCheckInterrupt() {
            LockSupport.park(this);
            return Thread.interrupted();
        }
```

parkAndCheckInterrupt() 方法主要是把当前线程挂起，从而阻塞住线程的调用栈，同时返回当前线程的中断状态。其内部则是调用LockSupport工具类的park()方法来阻塞该方法。 当线程释放同步状态后，则需要唤醒该线程的后继节点：

```java
        public final boolean release(int arg) {
            if (tryRelease(arg)) {
                Node h = head;
                if (h != null && h.waitStatus != 0)
                    //唤醒后继节点
                    unparkSuccessor(h);
                return true;
            }
            return false;
        }
    
```

调用unparkSuccessor(Node node)唤醒后继节点：

```java
        private void unparkSuccessor(Node node) {
            //当前节点状态
            int ws = node.waitStatus;
            //当前状态 < 0 则设置为 0
            if (ws < 0)
                compareAndSetWaitStatus(node, ws, 0);
    
            //当前节点的后继节点
            Node s = node.next;
            //后继节点为null或者其状态 > 0 (超时或者被中断了)
            if (s == null || s.waitStatus > 0) {
                s = null;
                //从tail节点来找可用节点
                for (Node t = tail; t != null && t != node; t = t.prev)
                    if (t.waitStatus <= 0)
                        s = t;
            }
            //唤醒后继节点
            if (s != null)
                LockSupport.unpark(s.thread);
        }
   
```

可能会存在当前线程的后继节点为null，超时、被中断的情况，采用tail回溯办法找第一个可用的线程。最后调用LockSupport的unpark(Thread thread)方法唤醒该线程。

## CountDownLatch

- CountDownLatch 类位于 java.util.concurrent 包下，**利用它可以实现类似计数器的功能**。比如有一个任务A，它要等待其他4个任务执行完毕之后才能执行，此时就可以利用CountDownLatch来实现这种功能了。

```JAVA
public CountDownLatch(int count) {  };  //参数count为计数值，唯一的一个构造器
```

- 常用API：

 ```java
 public void await() throws InterruptedException { };   //调用await()方法的线程会被挂起，它会等待直到count值为0才继续执行
 public boolean await(long timeout, TimeUnit unit) throws InterruptedException { };  //和await()类似，只不过等待一定的时间后count值还没变为0的话就会继续执行
 public void countDown() { };  //将count值减1
 ```

## CyclicBarrier

回环栅栏，通过它可以实现**让一组线程等待至某个状态之后再全部同时执行**。叫做**回环是因为当所有等待线程都被释放以后，CyclicBarrier可以被重用**。我们暂且把这个状态就叫做barrier，当调用await()方法之后，线程就处于barrier了。

- CyclicBarrier类位于java.util.concurrent包下，CyclicBarrier提供2个构造器：
- **参数parties指让多少个线程或者任务等待至barrier状态**
- **参数barrierAction为当这些线程都达到barrier状态时会执行的内容**

```java
public CyclicBarrier(int parties, Runnable barrierAction) {}
public CyclicBarrier(int parties) {}
```

- 然后CyclicBarrier中最重要的方法就是 **await** 方法，它有2个重载版本：
- **第一个版本比较常用，用来挂起当前线程，直至所有线程都到达barrier状态再同时执行后续任务；**
- 第二个版本是让这些线程等待至一定的时间，如果还有线程没有到达barrier状态就直接让到达barrier的线程执行后续任务。

```java
public int await() throws InterruptedException, BrokenBarrierException { };
public int await(long timeout, TimeUnit unit)throws InterruptedException,BrokenBarrierExcep
```

## Semaphore

- 信号量，**Semaphore 可以同时让多个线程同时访问共享资源，通过 acquire() 获取一个许可，如果没有就等待，而 release() 释放一个许可。**
- Semaphore类位于java.util.concurrent包下，它提供了2个构造器：

```java
public Semaphore(int permits) {          //参数permits表示许可数目，即同时可以允许多少线程进行访问
    sync = new NonfairSync(permits);
}
public Semaphore(int permits, boolean fair) {    //这个多了一个参数fair表示是否是公平的，即等待时间越久的越先获取许可
    sync = (fair)? new FairSync(permits) : new NonfairSync(permits);
}
```

- 重要API：

  ```JAVA
  public void acquire() throws InterruptedException {  }     //获取一个许可
  public void acquire(int permits) throws InterruptedException { }    //获取permits个许可
  public void release() {}          //释放一个许可
  public void release(int permits) {}    //释放permits个许可
  ```

  - **acquire()用来获取一个许可**，若无许可能够获得，则会一直等待，直到获得许可。
  - **release()用来释放许可**。
  - **注意，在释放许可之前，必须先获获得许可。**

  　**这4个方法都会被阻塞，如果想立即得到执行结果，可以使用下面几个方法：**

  ```java
  public boolean tryAcquire() { };    //尝试获取一个许可，若获取成功，则立即返回true，若获取失败，则立即返回false
  public boolean tryAcquire(long timeout, TimeUnit unit) throws InterruptedException { };  //尝试获取一个许可，若在指定的时间内获取成功，则立即返回true，否则则立即返回false
  public boolean tryAcquire(int permits) { }; //尝试获取permits个许可，若获取成功，则立即返回true，若获取失败，则立即返回false
  public boolean tryAcquire(int permits, long timeout, TimeUnit unit) throws InterruptedExce
  ```

## LockSupport

> LockSupport是用来创建锁和其他同步类的基本线程阻塞原语

每个使用LockSupport的线程都会与一个许可关联，许可不可重入且会阻塞，常见方法如下：

![202105091543488191.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091543488191.png)

park(Object blocker)方法的blocker参数，主要是用来标识当前线程在等待的对象，该对象主要用于问题排查和系统监控。 

park()方法的源码如下：

```java
        public static void park() {
            UNSAFE.park(false, 0L);
        }
```

unpark(Thread thread)方法源码如下：

```java
        public static void unpark(Thread thread) {
            if (thread != null)
                UNSAFE.unpark(thread);
        }
```

从上面可以看出，其内部的实现都是通过UNSAFE（sun.misc.Unsafe UNSAFE）来实现的，其定义如下：

```java
    public native void park(boolean var1, long var2);
    public native void unpark(Object var1);
```

两个都是native本地方法。Unsafe 是一个比较危险的类，主要是用于执行低级别、不安全的方法集合。尽管这个类和所有的方法都是公开的（public），但是这个类的使用仍然受限，你无法在自己的java程序中直接使用该类，因为只有授信的代码才能获得该类的实例。



## ReentrantLock

在实现层面除了依赖于CAS外，还依赖于LockSupport中的一些方法。

```java
abstract static class Sync extends AbstractQueuedSynchronizer  // AQS的实现类
static final class NonfairSync extends Sync
static final class FairSync extends Sync
```

其中Sync是抽象类，NonfairSync是 fair 为 false 时使用的类，而 FairSync 是 fair 为 true 时需要使用的类。在构造方法的时候可以指定是否为公平/非公平锁。**默认为非公平锁。**

- 公平与非公平的对比：

  - FairSync 和 NonfairSync 中的主要区别为： 在获取锁时，在tryAcquire 方法中，如果当前线程状态没有被锁定，FairSysc 会多一个检查
  - 目的是，当没有其他等待时间更长的线程时候，才能获取到锁

  **为什么不默认保证公平？** 保证公平整体性能会比较低，其原因不是因为检查慢，而是因为会让活跃线程无法得到锁，从而进入等待状态，引起了频繁的上下文切换，降低了整体的效率。

### 获得锁

```java
        public void lock() {
            sync.lock();  // 调用了sync的lock实现
        }

       // sync的lock实现
        final void lock() {
            //尝试获取锁
            if (compareAndSetState(0, 1))
                setExclusiveOwnerThread(Thread.currentThread());
            else
                //获取失败，调用AQS的acquire(int arg)方法
                acquire(1);
        }
    
```

CAS失败后则调用acquire(int arg)方法，该方法定义在AQS中

```java
        public final void acquire(int arg) {
            if (!tryAcquire(arg) &&
                    acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
                selfInterrupt();
        }
```

这个方法首先调用tryAcquire(int arg)方法，在AQS中讲述过，tryAcquire(int arg)需要自定义同步组件提供实现，非公平锁实现如下：

```java
       protected final boolean tryAcquire(int acquires) {
            return nonfairTryAcquire(acquires);
        }
    
        final boolean nonfairTryAcquire(int acquires) {
            //当前线程
            final Thread current = Thread.currentThread();
            //获取同步状态
            int c = getState();
            //state == 0,表示没有该锁处于空闲状态
            if (c == 0) {
                //获取锁成功，设置为当前线程所有
                if (compareAndSetState(0, acquires)) {
                    setExclusiveOwnerThread(current);
                    return true;
                }
            }
            //线程重入
            //判断锁持有的线程是否为当前线程
            else if (current == getExclusiveOwnerThread()) {
                int nextc = c + acquires;
                if (nextc < 0) // overflow
                    throw new Error("Maximum lock count exceeded");
                setState(nextc);
                return true;
            }
            return false;
        }
```

该方法主要逻辑：判断同步状态state == 0 ?，如果是表示该锁还没有被线程持有，直接通过CAS获取同步状态，如果成功返回true。如果state != 0，则判断当前线程是否为获取锁的线程，如果是则获取锁，成功返回true。成功获取锁的线程再次获取锁，这时增加了同步状态state。

### 释放锁

```java
        public void unlock() {
            sync.release(1);
        }
```

unlock内部使用Sync的release(int arg)释放锁，release(int arg)是在AQS中定义的：

```java
        public final boolean release(int arg) {
            if (tryRelease(arg)) {
                Node h = head;
                if (h != null && h.waitStatus != 0)
                    unparkSuccessor(h);
                return true;
            }
            return false;
        }
```

与获取同步状态的acquire(int arg)方法相似，释放同步状态的tryRelease(int arg)同样是需要自定义同步组件自己实现：

```java
        protected final boolean tryRelease(int releases) {
            //减掉releases
            int c = getState() - releases;
            //如果释放的不是持有锁的线程，抛出异常
            if (Thread.currentThread() != getExclusiveOwnerThread())
                throw new IllegalMonitorStateException();
            boolean free = false;
            //state == 0 表示已经释放完全了，其他线程可以获取同步状态了
            if (c == 0) {
                free = true;
                setExclusiveOwnerThread(null);
            }
            setState(c);
            return free;
        }
    
```

只有当同步状态彻底释放后该方法才会返回true。当state == 0 时，则将锁持有线程设置为null，free= true，表示释放成功。

### 公平锁和非公平锁

公平锁与非公平锁的区别在于获取锁的时候是否按照FIFO的顺序来。释放锁不存在公平性和非公平性，上面以非公平锁为例，下面我们来看看公平锁fairsync的tryAcquire(int arg)：

```java
      protected final boolean tryAcquire(int acquires) {
            final Thread current = Thread.currentThread();
            int c = getState();
            if (c == 0) {
                if (!hasQueuedPredecessors() &&
                        compareAndSetState(0, acquires)) {
                    setExclusiveOwnerThread(current);
                    return true;
                }
            }
            else if (current == getExclusiveOwnerThread()) {
                int nextc = c + acquires;
                if (nextc < 0)
                    throw new Error("Maximum lock count exceeded");
                setState(nextc);
                return true;
            }
            return false;
        }
    
```

比较非公平锁和公平锁获取同步状态的过程，会发现两者唯一的区别就在于公平锁在获取同步状态时多了一个限制条件：hasQueuedPredecessors()，定义如下：

```java
        public final boolean hasQueuedPredecessors() {
            Node t = tail;  //尾节点
            Node h = head;  //头节点
            Node s;
    
            //头节点 != 尾节点
            //同步队列第一个节点不为null
            //当前线程是同步队列第一个节点
            return h != t &&
                    ((s = h.next) == null || s.thread != Thread.currentThread());
        }
```

### Condition

Condition与Object的监视器方法的对比：

![202105091543527671.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091543527671.png)

##### 常见阻塞和唤醒API

```java
Condition是一种广义上的条件队列。Condition必须要配合锁一起使用，因为对共享状态变量的访问发生在多线程环境下。一个Condition的实例必须与一个Lock绑定，因此Condition一般都是作为Lock的内部实现。

await() ：造成当前线程在接到信号或被中断之前一直处于等待状态。
await(long time, TimeUnit unit) ：造成当前线程在接到信号、被中断或到达指定等待时间之前一直处于等待状态。
awaitNanos(long nanosTimeout) ：造成当前线程在接到信号、被中断或到达指定等待时间之前一直处于等待状态。返回值表示剩余时间。
awaitUninterruptibly() ：造成当前线程在接到信号之前一直处于等待状态。【注意：该方法对中断不敏感】。
awaitUntil(Date deadline) ：造成当前线程在接到信号、被中断或到达指定最后期限之前一直处于等待状态。如果没有到指定时间就被通知，则返回true，否则表示到了指定时间，返回返回false。
signal() ：唤醒一个等待线程。该线程从等待方法返回前必须获得与Condition相关的锁。
signalAll() ：唤醒所有等待线程。能够从等待方法返回的线程必须获得与Condition相关的锁。
```

获取一个Condition必须要通过Lock的newCondition()方法。该方法定义在接口Lock下面，返回的结果是绑定到此 Lock 实例的新 Condition 实例。

Condition为一个接口，其下仅有一个实现类ConditionObject，由于Condition的操作需要获取相关的锁，而AQS则是同步锁的实现基础，所以ConditionObject则定义为AQS的内部类。定义如下：

```java
    public class ConditionObject implements Condition, java.io.Serializable { 
    }
```

##### 等待队列

每个Condition对象都包含着一个FIFO队列，该队列是Condition对象通知/等待功能的关键。在队列中每一个节点都包含着一个线程引用，该线程就是在该Condition对象上等待的线程。

```java
    public class ConditionObject implements Condition, java.io.Serializable {
        private static final long serialVersionUID = 1173984872572414699L;
    
        //头节点
        private transient Node firstWaiter;
        //尾节点
        private transient Node lastWaiter;
    
        public ConditionObject() {
        }
    
        /** 省略方法 **/
    }
```

Node里面包含了当前线程的引用。Node定义与AQS的CLH同步队列的节点使用的都是同一个类。

![202105091543533152.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091543533152.png)

##### 等待

调用Condition的await()方法会使当前线程进入等待状态，同时会加入到Condition等待队列同时释放锁。当从await()方法返回时，当前线程一定是获取了Condition相关联的锁。

```java
        public final void await() throws InterruptedException {
            // 当前线程中断
            if (Thread.interrupted())
                throw new InterruptedException();
            //当前线程加入等待队列
            Node node = addConditionWaiter();
            //释放锁
            long savedState = fullyRelease(node);
            int interruptMode = 0;
            /**
             * 检测此节点的线程是否在同步队上，如果不在，则说明该线程还不具备竞争锁的资格，则继续等待
             * 直到检测到此节点在同步队列上
             */
            while (!isOnSyncQueue(node)) {
                //线程挂起
                LockSupport.park(this);
                //如果已经中断了，则退出
                if ((interruptMode = checkInterruptWhileWaiting(node)) != 0)
                    break;
            }
            //竞争同步状态
            if (acquireQueued(node, savedState) && interruptMode != THROW_IE)
                interruptMode = REINTERRUPT;
            //清理下条件队列中的不是在等待条件的节点
            if (node.nextWaiter != null) // clean up if cancelled
                unlinkCancelledWaiters();
            if (interruptMode != 0)
                reportInterruptAfterWait(interruptMode);
        }
```

逻辑：将当前线程新建一个节点同时加入到条件队列中，然后释放当前线程持有的同步状态。然后则是不断检测是否有signal唤醒，如果不存在则一直挂起，否则参与竞争同步状态。

**加入条件队列源码：**

```java
        private Node addConditionWaiter() {
            Node t = lastWaiter;    //尾节点
            //Node的节点状态如果不为CONDITION，则表示该节点不处于等待状态，需要清除节点
            if (t != null && t.waitStatus != Node.CONDITION) {
                //清除条件队列中所有状态不为Condition的节点
                unlinkCancelledWaiters();
                t = lastWaiter;
            }
            //当前线程新建节点，状态CONDITION
            Node node = new Node(Thread.currentThread(), Node.CONDITION);
            /**
             * 将该节点加入到条件队列中最后一个位置
             */
            if (t == null)
                firstWaiter = node;
            else
                t.nextWaiter = node;
            lastWaiter = node;
            return node;
        }
```

该方法主要是将当前线程加入到Condition条件队列中。当然在加入到尾节点之前会清除所有状态不为Condition的节点。

fullyRelease(Node node)，负责释放该线程持有的锁。

```java
        final long fullyRelease(Node node) {
            boolean failed = true;
            try {
                //节点状态--其实就是持有锁的数量
                long savedState = getState();
                //释放锁
                if (release(savedState)) {
                    failed = false;
                    return savedState;
                } else {
                    throw new IllegalMonitorStateException();
                }
            } finally {
                if (failed)
                    node.waitStatus = Node.CANCELLED;
            }
        }
```

isOnSyncQueue(Node node)：如果一个节点刚开始在条件队列上，现在在同步队列上获取锁则返回true

```java
        final boolean isOnSyncQueue(Node node) {
            //状态为Condition，获取前驱节点为null，返回false
            if (node.waitStatus == Node.CONDITION || node.prev == null)
                return false;
            //后继节点不为null，肯定在CLH同步队列中
            if (node.next != null)
                return true;
    
            return findNodeFromTail(node);
        }
```

unlinkCancelledWaiters()：负责将条件队列中状态不为Condition的节点删除

```java
            private void unlinkCancelledWaiters() {
                Node t = firstWaiter;
                Node trail = null;
                while (t != null) {
                    Node next = t.nextWaiter;
                    if (t.waitStatus != Node.CONDITION) {
                        t.nextWaiter = null;
                        if (trail == null)
                            firstWaiter = next;
                        else
                            trail.nextWaiter = next;
                        if (next == null)
                            lastWaiter = trail;
                    }
                    else
                        trail = t;
                    t = next;
                }
            }
```

##### 通知

调用Condition的signal()方法，将会唤醒在等待队列中等待最长时间的节点（条件队列里的首节点），在唤醒节点前，会将节点移到CLH同步队列中。

```java
        public final void signal() {
            //检测当前线程是否为拥有锁的独
            if (!isHeldExclusively())
                throw new IllegalMonitorStateException();
            //头节点，唤醒条件队列中的第一个节点
            Node first = firstWaiter;
            if (first != null)
                doSignal(first);    //唤醒
        }
```

该方法首先会判断当前线程是否已经获得了锁，这是前置条件。然后唤醒条件队列中的头节点。

doSignal(Node first)：唤醒头节点

```java
        private void doSignal(Node first) {
            do {
                //修改头结点，完成旧头结点的移出工作
                if ( (firstWaiter = first.nextWaiter) == null)
                    lastWaiter = null;
                first.nextWaiter = null;
            } while (!transferForSignal(first) &&
                    (first = firstWaiter) != null);
        }
```

doSignal(Node first)主要是做两件事：修改头节点与调用transferForSignal(Node first) 方法将节点移动到CLH同步队列中。

transferForSignal(Node first)源码如下：

```java
         final boolean transferForSignal(Node node) {
            //将该节点从状态CONDITION改变为初始状态0,
            if (!compareAndSetWaitStatus(node, Node.CONDITION, 0))
                return false;
    
            //将节点加入到syn队列中去，返回的是syn队列中node节点前面的一个节点
            Node p = enq(node);
            int ws = p.waitStatus;
            //如果结点p的状态为cancel 或者修改waitStatus失败，则直接唤醒
            if (ws > 0 || !compareAndSetWaitStatus(p, ws, Node.SIGNAL))
                LockSupport.unpark(node.thread);
            return true;
        }
```

整个通知的流程如下：

1. 判断当前线程是否已经获取了锁，如果没有获取则直接抛出异常，因为获取锁为通知的前置条件。
2. 如果线程已经获取了锁，则将唤醒条件队列的首节点
3. 唤醒首节点是先将条件队列中的头节点移出，然后调用AQS的enq(Node node)方法将其安全地移到CLH同步队列中
4. 最后判断如果该节点的同步状态是否为Cancel，或者修改状态为Signal失败时，则直接调用LockSupport唤醒该节点的线程。

##### 总结

线程获取锁后，调用Condition的await()方法，将当前线程加入到条件队列中，然后释放锁，最后通过isOnSyncQueue(Node node)方法不断自检看节点是否已经在CLH同步队列了，如果是则尝试获取锁，否则一直挂起。

当线程调用signal()方法后，程序首先检查当前线程是否获取了锁，然后通过doSignal(Node first)方法唤醒CLH同步队列的首节点。被唤醒的线程，将从await()方法中的while循环中退出来，然后调用acquireQueued()方法竞争同步状态。

### ReentrantLock与synchronized的区别

前面提到ReentrantLock提供了比synchronized更加灵活和强大的锁机制

1. 与synchronized相比，ReentrantLock提供了更多，更加全面的功能，具备更强的扩展性。例如：时间锁等候，可中断锁等候，锁投票。
2. ReentrantLock还提供了条件Condition，对线程的等待、唤醒操作更加详细和灵活，所以在多个条件变量和高度竞争锁的地方，ReentrantLock更加适合。
3. ReentrantLock提供了可轮询的锁请求。它会尝试着去获取锁，如果成功则继续，否则可以等到下次运行时处理，而synchronized则一旦进入锁请求要么成功要么阻塞，所以相比synchronized而言，ReentrantLock会不容易产生死锁些。
4. ReentrantLock支持更加灵活的同步代码块，但是使用synchronized时，只能在同一个synchronized块结构中获取和释放。注：ReentrantLock的锁释放一定要在finally中处理，否则可能会产生严重的后果。
5. ReentrantLock支持中断处理，且性能较synchronized会好些。

## ReentrantReadWriteLock

通过分离读锁和写锁，使得并发性比一般的排他锁有了较大的提升：在同一时间可以允许多个读线程同时访问，但是在写线程访问时，所有读线程和写线程都会被阻塞。 读写锁的主要特性：

1. 公平性：支持公平性和非公平性。
2. 重入性：支持重入。读写锁最多支持65535个递归写入锁和65535个递归读取锁。
3. 锁降级：遵循获取写锁、获取读锁在释放写锁的次序，写锁能够降级成为读锁

读写锁ReentrantReadWriteLock实现接口ReadWriteLock，该接口维护了一对相关的锁，一个用于只读操作，另一个用于写入操作。只要没有 writer，读取锁可以由多个 reader 线程同时保持。写入锁是独占的。

```java
    public interface ReadWriteLock {
        Lock readLock();
        Lock writeLock();
    }
```

ReadWriteLock定义了两个方法。readLock()返回用于读操作的锁，writeLock()返回用于写操作的锁。ReentrantReadWriteLock定义如下：

```java
        /** 内部类  读锁 */
        private final ReentrantReadWriteLock.ReadLock readerLock;
        /** 内部类  写锁 */
        private final ReentrantReadWriteLock.WriteLock writerLock;
    
        final Sync sync;
    
        /** 使用默认（非公平）的排序属性创建一个新的 ReentrantReadWriteLock */
        public ReentrantReadWriteLock() {
            this(false);
        }
    
        /** 使用给定的公平策略创建一个新的 ReentrantReadWriteLock */
        public ReentrantReadWriteLock(boolean fair) {
            sync = fair ? new FairSync() : new NonfairSync();
            readerLock = new ReadLock(this);
            writerLock = new WriteLock(this);
        }
    
        /** 返回用于写入操作的锁 */
        public ReentrantReadWriteLock.WriteLock writeLock() { return writerLock; }
        /** 返回用于读取操作的锁 */
        public ReentrantReadWriteLock.ReadLock  readLock()  { return readerLock; }
    
        abstract static class Sync extends AbstractQueuedSynchronizer {
            /**
             * 省略其余源代码
             */
        }
        public static class WriteLock implements Lock, java.io.Serializable{
            /**
             * 省略其余源代码
             */
        }
    
        public static class ReadLock implements Lock, java.io.Serializable {
            /**
             * 省略其余源代码
             */
        }
    
```

**AQS中的锁同步机制是通过volatile的state变量进行同步的，而ReentrantReadWriteLock则是通过一个变量的高16位标识读，低16位标识写，来维护两种锁的同步。**

#### 写锁

写锁就是一个支持可重入的排他锁。

**获取**

该方法和ReentrantLock的tryAcquire(int arg)大致一样，在判断重入时增加了一项条件：读锁是否存在。因为要确保写锁的操作对读锁是可见的，只有等读锁完全释放后，写锁才能够被当前线程所获取，一旦写锁获取了，所有其他读、写线程均会被阻塞。

```java
       protected final boolean tryAcquire(int acquires) {
            Thread current = Thread.currentThread();
            //当前锁个数
            int c = getState();
            //写锁
            int w = exclusiveCount(c);
            if (c != 0) {
                //c != 0 && w == 0 表示存在读锁
                //当前线程不是已经获取写锁的线程
                if (w == 0 || current != getExclusiveOwnerThread())
                    return false;
                //超出最大范围
                if (w + exclusiveCount(acquires) > MAX_COUNT)
                    throw new Error("Maximum lock count exceeded");
                setState(c + acquires);
                return true;
            }
            //是否需要阻塞
            if (writerShouldBlock() ||
                    !compareAndSetState(c, c + acquires))
                return false;
            //设置获取锁的线程为当前线程
            setExclusiveOwnerThread(current);
            return true;
        }
```

**释放**

```java
        public void unlock() {
            sync.release(1);
        }
    
        public final boolean release(int arg) {
            if (tryRelease(arg)) {
                Node h = head;
                if (h != null && h.waitStatus != 0)
                    unparkSuccessor(h);
                return true;
            }
            return false;
        }
```

写锁的释放最终还是会调用AQS的模板方法release(int arg)方法，该方法首先调用tryRelease(int arg)方法尝试释放锁，tryRelease(int arg)方法为读写锁内部类Sync中定义了

```java
        protected final boolean tryRelease(int releases) {
            //释放的线程不为锁的持有者
            if (!isHeldExclusively())
                throw new IllegalMonitorStateException();
            int nextc = getState() - releases;
            //若写锁的新线程数为0，则将锁的持有者设置为null
            boolean free = exclusiveCount(nextc) == 0;
            if (free)
                setExclusiveOwnerThread(null);
            setState(nextc);
            return free;
        }
    
```

#### 读锁

读锁为一个可重入的共享锁，它能够被多个线程同时持有，在没有其他写线程访问时，读锁总是或获取成功。

**获取**

```java
        public void lock() {
                sync.acquireShared(1);
        }

         // 调用了AQS中的sync实现类中的acquireShared方法
        public final void acquireShared(int arg) {
            if (tryAcquireShared(arg) < 0)
                doAcquireShared(arg);
        }

        protected final int tryAcquireShared(int unused) {
            //当前线程
            Thread current = Thread.currentThread();
            int c = getState();
            //exclusiveCount(c)计算写锁
            //如果存在写锁，且锁的持有者不是当前线程，直接返回-1
            //存在锁降级问题，后续阐述
            if (exclusiveCount(c) != 0 &&
                    getExclusiveOwnerThread() != current)
                return -1;
            //读锁
            int r = sharedCount(c);
    
            /*
             * readerShouldBlock():读锁是否需要等待（公平锁原则）
             * r < MAX_COUNT：持有线程小于最大数（65535）
             * compareAndSetState(c, c + SHARED_UNIT)：设置读取锁状态
             */
            if (!readerShouldBlock() &&
                    r < MAX_COUNT &&
                    compareAndSetState(c, c + SHARED_UNIT)) {
                /*
                 * holdCount部分后面讲解
                 */
                if (r == 0) {
                    firstReader = current;
                    firstReaderHoldCount = 1;
                } else if (firstReader == current) {
                    firstReaderHoldCount++;
                } else {
                    HoldCounter rh = cachedHoldCounter;
                    if (rh == null || rh.tid != getThreadId(current))
                        cachedHoldCounter = rh = readHolds.get();
                    else if (rh.count == 0)
                        readHolds.set(rh);
                    rh.count++;
                }
                return 1;
            }
            return fullTryAcquireShared(current);
        }
    
```

读锁获取的过程相对于独占锁会复杂，整个过程如下：

1. 因为存在锁降级情况，如果存在写锁且锁的持有者不是当前线程则直接返回失败，否则继续
2. 依据公平性原则，判断读锁是否需要阻塞，读锁持有线程数小于最大值（65535），且设置锁状态成功，执行以下代码（对于HoldCounter下面再阐述），并返回1。如果不满足改条件，执行fullTryAcquireShared()。

```java
        final int fullTryAcquireShared(Thread current) {
            HoldCounter rh = null;
            for (;;) {
                int c = getState();
                //锁降级
                if (exclusiveCount(c) != 0) {
                    if (getExclusiveOwnerThread() != current)
                        return -1;
                }
                //读锁需要阻塞
                else if (readerShouldBlock()) {
                    //列头为当前线程
                    if (firstReader == current) {
                    }
                    //HoldCounter后面讲解
                    else {
                        if (rh == null) {
                            rh = cachedHoldCounter;
                            if (rh == null || rh.tid != getThreadId(current)) {
                                rh = readHolds.get();
                                if (rh.count == 0)
                                    readHolds.remove();
                            }
                        }
                        if (rh.count == 0)
                            return -1;
                    }
                }
                //读锁超出最大范围
                if (sharedCount(c) == MAX_COUNT)
                    throw new Error("Maximum lock count exceeded");
                //CAS设置读锁成功
                if (compareAndSetState(c, c + SHARED_UNIT)) {
                    //如果是第1次获取“读取锁”，则更新firstReader和firstReaderHoldCount
                    if (sharedCount(c) == 0) {
                        firstReader = current;
                        firstReaderHoldCount = 1;
                    }
                    //如果想要获取锁的线程(current)是第1个获取锁(firstReader)的线程,则将firstReaderHoldCount+1
                    else if (firstReader == current) {
                        firstReaderHoldCount++;
                    } else {
                        if (rh == null)
                            rh = cachedHoldCounter;
                        if (rh == null || rh.tid != getThreadId(current))
                            rh = readHolds.get();
                        else if (rh.count == 0)
                            readHolds.set(rh);
                        //更新线程的获取“读取锁”的共享计数
                        rh.count++;
                        cachedHoldCounter = rh; // cache for release
                    }
                    return 1;
                }
            }
        }
    
```

fullTryAcquireShared(Thread current)会根据“是否需要阻塞等待”，“读取锁的共享计数是否超过限制”等等进行处理。如果不需要阻塞等待，并且锁的共享计数没有超过限制，则通过CAS尝试获取锁，并返回1

**释放**

```java
        public void unlock() {
                sync.releaseShared(1);
        }
        
        // 使用Sync的releaseShared(int arg)方法，该方法定义在AQS中：
        public final boolean releaseShared(int arg) {
            if (tryReleaseShared(arg)) {
                doReleaseShared();
                return true;
            }
            return false;
        }

       // 调用tryReleaseShared(int arg)尝试释放读锁，该方法定义在读写锁的Sync内部类中
       protected final boolean tryReleaseShared(int unused) {
            Thread current = Thread.currentThread();
            //如果想要释放锁的线程为第一个获取锁的线程
            if (firstReader == current) {
                //仅获取了一次，则需要将firstReader 设置null，否则 firstReaderHoldCount - 1
                if (firstReaderHoldCount == 1)
                    firstReader = null;
                else
                    firstReaderHoldCount--;
            }
            //获取rh对象，并更新“当前线程获取锁的信息”
            else {
                HoldCounter rh = cachedHoldCounter;
                if (rh == null || rh.tid != getThreadId(current))
                    rh = readHolds.get();
                int count = rh.count;
                if (count <= 1) {
                    readHolds.remove();
                    if (count <= 0)
                        throw unmatchedUnlockException();
                }
                --rh.count;
            }
            //CAS更新同步状态
            for (;;) {
                int c = getState();
                int nextc = c - SHARED_UNIT;
                if (compareAndSetState(c, nextc))
                    return nextc == 0;
            }
        }
    
```

#### HoldCounter

HoldCounter的作用就是当前线程持有共享锁的数量，这个数量必须要与线程绑定在一起，否则操作其他线程锁就会抛出异常。

```java
            static final class HoldCounter {
                int count = 0;
                final long tid = getThreadId(Thread.currentThread());
            }
```

HoldCounter 是需要和某线程进行绑定，通过ThreadLocal来对tid和线程达到绑定作用。

```java
            static final class ThreadLocalHoldCounter
                extends ThreadLocal<HoldCounter> {
                public HoldCounter initialValue() {
                    return new HoldCounter();
                }
            }
```

需要说明的是这样HoldCounter绑定线程id而不绑定线程对象的原因是避免HoldCounter和ThreadLocal互相绑定而GC难以释放它们，所以其实这样做只是为了帮助GC快速回收对象而已。

以获取读锁的代码分析

```java
                    else if (firstReader == current) {
                        firstReaderHoldCount++;
                    } else {
                        if (rh == null)
                            rh = cachedHoldCounter;
                        if (rh == null || rh.tid != getThreadId(current))
                            rh = readHolds.get();
                        else if (rh.count == 0)
                            readHolds.set(rh);
                        rh.count++;
                        cachedHoldCounter = rh; // cache for release
                    }
```

这段代码涉及了几个变量：firstReader 、firstReaderHoldCount、cachedHoldCounter 。

```java
  private transient Thread firstReader = null;   // 第一个获取读锁的线程
  private transient int firstReaderHoldCount;    // 第一个获取读锁的重入数
  private transient HoldCounter cachedHoldCounter; // HoldCounter的缓存
```

```java
     //如果获取读锁的线程为第一次获取读锁的线程，则firstReaderHoldCount重入数 + 1
        else if (firstReader == current) {
            firstReaderHoldCount++;
        } else {
            //非firstReader计数
            if (rh == null)
                rh = cachedHoldCounter;
            //rh == null 或者 rh.tid != current.getId()，需要获取rh
            if (rh == null || rh.tid != getThreadId(current))
                rh = readHolds.get();
                //加入到readHolds中
            else if (rh.count == 0)
                readHolds.set(rh);
            //计数+1
            rh.count++;
            cachedHoldCounter = rh; // cache for release
        }
    
```

这里解释下为何要引入firstRead、firstReaderHoldCount。这是为了一个效率问题，firstReader是不会放入到readHolds中的，如果读锁仅有一个的情况下就会避免查找readHolds。

#### 锁降级

锁降级就意味着写锁是可以降级为读锁的，但需遵循先获取写锁、获取读锁在释放写锁的次序。 

## ThreadLocal

ThreadLocal提供了线程的局部变量，每个线程都可以通过`set()`和`get()`来对这个局部变量进行操作，但不会和其他线程的局部变量进行冲突，**实现了线程的数据隔离**。

![9c0cf8412fd7738dc6e88813543c7cd4.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/9c0cf8412fd7738dc6e88813543c7cd4.png)

**其中，Map存放的是ThreadLocal的弱引用。**

1. 引用ThreadLocal的对象被回收了，但是ThreadLocalMap还持有ThreadLocal的强引用，如果没有手动删除，ThreadLocal不会被回收，导致Entry内存泄漏。

2. 引用ThreadLocal的对象被回收了，由于ThreadLocalMap持有ThreadLocal的弱引用，即使没有手动删除，ThreadLocal也会被回收。value在下一次ThreadLocalMap调用set、get、remove的时候会被清除。

**ThreadLocal内存泄漏的根源是：由于ThreadLocalMap的生命周期跟Thread一样长，如果没有手动删除对应key就会导致内存泄漏，而不是因为弱引用。**

**set()方法设置值时**

```java
public void set(T value) {

		// 得到当前线程对象
        Thread t = Thread.currentThread();
		
		// 这里获取ThreadLocalMap
        ThreadLocalMap map = getMap(t);

		// 如果map存在，则将当前线程对象t作为key，要存储的对象作为value存到map里面去
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
    }
```

**如果map不存在，则从Thread里面获取**

```java
void createMap(Thread t, T firstValue) {
        t.threadLocals = new ThreadLocalMap(this, firstValue);
}
```

****

**threadLocals是由Thread维护的Map变量**

```java
ThreadLocal.ThreadLocalMap threadLocals = null
```

**而ThreadLocalMap是Thread的内部静态类，且对于不同的ThreadLocal对象，都有唯一的hash值，因为ThreadLocal内部采用了原子类来保证hash值，且每次初始化hash都会自增**

```java
static class ThreadLocalMap {
    static class Entry extends WeakReference<ThreadLocal<?>> {

        Object value;

        Entry(ThreadLocal<?> k, Object v) {
            super(k);
            value = v;
        }
    }
	//....很长
    }
```

**get方法**

```java
    public T get() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null) {
                @SuppressWarnings("unchecked")
                T result = (T)e.value;
                return result;
            }
        }
        return setInitialValue();
    }

```

## 线程池

### Executor

Executor，任务的执行者，线程池框架中几乎所有类都直接或者间接实现Executor接口，它是线程池框架的基础。

```java
    public interface Executor {
        void execute(Runnable command);
    }

```

#### ExcutorService

继承Executor，它是“执行者服务”接口，它是为"执行者接口Executor"服务而存在的。

```java
    public interface ExecutorService extends Executor {
    
        /**
         * 启动一次顺序关闭，执行以前提交的任务，但不接受新任务
         */
        void shutdown();
    
        /**
         * 试图停止所有正在执行的活动任务，暂停处理正在等待的任务，并返回等待执行的任务列表
         */
        List<Runnable> shutdownNow();
    
        /**
         * 如果此执行程序已关闭，则返回 true。
         */
        boolean isShutdown();
    
        /**
         * 如果关闭后所有任务都已完成，则返回 true
         */
        boolean isTerminated();
    
        /**
         * 请求关闭、发生超时或者当前线程中断，无论哪一个首先发生之后，都将导致阻塞，直到所有任务完成执行
         */
        boolean awaitTermination(long timeout, TimeUnit unit)
            throws InterruptedException;
    
        /**
         * 提交一个返回值的任务用于执行，返回一个表示任务的未决结果的 Future
         */
        <T> Future<T> submit(Callable<T> task);
    
        /**
         * 提交一个 Runnable 任务用于执行，并返回一个表示该任务的 Future
         */
        <T> Future<T> submit(Runnable task, T result);
    
        /**
         * 提交一个 Runnable 任务用于执行，并返回一个表示该任务的 Future
         */
        Future<?> submit(Runnable task);
    
        /**
         * 执行给定的任务，当所有任务完成时，返回保持任务状态和结果的 Future 列表
         */
        <T> List<Future<T>> invokeAll(Collection<? extends Callable<T>> tasks)
            throws InterruptedException;
    
        /**
         * 执行给定的任务，当所有任务完成或超时期满时（无论哪个首先发生），返回保持任务状态和结果的 Future 列表
         */
        <T> List<Future<T>> invokeAll(Collection<? extends Callable<T>> tasks,
                                      long timeout, TimeUnit unit)
            throws InterruptedException;
    
        /**
         * 执行给定的任务，如果某个任务已成功完成（也就是未抛出异常），则返回其结果
         */
        <T> T invokeAny(Collection<? extends Callable<T>> tasks)
            throws InterruptedException, ExecutionException;
    
        /**
         * 执行给定的任务，如果在给定的超时期满前某个任务已成功完成（也就是未抛出异常），则返回其结果
         */
        <T> T invokeAny(Collection<? extends Callable<T>> tasks,
                        long timeout, TimeUnit unit)
            throws InterruptedException, ExecutionException, TimeoutException;
    }
```

#### AbstractExecutorService

抽象类，实现ExecutorService接口，为其提供默认实现。AbstractExecutorService除了实现ExecutorService接口外，还提供了newTaskFor()方法返回一个RunnableFuture，在运行的时候，它将调用底层可调用任务，作为 Future 任务，它将生成可调用的结果作为其结果，并为底层任务提供取消操作。

#### ScheduledExecutorService

继承ExcutorService，为一个“延迟”和“定期执行”的ExecutorService。他提供了一些如下几个方法安排任务在给定的延时执行或者周期性执行。

```java
    // 创建并执行在给定延迟后启用的 ScheduledFuture。
    <V> ScheduledFuture<V> schedule(Callable<V> callable, long delay, TimeUnit unit)
    
    // 创建并执行在给定延迟后启用的一次性操作。
    ScheduledFuture<?> schedule(Runnable command, long delay, TimeUnit unit)
    
    // 创建并执行一个在给定初始延迟后首次启用的定期操作，后续操作具有给定的周期；
    //也就是将在 initialDelay 后开始执行，然后在 initialDelay+period 后执行，接着在 initialDelay + 2 * period 后执行，依此类推。
    ScheduledFuture<?> scheduleAtFixedRate(Runnable command, long initialDelay, long period, TimeUnit unit)
    
    // 创建并执行一个在给定初始延迟后首次启用的定期操作，随后，在每一次执行终止和下一次执行开始之间都存在给定的延迟。
    ScheduledFuture<?> scheduleWithFixedDelay(Runnable command, long initialDelay, long delay, TimeUnit unit)
```

**ThreadPoolExecutor**

大名鼎鼎的“线程池”。

**ScheduledThreadPoolExecutor**

ScheduledThreadPoolExecutor继承ThreadPoolExecutor并且实现ScheduledExecutorService接口，是两者的集大成者，相当于提供了“延迟”和“周期执行”功能的ThreadPoolExecutor。

**Executors**

静态工厂类，提供了Executor、ExecutorService、ScheduledExecutorService、ThreadFactory 、Callable 等类的静态工厂方法，通过这些工厂方法我们可以得到相对应的对象。

### Future

作为异步计算的顶层接口，Future对具体的Runnable或者Callable任务提供了三种操作：执行任务的取消、查询任务是否完成、获取任务的执行结果。其接口定义如下：

```java
    public interface Future<V> {
    
        /**
         * 试图取消对此任务的执行
         * 如果任务已完成、或已取消，或者由于某些其他原因而无法取消，则此尝试将失败。
         * 当调用 cancel 时，如果调用成功，而此任务尚未启动，则此任务将永不运行。
         * 如果任务已经启动，则 mayInterruptIfRunning 参数确定是否应该以试图停止任务的方式来中断执行此任务的线程
         */
        boolean cancel(boolean mayInterruptIfRunning);
    
        /**
         * 如果在任务正常完成前将其取消，则返回 true
         */
        boolean isCancelled();
    
        /**
         * 如果任务已完成，则返回 true
         */
        boolean isDone();
    
        /**
         *   如有必要，等待计算完成，然后获取其结果
         */
        V get() throws InterruptedException, ExecutionException;
    
        /**
         * 如有必要，最多等待为使计算完成所给定的时间之后，获取其结果（如果结果可用）
         */
        V get(long timeout, TimeUnit unit)
            throws InterruptedException, ExecutionException, TimeoutException;
    }
```

**RunnableFuture**

继承Future、Runnable两个接口，为两者的合体，即所谓的Runnable的Future。提供了一个run()方法可以完成Future并允许访问其结果。

```java
    public interface RunnableFuture<V> extends Runnable, Future<V> {
        //在未被取消的情况下，将此 Future 设置为计算的结果
        void run();
    }
```

**FutureTask**

实现RunnableFuture接口，既可以作为Runnable被执行，也可以作为Future得到Callable的返回值。

### ThreadPoolExecutor

#### 内部状态

线程有五种状态：新建，就绪，运行，阻塞，死亡，线程池同样有五种状态：Running, SHUTDOWN, STOP, TIDYING, TERMINATED。

```java
      private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
        private static final int COUNT_BITS = Integer.SIZE - 3;
        private static final int CAPACITY   = (1 << COUNT_BITS) - 1;
    
        // runState is stored in the high-order bits
        private static final int RUNNING    = -1 << COUNT_BITS;  // 111
        private static final int SHUTDOWN   =  0 << COUNT_BITS;  // 000
        private static final int STOP       =  1 << COUNT_BITS;  // 001
        private static final int TIDYING    =  2 << COUNT_BITS;  // 010
        private static final int TERMINATED =  3 << COUNT_BITS;  // 011
    
        // Packing and unpacking ctl
        private static int runStateOf(int c)     { return c & ~CAPACITY; }
        private static int workerCountOf(int c)  { return c & CAPACITY; }
        private static int ctlOf(int rs, int wc) { return rs | wc; }
```

变量**ctl**定义为AtomicInteger ，其功能非常强大，记录了“线程池中的任务数量”和“线程池的状态”两个信息。共32位，其中高3位表示"线程池状态"，低29位表示"线程池中的任务数量"。

**RUNNING**：处于RUNNING状态的线程池能够接受新任务，以及对新添加的任务进行处理。

**SHUTDOWN**：处于SHUTDOWN状态的线程池不可以接受新任务，但是可以对已添加的任务进行处理。

**STOP**：处于STOP状态的线程池不接收新任务，不处理已添加的任务，并且会中断正在处理的任务。

**TIDYING**：当所有的任务已终止，ctl记录的"任务数量"为0，线程池会变为TIDYING状态。当线程池变为TIDYING状态时，会执行钩子函数terminated()。terminated()在ThreadPoolExecutor类中是空的，若用户想在线程池变为TIDYING时，进行相应的处理；可以通过重载terminated()函数来实现。

**TERMINATED**：线程池彻底终止的状态。

各个状态的转换如下：

![202105091544329011.png](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/202105091544329011.png)

#### 创建线程池

通过ThreadPollExecutor的构造函数

```java
        public ThreadPoolExecutor(int corePoolSize,
                                  int maximumPoolSize,
                                  long keepAliveTime,
                                  TimeUnit unit,
                                  BlockingQueue<Runnable> workQueue,
                                  ThreadFactory threadFactory,
                                  RejectedExecutionHandler handler) {
            if (corePoolSize < 0 ||
                maximumPoolSize <= 0 ||
                maximumPoolSize < corePoolSize ||
                keepAliveTime < 0)
                throw new IllegalArgumentException();
            if (workQueue == null || threadFactory == null || handler == null)
                throw new NullPointerException();
            this.corePoolSize = corePoolSize;
            this.maximumPoolSize = maximumPoolSize;
            this.workQueue = workQueue;
            this.keepAliveTime = unit.toNanos(keepAliveTime);
            this.threadFactory = threadFactory;
            this.handler = handler;
        }
```

共有七个参数，每个参数含义如下：

**corePoolSize**：**线程池中核心线程的数量。**当提交一个任务时，线程池会新建一个线程来执行任务，直到当前线程数等于corePoolSize。如果调用了线程池的prestartAllCoreThreads()方法，线程池会提前创建并启动所有基本线程。

**maximumPoolSize**：**线程池中允许的最大线程数。**线程池的阻塞队列满了之后，如果还有任务提交，如果当前的线程数小于maximumPoolSize，则会新建线程来执行任务。注意，如果使用的是无界队列，该参数也就没有什么效果了。

**keepAliveTime**：**线程空闲的时间。**线程的创建和销毁是需要代价的。线程执行完任务后不会立即销毁，而是继续存活一段时间：keepAliveTime。默认情况下，该参数只有在线程数大于corePoolSize时才会生效。

**unit**：**keepAliveTime的单位。**

**workQueue**：***用来保存等待执行的任务的阻塞队列，等待的任务必须实现Runnable接口***。我们可以选择如下几种：

- ArrayBlockingQueue：基于数组结构的有界阻塞队列，FIFO。
- LinkedBlockingQueue：基于链表结构的有界阻塞队列，FIFO。
- SynchronousQueue：不存储元素的阻塞队列，每个插入操作都必须等待一个移出操作，反之亦然。
- PriorityBlockingQueue：具有优先界别的阻塞队列。

**threadFactory**

用于设置创建线程的工厂。该对象可以通过Executors.defaultThreadFactory()，返回的是DefaultThreadFactory对象.

```java
        public static ThreadFactory defaultThreadFactory() {
            return new DefaultThreadFactory();
        }


        static class DefaultThreadFactory implements ThreadFactory {
            private static final AtomicInteger poolNumber = new AtomicInteger(1);
            private final ThreadGroup group;
            private final AtomicInteger threadNumber = new AtomicInteger(1);
            private final String namePrefix;
    
            DefaultThreadFactory() {
                SecurityManager s = System.getSecurityManager();
                group = (s != null) ? s.getThreadGroup() :
                                      Thread.currentThread().getThreadGroup();
                namePrefix = "pool-" +
                              poolNumber.getAndIncrement() +
                             "-thread-";
            }
    
            public Thread newThread(Runnable r) {
                Thread t = new Thread(group, r,
                                      namePrefix + threadNumber.getAndIncrement(),
                                      0);
                if (t.isDaemon())
                    t.setDaemon(false);
                if (t.getPriority() != Thread.NORM_PRIORITY)
                    t.setPriority(Thread.NORM_PRIORITY);
                return t;
            }
        }
```

ThreadFactory的左右就是提供创建线程的功能的线程工厂。通过newThread()方法提供创建线程，且创建的线程都是“非守护线程”而且“线程优先级都是Thread.NORM_PRIORITY”。

**handler**：**RejectedExecutionHandler，线程池的拒绝策略**。拒绝策略指将任务添加到线程池中时，线程池拒绝该任务所采取的相应策略。当向线程池中提交任务时，如果此时线程池中的线程与阻塞队列已满，则线程池会选择一种拒绝策略来处理该任务。

线程池提供了四种拒绝策略：

1. AbortPolicy：直接抛出异常，默认策略；
2. CallerRunsPolicy：用调用者所在的线程来执行任务；
3. DiscardOldestPolicy：丢弃阻塞队列中靠最前的任务，并执行当前任务；
4. DiscardPolicy：直接丢弃任务；

#### 内部线程池

Executor框架提供了三种线程池，他们都可以通过工具类Executors来创建。

**FixedThreadPool**

FixedThreadPool，可重用固定线程数的线程池，其定义如下：

```java
        public static ExecutorService newFixedThreadPool(int nThreads) {         // 易OOM
            return new ThreadPoolExecutor(nThreads, nThreads,
                                          0L, TimeUnit.MILLISECONDS,
                                          new LinkedBlockingQueue<Runnable>());
        }
```

**SingleThreadExecutor**

SingleThreadExecutor是使用单个worker线程的Executor，定义如下：

```java
        public static ExecutorService newSingleThreadExecutor() {          // 易OOM
            return new FinalizableDelegatedExecutorService
                (new ThreadPoolExecutor(1, 1,
                                        0L, TimeUnit.MILLISECONDS,
                                        new LinkedBlockingQueue<Runnable>()));
        }
```

**CachedThreadPool**

CachedThreadPool是一个会根据需要创建新线程的线程池 ，定义如下：

```java
        public static ExecutorService newCachedThreadPool() {
            return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                          60L, TimeUnit.SECONDS,
                                          new SynchronousQueue<Runnable>());
        }
```

若主线程提交任务的速度远远大于CachedThreadPool的处理速度，则CachedThreadPool会不断地创建新线程来执行任务，这样有可能会导致系统耗尽CPU和内存资源，所以在**使用该线程池是，一定要注意控制并发的任务数，否则创建大量的线程可能导致严重的性能问题**。

#### 任务提交

线程池根据业务不同的需求提供了两种方式提交任务：Executor.execute()、ExecutorService.submit()。其中ExecutorService.submit()可以获取该任务执行的Future。 

```java
    public interface Executor {
    
        void execute(Runnable command);
    }
```

ThreadPoolExecutor提供实现：

```java
        public void execute(Runnable command) {
            if (command == null)
                throw new NullPointerException();
            int c = ctl.get();
            if (workerCountOf(c) < corePoolSize) {
                if (addWorker(command, true))
                    return;
                c = ctl.get();
            }
            if (isRunning(c) && workQueue.offer(command)) {
                int recheck = ctl.get();
                if (! isRunning(recheck) && remove(command))
                    reject(command);
                else if (workerCountOf(recheck) == 0)
                    addWorker(null, false);
            }
            else if (!addWorker(command, false))
                reject(command);
        }
```

通过源代码中的ctl变量高3位和低29位执行判断逻辑，执行流程如下：

1. 若线程池当前线程数小于corePoolSize，则调用addWorker创建新线程执行任务，成功返回true，失败执行步骤2。
2. 若线程池处于RUNNING状态，则尝试加入阻塞队列，如果加入阻塞队列成功，则尝试进行Double Check，如果加入失败，则执行步骤3。
3. 若线程池不是RUNNING状态或者加入阻塞队列失败，则尝试创建新线程直到maxPoolSize，如果失败，则调用reject()方法运行相应的拒绝策略。

addWorker(Runnable firstTask, boolean core)方法用于创建线程执行任务，源码如下：

```java
        private boolean addWorker(Runnable firstTask, boolean core) {
            retry:
            for (;;) {
                int c = ctl.get();
    
                // 获取当前线程状态
                int rs = runStateOf(c);
    
                if (rs >= SHUTDOWN &&
                        ! (rs == SHUTDOWN &&
                                firstTask == null &&
                                ! workQueue.isEmpty()))
                    return false;
    
                // 内层循环，worker + 1
                for (;;) {
                    // 线程数量
                    int wc = workerCountOf(c);
                    // 如果当前线程数大于线程最大上限CAPACITY  return false
                    // 若core == true，则与corePoolSize 比较，否则与maximumPoolSize ，大于 return false
                    if (wc >= CAPACITY ||
                            wc >= (core ? corePoolSize : maximumPoolSize))
                        return false;
                    // worker + 1,成功跳出retry循环
                    if (compareAndIncrementWorkerCount(c))
                        break retry;
    
                    // CAS add worker 失败，再次读取ctl
                    c = ctl.get();
    
                    // 如果状态不等于之前获取的state，跳出内层循环，继续去外层循环判断
                    if (runStateOf(c) != rs)
                        continue retry;
                }
            }
    
            boolean workerStarted = false;
            boolean workerAdded = false;
            Worker w = null;
            try {
    
                // 新建线程：Worker
                w = new Worker(firstTask);
                // 当前线程
                final Thread t = w.thread;
                if (t != null) {
                    // 获取主锁：mainLock
                    final ReentrantLock mainLock = this.mainLock;
                    mainLock.lock();
                    try {
    
                        // 线程状态
                        int rs = runStateOf(ctl.get());
    
                        // rs < SHUTDOWN ==> 线程处于RUNNING状态
                        // 或者线程处于SHUTDOWN状态，且firstTask == null（可能是workQueue中仍有未执行完成的任务，创建没有初始任务的worker线程执行）
                        if (rs < SHUTDOWN ||
                                (rs == SHUTDOWN && firstTask == null)) {
    
                            // 当前线程已经启动，抛出异常
                            if (t.isAlive()) // precheck that t is startable
                                throw new IllegalThreadStateException();
    
                            // workers是一个HashSet<Worker>
                            workers.add(w);
    
                            // 设置最大的池大小largestPoolSize，workerAdded设置为true
                            int s = workers.size();
                            if (s > largestPoolSize)
                                largestPoolSize = s;
                            workerAdded = true;
                        }
                    } finally {
                        // 释放锁
                        mainLock.unlock();
                    }
                    // 启动线程
                    if (workerAdded) {
                        t.start();
                        workerStarted = true;
                    }
                }
            } finally {
    
                // 线程启动失败
                if (! workerStarted)
                    addWorkerFailed(w);
            }
            return workerStarted;
        }
```

1. 判断当前线程是否可以添加任务，如果可以则进行下一步，否则return false；
   1. rs >= SHUTDOWN ，表示当前线程处于SHUTDOWN ，STOP、TIDYING、TERMINATED状态
   2. rs == SHUTDOWN , firstTask != null时不允许添加线程，因为线程处于SHUTDOWN 状态，不允许添加任务
   3. rs == SHUTDOWN , firstTask == null，但workQueue.isEmpty() == true，不允许添加线程，因为firstTask == null是为了添加一个没有任务的线程然后再从workQueue中获取任务的，如果workQueue == null，则说明添加的任务没有任何意义。
2. 内嵌循环，通过CAS worker + 1
3. 获取主锁mailLock，如果线程池处于RUNNING状态获取处于SHUTDOWN状态且 firstTask == null，则将任务添加到workers Queue中，然后释放主锁mainLock，然后启动线程，然后return true，如果中途失败导致workerStarted= false，则调用addWorkerFailed()方法进行处理。

**Woker内部类**

Woker的源码如下：

```java
        private final class Worker extends AbstractQueuedSynchronizer
                implements Runnable {
            private static final long serialVersionUID = 6138294804551838833L;
    
            // task 的thread
            final Thread thread;
    
            // 运行的任务task
            Runnable firstTask;
    
            volatile long completedTasks;
    
            Worker(Runnable firstTask) {
    
                //设置AQS的同步状态private volatile int state，是一个计数器，大于0代表锁已经被获取
                setState(-1);
                this.firstTask = firstTask;
    
                // 利用ThreadFactory和 Worker这个Runnable创建的线程对象
                this.thread = getThreadFactory().newThread(this);
            }
    
            // 任务执行
            public void run() {
                runWorker(this);
            }
    
        }
```

Woker继承AQS，实现Runnable接口，所以Worker既可以执行任务，也可以达到获取锁释放锁的效果。这里继承AQS主要是为了方便线程的中断处理。

构造函数主要是做三件事：

- 1.设置同步状态state为-1，同步状态大于0表示就已经获取了锁
- 2.设置将当前任务task设置为firstTask
- 3.利用Worker本身对象this和ThreadFactory创建线程对象。

当线程thread启动（调用start()方法）时，其实就是执行Worker的run()方法，内部调用runWorker()。

**runWorker**

```java
        final void runWorker(Worker w) {
    
            // 当前线程
            Thread wt = Thread.currentThread();
    
            // 要执行的任务
            Runnable task = w.firstTask;
    
            w.firstTask = null;
    
            // 释放锁，运行中断
            w.unlock(); // allow interrupts
            boolean completedAbruptly = true;
            try {
                while (task != null || (task = getTask()) != null) {
                    // worker 获取锁
                    w.lock();
    
                    // 确保只有当线程是stoping时，才会被设置为中断，否则清楚中断标示
                    // 如果线程池状态 >= STOP ,且当前线程没有设置中断状态，则wt.interrupt()
                    // 如果线程池状态 < STOP，但是线程已经中断了，再次判断线程池是否 >= STOP，如果是 wt.interrupt()
                    if ((runStateAtLeast(ctl.get(), STOP) ||
                            (Thread.interrupted() &&
                                    runStateAtLeast(ctl.get(), STOP))) &&
                            !wt.isInterrupted())
                        wt.interrupt();
                    try {
                        // 自定义方法
                        beforeExecute(wt, task);
                        Throwable thrown = null;
                        try {
                            // 执行任务
                            task.run();
                        } catch (RuntimeException x) {
                            thrown = x; throw x;
                        } catch (Error x) {
                            thrown = x; throw x;
                        } catch (Throwable x) {
                            thrown = x; throw new Error(x);
                        } finally {
                            afterExecute(task, thrown);
                        }
                    } finally {
                        task = null;
                        // 完成任务数 + 1
                        w.completedTasks++;
                        // 释放锁
                        w.unlock();
                    }
                }
                completedAbruptly = false;
            } finally {
                processWorkerExit(w, completedAbruptly);
            }
        }
```

运行流程

1. 根据worker获取要执行的任务task，然后调用unlock()方法释放锁，这里释放锁的主要目的在于中断，因为在new Worker时，设置的state为-1，调用unlock()方法可以将state设置为0，这里主要原因就在于interruptWorkers()方法只有在state >= 0时才会执行；
2. 通过getTask()获取执行的任务，调用task.run()执行，当然在执行之前会调用worker.lock()上锁，执行之后调用worker.unlock()放锁；
3. 在任务执行前后，可以根据业务场景自定义beforeExecute() 和 afterExecute()方法，则两个方法在ThreadPoolExecutor中是空实现；
4. 如果线程执行完成，则会调用getTask()方法从阻塞队列中获取新任务，如果阻塞队列为空，则根据是否超时来判断是否需要阻塞；
5. task == null或者抛出异常（beforeExecute()、task.run()、afterExecute()均有可能）导致worker线程终止，则调用processWorkerExit()方法处理worker退出流程。

**getTask()**

```java
       private Runnable getTask() {
            boolean timedOut = false; // Did the last poll() time out?
    
            for (;;) {
    
                // 线程池状态
                int c = ctl.get();
                int rs = runStateOf(c);
    
                // 线程池中状态 >= STOP 或者 线程池状态 == SHUTDOWN且阻塞队列为空，则worker - 1，return null
                if (rs >= SHUTDOWN && (rs >= STOP || workQueue.isEmpty())) {
                    decrementWorkerCount();
                    return null;
                }
    
                int wc = workerCountOf(c);
    
                // 判断是否需要超时控制
                boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;
    
                if ((wc > maximumPoolSize || (timed && timedOut)) && (wc > 1 || workQueue.isEmpty())) {
                    if (compareAndDecrementWorkerCount(c))
                        return null;
                    continue;
                }
    
                try {
    
                    // 从阻塞队列中获取task
                    // 如果需要超时控制，则调用poll()，否则调用take()
                    Runnable r = timed ?
                            workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :
                            workQueue.take();
                    if (r != null)
                        return r;
                    timedOut = true;
                } catch (InterruptedException retry) {
                    timedOut = false;
                }
            }
        }
```

timed == true，调用poll()方法，如果在keepAliveTime时间内还没有获取task的话，则返回null，继续循环。timed == false，则调用take()方法，该方法为一个阻塞方法，没有任务时会一直阻塞挂起，直到有任务加入时对该线程唤醒，返回任务。

在runWorker()方法中，无论最终结果如何，都会执行processWorkerExit()方法对worker进行退出处理。

**processWorkerExit()**

```java
        private void processWorkerExit(Worker w, boolean completedAbruptly) {
    
            // true：用户线程运行异常,需要扣减
            // false：getTask方法中扣减线程数量
            if (completedAbruptly)
                decrementWorkerCount();
    
            // 获取主锁
            final ReentrantLock mainLock = this.mainLock;
            mainLock.lock();
            try {
                completedTaskCount += w.completedTasks;
                // 从HashSet中移出worker
                workers.remove(w);
            } finally {
                mainLock.unlock();
            }
    
            // 有worker线程移除，可能是最后一个线程退出需要尝试终止线程池
            tryTerminate();
    
            int c = ctl.get();
            // 如果线程为running或shutdown状态，即tryTerminate()没有成功终止线程池，则判断是否有必要一个worker
            if (runStateLessThan(c, STOP)) {
                // 正常退出，计算min：需要维护的最小线程数量
                if (!completedAbruptly) {
                    // allowCoreThreadTimeOut 默认false：是否需要维持核心线程的数量
                    int min = allowCoreThreadTimeOut ? 0 : corePoolSize;
                    // 如果min ==0 或者workerQueue为空，min = 1
                    if (min == 0 && ! workQueue.isEmpty())
                        min = 1;
    
                    // 如果线程数量大于最少数量min，直接返回，不需要新增线程
                    if (workerCountOf(c) >= min)
                        return; // replacement not needed
                }
                // 添加一个没有firstTask的worker
                addWorker(null, false);
            }
        }
```

首先completedAbruptly的值来判断是否需要对线程数-1处理，如果completedAbruptly == true，说明在任务运行过程中出现了异常，那么需要进行减1处理，否则不需要，因为减1处理在getTask()方法中处理了。然后从HashSet中移出该worker，过程需要获取mainlock。然后调用tryTerminate()方法处理，该方法是对最后一个线程退出做终止线程池动作。如果线程池没有终止，那么线程池需要保持一定数量的线程，则通过addWorker(null,false)新增一个空的线程。

**addWorkerFailed()**

在addWorker()方法中，如果线程t==null，或者在add过程出现异常，会导致workerStarted == false，那么在最后会调用addWorkerFailed()方法：

```java
       private void addWorkerFailed(Worker w) {
            final ReentrantLock mainLock = this.mainLock;
            mainLock.lock();
            try {
                // 从HashSet中移除该worker
                if (w != null)
                    workers.remove(w);
    
                // 线程数 - 1
                decrementWorkerCount();
                // 尝试终止线程
                tryTerminate();
            } finally {
                mainLock.unlock();
            }
        }
```

整个逻辑显得比较简单。

**tryTerminate()**

当线程池涉及到要移除worker时候都会调用tryTerminate()，该方法主要用于判断线程池中的线程是否已经全部移除了，如果是的话则关闭线程池。

```java
        final void tryTerminate() {
            for (;;) {
                int c = ctl.get();
                // 线程池处于Running状态
                // 线程池已经终止了
                // 线程池处于ShutDown状态，但是阻塞队列不为空
                if (isRunning(c) ||
                        runStateAtLeast(c, TIDYING) ||
                        (runStateOf(c) == SHUTDOWN && ! workQueue.isEmpty()))
                    return;
    
                // 执行到这里，就意味着线程池要么处于STOP状态，要么处于SHUTDOWN且阻塞队列为空
                // 这时如果线程池中还存在线程，则会尝试中断线程
                if (workerCountOf(c) != 0) {
                    // /线程池还有线程，但是队列没有任务了，需要中断唤醒等待任务的线程
                    // （runwoker的时候首先就通过w.unlock设置线程可中断，getTask最后面的catch处理中断）
                    interruptIdleWorkers(ONLY_ONE);
                    return;
                }
    
                final ReentrantLock mainLock = this.mainLock;
                mainLock.lock();
                try {
                    // 尝试终止线程池
                    if (ctl.compareAndSet(c, ctlOf(TIDYING, 0))) {
                        try {
                            terminated();
                        } finally {
                            // 线程池状态转为TERMINATED
                            ctl.set(ctlOf(TERMINATED, 0));
                            termination.signalAll();
                        }
                        return;
                    }
                } finally {
                    mainLock.unlock();
                }
            }
        }
```

在关闭线程池的过程中，如果线程池处于STOP状态或者处于SHUDOWN状态且阻塞队列为null，则线程池会调用interruptIdleWorkers()方法中断所有线程，注意ONLY_ONE== true，表示仅中断一个线程。

**interruptIdleWorkers**

```java
        private void interruptIdleWorkers(boolean onlyOne) {
            final ReentrantLock mainLock = this.mainLock;
            mainLock.lock();
            try {
                for (Worker w : workers) {
                    Thread t = w.thread;
                    if (!t.isInterrupted() && w.tryLock()) {
                        try {
                            t.interrupt();
                        } catch (SecurityException ignore) {
                        } finally {
                            w.unlock();
                        }
                    }
                    if (onlyOne)
                        break;
                }
            } finally {
                mainLock.unlock();
            }
        }
```

onlyOne==true仅终止一个线程，否则终止所有线程。

#### 线程终止

线程池ThreadPoolExecutor提供了shutdown()和shutDownNow()用于关闭线程池。

shutdown()：按过去执行已提交任务的顺序发起一个有序的关闭，但是不接受新任务。

shutdownNow() :尝试停止所有的活动执行任务、暂停等待任务的处理，并返回等待执行的任务列表。

**shutdown**

```java
        public void shutdown() {
            final ReentrantLock mainLock = this.mainLock;
            mainLock.lock();
            try {
                checkShutdownAccess();
                // 推进线程状态
                advanceRunState(SHUTDOWN);
                // 中断空闲的线程
                interruptIdleWorkers();
                // 交给子类实现
                onShutdown();
            } finally {
                mainLock.unlock();
            }
            tryTerminate();
        }
```

**shutdownNow**

```java
       public List<Runnable> shutdownNow() {
            List<Runnable> tasks;
            final ReentrantLock mainLock = this.mainLock;
            mainLock.lock();
            try {
                checkShutdownAccess();
                advanceRunState(STOP);
                // 中断所有线程
                interruptWorkers();
                // 返回等待执行的任务列表
                tasks = drainQueue();
            } finally {
                mainLock.unlock();
            }
            tryTerminate();
            return tasks;
        }
```

与shutdown不同，shutdownNow会调用interruptWorkers()方法中断所有线程。

```java
        private void interruptWorkers() {
            final ReentrantLock mainLock = this.mainLock;
            mainLock.lock();
            try {
                for (Worker w : workers)
                    w.interruptIfStarted();
            } finally {
                mainLock.unlock();
            }
        }
```

同时会调用drainQueue()方法返回等待执行到任务列表。

```java
        private List<Runnable> drainQueue() {
            BlockingQueue<Runnable> q = workQueue;
            ArrayList<Runnable> taskList = new ArrayList<Runnable>();
            q.drainTo(taskList);
            if (!q.isEmpty()) {
                for (Runnable r : q.toArray(new Runnable[0])) {
                    if (q.remove(r))
                        taskList.add(r);
                }
            }
            return taskList;
        }
```

### ScheduledThreadPoolExecutor

对于定期、周期执行任务的调度策略，我们一般都是推荐ScheduledThreadPoolExecutor来实现。

 ScheduledThreadPoolExecutor，继承ThreadPoolExecutor且实现了ScheduledExecutorService接口，它就相当于提供了“延迟”和“周期执行”功能的ThreadPoolExecutor。

提供了四个构造方法：

```java
        public ScheduledThreadPoolExecutor(int corePoolSize) {
            super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
                    new DelayedWorkQueue());
        }
    
        public ScheduledThreadPoolExecutor(int corePoolSize,
                                           ThreadFactory threadFactory) {
            super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
                    new DelayedWorkQueue(), threadFactory);
        }
    
        public ScheduledThreadPoolExecutor(int corePoolSize,
                                           RejectedExecutionHandler handler) {
            super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
                    new DelayedWorkQueue(), handler);
        }
    
    
        public ScheduledThreadPoolExecutor(int corePoolSize,
                                           ThreadFactory threadFactory,
                                           RejectedExecutionHandler handler) {
            super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
                    new DelayedWorkQueue(), threadFactory, handler);
        }

```

一般通过Executors类的newScheduledThreadPool方法构造此对象。DelayedWorkQueue中的任务必然是按照延迟时间从短到长来进行排序的。下面我们再深入分析DelayedWorkQueue，这里留一个引子。 ScheduledThreadPoolExecutor提供了如下四个方法，也就是四个调度器：

1. schedule(Callable callable, long delay, TimeUnit unit) :创建并执行在给定延迟后启用的 ScheduledFuture。
2. schedule(Runnable command, long delay, TimeUnit unit) :创建并执行在给定延迟后启用的一次性操作。
3. scheduleAtFixedRate(Runnable command, long initialDelay, long period, TimeUnit unit) :创建并执行一个在给定初始延迟后首次启用的定期操作，后续操作具有给定的周期；也就是将在 initialDelay 后开始执行，然后在 initialDelay+period 后执行，接着在 initialDelay + 2 * period 后执行，依此类推。
4. scheduleWithFixedDelay(Runnable command, long initialDelay, long delay, TimeUnit unit) :创建并执行一个在给定初始延迟后首次启用的定期操作，随后，在每一次执行终止和下一次执行开始之间都存在给定的延迟。

以scheduleWithFixedDelay方法来分析，如下：

```java
        public ScheduledFuture<?> scheduleWithFixedDelay(Runnable command,
                                                         long initialDelay,
                                                         long delay,
                                                         TimeUnit unit) {
            if (command == null || unit == null)
                throw new NullPointerException();
            if (delay <= 0)
                throw new IllegalArgumentException();
            ScheduledFutureTask<Void> sft =
                new ScheduledFutureTask<Void>(command,
                                              null,
                                              triggerTime(initialDelay, unit),
                                              unit.toNanos(-delay));
            RunnableScheduledFuture<Void> t = decorateTask(command, sft);
            sft.outerTask = t;
            delayedExecute(t);
            return t;
        }
    
```

scheduleWithFixedDelay方法处理的逻辑如下：

1. 校验，如果参数不合法则抛出异常
2. 构造一个task，该task为ScheduledFutureTask
3. 调用delayedExecute()方法做后续相关处理

RunnableScheduledFuture继承RunnableFuture和ScheduledFuture两个接口，除了具备RunnableFuture和ScheduledFuture两类特性外，它还定义了一个方法isPeriodic() ，该方法用于判断执行的任务是否为定期任务，如果是则返回true。

而ScheduledFutureTask作为ScheduledThreadPoolExecutor的内部类，作用是负责ScheduledThreadPoolExecutor中任务的调度。 ScheduledFutureTask内部继承FutureTask，实现RunnableScheduledFuture接口，它内部定义了三个比较重要的变量

```java
            /** 任务被添加到ScheduledThreadPoolExecutor中的序号 */
            private final long sequenceNumber;
    
            /** 任务要执行的具体时间 */
            private long time;
    
            /**  任务的间隔周期 */
            private final long period;
    
```

这三个变量与任务的执行关联，先看ScheduledFutureTask的几个构造函数和核心方法：

```java
           ScheduledFutureTask(Runnable r, V result, long ns) {
                super(r, result);
                this.time = ns;
                this.period = 0;
                this.sequenceNumber = sequencer.getAndIncrement();
            }
    
            ScheduledFutureTask(Runnable r, V result, long ns, long period) {
                super(r, result);
                this.time = ns;
                this.period = period;
                this.sequenceNumber = sequencer.getAndIncrement();
            }
    
            ScheduledFutureTask(Callable<V> callable, long ns) {
                super(callable);
                this.time = ns;
                this.period = 0;
                this.sequenceNumber = sequencer.getAndIncrement();
            }
    
            ScheduledFutureTask(Callable<V> callable, long ns) {
                super(callable);
                this.time = ns;
                this.period = 0;
                this.sequenceNumber = sequencer.getAndIncrement();
            }
    
```

ScheduledFutureTask 在ScheduledFutureTask中有一个compareTo()方法，使用了这些变量

```java
        public int compareTo(Delayed other) {
            if (other == this) // compare zero if same object
                return 0;
            if (other instanceof ScheduledFutureTask) {
                ScheduledFutureTask<?> x = (ScheduledFutureTask<?>)other;
                long diff = time - x.time;
                if (diff < 0)
                    return -1;
                else if (diff > 0)
                    return 1;
                else if (sequenceNumber < x.sequenceNumber)
                    return -1;
                else
                    return 1;
            }
            long diff = getDelay(NANOSECONDS) - other.getDelay(NANOSECONDS);
            return (diff < 0) ? -1 : (diff > 0) ? 1 : 0;
        }
```

任何线程的执行，都是通过run()方法执行，ScheduledThreadPoolExecutor 的run()方法如下：

```java
            public void run() {
                boolean periodic = isPeriodic();
                if (!canRunInCurrentRunState(periodic))
                    cancel(false);
                else if (!periodic)
                    ScheduledFutureTask.super.run();
                else if (ScheduledFutureTask.super.runAndReset()) {
                    setNextRunTime();
                    reExecutePeriodic(outerTask);
                }
            }
```

1. 调用isPeriodic()获取该线程是否为周期性任务标志，然后调用canRunInCurrentRunState()方法判断该线程是否可以执行，如果不可以执行则调用cancel()取消任务。
2. 如果当线程已经到达了执行点，则调用run()方法执行task，该run()方法是在FutureTask中定义的。
3. 否则调用runAndReset()方法运行并充值，调用setNextRunTime()方法计算任务下次的执行时间，重新把任务添加到队列中，让该任务可以重复执行。

**isPeriodic()** 该方法用于判断指定的任务是否为定期任务。

```java
            public boolean isPeriodic() {
                return period != 0;
            }
   
```

canRunInCurrentRunState()判断任务是否可以取消，cancel()取消任务，这两个方法比较简单，而run()执行任务，runAndReset()运行并重置状态，牵涉比较广，我们放在FutureTask后面介绍。所以重点介绍setNextRunTime()和reExecutePeriodic()这两个涉及到延迟的方法。 **setNextRunTime()** setNextRunTime()方法用于重新计算任务的下次执行时间。如下：

```java
            private void setNextRunTime() {
                long p = period;
                if (p > 0)
                    time += p;
                else
                    time = triggerTime(-p);
            }
    
```

该方法定义很简单，p > 0 ,time += p ，否则调用triggerTime()方法重新计算time：

```java
        long triggerTime(long delay) {
            return now() +
                ((delay < (Long.MAX_VALUE >> 1)) ? delay : overflowFree(delay));
        }
```

**reExecutePeriodic**

```java
        void reExecutePeriodic(RunnableScheduledFuture<?> task) {
            if (canRunInCurrentRunState(true)) {
                super.getQueue().add(task);
                if (!canRunInCurrentRunState(true) && remove(task))
                    task.cancel(false);
                else
                    ensurePrestart();
            }
        }
```

reExecutePeriodic重要的是调用super.getQueue().add(task);将任务task加入的队列DelayedWorkQueue中。

# 四、Java特性

## Java 8 特性

### Collectors

Java 8 流的新类 `java.util.stream.Collectors` 实现了 `java.util.stream.Collector` 接口，同时又提供了大量的方法对流 ( stream ) 的元素执行 `map and reduce` 操作，或者统计操作。

**计算平均数**

`Collectors.averagingDouble()` 方法将流中的所有元素视为 `double` 类型并计算他们的平均值。该方法返回的是同一个 `Collectors` 实例，因此可以进行链式操作。

`Collectors.averagingDouble()` 接受一个参数，这个参数是一个 lambda 表达式，用于对所有的元素执行一个 `map` 操作。

Java 所有集合的 `stream().collect()` 可以接受一个收集器实例作为其参数并返回该收集器的计算结果

```java
        package cn.twle.util.stream;
        import java.util.Arrays;
        import java.util.List;
        import java.util.stream.Collectors;
        public class AveragingDoubleExample {
            public static void main(String[] args) {
                List<Integer> list = Arrays.asList(1,2,3,4);
                Double result = list.stream().collect(Collectors.averagingDouble(d->d*2));
                System.out.println(result);
            }
        }
```

**同理averagingInt()，averagingLong()，只是元素要求类型以及返回类型不同。**

**collectingAndThen()**

`Collectors.collectingAndThen()` 函数应该最像 `map and reduce` 了，它可接受两个参数，第一个参数用于 `reduce` 操作，而第二参数用于 `map` 操作。

也就是，先把流中的所有元素传递给第二个参数，然后把生成的集合传递给第一个参数来处理。

```java
        package cn.twle..util.stream;
    
        import java.util.Arrays;
        import java.util.List;
        import java.util.stream.Collectors;
        public class CollectingAndThenExample {
            public static void main(String[] args) {
                List<Integer> list = Arrays.asList(1,2,3,4);
                Double result = list.stream().collect(Collectors.collectingAndThen(Collectors.averagingLong(v->v*2),
                        s-> s*s));
                System.out.println(result);
            }
        } 
//先计算第二个参数，再将结果集合通过第一个参数运算，得到最终的集合，得到平均数，本例结果为25.0
```

`Collectors.counting()` 用于统计流中元素的个数

`Collectors.maxBy()` 和 `Collectors.minBy()` 两个方法分别用于计算流中所有元素的最大值和最小值

`Collectors.summingInt()` 方法将流中的所有元素视为 `int` 类型，并计算所有元素的总和 ( sum )

`Collectors.summingLong()` 将流中的所有元素视为 `long` 类型，并计算所有元素的总和

`Collectors.summingDouble()` 将流中的所有元素视为 `double` 类型，并计算所有元素的总和

`Collectors.toList()` 将流中的所有元素导出到一个列表 ( List ) 中

`Collectors.toSet()` 把流中的所有元素导出到一个集合 ( Set ) 中，并排除重复的元素 ( Set 的特性 )

`Collectors.toMap()` 将流中的所有元素导出到一个哈希表 ( Map ) 中。该方法接受两个参数，第一个参数用于生成键 ( key ) ，第二个参数用于生成值 ( value )。两个参数都是 Lambda 表达式。

`Collectors.mapping()` 一般用于多重 `map and reduce` 中。

### 原始流类

`IntStream`、`LongStream` 和 `DoubleStream` 分别表示原始 `int` 流、 原始 `long` 流 和 原始 `double` 流。提供了大量的方法用于操作流中的数据，同时提供了相应的静态方法来初始化它们自己。这三个原始流类都在 `java.util.stream` 命名空间下。

| 方法             | 说明                                                    |
| ---------------- | ------------------------------------------------------- |
| rangeClosed(a,b) | 返回子序列[a,b)，左闭又开。意味着包括b元素，增长步值为1 |
| range(a,b)       | 返回子序列[a,b)，左闭右开，意味着不包括b                |
| sum              | 计算所有元素的总和                                      |
| sorted           | 排序元素                                                |
| max()            | 返回最大值                                              |
| average()        | 返回平均值                                              |

### Lambda表达式

实现了`@FunctionalInterface` 注解均为函数式接口，均可以采用Lambda表达式来简化代码。

Java Lambda 表达式的语法结构如下

```java
        parameter -> expression body
```

针对这个 Java Lambda 表达式语法，有几个重要的特征需要说明

- **可选的参数类型声明** ： 无需声明参数的类型。编译器可以从参数的值推断出相同的值。
- **可选的参数周围的小括号 `()`** ： 如果只有一个参数，可以忽略参数周围的小括号。但如果有多个参数，则必须添加小括号。
- **可选的大括号 `{}`** : 如果 Lambda 表达式只包含一条语句，那么可以省略大括号。但如果有多条语句，则必须添加大括号。
- **可选的 `return` 关键字** ： 如果 Lambda 表达式只有一条语句，那么编译器会自动 `return` 该语句最后的结果。但如果显式使用了 `return` 语句，则必须添加大括号 `{}` ，哪怕只有一条语句。

**作用域**

因为 Java 8 的 lambda 表达式其实是函数接口的内联实现，也就是匿名内部类，因此，可以引用任何外部的变量或者常量。

但是，lambda 对这些外部的变量是有要求的： 它们必须使用 `final` 修饰符修饰。

### 函数式接口

函数接口一般用于 Java 8 中的 Lambda 表达式 。 而且 Java 8 为了支持 Lambda 表达式，更是定义了许多函数接口。这些接口基本都在 `java.util.function` 包中。

| 常见函数式接口 | 说明                                                    |
| -------------- | ------------------------------------------------------- |
| Function<T,R>  | 表示一个接受T类型的参数，且返回一个R类型结果的函数      |
| Consumer<T>    | 表示接受一个参数，但不返回任何结果的操作                |
| Predicate<T>   | 表示接受一个指定类型T的参数，但返回布尔类型的结果的操作 |
| Supplier<T>    | 表示不接受任何参数，但返回一个T类型的结果的操作         |
|                |                                                         |

### Base64编码解码

Base64 是一种常见的字符编码解码方式，一般用于将二进制数据编码为更具可读性的 Base64 进制格式。在 `java.util` 包下提供了 `Base64` 类用于编码和解码 Base64 数据。

#### Decoder

** java.util.Base64.Decoder 类的方法**

| 方法                               | 说明                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| byte[]decode(byte[]src)            | 使用Base64编码方案解码输入字节数组中的所有字节，将结果写入新分配的输出字节数组。 |
| intdecode(byte[]src,byte[]dst)     | 使用Base64编码方案解码输入字节数组中的所有字节，将结果写入给定的输出字节数组，从偏移量0开始 |
| ByteBufferdecode(ByteBufferbuffer) | 使用Base64编码方案解码输入字节缓冲区中的所有字节，将结果写入新分配的ByteBuffer |
| byte[]decode(Stringsrc)            | 使用Base64编码方案将Base64编码的String解码到新分配的字节数组中 |
| InputStreamwrap(InputStreamis)     | 返回用于解码Base64编码字节流的输入流                         |

上面这 5 个方法，可以说都是 `decode` 方法各种重载，接收各种类型的数据，也支持将结果输出为相应类型。

**获取 java.util.Base64.Decoder 类的实例**

`java.util.Base64` 类提供了三个静态方法可以返回 java.util.Base64.Decoder 类的实例

| 方法                           | 说明                                                |
| ------------------------------ | --------------------------------------------------- |
| Base64.DecodergetDecoder()     | 返回一个Base64.Decoder类型的简单解码器              |
| Base64.DecodergetMimeDecoder() | 返回一个Base64.Decoder类型的MIME解码器              |
| Base64.DecodergetUrlDecoder()  | 返回一个Base64.Decoder类型的URL和文件名安全的解码器 |

##### Encoder



**java.util.Base64.Encoder 类的方法**

| 方法                               | 说明                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| []byteencode(byte[]src)            | 使用Base64编码方案将指定字节数组中的所有字节编码并输出到一个新的字节数组中 |
| intencode(byte[]src,byte[]dst)     | 使用Base64编码方案对指定字节数组中的所有字节进行编码，将结果字节写入给定的输出字节数组，从偏移量0开始 |
| ByteBufferencode(ByteBufferbuffer) | 使用Base64编码方案将指定字节缓冲区中的所有剩余字节编码并输出到新的ByteBuffer中 |
| StringencodeToString(byte[]src)    | 使用Base64编码方案将指定的字节数组编码，并转换为字符串       |
| Base64.EncoderwithoutPadding()     | 返回一个Base64.Encoder实例，该实例与当前实例等效编码，不同的是前者不会在编码字节数据的末尾添加任何填充字符 |
| OutputStreamwrap(OutputStreamos)   | 使用Base64编码方案包装输出流以编码字节数据                   |

上面这 6 个方法，除了 `withoutPadding()` 外，可以说都是 `encode` 方法各种重载，接收各种类型的数据，也支持将结果输出为相应类型。

**获取 java.util.Base64.Encoder 类的实例**

`java.util.Base64` 类提供了四个静态方法可以返回 java.util.Base64.Encoder 类的实例

| 方法                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Base64.EncodergetEncoder()                                   | 返回一个Base64.Encoder类型的简单编码器                       |
| Base64.EncodergetMimeEncoder()                               | 返回一个Base64.Encoder类型的MINE编码器                       |
| Base64.EncodergetMimeEncoder(intlineLength,byte[]lineSeparator) | 返回一个Base64.Encoder类型的使用特定长度和行分隔符的MINE编码器 |
| Base64.EncodergetUrlEncoder()                                | 返回一个Base64.Encoder类型的URL和文件名安全的编码器          |

### 方法引用

**方法引用** 是 Java 8 新增加的功能。方法引用有点类似于 C / C++ 中的 **函数指针** ，通过方法名称指向方法。

Java 8 中的方法引用通过 `::` 符号引用方法，而且支持一下类型的方法引用

1. 静态方法
2. 实例方法
3. 使用 `new` 运算符的构造函数。例如 `TreeSet::new`

### 接口特性

在 Java 7 和之前的版本中，接口 `interface` 是不能包含具体的方法实现的。Java 8 为 **接口** ( interface ) 中引入了 「 默认方法 」( default method ) 实现这个新的概念。

Java 8 除了给接口带来了 默认方法之外，还给接口带来了 **静态方法**。而且，Java 8 中的静态方法还可以有具体的实现。

### forEach()

`forEach()` 方法是 Java 8 为所有集合新增的方法。该方法定义在 `java.lang.Iterable` 接口中。

```java
        default void forEach(Consumer<? super T> action)
```

从函数原型中可以看出，该方法是 `java.lang.Iterable` 接口的默认方法，所有子类可以不用实现，也没必要实现。

该方法对 `Iterable` 中的的每个元素执行给定的操作 ( `action` )，直到处理完所有元素或操作抛出异常为止。

### Optional

Java 8 在 `java.util` 包中添加了一个新的类 `Optional` 。

`Optional` 类是一个容器，用于表示可能包含也可能不包含非 null 值。如果存在值，`isPresent()` 方法将返回 `true`，`get()` 将返回该值。

`Optional` 类提供了许多方法用于处理 「 可用 」 或 「 不可用 」 ，而不是简单的检查空值情况。

`java.util.Optional` 类的声明如下

```java
        public final class Optional<T> extends Object
```

```
Optional` 类提供了三个静态方法用于创建 `Optional` 类的实例，这三个方法的返回值都是 `Optional<T>
```

| 方法               | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| empty()            | 创建一个空(empty)的Optional类的实例                          |
| of(Tvalue)         | 创建一个包含了指定T类型的value值的Optional实例               |
| ofNullable(Tvalue) | 如果value非null，则创建一个包含了指定T类型的value值的Optional实例，否则创建一个空的Optional实例 |

### `Optional` 类提供的方法

| 方法/说明                                                    |
| ------------------------------------------------------------ |
| booleanequals(Objectobj)判断某个其它的对象是否「等于」此Optional |
| Optional<T&gts;filter(Predicate<?superT>predicate)如果存在值，并且值与给定谓词匹配，则返回描述值的Optional，否则返回空Optional |
| <U>Optional<U&gst;flatMap(Function<?superT,Optional<U>>mapper)如果值存在，则将map应用到该值上并返回应用后的结果，如果值不存在，则返回一个空的Optional |
| Tget()如果此Optional中存在值，则返回该值，否则抛出NoSuchElementException异常 |
| inthashCode()如果值存在，则返回当前值的哈希值，如果不存在值，则返回0 |
| voidifPresent(Consumer<?superT>consumer)如果值存在，则使用该值作为参数调用方法consumer。如果值不存在，则什么事情都不做 |
| booleanisPresent()如果值存在则返回true，否则返回false        |
| <U>Optional<U>map(Function<?superT,?extendsU>mapper)如果存在值，则将传递的map函数应用于该值，如果结果为非null，则返回描述结果的Optionals |
| TorElse(Tother)如果值存在则返回值，否则返回other             |
| TorElseGet(Supplier<?extendsT>other)如果值存在则返回值，否则调用other并返回该调用的结果 |
| <XextendsThrowable>TorElseThrow(Supplier<?extendsX>>exceptionSupplier)如果值存在，则返回包含的值，否则抛出由开发者提供的异常 |
| StringtoString()返回此Optional的非空字符串表示形式，一般用于调试 |

### 新日期API

常见的日期时间API有`java.util.Date` 、`java.util.Calendar` 、`java.util.GregoiranCalendar` 和 `java.text.SimpleDateFormat` 四大类，分别用于处理日期、日历、日历表示、日期时间格式化。

这四个类，对于编程老人来讲，应该是习惯了，但对于编程新人来讲，就有好多疑问，有好多陷阱和坑等着它们跳，比如

1. **非线程安全**：`java.util.Date` 并不是线程安全的。开发者在使用这个类时必须自己处理多线程并发问题。
2. **设计不佳** ：一方面日期和日期格式化分布在多个包中。另一方面，`java.util.Date` 的默认日期，年竟然是从 `1900` 开始，月从 `1` 开始，日从 `0` 开始，没有统一性。而且 `Date` 类也缺少直接操作日期的相关方法。
3. **时区处理困难**：因为设计不佳，开发人员不得不编写大量代码来处理时区问题。
4. 还有其它一些问题

面对种种问题，Java 8 终于重新设计了所有日期时间、日历及时区相关的 API。并把它们都统一放置在 `java.time` 包和子包下。并作出了以下改进

1. 新的日期时间 API 是线程安全的。不仅没有 `setter` 方法，而且任何对实例的变更都会返回一个新的实例而保证原来的实例不变。
2. 新的日期时间 API 提供了大量的方法，用于修改日期时间的各个部分，并返回一个新的实例。
3. 在时区方面，新的日期时间 API 引入了 **域** ( domain ) 这个概念。

**本地日期时间**

Java 8 为处理本地的日期时间提供了三个类 `LocalDate` 、`LocalTime` 和 `LocalDateTime`。分别用于处理 **本地日期**、**本地时间** 和 **本地日期时间**。

当使用这三个类时，开发者并不需要关心时区是什么。因为它默认使用的是操作系统的时区。

可以调用 `LocalDateTime.parse()` 、`LocalDate.parse()` 和 `LocalTime.parse()` 方法解析字符串格式的日期时间、日期和时间。

新的日期时间 API 还大量引入了 `of()` 方法，比如我们可以调用 `LocalDate.of()` 方法创建一个日期实例，调用 `LocalTime.of()` 方法创建一个时间实例。

可以调用 `LocalDateTime` 对象的 `withDayOfMonth()` 修改日并返回一个新的实例，调用 `withYear()` 修改年，调用其它 `with*` 方法修改其它属性。

这些 `with` 方法都是返回新的实例，而原来的实例并不会改变。

**时区日期时间**

Java 在 `java.time` 包中也提供了几个类用于处理需要关注时区的日期时间 API。它们是 `java.time.ZonedDateTime` 和 `java.time.ZoneId`。前者用于处理需要时区的日期时间，后者用于处理时区。

`ZonedDateTime` 和 `LocalDateTime` 类似，几乎有着相同的 API。从某些方面说，`ZonedLocalTime` 如果不传递时区信息，那么它会默认使用操作系统的时区，这样，结果其实和 `LocalDateTime` 是类似的。

**时间格式化**

`java.time.format` 包，该包下包含了几个类和枚举用于格式化日期时间。

`java.time.format` 包提供了以下几个类用于格式化日期时间

| 类                       | 说明                                   |
| ------------------------ | -------------------------------------- |
| DateTimeFormatter        | 用于打印和解析日期时间对象的格式化程序 |
| DateTimeFormatterBuilder | 创建日期时间格式化样式的构建器         |
| DecimalStyle             | 日期和时间格式中使用的本地化十进制样式 |

`java.time.format` 包还提供了以下几个枚举，包含了常见的几种日期时间格式。

| 枚举          | 说明                                               |
| ------------- | -------------------------------------------------- |
| FormatStyle   | 包含了本地化日期，时间或日期时间格式器的样式的枚举 |
| ResolverStyle | 包含了解决日期和时间的不同方法的枚举               |
| SignStyle     | 包含了如何处理正/负号的方法的枚举                  |
| TextStyle     | 包含了文本格式和解析的样式的枚举                   |



`DateTimeFormatter` 类格式化日期时间的最重要的类，该类是一个最终类，只能实例化，不能被扩展和继承。

`DateTimeFormatter` 类的定义如下

```java
        public final class DateTimeFormatter extends Object    
```

`DateTimeFormatter` 类用于打印和解析日期时间对象的格式化器。此类提供打印和解析的主要应用程序入口点，并提供 DateTimeFormatter 的常见模式

- 使用预定义的常量，比如 `ISO_LOCAL_DATE`
- 使用模式字母，例如 `uuuu-MMM-dd`
- 使用本地化样式，例如 `long` 或 `medium`

所有的日期时间类，包括本地日期时间和包含时区的日期时间类，都提供了两个重要的方法

1. 一个用于格式化，`format(DateTimeFormatter formatter)`
2. 另一个用于解析，`parse(CharSequence text, DateTimeFormatter formatter)`

### Stream

Java 中的 **流** ( Stream ) 表示来自 **源** ( source ) 的一系列对象，它支持统计、求和、求平均值等聚合操作。

流具有以下特征：

- **元素序列** : 流以顺序方式提供特定类型的一组元素。流只会按需获取/计算元素。但它从不存储元素。
- **源 ( Source )**：流可以将集合，数组或 I/O 资源作为输入源。
- **聚合操作**： 流支持聚合操作，如 `filter`、`map`、`limit`、`reduce`、`find`、`match` 等
- **管道 ( pipelining )**：大多数流操作都返回流本身，以便可以对其结果进行流水线操作。这些操作称为 **中间** 操作，它们的功能是获取输入，处理它们并将输出返回到目标。`collect()` 方法是一个终端操作，通常在流水线操作结束时出现，以标记流的结尾。
- **原子性迭代 ( Automatic iterations )** ： 与需要显式迭代的集合相比，流操作在内部对所提供的源元素进行迭代。

#### 创建

Java 8 在推出流的同时，对集合框架也进行了一些比较大变更。主要是在 `Collection` 接口上提供了两种生成 Stream 的方法:

- `stream()` 方法，该方法以集合作为源，返回集合中的所有元素以在集合中出现的顺序组成的流。
- `parallelStream()` 方法，该方法以集合作为源，返回一个支持并发操作的流。

#### 聚合操作

##### forEach() 方法

Java 8 为 Stream 提供了一种新方法 `forEach()`，用于迭代流的每个元素。

下面的代码片段演示了如何使用 `forEach` 打印 `10` 个随机数。

```
        Random random = new Random();        random.ints().limit(10).forEach(System.out::println);    
```

上面这个代码片段中，`Random` 对象的 `ints()` 方法会返回一个整数流。而 `limit()` 方法则限制了流中的元素个数。从某些方面说，可以理解为当源产生了 10 个随机数之后就关闭源。

##### map() 方法

`map()` 方法会迭代流中的元素，并为每个元素应用一个方法，然后返回应用后的流。

例如下面的代码，使用 `map()` 方法把求出每个元素的平方，然后过滤掉重复的元素，最后在转换为列表集合

```
        List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);            //获取每个元素的平方        List<Integer> squaresList = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());    
```

`map( i -> i*i)` 操作求取流中每个元素的平方，并返回一个新的流。`distinct()` 方法则用于过滤流中的重复元素。

##### filter() 方法

`filter()` 方法根据一个谓词来过滤元素。这个谓词是一个方法，以流中的每一个元素作为参数，如果返回 `true` 则会被过滤掉。

例如下面的代码，使用 `filter()` 方法过滤那些空字符串。

```java
        List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");            int count = strings.stream().filter(string -> string.isEmpty()).count();    
```

##### limit() 方法

`limit()` 方法用于减少( 限制 ) 流中的元素数量。

例如下面的代码段演示了如何使用 `limit()` 方法只输出 10 个随机数

```
        Random random = new Random();        random.ints().limit(10).forEach(System.out::println);    
```

##### sorted() 方法

`sorted()` 方法用于给流中的元素进行排序。

下面的范例演示了如何按照排序顺序打印 10 个随机数

```
        Random random = new Random();        random.ints().limit(10).sorted().forEach(System.out::println);    
```

#### 并发处理

`parallelStream()` 是需要并发处理的流的替代方案。`stream()` 方法产生的流只能是串行处理，可以理解为只在一个线程中，按照流中元素的顺序一个接一个的处理。

而并发处理，就是传说中的 `map-reduce` 方法，可以充分利用多核优势。

需要注意的是，`parallelStream()` 会打乱流的顺序，也就是返回的序列顺序不一定是输入的序列顺序。

例如下面的代码用于打印序列中的空字符串的数量

```
        List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");            //get count of empty string        int count = strings.parallelStream().filter(string -> string.isEmpty()).count();    
```

因为 `stream()` 返回是串行流，而 `parallelStream()` 返回的是并行流。因此在串行和并行之间切换是非常简单的。

#### 收集器 （ Collectors ）

收集器 （ Collectors ）用于将已经处理的流中的元素组合到一起。

`Collectors` 类提供了大量方法用于指示如何收集元素。

比如 `Collectors.toList()` 方法可以将流中的元素收集起来，并转换为列表

```
        List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");        List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());            System.out.println("Filtered List: " + filtered);    
```

比如 `Collectors.joining()` 方法可以将流中的元素收集起来，并使用指定的字符串拼接符拼接成一个字符串。

```
        List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");        String mergedString = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.joining(", "));            System.out.println("Merged String: " + mergedString);    
```

#### 统计 ( Statistics )

Java 8 同时新增了大量的统计收集器来来获取流中的元素的一些统计信息。

前提是我们先要在流上调用 `summaryStatistics()` 方法返回统计信息概要，然后在调用相应的方法来获取具体的统计信息。

例如下面的代码，先调用 `summaryStatistics()` 方法返回统计概要，然后调用 `getMax()` 方法获取最大值

```
        List numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);            IntSummaryStatistics stats = integers.stream().mapToInt((x) -> x).summaryStatistics();            System.out.println("Highest number in List : " + stats.getMax());    
```

例如下面的代码，先调用 `summaryStatistics()` 方法返回统计概要，然后调用 `getMin()` 和 `getSum()` 方法获取最小值和所有数字之和

```
        List numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);            IntSummaryStatistics stats = integers.stream().mapToInt((x) -> x).summaryStatistics();            System.out.println("Lowest number in List : " + stats.getMin());        System.out.println("Sum of all numbers : " + stats.getSum());    
```

例如下面的代码，先调用 `summaryStatistics()` 方法返回统计概要，然后调用 `getAverage()` 方法获取平均值

```
        List numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);            IntSummaryStatistics stats = integers.stream().mapToInt((x) -> x).summaryStatistics();            System.out.println("Average of all numbers : " + stats.getAverage());    
```



# 五、JVM

- JVM图解

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/5401760-cde4aefdad5438ca.png)

## 一、方法区

- **方法区为JVM中的线程所共有的**
- 在Hotspot种方法区称为永久代，划入了GC范围，但在Java 8后取消了永久代的概念，从而转为元空间，共享本地内存。

主要存放已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据（比如spring 使用IOC或者AOP创建bean时，或者使用cglib，反射的形式动态生成class信息等）。

>注意：JDK 6 时，String等字符串常量的信息是置于方法区中的，但是到了JDK 7 时，已经移动到了Java堆。所以，方法区也好，Java堆也罢，到底详细的保存了什么，其实没有具体定论，要结合不同的JVM版本来分析。

>当方法区无法满足内存分配需求时，将抛出OutOfMemoryError。
>运行时常量池溢出：比如一直往常量池加入数据，就会引起OutOfMemoryError异常。

### 类信息

- 类型全限定名。
- 类型的直接超类的全限定名（除非这个类型是java.lang.Object，它没有超类）。
- 类型是类类型还是接口类型。
- 类型的访问修饰符（public、abstract或final的某个子集）。
- 任何直接超接口的全限定名的有序列表。
- 类型的常量池。
- 字段信息。
- 方法信息。
- 除了常量意外的所有类（静态）变量。
- 一个到类ClassLoader的引用。
- 一个到Class类的引用。

### 常量池

#### Class文件中的常量池

在Class文件结构中，最头的4个字节用于存储Megic Number，用于确定一个文件是否能被JVM接受，再接着4个字节用于存储版本号，前2个字节存储次版本号，后2个存储主版本号，再接着是用于存放常量的常量池，由于常量的数量是不固定的，所以常量池的入口放置一个U2类型的数据(constant_pool_count)存储常量池容量计数值。

常量池主要用于存放两大类常量：字面量(Literal)和符号引用量(Symbolic References)，字面量相当于Java语言层面常量的概念，如文本字符串，声明为final的常量值等，符号引用则属于编译原理方面的概念，包括了如下三种类型的常量：

- 类和接口的全限定名
- 字段名称和描述符
- 方法名称和描述符

#### 运行时常量池

Class文件中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息是常量池，用于存放编译期生成的各种字面量和符号引用，这部分内容将在类加载后进入方法区的运行时常量池中存放。

运行时常量池相对于Class文件常量池的另外一个重要特征是具备动态性，Java语言并不要求常量一定只有编译期才能产生，也就是并非预置入Class文件中常量池的内容才能进入方法区运行时常量池，运行期间也可能将新的常量放入池中，这种特性被开发人员利用比较多的就是String类的intern()方法。

#### 常量池好处

常量池是为了避免频繁的创建和销毁对象而影响系统性能，其实现了对象的共享。

例如字符串常量池，在编译阶段就把所有的字符串文字放到一个常量池中。

- （1）节省内存空间：常量池中所有相同的字符串常量被合并，只占用一个空间。
- （2）节省运行时间：比较字符串时，= =比equals()快。对于两个引用变量，只用= =判断引用是否相等，也就可以判断实际值是否相等。

#### 基本类型的包装类和常量池

java中基本类型的包装类的大部分都实现了常量池技术，即Byte,Short,Integer,Long,Character,Boolean。

这5种包装类默认创建了数值[-128，127]的相应类型的缓存数据，但是超出此范围仍然会去创建新的对象。 两种浮点数类型的包装类Float,Double并没有实现常量池技术。

**原理：在相关的类中，有个内部相关的cache类，并且有相关的静态代码块，在类被加载的时候，便实现了常量池技术**。

#### String与常量池

String的intern()方法会查找在常量池中是否存在一份equal相等的字符串，如果有则返回该字符串的引用，如果没有则添加自己的字符串进入常量池。

```java
String str1 = "abcd"; //在常量池里生成，并且str1是此常量池字符串的引用
String str2 = new String("abcd"); //在常量池拿对象到堆内存中，并且str2是堆对象的引用
String str3 = "str" + "ing";  //会由编译期自动优化为一个字符串，即编译期确定的值可以自动优化，而必须由运行期推断的则不可以
String str4 = str1 + str2; //本质上会优化为StringBuilder的append
String s1 = new String("xyz"); //创建了2个对象和1个引用
```

## 二、堆

- **堆为JVM中的线程所共有的**

- 堆中存储对象、数组，不存放基本类型和对象引用。

## 三、栈

- **栈为JVM中的线程所私有的**
- 每个线程包含一个栈区，栈中保存基本数据类型和对象的引用，其他线程访问不了，栈分为：基本类型变量区、执行环境上下文、操作指令区

## 四、JVM类加载

- JVM采用父类委托机制，包括了四种类加载器 

  - BootStrapClassLoader 引导类加载器

  - ExtClassLoader 扩展类加载器

  - AppClassLoader 应用类加载器

  - CustomClassLoader 用户自定义类加载器

![20190410111212200](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/20190410111212200.jpg)



## 五、CMS垃圾收集器

- CMS是老年代垃圾收集器，手机过程中可以与用户线程并发操作，可与其他新生代收集器配合使用，**CMS牺牲了系统的吞吐量追求垃圾速度。**

### 1、CMS收集过程

CMS 处理过程有七个步骤：

1. 初始标记(CMS-initial-mark) ,会导致stw;
2. 并发标记(CMS-concurrent-mark)，与用户线程同时运行；
3. 预清理（CMS-concurrent-preclean），与用户线程同时运行；
4. 可被终止的预清理（CMS-concurrent-abortable-preclean） 与用户线程同时运行；
5. 重新标记(CMS-remark) ，会导致swt；
6. 并发清除(CMS-concurrent-sweep)，与用户线程同时运行；
7. 并发重置状态等待下次CMS的触发(CMS-concurrent-reset)，与用户线程同时运行； 其运行流程图如下所示：

![image-20211221123019936](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20211221123019936.png)

#### 初始标记

标记存活的对象，主要分为：

1. 标记老年代中所有的GC Roots对象
2. 标记年轻代中或者的对象引用到的老年代的对象

可作为GC Roots对象：

1. 虚拟机栈(栈桢中的本地变量表)中的引用的对象
2. 方法区中的类静态属性引用、常量引用的对象
3. 本地方法栈中JNI的引用的对象

#### 并发标记

  从“初始标记”阶段标记的对象开始找出所有存活的对象，修正程序运行期间对象行为导致的情形，减小重新标记的时间代价。并把需要回收的对象标记为Dirty。

#### 预清理

将标记为Dirty的对象进行回收，减小回收对象的时间代价。

#### 重新标记

这个阶段会发现第二次STW，完成标记整个老年代中的所有存活对象，需要扫描新生代（可能引用老年代对象）和老年代。如果此阶段耗时较长，可先执行一次young gc。

#### 并发清理

清楚没有标记的对象并且回回收空间，由于CMS并发清理阶段的时候用户线程还在运行着，这一部分新产生的GC垃圾由于在标记阶段之后，只能留到下一部分清理，称为“浮动垃圾”。

### 2、CMS注意点

**减少remark阶段停顿**：remark之前执行一次young gc，减小扫描代价，参数：**-XX:+CMSScavengeBeforeRemark**

**内存碎片问题**：CMS是基于标记-清除算法的，可以设置多少次full gc后做一次压缩，参数：-**XX:CMSFullGCsBeforeCompaction=n**

**concurrent mode failure**：发生在CMS GC的时候，由于业务线程同时运行，年轻代空间满了需要执行young gc，升入老年代，而老年代空间不足产生。可以设置提前GC，参数：**-XX:+UseCMSInitiatingOccupancyOnly**、**-XX:CMSInitiatingOccupancyFraction=70**

**promotion failed**：对象放入老年代的时候，老年代的碎片过多，无法存放年轻代升入的对象，解决：cms进行空间整理压缩、调大年轻代空间、调低CMS触发的阈值











# 六、其他

## 1、一致性Hash算法

一致性hash算法是分布式中一个常用且好用的分片算法、或者数据库分库分表算法。常见的数据分片有：取模、划段、一致性Hash，前两者存在节点变更从而数据需要迁移等问题，故常采用一致性Hash算法。

**一致性Hash**：采用大小为2^32的哈希环，对节点和数据使用Hash算法即可确定其在哈希环上的位置。此时对于某个节点来说，如果该节点不可用或者新增，只会影响其前一个节点的数据，而不会影响其他数据。同时，为了避免数据倾斜问题，我们可以引入虚拟节点的概念，将真实的节点虚拟为多个节点，在环上进行均匀分布，从而解决数据倾斜的问题。

<img src="https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/67da77e2f7334aef0f26b5a8d941cbb3.png" style="zoom:50%;" />

