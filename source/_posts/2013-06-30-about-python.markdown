---
layout: post
title: "Python 装饰器、魔术方法和元类"
date: 2013-06-30 19:41
comments: true
categories: python
---

装饰器
------------
         
装饰器从字面上看，是一个起到装饰作用的工具，而这个工具其实还是函数，起到装饰函数的作用。也就是说，装饰器是一个用来装饰函数的函数，实现对函数的修改。来看一个例子。      

``` python
def br(fn):
    def wrapped():
        return '<br>' + fn()+ '</br>'
    return wrapped

def li(fn):
    def wrapped():
        return '<li>' + fn()+ '</li>'
    return wrapped

@br
@li
def hello():
    return 'Hello'
print hello()
```    
        
输出的结果是：           
`<br><li>Hello</li></br>`
      
@称之为语法糖，我们把
```python
@li
@br
def hello():
    return 'Hello'
print hello()
```       
展开，可以等价为
```python
def hello():
    return 'Hello'
print li(br(hello))()
```     
需要注意的是，装饰器的顺序很重要，装饰器必须在引用前定义，例子中的li和br如果在hello函数之后定义，运行的时候程序将会报错。        
              
装饰器说明了一个问题，在Python中，函数也是对象，所以函数可以被赋给变量，可以作为参数进行参数传递，可以返回函数，也可以在函数中定义函数。
           
其实在python中，一切皆对象，类是对象，类中的方法也是对象，类实例的行为也可以表现的像函数一样。可以将类赋值给一个变量，类可以复制，可以增加属性，也可以作为参数传递。
         
```python
class student:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        print 'Class initial.'
        print name, age
    
    def __call__(self, sexy='Male'):
        print 'Function is called.'
        print 'Name=%s, Age=%s, Sexy=%s' % (self.name, self.age, sexy)

xiaoming = student('xiaoming', '8')
xiaoming()
```     
          
输出：        
    Class initial.          
    xiaoming 8            
    Function is called.               
    Name=xiaoming, Age=8, Sexy=Male              
       
可以看到xiaoming这个类实例表现的像函数一样，调用xiaoming()的结果是执行了\_\_call\_\_方法，这个前后都带双下划线的方法称为魔术方法。通过魔术方法\_\_call\_\_，类实例表现出了函数的特征。
        
装饰器有很多用途，包括        
          
* 访问控制        
* 清除临时对象         
* 错误处理          
* 缓存         
* 日志           
     
来看一个缓存的例子：
    >>> def cache(func):
    ...     c = {}
    ...     def wrapper(*args):
    ...         if args in c:
    ...             return c[args]
    ...         print 'Calling %s()' % func.__name__
    ...         res = func(*args)
    ...         c[args] = res
    ...         return res
    ...     return wrapper
    >>> @cache
    ... def mum(x,y):
    ...     return x*y
    ... 
    >>> mum(6,7)
    Calling mum()
    42
    >>> mum(6,7)
    42

       
魔术方法
-----------
       
魔术方法很多，比如\_\_enter\_\_，\_\_exit\_\_。我们知道with函数的用法：      
```python  
with open('test.txt') as f:
    print 'Open file test.txt'
```   
with函数会自动执行\_\_enter\_\_和\_\_exit\_\_方法，在with开始执行的时候，就会调用\_\_enter\_\_方法，而在with函数结束的时候调用\_\_exit\_\_方法，open的\_\_exit\_\_会自动关闭文件，省去了显式调用close方法的步骤。      
         
举个例子：
       
```python
class Vipsl：
    def __init__(self):
        print 'Class initial.'
    
    def __enter__(self):
        print 'Enter Vipsl'

    def __exit__(self, val1, val2, val3):
        print 'Exit.'

with Vipsl() as v:
    print 'processing...'
```     

输出结果是：       
    Class initial.      
    Enter Vipsl      
    processing...    
    Exit.         
      
我们可以看到\_\_exit\_\_最后执行。       
         
上例有最基本的魔法函数\_\_init\_\_，他在创建对象时调用，可以视为C++中的构造函数，而\_\_del\_\_视为析构器。其实\_\_init\_\_并不是类实例化时所调用的第一个方法，第一个调用的方法是\_\_new\_\_。

元类
--------------     

说到\_\_new\_\_，就涉及到元类的概念了。       
元类就是类的类。       
我们知道类可以使用class关键字来定义，这时候python解释器会自动创建这个对象，但类也是对象，像其他对象一样，为了生成类，python也会提供一个手动处理的方法，这个方法就是type。type创建类的格式是：         
`type(类名，父类元组，属性字典)`

举个例子：
```python
class student(person)：
    SmallThan18 = True
```
那么用type创建就是：

`student = type(student, (person), {'SmallThan18':True})`     

可以看到，type创建了一个类。其实type就是元类，而且是python中创建所有类的元类。
str是类，int也是类，从这些类中可以创建出字符串对象，整型对象，而str类和int类的元类也是type。

我们可以从\_\_class\_\_属性中看出端倪。       
     
    >>> a = 'hello world'
    >>> a.__class__
    <type 'str'>
    >>> a.__class__.__class__
    <type 'type'>
    
字符串a是从str类而来，str类来自元类type，那么a.\_\_class\_\_.\_\_class\_\_.\_\_class\_\_是什么呢？还是type。

type是元类，当然我们可以创建自己的元类，这就是\_\_metaclass\_\_。如果指定了\_\_metaclass\_\_，那么，这个类将不会由type创建，而是由指定的\_\_mataclass\_\_创建，那么\_\_metaclass\_\_是什么呢？其实就是可以创建类的东西，可以是一个类（来自type），也可以是type。

举例子：
```python
class mc(type):
    def __init__(cls, name, base, dict):
        if not hasattr(cls, 'nlist'):
            cls.nlist = []
        else:
            cls.nlist.append(name)

class person:
    __metaclass__ = mc

class student(person):
    SmallThan18 = True

class gilrs(person):
    Sexy = 'Female'

print student.nlist
```

输出：     
`['student', 'gilrs']`

可以看到mc其实是type的子类，任意person的子类名，都会放到nlist里面。定义type的时候注意，要符合type的参数定义格式。就是：
type(类名，父类元组，属性字典)         
         
回到\_\_new\_\_属性，记不记得前面讲的\_\_call\_\_，那么type是不是可以类实例函数化，所以       
`type(类名，父类元组，属性字典)`  
等价为       
`type(类名).__call__(类名，父类元组，属性字典)`

而在\_\_call\_\_的实现中，是先尝试用\_\_new\_\_来创建类实例的，只有\_\_new\_\_返回了类的实例，\_\_init\_\_才会执行。
可以这样表示：
```python
def __call__(cls, *args, **kwargs):
    result = cls.__new__(cls, *args, **kwargs)
    if isinstance(result, cls):
        type(result).__init__(result,*args,**kwargs)
    return result
```

参考资料：     
[http://blog.danmarner.com/me/entry/python-new-metaclass-init-plugin-system/](http://blog.danmarner.com/me/entry/python-new-metaclass-init-plugin-system/)         
[http://blog.jobbole.com/21351/](http://blog.jobbole.com/21351/)
