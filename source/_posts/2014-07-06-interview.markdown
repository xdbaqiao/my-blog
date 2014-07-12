---
layout: post
title: "常见的IT类面试题[未完]"
date: 2014-07-06 18:06
comments: true
categories: learning
---
       
    1. 讲述计算机启动过程？            
    2. 什么是MBR，有多大？               
    3. Linux 第一个进程叫什么名字？              
    4. http协议状态码，4XX有什么？              
    5. raid2,3,4用途？              
    6. traceroute的基本原理是什么？              
    7. 翻转字符串              
    8. 字符串转数字              
    9. crontab 同步时间              
    10. mysql nginx apache 的配置文件位置              
    11. C++中的容器              
    12. 解释$0 $$ $# $! $@ $? $*              
    13. python 生成器和修饰器              
    14. iptables              
    15. Django工作原理              
    16. vim 替换1-10行字符
    17. ls -al
    18. mysql, nginx, apache 优化              
    19. 网站加速              
    20. uwsgi 应用服务器              
    21. ip route add default，网络配置相关              
    22. 关于linux的runlevel              
                  
              

**我的解答**          

1\. 讲述计算机启动过程？            
>计算机从BIOS中启动，BIOS一般位于主板的flash中，计算机上电之后，进入BIOS，进行硬件自检，并决定从哪里（可选u盘，硬盘等）启动计算机操作系统，接着BIOS把控制权交给设备，从设备中的前512字节中，读取MBR，即主引导记录，放入内存，里面保存有硬盘分区表等信息。接着进入启动管理器，常见的有grub，lilo等，交由用户决定启动哪个操作系统。           

2\. 什么是MBR，有多大？               
>Master Boot Record，MBR主引导记录，有512字节，前446个字节是主引导程序，将系统控制转交给用户制定的分区表中的系统;然后是四个分区表，用于记录磁盘分区状况，每个分区表16个字节;最后是结束标志AA55         

3\. Linux 第一个进程叫什么名字？              
>init(1)，是进程号为1的进程，用于开机初始化       

4\. http协议状态码，4XX有什么？              
>400：请求报文错误    401：未认证    403：Forbidden        
>404：not found等          

5\. raid2,3,4用途？              
>raid2 现在用的很少，因为他对大数据有很高读写性能，但少量读写性能不好。它是为超级计算机开发的磁盘阵列，带海明码，在2的n次幂磁盘中设立校验盘。基本思想是在IO到来之后，控制器将数据按位分割，并顺序将每一位数据放置在每一个磁盘中，这样任何数据都能并行读取，提高性能。但这样也不能实现并发IO，因为硬盘被占用。             
>raid3现在用的也很少，因为其校验盘很容易成为性能瓶颈，其将数据分成多“块”，放在N+1个磁盘上，N个磁盘存放数据，1个磁盘存放校验数据     
>raid4也用的很少，其校验也是放置在一个磁盘上，这个磁盘很容易成为性能瓶颈。    
>所以raid2,3,4基本很少见，其主要运用场景是大量数据读写，比如视频，影音等。          

6\. traceroute的基本原理是什么？              
>traceroute主要用于追踪数据包的路由路径，采用icmp协议，数据包经过一个路由器，TTL生存时间将会减一，直到数据包到达目的地或者TTL被置为零。         

7\. 翻转字符串              
8\. 字符串转数字              
9\. crontab 同步时间              
> 10 10-16/2 * 7 *  /usr/bin/ntpd -s 133.100.9.2         
> 7月十点十分到下午四点十分，每隔两个小时，进行时间同步         
> 有面试官问，crontab同步失败，是什么原因，很有可能是环境变量设置有误，找不到相关命令         

10\. mysql nginx apache 的配置文件位置              
> 我完全没有料到居然这也能考，不过我确实没有在意，因为平时凭肌肉记忆和自动补全就能找到相关文件，但笔试起来，我却是如此水。。。。         
> 自动补全是如此关键，另外我觉得这真没什么可考的。。。        
> 以archlinux为例，我在自动补全的帮助下，轻轻松松找到。。。           
> /etc/mysql/my.cnf   /etc/nginx/nginx.conf    /etc/apache2/httpd.conf         

11\. C++中的容器              
> 包括顺序容器和关联容器，顺序容器有vector,list,queue,deque,stack等，其属性和操作包括，size,pop,push,top,insert,erase等，关联容器有map，set等，其属性和操作包括insert,find,count,erase等。       

12\. $0 $$ $# $! $@ $? $*              
> $0 程序名       
> $$ 当前进程进程号       
> $# 参数个数             
> $! 后台运行最后一个进行进程号             
> $@ 输入参数组合，形如("$1", "$2", "$3")          
> $? 前一命令返回码                   
> $* 输入参数组合，形如("$1 $2 $3")            

13\. python 生成器和修饰器              
> python的生成器就是yield，表示带yield的函数，其实很简单，类似一个list，不同的是，生成器在使用之后会消耗        
>
>

14\. iptables              
> iptables有三表五链，分别是nat、filter、mangle表，prerouting、postrouting、output、input、forward链        
> nat表用于DNAT与SNAT，包含两个链，prerouting、postrouting         
> filter表是默认表，包含input、output、forward链        
> mangle表包含postrouting、prerouting和forward链         
> 可以分三种情况理解防火墙。第一种是进入的数据，先通过prerouting链，进行地址变换，然后进行路由判断，如果目的地址是本机，则通过input链处理;如果目的地不是本机，则是第二种情况，转发的数据，则先通过forward链处理，判断是否拦截或转发，然后交给postrouting链进行处理;第三种情况是出站的数据，首先通过output链，然后进行路由判断，最后通过postrouting，进行SNAT。

15\. Django工作原理              
> 

16\. vim 替换1-10行字符      
> :1,10s/www/wwww/g              

17\. ls -al      
> 命令第二列表示，子文件夹数量或者文件硬链接数量              

18\. mysql, nginx, apache 优化              
19\. 网站加速              
> 1. 前端优化，静态页面加载速度快
> 2. CDN分发，负载均衡
> 3. nginx, squid代理作cache加速

20\. uwsgi 应用服务器              

21\. ip route add default，网络配置相关              
> archlinux 使用最新的网络管理工具包 iproute2，替代之前的nettools          
>
> ip raute add default 192.168.0.0/24 via 192.168.0.1 dev eth0

22\. 关于linux的runlevel              
> runlevel 0 是 关机        
> runlevel 1 是 单用户模式         
> runlevel 3 是 多用户模式         
> runlevel 5 是 图形界面           
> runlevel 6 是 重启               
> runlevel s 是 单用户模式         
> 较旧的发行版 其位置为/etc/inittab            

23\. load 的含义       
> 分别表示1分钟、5分钟和15分钟的负载情况
> 这个负载情况指的是最近时间内，在CPU中，正在运行与准备运行的平均进程数量
