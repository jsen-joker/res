### 线程

https://www.cnblogs.com/yanggb/p/10629387.html

https://www.jianshu.com/p/f30ee2346f9f

1. 线程池的几种状态

   ![img](https://img2018.cnblogs.com/blog/842514/201903/842514-20190330221205332-1328151121.png)

2. 线程池的工作模式

   1）提交任务

   2）创建工作线程

   3）启动工作线程

   4）获取任务并执行

3. 线程池的拒绝策略

   当线程池的任务缓存队列已满并且线程池中的线程数目达到maximumPoolSize时，如果还有任务到来就会采取任务拒绝策略，通常有以下四种策略：
ThreadPoolExecutor.AbortPolicy:丢弃任务并抛出RejectedExecutionException异常。 ThreadPoolExecutor.DiscardPolicy：丢弃任务，但是不抛出异常。 ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新提交被拒绝的任务 ThreadPoolExecutor.CallerRunsPolicy：由调用线程（提交任务的线程）处理该任务
   
4. 线程池怎么唤醒队列里的任务

   https://blog.csdn.net/nobody_1/article/details/98799662

   **把去等待队列中获取任务的过程封装成一个Runnable任务(就是该Worker对象)，新创建的线程启动后，就会执行该Runnable任务，该Runnable任务执行完firstTask任务后，就会不断的从等待队列中获取任务，直到等待队列为空，该线程才会销毁**。

5. 线程池的几个参数

   构造函数的参数：核心线程数、最大工作线程数、超时时间、超时时间单位、创建线程的工厂、缓存任务的阻塞队列、拒绝策略

   线程池的重要属性：线程状态、工作线程的数量，核心线程数、最大线程数、创建线程的工厂、缓存任务的阻塞队列、非核心线程存活的时间、拒绝策略。

6. 线程的几种状态

   ![image-20200708235956846](C:\Users\Su\AppData\Roaming\Typora\typora-user-images\image-20200708235956846.png)

### 锁

锁自动升级过程

无锁向 —（单个线程执行）→ 偏向锁 —（有线程竞争）→ 轻量级锁 —（自旋超过10次or等待线程数超过CPU数量的一半）→ 重量级锁 

synchionzed的原理：锁的自动升级
CAS ：compareAndSwap
AQS

公平锁：所有线程都有获取到锁的机会

非公平锁：不是所有线程都有获取到锁的机会

https://www.jianshu.com/p/eaea337c5e5b

### 数据库

mysql引擎 两种引擎区别

innodb & myisam

索引：innodb是聚集索引，聚簇索引的文件存放在主键索引的叶子节点上。myisam是非聚集索引，数据文件是分离的，索引保存的是数据文件的指针。

事务：innodb支持事务，myisam不支持事务。

外键：innodb支持外键（FOREIGN KEY），myisam不支持。

行数：InnoDB 不保存表的具体行数，执行 select count(*) from table 时需要全表扫描。而MyISAM 用一个变量保存了整个表的行数，执行上述语句时只需要读出该变量即可，速度很快；

锁：InnoDB 最小的锁粒度是行锁，MyISAM 最小的锁粒度是表锁。

用的索引种类
B+树 
(A,B,C) where A and C
事务的原理

1. 日志

   redo log是用来恢复数据的 用于保障，已提交事务的持久化特性；

   undo log是用来回滚数据的用于保障，未提交事务的原子性

2. 读写锁

   ![img](https://user-gold-cdn.xitu.io/2019/4/17/16a27696def80b5f?w=416&h=183&f=png&s=16219)

3. MVCC (MultiVersion Concurrency Control) -多版本并发控制

- 原子性：使用 undo log ，从而达到回滚
- 持久性：使用 redo log，从而达到故障后恢复
- 隔离性：使用锁以及MVCC,运用的优化思想有读写分离，读读并行，读写并行
- 一致性：通过回滚，以及恢复，和在并发环境下的隔离做到一致性。

https://www.cnblogs.com/wyc1994666/p/11367051.html

wait 和 sleep 和 park 的区别

![image-20200713222555240](C:\Users\Su\AppData\Roaming\Typora\typora-user-images\image-20200713222555240.png)

怎么减库存

缓存穿透的解决

问题：为什么不使用AVL树而使用红黑树？
红黑树和AVL树都是最常用的平衡二叉搜索树，它们的查找、删除、修改都是O(lgn) time
AVL树和红黑树有几点比较和区别：
（1）AVL树是更加严格的平衡，因此可以提供更快的查找速度，一般读取查找密集型任务，适用AVL树。
（2）红黑树更适合于插入修改密集型任务。
（3）通常，AVL树的旋转比红黑树的旋转更加难以平衡和调试。
总结：
（1）AVL以及红黑树是高度平衡的树数据结构。它们非常相似，真正的区别在于在任何添加/删除操作时完成的旋转操作次数。
（2）两种实现都缩放为a O(lg N)，其中N是叶子的数量，但实际上AVL树在查找密集型任务上更快：利用更好的平衡，树遍历平均更短。另一方面，插入和删除方面，AVL树速度较慢：需要更高的旋转次数才能在修改时正确地重新平衡数据结构。
（3）在AVL树中，从根到任何叶子的最短路径和最长路径之间的差异最多为1。在红黑树中，差异可以是2倍。
（4）两个都给O（log n）查找，但平衡AVL树可能需要O（log n）旋转，而红黑树将需要最多两次旋转使其达到平衡（尽管可能需要检查O（log n）节点以确定旋转的位置）。旋转本身是O（1）操作，因为你只是移动指针。

面试美团
项目
线程池
JVM 7和8的区别，常量放在8的哪个区
HashMap 和ConcunrrentHashMap 区别
JDK新版本特性
线程的几个状态
sleep和wait区别
redis特性
反转链表

linux 查询IP出现次数排序
sort test_ip.txt | uniq -c | sort -rn | head -n 1

CPU过高怎么查问题

https://blog.csdn.net/weixin_43778179/article/details/90147523

内存溢出怎么查问题
反转单向链表

美团二面
算法1 三个线程循环打印ABC
算法2 
求成绩第二名的名字
// 姓名 学号 分数 
        

explain SELECT
	* 
FROM
	result 
WHERE
	score = ( SELECT max( score ) FROM result WHERE score <> ( SELECT max( score ) FROM result ) );
    



找工作
项目介绍
供应平台搭建

火车票有三个系统
订单、资金、票台
票台有Q票台，携程分销、小代理商分销
为的就是一个订单快速推票台

火车票系统与携程、利安等代理商交互代码分散在各个工程没有专门团队维护，经常出现问题。春运期间，推携程加速包由于流程错误，导致30%推送失败。需要将相关业务进行重新整合，保障与代理商服务质量，最终实现各模块业务分离，职责单一。

1.业务边界更清楚，统一提供代理商服务
2.集中做保障机制，失败重试和错误落库
3.流程控制，避免消息到达先后顺序对业务的影响
4.监控细化，对代理商能力监控


统一入口，策略模式，模板方法

封装接口和参数，只要必须的参数，剩余的自己查询，或计算

票台可以理解为能力测，打码算一种能力，谁风控做的好，谁出票好。票台是有成本的，谁能力抢。


主要负责三部分

供应平台，和多个票台对接
票系统，负责所有火车票出票后的退票、改签等功能
对账，Q票台的核销退款





美团面试
线程池几个参数，过程，拒绝策略

redis
redis aof rdb ,aof怎么压缩的， del set lpush怎么压缩
跳表怎么实现的，hash怎么实现的，和Java的HashMap有什么区别

hash冲突怎么解决


HashMap为什么不安全
voliate 怎么实现的内存屏障

GC
CMS怎么工作 G1怎么工作

算法
两个大数相乘，超过long的范围了，不能用BigDecimal

为什么不用mq，二用redis
一、mq 高峰期可能消费有延迟，而且是所有的消息，不能通过配置重点关注
二、mq 监听多个，扩展不方便，监听一个，一个类型出问题，拖累其他的也消费慢
三、有成熟的delayQueue机制
四、发了消息就可能有多个消费者，这不利于我们维护，就算发消息也要我们决定什么时候发消息，发的也是子类型
五、让我们代码分散，不容易维护，而且消费者只有一个， 不给外界消费的机会，我们只是做异步，不是解耦，系统还是一个整体。我们用mq的地方肯定是afterSuccess


Spring event 事件驱动

dataBus 驱动总线



火币网面试
线程池是怎么工作的，为什么一个线程start后就结束了，线程池还能复用
怎样设计一个安全的 外网 url 不被爬
异步 httpclient 与线程池之间关系
线程池 原理 参数，最大数配置

设计外网访问内网 安全 url 策略

异步 http client 与 线程池是否可以替代

CMS 对应 老年代 年轻代 配置（公司配置）为什么这么配置
缺点是什么，怎么处理内存碎片

volatile 与 atomicInteger 区别

mysql 隔离级别


想问的问题
美团火车票有啥痛点吗
有线下的能力吗
线下能力实时吗
有没有12306公关方面的人才


美团面试
ThreadLocal

 outofmemory stackoverflow 是什么，遇到怎样情况会发生举例

 数据库，redis一致性
         
 线程的生命周期
         
 线程池 使用
         
 强引用与弱引用区别

 mysql 语句
 张三 语文 80
 张三 英语 70
 李四 语文 79
 李四 英语 99
 1、每个人最大分数

select * from score a where score=(select max(score) from score where course=a.course)

 2、输出
    张三 语文 英语
    李四 语文 英语

3、![image-20200714200208184](C:\Users\Su\AppData\Roaming\Typora\typora-user-images\image-20200714200208184.png)

select name as '姓名',
max(case course when '语文' then score else 0 end) as '语文',
max(case course when '英语' then score else 0 end) as '英语'
from score
group by name



算法，求连续相同最大长度
 str1 abcde  str2 bcdf   结果 3


脉脉面试
桶排序
mq ack 
redis IO多路复用
jvm 垃圾回收stw几次
NIO

面试题
dubbo provider下线了，consumer怎么感知到
mysql怎么实现死锁
start TRANSACTION;
select * from ticket where id in(8,9) for update;
commit;

start transaction;
select * from ticket where id in(18,8,7) for update;
此时id为18的没被锁住，id为7的被锁住了。
commit;


Select * from xxx where id in (xx,xx,xx) for update
在in里面的列表值mysql是会自动从小到大排序，加锁也是一条条从小到大加的锁

当对存在的行进行锁的时候(主键)，mysql就只有行锁。
当对未存在的行进行锁的时候(即使条件为主键)，mysql是会锁住一段范围（有gap锁）

强引用、弱引用、虚引用、软引用
redis IO多路复用了解吗
mysql数据B树，怎么存储的
锁升级的过程
CPU 100%问题怎么查
jstack里面的是什么
里面放着线程的运行状态，Block的话，在等待什么锁，Found 1 deadlock.
mq怎么保证一定消费到
alter table TABLE_NAME add column NEW_COLUMN_NAME varchar(20) not null;是怎么实现加一列的
CAS Unsafe怎么实现的
Linux的X86下主要是通过cmpxchgl这个指令在CPU级完成CAS操作的，但在多处理器情况下必须使用lock指令加锁来完成。从这个例子就可以比较清晰的了解CAS的底层实现了，当然不同的操作系统和处理器的实现会有所不同，大家可以自行了解。
Redis为什么是原子的
对于Redis而言，命令的原子性指的是：一个操作的不可以再分，操作要么执行，要么不执行。
Redis操作原子性的原因
Redis的操作之所以是原子性的，是因为Redis是单线程的。
由于对操作系统相关的知识不是很熟悉，从上面这句话并不能真正理解Redis操作是原子性的原因，进一步查阅进程与线程的概念及其区别。

CMS标记清除,什么时候整理碎片

dubbo里的是什么线程池


​    
算法
二分查找
Java实现一个死锁
wait notify必须在锁里面用
mysql 索引>100加锁范围
mysql的事务怎么实现的
redis热key

packagecom.practice.basic.base.lock;

/**
*@author:kai.dai
*@date:2020-06-0301:03
*/
publicclassDeadLock{
staticObjectdkLock1=newObject();
staticObjectdkLock2=newObject();

publicstaticvoidmain(String[]args){
Threadt1=newThread(
()->{
synchronized(dkLock1){
try{
System.out.println("线程1拿到a锁");
Thread.sleep(1000);
synchronized(dkLock2){
System.out.println("线程1拿到b锁");
System.out.println("线程1拿到b锁");

}
System.out.println("线程1释放a锁");

}catch(InterruptedExceptione){
e.printStackTrace();
}
}

});
Threadt2=newThread(
()->{
synchronized(dkLock2){
try{
System.out.println("线程2拿到b锁");
Thread.sleep(1000);
synchronized(dkLock1){
System.out.println("线程2拿到a锁");
System.out.println("线程2拿到a锁");

}
System.out.println("线程2释放b锁");

}catch(InterruptedExceptione){
e.printStackTrace();
}
}

});
t1.setName("daikaiThread1");
t2.setName("daikaiThread2");
t1.start();
t2.start();
}
}




2020-06-03 01:19:57
Full thread dump Java HotSpot(TM) 64-Bit Server VM (25.191-b12 mixed mode):

"Attach Listener" #13 daemon prio=9 os_prio=31 tid=0x00007ff18d1bc000 nid=0x3f03 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"DestroyJavaVM" #12 prio=5 os_prio=31 tid=0x00007ff18c0a7800 nid=0xd03 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"daikaiThread2" #11 prio=5 os_prio=31 tid=0x00007ff18c978000 nid=0x3b03 waiting for monitor entry [0x000070000175f000]
   java.lang.Thread.State: BLOCKED (on object monitor)
	at com.practice.basic.base.lock.DeadLock.lambda$main$1(DeadLock.java:38)
	- waiting to lock <0x0000000795820548> (a java.lang.Object)
	- locked <0x0000000795820558> (a java.lang.Object)
	at com.practice.basic.base.lock.DeadLock$$Lambda$2/1996181658.run(Unknown Source)
	at java.lang.Thread.run(Thread.java:748)

"daikaiThread1" #10 prio=5 os_prio=31 tid=0x00007ff18c0a6800 nid=0x3a03 waiting for monitor entry [0x000070000165c000]
   java.lang.Thread.State: BLOCKED (on object monitor)
	at com.practice.basic.base.lock.DeadLock.lambda$main$0(DeadLock.java:19)
	- waiting to lock <0x0000000795820558> (a java.lang.Object)
	- locked <0x0000000795820548> (a java.lang.Object)
	at com.practice.basic.base.lock.DeadLock$$Lambda$1/1896277646.run(Unknown Source)
	at java.lang.Thread.run(Thread.java:748)

"Service Thread" #9 daemon prio=9 os_prio=31 tid=0x00007ff18d84d800 nid=0x4503 runnable [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C1 CompilerThread2" #8 daemon prio=9 os_prio=31 tid=0x00007ff18d84d000 nid=0x3803 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C2 CompilerThread1" #7 daemon prio=9 os_prio=31 tid=0x00007ff18d1b0000 nid=0x4703 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C2 CompilerThread0" #6 daemon prio=9 os_prio=31 tid=0x00007ff18d84c000 nid=0x3603 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Monitor Ctrl-Break" #5 daemon prio=5 os_prio=31 tid=0x00007ff18d848800 nid=0x3503 runnable [0x000070000104a000]
   java.lang.Thread.State: RUNNABLE
	at java.net.SocketInputStream.socketRead0(Native Method)
	at java.net.SocketInputStream.socketRead(SocketInputStream.java:116)
	at java.net.SocketInputStream.read(SocketInputStream.java:171)
	at java.net.SocketInputStream.read(SocketInputStream.java:141)
	at sun.nio.cs.StreamDecoder.readBytes(StreamDecoder.java:284)
	at sun.nio.cs.StreamDecoder.implRead(StreamDecoder.java:326)
	at sun.nio.cs.StreamDecoder.read(StreamDecoder.java:178)
	- locked <0x00000007957b55c8> (a java.io.InputStreamReader)
	at java.io.InputStreamReader.read(InputStreamReader.java:184)
	at java.io.BufferedReader.fill(BufferedReader.java:161)
	at java.io.BufferedReader.readLine(BufferedReader.java:324)
	- locked <0x00000007957b55c8> (a java.io.InputStreamReader)
	at java.io.BufferedReader.readLine(BufferedReader.java:389)
	at com.intellij.rt.execution.application.AppMainV2$1.run(AppMainV2.java:64)

"Signal Dispatcher" #4 daemon prio=9 os_prio=31 tid=0x00007ff18d037000 nid=0x3303 runnable [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Finalizer" #3 daemon prio=8 os_prio=31 tid=0x00007ff18d020000 nid=0x5203 in Object.wait() [0x0000700000d3e000]
   java.lang.Thread.State: WAITING (on object monitor)
	at java.lang.Object.wait(Native Method)
	- waiting on <0x0000000795588ed0> (a java.lang.ref.ReferenceQueue$Lock)
	at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:144)
	- locked <0x0000000795588ed0> (a java.lang.ref.ReferenceQueue$Lock)
	at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:165)
	at java.lang.ref.Finalizer$FinalizerThread.run(Finalizer.java:216)

