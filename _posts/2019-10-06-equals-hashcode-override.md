---
title: 'equals()和hashCode()同时重写？'
layout: post
tags:
  - Java
category: Java
---

Java编码规范中，通常会要求同时重写equals()和hashCode()，具体原因却从来不清楚，并且重写时应该按照何种原则或规范也不知道。因此探究了一下Object和String的源码，总结了重写时的规范和步骤

<!-- more -->

# equals()

首先看下Object中的equals()，仅简单的用`==`比较两个对象
```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

源码中对equals()的注释如下
```text
Indicates(指出) whether some other object is "equal to" this one.

The equals method implements an equivalence(等价) relation on non-null
object references:

    1. It is reflexive(自反的): for any non-null reference value
    x, x.equals(x) should return true.
    
    2. It is symmetric(对称的): for any non-null reference values
    x and y, x.equals(y) should return true if and only if

    3. It is transitive(传递的): for any non-null reference values
    x, y, and z, if x.equals(y) returns true and y.equals(z) returns
    true, then x.equals(z) should return true.
    
    4. It is consistent(一致的): for any non-null reference values x and
    y, multiple invocations of x.equals(y) consistently(始终) return true
    or consistently return false, provided no information used in equals
    comparisons on the objects is modified.

    5. For any non-null reference value x, x.equals(null) should return false.

The equals method for class Object implements the most discriminating(判别)
possible equivalence relation on objects; that is, for any non-null reference
values x and y, this method returns true if and only if x and y refer to
the same object (x == y has the value true).

Note that it is generally necessary to override the hashCode method whenever
this method is overridden, so as to maintain the general contract for the
hashCode method, which states that equal objects must have equal hash codes.
```

在注释中主要描述了equals()应该具有的一些特点，或者说是重写equals()时应当遵守的规范
1. 自反性：x = x
2. 对称性：x = y => y = x
3. 传递性：x = y, y = z => x = z
4. 一致性：只要没有修改用于比较的信息，x.equals(y)总是返回一致的结果
5. 非空性：对于任何非null的对象，x.equals(null)总是返回false

对于equals()的重写，可以参考String的做法
```java
public boolean equals(Object anObject) {
    // 若指向同一个内存地址，即为同一个String实例，返回true
    if (this == anObject) {
        return true;
    }

    // 如果对象不是String实例，返回false
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        // 如果value长度不一样，返回false
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            // 逐个字符比较，若有不相等字符返回false
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            // 所有字符都相同，返回true
            return true;
        }
    }
    return false;
}
```

可以从String的equals()方法中总结出覆盖equals()方法的一般形式
1. 使用==检查是否指向同一内存地址，如果是则返回true（这是一种性能优化，如果比较操作有可能很昂贵，就值得这么做）
2. 使用instanceof检查对象是否是“正确类型”，如果不是则返回false（一般来说，“正确类型”是指equals()方法所在的类）
3. 把参数转换成正确的类型（因为转换之前进行过instanceof测试，所以确保会成功）
4. 检查对象中的“关键”域是否对应“相等”。如果这些测试全部成功则返回true，否则返回false
5. 当重写equals()方法后，检查是否符合上述Object.equals()规范

# hashCode()

Object中的hashCode()是一个native函数，返回值是int类型
```java
public native int hashCode();
```

源码中对hashCode()的注释如下
```text
Returns a hash code value for the object. This method is supported for the
benefit of hash tables such as(诸如) those provided by java.util.HashMap.

The general contract of hashCode is:

    1. Whenever it is invoked on the same object more than once during
    an execution of a Java application, the hashCode method must
    consistently(总是) return the same integer, provided no information
    used in equals comparisons on the object is modified. This integer need
    not remain consistent from one execution of an application to another
    execution of the same application.

    2. If two objects are equal according to the equals(Object) method, then
    calling the hashCode method on each of the two objects must produce the
    same integer result.

    3. It is not required that if two objects are unequal according to the
    java.lang.Object#equals(java.lang.Object) method, then calling the
    hashCode method on each of the two objects must produce distinct(不同)
    integer results.  However, the programmer should be aware(意识到) that
    producing distinct integer results for unequal objects may improve the
    performance of hash tables.

As much as is reasonably practical, the hashCode method defined by class
Object does return distinct integers for distinct objects. (This is typically
implemented by converting the internal address of the object into an integer,
but this implementation technique is not required by the Java&trade;
programming language.)
```

注释中主要描述的也是对重写hashCode()时应当遵守的规范
1. 在程序的一次运行中，只要没有修改equals()中用于比较的信息，x.hashCode()总是返回一致的hash值
2. 如果x.equals(y)返回true，则x和y具有相等的hash值
3. 如果x和y具有相等的hash值，x.equals(y)并不一定返回true

比较重要的一点是“x.equals(y)返回true”是“x和y具有相同的hash值”的充分不必要条件，因为hashCode()无法避免hash碰撞，所以即使不同的两个对象也可能产生相同的hash值

通常需要一些方法解决hash碰撞
1. 开放定址法：Hi(k) = (H(k) + di) % m, (i = 1, 2, …, k(k <= m-1))
2. 再哈希法：出现碰撞时，依次使用H1(k), H2(k), ..., Hi(k), (i = 1, 2, ...)计算hash值
3. 链地址法：把所有hash值为i的元素构成一个称为同义词链的链表，并将头指针放在第i个单元
4. 公共溢出区：如果Hash函数值域为[0,m-1]，则令HashTable[0..m-1]为基本表，另外设立溢出表OverTable[0..v]存储产生碰撞的记录

对于hashCode()的重写，可以参考String的做法
```java
public int hashCode() {
    int h = hash;
    if (h == 0 && value.length > 0) {
        char val[] = value;
        for (int i = 0; i < value.length; i++) {
            h = 31 * h + val[i];
        }
        hash = h;
    }
    return h;
}
```

实现的计算表达式如下所示（s[i]是字符串的第i个字符，n是字符串的长度，^表示求幂。空字符串的hash值是0）
```
s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]
```

Java源码中许多地方都使用31作为计算hash值的乘子，对于乘子31的选择，主要有两点原因
* 31是一个不大不小的质数，是作为hashCode()乘子的优选质数之一，可以均匀分布hash值并减少信息丢失
* 31可以被JVM优化，31 * i = (i << 5) - i

String的hashCode()方法就是根据value生成hash值，这样不同value的字符串就容易生成不同的hash值，既能减少碰撞又能生成与内容相关的hash值，这样就能实现上述Object.hashCode()规范

# 同时重写？

hashCode()方法主要用于hash表，比如HashMap，当集合要添加元素时，大致按如下步骤
1. 先调用该元素的hashCode()方法，直接定位到它应该放置的物理位置上
2. 如果这个位置上没有元素，就直接存储在这个位置上
3. 如果这个位置上已经有元素，就调用equals()方法进行比较，相同的话就更新，不相同就使用上述解决碰撞的方法存储新元素

所以重写equals()方法时，也必须重写hashCode()方法。如果不这样做，就会违反Object.hashCode()的规范，导致无法结合所有基于hash的集合一起正常运作，这样的集合包括HashMap、HashSet和Hashtable

那为什么不直接使用equals()进行操作呢？如果只使用equals()，意味着需要迭代整个集合进行比较操作，如果集合中有1万个元素，就需要进行1万次比较，这明显不可行