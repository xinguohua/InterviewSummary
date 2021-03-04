# Java基础
## final关键字的作用

　　在Java中，final关键字可以用来修饰**类、方法和变量（包括成员变量和局部变量）**。下面就从这三个方面来了解一下final关键字的基本用法。

* 修饰类

  　　当用final修饰一个类时，表明这个类不能被继承。也就是说，**如果一个类你永远不会让他被继承**，就可以用final进行修饰。final类中的成员变量可以根据需要设为final，但是要注意final类中的所有成员方法都会被隐式地指定为final方法。

* 修饰方法

  	只有在想明确禁止该方法在子类中被覆盖的情况下才将方法设置为final的。

* 修饰变量

   对于一个final变量。

  * 如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；
  * 如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。

[参考](https://www.cnblogs.com/dolphin0520/p/3736238.html)

## ArrayList和LinkedList的区别 

ArrayList和LinkedList的区别有以下几点：

1. ArrayList是实现了**基于动态数组**的数据结构，而LinkedList是**基于链表**的数据结构；

2. 对于**随机访问get和set，ArrayList要优于LinkedList**，因为LinkedList要移动指针；

3. 对于添加和删除操作add和remove，一般大家都会说LinkedList要比ArrayList快，因为ArrayList要移动数据。但是实际情况并非这样，对于添加或删除，LinkedList和ArrayList**并不能明确说明谁快谁慢**

   [参考](https://blog.csdn.net/eson_15/article/details/51145788)