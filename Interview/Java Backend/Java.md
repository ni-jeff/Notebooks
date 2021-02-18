# Java基础
### 1. 八种基本数据类型的大小，以及他们的封装类

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

### 2. 引用数据类型  
- 数组、类、接口  
### 3. Switch能否用string做参数  
- 在Java 7之前，switch只能支持byte、short、char、int或者其对应的封装类以及Enum类型。  
（实际上switch的括号中只能放int，比int空间小的byte、short、char会被强制转换）  
- 在Java 7中，String支持被加上了。  

### 4. equals与==的区别  

### 5. 自动装箱，常量池  
#### 5.1 自动拆装箱
- 定义：  
自动装箱: 就是将基本数据类型自动转换成对应的包装类。  
自动拆箱：就是将包装类自动转换成对应的基本数据类型。  
- 实现原理：  
自动装箱都是通过包装类的valueOf()方法来实现的  
自动拆箱都是通过包装类对象的xxxValue()来实现的。
- 原因：  
  因为Java是一种面向对象语言，很多地方都需要使用对象而不是基本数据类型。比如，在集合类中，我们是无法将int 、double等类型放进去的。因为集合的容器要求元素是Object类型。为了让基本类型也具有对象的特征，就出现了包装类型，它相当于将基本类型“包装起来”，使得它具有了对象的性质，并且为其添加了属性和方法，丰富了基本类型的操作。  
- 自动拆装箱的场景：  
  1. 将基本类型放入集合类 
  2. 基本类型和包装类需要转换：大小对比、运算、传参、返回值  
- 当需要进行自动装箱时，如果数字在-128至127之间时，会直接使用缓存中的对象，而不是重新创建一个对象。  
- 问题：自动拆箱时若对象为null，可能会抛出NPE。  
#### 5.2 常量池
- 目的：  
常量池是为了避免频繁的创建和销毁对象而影响系统性能，其实现了对象的共享。例如字符串常量池，在编译阶段就把所有的字符串文字放到一个常量池中。
  1. 节省内存空间：常量池中所有相同的字符串常量被合并，只占用一个空间。
  2. 节省运行时间：比较字符串时，==比equals()快。对于两个引用变量，只用==判断引用是否相等，也就可以判断实际值是否相等。  
- Java中的常量池分为三种形态：静态常量池（class文件常量池），运行时常量池，全局字符串常量池。  
  1. 静态常量池  
  在Class文件中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息是常量池(Constant Pool Table)，用于存放编译期生成的各种字面量（文本字符串，常量值）和符号引用（类和接口的全限定名，字段名称和描述符，方法名称和描述符）。  
  2. 运行时常量池  
  jvm虚拟机在完成类装载操作后，将class文件中的常量池载入到内存中，并保存在方法区中，我们常说的常量池，就是指方法区中的运行时常量池。  
  运行时常量池相对于Class文件常量池的另外一个重要特征是具备动态性，Java语言并不要求常量一定只有编译期才能产生，也就是并非预置入class文件中常量池的内容才能进入方法区运行时常量池，运行期间也可能将新的常量放入池中，这种特性被开发人员利用比较多的就是String类的intern()方法。  
  3. 全局字符串常量池  
  3.1 intern函数  
  在1.6中，intern的处理是：先判断字符串常量是否在字符串常量池中，如果存在直接返回该常量；如果没有找到，则将该字符串常量加入到字符串常量区，也就是在字符串常量区建立该常量；  
  在1.7中，intern的处理是：先判断字符串常量是否在字符串常量池中，如果存在直接返回该常量；如果没有找到，说明该字符串常量在堆中，则处理是把堆区该对象的引用加入到字符串常量池中，以后别人拿到的是该字符串常量的引用，实际存在堆中；  
  3.2 全局字符串常量池  
  常量池中的文本字符串会在类加载时进入字符串常量池。常量池中的字面量会在类加载后进入运行时常量池，其中字面量中有包括文本字符串，字符串常量池存在于运行时常量池中，也就存在于方法区中。不过在JDK1.7时，字符串常量池就被移出了方法区，转移到了堆里了。
### 6. Object有哪些公用方法
Object是所有类的父类，任何类都默认继承Object。

