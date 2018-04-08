---
layout:     post
title:      "Spring Data REST PATCH请求代码执行漏洞"
categories: 04_security_notes
subtitle:   ""
date:       2018-04-08
author:     "object8421"
header-img: "img/home-bg.jpg"
tags:
    - 红日安全作业
---

红日安全作业2 --- Spring Data REST PATCH请求代码执行漏洞
===

参考漏洞http://vulapps.evalbug.com/s_spring_1/ ，以下简单记录下复现的过程, 漏洞的原理后续再分析。

1. 导入s_spring_1镜像
    
		  red@ubuntu:~$ sudo docker pull medicean/vulapps:s_spring_1
    
2. 启动环境

		  red@ubuntu:~$ sudo docker run -d -p 8080:8080 medicean/vulapps:s_spring_1
    
3. 访问 http://127.0.0.1:8080/，测试服务是否启动成功 

4. 执行 PoC 

   <img src="http://123.207.1.187:4000/img/04_security_notes/spring_data/post_person.png" />
5. 进入容器，发现成功创建文件

   <img src="http://123.207.1.187:4000/img/04_security_notes/spring_data/show_vlun.png" />
