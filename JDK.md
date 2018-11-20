# source code

# classLoader
* Bootstrap类加载器 C++实现，加载JAVA_HOME\lib目录下jar
* Extension类加载器 java实现，继承自ClassLoader，加载JAVA_HOME\lib\ext目录下jar，或者java.ext.dirs系统变量指定类库
* Applicatiion类加载器，java实现，继承自ClassLoader，加载classpath下类库


# 对象头ObjectHeader组成
* 25bit哈希码 4bit分代年龄 2bit锁标志位 1bit固定0
* 指向对象所属类class对象的指针

# java内存模型
## 内存划分
* 主内存：所有线程共享(物理内存)，所有变量存放的位置。
    * lock
    * unlock
    * read
    * write
* 工作内存：线程私有(高速缓存)，读取，赋值在此进行，不可以直接读取主存。
    * load与read同时使用
    * store与write同时使用
    * assign执行引擎到工作内存
    * use工作内存到执行引擎

## 内存模型特性
* 原子性：以上操作具备原子性
* 可见性：线程可立即感知变量的修改，需要通过volatile实现
* 有序性：单线程天然有序，多线程通过volatile和synchronized实现

# 线程安全
1.不可变 final
2.绝对线程安全 大部分java线程安全类不是绝对线程安全的，线程安全类只保证方法内部线程安全，但不保证调用多个方法线程安全
3.相对线程安全 java线程安全类Vector，HashTable等
4.线程兼容 通过同步手段保证并发环境下安全使用 HashMap

# 锁优化(java1.6引入或者1.6开启)
* 自旋锁(1.4引入，1.6默认开启)和自适应自旋锁(1.6引入)，由于共享数据锁定时间比较短，所以挂起和恢复线程不值得，可以让请求锁的线程稍等一会。
在同一个对象锁上，成功自旋的次数越多，可以等待的时间越久(自适应自旋)
* 轻量级锁(1.6引入) 将对象头拷贝进栈帧，CAS对象头设置为指向LockRecord的指针，成功则更新对象头锁标记为00。解锁时将对象头CAS设置回初始值。
在争夺较少的情况下，轻量级加锁和解锁等价于一次CAS赋值操作。否则会比传统重量级锁更加慢。
* 偏向锁 在获取轻量级锁的时候，记录获取该锁线程ID，后续该线程再次获取该锁时，无需做任何操作。倘若其他线程要求获取锁，则偏向锁失效。

