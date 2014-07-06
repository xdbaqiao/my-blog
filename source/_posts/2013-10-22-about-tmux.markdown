---
layout: post
title: "tmux"
date: 2013-10-22 09:17
comments: true
categories: linux
---

一直以来都是在screen上运行远程程序，偶然发现tmux也有这样的功能，我一直把tmux当分屏工具用，看来真是浪费了额。    

对比一下：       

    tmux attach -t baqiao    ===   screen -D -r baqiao
    
    tmux new -s baqiao       ===   screen -S baqiao
    
    tmux ls               ===   screen -list
    
    Ctrl + b, d           ===   Ctrl + a + d