"Reference Handler" #2 daemon prio=10 os_prio=31 tid=0x00007ff18d00b000 nid=0x5303 in Object.wait() [0x0000700000c3b000]
   java.lang.Thread.State: WAITING (on object monitor)
	at java.lang.Object.wait(Native Method)
	- waiting on <0x0000000795586bf8> (a java.lang.ref.Reference$Lock)
	at java.lang.Object.wait(Object.java:502)
	at java.lang.ref.Reference.tryHandlePending(Reference.java:191)
	- locked <0x0000000795586bf8> (a java.lang.ref.Reference$Lock)
	at java.lang.ref.Reference$ReferenceHandler.run(Reference.java:153)

"VM Thread" os_prio=31 tid=0x00007ff18c01b800 nid=0x2c03 runnable

"GC task thread#0 (ParallelGC)" os_prio=31 tid=0x00007ff18d00a000 nid=0x2107 runnable

"GC task thread#1 (ParallelGC)" os_prio=31 tid=0x00007ff18c008800 nid=0x2003 runnable

"GC task thread#2 (ParallelGC)" os_prio=31 tid=0x00007ff18c016000 nid=0x1e03 runnable

"GC task thread#3 (ParallelGC)" os_prio=31 tid=0x00007ff18c016800 nid=0x2a03 runnable

