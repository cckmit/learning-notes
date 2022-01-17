# 基础



## python语法特征

空格和冒号是python的重要表达符号，使用空格形成缩进来表示代码块的，示例如下:

```python
if True:
	print("真")
else:
	print("假")
```

## 多变量赋值 ##

```python
x, y, z = 10, 15, 20
print(y)  # output:15

x = y = z = 20
print(y)  # output:20
```

## 变量交换 ##

假设x=2,y=4，现在需要交换x和y的值，其他的编程语言的做法是使用一个中间变量y来辅助，x、y的值交换，但python不需要，交换代码如下：

```python
x, y = 2, 4
x, y = y, x
print("x:%d y:%d" % (x, y))
# 输出：x: 4 y: 2
```

## 使用input等待用户输入 ##

```python
msg = input("请输入内容")
print(msg)
```

## 查看变量类型 ##

使用内置方法type()查看变量类型，示例如下：

```python
print(type("老王是个好人"))
# 输出：<class 'str'>

print(type(11))
# 输出：<class 'int'>

print(type(11.0))
# 输出：<class 'float'>
```

## 关于++i和i++ ##

python是不支持++i和i++等操作的，可以用+=代替，示例如下：

```python
age = 18
age += 1
print(age)
# 输出：19
```

字符串首字母大写
使用python中的title()内置方法，可以自动把首字母大写，示例如下：

```python
name = "jack"
print(name.title())
# 输出：Jack
```

## 使用range()生成列表 ##

python可以使用range()生成列表，格式：range([开始数],结束数,[步长])，开始数不必填默认0，生成的元素不包含结束数，可以指定步长非必填参数，普通示例如下：

```python
for item in range(1, 5):
    print(item)
# 输出如下：
# 1
# 2
# 3
# 4
```

指定步长，示例如下：

```python
for item in range(1, 5, 2):
    print(item)
# 输出：
# 1
# 3
```

## 随机数 ##

python使用random模块生成随机数。

random.random() ：生成0-1之内的随机数

random.randint(0, 2)：生成0-2的随机数，包含0和2

示例如下：

```python
import random

# 循坏10次1-10的随机数
for item in range(10):
    print(str(random.randint(1, 10)))
```

## 占位符 ##

python中的占位符可以用：%s（替代字符）、%d（替代数字）、%f（替代浮点）来占位，示例如下：

```python
info = "姓名：%s \n年龄：%d" % ("老王", 18)
print(info)
# 输出如下：
# 姓名：老王
# 年龄：18
```

## format ##

format和占位符很像，只不过不需要指定占位的类型，格式：

>"...{}...{}".format("老王",18)

示例代码如下：

```python
#coding=utf-8

info = "姓名：{} 年龄：{}".format("老王", 18)
print(info)  #输出：姓名：老王 年龄：18
```


## 程序计时器 ##

使用场景：有时候我们需要计算程序的运行时长，可以使用以下代码计算：

```python
import datetime
import time

#开始计时
startTime = datetime.datetime.now()

time.sleep(1)

#结束计时
endTime = datetime.datetime.now()
print(endTime - startTime)
#输出：0:00:01.000791
```

## 打印日志 ##

使用关键字“print”输入日志信息，示例如下：

```python
print("输出日志")
```

## 注释 ##

单行注释：使用“#”作为注释开头

```python
number = 1
# number = 2 (此行没有运行)
print(number)  # output:1
```

