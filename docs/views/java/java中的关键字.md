---
title: Java中的关键字
date: 2020-06-22
tags:
 - java
categories:
 - java
description: false
---

## final关键字
常量关键字，使用final的变量第一次赋值之后不能被更改。final这个不可更改的意思是不更改引用对象，即用final修饰的字段和对象永远指向第一次赋值的时候的那个对象，但是对象本身的数据可以更改

- fianl类和方法    
  final除了修饰域外，还可以修饰类和方法，当final修饰类时，类中所有的方法自动添加final类，这种举动是为了避免子类在继承的时候改变相对应的方法，也就是fianl不允许子类改变自身的方法。

## strictfp关键字
strict float point 精确浮点数的简称，可以用与类和方法，接口，使用了这个关键字可以保证在不同机器上的浮点数的准确度。

## static关键字

- 静态域
  如果static修饰到域上，那么每一个类都共享这个域,所有对象访问nextId都是相同的值
  ```java
  private static nextId;
  ```
- 静态常量
  static在java中更多的是放在常量里 final保证无法更改，static保证所有对象一致
  ```java
  private static final PI
  ```
- 静态方法
  修饰在方法前，这个方法就相当于独立于类了，不能再访问其他域，只能访问自身类的静态域。换句话说，就没有隐式参数了。

## abstract关键字
抽象关键字，可以用在类或者方法上，用在类上，类中所有的方法都是抽象方法，抽象类不能实例化