"VM Periodic Task Thread" os_prio=31 tid=0x00007ff18d84e800 nid=0x4303 waiting on condition

JNI global references: 320


Found one Java-level deadlock:
=============================
"daikaiThread2":
  waiting to lock monitor 0x00007ff18c8320a8 (object 0x0000000795820548, a java.lang.Object),
  which is held by "daikaiThread1"
"daikaiThread1":
  waiting to lock monitor 0x00007ff18c82f6b8 (object 0x0000000795820558, a java.lang.Object),
  which is held by "daikaiThread2"

Java stack information for the threads listed above:
===================================================
"daikaiThread2":
	at com.practice.basic.base.lock.DeadLock.lambda$main$1(DeadLock.java:38)
	- waiting to lock <0x0000000795820548> (a java.lang.Object)
	- locked <0x0000000795820558> (a java.lang.Object)
	at com.practice.basic.base.lock.DeadLock$$Lambda$2/1996181658.run(Unknown Source)
	at java.lang.Thread.run(Thread.java:748)
"daikaiThread1":
	at com.practice.basic.base.lock.DeadLock.lambda$main$0(DeadLock.java:19)
	- waiting to lock <0x0000000795820558> (a java.lang.Object)
	- locked <0x0000000795820548> (a java.lang.Object)
	at com.practice.basic.base.lock.DeadLock$$Lambda$1/1896277646.run(Unknown Source)
	at java.lang.Thread.run(Thread.java:748)

