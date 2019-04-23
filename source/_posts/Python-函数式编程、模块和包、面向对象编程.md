---
title: Python--函数式编程、模块和包、面向对象编程
date: 2019-04-23 21:41:07
tags: Python
categories: 后端
---

Python--函数式编程、模块和包、面向对象编程
<escape><!-- more --></escape>

### 1 函数式编程
#### 高阶函数
高阶函数：能接收一个函数作为参数的函数。
Python的函数不但可以返回int、str、list、dict等数据类型，还可以返回函数。

Python 内置的高阶函数：

**map(f, list)** 把函数 f 依次作用在 list 的每个元素上，得到一个新的 list 并返回。
```py
def f(x):
  return x*x
print map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
```

**reduce(f, list)** reduce()传入的函数 f 必须接收两个参数，reduce()对list的每个元素反复调用函数f，并返回最终结果值。reduce()还可以接收第3个可选参数，作为计算的初始值。
```py
# 求一个list的积：
def prod(x, y):    
    return x*y
print reduce(prod, [2, 4, 5, 7, 12])
```

**filter(f, list)** 函数 f 的作用是对每个元素进行判断，返回 True或 False，filter()根据判断结果自动过滤掉不符合条件的元素，返回由符合条件元素组成的新list。
```py
# 利用filter()过滤出1~100中平方根是整数的数：
import math
def is_sqr(x):
  temp = int(math.sqrt(x))
    if temp*temp==x:
      return x
print filter(is_sqr, range(1, 101))

# s.strip(rm) 删除 s 字符串中开头、结尾处的 rm 序列的字符。当rm为空时，默认删除空白符（包括'\n', '\r', '\t', ' ')：
a = '     123'
a.strip()
# '123'
```

**sorted(list, f)** 函数可对list进行排序，可以接收一个比较函数来实现自定义排序，比较函数的定义是，传入两个待比较的元素 x, y如果 x 应该排在 y 的前面，返回 -1，如果 x 应该排在 y 的后面，返回 1。如果 x 和 y 相等，返回 0。
```py
sorted([36, 5, 12, 9, 21])
# [5, 9, 12, 21, 36]

# sorted() 函数实现倒叙排序：
def reversed_cmp(x, y):
    if x > y:
        return -1
    if x < y:
        return 1
    return 0
     
sorted([36, 5, 12, 9, 21], reversed_cmp)
# [36, 21, 12, 9, 5]
```

**sort 与 sorted 区别：**
`sort` 是应用在 list 上的方法，`sorted` 可以对所有可迭代的对象进行排序操作。
list 的 `sort` 方法返回的是对已经存在的列表进行操作，无返回值，而内建函数 `sorted` 方法返回的是一个新的 list，而不是在原来的基础上进行的操作。

#### 闭包
闭包的特点是返回的函数还引用了外层函数的局部变量，所以，要正确使用闭包，就要确保引用的局部变量在函数返回后不能变。
```py
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs
 
f1, f2, f3 = count()
# 9 9 9
# 以上代码期望输出的是 1 4 9，但最终结果却是 9 9 9 。改动代码达到期望效果：

def count():
    fs = []
    for i in range(1, 4):
        def f(i = i):
             return i*i
        fs.append(f)
    return fs
 
f1, f2, f3 = count()
# 1 4 9
```

#### 匿名函数
匿名函数 lambda x: x * x 实际上就是：
```py
def f(x):
    return x * x
# 关键字lambda 表示匿名函数，冒号前面的 x 表示函数参数。
```
ps：匿名函数有个限制，就是只能有一个表达式，不写 return，返回值就是该表达式的结果。

