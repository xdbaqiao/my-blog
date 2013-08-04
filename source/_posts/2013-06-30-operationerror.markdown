---
layout: post
title: "关于OperationalError"
date: 2013-02-03 19:27
comments: true
categories: mysql
---

往mysql插入数据时，总是提示      
>‘OperationalError: (1118, ‘Row size too large. The maximum row size for the used table type, not counting BLOBs, is 8126. You have to change some columns to TEXT or BLOBs’)

起初以为是表设计的问题，将超过255的vachar更改成text后，还是报错。但是在另一台计算机上插入数据时，又没有问题。所以，我想可能是我本地mysql配置的问题。网上搜索一下，发现果真是配置的问题。          
在mysql配置文件my.cnf中修改或者添加：        
    [mysqld]
    max_allowed_packet = 16M
    innodb_file_per_table=1
    innodb_file_format='Barracuda'
注意mysql的配置文件一定要加上`innodb_file_format=’Barracuda` ’, 不然会报错           
     
>requires innodb_file_format > Antelope
       
在设计表时，添加 `ROW_FORMAT=COMPRESSED`      
       
另外，插入数据时，某些字符插入时会产生警告       
    
>Warning: Incorrect string value: ‘\xD0\x91\xD0\xB8\xD1\x82…’ for column…
       
解决方法是修改表列类型          
    
    ALTER TABLE table_name MODIFY COLUMN column_name TEXT CHARACTER SET utf8 COLLATE utf8_general_ci NULL