- clone
保护方法，实现对象的浅复制，只有实现了Cloneable接口才可以调用该方法，否则抛出CloneNotSupportedException异常

- equals
在Object中与==是一样的，子类一般需要重写该方法

- hashCode
该方法用于哈希查找，重写了equals方法一般都要重写hashCode方法。这个方法在一些具有哈希功能的Collection中用到

- getClass
final方法，获得运行时类型

- wait
使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。wait()方法一直等待，直到获得锁或者被中断。wait(long timeout)设定一个超时间隔，如果在规定时间内没有获得锁就返回。
调用该方法后当前线程进入睡眠状态，直到以下事件发生该线程就可以被调度了，如果是被中断的话就抛出一个InterruptedException异常：
  1. 其他线程调用了该对象的notify方法
  2. 其他线程调用了该对象的notifyAll方法
  3. 其他线程调用了interrupt中断该线程
  4. 时间间隔到了  

- notify
唤醒在该对象上等待的某个线程

- notifyAll
唤醒在该对象上等待的所有线程

- toString
转换成字符串，一般子类都有重写，否则打印句柄
### 7. Java的四种引用，强弱软虚，用到的场景  
0. 作用  
灵活的控制对象的生命周期提高对象的回收机率
1. 强引用（FinalReference）  
定义：指创建一个对象并它赋值给一个引用。  
特性：强引用只要有引用变量指向它们的时候，它们将不会被垃圾回收，即使在程序内存不足（OOM）的时候也不会被回收。如果想中断强引用和某个对象之间的关联，可以显示地将引用赋值为null，这样一来的话，JVM在合适的时间就会回收该对象。  
    ```
    String str = new String("str");
    ```
2. 软引用（SoftReference）  
特性：软引用在程序内存不足时，会被回收。  
使用场景：软引用可用来实现内存敏感的高速缓存,比如网页缓存、图片缓存等。使用软引用能防止内存泄露，增强程序的健壮性。  
    ```
    MyObject aRef = new  MyObject();  
    SoftReference aSoftRef=new SoftReference(aRef);   
    ```
3. 弱引用（WeakReference）  
特性：弱引用也是用来描述非必需对象的，当JVM进行垃圾回收时，无论内存是否充足，都会回收被弱引用关联的对象。  
使用场景：短时间缓存某些次要数据。
    ```
    WeakReference<String> wrf = new WeakReference<String>(str); 
    ```
4. 虚引用（PhantomReference）  
特性：如果一个对象与虚引用关联，则跟没有引用与之关联一样，在任何时候都可能被垃圾回收器回收。要注意的是，虚引用必须和引用队列关联使用，当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会把这个虚引用加入到与之关联的引用队列中。程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。如果程序发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。  
使用场景：主要用来跟踪对象被垃圾回收器回收的活动。  
    ```
    PhantomReference<String> prf = new PhantomReference<String>(new String("str"), new ReferenceQueue<>());
    ```
