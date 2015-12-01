---
layout: post
title: "使用Xcode 7和iOS9遇到的问题总结"
date: 2015-09-26 20:49:32 +0800
comments: true
categories: iOS开发
---
##1、Bitcode
Bitcode是被编译程序的一种中间形式的代码。包含Bitcode配置的程序将会在App Store上被编译和链接。当我们提交程序到App Store时，Xcode会将程序编译为一个中间形式，也就是Bitcode，然后App Store会再将这个Bitcode编译为64位程序。Bitcode允许苹果在后期重新优化我们程序的二进制文件，而不需要我们重新提交一个新版本到App Store上(比如编译器更新，苹果会自动用之前提交的Bitcode进行优化，这样就不必再编译器更新后再次提交App也能享受编译器升级带来的好处。另外比如我们的软件包中一般都有2x和3x的图片，在iOS9中可以只选择需要的内容下载，节省了用户的流量)。<!--more--><br>在Watch中的应用必须包含Bitcode，iOS不强制，MacOS不支持，但是在Xcode 7及以上版本中会默认开启Bitcode。但是有一些第三方的包还不支持Bitcode，就只能通过修改编译选项了，如下图：<br>![关闭Bitcode](http://7xn37b.com1.z0.glb.clouddn.com/Bitcode.png)<br>

##2、网络协议
iOS9之后改用更安全的https，如果应用中还是使用的HTTP通讯协议，则不能成功，需要在info.plist文件中加上以下声明：<br>
![使用HTTP](http://7xn37b.com1.z0.glb.clouddn.com/httpsKey.png)<br>

##3、应用跳转白名单URL Scheme
iOS9中引入了白名单的概念，应用间的跳转需要在info.plist中进行配置，如下图：![URLScheme](http://7xn37b.com1.z0.glb.clouddn.com/URLScheme.png)<br>

##4、上传App Store时遇到的问题
![上传App遇到的问题](http://7xn37b.com1.z0.glb.clouddn.com/TencentOpenApiBundle.png)<br>
需要进入到对应bundle目录下的info.plist文件中做相应的修改即可。

##5、使用UIAlertView作为输入框
iOS9中正式废除了UIAlertView，改为使用UIAlertController，可以参考我的[这篇博客](http://www.xuelixiang.com/blog/2015/09/21/uialertcontroller/)。


