---
title: Java 重写 equals 与 hashCode 的注意事项
date: 2015-09-21 23:45:10
categories: 编程技术
tags: Java
---
为什么重写 equals 的时候必须重写 hashCode？我们需要注意什么？
<!--more-->

## 为什么重写 equals 的时候必须重写 hashCode
大家可能从很多教程中了解到： 

SUN官方的文档中规定：
> 如果重定义 equals 方法，就必须重定义 hashCode 方法,以便用户可以将对象插入到散列(哈希)表中

那么 SUN 公司是出于什么考虑做了这个规定呢？ 

在集合框架中的 `HashSet`，`HashTable` 和 `HashMap` 都使用哈希表的形式存储数据，而 `hashCode` 计算出来的哈希码便是它们的身份证。哈希码的存在便可以： 

快速定位对象，提高哈希表集合的性能。只有当哈希表中对象的索引即 `hashCode` 和对象的属性即 `equals` 同时相等时，才能够判断两个对象相等。

从上面可以看出，哈希码主要是为哈希表服务的，其实如果不需要使用哈希表，也可以不重写 `hashCode`。但是 SUN 公司应该是出于对程序扩展性的考虑（万一以后需要将对象放入哈希表集合中），才会规定重写 `equals` 的同时需要重写 `hashCode`，以避免后续开发不必要的麻烦。

## 重写 equals 的注意事项
Java 语言规范要求 `equals` 需要具有如下的特性： 

- 自反性：对于任何非空引用 x，`x.equals()` 应该返回 true。
- 对称性：对于任何引用 x 和 y，当且仅当 `y.equals(x)` 返回 true，`x.equals(y)` 也应该返回 true。
- 传递性：对于任何引用 x、y 和 z，如果 `x.equals(y)` 返回 true，`y.equals(z)` 也应返回同样的结果。
- 一致性：如果 x 和 y 引用的对象没有发生变化，反复调用 `x.equals(y)` 应该返回同样的结果。对于任意非空引用 x，`x.equals(null)` 应该返回 false。

## 如何重写 equals
在对象比较时，我们应该如何编写出一个符合特性的 `equals` 方法呢，《Core Java》中提出了如下建议：

1. 显式参数命名为 `otherObject`，稍后将它转换成另一个叫做 `other` 的变量。
2. 检测 `this` 与 `otherObject` 是否引用同一个对象： 
	
	``` java
	if (this == otherObject) return true;
	```
	计算这个等式可以避免一个个比较类中的域，实现优化。
3. 检测 `otherObject` 是否为 null，如果为 null，返回 false。进行非空校验是十分重要的。
4. 比较 `this` 与 `otherObject` 是否属于同一个类。
	- 如果每个子类都重写了 `equals`，使用 `getClass` 检验：
	
		``` java
		if (getClass() != otherObject.getClass()) 
    		return false; 
    	```
	- 如果所有子类都使用同一个 `equals`，就用 `instanceof` 检验：
		
		``` java
		if (!(otherObject instanceof ClassName))
		    return false;
		```
5. 将 `otherObject` 转换为相应的类型变量。

	``` java
	ClassName other = (ClassName) otherObject;
	```
6. 现在可以对所有需要比较的域进行比较了。

	- 基本类型使用 == 比较
	- 对象使用 equals 比较
	- 数组类型的域可以使用静态方法 `Arrays.equals` 检测相应数组元素是否相等
7. 如果所有域匹配，则返回 true

**注意**：子类重写父类 equals 方法时，必须完全覆盖父类方法，不能因为类型错误或者其他原因定义了一个完全无关的方法。可以使用 `@Override` 注解对覆盖父类的方法进行标记，这样编译器便会检测到覆盖过程中的错误。

## 重写 hashCode 的注意事项
散列码（hash code）是由对象导出的一个整型值。散列码没有规律，在不同的对象中通过不同的算法生成，Java 中生成 hashCode 的策略为（以下说明均摘自 Java API 8）：

- String 类的 hashCode 根据其字符串内容，使用算法计算后返回哈希码。 

	> Returns a hash code for this string. The hash code for a String object is computed as s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]

- Integer 类返回的哈希码为其包含的整数数值。

	> Returns: a hash code value for this object, equal to the primitive int value represented by this Integer object.

- Object 类的 hashCode 返回对象的内存地址经过处理后的数值。

	> Returns a hash code value for the object. This method is supported for the benefit of hash tables such as those provided by HashMap.

在自己的类中想要重写 hashCode 的话一般怎么做呢？建议合理地组合实例域的散列码，让各个不同对象产生的散列码更加均匀。例如我们现在有一个 `Cat` 对象，它有 name、size 和 color 三个不同域，那么可以重写 hashCode 方法如下：

``` java
class Cat {
    ......
    public int hashCode() {
        // hashCode 是可以返回负值的
        return 6 * name.hashCode()
            + 8 * new Double(size).hashCode()
            + 10 * color.hashCode();
    }
    ......
}
```

当然还有更好的做法，我们可以直接调用静态方法 `Objects.hash` 并提供多个参数。这个方法会对各个参数调用 `Object.hashCode`，并组合返回的散列码。故以上的方法可以缩写为：

``` java
public int hashCode() {
    return Objects.hash(name, size, color);
}
```

**注意**： `equals` 与 `hashCode` 的定义必须一致，两个对象 `equals` 为 true，就必须有相同的 `hashCode`。例如：如果定义的 `equals` 比较的是小猫的 name，那么 `hashCode` 就需要散列该 name，而不是小猫的 color 或 size。

## 参考
- 《Core Java》卷一
- [【哈希表数据结构】【深入理解hashcode & equals】](http://blog.csdn.net/ie800/article/details/19012291)