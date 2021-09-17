### sychronized

```
	 0: ldc   #2   // class juc/Synchronized
         2: dup
         3: astore_1
         4: monitorenter   // 这里
         5: aload_1
         6: monitorexit    // 这里
         7: goto          15
        10: astore_2
        11: aload_1
        12: monitorexit    // 这里
        13: aload_2
        14: athrow
        15: return

```                
- 为什么一个加锁操作，却有两次释放？原因：一次正常释放锁，一次为程序异常释放
- 使用场景：

![8f00ae410957db339e15d1211067b55d](https://user-images.githubusercontent.com/29136753/132813222-da66f236-63e9-4ee1-90c2-39b1c0f5e043.png)


- 可重入 : 线程对象会拥有一个计数器，每当线程获取到锁对象时计数器+1，释放-1，值为0时完全释放锁，不为0时该线程可以无需竞争即可获取锁;

- 锁的实现：
    - JVM对象内存区域：
        - 对象头
            - Mark Word:存储对象的hashcode,分代年龄和锁标志信息。
            - Class Point:对象指向类元数据的指针。
        - 实例数据:类的数据信息，父类的信息。
        - 对齐填充：虚拟机要求对象地址为8字节的整数倍，所以不足会进行对齐填充。

    - 同一时刻只用一个线程可以获取到对象的监视器。

![image](https://user-images.githubusercontent.com/29136753/132813389-bd9faf13-5c92-4c2b-9971-c5337a828cd5.png)


- 锁状态

![image](https://user-images.githubusercontent.com/29136753/132813476-9347d7f7-fab1-4190-9552-4389ff0d8c4d.png)

- 无锁
- 偏向锁
    - 当一个线程访问同步块并获取锁时，会在对象头和栈帧中的锁记录里存储锁偏向的线程ID，以后该线程在进入和退出同步块时不需要进行CAS操作来加锁和解锁，只需简单地测试一下对象头的Mark Word里是否存储着指向当前线程的偏向锁。如果测试成功，表示线程已经获得了锁。如果测试失败，则需要再测试一下Mark Word中偏向锁的标识是否设置成1（表示当前是偏向锁）：如果没有设置，则使用CAS竞争锁；如果设置了，则尝试使用CAS将对象头的偏向锁指向当前线程

- 轻量级锁
    - 线程在执行同步块之前，JVM会先在当前线程的栈桢中创建用于存储锁记录的空间，并将对象头中的Mark Word复制到锁记录中，官方称为Displaced Mark Word。然后线程尝试使用CAS将对象头中的Mark Word替换为指向锁记录的指针。如果成功，当前线程获得锁，如果失败，表示其他线程竞争锁，当前线程便尝试使用自旋来获取锁。

- 重量级锁

#### 参考
- synchronized
    - [1](https://www.cnblogs.com/wangwudi/p/12302668.html)
    - [2](https://www.zhihu.com/question/57794716?sort=created)

> 大脑：好的，我都已经会了!
> 手：MD真笨，键盘倒是敲啊！
