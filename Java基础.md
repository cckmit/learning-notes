

# instanceof关键字

- - **用于判断该运算符前面引用类型变量指向的对象是否是后面类、其子类、接口实现类创建的对象。如果是则返回true，否则返回false**
  - **其使用格式如下：引用类型变量 instanceof （类、抽象类或接口）**

- - 向下转型的之前判断一下当前的实例是否为某个类的对象，避免出现ClassCastException异常
  - **对象 instanceof 类→返回 boolean类型**

# Object

- - 无任何继承结构，一切的类都是Object子类
  - class Ball{} 与class Ball extends Object{}效果相同，object可以接收所有的子类实例

- - object可以接收所有引用数据类型（数组、类、接口）
  - 获取对象信息:**public String toString()**，object默认的toString()方法返回的只是实例化对象的地址，有需要可以自己覆写

- - **对象打印可以调用toString()**，对象是引用类型，而toString()方法是将对象转为字符串，返回其地址，即printfln(对象)，返回的是对象的地址，而重写后便可返回自己想要的内容
  - object类对象的比较方法：**public boolean equals(Object obj)，比较hashcode，相同返回true，可以根据需求重写，不同返回false，传统的"=="用于对象实现的是两个堆内存地址的比较，用于基本数据类型则是值的比较**

- - 当**toString**拼写错误成为tostring的时候，程序仍可以运行，其认为tostring是子类扩充的新方法，此时可以用''@Override''明确定义在子类覆写的方法中，表明其不为子类独有，而是通过覆写得到
  - ''@Override''利用此注解有两个好处：

- - - 避免错误的方法覆写定义；
    - 是提示用户那些方法是覆写定义的，那些方法是子类新定义的。

- - ''@Deprecated''实现过期的定义操作，程序执行不出现任何问题，只有过期的提示信息
  - ''@SuppressWarnings('''')''压制警告信息，使得警告信息不重复提示

- - - 

# Runtime类

- - Runtime描述的是运行时的概念，每一个JVM进程里面都会包含有一个运行时的实例化对象，该对象运行类型就是Runtime类
  - 唯一一个可以描述JVM进程信息的程序类，Runtime run=Runtime.getRuntime()；

- - java中提供有垃圾收集机制，两种形式：自动垃圾回收、手工回收，Runtime.gc()表示手工回收垃圾的处理



| No   | 方法名称                  | 类型 | 描述                                                         |
| ---- | ------------------------- | ---- | ------------------------------------------------------------ |
| 01   | public long maxMemory()   | 方法 | 获取该JVM进程可以操作的最大内存数量【整体内存的1/4】         |
| 02   | public long totalMemory() | 方法 | 获取该JVM进程中可以使用到的常规最大内存数量【整体内存的1/64】 |
| 03   | public long freeMemory()  | 方法 | 获取空余的内存数量                                           |
| 04   | public void gc()          | 方法 | 调用垃圾收集机制                                             |

# System类

- - public static long currentTimeMillis()：获取当前的日期时间数，实现最简单的耗时统计处理
  - System类中有一个gc方法：public static void gc()，本质上调用了Runtime类中的gc()，手工的gc()方法只有一个，就是在Runtime类中定义的

#  Cleaner类

- - 传统的对象回收是使用Object类中的finallize()方法，但这种方法会由于对对象本身的回收问题或者产生死锁问题，后来被废除了

```java
import java.lang.ref.Cleaner;
class Member implements Runnable{      //线程类
  public Member(){
     System.out.println("对象实例化调用");
  }
@Override
public void run(){
   System.out.println("对象回收");
  }
}
class MemberCleaner implements AutoCloseable{
 private static final Cleaner cleaner=Cleaner.create();  //创建一个回收对象
 private Cleaner.Cleanable cleanable;   //可以被回收的对象
 public MemberCleaner(Member member){   //处理要回收的对象
     this.cleanable=cleaner.register(this,member);   //注册一个可回收对象
}
 @Override
 public void close() throws Exception{   //释放资源
    this.cleanable.clean(); //回收对象 
 }
}
public class TestDemo{
  public static void main(String[] args) throws Exception{
  Member men=new Member();
  System.gc();  //强制性的进行回收 
  try(MemberCleaner mc=new MemberCleaner(mem)){  //进行对象回收的处理
//如果有需求则可以进行其他的处理操作
 }catch(Exception e){}
}
}
```

