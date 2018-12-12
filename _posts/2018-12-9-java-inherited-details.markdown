---
layout:     post
title:      "Java类继承的那些细节"
date:       2018-12-9 15:15:00
author:     "KM"
catalog: true
tags:
    - Java
---

# Java类继承的那些细节

## 细节1：子类继承父类，子类如何调用父类的变量和方法？

假设有父类Person，如下

```java
public class Person {
    public int age = 99;
    
    public void printfPersonAge() {
        System.out.println("Person:age=" + age);
    }
}
```

子类Student，如下

```java
public class Student extends Person {
    String school;

    public void printfAge() {
        printfPersonAge();  //调用父类方法
        System.out.println("age:" + age); //调用父类变量
    }
}
```

理论上子类继承父类那么子类可以直接调用父类的变量和方法，但是当子类出现和父类变量名一样的变量或和父类方法名一样的方法，那么这种调用就会出问题。

**问题一：**子类和父类有同名同类型的变量时，子类变量是否会覆盖父类变量？

答：不会，但子类调用父类的变量需要用super变量作为限制，否则将默认先调用子类变量。

**问题二：**子类和父类有同名不同类型的变量时，编译是否会出错？

答：不会

## 细节2：default属性对继承的影响

我们知道被default访问修饰符修饰时只能在本类或同一个包内被访问，子类无法访问，那么其实意味着如果是同一个包的子类也是能够访问的，其它包的子类无法访问。

![](/img/article-picture/2018-12-9-java-inherited-details-img-1.png)
