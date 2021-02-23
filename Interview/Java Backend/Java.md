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
只有当类需要放在HashTable、HashMap、HashSet等等hash结构的集合时才会重载hashCode。如果是object对象，必须重载hashCode和equal方法。  
equals()相等的两个对象，hashcode()一定相等；  
equals()不相等的两个对象，却并不能证明他们的hashcode()不相等。  
hashcode()不等，一定能推出equals()也不等；  
hashcode()相等，equals()可能相等，也可能不等。  
所以重载equals()重载时，hashCode()必须重载。  
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
- Map  
键映射到值的对象。一个映射不能包含重复的键；每个键最多只能映射到一个值。  
某些映射实现可明确保证其顺序，如 TreeMap 类；另一些映射实现则不保证顺序，如 HashMap 类。Map中元素，可以将key序列、value序列单独抽取出来。使用keySet()抽取key序列，将map中的所有keys生成一个Set。使用values()抽取value序列，将map中的所有values生成一个Collection。为什么一个生成Set，一个生成Collection？那是因为，key总是独一无二的，value允许重复。  
Map 同样对每个元素保存一份，但这是基于 " 键 " 的， Map 也有内置的排序，因而不关心元素添加的顺序。如果添加元素的顺序对你很重要，应该使用 LinkedHashSet 或者 LinkedHashMap。  
对于效率， Map 由于采用了哈希散列，查找元素时明显比 ArrayList 快。

- Set  
一个不包含重复元素的 collection。不可随机访问包含的元素。只能用Iterator实现单向遍历。Set 没有同步方法。只关心某元素是否属于 Set。  
- List  
可随机访问包含的元素，元素是有序的，可在任意位置增、删元素，不管访问多少次，元素位置不变，允许重复元素，用Iterator实现单向遍历，也可用ListIterator实现双向遍历。关心的是顺序。  
- Queue  
先进先出  
Queue使用时要尽量避免Collection的add()和remove()方法，而是要使用offer()来加入元素，使用poll()来获取并移出元素。它们的优点是通过返回值可以判断成功与否，add()和remove()方法在失败的时候会抛出异常。如果要使用前端而不移出该元素，使用element()或者peek()方法。值得注意的是LinkedList类实现了Queue接口，因此我们可以把LinkedList当成Queue来用。  
Queue 实现通常不允许插入 null 元素，尽管某些实现（如 LinkedList）并不禁止插入 null。即使在允许 null 的实现中，也不应该将 null 插入到 Queue 中，因为 null 也用作 poll 方法的一个特殊返回值，表明队列不包含元素。  
- Stack  
后进先出  
Stack继承自Vector（可增长的对象数组），也是同步的 
它通过五个操作对类 Vector 进行了扩展 ，允许将向量视为堆栈。它提供了通常的 push 和 pop 操作，以及取堆栈顶点的 peek 方法、测试堆栈是否为空的 empty 方法、在堆栈中查找项并确定到堆栈顶距离的 search 方法。  
- 总结  
  - Collection 是对象集合， Collection 有两个子接口 List 和 Set  
  - List 可以通过下标 (1,2..) 来取得值，值可以重复  
  ArrayList ， Vector ， LinkedList 是 List 的实现类  
ArrayList 是线程不安全的， Vector 是线程安全的，这两个类底层都是由数组实现的  
LinkedList 是线程不安全的，底层是由链表实现的   
  -  Set 只能通过游标来取值，并且值是不能重复的  
  - Map 是键值对集合  
HashTable 和 HashMap 是 Map 的实现类   
HashTable 是线程安全的，不能存储 null 值   
HashMap 不是线程安全的，可以存储 null 值  
  - Stack类：继承自Vector，实现一个后进先出的栈。提供了几个基本方法，push、pop、peak、empty、search等。  
  - Queue接口：提供了几个基本方法，offer、poll、peek等。已知实现类有LinkedList、PriorityQueue等。  