### 8. Hashcode的作用  
参数：无。  
返回值：返回对象的哈希码值（int）。根类Object的hashCode()方法的计算依赖于对象实例的D（内存地址）。  
作用：HashCode的存在主要是为了查找的快捷性，HashCode用来在散列存储结构中确定对象的存储地址。  
当集合要添加新的元素时，先调用这个元素的hashCode方法，就一下子能定位到它应该放置的在hash表中的位置上。如果这个位置上没有元素，它就可以直接存储在这个位置上，不用再进行任何比较了；如果这个位置上已经有元素了，就调用它的equals方法与新元素进行比较，相同的话就不存了，不相同就散列其它的地址。这样一来实际调用equals方法的次数就大大降低了，几乎只需要一两次。  
特点：equals()相等的两个对象，hashcode()一定相等；equals（）不相等的两个对象，却并不能证明他们的hashcode()不相等。  
注意：我们在重写了equals方法后，尽量也重写了hashcode方法。  
### 9. HashMap的hashcode的作用
1. hashCode的存在主要是用于查找的快捷性，如Hashtable，HashMap等，hashCode是用来在散列存储结构中确定对象的存储地址的；
2. 如果两个对象相同，就是适用于equals(java.lang.Object) 方法，那么这两个对象的hashCode一定要相同；
3. 如果对象的equals方法被重写，那么对象的hashCode也尽量重写，并且产生hashCode使用的对象，一定要和equals方法中使用的一致，否则就会违反上面提到的第2点；
4. 两个对象的hashCode相同，并不一定表示两个对象就相同，也就是不一定适用于equals(java.lang.Object)方法，只能够说明这两个对象在散列存储结构中，如Hashtable，他们“存放在同一个篮子里”。
### 10. 为什么重载hashCode方法？
只有当类需要放在HashTable、HashMap、HashSet等等hash结构的集合时才会重载hashCode。如果是object对象，必须重载hashCode和equal方法。equals()相等的两个对象，hashcode()一定相等；equals（）不相等的两个对象，却并不能证明他们的hashcode()不相等。所以重载equals()重载时，hashCode()必须重载。  
### 11. ArrayList、LinkedList、Vector的区别
1. 存储结构  
ArrayList和Vector是基于动态数组实现的，LinkedList是基于双向链表实现的。三者都继承自List接口  
2. 线程安全  
ArrayList和Vector的区别就是ArrayList是非线程安全的，Vector是线程安全的，Vector中的方法都是同步方法(synchronized)，所以ArrayList的执行效率要高于Vector，它也是用的最广泛的一种集合。  
LinkedList也是非线程安全的。  
3. 扩容机制  
从内部实现机制来讲，ArrayList和Vector都是使用Object的数组形式来存储的，当向这两种类型中增加元素的时候，若容量不够，需要进行扩容。ArrayList扩容后的容量是之前的1.5倍，然后把之前的数据拷贝到新建的数组中去。而Vector默认情况下扩容后的容量是之前的2倍。  
Vector可以设置容量增量，而ArrayList不可以。
4. 增删改查的效率  
ArrayList和Vector中，从指定的位置检索一个对象，或在集合的**末尾**插入、删除一个元素的时间是一样的，时间复杂度都是O(1)，但是如果在其他位置插入、删除元素花费的时间是O(n)。  
LinkedList中，在插入、删除时间复杂度都为O(1)，检索一个元素的时间复杂度为O(n)。
### 12. java.lang.String、StringBuffer与StringBuilder的区别  
- 区别——可变、线程安全  
String是不可变类。  
StringBuffer、StringBuilder都是可变类。
StringBuffer支持并发操作，线程安全的，适合多线程中使用。StringBuilder不支持并发操作，非线程安全的，不适合多线程中使用，但其在单线程中的性能比StringBuffer高。  
- 处理速度  
一般情况下，StringBuilder > StringBuffer > String。  
当String的字符串拼接发生在初始化时`String str="1"+"2"+"hello";`，在jvm中实际上不产生拼接的过程，故速度最快。  
在大多数实现中，StringBuilder > StringBuffer。  
### 13. Map、Set、List、Queue、Stack的特点与用法  

### 14. HashMap和HashTable的区别
### 15. JDK7与JDK8中HashMap的实现
### 16. HashMap和ConcurrentHashMap的区别，HashMap的底层源码
### 17. ConcurrentHashMap能完全替代HashTable吗
### 18. 为什么HashMap是线程不安全的
### 19. 如何线程安全的使用HashMap
### 20. 多并发情况下HashMap是否还会产生死循环
### 21. TreeMap、HashMap、LindedHashMap的区别
### 22. Collection包结构，与Collections的区别
### 23. try?catch?finally，try里有return，finally还执行么
### 24. Excption与Error包结构，OOM你遇到过哪些情况，SOF你遇到过哪些情况
### 25. Java(OOP)面向对象的三个特征与含义
### 26. Override和Overload的含义去区别
### 27. Interface与abstract类的区别
### 28. Static?class?与non?static?class的区别
### 29. java多态的实现原理
### 30. foreach与正常for循环效率对比
### 31. Java?IO与NIO
### 32. java反射的作用于原理
### 33. 泛型常用特点
### 34. 解析XML的几种方式的原理与特点：DOM、SAX
### 35. Java1.7与1.8,1.9,10 新特性
### 36. 设计模式：单例、工厂、适配器、责任链、观察者等等
### 37. JNI的使用  
### 38. AOP是什么
### 39. OOP是什么
### 40. AOP与OOP的区别