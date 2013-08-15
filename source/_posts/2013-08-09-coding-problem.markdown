---
layout: post
title: "python 的编码问题"
date: 2013-08-09 20:36
comments: true
categories: python
---
编码涉及到很多问题，本文简单讲一讲    
              
写python脚本的时候，头两行的常见格式是：          
    #!/usr/bin/env python                  
    #coding:utf-8               
第一行的意义是在Linux下，当脚本是可执行文件是，以"./\*.py"执行程序时需要调用的命令，如果以命令'python  \*.py'执行程序，则第一行是没有意义的                
第二行涉及到python编码问题，它指定脚本内容的编码方式。这一行的写法很灵活，但必须包含三项内容，即"#", "coding" 和编码     
所以，也可以写作`# -*- coding: UTF-8 -*-`
                 
__基本知识__                 
在讲python编码之前，需要了解一些基本知识：           
1\. python有两种字符串类型，str和unicode，例如           
    >>> a = u'你好'          
    >>> a.__class__             
    <type 'unicode'>           
    >>> a = '你好'           
    >>> a.__class__      
    <type 'str'>          
2\. python里         
encode是指将unicode 转换成  其他编码           
    encode： unicode  -->  other            
decode 是将其他编码  转换成 unicode            
    decode ： other   -->  unicode               
3\. 注意区分字符集和字符编码。         
    字符集是一个系统支持的所有抽象字符的集合，包括各种文字和符号。         
    字符编码是一套法则，使用该法则，将一个字符集与另一个集合配对。       
    字符集包括ASCII字符集，GB2312字符集，BIG5字符集，UNICODE字符集等        
4\. UNICODE是字符集，UTF-8,UTF-16,UTF-32是三种字符编码方案。          
               
               
__python的编码问题__            
               
python在安装的时候，默认的编码是ASCII码，当程序中出现非ASCII码时，程序会报错.            
在终端里，运行           
    >>> import sys
    >>> sys.getdefaultencoding()
    'ascii'
可以看到，默认编码是ASCII码，看看"你好"是怎么编码的            
    >>> a = '你好'
    >>> a
    '\xe4\xbd\xa0\xe5\xa5\xbd'
这是"你好"的__utf-8__编码，如果对a解码呢，解码就是转换成unicode            
    >>> a.decode('utf8')
    u'\u4f60\u597d'
    >>> a = u'你好'
    >>> a
    u'\u4f60\u597d'
可以看到，a被解码成unicode了，那对a编码呢，编码的定义是将unicode转换成其他编码，看看python会怎么处理            
    >>> a = '你好'
    >>> a.encode('utf8')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    UnicodeDecodeError: 'ascii' codec can't decode byte 0xe4 in position 0: ordinal not in range(128)
__报错了__！为什么会报错呢？这是因为对a进行encode操作，实际的代码是             
    unicode(a).encode('utf-8')
会先将a解码成unicode，但没有指定解码方式，会使用系统默认的ASCII码，进行解码，__解码__出错。           
而对unicode类型的字符串进行解码也是一样的道理，会先将unicode编码成ASCII码，__编码__出错。           
    >>> a.decode('utf8')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "/usr/lib/python2.7/encodings/utf_8.py", line 16, in decode
        return codecs.utf_8_decode(input, errors, True)
    UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
所以，__最好对unicode对象只做encode操作，对str对象只做decode操作__              
前面都没有用print函数打印，这时因为print会对编码进行自动处理
    >>> a = '你好'
    >>> print a.decode('utf8')
    你好
因为print函数对编码做了自动处理，所以显示汉字，这个操作系统环境变量设置有关。           
    >>> a.decode('utf-8').encode('utf-8')
    '\xe4\xbd\xa0\xe5\xa5\xbd'
    >>> print a.decode('utf8').encode('utf-8')
    你好
系统默认编码是ASCII码，那怎么修改系统默认编码呢：            
    >>> import sys
    >>> reload(sys)
    >>> sys.setdefaultencoding('utf-8')
这样系统默认编码就是utf-8了。             
    >>> sys.getdefaultencoding()
    'utf-8'
              
而在脚本文件里，如果没有`#coding:utf-8`，会发生什么情况呢？            
```python
#!/usr/bin/env python

if __name__ == '__main__':
    print '你好'
```
运行程序，会报错：               
`SyntaxError: Non-ASCII character '\xe4' in file 1.py on line 4, but no encoding declared;`                
第四行不是ASCII码，出错，由打印的错误信息也可以看出系统默认编码是ASCII码         
         
如果加上`#coding:utf-8`，则会默认将非ASCII码，编码成utf-8             
               
OK，今天的研究就到这，以后有空再深入研究