Found 1 deadlock

高德
redis为什么单线程
Redis很快！官方FAQ表示，因为Redis是基于内存的操作，CPU不是Redis的瓶颈，Redis的瓶颈最有可能是机器内存的大小或者网络带宽。既然单线程容易实现，而且CPU不会成为瓶颈
假如设计成多线程，怎么保证原子
设计一个微博信息流feed
5亿条数据，取前五百条
TopK使用小顶堆
redis的淘汰策略
进程线程协程
awk实现第二行求和
sed用过吗
Lock和synchronized关键字

重构改善代码的既有逻辑

Spring MVC 怎么实现的
为什么用redis不用mq
synchnized和Lock有什么区别
HashMap怎么扩容
HashMap和ConcurrentHashMap区别
可重入锁怎么实现的
dubbo的负载均衡有哪些
redis zset 怎么实现的，根据key查分数值怎么查，根据分数值查key怎么查


快手
AOF RDB 
redis怎么同步消息的
mysql mvcc了解吗
Java 线程之间怎么同步数据的
HashTable和HashMap的put有啥不一样？
HashTable在不指定容量的情况下的默认容量为11，而HashMap为16，Hashtable不要求底层数组的容量一定要为2的整数次幂，而HashMap则要求一定为2的整数次幂。
      Hashtable扩容时，将容量变为原来的2倍加1，而HashMap扩容时，将容量变为原来的2倍。
      Hashtable和HashMap它们两个内部实现方式的数组的初始大小和扩容的方式。HashTable中hash数组默认大小是11，增加的方式是 old*2+1。

