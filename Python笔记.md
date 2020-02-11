#Python

## 数据类型

###整数

###浮点数

###字符串

​	python字符串是以单引号`'`或双引号`"`括起来的任意文本,如果字符串内部既包含`'`又包含`"`可以用转义字符`\`来标识,如果字符串内部有很多换行，用`\n`写在一行里不好阅读，为了简化，Python允许用`'''...'''`的格式表示多行内容.

###list以及tuple

####list

​	Python内置的一种数据类型是列表：list。list是一种有序的集合，可以随时添加和删除其中的元素。list里面的元素的数据类型也可以不同,也可以是另一个list.

```python
#list的申明
classmates = ['Michael', 'Bob', 'Tracy']
#list元素属性的获取
len(classmates) #获取list的长度
classmates[1] classmates[-1] #获取指定位置的元素 正数表示正序,复数表示逆序-1为最后一个
classmates.insert(1, 'Jack') #指定位置插入元素
classmates.append('Adam') #末尾添加元素
classmates.pop() #删除末尾元素
classmates.pop(1) #删除指定位置元素
classmates[1] = 'Sarah' #指定位置元素赋值
```

####tuple

​	另一种有序列表叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改.它也没有append()，insert()这样的方法。其他获取元素的方法和list是一样的.

```python
#tuple的定义
t = (1, 2) #定义一个多元素的tuple
t = () #如果要定义一个空的tuple
t = (1,) #定义一个只有1个元素的tuple
```

### dict

​	Python内置了字典：dict的支持，dict全称dictionary，在其他语言中也称为map，使用键-值（key-value）存储。

```python
#dict声明
d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
#dict元素的操作
v = d['Adam'] #获取指定key的值
d['Adam'] = 67 #添加元素，或者修改指定key对应的值
key in dict #判断key是否在dict中存在
pop(key) #删除
```

### set

​	set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。

```python
#set声明
s = set([1, 2, 3])
#set的元素操作
s.add(value) #增
s.remove(value) #删
```

------

## 流程控制

### 条件判断

​	python的条件判断可以通过if : else,以及else if (elif)

```python
if condition1:
    ...
else if condition2:
    ...
elif condition3:
  	...
else:
    ...
```

### 循环

​	python中循环可以使用`for x in ...:`语句，`for x in ...`循环就是把每个元素代入变量`x`，然后执行缩进块的语句。

```python
for a in collection:
    ....
```

------

## 函数

### 函数的调用

[python内置函数文档]: https://docs.python.org/3/library/functions.html

### 定义函数

​	在Python中，定义一个函数要使用`def`语句，依次写出函数名、括号、括号中的参数和冒号`:`，然后，在缩进块中编写函数体，函数的返回值用`return`语句返回。

```python
def fuc(x):
    if a:
        return 
    else：
    	return
```

​	如果想定义一个什么事也不做的空函数，可以用`pass`语句

```pytho
def nop():
    pass
```

### 函数的参数

​	Python的函数定义非常简单，但灵活度却非常大。除了正常定义的必选参数外，还可以使用默认参数、可变参数和关键字参数，使得函数定义出来的接口，不但能处理复杂的参数，还可以简化调用者的代码。

####位置参数

```python
def power(x):
    return x * x
```

​	对于`power(x)`函数，参数`x`就是一个位置参数，当我们调用`power`函数时，必须传入有且仅有的一个参数`x`。

####默认参数

​	对函数中的某个参数可以对其设置默认值，调用是不传入则取默认值。

```python
def power(x, n=默认值):
    return
```

设置默认参数时，有几点要注意：

1. 是必选参数在前，默认参数在后，否则Python的解释器会报错；

2. 是如何设置默认参数。
3. 默认参数必须指向不变对象

####可变参数

​	可变参数就是传入的参数个数是可变的，定义可变参数在参数前面加了一个`*`号。在函数内部，参数`numbers`接收到的是一个tuple，因此，函数代码完全不变。但是，调用该函数时，可以传入任意个参数，包括0个参数：

```python
def fuc(*number):
    return
#调用
fuc(1,2,3,4,5)
#当传入list或者tuple到可变参数时
fuc(*list)
```

#### 关键字参数

​	可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict。

```python
def fuc(a,b,**kw):
#调用
fuc(1,2,key='value'...)
#当传入dict参数到关键字参数时
fuc(1,2,**dict)
```

####命名关键字参数

​	对于关键字参数，函数的调用者可以传入任意不受限制的关键字参数。至于到底传入了哪些，就需要在函数内部通过`kw`检查。如果要限制关键字参数的名字，就可以用命名关键字参数，命名关键字参数需要一个特殊分隔符`*`，`*`后面的参数被视为命名关键字参数。

```python
def fuc(a,b,*,key1,key2):
    return
