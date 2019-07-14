---
layout:     post
title:      hashCode和equals
subtitle:   哈希值和对象比较
date:       2019-07-14
author:     YZYSUPER
header-img: img/post-bg-debug.png
catalog: true
tags:
    - JAVA集合合集
    - 算法
    - 集合
---

## hashCode和equals

### Object对象中的hashCode方法

hashCode是native的方法，返回值是对象实例在内存中的地址整数（16进制）；

### 重写hashCode方法

重写要降低hash碰撞的概率。

### Object对象中的equals方法

使用"=="进行对象比较，实际上就是引用地址的比较是否指向同一个内存地址；

### 重写equals方法

需要遵循如下规则：
- 自反性：非null对象x，永远x.equals(x)；
- 对称性：非null对象x,y，x.equals(y)的结果永远和y.equals(x)的结果一致；
- 传递性：非null对象x,y,z，x.equals(y)结果为true，y.equals(z)结果为true，那么x.equals(y)的结果也为true；
- 一致性：非null对象x,y，只要实例的域不变，计算多次，每次x.equals(y)的结果不变；
- 非null性：非null对象x，x.equals(null)永远为false，x不可能为null；
- hashCode要重写：重写equals方法一定要重写hashCode方法，以保证equals为true的两个对象hashCode值也一样，尽可能的保证equals为false的两个对象hashCode值以不一样（有可能一样）；
- compareTo要重写：类实现了Comparable结果时，重写equals方法一定要重写compareTo方法，保证comprareTo方法的结果与equals方法的结果一致；

自反性，一致性，非null性很好理解。

* 什么时候会违反对称性：在使用包装类时，equals方法对包装类型进行了兼容处理，但是被包装类的equals方法并不兼容包装类；
* 什么时候会违反传递性：当使用继承方式实现子类时，具有不同域的子类分别实现了自己的equals方法时，子类和父类可以很好的进行equals运算，但是子类对象之间不具备传递性；
* 为什么要重写hashCode方法：HashMap的key去重算法是先使用key的hash值进行比较（null的hash值为0），如果不同两个对象肯定是不同的（重写hashCode的必要性）。如果hash值相同（发生哈希碰撞）要进行对象的equals比较，结果为true时表明对象相等，结果为false表明对象不相等；两个equals为true的对象hashCode的值不相等的话，就会出现相同key或是取值为null的情况；
* 为什么要重写compareTo方法：实现Comparable的接口具有自然排序的能力，在有排序需求的场景下保证和预期一致；

### 不需要重写equals方法

- 父类（非Object）已经重新实现了equals方法（如AbstractMap），参照“什么时候会违反传递性”；
- 不可变类（如String，Integer），本身保证唯一；
- 不提供外部构造能力的类，访问级别为包级，private的类；
- 不需要进行比较的类；

