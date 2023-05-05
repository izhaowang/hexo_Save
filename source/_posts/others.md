---
title: others
tags:
  - js
  - web前端
categories:
  - web前端
date: 2023-04-29 23:42:57
---
# 查看进程
## win 
1. 首先win+r 输入cmd
2. netstat -ano 查看进程
3. 找到进程对应的pid
4. taskill -pid xxxx
## mac
1. sudo lsof -i tcp:port（port是端口号）
2. sudo kill -9 PID

## linux
1.  netstat -anp | grep 8080