Synch和Lock有啥区别
线程池的状态
redis key 集中过期怎么处理
JVM分区
重构怎么规划的过程
设计模式有哪几类
总体来说设计模式分为三大类：

创建型模式，共五种：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。

结构型模式，共七种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。

行为型模式，共十一种：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

单例中的饿汉懒汉dubbo check
为什么懒汉会出现两个对象？
你们用的啥垃圾收集器，CMS和G1区别
trainsction注解什么时候不起作用
Transactional不生效的场景
数据库引擎是否支持事务。
注解所在的类是否被加载。
注解所在的方法是否为public修饰。如果要用在非 public 方法上，可以开启 AspectJ 代理模式。
是否发生了自调用问题。就调该类自己的方法，而没有经过 Spring 的代理类，默认只有在外部调用事务才会生效。Spring 如何在一个事务中开启另一个事务？
所用的数据源是否加载了事务管理器。
@Transactional的扩展配置propagation是否正确。Propagation.NOT_SUPPORTED： 表示不以事务运行。
异常不抛出。
设置触发事务的异常错误。默认回滚的是：RuntimeException，如果想触发其他异常的回滚，需要在注解上配置一下，如：@Transactional(rollbackFor = Exception.class)这个配置仅限于 Throwable 异常类及其子类。
共享锁和独占锁
独占锁：也是悲观锁
synchronized和ReentrantLock
共享锁接口：
ReadWriteLock接口
共享锁：该锁可被多个线程共有，典型的就是ReentrantReadWriteLock里的读锁，它的读锁是可以被共享的，但是它的写锁确每次只能被独占。

