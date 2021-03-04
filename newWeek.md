https://www.nowcoder.com/discuss/601110?source_id=discuss_experience_nctrack&channel=-1

https://www.nowcoder.com/discuss/601466?source_id=discuss_experience_nctrack&channel=-1

https://www.nowcoder.com/discuss/594039?source_id=discuss_experience_nctrack&channel=-1

# Spring

## 1 简单介绍springIOC容器思想

## 2 Spring的aop 

## 3 注入方式 

## 4 事务传播行为

## 5 你是怎么实现Bean的？在Spring里，Bean的生命周期知道吗？

简单说下Spring Boot启动后的过程。

Java代理了解吗？jdk/cglib，Java代理模式了解吗？

设计模式了解过吗？介绍几种

Bean是怎么实现的？

Spring中的Controller ，Service，Dao是不是线程安全的
如何保证Bean的线程安全
为什么Spring框架不加一些多线程同步机制来保证Bean的线程安全？

一个接口有两个实现类，在Spring中怎么去调用到自己想调用的实现类
Spring循环依赖的解决方法
SpringBean的生命周期
SpringBoot的启动流程
SpringBoot的自动装配原理
和@Conditional注解功能类似的注解有哪些
SpringAOP的实现原理
如何用cglib来实现动态代理



# JVM
Java类加载相关。
类加载机制？
双亲委派机制？Spring的类用的是什么加载器？双亲委派中最底层的是哪种加载器？最顶层的呢？


# 数据库
数据库事务。
什么是数据库的事务？什么是事务的一致性？怎么保证事务的一致性？
MySQL数据库隔离的实现。
数据库索引命中和最左前缀，面试官给了一个学生表实例。
索引慢是怎么回事？如何解决？MySQL优化

MySQL表锁
死锁如何解决？并发条件下两方相互转账，A转账需要先锁定自己账户再锁定B账户，B转账需要先锁定自己账户再锁定A账户，两方同时操作如何解决死锁？我答了破坏不剥夺的条件，也就是说在transaction注解上加入超时判断，又问我还有别的办法吗？我说一次性获取所有资源（破坏请求与保持），又问如何一次性获取所有资源？愣了好久才想起来是表锁。。。


# 算法
===========x=============
算法：找出超出数组长度一半的众数；
股票一天交易
给定一个以字符串表示的非负整数 num，移除这个数中的 k 位数字，使得剩下的数字最小

==========m=============
判断两链表是否有交点 lc160

查找峰值 lc162