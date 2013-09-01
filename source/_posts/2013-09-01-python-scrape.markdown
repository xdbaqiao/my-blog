---
layout: post
title: "python爬虫程序(多线程，get，post，代理，日志)"
date: 2013-09-01 15:58
comments: true
categories: python
---
```python
#!/usr/bin/env python2
#coding: utf-8

import re
import string
import urllib
import urllib2
import logging
import cookielib
from colloctions import deque

def set_log():
    FMT = '%(asctime)s-%(levelname)s\t%(message)s'
    logging.basicConfig(format=FMT, level=logging.INFO, filename='downloading.log', filemode='a')

class download:
    def __init__(self, first_url, proxy=None):
        self.user_agent = 'Mozilla/5.0 (Windows NT 6.1; WOW64; rv:5.0) Gecko/20100101 Firefox/5.0'
        self.headers = {'User-Agent': self.user_agent, 'Accept-encoding':'gzip, deflate', 'Referer': first_url}
        self.opener = urllib2.build_opener()
        self.proxy = proxy

    def add_proxy(self, url, proxy):
        if proxy:
            if url.startswith('https'):
                self.opener.add_handler(urllib2.ProxyHandler({'https': proxy}))
            else:
                self.opener.add_handler(urllib2.ProxyHandler({'http': proxy}))
    
    def get(self, url):
        self.add_proxy(url, self.proxy)
        request = urllib2.Request(url)
        response = self.opener.open(request)
        html = response.read()
        return html

    def post(self, url, data):
        self.add_proxy(url, self.proxy)
        if isinstance(data, dict):
            data = urllib.urlencode(data)
        request = urllib2.Request(url, data, self.headers)
        response = self.opener.open(request)
        html = response.read()
        return html

def fetch():
    url = 'http://medboard.nv.gov/verification/'
    surl = 'http://medboard.nv.gov/verification/Search.aspx'

    name = deque()
    for i in string.lowercase():
        name.append(i)

    def worker():
        D = download(url)
        D.opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cookielib.MozillaCookieJar()))
        while True:
            try:
                letter = name.popleft()
            except IndexError:
                break
            html = D.get(url=url)

            post_data = {}
            m = re.compile(r'__VIEWSTATE"\svalue="([^"]+)', re.DOTALL|re.IGNORECASE).search(html)
            post_data['__VIEWSTATE'] = m.groups()[0] if m else ''
            m = re.compile(r'__EVENTVALIDATION"\svalue="([^"]+)', re.DOTALL|re.IGNORECASE).search(html)
            post_data['__EVENTVALIDATION'] = m.groups()[0] if m else ''
            post_data['t_web_lookup__license_type_name'] = 'Medical Doctor'
            post_data['t_web_lookup__first_name'] = letter
            post_data['sch_button'] = 'Search'
            html = D.post(url=surl, data=post_data)
            logging.info(html)

    muilt_thread(worker, 30)

def muilt_thread(target, num_threads):
    threads = [thread.Thread(target=target) for i in range(num_threads)]
    for thread in threads:
        thread.start()
    if wait:
        for thread in threads:
            thread.join()

if __name__ == '__main__':
    set_log()
    fetch()
```