猿辅导
	1. Redis的多路复用
	2. 尾递归的使用条件
	3. 算法 链表表示数字，求和
	4. 重构怎么保证不出问题
	5. Zset怎么实现的，怎么添加节点
	6. TCP四次挥手

滴滴
线程池,为什么会线程不安全，
jvm 内存模型，运行时模型，堆内放哪些资源

怎样解决 oom，cpu占用高，GC 相关

mysql  引擎，索引，行锁，间隙锁，mysql隔离级别，怎样解决可重复读，mvcc 怎样解决的，redo log

怎样查 a 索引未生效，select * from table where a=1
explain ，explain 包含哪些模块

为什么用B+树，不用二叉查找树，区别

redis 平时都用过哪些结构，跳表，淘汰机制，lru ，怎样实现一个lru，都有哪些方法

快手三面
算法 5+4*2-3+50
idea插件
项目
怎么把控项目进度


美团-WMPS-骑手
项目介绍
供应平台和对账系统
算法：删除倒数第n个节点
线程池的工作流程
线程池的拒绝策略
synchronized和ReentrantLock的区别
threadlocal
redis
工程中的应用、大数、持久化（aof和rdb）、跳表
mq的应用

美团-优选
jdk
用过stream吗？说下stream中lambda表达式的实现原理
用过哪些数据结构，哪些是线程安全的，哪些是线程不安全的。
线程不安全的只想起来hashmap。。。都看了面经，就不问hashmap的不安全原理了。介绍下concurrentHashMap的size方法实现原理
多线程
创建新线程的方法
线程池工作的线程变化，线程池设置最小线程数的意义
设计模式及工程应用
spring
用过aop吗？怎么用的
bean的初始化过程、bean初始化内容
数据库
用的什么数据库。数据库的数据隔离级别、数据结构？
redis
压缩数据存储原理、持久化方式、用过哪些操作命令
对下份工作的规划和期待

