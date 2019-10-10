---
title: '静态导入import static'
layout: post
tags:
  - Java
category: Java
lang: zh
---

偶然看到import static，感觉很新奇，以前没用过，所以整理了一下关于静态导入的使用方式和注意事项

<!-- more -->

# 意义

`import static`静态导入是`JDK 1.5`增加的特性，有两种使用方式
```java
import static java.lang.Integer.*;
或者
import static java.lang.Integer.MAX_VALUE;
```

解释
* 用`.*`时，表示导入类中的所有静态属性和方法
* 用静态方法名时，表示只导入该静态属性或方法

静态导入之后就可以直接用名称访问静态属性和方法，而不必用`ClassName.methodName()`的方式来访问

其目的是为了减少字符输入量，提高代码的可阅读性，以便更好地理解程序。但是，滥用静态导入会使程序更难阅读，更难维护

# 使用

## 正确使用

当程序中需要经常使用某个静态方法时，可以使用静态导入，用于减少字符输入量和提高程序可读性。比如封装一下常用的`System.out.println()`，就可以直接使用`println()`了

使用静态方法封装目标方法
```java
package com.demo.importstatic

public class Utils {
    public static void println(String x) {
        System.out.println(x);
    }
}
```

静态导入
```java
package com.demo.importstatic

import static com.demo.importstatic.Utils.println;

public class Test {
    public static void main(String[] args) {
        println("a test string");
    }
}
```

## 错误使用

如果用`.*`方式导入多个类的静态方法，就会使代码可读性降低，分不清静态方法到底属于哪个类，也不清楚所使用的静态方法的意义。比如
```java
import static java.lang.Double.*; 
import static java.lang.Math.*; 
import static java.lang.Integer.*; 
```

静态导入这些类的静态属性和方法之后，当调用`MAX_VALUE`时，就不知道到底调用的哪个类的属性，编辑器这时候就会报错

# 总结

对于静态导入，一定要遵循两个规则
1. 不要使用`.*`方式，除非是导入静态常量（只包含常量的类或接口）
2. 方法名具有明确、清晰的表象意义

# 参考

* [java 静态导入 import static和import的区别](http://webcache.googleusercontent.com/search?q=cache:http://864343928.iteye.com/blog/2093663)
* [Java提高：静态导入给你带来的麻烦](https://blog.csdn.net/p106786860/article/details/9297783)