多行注释：使用三个单引号(')或三个双引号(")开始，三个单引号(')或三个双引号(")结束

```python
'''
方式一
这里面也是多行注释
更多注释信息...
'''

"""
方式二
这里面也是多行注释
更多注释信息...
"""
```

# 变量

## 使用

变量格式: 名称 = 值

```python
name = "老王"
print(name)  
#  输出：老王
```

## 变量命名规则 ##

变量支持：字母、数字、下划线组合。

注意：变量不能使用数字开头，变量区分大小写，以下划线开头的变量标识私有变量，不能被其他模块使用。

## 变量赋值 ##

变量赋值支持多变量赋值，示例如下：

```python
name = "老王"
print(name)  
# 输出：老王
    
age, sex = "你猜", "男"
print(age)  
#  输出：你猜
    
x = y = 10
print(x)  
#  输出：10
```



# 数据类型


----------

## 所有类型 ##

int(整型)

float(浮点)

complex(复数)

bool(布尔)

str(字符串)

list(列表)

tuple(元祖)

dict(字典)

查看数据类型
----------

在ptyhon中可以使用type()方法返回元素数据类型，示例如下：

```python
print(type(1))
# 输出：<class 'int'>

print(type(1.0))
# 输出：<class 'float'>

print(type("1.0"))
# 输出：<class 'str'>
```


## 数字 ##

```python
x = 3
y = 2
print(x/y)  # output:1.5
```

### 数字的乘方 ###

python中使用2个乘号(*)来计算数字的乘方，比如3**2,表示3的平方。

```python
print(3**2)  # output:9
```

### 取模运算符 ###

取模运算符(%)，返回两个相除数的余数，实例如下：

```python
print(4 % 3)  # output:1
print(5 % 3)  # output:2
```

### int类型转换 ###

python不支持模糊的数据类型，比如数字加字符，比如：

```python
x = 18
y = "2"
print(x+y)
```

程序运行报错：TypeError: unsupported operand type(s) for +: 'int' and 'str'

这时候需要把非数字类型转换为数字类型，使用语法"int(str)",实例如下：

```python
x = 18
y = "2"
print(x+int(y))  # output:20
```

----------

## bool ##

bool的值为：True | False，注意首字母都要大写。

**在python中0,、空字符、空列表[]、空元祖()、空字典都等于False。**

## 字符串 ##

python中可以使用单引号(')、双引号(")、三引号('''或""")表示。

**去除空格：lstrip()、rstrip()、strip()=>去首部、尾部、首尾部**

实例如下:

```python
# lstrip()去字符开始空格
name = ' 老王 '
print("开始"+name.lstrip()+"结束")  # output:开始老王 结束

# rstrip()去字符串结束空格
name = ' 老王 '
print("开始"+name.rstrip()+"结束")  # output:开始 老王结束

# strip() 去看首尾两端空格
name = ' 老王 '
print("开始"+name.strip()+"结束")  # output:开始老王结束
```

**类型转换为字符**
使用str(),把非字符类型转换为字符类型，实例如下：

```python
print(str(1)+str(2))  # output:12
```

全部转换大、小写
使用upper()、lower()把字符全部转换成大、小写，实例如下:

```python
name = "Laowang"
print(name.upper())  # output:LAOWANG
print(name.lower())  # output:laowang
```

----------

## 列表 ##

python中使用中括号([])表示列表,并用逗号分隔其元素，实例如下：

```python
list = [111, 222, 333]
print(list)  # output:[111, 222, 333]
```

**获取列表长度**
使用len()获取列表长度，实例如下：

```python
list = [111, 222, 333]
print(len(list))  # output:3
```

**获取某个元素**
使用下标来获取元素list[0],第一个元素的下标为0，使用-1可以获得最后一个元素，实例如下：

```python
list = [111, 222, 333, 444]
print(list[0])  # output:111
print(list[-1])  # output:444
print(list[1:3])  # output:[222, 333]
```

**新增元素**

方式一：使用append()添加元素到数组最后，实例如下：

```python
list = ["hello", "world"]
list.append("!")
print(len(list))  # output:3
print(list)  # output:['hello', 'world', '!']
```

方式二：insert(n,object)添加元素到指定位置，实例如下：

```python
list = ["hello", "world"]
list.insert(0, "!")
print(list)  # output:['!', 'hello', 'world']
```

**删除元素**

方式一：使用del(index)关键字，实例如下：

```python
list = ["hello", "world", "!"]
del list[0]
print(list)  # output:['world', '!']
```

方式二：使用pop()删除元素最后一项，实例如下：

```python
list = ["hello", "world", "!"]
list.pop()
print(list)  # output:['hello', 'world']
```

方式三：如果你知道要删除的值，可以使用remove删除，实例如下：

```python
list = ["hello", "world", "!"]
list.remove("hello")
print(list)  # output:['world', '!']
```

**in查询**
查询元素是否存在数组使用in查询，实例如下,

```python
list = ["hello", "world", "!"]
dstr = "hello2"
if dstr in list:
    print("存在")
else:
    print("不存在")
# output:不存在
```

**最大值、最小值、求和**
使用min(),max(),sum()，实例如下:

```python
list = [10, 4, 6, 8]
print(min(list))  # output:4
print(max(list))  # output:10
print(sum(list))  # output:28
```

**数组排序**
使用sort()对列表永久性排序，实例如下：

```python
list = ["ahref", "focus", "mouse", "click"]
list.sort()
print(list)  # output:['ahref', 'click', 'focus', 'mouse']
```

如果你需要一个**相反的排序，使用sort(reverse=True)**即可，实例如下：

```python
list = ["ahref", "focus", "mouse", "click"]
list.sort(reverse=True)
print(list)  # output:['mouse', 'focus', 'click', 'ahref']
```

使用sorted(),对列表临时排序，实例如下：

```python
list = ["ahref", "focus", "mouse", "click"]
print(sorted(list))  # output:['ahref', 'click', 'focus', 'mouse']
print(list)  # output:['ahref', 'focus', 'mouse', 'click']
```

颠倒列表使用reverse()，实例如下：

```python
list = ["ahref", "focus", "mouse", "click"]
list.reverse()
print(list)  # output:['click', 'mouse', 'focus', 'ahref']

list = [222, 111, 333]
list.reverse()
print(list)  # output:[333, 111, 222]
```

注意：reverse()不是按序排序之后再倒叙，而是直接颠倒列表，如果要按序排序在颠倒顺序使用，sort(reverse=True).


**切片**
你可以处理列表中的部分元素，python中称之为切片.
例如你需要列表中的前两位元素，可以这样使用：

```python
list = ["ahref", "focus", "mouse", "click"]
print(list[0:2])  # output:['ahref', 'focus']
```

[x:y]其中x表示开始下标，截取包含开始下标，y表示结束下标，截取时不包含结束下标，可以使用负数，代表列表的倒数几位，实例如下：

```python
list = ["ahref", "focus", "mouse", "click"]
print(list[:2])  # output:['ahref', 'focus']
print(list[:])  # output:['ahref', 'focus', 'mouse', 'click']
print(list[:-1])  # output:['ahref', 'focus', 'mouse']
```

----------

## 元祖(tuple) ##

python中有括号()表示元祖，是一种只读的列表类型，元素值不能被修改。
实例如下：

```python
list = ("ahref", "focus", "mouse", "click")
print(list[1])  # output:focus

list[1] = "myfocuse"  # output:报错，元祖的元素不能被修改
```

注意：元祖的元素虽然不能修改，但元祖的接受变量是运行修改的，实例如下：

```python
list = ("ahref", "focus", "mouse", "click")
print(list)  # output:('ahref', 'focus', 'mouse', 'click')

list = ("dbclick", "keyup")
print(list)  # output:('dbclick', 'keyup')
```

上面的代码表示把元素("ahref", "focus", "mouse", "click")赋值给list变量，但是list变量值的修改是合法的。

----------

## 字典 ##

python中用{}表示字典，字典是用键值对表示的，和列表相比，字典是无须的对象集合。

```python
dict = {}
dict["name"] = "laowang"
dict["age"] = 18

print(dict)  # output:{'name': 'laowang', 'age': 18}
print(dict["name"])  # laowang
print(dict.keys())  # output:dict_keys(['name', 'age'])
print(dict.values())  # output:dict_values(['laowang', 18])

for key, value in dict.items():
    print("key:"+key)
# output:key:name
# output:key:age
```

----------

## 类型转换方法集合 ##

```python
chr(i) 把一个ASCII数值,变成字符
ord(i) 把一个字符或者unicode字符,变成ASCII数值
oct(x) 把整数x变成八进制表示的字符串
hex(x) 把整数x变成十六进制表示的字符串
str(obj) 得到obj的字符串描述
list(seq) 转换成列表   
tuple(seq) 转换成元祖  
dict() 转换成字典   
int(x) 转换成一个整数
float(x) 转换成一个浮点数
complex(x) 转换成复数
```

# 条件判断和循环 #

----------

## 条件判断 ##

条件判断的重要值是True和False，注意首字母大写，示例如下：

```python
if True:
    print("真")
else:
    print("假")
# 输出：真
```

**非真判断**

非真判断使用not关键字，示例如下：

```python
if not True:
    print("True")
else:
    print("False")
# 输出：False
```

**多情况判断**

多情况判断使用if/elif/else，示例如下：

```python
age = 18
if age < 16:
    print("青少年")
elif age < 18:
    print("青年")
elif age < 60:
    print("成人")
else:
    print("老年")
# 输出：成人
```

python用空格缩进代表代码块，所以要主要代码缩进.

**满足多条件**

使用and关键字，示例如下：

```python
age = 18
name = "laowang"
if age == 18 and name == "laowang":
    print("良好少年")
else:
    print("不良少年")
# 输出：良好少年
```

**至少满足一种条件**

使用or关键字，示例如下：

```python
age = 18
name = "laowang"
if age == 18 or name == "xiaoli":
    print("良好少年")
else:
    print("不良少年")
# 输出：良好少年
```

**False值**
python中0、空字符串、空列表、空元祖值、空字典都为false.

## 循环 ##

**for循环**
基础示例如下：

```python
list = ["focus", "mouse", "click"]
for item in list:
    print(item)
# 输出：focus
# 输出：mouse
# 输出：click
```

**break跳出循环**，实例如下：

```python
list = ["focus", "mouse", "click"]
for item in list:
    if item == "mouse":
        break
    print(item)
# 输出：focus
```

**continue跳过该次循环**，实例如下：

```python
list = ["focus", "mouse", "click"]
for item in list:
    if item == "mouse":
        continue
    print(item)
# 输出：focus
# 输出：click
```

**使用enumerate获取下标**

```python
list = ["focus", "mouse", "click"]
for index, item in enumerate(list):
    print("index:{} item:{}".format(index, item))
# 输出如下：
# index:0 item:focus
# index:1 item:mouse
# index:2 item:click
```

**while循环**
基础示例如下：

```python
num = 1
while num < 3:
    print(num)
    num = num+1
# 输出：1
# 输出：2
```

在while中break和continue同样有效，和上文for循环作用相同，请参考上文。



# 函数和类

----------

**函数**
函数（有些语言称之为方法）是组织好的，可以复用的功能代码段。
python定义函数的格式：

```python
def 函数名(参数):
	函数体
```

基础示例如下：

```python
# 自定义数值相加函数
def mySum(x, y):
    return x+y


print(mySum(4, 4))  # output:8
print(mySum(3, 1))  # output:4
```

**传递可变对象**
传递可变对象指的是传递了对象，在函数体被重新赋值后原来的值也发生了改变，示例如下：

```python
def updateList(ls):
    ls.append("laowang")
    return ls


list = ["hello"]
updateList(list)
print(list)
# 输出：['hello', 'laowang']
```

原来的列表的值也被修改了，**如果不想改变原来值的情况下，可使用[:]，传递切片副本**，示例如下：

```python
def updateList(ls):
    ls.append("laowang")
    return ls


list = ["hello"]
updateList(list[:])
print(list)
# 输出：['hello']
```

**缺省参数**
缺省参数表示调用函数的时候是可以不传递当前参数值，而使用默认参数值的，示例如下：

```python
def showInfo(name, sex="男", age=18):
    print("姓名：%s\t性别：%s\t年龄：%d" % (name, sex, age))


showInfo(name="老王")  # 输出：姓名：老王      性别：男        年龄：18
showInfo(name="老王", age=19)  # 输出：姓名：老王      性别：男        年龄：19
```

> 注意：有默认值的参数一定要放在无默认值参数的后面。

**指定参数名称**
调用函数的时候，可以指定参数名称，从而用户可以不用关注调用参数的位置顺序，示例如下：

```python
def showInfo(name, sex):
    print("姓名：%s 性别：%s" % (name, sex))


showInfo(sex="男", name="老王")  # 输出：姓名：老王 性别：男
```

> 注意：如果要指定参数的名称，那么所有的非缺省参数都必须全部指定参数名称，python不支持，部分指定参数名的使用方式。

**不定参数**
不定参数使用*参数名来表示，示例如下：

```python
def doPrint(name, *list):
    print(name)
    for item in list:
        print(item)


doPrint("老王", "你好")  # 输出：老王 你好
doPrint("老王", "你好", "大家好")  # 输出：老王 你好 大家好
```

同时存在缺省参数和不定参数示例：

```python
def doPrint(name, age=18, *list):
    print("姓名：%s 年龄：%d" % (name, age))
    for item in list:
        print(item)


doPrint("老王", 19, "你好", "世界")

# 输出如下：
# 姓名：老王 年龄：19
# 你好
# 世界
```

> 注意：不定参数任何情况下都要放在参数最后方。

----------

## 类 ##

python中用class关键声明类，推荐使用驼峰命名法，首字母大写。
python中的类格式：

```python
class 类名():
	def __init__(self,参数):
		self.参数 = 参数
	其他方法体
```

示例如下：

```python
class Car():
    def __init__(self, name):
        self.name = name

    def printInfo(self, color):
        print("车型：%s 颜色：%s" % (self.name, color))


mycar = Car("福特")
mycar.printInfo("白色")
# 输出：车型：福特 颜色：白色
```

> 注意：构造函数为__init__(self,参数)此方法是不能省略的。

**类继承，方法重写**
类继承语法格式：

```python
class 子类名称(父类名称):
```

类继承&重写方法，示例如下：

```python
class Car():
    def __init__(self, name):
        self.name = name

    def printInfo(self, color):
        print("车型：%s 颜色：%s" % (self.name, color))


class WhiteCar(Car):
    def __init__(self, name):
        # 初始化父类构造函数
        super().__init__(name)

    def printInfo(self, mileage):
        print("车型：%s 颜色：白色 行驶里程：%dKM" % (self.name, mileage))


wCar = WhiteCar("福特")
wCar.printInfo(1000)
```

> 注意：super().__init__(参数)初始化父类构造函数不能省。

# 模块 #

python代码存储的文件叫做模块。



## import语句

import模块引入
首先，定义一个简单cat.py文件，示例如下：

```python
# coding=utf-8


def sayHi():
    print("喵喵喵~")
```

在其他地方使用sayHi()方法，示例如下：

```python
import cat

cat.sayHi()
# 输出：喵喵喵~
```

## from...import语句 ##

from...import模块引入的格式是：

```python
from 模块 import 函数集合...
```

首先，定义一个简单cat.py文件，示例如下：

```python
# coding=utf-8


def sayHi():
    print("喵喵喵~")


def showColor():
    print("白色")
```

在其他地方调用sayHi()方法和showColor()方法，示例如下：

```python
from cat import showColor as color, sayHi as hi

hi()
# 输出：喵喵喵

color()
# 输出：白色
```

> 小技巧：
> 1.使用as可重命名函数名。
> 2.导入一个模块的所有方法可以使用：from 模块 import *

# 文件操作 #

----------

## 读取文件 ##

读取文件使用python内置方法open()打开文件，使用.read()读取全部内容，示例如下：

```python
path = "c:\py.txt"
fi = open(path, "r")
print(fi.read())
fi.close()
```

**with语法**

with是python2.5引入的自动释放资源的语法模式，确保使用过程中不管是否发生了异常，都会释放资源.
使用with读取文件，是不需要自己手动close的，示例如下：

```python
with open(path) as fi:
    print(fi.read())
```

**逐行读取文件**
.read()是读取文件的全部内容，使用.readlines()，示例如下：

```python
with open(path, "r") as fi:
    lines = fi.readlines()
print(len(lines))
```

**open方法的模式**

上面的示例可以看出来，open(目录,操作模式)的时候必须指定操作模式，open的操作模式：

> 读取模式：'r'(默认模式) | 写入模式：'w' | 附加模式：'a'.

python模式是读取，所以"r"可以省略，示例如下：

```python
path = "c:\py.txt"
fi = open(path)
print(fi.read())
fi.close()
```

## 文件写入 ##

文件写入分为两种方式，一种是覆盖(w)，另一种是追加(a)。

文件覆盖的写入，代码如下：

```python
with open(path, "w") as fi:
    fi.write("第一行\n第二行\n第三行")
```

文件的追加，代码如下：

```python
with open(path, "a") as fi:
    fi.write("\n第1行\n第2行\n第3行")
```

> 注意：文件写入，如果文件不存在会重建，不会报错。

## 更多文件操作 ##

os.getcwd()：获取当前运行目录

os.listdir(path)：获取指定目录下的列表

os.path.exists(path)：检查是否存在文件或文件夹

os.mkdir(path)：创建文件夹，已经存在文件会报错

os.remove(path)：删除文件夹，只能删除文件夹

os.rename(old, new)：文件重命名

示例如下：

```python
import os

# 获取当前程序运行目录
print(os.getcwd())

# 获取指定目录下的列表
print(os.listdir("E:\\server"))

# 检查是否存在文件或文件夹
print(os.path.exists("E:\\test\\a.txt"))

# 创建文件夹，已经存在文件会报错
os.mkdir("E:\\test1")

# 删除文件夹，只能删除文件夹
os.remove("E:\\test1")

# 文件重命名
os.rename("E:\\test\\a.txt", "E:\\test\\b.txt")

```

# 异常处理

python中使用try/except/finally关键字处理异常.

语法格式：

```python
try:
	代码
except 异常类型1:
	处理	
except 异常类型2:
	处理	
finally:
	代码（没有异常）
```

基础示例：

```python
path = "c:\pytest.txt"
try:
    fi = open(path)
    try:
        fi.read()
    finally:
        fi.close()
        print("最后都会执行")
except FileNotFoundError:
    print("找不到相应的文件")
except IOError:
    print("文件操作失败IOError")
```

异常类型
----------

BaseException	所有异常的基类

SystemExit	解释器请求退出

KeyboardInterrupt	用户中断执行(通常是输入^C)

Exception	常规错误的基类

StopIteration	迭代器没有更多的值

GeneratorExit	生成器(generator)发生异常来通知退出

StandardError	所有的内建标准异常的基类

ArithmeticError	所有数值计算错误的基类

FloatingPointError	浮点计算错误

OverflowError	数值运算超出最大限制

ZeroDivisionError	除(或取模)零 (所有数据类型)

AssertionError	断言语句失败

AttributeError	对象没有这个属性

EOFError	没有内建输入,到达EOF 标记

EnvironmentError	操作系统错误的基类

IOError	输入/输出操作失败

OSError	操作系统错误

WindowsError	系统调用失败

ImportError	导入模块/对象失败

LookupError	无效数据查询的基类

IndexError	序列中没有此索引(index)

KeyError	映射中没有这个键

MemoryError	内存溢出错误(对于Python 解释器不是致命的)

NameError	未声明/初始化对象 (没有属性)

UnboundLocalError	访问未初始化的本地变量

ReferenceError	弱引用(Weak reference)试图访问已经垃圾回收了的对象

RuntimeError	一般的运行时错误

NotImplementedError	尚未实现的方法

SyntaxError	Python 语法错误

IndentationError	缩进错误

TabError	Tab 和空格混用

SystemError	一般的解释器系统错误

TypeError	对类型无效的操作

ValueError	传入无效的参数

UnicodeError	Unicode 相关的错误

UnicodeDecodeError	Unicode 解码时的错误

UnicodeEncodeError	Unicode 编码时错误

UnicodeTranslateError	Unicode 转换时错误

Warning	警告的基类

DeprecationWarning	关于被弃用的特征的警告

FutureWarning	关于构造将来语义会有改变的警告

OverflowWarning	旧的关于自动提升为长整型(long)的警告

PendingDeprecationWarning	关于特性将会被废弃的警告

RuntimeWarning	可疑的运行时行为(runtime behavior)的警告

SyntaxWarning	可疑的语法的警告

UserWarning	用户代码生成的警告

# 垃圾回收gc #

----------

python的垃圾收回机制不想c和c++是开发者自己管理维护内存的，python的垃圾回收是系统自己处理的，所以作为普通的开发者，我们不需要关注垃圾回收部分的内容，如果想要深层次理解python请继续看下文。

**python垃圾回收机制**
Python的GC模块主要运用了引用计数来跟踪和回收垃圾。在引用计数的基础上，还可以通过“标记－清除”解决容器对象可能产生的循环引用的问题。通过分代回收以空间换取时间进一步提高垃圾回收的效率。

## 引用计数 ##

**原理：**当一个对象的引用被创建或者复制时，对象的引用计数加1；当一个对象的引用被销毁时，对象的引用计数减1，当对象的引用计数减少为0时，就意味着对象已经再没有被使用了，可以将其内存释放掉。

**优点：**引用计数有一个很大的优点，即实时性，任何内存，一旦没有指向它的引用，就会被立即回收，而其他的垃圾收集技术必须在某种特殊条件下才能进行无效内存的回收。

**缺点：**但是它也有弱点，引用计数机制所带来的维护引用计数的额外操作与Python运行中所进行的内存分配和释放，引用赋值的次数是成正比的，这显然比其它那些垃圾收集技术所带来的额外操作只是与待回收的内存数量有关的效率要低。同时，引用技术还存在另外一个很大的问题－循环引用，因为对象之间相互引用，每个对象的引用都不会为0，所以这些对象所占用的内存始终都不会被释放掉。

## 标记－清除 ##

标记－清除只关注那些可能会产生循环引用的对象，显然，像是PyIntObject、PyStringObject这些不可变对象是不可能产生循环引用的，因为它们内部不可能持有其它对象的引用。Python中的循环引用总是发生在container对象之间，也就是能够在内部持有其它对象的对象，比如list、dict、class等等。这也使得该方法带来的开销只依赖于container对象的的数量。

**原理：**

1. 寻找跟对象（root object）的集合作为垃圾检测动作的起点，跟对象也就是一些全局引用和函数栈中的引用，这些引用所指向的对象是不可被删除的；
2. 从root object集合出发，沿着root object集合中的每一个引用，如果能够到达某个对象，则说明这个对象是可达的，那么就不会被删除，这个过程就是垃圾检测阶段；
3. 当检测阶段结束以后，所有的对象就分成可达和不可达两部分，所有的可达对象都进行保留，其它的不可达对象所占用的内存将会被回收，这就是垃圾回收阶段。（底层采用的是链表将这些集合的对象连接在一起）；

**缺点：**标记和清除的过程效率不高。

## 分代回收 ##

**原理：**将系统中的所有内存块根据其存活时间划分为不同的集合，每一个集合就成为一个“代”，Python默认定义了三代对象集合，垃圾收集的频率随着“代”的存活时间的增大而减小。也就是说，活得越长的对象，就越不可能是垃圾，就应该减少对它的垃圾收集频率。那么如何来衡量这个存活时间：通常是利用几次垃圾收集动作来衡量，如果一个对象经过的垃圾收集次数越多，可以得出：该对象存活时间就越长。

# 多线程
----------

python是一门解析性语言，python的解析器默认也是单线程的，但python3提供几个用于多线程编程的模块，_thread、threading
。
python2中的thread已经废弃，为了兼容性在python3中使用_thread代替，_thread提供了原始的线程操作和简单的锁，推荐使用threading模块。


## _thread模块 ##

_thread模块实现多线程，使用_thread.start_new_thread()方法实现，示例如下：

```python
import _thread
import time


def sayHi(name):
    num = 0
    while num < 5:
        num += 1
        time.sleep(1)
        print("%s执行——现在时间：%s\n" % (name, time.strftime(
            "%Y-%m-%d %H:%M:%S", time.localtime(time.time()))))


try:
    _thread.start_new_thread(sayHi, ("线程1",))
    _thread.start_new_thread(sayHi, ("线程2",))
except:
    print("线程启动失败")

# 保持主线程10s后退出
time.sleep(10)

# 输出结果如下：
# 线程1执行——现在时间：2018-04-12 14: 30: 38
# 线程2执行——现在时间：2018-04-12 14: 30: 38
# 线程1执行——现在时间：2018-04-12 14: 30: 39
# 线程2执行——现在时间：2018-04-12 14: 30: 39
# 线程2执行——现在时间：2018-04-12 14: 30: 40
# 线程1执行——现在时间：2018-04-12 14: 30: 40
# 线程1执行——现在时间：2018-04-12 14: 30: 41
# 线程2执行——现在时间：2018-04-12 14: 30: 41
# 线程2执行——现在时间：2018-04-12 14: 30: 42
# 线程1执行——现在时间：2018-04-12 14: 30: 42
```

## threading模块 ##

threading除了包含_thread的所有方法外，还提供了其他方法：

threading.currentThread()：返回当前线程变量

threading.enumerate()：返回正在运行的线程列表

threading.activeCount()：返回当前正在运行的线程数量和len(threading.enumerate())效果相同

run()：用于表示线程活动的方法

start()：启动线程活动

join([time])：等待至线程结束

isAlive()：线程是否是活跃的

getName()：返回线程名

setName()：设置线程名


**threading基础使用**

```python
import time
import threading


class myThreading(threading.Thread):
    def __init__(self, name):
        threading.Thread.__init__(self)
		self.name = name

    def run(self):
        time.sleep(1)
        print("%s 时间：%s" %
              (self.name, time.strftime("%Y-%m-%d %H:%M:%S", time.localtime(time.time()))))


td1 = myThreading("threading1")
td2 = myThreading("threading2")
td1.start()
td2.start()

td1.join()
td2.join()

# 输出：
# Thread-1 时间：2018-04-14 11:19:54
# Thread-2 时间：2018-04-14 11:19:54
```

**threading线程同步**

threading使用threading.Lock()来同步线程，threading.Lock()包含锁定的方法acquire()和解锁的方法release()来同步线程，示例如下：

```python
import time
import threading


class myThreading(threading.Thread):
    def __init__(self, name):
        threading.Thread.__init__(self)
		self.name = name

    def run(self):
        # 上锁
        threadLock.acquire()
        time.sleep(1)
        print("%s 时间：%s" %
              (self.name, time.strftime("%Y-%m-%d %H:%M:%S", time.localtime(time.time()))))
        # 解锁
        threadLock.release()


# 声明线程同步锁
threadLock = threading.Lock()

td1 = myThreading("threading1")
td2 = myThreading("threading2")
td1.start()
td2.start()

td1.join()
td2.join()
```

# 时间模块 #

python中使用时间需要导入time模块，使用time.time()方法获取当前时间戳，示例如下：

```python
# 导入time模块
import time

print(time.time())
# 输出：1523584077.842348

```

## 格式化时间 ##

格式化时间，使用time中的strftime()，示例如下：

```python
# 导入time模块
import time

print(time.time())
# 输出：1523584077.842348

print(time.localtime(time.time()))
# 输出：time.struct_time(tm_year=2018, tm_mon=4, tm_mday=13, tm_hour=9, tm_min=50, tm_sec=12, tm_wday=4, tm_yday=103, tm_isdst=0)

# 时间格式化
print(time.strftime("%Y-%m-%d %H:%M:%S", time.localtime(time.time())))
# 输出：2018-04-13 09:52:10
```

**程序计时器**

使用场景：有时候我们需要计算程序的运行时长，使用以下代码：

```python
import datetime
import time

#开始计时
startTime = datetime.datetime.now()

time.sleep(1)

#结束计时
endTime = datetime.datetime.now()
print(endTime - startTime)
#输出：0:00:01.000791
```


## 格式化符号说明 ##

%y 两位数的年份表示（00-99）

%Y 四位数的年份表示（000-9999）

%m 月份（01-12）

%d 月内中的一天（0-31）

%H 24小时制小时数（0-23）

%I 12小时制小时数（01-12）

%M 分钟数（00=59）

%S 秒（00-59）

%a 本地简化星期名称

%A 本地完整星期名称

%b 本地简化的月份名称

%B 本地完整的月份名称

%c 本地相应的日期表示和时间表示

%j 年内的一天（001-366）

%p 本地A.M.或P.M.的等价符

%U 一年中的星期数（00-53）星期天为星期的开始

%w 星期（0-6），星期天为星期的开始

%W 一年中的星期数（00-53）星期一为星期的开始

%x 本地相应的日期表示

%X 本地相应的时间表示

%Z 当前时区的名称

%% %号本身



# http模块 #

python中的http/https请求使用urllib库，使用urllib的request模块的发送get和post请求。



## get请求

请求网页地址并返回网页html内容，示例如下：

```python
from urllib import request


def getHtml(url):
    with request.urlopen(url) as r:
        data = r.read()
        return data.decode("utf-8")


print(getHtml("http://vipstone.cnblogs.com"))
```

对返回的数据进行编码处理data.decode("utf-8")即可。

## post请求 ##

post请求并传递参数，对参数进行encode处理，示例如下：

```python
from urllib import request, parse


params = parse.urlencode([("name", "老王"), ("pwd", "123456")])
req = request.Request("http://127.0.0.1:8360/video/login")
req.add_header("User-Agent", "Mozilla/6.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/8.0 Mobile/10A5376e Safari/8536.25")

with request.urlopen(req, data=params.encode("utf-8")) as r:
    data = r.read()
    print(data.decode("utf-8"))
```

如上所示，需要使用urllib的parse对参数进行编码处理，也可以给http头添加内容。

# 常用内置模块

常用内置模块列表：

- os
- sys
- json

## os模块 

os.getcwd() #获取当前程序目录

os.listdir('dirname') #列出指定目录下的所有文件和子目录，包括隐藏文件，并以列表方式打印

os.remove() #删除一个文件

os.rename("oldname","newname") #重命名文件/目录

os.path.isfile(path) #如果path是一个存在的文件，返回True，否则返回False

os.path.exists(path) #如果path存在，返回True；如果path不存在，返回False

os.path.getatime(path) #返回path所指向的文件或者目录的最后存取时间

os.path.getmtime(path) #返回path所指向的文件或者目录的最后修改时间

## sys模块 

sys.exit(n) #退出程序，正常退出时exit(0)

sys.version  #获取Python解释程序的版本信息

sys.maxint #最大的Int值

sys.platform #返回操作系统平台名称

## json模块

json模块用于字符串 和 python数据类型间进行转换

json模块提供了四个功能：dumps、dump、loads、load

dumps、dump #把对象转换成str

loads、load #把str转换成json

