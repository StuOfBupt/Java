##### 1、并发 & 并行

- 并发：在多个处理器上处理多个任务
- 并行：在一个处理器上处理多个任务

##### 2、线程 & 进程

- 进程
  - 操作系统资源分配的基本单位
  - 不同进程起始地址不同
- 线程
  - 操作系统调度的基本单位
  - 多个线程共享内存地址
  - 一个线程崩溃则进程崩溃

##### 3、创建线程的方法

- 继承Thread类重写run方法
- 实现Runnable接口
- 实现Callable接口

##### 4、Runnable & Callable

- Runnable没有返回值，Callable有返回值

##### 5、Sleep & Wait

- sleep（）来自Thread，wait（）来自于object
- sleep（）不释放锁，wait释放锁
- sleep（）到时间自动唤醒，wait（）可以使用notify（）或者notifyAll（）直接唤醒

##### 6、notify & notifyAll

- notifyAll()会唤醒所有的线程，notify()之后唤醒一个线程。notifyAll() 调用后，会将全部线程由等待池移到锁池，然后参与锁的竞争，竞争成功则继续执行，如果不成功则留在锁池等待锁被释放后再次参与竞争。而 notify()只会唤醒一个线程，具体唤醒哪一个线程由虚拟机控制

##### 7、创建线程池的方式

- newSingleThreadExecutor()：它的特点在于工作线程数目被限制为 1，操作一个无界的工作队列，所以它保证了所有任务的都是被顺序执行，最多会有一个任务处于活动状态，并且不允许使用者改动线程池实例，因此可以避免其改变线程数目；
- newCachedThreadPool()：它是一种用来处理大量短时间工作任务的线程池，具有几个鲜明特点：它会试图缓存线程并重用，当无缓存线程可用时，就会创建新的工作线程；如果线程闲置的时间超过 60 秒，则被终止并移出缓存；长时间闲置时，这种线程池，不会消耗什么资源。其内部使用 SynchronousQueue 作为工作队列；
- newFixedThreadPool(int nThreads)：重用指定数目（nThreads）的线程，其背后使用的是无界的工作队列，任何时候最多有 nThreads 个工作线程是活动的。这意味着，如果任务数量超过了活动队列数目，将在工作队列中等待空闲线程出现；如果有工作线程退出，将会有新的工作线程被创建，以补足指定的数目 nThreads；
- newSingleThreadScheduledExecutor()：创建单线程池，返回 ScheduledExecutorService，可以进行定时或周期性的工作调度；
- newScheduledThreadPool(int corePoolSize)：和newSingleThreadScheduledExecutor()类似，创建的是个  ScheduledExecutorService，可以进行定时或周期性的工作调度，区别在于单一工作线程还是多个工作线程；
- newWorkStealingPool(int parallelism)：这是一个经常被人忽略的线程池，Java 8 才加入这个创建方法，其内部会构建ForkJoinPool，利用Work-Stealing算法，并行地处理任务，不保证处理顺序；
- ThreadPoolExecutor()：是最原始的线程池创建，上面1-3创建方式都是对ThreadPoolExecutor的封装

##### 8、synchronized锁升级过程

- 锁对象的对象头中有threadid字段，在第一次访问时 threadid 为空，jvm 让其持有偏向锁，并将 threadid 设置为其线程 id，再次进入的时候会先判断  threadid 是否与其线程 id  一致，如果一致则可以直接使用此对象，如果不一致，则升级偏向锁为轻量级锁，通过自旋循环一定次数来获取锁，执行一定次数之后，如果还没有正常获取到要使用的对象，此时就会把锁从轻量级升级为重量级锁。