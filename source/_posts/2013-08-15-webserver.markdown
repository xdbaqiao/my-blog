---
layout: post
title: "python与gnuplot"
date: 2013-08-15 15:53
comments: true
categories: python
---
             
写了一个测试web服务器性能的简单脚本，运用了apache中的测试工具ab，代码如下：          
额，这不是什么机密吧，只是一个框架，以后可以加cache机制，加config或者argv，领导不要抓我。
```python
#!/usr/bin/python
#coding:utf-8

import re
import commands
import Gnuplot

class nginxTest:
    def __init__(self, num):
        self.request_finish_time = []
        self.command = 'ab -n %s -c 10 -k http://localhost/' # 把localhost 换成www.google.com，让暴风雨来的猛烈些吧
        self.num = num

    def start(self):
        for i_num, i in enumerate(self.num): 
            self.getOutput(i)
            self.parseOutput()
            print 'Total %s, finish %s.' % (str(len(self.num)), str(i_num+1))

    def getOutput(self, num):
        (status, self.output) = commands.getstatusoutput(self.command % num)

    def parseOutput(self):
        m = re.compile('Time taken for tests:\s+([\.\d]+)').search(self.output)
        if m:
            rft = m.groups()[0]
            self.request_finish_time.append(rft)

class gnuplot:
    def __init__(self, x, y, title):
        self.x = x
        self.y = y
        self.title = title
        self.plot()

    def plot(self):
        self.g = Gnuplot.Gnuplot(debug=1)
        d = Gnuplot.Data(self.x, self.y, with_='lines')
        self.g.title(self.title)
        self.g.xlabel('times')
        self.g.ylabel('%s' % self.title)
        self.g.plot(d)
        self.g.hardcopy('./%s.ps' % self.title, enhanced=1, color=1)

if __name__ == '__main__':
    times = [i for i in range(10, 45)]
    n = nginxTest(num=times)
    n.start()
    gnuplot(x=times, y=n.request_finish_time, title='request finish time')
```

__The result is following__

![request_finish_time](http://imgur.com/okeEBq1.png)