1、redis的永久化机制、主从复制机制
2、synchronized和reentrantLock实现原理
3、数据库的隔离级别
4、volatile关键字

int sum = n1[i] * n2[j] + multiply[i + j];
multiply[i + j] = sum % 10;
multiply[i + j + 1] += sum / 10;


public int[] printInZHI(int[][] matrix) {
    if (matrix.length == 0) {
        return new int[0];
    }
    // write code here
    int rows = matrix.length;
    int cols = matrix[0].length;
    int len = rows * cols;
    int[] result = new int[len];
    int index = 0;
    for (int sum = 0; sum < rows + cols - 1; sum++) {
        if (sum % 2 != 0) {
            // 奇数：右上 → 左下
            int j = Math.min(sum, cols - 1);
            int i = sum - j;
            while (j >= 0 && i < rows) {
                result[index++] = matrix[i][j];
                i++;
                j--;
            }
        } else {
            // 偶数：左下 → 右上
            int i = Math.min(sum, rows - 1);
            int j = sum - i;
            while (i >= 0 && j < cols) {
                result[index++] = matrix[i][j];
                i--;
                j++;
            }
        }
    }
    return result;
}


//[1,2, 3, 4]
//[5,6, 7, 8]
//[9,10,11,12]
//[13,14,15,16]
//[1, 2, 3, 4, 8, 12, 16, 15, 14, 13, 3, 2, 6, 7, 11, 10]

    public static int[] printMatrix(int[][] matrix) {
        int rows = matrix.length;
        int cols = matrix[0].length;
        int[] result = new int[rows * cols];
        int index = 0;
        for (int i = 0; i <= rows >> 1; i++) {
            // print one circle
            for (int j = i; j < cols - i; j++) {
                result[index++] = matrix[i][j];
            }
            // 注意越界问题
            for (int j = i + 1; j < rows - i && cols - 1 - i > i; j++) {
                result[index++] = matrix[j][cols - 1 - i];
            }
            // 注意越界问题
            for (int j = cols - 2 - i; j >= i && rows - 1 - i > i; j--) {
                result[index++] = matrix[rows - 1 - i][j];
            }
            for (int j = rows - 2 - i; j > i; j--) {
                result[index++] = matrix[j][i];
            }
        }
        return result;
    }


    BIO (Blocking I/O): 同步阻塞 I/O 模式，数据的读取写入必须阻塞在一个线程内等待其完成。在活动连接数不是特别高（小于单机 1000）的情况下，这种模型是比较不错的，可以让每一个连接专注于自己的 I/O 并且编程模型简单，也不用过多考虑系统的过载、限流等问题。线程池本身就是一个天然的漏斗，可以缓冲一些系统处理不了的连接或请求。但是，当面对十万甚至百万级连接的时候，传统的 BIO 模型是无能为力的。因此，我们需要一种更高效的 I/O 处理模型来应对更高的并发量。

