# 多线程
### 1. 什么是线程？ 
线程是一个轻量级的子进程。  
进程和线程都是一个时间段的描述，是CPU工作时间段的描述，不过是颗粒大小不同。  
### 2. 什么是线程安全和线程不安全？ 
1. 线程安全：指多个线程在执行同一段代码的时候采用加锁机制，使每次的执行结果和单线程执行的结果都是一样的，不存在执行程序时出现意外结果。 
2. 线程不安全：是指不提供加锁机制保护，有可能出现多个线程先后更改数据造成所得到的数据是脏数据。
### 3. 什么是自旋锁？ 
自旋锁（spinlock）：是指当一个线程在获取锁的时候，如果锁已经被其它线程获取，那么该线程将循环等待，然后不断的判断锁是否能够被成功获取，直到获取到锁才会退出循环。  
获取锁的线程一直处于活跃状态，但是并没有执行任何有效的任务，使用这种锁会造成busy-waiting。  
它是为实现保护共享资源而提出一种锁机制。其实，自旋锁与互斥锁比较类似，它们都是为了解决对某项资源的互斥使用。无论是互斥锁，还是自旋锁，在任何时刻，最多只能有一个保持者，也就说，在任何时刻最多只能有一个执行单元获得锁。但是两者在调度机制上略有不同。对于互斥锁，如果资源已经被占用，资源申请者只能进入睡眠状态。但是自旋锁不会引起调用者睡眠，如果自旋锁已经被别的执行单元保持，调用者就一直循环在那里看是否该自旋锁的保持者已经释放了锁，”自旋”一词就是因此而得名。  
### 4. 什么是Java内存模型？ 
[Reference](https://blog.csdn.net/javazejian/article/details/72772461?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control&dist_request_id=1328760.1068.16171809566525093&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control)  
内存模型可以理解为在特定的操作协议下，对特定的内存或者高速缓存进行读写访问的过程抽象描述，不同架构下的物理机拥有不一样的内存模型，Java虚拟机是一个实现了跨平台的虚拟系统，因此它也有自己的内存模型，即Java内存模型（Java Memory Model, JMM）。  
内存模型描述了程序中各个变量（实例域、静态域和数组元素）之间的关系，以及在实际计算机系统中将变量存储到内存和从内存中取出变量这样的底层细节。  
JMM规定了所有的变量都存储在主内存（Main Memory）中。每个线程还有自己的工作内存（Working Memory），线程的工作内存中保存了该线程使用到的变量的主内存的副本拷贝，线程对变量的所有操作（读取、赋值等）都必须在工作内存中进行，而不能直接读写主内存中的变量（volatile变量仍然有工作内存的拷贝，但是由于它特殊的操作顺序性规定，所以看起来如同直接在主内存中读写访问一般）。不同的线程之间也无法直接访问对方工作内存中的变量，线程之间值的传递都需要通过主内存来完成。  

### 5. 什么是CAS？ 
- 概述  
CAS是英文单词Compare and Swap的缩写，翻译过来就是比较并替换。  
CAS机制中使用了3个基本操作数：内存地址V，旧的预期值A，要修改的新值B。  
更新一个变量的时候，只有当变量的预期值A和内存地址V当中的实际值相同时，才会将内存地址V对应的值修改为B。
- 缺点  
  1. CPU开销过大  
  在并发量比较高的情况下，如果许多线程反复尝试更新某一个变量，却又一直更新不成功，循环往复，会给CPU带来很到的压力。  
  2. 不能保证代码块的原子性  
  CAS机制所保证的知识一个变量的原子性操作，而不能保证整个代码块的原子性。比如需要保证3个变量共同进行原子性的更新，就不得不使用synchronized了。  
  3. ABA问题  
  前后判断一致，但实际上中间过程变量被修改过，即A变成B又变回A，CAS判断为正确。  
### 6. 什么是乐观锁和悲观锁？ 
- 乐观锁：  
  乐观锁其实不会上锁。顾名思义，很乐观，它默认别的线程不会修改数据，所以不会上锁。  
  Java中java.util.concurrent.atomic包下面的原子变量类就是使用了乐观锁的一种实现方式CAS实现的。
- 悲观锁：  
顾名思义，很悲观，就是每次拿数据的时候都认为别的线程会修改数据，所以在每次拿的时候都会给数据上锁。  
Java中synchronized和ReentrantLock等独占锁就是悲观锁思想的实现。
### 7. 什么是AQS？ 
AQS全称"AbstractQueuedSynchronized"意为抽象队列同步器。我们在JUC中常用的ReentrantLock、CountDownLatch底层都是基于AQS来实现的加锁和释放锁等功能。它用一个volatile int类型的变量state表示同步状态，并提供了一系列的CAS操作来管理这个同步状态。  
### 8. 什么是原子操作？在Java Concurrency API中有哪些原子类(atomic classes)？ 
原子操作（atomic operation）意为”不可被中断的一个或一系列操作” 。处理器使用基于对缓存加锁或总线加锁的方式来实现多处理器之间的原子操作。在Java中可以通过锁和循环CAS的方式来实现原子操作。  
1. 原子类：AtomicBoolean，AtomicInteger，AtomicLong，AtomicReference  
2. 原子数组：AtomicIntegerArray，AtomicLongArray，AtomicReferenceArray  
3. 原子属性更新器：AtomicLongFieldUpdater，AtomicIntegerFieldUpdater，AtomicReferenceFieldUpdater  
4. 解决ABA问题的原子类：AtomicMarkableReference（通过引入一个boolean来反映中间有没有变过），AtomicStampedReference（通过引入一个int来累加来反映中间有没有变过）
### 9. 什么是Executors框架？ 
Executors 框架是一个根据一组执行策略调用，调度，执行和控制的异步任务的框架。无限制的创建线程会引起应用程序内存溢出。所以创 建一个线程池是个更好的的解决方案，因为可以限制线程的数量并且可以回收再利用这些线程。利用Executors 框架可以非常方便的创建一 个线程池。  
- Executors 和 Executor 的区别  
  Executors 工具类的不同方法按照我们的需求创建了不同的线程池，来满足业务的需求。  
  Executor 接口对象能执行我们的线程任务。
### 10. 什么是阻塞队列？如何使用阻塞队列来实现生产者-消费者模型？ 
- 概念：阻塞队列是一个在队列基础上又支持了两个附加操作的队列。  
    1. 支持阻塞的插入方法：队列满时，队列会阻塞插入元素的线程，直到队列不满。  
    2. 支持阻塞的移除方法：队列空时，获取元素的线程会等待队列变为非空。
- 线程阻塞的两种情况：
  1. 当队列中没有数据的情况下，消费者端的所有线程都会被自动阻塞（挂起），直到有数据放 入队列。
  2. 当队列中填满数据的情况下，生产者端的所有线程都会被自动阻塞（挂起），直到队列中有 空的位置，线程被自动唤醒。  
- 如何使用阻塞队列来实现生产者-消费者模型？  
  通知模式实现：所谓通知模式，就是当生产者往满的队列里添加元素时会阻塞住生产者，当消费者消费了一个队列中的元素后，会通知生产者当前队列可用。  
  任何有效的生产者-消费者问题解决方案都是通过控制生产者put()方法（生产资源）和消费者take()方法（消费资源）的调用来实现的，一旦你实现了对方法的阻塞控制，那么你将解决该问题。  
  Java通过BlockingQueue提供了开箱即用的支持来控制这些方法的调用（一个线程创建资源，另一个消费资源）。java.util.concurrent包下的BlockingQueue接口是一个线程安全的可用于存取对象的队列。  
  Consumer用take()方法取数，Producer用put()方法放数。
### 11. 什么是Callable和Future?   
在 Java 中为了编程异步事件，我们使用 Thread 类和 Runnable 接口，它们可以开发并行应用程序。问题是在执行结束时不能返回值。因此，添加了 FutureTaks，Future 和 Callable 类，它们与以前的类具有大致相同的功能，但极大地促进了并行应用程序的开发。由于线程 Thread 只支持 Runnable 构造，于是有了 Future 可以根据 Callable 构建线程。由于 Future 只是一个接口，无法直接创建对象，因此有了 FutureTask 。  
Callable 接口类似于 Runnable，从名字就可以看出来了，但是 Runnable 不会返回结果，并且无法抛出返回结果的异常，而 Callable 功能更强大一些，被线程执行后，可以返回异常，也可以返回值，这个返回值可以被 Future 拿到，也就是说，Future 可以拿到异步执行任务的返回值。可以认为是带有回调的 Runnable。 Future 接口表示异步任务，是还没有完成的任务给出的未来结果。所以说 Callable 用于产生结果，Future 用于获取结果。
### 12. 什么是FutureTask? 
FutureTask表示一个可以取消的异步运算。它有启动和取消运算、查询运算是否完成和取回运算结果等方法。只有当运算完成的时候结果才能取回，如果运算尚未完成get方法将会阻塞。一个FutureTask对象可以对调用了Callable和Runnable的对象进行包装，由于FutureTask也是调用了Runnable接口所以它可以提交给Executor来执行。
### 14. 什么是多线程？优缺点？ 
- 多线程指的是在单个程序中可以同时运行多个不同的线程，执行不同的任务
- - 优点：  
    1. 提升性能
    2. 充分利用计算机资源
  - 缺点：  
    1. 不一定比单线程快
    2. 上下文切换开销
    3. 线程安全，可能导致死锁。
### 15. 什么是多线程的上下文切换？ 
CPU通过时间片段的算法来循环执行线程任务，而循环执行即每个线程允许运行的时间后的切换，而这种循环的切换使各个程序从表面上看是同时进行的。而切换时会保存之前的线程任务状态，当切换到该线程任务的时候，会重新加载该线程的任务状态。而这个从保存到加载的过程称之为上下文切换。  
### 16. ThreadLocal的设计理念与作用？
- 作用：在JDK的早期版本中,提供了一种解决多线程并发问题的方案: java.lang.ThreadLocal类.ThreadLocal类在维护变量时,实际使用了当前线程（Thread）中的一个叫做ThreadLocalMap的独立副本,每个线程可以独立修改属于自己的副本而不会互相影响,从而隔离了线程和线程,避免了线程访问实例变量发生冲突的问题.
- 原理：每个线程中都会维护一个ThreadLocalMap，当在某个线程中访问时，会取出这个线程自己的Map并且用当前ThreadLocal对象做索引来取出相对应的Value值，从而达到不同线程不同值的效果。
### 17. ThreadPool（线程池）用法与优势？ 
- 优势
  1. 降低资源消耗。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。
  2. 提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行。
  3. 提高线程的可管理性。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。
- 用法  
[Reference](https://zhuanlan.zhihu.com/p/357069665)  
### 18. Concurrent包里的其他东西：ArrayBlockingQueue、CountDownLatch等等。 
- ArrayBlockingQueue：规定大小的BlockingQueue，其构造函数必须带一个int参数来指明其大小。其所含的对象是以FIFO（先入先出）顺序排序的。   
- countdownLatch：CountDownLatch的一个非常典型的应用场景是：有一个任务想要往下执行，但必须要等到其他的任务执行完毕后才可以继续往下执行。假如我们这个想要继续往下执行的任务调用一个CountDownLatch对象的await()方法，其他的任务执行完自己的任务后调用同一个CountDownLatch对象上的countDown()方法，这个调用await()方法的任务将一直阻塞等待，直到这个CountDownLatch对象的计数值减到0为止。
### 19. synchronized和ReentrantLock的区别？ 
1. 这两种方式最大区别就是对于Synchronized来说，它是java语言的关键字，是原生语法层面的互斥，需要jvm实现。而ReentrantLock它是JDK 1.5之后提供的API层面的互斥锁，需要lock()和unlock()方法配合try/finally语句块来完成。
2. synchronized既可以修饰方法，也可以修饰代码块。
3. 等待可中断，持有锁的线程长期不释放的时候，正在等待的线程可以选择放弃等待，这相当于Synchronized来说可以避免出现死锁的情况。
4. 公平锁，多个线程等待同一个锁时，必须按照申请锁的时间顺序获得锁，Synchronized锁非公平锁，ReentrantLock默认的构造函数是创建的非公平锁，可以通过参数isFair设为公平锁，但公平锁表现的性能不是很好。
5. 锁绑定多个条件，一个ReentrantLock对象可以同时绑定对个对象。
### 20. Semaphore有什么作用？ 
Semaphore 是一种基于计数的信号量。它可以设定一个阈值，基于此，多个线程竞争获取许可信 号，做完自己的申请后归还，超过阈值后，线程申请许可信号将会被阻塞。 Semaphore 可以用来 构建一些对象池，资源池之类的， 比如数据库连接池
### 21. Java Concurrency API中的Lock接口(Lock interface)是什么？对比同步它有什么优势？ 
- Lock 接口比同步方法和同步块提供了更具扩展性的锁操作。他们允许更灵活的结构，可以具有完全不同的性质，并且可以支持多个相关类的条件对象。
- 它的优势有：
  1. 可以使锁更公平
  2. 可以使线程在等待锁的时候响应中断
  3. 可以让线程尝试获取锁，并在无法获取锁的时候立即返回或者等待一段时间
  4. 可以在不同的范围，以不同的顺序获取和释放锁  
- 整体上来说 Lock 是 synchronized 的扩展版，Lock 提供了无条件的、可轮询的(tryLock 方法)、定时的(tryLock 带参方法)、可中断的 (lockInterruptibly)、可多条件队列的(newCondition 方法)锁操作。另外 Lock 的实现类基本都支持非公平锁(默认)和公平锁，synchronized 只支持非公平锁，当然，在大部分情况下，非公平锁是高效的选择。
### 22. Hashtable的size()方法中明明只有一条语句”return count”，为什么还要做同步？ 
1. 对于类的非同步方法，可以多条线程同时访问。保证数据一致。
2. 指令重排问题。
### 23. ConcurrentHashMap的并发度是什么？ 
分的segment的个数，默认为16。
### 24. ReentrantReadWriteLock读写锁的使用？ 
- 原则
  1. 当一个线程对资源进行写操作的时候,别的线程既不能对资源读也不能对资源写。
  2. 当一个线程对资源进行读操作的时候,别的线程不能对资源写。
  3. 当一个线程对资源进行读操作的时候,别的线程能对资源读。
- 使用：调用对象的readLock()或writeLock()下的lock()和unlock()
### 25. CyclicBarrier和CountDownLatch的用法及区别？ 
- countdownLatch：CountDownLatch的一个非常典型的应用场景是：有一个任务想要往下执行，但必须要等到其他的任务执行完毕后才可以继续往下执行。假如我们这个想要继续往下执行的任务调用一个CountDownLatch对象的await()方法，其他的任务执行完自己的任务后调用同一个CountDownLatch对象上的countDown()方法，这个调用await()方法的任务将一直阻塞等待，直到这个CountDownLatch对象的计数值减到0为止。（当每个线程调用countdown方法直到将countdownlatch方法创建时数减为0时，那么之前调用await（）方法的线程才会继续执行。有一点注意，那就是只执行一次，不能到0以后重新执行） 
- CyclicBarrier：类有一个整数初始值，此值表示将在同一点同步的线程数量。当其中一个线程到达确定点，它会调用await() 方法来等待其他线程。当线程调用这个方法，CyclicBarrier阻塞线程进入休眠直到其他线程到达。当最后一个线程调用CyclicBarrier 类的await() 方法，它唤醒所有等待的线程并继续执行它们的任务。（当等待的线程数量达到CyclicBarrier线程指定的数量以后（调用await方法的线程数），才一起往下执行，否则大家都在等待，注意：如果达到指定的线程数量的时候：则可以重新计数，上面的过程可以循环） 
- CountDownLatch是减计数方式，计数==0时释放所有等待的线程；CyclicBarrier是加计数方式，计数达到构造方法中参数指定的值时释放所有等待的线程。 
### 26. LockSupport工具？ 
- LockSupport是个工具类，它的主要作用是挂起park()和唤醒线程unpark()。
### 27. Condition接口及其实现原理？ 
- 在java1.5之前，线程之间的通信主要通过notify和wait。而Condition支持多路等待，就是定义多个Condition，每个Condition控制一个支路，典型问题生产者和消费者问题现在可以通过这个接口来进行优化。
- ConditionObject是同步器AbstractQueuedSynchronizer的内部类
### 28. Fork/Join框架的理解? 
[Reference](https://www.cnblogs.com/senlinyang/p/7885964.html)
- Fork/Join框架是Java 7提供的一个用于并行执行任务的框架，是一个把大任务分割成若干个小任务，最终汇总每个小任务结果后得到大任务结果的框架。
- fork/join框架与其它ExecutorService的实现类相似，会给线程池中的线程分发任务，不同之处在于它使用了工作窃取算法，所谓工作窃取，指的是对那些处理完自身任务的线程，会从其它线程窃取任务执行。
  1. 任务分割：首先Fork/Join框架需要把大的任务分割成足够小的子任务，如果子任务比较大的话还要对子任务进行继续分割2
  2. 执行任务并合并结果：分割的子任务分别放到双端队列里，然后几个启动线程分别从双端队列里获取任务执行。子任务执行完的结果都放在另外一个队列里，启动一个线程从队列里取数据，然后合并这些数据。
### 29. wait()和sleep()的区别?  
||wait|	sleep|
|:--|:--|:---|
|同步|	只能在同步上下文中调用wait方法，否则或抛出IllegalMonitorStateException异常|	不需要在同步方法或同步块中调用|
|作用对象|	wait方法定义在Object类中，作用于对象本身|	sleep方法定义在java.lang.Thread中，作用于当前线程|
|释放锁资源|	是|	否|
|唤醒条件|	其他线程调用对象的notify()或者notifyAll()方法	|超时或者调用interrupt()方法体
|方法属性|	wait是实例方法|	sleep是静态方法|

[Reference](https://blog.csdn.net/weixin_39843989/article/details/90294825)
### 30. 线程的五个状态（五种状态，创建、就绪、运行、阻塞和死亡）? 
1. 新建(NEW)：新创建了一个线程对象。

2. 一个新创建的线程并不自动开始运行，要执行线程，必须调用线程的start()方法。当线程对象调用start()方法即启动了线程，start()方法创建线程运行的系统资源，并调度线程运行run()方法。当start()方法返回后，线程就处于就绪状态。  
处于就绪状态的线程并不一定立即运行run()方法，线程还必须同其他线程竞争CPU时间，只有获得CPU时间才可以运行线程。因为在单CPU的计算机系统中，不可能同时运行多个线程，一个时刻仅有一个线程处于运行状态。因此此时可能有多个线程处于就绪状态。对多个处于就绪状态的线程是由Java运行时系统的线程调度程序(thread scheduler)来调度的。
1. 运行(RUNNING)：可运行状态(runnable)的线程获得了cpu 时间片（timeslice） ，执行程序代码。
2. 阻塞(BLOCKED)：阻塞状态是指线程因为某种原因**放弃了cpu 使用权**，也即让出了cpu timeslice，暂时停止运行。直到线程进入可运行(runnable)状态，才有机会再次获得cpu timeslice 转到运行(running)状态。阻塞的情况分三种： 
   1. 等待阻塞：运行(running)的线程执行o.wait()方法，JVM会把该线程放入等待队列(waitting queue)中。
   2. 同步阻塞：运行(running)的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放入锁池(lock pool)中。
   3. 其他阻塞：运行(running)的线程执行Thread.sleep(long ms)或t.join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入可运行(runnable)状态。
3. 死亡(DEAD)：线程run()、main() 方法执行结束，或者因异常退出了run()方法，则该线程结束生命周期。死亡的线程不可再次复生。
### 31. start()方法和run()方法的区别？ 
通过调用线程类的start()方法来启动一个线程，使线程处于就绪状态，即可以被JVM来调度执行，在调度过程中，JVM通过调用线程类的run()方法来完成实际的业务逻辑，当run()方法结束后，此线程就会终止。  
如果直接调用线程类的run()方法，会被当作一个普通的函数调用，程序中仍然只有主线程这一个线程。即start()方法能够异步的调用run()方法，但是直接调用run()方法却是同步的，无法达到多线程的目的。  
调用start（）后，线程会被放到等待队列，等待CPU调度， 并不一定要马上开始执行，只是将这个线程置于可动行状态。然后通过JVM，线程Thread会调用run（）方法，执行本线程的线程体。
### 32. Runnable接口和Callable接口的区别？ 
Runnable接口中的run()方法的返回值是void，它做的事情只是纯粹地去执行run()方法中的代码而已；Callable接口中的call()方法是有返回值的，是一个泛型，和Future、FutureTask配合可以用来获取异步执行的结果。  
### 33. volatile关键字的作用？ 
1. 保持内存可见性
内存可见性（Memory Visibility）：所有线程都能看到共享内存的最新状态。
2. 防止指令重排
### 34. Java中如何获取到线程dump文件？ 
- 在 Linux 下，你可以通过命令 kill -3 PID （Java 进程的进程 ID）来获取 Java
应用的 dump 文件。  
- 在 Windows 下，你可以按下 Ctrl + Break 来获取。
### 35. 线程和进程有什么区别？ 
- 进程是资源分配的最小单位，线程是CPU调度的最小单位.
- 进程和线程都是一个时间段的描述，是CPU工作时间段的描述，不过是颗粒大小不同。
- CPU在执行的时候仅仅切换线程的上下文，而没有进行进程上下文切换的。进程的上下文切换的时间开销是远远大于线程上下文时间的开销。这样就让CPU的有效使用率得到提高。
### 36. 线程实现的方式有几种（四种）？ 
1. 继承Thread类； 
2. 实现Runnable接口； 
3. 实现Callable接口通过FutureTask包装器来创建Thread线程； 
4. 利用ExecutorService线程池的方式创建线程（也就是使用了ExecutorService来管理前面的三种方式）。
### 37. 高并发、任务执行时间短的业务怎样使用线程池？并发不高、任务执行时间长的业务怎样使用线程池？并发高、业务执行时间长的业务怎样使用线程池？ 
1. 高并发、任务执行时间短的业务，线程池线程数可以设置为CPU核数+1，减少线程上下文的切换
2. 并发不高、任务执行时间长的业务要区分开看：
     - 假如是业务时间长集中在IO操作上，也就是IO密集型的任务，因为IO操作并不占用CPU，所以不要让所有的CPU闲下来，可以加大线程池中的线程数目，让CPU处理更多的业务
     - 假如是业务时间长集中在计算操作上，也就是计算密集型任务，这个就没办法了，和（1）一样吧，线程池中的线程数设置得少一些，减少线程上下文的切换
3. 并发高、业务执行时间长，解决这种类型任务的关键不在于线程池而在于整体架构的设计，看看这些业务里面某些数据是否能做缓存是第一步，增加服务器是第二步，至于线程池的设置，设置参考（2）。最后，业务执行时间长的问题，也可能需要分析一下，看看能不能使用中间件对任务进行拆分和解耦。
### 38. 如果你提交任务时，线程池队列已满，这时会发生什么？ 
- 如果使用的是无界队列 LinkedBlockingQueue，也就是无界队列的话，没关系，继续添加任务到阻塞队列中等待执行，因为 LinkedBlockingQueue 可以近乎认为是一个无穷大的队列，可以无限存放任务
- 如果使用的是有界队列比如 ArrayBlockingQueue，任务首先会被添加到ArrayBlockingQueue 中，ArrayBlockingQueue 满了，会根据maximumPoolSize 的值增加线程数量；  
  如果增加了线程数量还是处理不过来，ArrayBlockingQueue 继续满，那么则会使用拒绝策略RejectedExecutionHandler 处理满了的任务，默认是 AbortPolicy
### 39. 锁的等级：方法锁、对象锁、类锁? 
1. 方法锁:通过在方法声明中加入 synchronized关键字来声明 synchronized 方法。
2. 对象锁（实例对象锁）:当一个对象中有synchronized method或synchronized block的时候调用此对象的同步方法或进入其同步区域时，就必须先获得对象锁。如果此对象的对象锁已被其他调用者占用，则需要等待此锁被释放。（方法锁也是对象锁）
```
public class Test {
    // 对象锁：形式1(方法锁) 
    public synchronized void Method1(){ 
        System.out.println("我是对象锁也是方法锁"); 
        try{ 
            Thread.sleep(500); 
        } catch (InterruptedException e){ 
            e.printStackTrace(); 
        } 
 
    } 
 
    // 对象锁：形式2（代码块形式） 
    public void Method2(){ 
        synchronized (this){ 
            System.out.println("我是对象锁"); 
            try{ 
                Thread.sleep(500); 
            } catch (InterruptedException e){ 
                e.printStackTrace(); 
            } 
        } 
 
    } 
}
```
3. 类锁（Class对象锁）:类锁即synchronized修饰静态的方法或代码块。由于一个class不论被实例化多少次，其中的静态方法和静态变量在内存中都只有一份。所以，一旦一个静态的方法被申明为synchronized。此类所有的实例化对象在调用此方法，共用同一把锁，我们称之为类锁。
### 40. 如果同步块内的线程抛出异常会发生什么？ 
无论同步块是正常还是异常退出的，里面的线程都会释放锁
### 41. 并发编程（concurrency）并行编程（parallellism）有什么区别？ 
并行是指两个或者多个事件在同一时刻发生；而并发是指两个或多个事件在同一时间间隔发生。
### 42. 如何保证多线程下 i++ 结果正确？ 
1. CAS 声明Atomic变量，使用getAndIncrement()进行自增
2. 使用锁
3. 使用synchronized
### 43. 一个线程如果出现了运行时异常会怎么样? 
[Reference](https://blog.csdn.net/Fly_as_tadpole/article/details/98344574)  
- 如果异常没有被捕获该线程将会停止执行。 
- 如果该异常被捕获或抛出，则程序继续运行。 
  当一个未捕获异常将造成线程中断的时候JVM会使用Thread.getUncaughtExceptionHandler()来查询线程的UncaughtExceptionHandler
### 44. 如何在两个线程之间共享数据? 
[Reference](https://blog.csdn.net/hejingyuan6/article/details/47053409)  
1. 如果每个线程执行的代码相同，可以使用同一个Runnable对象，这个Runnable对象中有那个共享数据，例如，卖票系统就可以这么做。
2. 如果每个线程执行的代码不同，这时候需要用不同的Runnable对象，例如，设计4个线程。其中两个线程每次对j增加1，另外两个线程对j每次减1，银行存取款
### 45. 生产者消费者模型的作用是什么? 
1. 通过平衡生产者的生产能力和消费者的消费能力来提升整个系统的运行效率，这个是生产者消费者模型最重要的作用。
2. 解耦，这是生产者消费者模型附带的作用，解耦意味着生产者和消费者之间的联系少，联系越少越可以独自发展而不需要收到相互的制约。
3. 异步，生产者只需要将消息放到队列中，直接就可以进行其他的操作。而消费者则只负责从消息队列中取出消息进行后续的逻辑处理，当然在实际的场景中线程池也可以满足异步并发的功能，这个也不算是主要作用。
### 46. 怎么唤醒一个阻塞的线程? 
1. 同步阻塞：等待锁的释放
2. 等待阻塞：
   1. 使用Thread.sleep造成的阻塞:时间结束后自动进入RUNNABLE状态
   2. 使用Thread.wait造成的阻塞:使用Thread.notify或者Thread.notifyAll唤醒
   3. 使用Thread.join造成的阻塞:等待上一个线程执行完后自动进入RUNNABLE状态
   4. 使用Thread.suspend造成的阻塞:使用Thread.resum唤醒
   5. 使用LockSupport.park造成的阻塞:使用LockSupport.unpark唤醒
   6. 使用LockSupport.parkNanos造成的阻塞:指定时间结束后，自动唤醒
   7. 使用LockSupport.parkUntil造成的阻塞:到达指定的时间，自动唤醒
### 47. Java中用到的线程调度算法是什么 
Java虚拟机采用抢占式调度模型，是指优先让可运行池中优先级高的线程占用CPU，如果可运行池中的线程优先级相同，那么就随机选择一个线程，使其占用CPU。处于运行状态的线程会一直运行，直至它不得不放弃 CPU。
### 48. 单例模式的线程安全性? 
[ref](https://blog.csdn.net/cselmu9/article/details/51366946#)
- 饿汉式是线程安全的。
- 懒汉式是非线程安全的，优化方法
  1. 对方法进行synchronized同步
   ```
	public synchronized static MySingleton getInstance(){}
  ```
  2. 同步代码块
   ```
   synchronized (MySingleton.class) {
				if(instance != null){}
  ```
  3. 双重校验锁
  ```
  volatile private static MySingleton instance = null;
  synchronized (MySingleton.class) {
					if(instance == null){//二次检查
						instance = new MySingleton();
					}
  ```
  4. 使用静态内置类实现单例模式
  5. 使用static代码块实现单例
### 49. 线程类的构造方法、静态块是被哪个线程调用的? 
线程类的构造方法、静态块是被new这个线程类所在的线程所调用的，而run方法里面的代码才是被线程自身所调用的。
### 50. 同步方法和同步块，哪个是更好的选择? 
同步块是更好的选择，因为它不会锁住整个对象（当然你也可以让它锁住整个对象）。同步方法会锁住整个对象，哪怕这个类中有多个不相关联的同步块，这通常会导致他们停止执行并需要等待获得这个对象上的锁。同步块更要符合开放调用的原则，只在需要锁住的代码块锁住相应的对象，这样从侧面来说也可以避免死锁。
### 51. 如何检测死锁？怎么预防死锁？
- 检查
  1. 通过jdk工具jps、jstack排查死锁问题
  2. 通过jdk提供的工具jconsole排查死锁问题
  3. 通过jdk提供的工具VisualVM排查死锁问题
- 预防
  1. 使用tryLock的方法设置超时时间
  2. 按照顺序加锁是一种有效的死锁预防机制。但是，这种方式需要你事先知道所有可能会用到的锁
  3. 死锁检测

![](https://github.com/ni-jeff/Notebooks/blob/main/Interview/Java_Backend/MultiThread.png)