### 14. HashMap和HashTable的区别  
1. 父类  
HashMap是继承自AbstractMap类，而HashTable是继承自Dictionary（已被废弃，详情看源代码）。不过它们都实现了同时实现了map、Cloneable（可复制）、Serializable（可序列化）这三个接口。  
Hashtable比HashMap多提供了elments() 和contains() 两个方法。elments() 方法继承自Hashtable的父类Dictionnary。elements() 方法用于返回此Hashtable中的value的枚举。contains()方法判断该Hashtable是否包含传入的value。它的作用与containsValue()一致。事实上，contansValue() 就只是调用了一下contains() 方法。  
2. null值问题  
Hashtable既不支持Null key也不支持Null value。Hashtable的put()方法的注释中有说明。HashMap中，null可以作为键，但不可重复；可以有一个或多个键所对应的值为null。当get()方法返回null值时，可能是HashMap中没有该键，也可能使该键所对应的值为null。因此，在HashMap中不能由get()方法来判断HashMap中是否存在某个键， 而应该用containsKey()方法来判断。  
3. 线程安全性  
Hashtable是线程安全的，它的每个方法中都加入了Synchronize方法。在多线程并发的环境下，可以直接使用Hashtable，不需要自己为它的方法实现同步。  
HashMap不是线程安全的，在多线程并发的环境下，可能会产生死锁等问题。  
虽然HashMap不是线程安全的，但是它的效率会比Hashtable要好很多。这样设计是合理的。在我们的日常使用当中，大部分时间是单线程操作的。HashMap把这部分操作解放出来了。当需要多线程操作的时候可以使用线程安全的ConcurrentHashMap。ConcurrentHashMap虽然也是线程安全的，但是它的效率比Hashtable要高好多倍。因为ConcurrentHashMap使用了分段锁，并不对整个数据进行锁定。  
4. 遍历方式不同  
Hashtable、HashMap都使用了Iterator。而由于历史原因，Hashtable还使用了Enumeration的方式。  
HashMap的Iterator是fail-fast迭代器。当有其它线程改变了HashMap的结构（增加，删除，修改元素），将会抛出ConcurrentModificationException。不过，通过Iterator的remove()方法移除元素则不会抛出ConcurrentModificationException异常。但这并不是一个一定发生的行为，要看JVM。  
JDK8之前的版本中，Hashtable是没有fast-fail机制的。在JDK8及以后的版本中 ，Hashtable也是使用fast-fail的。  
5. 扩容方式不同  
Hashtable的初始长度是11，之后每次扩充容量变为之前的2n+1（n为上一次的长度）。  
而HashMap的初始长度为16，之后每次扩充变为原来的两倍。  
创建时，如果给定了容量初始值，那么Hashtable会直接使用你给定的大小，而HashMap会将其扩充为2的幂次方大小。  
6. 添加元素时计算哈希值的方法不同  
Hashtable直接使用对象的hashCode。  
HashMap使用的是key的hashcode 高低16位异或的结果。效率更高。  
[参考链接](https://www.cnblogs.com/javabg/p/7258550.html)
### 15. JDK7与JDK8中HashMap的实现  
1. 结构  
JDK7中HashMap采用的是位桶+链表的方式，即我们常说的散列链表的方式，而JDK8中采用的是位桶+链表/红黑树。JDK8中，当同一个hash值的节点数不小于8时（且数组长度必须大于等于MIN_TREEIFY_CAPACITY（64），否则继续采用扩容策略），将不再以单链表的形式存储了，会被调整成一颗红黑树。（树化操作的过程是将原本的单链表转化为双向链表，再遍历这个双向链表转化为红黑树）  
    - 红黑树是解决链表查询出现的O(n)情况,那么为什么不用其他树呢？  
    如平衡二叉树等,我们通过以下二方面分析：  
	  平均插入效率：链表>红黑树>平衡二叉树  
	平均查询效率：平衡二叉树>红黑树>链表  
可以看出红黑树介于二者之间,hashMap作为各种操作频繁的容器,自然选择综合性能较好的红黑树。  
    - 为什么阈值是6和8呢？  
	    1. 为什么8转红黑树?  
	红黑树的平均查找次数是log2(n)，  
		长度为8时:  
			红黑树平均查找次数为3，链表平均查找长度为8/2=4,此时选择红黑树优  
		长度为4为:  
			红黑树平均查找次数为2,链表平均长度为4/2=2,此时次数一样,红黑树开销大  
		至于567我们在这没有讨论的必要  
	  2. 为什么6转回链表？  
		若选择7,在7和8链表之间的增删元素,必然会导致频繁进行链表和红黑树的转换  
2. 节点表示  
JDK7中的HashMap底层节点叫Entry数组，JDK8中Entry的名字变成了Node，原因是和红黑树的实现TreeNode相关联。  
3. 数组的角标  
之前的indexFor()方法消失了，直接用(tab.length-1)&hash，所以看到这个，代表的就是数组的下角标。  
4. 新节点插入到链表是的插入顺序不同  
jdk7插入在头部，jdk8插入在尾部。（避免多线程时hashmap在jdk7的环形链表死循环问题）  
7. 扩容机制不同  
jdk1.7中resize，只有当 size>=threshold并且 table中的那个槽中已经有Entry时，才会发生resize。  
1.7扩容时需要重新计算哈希值和索引位置，1.8并不重新计算哈希值，巧妙地采用和扩容后容量进行&操作来计算新的索引位置。  
6. hash算法不同  
jdk8的hash函数变简单。jdk8之前之所以hash方法写的比较复杂，主要是为了提高散列行，进而提高遍历速度，但是jdk8以后引入红黑树后大大提高了遍历速度，继续采用复杂的hash算法也就没太大意义，反而还要消耗性能，因为不管是put()还是get()都需要调用hash()。  
[参考链接](https://yuanrengu.com/2020/ba184259.html)  
### 16. HashMap和ConcurrentHashMap的区别，HashMap的底层源码  
对ConcurrentHashMap，  
1. 底层采用分段的数组+链表实现，线程安全
2. 通过把整个Map分为N个Segment，可以提供相同的线程安全，但是效率提升N倍，默认提升16倍。(读操作不加锁，由于HashEntry的value变量是 volatile的，也能保证读取到最新的值。)ConcurrentHashMap默认将hash表分为16个桶，诸如get、put、remove等常用操作只锁住当前需要用到的桶。这样，原来只能一个线程进入，现在却能同时有16个写线程执行，并发性能的提升是显而易见的。
3. Hashtable的synchronized是针对整张Hash表的，即每次锁住整张表让线程独占，ConcurrentHashMap允许多个修改操作并发进行，其关键在于使用了锁分离技术
4. 有些方法需要跨段，比如size()和containsValue()，它们可能需要锁定整个表而而不仅仅是某个段，这需要按顺序锁定所有段，操作完毕后，又按顺序释放所有段的锁
5. 扩容：段内扩容（段内元素超过该段对应Entry数组长度的75%触发扩容，不会对整个Map进行扩容），插入前检测需不需要扩容，有效避免无效扩容
### 17. ConcurrentHashMap能完全替代HashTable吗  
HashTable虽然性能上不如ConcurrentHashMap，但并不能完全被取代，两者的迭代器的一致性不同的，HashTable的迭代器是强一致性的，而ConcurrentHashMap是弱一致的。 ConcurrentHashMap的get，clear，iterator 都是弱一致性的。   
jdk1.7中是分段锁   
- 正是因为get操作几乎所有时候都是一个无锁操作（get中有一个readValueUnderLock调用，不过这句执行到的几率极小），使得同一个Segment实例上的put和get可以同时进行，这就是get操作是弱一致的根本原因。  
- 因为没有全局的锁，在清除完一个segments之后，正在清理下一个segments的时候，已经清理segments可能又被加入了数据，因此clear返回的时候，ConcurrentHashMap中是可能存在数据的。  
- 在遍历过程中，如果已经遍历的数组上的内容变化了，迭代器不会抛出ConcurrentModificationException异常。如果未遍历的数组上的内容发生了变化，则有可能反映到迭代过程中。这就是ConcurrentHashMap迭代器弱一致的表现。  
jdk1.8中优化了（[参考链接](https://baijiahao.baidu.com/s?id=1617089947709260129&wfr=spider&for=pc)）：  
数据结构：取消了 Segment 分段锁的数据结构，取而代之的是数组+链表+红黑树的结构。  
保证线程安全机制：JDK1.7 采用 Segment 的分段锁机制实现线程安全，其中 Segment 继承自 ReentrantLock 。JDK1.8 采用CAS+synchronized 保证线程安全。  
锁的粒度：JDK1.7 是对需要进行数据操作的 Segment 加锁，JDK1.8 调整为对每个数组元素加锁（Node）。  
链表转化为红黑树：定位节点的 hash 算法简化会带来弊端，hash 冲突加剧，因此在链表节点数量大于 8（且数据总量大于等于 64）时，会将链表转化为红黑树进行存储。   
### 18. 为什么HashMap是线程不安全的  
1. 在JDK1.7中，当并发执行扩容操作时会造成环形链([参考链接](https://blog.csdn.net/swpu_ocean/article/details/88917958?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&dist_request_id=1cbd1e19-0f4f-4996-b8ac-dd76cfb13cc8&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control))和数据覆盖的情况。  
2. 在JDK1.8中，在并发执行put操作时会发生数据覆盖的情况。
### 19. 如何线程安全的使用HashMap
1. Hashtable  
使用 synchronized 来保证线程安全。
2. ConcurrentHashMap
3. Collections.synchronizedMap(hashMap)   
调用 synchronizedMap() 方法后会返回一个 SynchronizedMap 类的对象，而在 SynchronizedMap 类中使用了 synchronized 同步关键字来保证对 Map 的操作是线程安全的。
### 20. 多并发情况下HashMap是否还会产生死循环  
jdk1.8中，采用了尾插法更新数据，避免了死循环。但仍存在数据丢失的问题。
### 21. TreeMap、HashMap、LindedHashMap的区别  
- Hashmap
根据键的HashCode 值存储数据，根据键可以直接获取它的值，具有很快的访问速度，遍历时，取得数据的顺序是完全随机的。  
HashMap最多只允许一条记录的键为Null，不允许多条记录的值为 Null;  
HashMap不支持线程的同步。
- LinkedHashMap  
保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先进先出原则。也可以在构造时用带参数，按照应用次数排序。  
在遍历的时候会比HashMap慢，不过有种情况例外，当HashMap容量很大，实际数据较少时，遍历起来可能会比LinkedHashMap慢，因为LinkedHashMap的遍历速度只和实际数据有关，和容量无关，而HashMap的遍历速度和他的容量有关。  
- TreeMap  
实现了SortMap接口，能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器。当用Iterator遍历TreeMap时，得到的记录是排过序的。（key必须Key必须实现Comparable接口，或者传入Comparator。比较器通过compare()方法定义。）  
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