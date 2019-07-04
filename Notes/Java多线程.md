## Java多线程基础

**同步异步、阻塞非阻塞？**[怎样理解阻塞非阻塞与同步异步的区别](https://www.zhihu.com/question/19732473)

> 简单地说，同步与非同步是针对消息应答来说，阻塞与非阻塞是针对调用者状态来说。	如果调用者发出调用后，在没有得到结果之前，该调用就不返回，而是需要调用者轮询，那就是同步；而不论有没有完成调用，调用者发出调用后都立即返回（虽然没有结果），应答者过一段时间完成调用后发出一个信号（比如回调函数）通知调用者完成了调用，则是异步。	而阻塞与非阻塞是说，调用者在等待调用结果的时间里，是挂起不干事还是去干别的事。同步可以非阻塞也可以阻塞，异步可以非阻塞也可以阻塞，常见的是同步阻塞与异步非阻塞。
>

**临界资源**：临界资源是指每次仅允许一个进程访问的资源。各进程采取互斥的方式，实现共享的资源称作临界资源。硬件有打印机、磁带机等,软件有消息缓冲队列、变量、数组、缓冲区等。**临界区**：每个进程中访问临界资源的那段代码称为临界区，每次只允许一个进程进入临界区，进入后，不允许其他进程进入。

**如何创建新线程？**继承thread类、实现Runnable接口、实现Callable接口、**从线程池中创建**

> <u>继承Thread类</u>：注意区分线程运行的区别：run & start，run是直接调用运行方法真开始跑，但没有开新线程，没有并发，start是使线程就绪、需要得到cpu时间片才能跑
>
> <u>实现Runnable接口</u>：实现Runnable接口，重写run方法，创建实现类实例，将实例用Thread包装再start；
>
> <u>实现Callable接口</u>：创建Callable接口的实现类，并重写call()方法，该call()方法将作为线程执行体，并且有返回值；创建Callable实现类的实例，使用FutureTask类来包装Callable对象，该FutureTask对象封装了该Callable对象的call()方法的返回值；使用FutureTask对象作为Thread对象的target创建并启动新线程；调用FutureTask对象的get()方法来获得子线程执行结束后的返回值。！！！

对比：

> - 使用Runnable & Callable接口：线程类只是实现了接口，还可以继承其他类。在这种方式下，多个线程可以共享**同一个target对象**，所以非常适合多个相同线程来处理**同一份资源**的情况，从而可以将CPU、代码和数据分开，形成清晰的模型，较好地体现了面向对象的思想。编程稍微复杂，如果要访问当前线程，则必须使用`Thread.currentThread()`方法。
> - 继承Thread类：编写简单，如果需要访问当前线程，则无需使用`Thread.currentThread()`方法，直接使用this即可获得当前线程。劣势是：线程类已经继承了Thread类，所以不能再继承其他父类。

**线程状态切换** 被锁阻塞blocked，响应中断或挂起 waiting，timed_waiting

> 1. 初始(NEW)：新创建了一个线程对象，但还没有调用start()方法。
> 2. 运行(RUNNABLE)：Java线程中将就绪（ready）和运行中（running）两种状态笼统的称为“运行”。
> 线程对象创建后，其他线程(比如main线程）调用了该对象的start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获取CPU的使用权，此时处于就绪状态（ready）。就绪状态的线程在获得CPU时间片后变为运行中状态（running）。
> 3. 阻塞(BLOCKED)：表示线程阻塞于锁。
> 4. 等待(WAITING)：进入该状态的线程需要等待其他线程做出一些特定动作（通知或中断）。
> 5. 超时等待(TIMED_WAITING)：该状态不同于WAITING，它可以在指定的时间后自行返回。
> 6. 终止(TERMINATED)：表示该线程已经执行完毕

![img](assets/20181120173640764.jpeg)

**obj.wait()、obj.notify()、与等待队列、同步队列**

> - 调用obj的wait(), notify()方法前，必须获得obj锁，也就是必须写在synchronized(obj) 代码段内。
> - 线程1获取对象A的锁，正在使用对象A。
> - 线程1调用对象A的wait()方法。
> - 线程1释放对象A的锁，并马上进入等待队列。
> - 锁池（同步队列）里面的对象争抢对象A的锁。（简言之，同步队列里面放的都是想争夺对象锁的线程。）
> - 线程5获得对象A的锁，进入synchronized块，使用对象A。
> - 线程5调用对象A的notifyAll()方法，唤醒所有线程，所有线程进入同步队列（锁池）。若线程5调用对象A的notify()方法，则唤醒一个线程，不知道会唤醒谁，被唤醒的那个线程进入同步队列。
> - notifyAll()方法所在synchronized结束，线程5释放对象A的锁。
> - 同步队列的线程争抢对象锁，但线程1什么时候能抢到就不知道了，如果获得锁，进入RUNNABLE状态，否则进入BLOCKED状态等待获取锁。
>

![img](assets/20180701221233161-1562202105433.jpg)

**线程基本操作**：yield，join，wait，notify，sleep，interrupt

> yield一定是当前线程调用此方法nowThread.yield()，当前线程放弃获取的CPU时间片，但不释放锁资源，由运行状态变为就绪状态，让OS再次选择线程，yield()不会导致阻塞。
>
> join/join(millis)==是当前线程里调用其它线程==otherThread的join方法，当前线程进入WAITING/TIMED_WAITING状态，当前线程不会释放已经持有的对象锁。在A线程中调用了B线程的join()方法时，表示只有当B线程执行完毕时，A线程才能继续执行。线程otherThread执行完毕或者millis时间到，当前线程一般情况下进入RUNNABLE状态，也有可能进入BLOCKED状态（因为join是基于wait实现的）。其实，join方法是通过调用线程的wait方法来达到同步的目的的。例如，A线程中调用了B线程的join方法，则相当于A线程调用了B线程的wait方法，在调用了B线程的wait方法后，A线程就会进入阻塞状态，当B线程执行完（或者到达等待时间），B线程会自动调用自身的notifyAll方法唤醒A线程，从而达到同步的目的。
>
> wait 当前线程调用对象的wait()方法，当前线程释放对象锁，让出CPU，进入等待队列，依靠notify()/notifyAll()唤醒或者wait(long timeout) timeout时间到自动唤醒。obj.notify()唤醒在此对象监视器上等待的单个线程，选择是任意性的。notifyAll()唤醒在此对象监视器上等待的所有线程，但是调用notify方法的线程并不马上释放锁，还是会继续执行完。二者结合与synchronized关键字使用，可以建立很多优秀的同步模型。
>
> >  [Java 多线程编程之：notify 和 wait 用法](https://segmentfault.com/a/1190000018096174)：
> >
> > - 无论是执行对象的 wait、notify 还是 notifyAll 方法，必须保证当前运行的线程取得了该对象的控制权（monitor）
> > - 我们可以通过同步锁来获得对象控制权，例如：synchronized 代码块。
>
> sleep 一定是当前线程调用此方法，当前线程进入TIMED_WAITING状态，但不释放对象锁，millis后线程自动苏醒进入就绪状态。

**JMM内存模型** 

[Java 内存模型 JMM 深度解析](https://juejin.im/post/5a27ab3851882546d71f36e1)             [全面理解Java内存模型(JMM)及volatile关键字](https://blog.csdn.net/javazejian/article/details/72772461)               [Java内存模型（JMM）总结](https://zhuanlan.zhihu.com/p/29881777)

