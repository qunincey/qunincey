---
title: URL中的hash号和vue中的路由
date: 2018-12-15
tags:
 - vue
categories:
 -  vue
---
# URL中的hash号和vue中的路由

在使用vue-router时,看到URL中的#,都没有真正的理解过，写一篇博客记录一下。
首先理解hash要先理解URL和URI之间的区别
>URI，Uniform Resource Identifier，统一资源标识符<br>
>URL，Uniform Resource Location，统一资源定位符

URI描述了和定义一个资源，URL不仅描述和定义一个资源并且描述了如何获得资源，URL是URI的子集。URL会发送请求并且会重载页面，但是URI不会，URI本质上只是本地网页的资源的切换。

## fragment
fragment代表#号后面地址，用来标识资源

![alt](https://upload-images.jianshu.io/upload_images/1155692-865aa6e8904a0509.png?imageMogr2/auto-orient/strip|imageView2/2/w/918/format/webp)

这里介绍两个前端特性，一个用来监听#号后面的变化，一个用来设置hash的变化  
window.location.hash这个属性可读可写。读取时，可以用来判断网页状态是否改变；写入时，则会在不重载网页的前提下，创造一条访问历史记录。
这是一个HTML 5新增的事件，当#值发生变化时，就会触发这个事件。IE8+、Firefox 3.6+、Chrome 5+、Safari 4.0+支持该事件。

vue-router就是利用这三个的原理，用户点击按钮改变hash后的值，监听hash后面的变化，定位到具体的fragment  
![alt](https://upload-images.jianshu.io/upload_images/1155692-625518baf3633b89.png?imageMogr2/auto-orient/strip|imageView2/2/w/986/format/webp)






