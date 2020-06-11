---
title: Java中的时间处理
date: 2020-05-03
tags:
 - java
categories:
 - java
description: false
---

# Java中的时间处理
在详细介绍java相关api之前，我觉的很有必要了解一下关于时间的一些基本概念，java中大多数操作都要涉及到一些基本时间常识。
## 相关概念
时区：时间这种概念是一种规范，古时候由于太阳位置的不同，我们有了时间的概念。在现代，同一个太阳的位置，在不同的进度上，所谓的时间是不一样的。直到1863年，人们确立了真正时区的概念，时区代表了某一个经度范围内的一个标准时间。

理论时区: 理论上的时区就是以每15度经度划分一个时区，时区采用的时间是时区中央的本地时间。

实际时区: 理论上虽然划分好对应的时区，但是想俄罗斯，中国这种跨度很大的国家就需要处理不同时区的问题，所以，也有采取了法定时区的做法，各国安装相关行政需求划分时区线，比如中国就统一东八区时间，以北京时间为准。具体分布如下图(需翻墙)
![国际行政时区图](http://suo.im/6dncBB)   

国际日期变更线: 当我们划分了时区后，涉及到一个日期的问题，比如我在东八区北京给纽约的朋友发一个信息，假设没有日期变更线，那很有可能他那边已经是新的一天，而我们还在"昨天"，所以我们需要一个日期变更线，人为的变更日期，当经过这个国际日期变更线时，我们要加减日期。

GMT: 格林威治时间，指位于伦敦郊区的皇家格林尼治天文台的标准时间，在还在使用的GMT的时候，定义时区就经常使用GMT+8代表东八区的北京时间

UTC：原子钟时间，是比GMT更加准确的时间，其实格林威治的时间现在已经向UTC靠齐了，所以可以认为二者没什么区别。

### Java8之前

java中最常见的时间类是java.util.Date,这个类是java1.1之前就存在的类。如果现在去看java源码的话，还可以看到James Gosling等人所写的注释，但是这个类不适合国际化的使用，所以在1.1之后，引入了Calendar类，并且Date里的大部分方法废弃掉了，现在的Date更适合作为一个日期表示类，而日期相关的操作则需要使用Calendar类。    

Calendar类定义了常用的日期相关字段，比如时分秒等，如果想要改变现实的日期，只需要调用相对应的方法即可，比如这样
```java
public static Date getEndTime(Date date) {
        Calendar day = Calendar.getInstance();
        day.setTime(date);
        day.set(Calendar.HOUR_OF_DAY, 23);
        day.set(Calendar.MINUTE, 59);
        day.set(Calendar.SECOND, 59);
        day.set(Calendar.MILLISECOND, 999);
        return day.getTime();
    }
```
这是一个获取一天中结尾的时间，通过getInstance获取相关实例，再通过设置时分秒不同字段的值，最后使用getTime返回一个date类型。Calendar相关的方法还有很多，这一部分可以直接看文档。

### Java8之后

虽然Calendar类和相关的日期api能够满足日常开发需求，但是也有例如线程安全等问题，所以Java8引入了全新的时间api。在java.time包下，新的api将时间分为三个类，LocalDate，LocalTime，和LocalDateTime，前二者分别表示日期和具体时间，第三个表示日期加时间，下面提到获取一天结束时间，在java8之后则简便了很多
```
LocalDate date = LocalDate.now();
LocalTime localTime = LocalTime.of(0,0,0);
LocalDateTime localDateTime = LocalDateTime.of(date,localTime);
```
第一步获取日期，第二步设置时间，最后通过of方法同时设置日期和时间即可，不仅仅是代码简便了，相对应的代码语义化更好。







