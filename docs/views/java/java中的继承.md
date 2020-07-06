---
title: Java中的继承和接口
date: 2020-06-25
tags:
 - java
categories:
 - java
description: false
---

## 超类和子类
子类会继承超类的域和方法，子类还可以重写父类的方法，当子类调用父类的方法时，使用super调用超类的方法。
```java
public class Manger extends Employee {

    @Override
    public Integer getAge() {
        return super.getAge();
    }
}
```
除此之外,子类还可以调用父类的构造器，因为自动继承父类的域，但是无法为父类的域赋值，所以可以使用super调用父类的构造方法。
```java
public Manger(String name, Integer age) {
    super(name, age);
}
```

## 对象变量的多态
java中多态的表现形式有很多，其中对象变量也可以是多态的，比如下面这段代码，Manger是Employee的子类，所以可以将manger放入test中，但是注意，不可以将父类的对象放入子类的数组中。
```java
public static void main(String[] args) {
    Manger manger = new Manger("qiuxu",14);
    Employee[] test = new Employee[2];
    test[0] = manger;
}
```

## 方法调用
如何在对象上找到对应的方法，首先虚拟机会搜索一遍类中所有的public和父类中public方法，生成一份方法表。再根据相对应的方法的名字和参数(也就是方法签名)，调用相对应的方法。这种在运行时确认调用方法的形式叫做动态绑定，而一些被static，final和构造方法或者priva修饰的句子，在一开始就确定好了是哪个方法，这种称为静态绑定

## 反射
利用反射可以动态的对类进行增强。java中的Class类可以获取一个类的信息，使用getName()方法获取类名，如果类名在包下，还会顺便打出报名，newInstance方法可以获得类的实例。
```java
ReflectTest reflectTest = new ReflectTest();
System.out.println(reflectTest.getClass().getName());
System.out.println(Class.forName(reflectTest.getClass().getName()).newInstance());
//输出
reflectLearning.ReflectTest
class reflectLearning.ReflectTest
```
在java.lang.reflect包中有三个类Field、Method和Constructor分别用于描述类的域、方法和构造器。这三个类都有一个叫做getName的方法，用来返回项目的名称。   
Field类有一个getType方法，用来返回描述域所属类型的Class对象。Method和Constructor类有能够报告参数类型的方法，Method类还有一个可以报告返回类型的方法。这三个类还有一个叫做getModifiers的方法，它将返回一个整型数值，用不同的位开关描述public和static这样的修饰符使用状况。   
另外，还可以利用java.lang.reflect包中的Modifier类的静态方法分析getModifiers返回的整型数值。例如，可以使用Modifier类中的isPublic、isPrivate或isFinal判断方法或构造器是否是public、private或final。   
我们需要做的全部工作就是调用Modifier类的相应方法，并对返回的整型数值进行分析，另外，还可以利用Modifier.toString方法将修饰符打印出来。

- 获取域   
  由于name属性是private的，所以在获取的时候要设置accessible，才能够获取到相对应的方法。
  ```java
  try {
      ReflectTest reflectTest = new ReflectTest();
      reflectTest.name = "qiuxu";
      Field field = reflectTest.getClass().getDeclaredField("name");
      field.setAccessible(true);
      Object object=field.get(reflectTest);
      System.out.println(object);
  } catch (NoSuchFieldException e) {
      e.printStackTrace();
  }
  ```
- 调用方法   
  使用invoke可以反射调用方法，直接上代码，调用getDeclaredMethod方法查询相对应类中的方法，注意getDeclaredMethod还可以输入参数的类型以确定是哪个方法，然后使用invoke调用方法即可，该方法可以在参数中加入原方法参数
  ```java
  ReflectTest reflectTest = new ReflectTest();
  reflectTest.name = "qiuxu";
  try {
      Method m1 = reflectTest.getClass().getDeclaredMethod("getName");
      String name = (String) m1.invoke(reflectTest);
      System.out.println(name);
  } catch (NoSuchMethodException | InvocationTargetException e) {
      e.printStackTrace();
  }
  //输出
  qiuxu
  ```
## 接口

- 接口冲突    
  java8之后接口可以实现默认方法，但是如果一个同时继承两个接口或者父类，这两个接口或是父类中有签名完全一样的方法，那么java是怎么解决的？
  ```java
  1）超类优先。如果超类提供了一个具体方法，同名而且有相同参数类型的默认方法会被忽略。2）接口冲突。如果一个超接口提供了一个默认方法，另一个接口提供了一个同名而且参数类型（不论是否是默认参数）相同的方法，必须覆盖这个方法来解决冲突。
  ```
  在java核心卷中，是这么解释的，如果是父类和接口产生冲突，那么父类优先，如果是两个接口则必须要覆盖掉这个方法