#### 装饰器
目的：简化代码，避免每个函数编写重复的代码。
```py
# 编写一个无参数的@performance，它可以打印出函数调用的时间：
import time

def performance(f):
  def wrapper(*args, **kw):
    t1 = time.time()
    res = f(*args, **kw)
    t2 = time.time()
    print 'call %s() in %s' %(f.__name__, (t2 - t1))
    return res
  return wrapper

@performance
def factorial(n):
  return reduce(lambda x,y: x*y, range(1, n+1))

print factorial(10)

# 给 @performace 增加一个参数，允许传入's'或'ms'：
import time

def performance(unit):
  def log_decorator(f):
    def wrapper(*args,**kw):
      t1 = time.time()
      res = f(*args, **kw)
      t2 = time.time()
      t = (t2 - t1)*1000 if unit =='ms' else (t2 - t1)
      print 'call %s() in %s %s'%(f.__name__, t, unit)
      return res
    return wrapper
  return log_decorator

@performance('ms')
def factorial(n):
  return reduce(lambda x,y: x*y, range(1, n+1))

print factorial(10)
```

`@decorator` 可以动态实现函数功能的增加，但是，经过`@decorator` “改造”后的函数的函数名等属性也会被改造，变成改造器内部定义的wrapper，我们也很难把原函数的所有必要属性都一个一个复制到新函数上，所以Python内置的functools可以用来自动化完成这个“复制”的任务：
在wrapper函数前使用`@functools.wraps(f)`
```py
@functools.wraps(f)        
def wrapper(*args, **kw):
    # do something
```

#### 偏函数
functools.partial可以把一个参数多的函数变成一个参数少的新函数，少的参数需要在创建时指定默认值，这样，新函数调用的难度就降低了。


### 2 模块和包
包是文件夹，模块是.py文件，每一层包下面必须有一个__init__.py文件，即使包下面没有其他文件，因为只有这样，python才会把该文件夹当成包来处理。
目的：避免同一个名字的变量互相影响
#### 导入模块
```py
# import... 文件中导入模块：
import math
# math.pow(2,3)

# from...import... 导入用到的math模块的某几个函数，而不是所有函数：

from math import pow, sin, log
# pow(2,3)

# from...import...as... 解决不同模块的命名冲突：
from math import pow as poww
# poww(2,3)

# 两个不同的模块中相同功能的 StringIO 函数，没有模块 cStringIO ，则会从 StringIO 模块中导入 StringIO 函数： 
try:
    from cStringIO import StringIO
except ImportError:
    from StringIO import StringIO

# 两个不同名称功能相同的模块，没有模块 json ，则会导入 simplejson ，且重命名为 json ：
try:
    import json as json
except ImportError:
    import simplejson as json
```

#### 使用新版本的特性
当新版本的一个特性与旧版本不兼容时，该特性将会在旧版本中添加到__future__中，以便旧的代码能在旧版本中测试新特性。
```py
from __future__ import  #新规则名称

# 在Python 3.x中，字符串统一为unicode，不需要加前缀 u，而以字节存储的str则必须加前缀 b，__future__的unicode_literal可以兼容
from __future__ import unicode_literals
```

#### 模块管理工具
模块管理工具：easy_install、pip or pip3(Python 推荐，已内置到Python2.7.9)
查找第三发模块的名字：https://pypi.python.org/


### 3 面向对象编程
#### 类和实例
类用于定义抽象类型（class Person(object):）
实例根据类的定义被创建出来（nola = Person()）

#### 定义属性
**设置类的属性**：类属性是直接绑定在类上的，所以，访问类属性不需要创建实例，就可以直接访问。

**设置实例的属性**：在__init__() 里除了可以直接使用self.name = 'xxx'设置一个属性外，还可以通过 setattr(self, 'name', 'xxx') 设置属性。一个实例的私有属性就是以__开头的属性，无法被外部访问，但是，从类的内部是可以访问的。

```py
class Person(object):
  # 设置类的属性
  count = 0
  def __init__(self, name, age):
    # 设置实例的属性
    self.name = name
    setattr(self, 'age', age) 

    Person.count += 1

p = Person('Nola', 24)
print Person.count
print p.name  
print p.age 
```
ps：如果一个属性由双下划线开头"__"，该属性就无法被外部访问；如果一个属性以"__xxx__"的形式定义。类属性可以动态添加和修改。
ps：当实例属性和类属性重名时，实例属性优先级高。

