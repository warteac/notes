## Java 复习总结

### 1. 数据类型
- 基础数据类型：8种 byte（1字节）、short、char（2字节）、int、float（4字节）、long、double（8字节）、boolean（1位）
- 引用数据类型：3种 类（Class）接口（Interface）数组（Array）
  - Object：所有类的超类，包括数组
  - String
  - Date: Calender 
  - Void: 不可实例化的占位符类，它保持一个对象Java关键字void的class对象的引用
  - 8种基础数据类型的包装类：Byte、Short、Character、Integer、Float、Double、Long、Boolean 都集成自Number类

### 2. JVM 内存机制
![](http://i.imgur.com/go3PAND.png)

- 堆区：一个jvm只有一个堆区，是jvm所管理的内存中最大的一块，是所有线程共享的，在虚拟机启动时创建，存放对象实例
- 方法区，是堆区的一个部分，存放常量、静态变量等
- 栈区：线程私有，生命周期与线程相同，每个方法被执行的时候会创建一个栈帧，存储局部变量表、方法出口等信息
- GC：主要在堆区，把对象分为年轻代、老年代、持久代，采用不同的算法进行回收。

### 3. List
- 有序的、可重复的
- ArrayList: 底层是数组结构、查询快，增删慢，线程不同步的
- LinkedList: 底层是链表结构，查询慢，增删快

### 4. Map<K,V>
- HashMap:底层是哈希表，null可以作为键或者值，线程不同步的（运行效率高）
- TreeMap:底层是二叉树结构，会进行一个排序，线程不同步的（运行效率高）
- HashTable：底层是哈希表，null不能作为键或者值，线程同步的（运行效率低）

> Map是如何保证唯一性的，通过hashCode()和euqals()两个方法同时决定，当自己创建的类生成的作为key存放进mapde的时候，需要重写这两个方法指定规则，什么时候键重复


### 5. I/O流
- 字节流基类：InputStream、 OutputStream
- 字符流基类：Reader、Writer
- 子类：FileInputStream 以当前基类名字为后缀，前面的名字是具体操作对象
- 子列：FileReader同理
- 字符流是基于字节流，只要使用字符流，方便

### 6. variables

- local variables: in function

- instance variables: in class

- class variables: in class and static

### 7. declaration rules

- one public class in each file
- file name should be the same as class name
- package declaration should be the first line

### 8. access control modifier

- visible to the package, the default

- visible to the class only, private

- visible to the world, public

- visible t the package and the subclasses, protected

### 9. non-access modifier 

- static, for creating class functions and variables

- final, for finalizing the implementations of classes, functions, and variables

- abstract, for creating abstract classes and functions

- synchronized and volatile, are used for threads

### 10. 多线程

- 进程是程序的一次动态执行过程，线程是对进程的进一步细分。

- 对于单核CPU，多线程需要通过CPU抢占运行的。

- Java中可以采用两种方式：

	1. 继承Thread类
	2. 实现Runnable接口

- 继承Thread类

	java.lang包（自动导入），必须重写Thread类中的run()方法，次方法为线程的主体

	调用使用mythread.start()方法，实际上最终调用的就是run()方法

- 实现Runnable接口

	Runnable接口中只定义了一个抽象方法run()

	启动线程需要利用Thread类的start()方法，可以利用Thread类的构造方法

	public Thread（Runnable target） 来构造一个Thread对象

- Thread和Runnable接口的关系

	Thread implements Runnable，所以Thread也是Runnable接口的子类

- 区别：

	Thread类在操作多线程的时候无法达到资源共享的目的，而使用Runnable接口实现的多线程操作可以实现资源共享，Runnable实现可以使用同一个对象去实例化多个Thread类

- 线程的状态

	- 创建状态，创建了一个对象	
	- 就绪状态，调用了start()方法，等待CPU调度
	- 运行状态，执行run()方法
	- 阻塞状态，暂时停止执行，可能将资源交给其他线程使用
	- 终止状态（死亡状态），线程执行完毕

	线程调用start()方式的时候不是立刻启动的，而是等待CPU进行调度

- 取得当前线程

	Thread.currentThread() 

- JAVA运行时到底启动了多少个进程呢？

	至少两个：主线程、GC

- 判断线程是否启动

	t.isAlive()

- 线程强制运行

	t.join(), 线程强制运行期间，其他线程无法运行，必须等待此线程完成之后才可以继续执行

- 线程休眠

	Thread.sleep(50);

- 中断线程
	
	当一个线程运行的时候，另外一个线程可以直接通过interrupt()方法，中断其运行状态

	t.interrupt() 中断线程执行

- 后台线程

	在java程序中，只要前台有一个线程在运行，则整个java进程都不会消失，所以此时可以设置一个后台线程，这样即使JAVA进程结束了，此后台线程依然会继续执行。

	t.setDaemon(true);
	t.start();

- 线程优先级

	t.setPrioriry(ThreadMIN_PRIORITY); // 优先级最低 1

	t.setPriority(ThreadMAX_PRIORITY); // 优先级最高 10

	t.setPriority(ThreadNORM_PRIORITY); // 优先级中等 5

	Thread.currentThread().getPriority(); 

- 线程的礼让

	使用yield()方法将一个线程的操作暂时让给其他线程执行

	Thread.currentThread().yield();

### 11. 同步和死锁

- 同步代码块 和 同步方法

- 同步代码块格式

```

	synchronized(同步对象) {

		需要同步的代码;

	}
```

	同步的时候需要指明同步的对象，一般情况下会将当前对象作为同步的对象，使用this

- 同步方法

	synchronized 方法返回值 方法名称（参数列表） {}


### 12.等待与唤醒

- Object类是所有类的父类，在此类中有以下几个方法是对线程操作有所支持的

- wait() notify() notifyAll()

- notify() 唤醒下一个线程执行， notifyAll() 会唤醒所有的线程，优先级高的会先执行








