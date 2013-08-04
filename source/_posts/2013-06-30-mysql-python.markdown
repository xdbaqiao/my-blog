---
layout: post
title: "Python和mysql"
date: 2013-01-13 19:25
comments: true
categories: python
---

用python操作mysql很方便，主要运用MySQLdb库。Arch可以直接敲以下命令安装：           
    
>pacman -S mysql-python            

我的任务很简单，在已经存在的数据库中插入，查询和更新数据。代码也很简单，首先创建与数据库的链接：         
        
>self.conn = MySQLdb.connect(host=mysql_host, port=mysql_port, user=mysql_user, passwd=mysql_password, charset='utf8', use_unicode=True)       

然后获取游标：        

>self.cursor = self.conn.cursor()     

选择数据库：       
   
>conn.select_db(mysql_db)         

这样初始化就完成了，然后就可以执行SQL语句，进行数据库操作了。           
```python
class MySQLHandler:
    def __init__(self, mysql_host, mysql_user, mysql_password, mysql_db, mysql_port):
        try:
            self.conn = MySQLdb.connect(host=mysql_host, port=mysql_port, user=mysql_user, passwd=mysql_password, charset='utf8', use_unicode=True)
            self.cursor = self.conn.cursor()
            self.conn.select_db(mysql_db)
        except Exception, e:
            self.cursor = None
    # 插入
    def add_row(self, table, bag):
        if self.cursor:
            sql = 'INSERT INTO %s (%s) VALUES(%s);' % (table, ','.join(bag.keys()), ','.join(['%s']*len(bag)))
            self.cursor.execute(sql, tuple(bag.values()))
            self.conn.commit()
    # 查询
    def select_table(self, select_id, table, params, params_value):
        if self.cursor:
            sql = 'SELECT %s FROM %s WHERE %s = ' % (select_id, table, params) + '%s;'
            self.cursor.execute(sql, (params_value, ))
            r = self.cursor.fetchone()
            if r:
                return r[0]
            else:
                return ''
```
