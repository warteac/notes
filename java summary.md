## Java 复习总结

### 1. 数据类型
- 基础数据类型：8种 byte（1字节）、short、char（2字节）、int、float（4字节）、long、double（8字节）、boolean（1位）
- 引用数据类型：3种 类（Class）接口（Interface）数组（Array）
  - Object：所有类的超类，包括数组
  - String
  - Date: Calender 
  - Void: 不可实例化的占位符类，它保持一个对象Java关键字void的class对象的引用
  - 8种基础数据类型的包装类：Byte、Short、Character、Integer、Float、Double、Long、Boolean

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

