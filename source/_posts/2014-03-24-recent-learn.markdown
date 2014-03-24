---
layout: post
title: "学习"
date: 2014-03-24 19:45
comments: true
categories: learning
---

总结一下最近学习的东西，有点乱。。。       
         
1\. keep-alive              
-----------
http基于tcp协议，是一种请求响应机制的协议。         
所以，它的请求过程是：          
建立连接（三次握手），请求，响应，断开连接（四次挥手）          
而如果包含keep-alive属性，则他的请求过程将节省建立连接的过程：            
建立连接，请求，响应，请求，响应，请求，响应....断开连接       
           
2\. 数据库性能优化           
-----------
使用MyISAM存储引擎，加上唯一索引，设置主键           
（其实可以考虑用innoDB存储引擎，便于设置外键，而且是事务存储引擎）           
           
存储引擎：MyISAM, innoDB, Falcon, Memory, ARCHIVE, CSV, EXAMPLE, MERGE等           
索引类型：普通索引，唯一索引，Fulltext索引，Hash索引等           
数据类型的选择，int? tinyint? mediumint? float?double?           
blob和text 数据列里面的值在长度方面往往差距巨大，所以对他们的删除和修改操作容易产生大量的碎片。           
           
对数据库的某个字段进行检索，加上唯一索引与不加，速度上至少相差一个数量级！          
            
3\. 传输文件           
------------
高效的文件传输工具rsync           
`rsync -avzSHP --exclude "*.LJPEG" * root@192.168.0.101:/mnt/file_dir`

4\. HTTP协议相关
------------
Internet 最基本的通讯单元是数据包，http最基本的单元是消息，http协议有四个重要术语，消息，实体，资源，用户代理。          
          
消息
        1.请求消息：包括请求行，常规标头，请求标头（标头是自由格式的ASCII字符串）          
        2.响应消息：包括响应行，常规标头，响应标头          
实体，表示封装在消息中的一个资源，实体由标头和可选的主体组成。          
用户代理，表示客户机，可以是浏览器，爬虫等。用户代理信息会作为请求的一部分，在User-Agent标头中发送。          
          
标头：自由格式的ASCII字符串          
           常规标头：Date，Pragma(伪指令，发送给消息接收方，唯一的伪指令pragma：no-cache)
           请求标头：Authorization，From，If-Modified-Since，Referer，User-Agent
           响应标头：Location（将请求重定向可找到资源的地方），Server，WWW-Authorization
           实体标头：Allow，Content-Type，Content-Coding，Content-Length，Expires，Last-Modified

信息类：1xx                        成功类： 2xx             
重定向类： 3xx                    客户机错误类：4xx              
服务器错误类：  5xx               

5\.装饰器的用途          
------------
•  Access control           
•  Cleanup of temporary objects           
•  Error handling           
•  Caching           
•  Logging           

    >>> def mom(func):
    ...     cache = {}
    ...     def wrapper(*args):
    ...         if args in cache:
    ...             return cache[args]
    ...         print 'Calling %s()' % func.__name__
    ...         res = func(*args)
    ...         cache[args] = res
    ...         return res
    ...     return wrapper
    >>> @mom
    ... def mum(x,y):
    ...     return x*y
    ... 
    >>> mum(6,7)
    Calling mum()
    42
    >>> mum(6,7)
    42 

6\.生成器 yield
--------------
yield 就像return， 但不会终止程序，他将值推出函数体，当生成器被call时，这个值被消耗。       
    >>> def test():
    ...     for i in range(2):
    ...         yield i
    >>> t=test()
    >>> t
    <generator object test at 0x7f5609846910>
    >>> list(t)
    [0, 1, 2]
    >>> list(t)
    []

7\. @property        
-------
使得类中的方法可以像类属性一样访问（build_in decorator），但不能写，如果要写的话，如下例用@name.setter修饰符，还可以用@name.deleter 删除方法属性        
    >>> class person:
    ...     def __init__(self, first_name, last_name):
    ...         self.f = first_name
    ...         self.l = last_name
    ...     @property
    ...     def name(self):
    ...         return '%s %s' % (self.f, self.l)
    ...     @name.setter   #修饰之后，返回value值
    ...     def name(self, value):
    ...         return '%s %s' % (self.f, self.l) #所以这句话没意义了
    ... 
    >>> p = person('hello', 'world')
    >>> p.name    #不是p.name()
    'hello world’


