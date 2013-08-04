---
layout: post
title: "SVM和topic model工具箱在64位Archlinux上的安装"
date: 2013-01-03 11:09
comments: true
categories: linux
---

最近在用Matlab做实验，遇到不少问题，本机是64位Archlinux，折腾一下，把问题解决了。         
        
1.安装libsvm库          
版本选用libsvm加强版[libsvm-mat-2.89-3[FarutoUltimate3.0Mcode].rar](http://ishare.iask.sina.com.cn/f/20595187.html)，因为程序中有函数scaleForSVM的调用，所以需要包含 `scaleForSVM.m` 程序的版本。下载之后，需要对源文件目录位置做些微调。把文件夹implement里面的文件拷到主文件夹，修改主文件夹的Makefile路径为matlab软件安装目录，并且在matlab中添加路径常量，注意不要把子文件夹添加进去了，然后 `!make` ，成功搞定！

2.安装topictoolbox 
topictoolbox需要重新编译，如果未编译会出现如下错误：
    
    Attempt to execute SCRIPT GibbsSamplerLDA as a function:
    /usr/local/share/matlab/toolbox/topictoolbox/GibbsSamplerLDA.m
    
    Error in exampleLDA1 (line 47)
    [ WP,DP,Z ] = GibbsSamplerLDA( WS , DS , T , N , ALPHA , BETA , SEED , OUTPUT );
    
    Error in run (line 74)
    evalin('caller',[script ';']);

    Error in LDA (line 27)
    run 'exampleLDA1';'
版本选用[最新版本](http://psiexp.ss.uci.edu/research/programs_data/toolbox.htm)，支持64位操作系统。编译时，在Matlab中切换目录到工具箱所在目录，然后执行 `compilescripts` ，编译成功之后，topictoolbox就可以使用了，在某些情况下，需要sudo权限执行编译这个步骤。
