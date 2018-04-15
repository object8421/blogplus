---
layout:     post
title:      "Spring WebFlow 远程代码执行漏洞"
categories: 04_security_notes
subtitle:   ""
date:       2018-04-15
author:     "object8421"
header-img: "img/home-bg.jpg"
tags:
    - 红日安全作业
---

红日安全作业3 --- Spring WebFlow 远程代码执行漏洞
===

本次作业我的目标是尝试分析漏洞，所以预先学习了[Spring Web Flow 2.0 入门](https://www.ibm.com/developerworks/cn/education/java/j-spring-webflow/index.html) 和 [CVE-2017-4971：Spring WebFlow 远程代码执行漏洞分析](https://www.anquanke.com/post/id/86244)知识，由于执行过程中拉取漏洞的镜像出现网络问题和该漏洞的复现需要学习burpsuite工具，所以导致耗时较多，未完成分析。这里先记录下环境的安装和工具的安装，以及漏洞复现的过程, 漏洞的原理后续再分析。

burpsuite工具的安装和配置
---
1. 先下载[burpsuite](https://portswigger.net/burp/communitydownload)工具

2. burpsuite安装. 执行/burpsuite_community_linux_v1_7_33.sh

3. 进入firefox的preferences > General > network proxy, 设置firefox浏览器的http代理为127.0.0.1:8088

4. 执行java -jar /usr/local/BurpSuiteCommunity/burp/burpsuite_community_1.7.33-9.jar，启动burpsuite

5. 进入burpsuite的proxy > option选项卡，设置proxy listeners的interface为127.0.0.1:8088,running打钩

6. 切换到proxy > intercept选项卡，设置 intercept is on。
  
安装docker加速器
---

   参考[Docker加速器](https://vulhub.org/#/docs/docker-accelerator/)文档。


Spring WebFlow漏洞复现
---

1. 导入s_springwebflow_1镜像, 启动环境

    red@ubuntu:~$ cd ~/workspace/vulhub/spring/CVE-2017-4971
    red@ubuntu:~/workspace/vulhub/spring/CVE-2017-4971$ docker-compose up -d


2. 访问 http://127.0.0.1:8080/，测试服务是否启动成功 

3. 使用测试账号（keith：melbourne）登录目标站 http://127.0.0.1:8080/login

4. 找一个酒店(http://127.0.0.1:8080/hotels/1),点 Book Hotel

5. 填写订单详情，后点击 Proceed 生成订单

6. 用 BurpSuite 捕获点击下图 Confirm 后的数据包

7. 在POST数据包中加入这段数据：
			
            &_T(java.lang.Runtime).getRuntime().exec("touch /tmp/success")

8. 进入docker中，查看tmp目录下的success文件。
<img src="http://123.207.1.187:4000/img/04_security_notes/spring_flow/spring_flow_1.png" />

9. 查看web页面
<img src="http://123.207.1.187:4000/img/04_security_notes/spring_flow/spring_flow_2.png" />