NIO (Non-blocking/New I/O): NIO 是一种同步非阻塞的 I/O 模型，在 Java 1.4 中引入了 NIO 框架，对应 java.nio 包，提供了 Channel , Selector，Buffer 等抽象。NIO 中的 N 可以理解为 Non-blocking，不单纯是 New。它支持面向缓冲的，基于通道的 I/O 操作方法。 NIO 提供了与传统 BIO 模型中的 Socket 和 ServerSocket 相对应的 SocketChannel 和 ServerSocketChannel 两种不同的套接字通道实现,两种通道都支持阻塞和非阻塞两种模式。阻塞模式使用就像传统中的支持一样，比较简单，但是性能和可靠性都不好；非阻塞模式正好与之相反。对于低负载、低并发的应用程序，可以使用同步阻塞 I/O 来提升开发速率和更好的维护性；对于高负载、高并发的（网络）应用，应使用 NIO 的非阻塞模式来开发



AIO (Asynchronous I/O): AIO 也就是 NIO 2。在 Java 7 中引入了 NIO 的改进版 NIO 2,它是异步非阻塞的 IO 模型。异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。AIO 是异步 IO 的缩写，虽然 NIO 在网络操作中，提供了非阻塞的方法，但是 NIO 的 IO 行为还是同步的。对于 NIO 来说，我们的业务线程是在 IO 操作准备好时，得到通知，接着就由这个线程自行进行 IO 操作，IO 操作本身是同步的。查阅网上相关资料，我发现就目前来说 AIO 的应用还不是很广泛，Netty 之前也尝试使用过 AIO，不过又放弃了。





腾讯一面
1 spring cloud 有哪些组件
 服务发现——Netflix Eureka 客服端负载均衡——Netflix Ribbon 断路器——Netflix Hystrix 服务网关——Netflix Zuul 分布式配置——Spring Cloud Config
 我说我用的 nacos 时间的服务治理，分布式配置也是nacos，后来转的apollo

2 多线程参数含义
3 线程周期
4 mysql binlog 主从同步 
4 mysql 主键索引和其他的区别
5 redis 缓存和分布式锁
6 http
7 四次挥手
8 容器（一堆常用的，对比 内部怎么实现，扩容，安全 之类的）
9 synchronized和ReentrantLock 的区别   
10 多线程同步问题  ， 让 volitole atomic
   巴巴了一下aqs
11 阻塞io 和 非柱塞io 然后使用参金，为啥用select 后来问啥不用了，epoll性能缺点
12 mq ack 业务问题，怎么补偿 之类的
13 spring aop ioc 
14 加解密 签名 
15 
15 一对业务问题，忘记了


滴滴面试
算法： 
aqs 
mysql 锁
redis 锁
spring 事务传播
java 锁

美团美团单车 ： 一面二面
...
结果:【美团点评】熊帅您好，感谢您应聘两轮-高级Java工程师（IOT后端）职位！很遗憾您未通过此次面试，结果仅代表您与该岗位的匹配程度。

美团一面
数据库 三大范式 锁 事务 查找100w数据后的10条 存储引擎
redis 分布式锁
es
jvm
安全map 以及区别
dubbo
mq
冒泡排序
算法 重数










