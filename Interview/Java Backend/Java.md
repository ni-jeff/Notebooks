# Java基础
1. 八种基本数据类型的大小，以及他们的封装类

|类型|大小(Byte)|封装类|默认值|  
|:----|:----|:----|:----|  
|byte|1|Byte|0|  
|short|2|Short|0|  
|int|4|Integer|0|  
|long|8|Long|0|  
|Char|2|Character|'/u0000'|  
|float|4|Float|0.0f|  
|double|8|Double|0.0d|  
|boolean|1|boolean|false|  
- 默认值使用条件：当变量作为作为类成员使用时，java才给定初始值，防止程序运行时错误。([参考链接](https://blog.csdn.net/lxxlxx888/article/details/62228621/))
- [int和Integer的区别](https://www.cnblogs.com/guodongdidi/p/6953217.html)：  
    1. Integer是int的包装类，int则是java的一种基本数据类型  
    2. Integer变量必须实例化后才能使用，而int变量不需要  
    3. Integer是对象，用一个引用指向这个对象；而int是基本类型，直接存储数值。  
    4. Integer的默认值是null，int的默认值是0
    - Integer非new生成的变量在[-128~127]间时，java会在常量池中生成一个缓存。

2. 引用数据类型  
数组、类、接口  
3. Switch能否用string做参数  
- 在Java 7之前，switch只能支持byte、short、char、int或者其对应的封装类以及Enum类型。  
（实际上switch的括号中只能放int，比int空间小的byte、short、char会被强制转换）  
- 在Java 7中，String支持被加上了。  

4. equals与==的区别  

5. 自动装箱，常量池
6. Object有哪些公用方法
7. Java的四种引用，强弱软虚，用到的场景
8. Hashcode的作用
9. HashMap的hashcode的作用
10. 为什么重载hashCode方法？
11. ArrayList、LinkedList、Vector的区别
12. String、StringBuffer与StringBuilder的区别
13. Map、Set、List、Queue、Stack的特点与用法
14. HashMap和HashTable的区别
15. JDK7与JDK8中HashMap的实现
16. HashMap和ConcurrentHashMap的区别，HashMap的底层源码
17. ConcurrentHashMap能完全替代HashTable吗
18. 为什么HashMap是线程不安全的
19. 如何线程安全的使用HashMap
20. 多并发情况下HashMap是否还会产生死循环
21. TreeMap、HashMap、LindedHashMap的区别
22. Collection包结构，与Collections的区别
23. try?catch?finally，try里有return，finally还执行么
24. Excption与Error包结构，OOM你遇到过哪些情况，SOF你遇到过哪些情况
25. Java(OOP)面向对象的三个特征与含义
26. Override和Overload的含义去区别
27. Interface与abstract类的区别
28. Static?class?与non?static?class的区别
29. java多态的实现原理
30. foreach与正常for循环效率对比
31. Java?IO与NIO
32. java反射的作用于原理
33. 泛型常用特点
34. 解析XML的几种方式的原理与特点：DOM、SAX
35. Java1.7与1.8,1.9,10 新特性
36. 设计模式：单例、工厂、适配器、责任链、观察者等等
37. JNI的使用
38. AOP是什么
39. OOP是什么
40. AOP与OOP的区别