---

layout:     post
title:      "斗鱼面经"
subtitle:   "面试"
date:       2018-10-10 12:00:00
author:     "wellyoung"
header-img: "img/home-bg-douyu.jpg"
header-mask: 0.3
catalog:    true
tags:
    - iOS
    - 面试
    
---

# 斗鱼面试总结
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;——好不容易拿到个还算大厂的面试机会，没想到一面就跪了，把面试经验写下来，然后真正去弥补自己的不足，诸君共勉之。
<br >
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;一共有十几个问题，基本都是针对你的项目展开进行追问，当时有点懵，回答上的不到一半，现在回想起来有些问题其实还是比较简单的，面试官会偏重底层原理这块(现在的大厂都这样🐶)，不多哔哔，直接上干货。
### 1.AsyncDisplayKit是如何保持界面流畅性的？
由于自己以前的项目用过这个框架，在项目介绍的时候提到了然后面试官就顺口就问了。该框架是Facebook出的，具体的原理请[戳我](https://www.cnblogs.com/linganxiong/p/5884030.html)

## 2.RN到底是web还是原生
这么弱智的问题当时居然被考到了<br>
随便打开一个乱七糟八的Demo看看层级关系吧
![页面层级](https://upload-images.jianshu.io/upload_images/2484273-08d79a24501e47e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
完了再看看文件目录结构吧，node_modules什么的可不是空的
![里面有写是怎么把组件封装好的](https://upload-images.jianshu.io/upload_images/2484273-5f34b5d6e854f64f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3.js和原生是怎么交互的
## 4.点击事件从点击到事件结束发生了些什么
这里我回答的时候又提到了响应者链，但问题是响应者链也没完全搞清楚啊

[点击一个button究竟发生了什么](https://www.jianshu.com/p/3b6347cd01a4)

[iOS事件点击之发生了什么？](https://www.jianshu.com/p/98ed2eaa40ac)

[从用户点击屏幕到程序作出反应之间都发生了什么？ --- iOS事件响应](https://www.jianshu.com/p/6ff87b3ab2cb)

## 5.了解到的常用数据结构有哪些
二叉树，链表都说不清楚，还是再看看教材吧
## 6.排序算法有哪些简要说明一下
说了快排和冒泡，追问了时间复杂度
## 7.逆向的实现过程
由于简历上写了了解逆向，简直给自己挖坑啊，说了一下微信抢红包外挂的流程。具体可以参考[这篇文章](https://www.jianshu.com/p/189afbe3b429)
## 8.数据解析
说了json，用MJExtension，追问了MJExtension是如何实现的，我哪知道
## 9.图像处理如何调参
打扰了！不会的东西就不要在简历里提，班门弄斧
## 10.在项目中用什么技术解决了什么问题最有成就感
我发现现在好多公司都喜欢问这个问题，而且这个问题好像确实了解到你现在什么水平
## 总结：菜是原罪，你是真的鱼


