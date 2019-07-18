---
title: IDEA远程调试
date: 2019-07-18 19:46:37
tags: 
    - IDEA
---
你是否遇到过测试环境中根据日志追踪问题半天查不到原因的情况，这很大部分原因是对由于你对异常的处理不够好，日志没有记录导致的；通过远程调试能快速的帮助你找到测试环境产生的问题。
> 这种方式是个捷径，但我们不能过度的依赖它；而是应该处理好异常，异常日志不要漏打。
<!--more-->

## 服务端开启调试模式
在服务端tomcat的catalina.sh文件中添JVM参数
```shell
-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=xxxx
```
### 参数解释
1. -Xdebug：通知JVM工作在调试模式下
2. -Xrunjdwp：通知JVM使用（java debug wire protocol）来运行调试环境。参数同时有一系列的调试选项:
   1. `session`:指定了调试数据的传送方式，dt_socket是指用SOCKET模式，另外dt_shmem指用共享内存方式，其中dt_shmem只适用于窗口平台.server  参数是指是否支持在服务器模式的虚拟机中
3. onthrow：指明当产生该类型的异常时，JVM就会中断下来，进行调式该参数任选:
   1. `release`:指明当JVM被中断下来时，执行的可执行程序该参数可选
   2. `suspend`:指明：是否在调试客户端建立起来后，再执行 JVM
4. onuncaught（= y或n）指明出现未捕获的异常后，是否中断JVM的执行

## IDEA远程调试配置
![](http://ww1.sinaimg.cn/large/006tNc79ly1g5491exmluj317u0u0dmt.jpg)
1. 打开Inteliij IDEA，顶部菜单栏选择Run-> Edit Configurations，进入下图的运行/调试配置界面;点击左上角'+'号，选择Remote。
![](http://ww1.sinaimg.cn/large/006tNc79ly1g5493k7ippj317u0u0q6u.jpg)
2. 分别填写参数：Name，Host，Port；选择需要运行的模块。
![](http://ww1.sinaimg.cn/large/006tNc79ly1g5495o0sfoj30c401sq2u.jpg)
3. 选择配置的运行方式，点击Debug方式运行。
## 断点注意事项
由于测试环境可能是多个人在跑，为了尽可能少的影响别人，可以将断点的模式改为Thread，这样就不会阻断其它线程的运行。
![](http://ww4.sinaimg.cn/large/006tNc79ly1g5498m45xfj30ne0agjry.jpg)