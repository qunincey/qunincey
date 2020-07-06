---
title: Stringbuilder和Stringbuffer
date: 2020-06-28
tags:
 - java
categories:
 - java
description: false
---

## 使用
java中的String本质上是一个char类型的数组，并且在String内部，这个char数组被final修饰，在第一次赋值后不能更改，如果频繁的调用和更改，运行速度会很慢，所以诞生了Stringbuilder类。
```java
String str = "abc" + "de";
StringBuilder stringBuilder = new StringBuilder().append("abc").append("de");
System.out.println(str);
System.out.println(stringBuilder.toString());
```

由于Stringbuilder类并不是线程安全的类，所以又诞生了Stringbuffer，Stringbuffer类大部分方法都带有线程锁的。能够满足多线程环境下对String的操作