---
layout: post
title: "Python操作excel"
date: 2013-04-30 19:37
comments: true
categories: python
---
最近一直在研究用python操作windows平台下的excel，对数据进行处理。目前操作excel的python库比较多，有xlwt、xlrd、pyexcelerator、openpyxl、win32com等，功能不一，各有优劣。         
         
xlwt、xlrd、pyexcelerator针对excel2007（不包括）以前版本，生成的excel后缀是xls，而我的需求中需要支持excel2007及以上版本，文件后缀名是xlsx，这时就只有选用openpyxl或者win32com了。        
         
有必要说一下，为什么得选用xlsx版本。因为xls的列数有最大256的限制，xlsx没有，而需要处理的数据列数是超过256的，有700多列。        
         
win32com是python到VBA的接口，python利用win32com直接调用office API，在windows下兼容性好。office下的所有功能，都可以通过这个接口用python实现，但是，它有一个致命的缺点，速度慢。它是所有操作excel的库中最慢的一个。一个win32com的程序，处理120多行，700多列的数据，花了近一个半小时！        
         
最后，选用openpyxl库，这个库文档不是很完整，遇到问题只有google和看源码。写出来运算速度还是比较令人满意的，处理相同的数据，只花了30s！！比win32com效率提升180倍！！        
         
openpyxl可以实现很多excel的功能，我关注的功能包括合并单元格，调整单元格大小，设置单元格背景颜色，设置文本字体、大小、颜色等，都能轻易实现。      
       
代码不在本机，就不上了。         