#### 定义方法
**定义类方法**：通过标记一个 @classmethod，该方法将绑定到 Person 类上，而不是类的实例上。类方法的第一个参数将传入类本身，通常将参数名命名为 cls。调用方式：Person.how_many()。
```py
class Person(object):
    __count = 0   # 定义一个类属性，由于双下划线开头，只有类中可以访问到，实例上访问不到
    @classmethod  # 定义一个类方法，类可以调用，可以用于获取类中的属性
    def how_many(cls):
        return cls.__count  # cls.count 实际上相当于 Person.count
    def __init__(self, name):
        self.name = name
        Person.__count = Person.__count + 1
         
print Person.how_many()
p = Person('Bob')
print Person.how_many()
```

**定义实例方法**：
python中方法也是属性，调用方法和属性一样。在class中定义的实例方法第一个参数 self 是实例本身。同上一个例子，去掉 @classmethod 标记，传入表示实例的参数 self, how_many(self) 则变成实例方法。调用方式：p.how_many()。
```py
# coding = utf-8
class Person(object):

  def __init__(self, name, score):
    self._score = score

  # 实例方法， 第一个参数 self 是实例
  def get_grade(self):
    if self._score >= 80:
      return 'A-优秀'
    if self._score >= 60:
      return 'B-及格'
    if self._score < 60:
      return 'C-不及格'

p = Person('Bob', 90)

print p1.get_grade()
# 由于例子中有中文，所以需要在代码开始定义 # coding=utf-8
```

给一个实例动态添加方法，用 types.MethodType() 把一个函数变为一个方法（types需要先import）：
```py
import types
def fn_get_grade(self):
    if self.score >= 80:
        return 'A'
    if self.score >= 60:
        return 'B'
    return 'C'
 
class Person(object):
    def __init__(self, name, score):
        self.name = name
        self.score = score
 
p = Person('Bob', 90)
p.get_grade = types.MethodType(fn_get_grade, p, Person)
```

#### 类的继承
```py
# Person 类继承自 object
class Person(object):
  def __init__(self, name, gender):
    self.name = name
    self.gender = gender

# Student 类继承自 Person
class Student(Person):
  def __init__(self, name, gender, score): 
    # 继承需要调用 super(class, self).__init__(args) 初始化父类
    super(Student, self).__init__(name, gender)
    self.score = score
```

#### 获取对象信息
**isinstance()** 可以判断一个变量的类型，既可以用在Python内置的数据类型如str、list、dict，也可以用在我们自定义的类，它们本质上都是数据类型。
```py
isinstance(p, Person)     # （实例，原型）返回布尔值
```

**type()** 函数获取变量的类型，返回一个 Type 对象：
```py
type(123)
# <type 'int'>
```

**dir()** 函数获取变量的所有属性

**getattr()** 和 **setattr()** 函数获取或者设置对象的属性：
```py
getattr(s, 'name')          # 获取name属性'Bob'
 
setattr(s, 'name', 'Adam')  # 设置新的name属性为 'Adam'
```

#### 定制类--特殊方法
在Python中，函数其实是一个对象，所有的函数都是可调用对象。
**__call__(self, 调用实例时传入的参数)**：将一个类实例变成一个可调用对象

**__slots__**：限制当前类所能拥有的属性，如果不需要添加任意动态的属性，使用__slots__也能节省内存
```py
class Student(object):
  # 只能有 'name', 'gender', 'score' 三个属性
  __slots__ = ('name', 'gender', 'score')
  def __init__(self, name, gender, score):
    self.name = name
    self.gender = gender
    self.score = score
```

**__str__()**：把一个类的实例变成 str。用于显示给用户（print 出来的：print p）
**__repr__()**：用于显示给开发人员（直接调用的：p）
```py
# 让 __repr__ 和 __str__ 一样
__repr__ = __str__
```

整理自[Python进阶](https://www.imooc.com/learn/317)