#调用
fuc(1,2,key='value'...)
#如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了
def fuc(a,b,*p1,key1,key2):
  	return
#调用
fuc(1,2,3,4,key='value'...)
```

**命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错**

------

## 高级特性

### 切片

​	用于取集合中指定位置数量的元素

```python
L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
L[0:3] #取0到3位的元素，不包含3
L[-2:-1] #取倒数第二到第一的元素
L[10:] #取前10个元素
```

### 迭代

​	如果给定一个list或tuple，我们可以通过`for`循环来遍历这个list或tuple，这种遍历我们称为迭代（Iteration），在Python中，迭代是通过`for ... in`来完成的在Python中，迭代是通过`for ... in`来完成的。

*   对于list和tuple使用`for...in:`完成迭代
*   对于dict可以使用`for value in dict.values()`取所有的value，或使用`for k,v in dict.items()`取所有键值对

###列表生成器

```python
[对元素的操作 for...in... for...in...]
```

### 生成器

​	如果列表元素可以按照某种算法推算出来，这样就不必创建完整的list，从而节省大量的空间。在Python中，这种一边循环一边计算的机制，称为生成器：generator。

```python
g = (x * x for x in range(10))
next(g) #获取生成器的下一个值
```

### 迭代器

​	通过`iter(list)`方法获取集合的迭代器使用`next()`方法进行迭代获取元素

------

## 高阶函数

​	变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。

###map

​	`map()`函数接收两个参数，一个是函数，一个是`Iterable`，`map`将传入的函数依次作用到序列的每个元素，并把结果作为新的`Iterator`返回。

```python
def f(x):
	return x * x

r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

​	`map()`传入的第一个参数是`f`，即函数对象本身。由于结果`r`是一个`Iterator`，`Iterator`是惰性序列，因此通过`list()`函数让它把整个序列都计算出来并返回一个list。

###reduce

​	`reduce`把一个函数作用在一个序列`[x1, x2, x3, ...]`上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算。

```python
from functools import reduce
def add(x, y): #add 必须接受两个参数
	return x + y

reduce(add, [1, 3, 5, 7, 9])
```

### filter

​	Python内建的`filter()`函数用于过滤序列。`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。

```python
def is_odd(n):
    return n % 2 == 1

list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
```

### sorted

​	Python内置的`sorted()`函数就可以对list进行排序，`sorted()`函数也是一个高阶函数，它还可以接收一个`key`函数来实现自定义的排序。要进行反向排序，不必改动key函数，可以传入第三个参数`reverse=True`

```python
sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]
```

### 返回函数

​	高阶函数除了可以接受函数作为参数外，还可以把函数作为结果值返回。

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum #计算结果保存在sum函数中
```

### 匿名函数

​	在Python中，对匿名函数提供了有限支持。关键字`lambda`表示匿名函数，冒号前面的`x`表示函数参数。

```python
list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
f = lambda x: x * x
f(5) #25
```

### 装饰器

​	由于函数也是一个对象，而且函数对象可以被赋值给变量，所以，通过变量也能调用该函数。函数对象有一个`__name__`属性，可以拿到函数的名字。函数调用前后自动打印日志，但又不希望修改原函数的定义，这种在代码运行期间动态增加功能的方式，称之为“装饰器”（Decorator）。本质上，decorator就是一个返回函数的高阶函数。

```python
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
#借助Python的@语法，把decorator置于函数的定义处
@log
def fuc：
	return
```

------

## 面向对象

### 类的定义

​	`class`后面紧接着是类名，即`Student`，类名通常是大写开头的单词，紧接着是`(object)`，表示该类是从哪个类继承下来的。

​	通过定义一个特殊的`__init__`方法，在创建实例的时候，就把`name`，`score`等属性绑上去，`__init__`方法的第一个参数永远是`self`，表示创建的实例本身，因此，在`__init__`方法内部，就可以把各种属性绑定到`self`，因为`self`就指向创建的实例本身。	

```python
class Student(object):
    def __init__(self, name, score): #构造方法
        self.__name = name
        self.__score = score
    
    def getName():
        return __name
        
bart = Student()
```

### 获取类的信息

​	type()方法可以获取对象类型

​	isinstance(a,类名)可以判断a是否属于后面的类型

​	dir()可以获取对象的所有信息

