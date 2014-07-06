---
layout: post
title: "我的django博客"
date: 2014-06-08 23:06
comments: true
categories: python
---
用了两年Python，写了两年Python爬虫，水平一直停留在脚本层级。最近受工作的刺激，梳理了一下思绪，发现自己前端后端都有很大的发展潜力。所以先拿Django开刀，练练手。          

Linux和Python已经很熟悉了，所以代码写起来也很顺手。下面记录一下本博客的搭建过程。          

原来的博客是Octopress，基本上是嗟来之食，拿来就用，底层原理完全没有理会。现在用Django，感觉自然不一样，当然也学习到很多。          

我直接在自己的vps上操作，发行版是Debian。          
首先，安装Django         
>pip install django          

安装完成验证一下     
>python –c “import djang           

如果没有输出，则表示安装成功了。       
运行             
>django-admin.py startproject myblog          

创建自己的第一个工程，发现在当前目录下，多了文件夹myblog，其中有manage.py，内置众多管理命令。而myblog/myblog 文件夹内也有urls.py, settings.py等关键文件。          

执行            
>python manage.py runserver 127.0.0.1:8080             

就可以在本机浏览器访问了。当然也可以修改ip地址和端口，使远程访问，我就是这样的，因为我直接在vps上操作的。         

创建一个博客应用       
>python manage.py startapp sblog

会在myblog同级目录下新建sblog目录，里面有models.py, test.py, admin.py, views.py等重要文件。          

Django被称为MTV架构，我的理解是，他还是MVC架构，控制器就是整个Django框架，V就是view视图，M就是model进行事物处理，Django多了一个templat模板，模板用于前端展示。          

`M：models.py 进行数据库操作`           
`T：template 用于前端展示`          
`V：views  用于视图逻辑处理`          

另外urls.py用于解析url超链接，进行视图逻辑分发。          

首先我打开了admin，安装完Django之后，默认开启了settings.py 中的全部INSTALLED_APP应用（其中添加sblog应用）以及中间件MIDDLEWARE_CLASSES，其他我都没有修改，数据库使用默认的sqllite3             

执行        
>python manage.py syncdb              

进行数据库同步，创建用户。这样就可以通过127.0.0.1:8080/admin访问管理工具了。             

             
为了使管理工具能管理博客，需要编辑models.py 文件，             

```
from django.db import models
from django.contrib import admin

class BQBlog(models.Model):
    title = models.CharField(max_length=200) 
    content = models.TextField() 
timestamp = models.DateTimeField()
class BQBlogAdmin(admin.ModelAdmin)：
list_display = ('title','timestamp')

admin.site.register(BQBlog, BQBlogAdmin) //运行python manage.py syncdb前需注释
```
              
使保存在数据库中的博客有标题，内容和时间戳                
这样就可以在Django管理工具中进行博客管理了。                

进入sblog目录，新建文件夹templates进行模板管理                

新建文件chapter.html                

    {% for post in posts %} 
    <h2>{{ post.title }}</h2> 
    <p>{{ post.timestamp }}</p> 
    <p>{{ post. content }}</p> 
    {% endfor%}

在views.py 中添加视图逻辑                

```
from django.shortcuts import render_to_response 
from sblog.models import BQBlog

def chapter(request):
    posts = BQBlog.objects.all() 
    return render_to_response("chapter.html", {'posts':posts}) 
```

myblog文件夹urls.py 中添加url(r'^$', include('blog.urls')),                
sblog文件夹新建urls.py                

```
from django.conf.urls import * 
from sblog.views import chapter

urlpatterns = patterns('', url(r'^$', chapter), )   
```
                
                
这样一个基本的、最简单的Django博客就出来了！                

最后需要对其进行简单润色。                
见[`博客http://django-baqiao.tk`](http://django-baqiao.tk/)                

太丑了！以后有时间再修改！                