------

## 面向对象的高级编程

###使用\_\_slots\_\_

​	创建了一个class的实例后，我们可以给该实例绑定任何属性和方法，这就是动态语言的灵活性。

```python
class Student(object):
    pass

__slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称

s = Student()
s.name = 'Michael' # 动态给实例绑定一个属性

def set_age(self, age): # 定义一个函数作为实例方法
	self.age = age

from types import MethodType
s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
```

### @property

​	Python内置的`@property`装饰器就是负责把一个方法变成属性调用的。

```python
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
        
    @property
    def age(self): #定义只读属性
        return self._age
```

##错误，异常处理

````python
try:
    print('try...')
    r = 10 / 0
    print('result:', r)
except ZeroDivisionError as e:
    print('except:', e)
finally:
    print('finally...')
print('END')
````

### 断言

```python
def foo(s):
    n = int(s)
    assert n != 0, 'n is zero!'
    return 10 / n

def main():
    foo('0')
```

### logger

```python
import logging
logging.basicConfig(level=logging.INFO)

s = '0'
n = int(s)
logging.info('n = %d' % n)
print(10 / n)
```

### 单元测试

​	编写单元测试，我们需要引入Python自带的`unittest`模块

```python
import unittest

from mydict import Dict

class TestDict(unittest.TestCase):

    def test_init(self):
        d = Dict(a=1, b='test')
        self.assertEqual(d.a, 1)
        self.assertEqual(d.b, 'test')
        self.assertTrue(isinstance(d, dict))

    def test_key(self):
        d = Dict()
        d['key'] = 'value'
        self.assertEqual(d.key, 'value')

    def test_attr(self):
        d = Dict()
        d.key = 'value'
        self.assertTrue('key' in d)
        self.assertEqual(d['key'], 'value')

    def test_keyerror(self):
        d = Dict()
        with self.assertRaises(KeyError):
            value = d['empty']

    def test_attrerror(self):
        d = Dict()
        with self.assertRaises(AttributeError):
            value = d.empty
```

## 文件读写

### 读文件

​	要以读文件的模式打开一个文件对象，使用Python内置的`open()`函数，传入文件名和标示符

```python
try:
    f = open('/path/to/file', 'r',encoding='gbk', errors='ignore')
    print(f.read())
finally:
    if f:
        f.close()
```

​	Python引入了`with`语句来自动帮我们调用`close()`方法，简写为

```python
with open('/path/to/file', 'r') as f:
    print(f.read())

#支持的方法有
f.read() #一次读取文件全部内容
f.readline() #读取一行
f.readlines() #读取所有行，返回每行内容的list
```

###写文件

```python
with open('/Users/michael/test.txt', 'w') as f:
    f.write('Hello, world!')
```

------

## 多线程

###多进程

​	Unix/Linux操作系统提供了一个`fork()`系统调用，它非常特殊。普通的函数调用，调用一次，返回一次，但是`fork()`调用一次，返回两次，因为操作系统自动把当前进程（称为父进程）复制了一份（称为子进程），然后，分别在父进程和子进程内返回。

​	子进程永远返回`0`，而父进程返回子进程的ID。这样做的理由是，一个父进程可以fork出很多子进程，所以，父进程要记下每个子进程的ID，而子进程只需要调用`getppid()`就可以拿到父进程的ID。

```python
import os

print('Process (%s) start...' % os.getpid())
# Only works on Unix/Linux/Mac:
pid = os.fork()
if pid == 0:
    print('I am child process (%s) and my parent is %s.' % (os.getpid(), os.getppid()))
else:
    print('I (%s) just created a child process (%s).' % (os.getpid(), pid))
```

### 多线程

​	Python的标准库提供了两个模块：`_thread`和`threading`，`_thread`是低级模块，`threading`是高级模块，对`_thread`进行了封装。绝大多数情况下，我们只需要使用`threading`这个高级模块。启动一个线程就是把一个函数传入并创建`Thread`实例，然后调用`start()`开始执行

```python
import time, threading

# 新线程执行的代码:
def loop():
    print('thread %s is running...' % threading.current_thread().name)
    n = 0
    while n < 5:
        n = n + 1
        print('thread %s >>> %s' % (threading.current_thread().name, n))
        time.sleep(1)
    print('thread %s ended.' % threading.current_thread().name)

print('thread %s is running...' % threading.current_thread().name)
t = threading.Thread(target=loop, name='LoopThread')
t.start()
t.join()
print('thread %s ended.' % threading.current_thread().name)
```

