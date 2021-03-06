### Python

1. try finally。就算try与except中的return语句在返回之前会执行finally中的内容。
2. new()与init()。new()负责分配内存，init()负责初始化对象。
3. 列表推到与迭代器。列表推导会将数据一次性加载到内存中，而迭代器不会。
4. 装饰器。对函数进行统一封装。
5. from functools import wraps。wraps用于保留原有函数的属性。
6. 常用的设计模型：
   1. 单例模式：保证一个类仅有一个实例，并提供一个访问他的全局访问点，例如框架中的数据库连接
   2. 装饰器模式：不修改元类代码和继承的情况下动态扩展类的功能，例如框架中的每个controller文件会提供before和after方法。
   3. 迭代器模式： 提供一个方法顺序访问一个聚合对象中各个元素，在PHP中将继承 Iterator 类
   5. 工厂模型
   6. 模板模式
   6. 代理模式
7. python的垃圾回收机制。
   1. 引用计数
   2. 分代回收
   3. 标记-清除
8. python的GIL锁机制。
9. python的协程机制。比线程更加轻量级，遇到IO或完成执行时进行切换。
10. python多继承问题
    1. 按顺序继承，哪个父类在最前面且它又有自己的构造函数，就继承它的构造函数；
    2. 如果最前面第一个父类没有构造函数，则继承第2个的构造函数，第2个没有的话，再往后找，以此类推。
11. gevent小结
    1. 什么是协程，协程的切换是怎么做到的 ？
    2. 协程的调用栈切换
12. monkey补丁：monkey patch指的是在运行时动态替换
13. type和isinstance的区别
    - type获取一个实例所属的类
    - isinstance可以判断一个实例是否为一个或多个类的子类
14. python元类
    - 使用type创建类，type("A", (B, C), {"a": 1, "b", print_hello})
    - type是元类，所有类从type创建而来



### 网络知识

1. socket长连接和短链接
   1. 长连接适用于并发低，多操作的任务。短连接适用于高并发，少操作的任务。
2. select、poll和epoll
   1. select查询复杂度为O(n)，当出现网络IO事件时并不知道是发生在连接池中的哪一个，需要遍历一次。
   2. 和select没有区别，查询复杂度还是为O(n)，仅将连接池用链表实现。
   3. epoll为事件驱动，当IO事件发生时会通知那个连接发生了事件。
      - epoll的触发方式 什么是水平触发和边缘触发？
3. tcp和udp协议，tcp三次握手和四次挥手
   1. upd是无连接的为什么需要udp而不直接使用ip协议呢？ip协议是网络层协议，只定义了网络中的计算机；而tcp协议是传输层协议，在ip层的基础上还定位了计算机中的进程。
   2. 两种协议的区别：面向连接VS无连接、可靠性、有序性、速度、量级(报文头的大小)。
   3. tcp滑动窗口和拥塞控制
4. TIME_WAIT过多时因为什么：1.服务器过载。2.网络延迟。主动关闭进入CLOSE_WAIT，被动关闭进入TIME_WAIT。
5. http一次连接的全过程
   - 域名解析 -》 建立tcp连接 -》请求目标服务器 -》 得到返回结果 -》 浏览器解析网页 -》 下载资源并渲染 -》 关闭tcp连接
6. http连接方式，post和get的区别
   1. get发送的数据会记录在url中，而post不会
   2. get能存书签，post不能
   3. get有数据大小的显示，post没有
7. https和http的区别？https在http的基础上添加了加密的安全协议。
   1. HTTPS需要到CA申请证书
   2. HTTPS密文传输，HTTP明文传输
   3. 默认端口不同
   4. https并不一定安全，因为用户可能会默认使用http造成第三方劫持
8. restful是什么？http接口的定义规范。
9. http状态码？
10. https整数签发过程
11. http复用？
    - 通过标识位来区分不同的http请求。
12. tcp如何保证稳定传输？
    - 拥塞控制、滑动窗口、流量控制、ack机制
13. tcp重排序
    - tcp接收端将乱序报文重排序后交给上层应用
14. http的request和response有哪些字段，有哪些区别？
15. http缓存
    1. 强缓存：未过期就直接返回
    2. 对比换粗：向服务器询问缓存是否有效
16. 负载均衡
17. TCP服务端可以先关闭连接
18. http请求头有哪些字段？
    - accept-data-type、content-type、cookie、user agent、cache control。



### 数据库

1. mysql和PostgreSQL的区别
2. mysql的引擎InnoDB是怎么实现的？
3. mysql的数据一致性
4. 如何设计一个数据库系统？
5. 数据库索引模块
   - 使用二叉树，树的深度会很大，造成IO次数也会很大
   - B+树的优势？
     - 非叶子节点的子树指针与关键字个数相同
     - B+树非叶子节点不存数据，查询效率更稳定，高度也可以更矮
     - 所有叶子节点都有指向下一个叶子的指针，方便范围统计和数据遍历
   - Hash索引的缺点
     - 无法进行范围查询，只能使用"="、"IN"
     - 数据是无序的
     - 不能利用部分索引键查询
     - 不能避免表扫描
     - 遇到hash碰撞时效率会很低、查询效率不稳地
   - BitMap不适合高并发场景
   - 密集索引和稀疏索引的区别
     - 密集索引叶子节点会存整条数据
     - 稀疏索引叶子节点只存键值和指针
     - InnoDB的索引选择
       - 若一个主键被定义，则该主键则作为密集索引
       - 若没有主键定义，则该表的第一个唯一非空索引为密集索引
       - 若不满足以上条件，innodb内部会生成一个隐藏主键
       - 非主键索引存储相关键位和其对应的主键值，包含两次查询
   - 如何分析数据库慢查询
     1. 通过日志找到慢查询的sql语句
     2. **用explain命令分析sql**
        1. select_type：primary：复杂查询中最外层select，subquery：包含在select中的子查询，derived包含在from中的子查询mysql会创建一张临时表来保存
        2. table：语句访问的数据表
        3. type字段：速度all<index
        4. possible_keys：可能使用的索引
        5. key：实际使用的索引
        6. extra字段：using filesort外部排序会很慢，using temporary对查询结果使用临时表会很慢
     3. 修改sql，让sql使用索引
   - **联合索引的最左匹配原则**
     - 联合索引的底层结构？是一棵树还是多棵树，树中的索引值时如何确定的？
       - 联合索引只有一棵树，树中的值选用的是联合索引的第一列，联合索引和普通索引的区别在于联合索引的叶子节点存了多个值
   - MyISAM与InnoDB关于锁方面的区别是什么
     1. MyISAM默认使用的是表级别的锁，不支持行级锁
     2. InnoDB默认是行级锁，也支持表级锁
   - 数据库索引越多越好？
     - 过多的索引会带来维护成本的提升，如删除和插入操作
     - 过多的索引会占用不必要的磁盘空间
     - 数据量过少时加入索引反而会降低系统性能
     - 当数据列的取值数目/行数很小时索引的性能也不高
6. 数据库transaction、悲观锁乐观锁、CAS算法
7. 什么是数据库范式？
   - 1NF：同一列不能有多个值，数据库的每一列都是不可分割的基本数据项。
   - 2NF：属性依赖于主属性，每个实例必须被主属性唯一标识。
   - 3NF：属性不依赖其它非主属性。
   - BCNF：主属性不依赖于主属性。
8. **数据库事务机制**
   1. 特性：原子性A、一致性C、隔离性I、持久性D
   2. 事务并发引起的问题：
      1. 更新丢失：
9. **数据库锁**
   1. select * from xxx for update;为select语句创建排他锁
   2. MyISAM的锁调度
      1. 读写锁互斥。
      2. 写优先级高于读，即使读进程优先进入等待队列，写进程也会优先执行。高并发环境下容易造成读进程的饥饿。
   3. InnoDB
      1. 特性：支持事务机制、使用了行级锁
      2. 只有使用索引时才会触发行锁，否则将使用表锁
   4. innodb的意向锁
      1. 为什么要设置意向锁？当事务A获得行的读锁后，事务B想要获取表的写锁，解决获取判断的低效。
10. MyISAM和InnoDb的比较
   1. MyISAM的优势：
      - 频繁执行count语句，mysiam用一个全局变量保存了行数
      - 对数据的更改频率不高，但查询非常频繁
      - 没有事务
   2. InnoDB
      - 增删改查都非常频繁
      - 可靠性要求比较高，要求支持事务
11. InnoDB如何避免幻读？Gap锁
    1. Gap锁：
       - 如果where条件命中全部，则不会使用Gap锁
       - where条件部分命中或全没有命中，会加Gap锁
12. MYSQL优化
    - SQL语句优化
      - max()加索引能让查询时间保持常数
      - count(id)和count(*)，count(id)会忽略id为空的列，count语句尽量在一次扫描的情况下得到结果
      - 子查询优化：改为join查询
      - group by的优化：
      - limit查询的优化：使用主键进行order by 加 where id>xxx
    - 索引的优化
      - 哪些列需要建立索引？
        - 在where、group by、order by、on中出现
      - 建立索引的依据
        - 索引字段占用空间越小越好，数据库索引放在页中，索引字段越小io次数越少
        - 索引的取值个数多的列应该放在联合索引的前面
      - 什么样的索引是不必要的？
        - 主键上建唯一索引
        - 主键出现在联合索引中
        - 多个联合索引的前缀相同
        - 不再使用的索引
    - 表结构的优化
      - 选择合适的数据类型
        - 使用可以存下所有数据的最小数据类型
        - 使用简单的数据类型。Int比varchar类型要简单
        - 尽可能使用not null，非not null为空的数据需要使用额外的空间
        - 尽量少使用text类型，非用不可时最好考虑分表
      - 范式化
        - 让表的结构满足第三范式
        - 查询需要关联的表太多就需要反范式化
      - 表的拆分
        - 垂直拆分
        - 水平拆分(谨慎操作)
    - 系统的优化
      - 操作系统优化
        - tcp支持的队列数
        - 打开文件数的限制
        - 关闭iptables等软件防火墙
      - msyql配置优化
        - max_connnection：最大连接数
        - innodb_buffer_pool_size：mysql缓存池，尽可能的大，推荐>=所有表的空间+索引空间
        - innodb_buffer_pool_instances：缓存池的数量，数量越多并发性能越好
        - innodb_log_buffer_size：能存下一秒钟的日志就行
        - innodb_flush_log_at_trx_commit：对innodb的IO效率影响最大。默认为1，可以取0,1,2。一般设置为2，对安全性要求高则设置为1。0：每秒写入文件且flush，1：每次次提交写文件且flush，2：每次提交写，每秒flush。
        - innodb_read_io_threads：读线程数
        - innodb_write_io_threads：写线程数
        - innodb_file_per_table：默认关闭，控制每个表是否使用独立的文件空间
        - innodb_stats_on_metadata：决定了mysql在什么时候会刷新统计信息
      - 硬件优化
        - cpu：单核更快
        - 硬盘：RAID、SNA和NAT顺序读写效率高但顺序读写效率低故不推荐使用。
13. mysql分布式

    1. 一主一从：

       1. 主负责写请求，从负责读请求，从通过binlog向主同步数据。

    2. 多主多从：

       - 将数据水平拆分，分布在不同的机器上，通过hash取模的方式得到对应的路径

       - 如何解决跨表查询：
         - 尽量让查询的数据分布在同一张表中。1）按照查询的键对数据分片 2）按照时间进行分片。
         - 数据分片冗余。
       - 数据分片后如何做数据迁移？
         - 根据id或时间戳做判断，小于的走原先的路由规则，大于的走新的路由规则。
    3. mysql主从切换机制？
14. 分布式事务
15. 什么是回表？
    - 利用二级索引无法定位数据，需要再次查询主键索引
    - 避免回表查询的方法就是建立联合索引
18. mysql中join
    - join执行过程
    - 如何优化join
19. 什么是覆盖索引？联合索引包含所有需要查询的字段称为覆盖索引，覆盖索引无需回表。



### 数据结构和算法

1. 完全乱序的数据哪个排序算法最好？堆排序。
2. 将除法的结果用字符串返回，如果能够除尽，则返回相除的结果，如果不能除尽，则无限循环部分用[]标记
3. 蓄水池采样
4. Floyd算法
5. 什么是稳定排序，相同值的元素在排序后相对位置不发生改变



### 面向对象编程基础知识

1. Overload、Overwrite、Override的区别
2. C++如何实现多态
3. 虚函数表实现机制，虚函数表的继承
4. c++的copy机制



### 设计模式

1. DAO层、Service层、控制层。为什么会存在Service层？
2. 多态底层是如何实现的？



### Java

1. String、StringBuffer、StringBuilder的区别，怎么理解String不变性？String类是否能被继承？
   - 速度 StringBuilder > StringBuffer > String。StringBuilder和StringBuffer可以直接修改，但是StringBuilder不是线程安全的。String和StringBuffer都是线程安全的，但是String不能修改。
   - String为什么是不变的
2. ==和equals的区别，如果重写了equals()不重写hashCode()会发生什么
   - ==是对内存地址进行比较，而equals是对字符串的值进行比较。
   - 如果不重写hashCode方法，那么两个euqals的对象hashcode可能会不同，这是不符合规定的。
   - 默认的equals只比较两个对象的内存地址，所以必须重写
3. package关键字的作用
4. volatile保证了可见性和有序性。volatile怎么保证可见性？直接将修改写入内存，并且直接从内存读取数据。
   1. 可见性：一个线程对变量的修改，其它程序能够立刻知道。
   2. 原子性：操作要么不执行，要么全部执行，不会出现被打断的情况。
   3. 有序性：最终执行指令的顺序与代码中指定的顺序一致。
5. synchronized和lock对比
   1. synchronized是java内置关键字，lock只是一个类
   2. synchronized无法判断是否获取锁的状态，lock可以判断是否获取到锁
   3. synchronized可以自动释放锁，lock需要手动释放
   4. synchronized修饰的代码块只能由一个线程单独访问，其它线程必须一直等待。lock则能在等待一定时间后进行其它操作。
   5. synchronized可重入、不可中断、非公平。Lock可重入、可判断、可公平也可不公平。
   6. synchronized适合少量代码的同步。lock适合大量代码的同步。
   7. synchronized修饰方法和代码块有什么区别？修饰静态方法锁class、修饰非静态方法锁this、修饰代码块锁任意对象。
6. sleep和wait的区别，sleep会不会释放锁，wait需要notify和notifyAll来唤醒
7. 线程的局部变量和线程池参数
   - 如何使用线程池？
8. 产生死锁的条件？如何防止死锁？：
   - 互斥、占有且等待、不可抢占、循环等待
9. 多继承和多实现
10. **synchronize的实现机制**
11. java抽象类和接口，接口可以继承多个接口,类可以实现多个接口
12. java中为什么要写非static方法
    - static方法在类加载的时候就会分配内存空间，且不会被GC。非static方法在实例化时才会分配内存空间，能被GC。
    - 方法放在哪块内存中？
13. **线程等待时位于哪个区域，具体讲一下**
14. Java堆溢出问题怎么处理，内存泄漏和内存溢出的区别
15. 什么是可重入锁？java中有哪些可重入锁。
16. jvm调优做过没，-Xms、-Xmx、-Xss分别指什么
    - -Xss为每个线程虚拟机堆栈的大小。-Xms表示java堆的初始大小。-Xmx代表java堆的最大拓容大小。
17. ConcurrentHashMap 为什么放弃了分段锁，有什么问题吗，如果你来设计，你如何设计。[ConcurrentHashMap](https://blog.csdn.net/fanrenxiang/article/details/80435459)
18. **继承和聚合的区别。**
19. Java反射机制
20. **nio、 bio、aio 的区别是啥**
21. **Class.forName 和 ClassLoader 区别**
22. final 的用途
    - final 修饰的类叫最终类，该类不能被继承。
    - final 修饰的方法不能被重写。
    - final 修饰的变量叫常量，常量必须初始化，初始化之后值就不能被修改。
    - final修饰形参，形参不可改变。
23. public, private, protected
24. 异常处理
    1. error和exception的区别
       - exception是可以预期的异常。error是不可预期的异常。
    2. throws和throw的区别
       - throws用于方法的声明，throw用于代码块中
       - throws表示抛出异常的可能，throw则是执行抛出异常的动作
25. ArrayList 和 Vector 的区别是什么？ ArrayList和Vector都是通过数组实现的，Vector是线程安全的但访问效率低。LinkedList不是线程安全的。
26. Java线程的创建方式和生命周期
    - 创建：继承Thread类，实现Runnable/Callable接口，通过线程池启动
    - 使用runnable接口创建线程有哪些优势？避免单继承的局限性、增强了程序的拓展性
    - 生命周期：创建、就绪、运行、阻塞、完成
    - sleep不会释放锁、yield(运行->就绪)不会释放锁、wait(运行->阻塞)会释放锁
27. IOC和AOP的区别、静态代理、动态代理、CGLIB代理
    1. 静态代理必须为每一种类实现代理类，动态代理则能为任意的类实现代理。
28. 问你最后一个left join、right join和自然连接的区别、内连接和外连接
    - 自然连接中任意两条记录只有连接的键是一致的。除了连接键外不完全一致。
29. 接口实现的方法必须是public的。
30. 内部类和普通类的区别，内部类和静态内部类的区别
    - 内部类定义在普通类的内部，普通类能访问内部类的私有方法和属性。
    - 外部类的静态方法只能访问静态内部类，外部类的非静态方能能访问所有静态内部类。
31. static修饰的成员变量运行时机
    - static变量会被加载到方法区中，且在加载时就会被初始化。
32. HashMap中的put和resize，HashMap和HashTable的区别。
    1. hashtable通过对put、resize、get方法加synchronized关键字来实现同步
33. ConcurrentMap
34. 泛型中？和T的区别
35. 创建对象的方式有哪些？new、Class类的newInstance、Constructor类的newInstance、clone、反序列化
36. 如何对对象进行clone？如何实现深拷贝和浅拷贝？可以利用序列化进行对象的深拷贝。
37. 什么是匿名内部类？如何使用匿名内部类？
    - 匿名内部类必须实现父类或接口的所有抽象方法、且所有方法不能为抽象方法
    - 不能实现构造方法
    - 不能定义任何静态成员变量或方法
38. spring中bean的生命周期？
39. java中线程是如何被创建的？
40. **偏向锁、轻量级锁、重量级锁**
41. 如何实现原子操作？锁和cas。java中的cas是如何实现的？
42. 如何理解信号量
43. 接口和类的区别
    - 接口没有构造方法
    - 接口的所有方法必须是抽象的
    - 不能包含没有初始化的成员变量，且成员变量的默认继承权限为private
    - 类可以实现多个接口，但只能继承一个父类
    - 接口中的方法会被隐式的定义为public abstract
44. abstract和static不能同时使用
45. 如何理解native关键字？
46. static修饰的代码块的执行顺序是什么？
47. 什么是this引用的溢出？
48. 为什么CAS比Synchronized要快？
49. lambda表达式与接口
    1. 只有一个方法的接口是函数式接口
    2. 函数式接口与lambda表达式等价
50. **高并发编程**
    - AtomicLong、LongAddr实现原理。AtomicReference、AtomicIntegerFieldUpdater、AtomicStampReference、AtomicLongArray用法。
    - sychronized、Lock和Atomic的区别
    - sychronized的可见性，解锁前把最新值写入主内存，加锁前从主内存中读取最新值。
    - volatile通过内存屏障和禁止重排序优化（在指令中插入标记）来实现有序性。使用场景：多线程的标识位、double check。
    - **[发布与溢出](https://coding.imooc.com/lesson/195.html#mid=12305)**：
      - 懒汉模式：volatile + 双重检测 禁止指令重排来保证线程安全。
      - 懒汉模式：synchronized
      - 恶汉模式：静态加载
      - 使用枚举
    - 不可变对象：
      - 对象创建后就不能改变
      - 所有域都是final
      - 对象是正确创建的（在对象创建期间没有this引用的溢出）
      - Collections.unmodifiableXXX
      - ImmutableXXX
    - [线程封闭](https://coding.imooc.com/lesson/195.html#mid=16665)：
      -  ad-hoc、堆栈封闭、ThreadLocal
      -  threadlocal什么情况下会产生内存泄漏？new ThreadLocal().set(1)
      -  如何避免ThreadLocal内存泄漏？将ThreadLocal设置为弱引用
    - 常见线程不安全类
      - StringBuilder(不安全)、StringBuffer(不安全)、SimpleDateFormat(不安全)、DateTimeFormater(DateTime.parses、安全)、ArrayList(不安全)
      - if(condition(a)){handle(a);} 容易触发线程安全问题
    - 同步容器(线程安全容器)
      - Vector(不一定线程安全)、Stack、HashTable、Collections.synchronizedXXX
      - **遍历集合的时候不要进行remove操作，不安全。**
    - 并发容器(J.U.C)
      - CopyOnWriteArrayList、CopyOnWriteArraySet、ConcurrentSkipListSet
      - ConcurrentHashMap、ConcurrentSkipListMap
    - AQS(AbstractQueuedSynchronizer)
      - 底层是如何实现的？
        - 底层使用一个双向链表(Sync queue，Condition queue)
        - 使用int表示状态
      - 使用特性
        - 子类通过实现acquire和release的方法操纵状态
        - 可以同时实现排它锁和共享锁
      - 什么是公平锁和非公平锁？公平锁：FIFO。非公平锁：非FIFO。RentrantLock公平策略可选，synchronized是非公平锁。
      - CountDownLatch
      - Semaphore
      - CyclicBarrier
      - ReentrantLock
        - 独有功能：可指定公平锁还是非公平锁。提供了一个Condition类，可以分组唤醒需要唤醒的线程。提供能够中断等待锁的线程的机制。
      - ReentrantReadWriteLock
        - 没有任何读写锁的情况下才能获得写锁
      - StampLock
    - **锁的选用技巧**：
      - 少量竞争选用synchronized
      - 竞争者不少，线程增长的趋势能够预估，使用ReentrantLock
    - Future接口、FutureTask类。二者的区别？Future是接口、FutureTask是Runnable的实现类可以放入线程池中运行。
    - Fork/Join
    - BlockingQueue
      - ArrayBlokingQueue
      - DelayQueue
      - LinkedBlockingQueue
      - PriorityBlockingQueue
      - SychronousQueue
    - 线程池
      - 为什么要使用线程池？
        - 重用存在的线程，减少对象的创建，降低开销
        - 可有效控制最大并发数，提高系统资源利用的效率
        - 提供定期执行，定期指定等高级功能
      - ThreadPoolExecutor主要参数：
        - corePoolSize：核心线程数量
        - maximumPoolSize：线程最大线程数量
        - workQueue：阻塞队列，存储等待执行的任务
        - keepAliveTime：阻塞线程的超时等待时间
        - unit：时间单位
        - threadFactory：线程工厂，用来创建线程
        - rejectHandler：当拒绝处理任务时的策略
      - 线程池的运行状态？
        - submit和execute的区别
        - shutdown和shutdownNow的区别
      - **多线程并发最佳实践**
        - 使用本地变量
        - 使用不变类
        - 最小化锁的作用范围
        - 使用线程池
        - 宁可使用synchronized也不要使用wait和notify
        - 使用BlokingQueue实现生产-消费模式
        - 使用并发集合而不是加了锁的同步集合
        - 使用Semaphore创建有界访问
        - 宁可使用同步的代码块也不适用同步的方法
        - 避免使用静态变量
    - Spring与线程安全
      - Spring不保证线程安全
    - HashMap与ConcurrentHashMap
      - HashMap在多线程的环境下容易造成死循环
    - 缓存
      - Guava Cache
      - memcache
      - redis
51. 怎么破坏单例：反射、克隆、序列化
52. 包装类、装箱与拆箱
    - 装箱的发生时机？发生赋值和运算时会装箱。
    - 装箱的性能损耗？对象不能直接运算必须拆箱再装箱。
53. final、finally、finalize
54. final和volatile的异同：final通过保证变量的不可更改来确保线程安全。volatile只确保了有序性和可见性，并不保证原子性。
55. Java中有哪些线程安全的List：Vector、CopyOnWriteArrayList、Collections.synchronizedList
56. java中的BlockingQueue：SychronousBlockingQueu，ArrayBlockingQueue、LinkedBlockingQueue
57. java中守护线程和非守护线程的区别：守护线程在主线程结束后就结束了，非守护线程则会一直运行
58. 四种引用方式：
    - 强引用(不会GC)、软引用(内存不足时GC)、弱引用(执行GC时会被释放)、虚引用(随时可能被GC)
59. 处理hash冲突的方法
    - 拉链法、再哈希、开放寻址
60. 如何理解Java中的Collection：所有容器的接口
61. java中List和Array的区别？如何实现list和array的互转？
    - String[] array = list.toArray(new String[list.size()]);
    - List\<String\> list = Arrays.asList(array);
62. Collections的作用
63. Collection和Collections的区别
64. Buffer类的作用
65. 缓存击穿、缓存穿透、缓存雪崩
    - 缓存击穿：缓存和数据库中都没有数据。设置数据校验机制避免非法数据库访问。
    - 缓存穿透：缓存中没有数据，数据库中有数据。热点数据不过期，锁住访问数据的代码。
    - 缓存雪崩：大量数据同一时间过期，数据库并发查询压力巨大。热点数据永不过期、过期数据设置随机缓存时间。
66. 为什么使用泛型而不用Object代替？
    - Object需要强转类型，强转类型可能会造成信息丢失
    - 泛型的代码可读性好，自动转换类型
67. 手写对象池
68. RentrantLock底层实现机制
69. 如何终止线程？interrupt()、stop()
    - 二者的区别：
      - interrupt会抛出异常，并且会在线程内对异常进行处理，也会执行异常后的代码
      - stop则直接杀死线程
70. 泛型重载
71. Thread.sleep(0)：cpu让位。yield会让出锁，sleep不会让出锁
72. HashMap的拓容因子为什么是0.75？hash桶内的节点个数服从泊松分布，根据桶内节点数量小于等于8可求得拓容因子为0.75。
73. java程序运行的过程？源代码-》字节码-》机器码
74. 匿名内部类为什么默认使用final？



### C++

1. static关键字用法
2. 为什么要有虚析构函数，构造函数可以是虚函数吗
3. const关键字
4. 智能指针
5. new分配的物理内存还是虚拟内存



### Scala

1. Scala和Java的区别



### JVM

1. [垃圾回收机制](https://www.zhihu.com/question/19912197/answer/113385483)
   1. 标记-复制：将可用的内存块复制到一块新的完整内存中。适用于频繁回收的内存块。
   2. 标记-清除：将还未回收的往前移动，适用于不频繁回收的内存块。
   
2. 类加载机制，类加载器，双亲委派模型

3. JVM垃圾处理方法，对象什么时候进入老年代，什么时候进行FullGC
   
   - 进入老年代的条件：大于阈值的对象、存活时间超过阈值。MirrorGC的条件：新生代满了。FullGC的条件：老年代区域满了。
   
4. JVM如何加载.class文件？通过ClassLoader进行加载
   1. BootStrapClassLoader: c++编写的用于加载java的核心库
   2. ExtClassLoader：java编写，加载拓展类
   3. AppClassLoader: java编写，加载程序的类
   4. 自定义ClassLoader：用于自定义
   
5. 什么是双亲委派模式？为什么要这么做？避免多份同样的字节码被加载

6. 什么是反射？在运行时能任意的调用和获取对象的所有方法和属性。

7. 类的加载
   1. 类被加载后会存放在MetaSpace中，MetaSpace中也会有垃圾回收机制
   2. 同一种类加载器一般只会有一个实例
   3. 隐式加载：new。显式加载：loadClass和forName
   4. loadClass和forName的区别？
      1. loadClass只读取类的字节码，forName在loadClass的基础上还会链接和做初始化操作
   
8. MetaSpace与PermGen的比较
   1. 方法区、MetaSpace、PermGen的区别？
      1. 方法区是java虚拟机的规范，MetaSpace和PermGen是具体的实现
      2. MetaSpace出现于JDK1.7以后，用于替换PermGen
      3. MetaSpace存储在native memory中
   2. 字符串常亮存在永久代中，容易出现性能问题
   3. 类和方法的信息大小难以确定，给永久代大小指定带来困难
   4. 永久代为GC带来麻烦
   5. JDK1.7以后常量池放入堆中
   
9. **String.intern的理解**

10. jvm的启动模式
    1. client：gc串行，启动速度快，代码编译器效率低
    2. server：gc并行，启动速度慢，代码编译器效率高
    
11. 常用的垃圾回收器都有哪些？如何选择？
    1. SerialGC：单线程GC、稳定高效，耗时长。
    2. ParNewGC：新生代并行、老年代串行，老年代标记压缩。
    3. ParalleGC：
    4. CMS：
    
12. GC roots：
    
    - 虚拟机栈中的变量、激活状态的线程、JNI栈中的对象、JVM自身持有的对象、常亮区中变量
    
13. [JVM内存泄漏的情况](https://www.jianshu.com/p/d5a4fdc6a848)
    
    1. 静态集合
    2. 各种连接
    3. 不合理的变量作用域
    4. 内部类持有外部类
    5. 改变哈希值
    
14. 堆如何保证线程安全：每个线程独享一小块堆内存TLAB，当TLAB用完之后会拓容。

15. Object对象结构：

    ![](img/ObjectStructure.png)

16. 堆和栈空间增长的区别？



### Spring

1. IOC(控制反转是一种软件设计的模式)，具体实现有：
   1. 依赖注入：把底层类作为参数传递给控制类，实现依赖注入，降低代码的耦合度
   2. 依赖查找
2. 三种常用的注入方式
   1. 构造方法注入
   2. setter注入
   3. 基于注解的注入
3. AOP
   1. 不同的问题交给不同的部分去解决
4. linux下如何部署Spring
   1. 打包-运行
5. 什么是带状态的bean：session bean
6. 内嵌Tomcat配置
   1. server.tomcat.accept-count: 等待队列长度，默认100
   2. server.tomcat.max-connections: 最大可被连接数，默认10000
   3. server.tomcat.max-threads: 最大工作线程数，默认200
   4. server.tomcat.min-spare-threads: 最小工作线程数，默认10
   5. keepAliveTimeOut: 多少毫秒后断开keepalive连接
   6. maxKeepAliveRequests：多少次请求后断开连接



### 机器学习

1. map-reduce实现k-means
2. bagging和boosting的区别
   1. 随机的选取特征和样本生成若干个不相关的模型
   2. 串行的生成若干个相互关联的模型，下一个模型依赖于上一个模型的训练结果
3. 偏差和方差的理解
4. 训练集、验证集、测试集
5. roc曲线和auc的理解
6. 极大似然的原理
7. SVM 原理
8. GBDT和随机森林有什么区别
9. GBDT的原理，如何做分类和回归
10. lightgbm的原理如何做分类和回归
11. GBDT+LR是怎么做的?
12. 数据之间如果不是独立同分布的会怎样
13. boosting和bagging的区别？
14. bagging为什么能减小方差？
15. xgboost可调整参数？
16. 最小二乘和极大似然的关系？
17. 什么是线性模型？为什么说LR是线性模型？
18. 普通决策树中用于选择特征和特征分裂方式的指标有：信息增益、信息增益比、基尼指数和均方误差。
19. RF的随机性体现在哪里？它的代码中输出的特征重要程度是怎么进行计算的？ 
20. ROC曲线如何绘制、ROC和PRC的异同点，适用场景；
    1. ROC对样本比例不敏感，PRC对样本比例敏感。当样本比例及不均衡时使用PRC更能反映模型的效果。
21. Bagging和Boosting的实现以及区别；
22. 随机森林和Adaboost的原理及其实现思路；
23. bagging回归和分类的区别
24. xgboost回归和分类的区别
25. EM算法



### 深度学习

1. dropout机制

2. L1和L2

   1. L1和L2的区别，以及各自的使用场景
   2. L1和L2有什么区别，从数学角度解释L2为什么能提升模型的泛化能力。
   3. 深度学习中，L2和dropout有哪些区别？
   4. L1正则化有哪些好处

3. 深度学习中，提升模型泛化能力的方法都有哪些？

4. 深度学习防止过拟合的方法：1.增加正则项  2.dropout  3.减小参数  4.提前终止  5.增大数据集 

5. Batch normalization的意义

6. 损失函数

   1. [神经网络里面的损失函数有哪些？各自的优缺点分别是什么？ MSE，MAE(平均绝对误差)，交叉熵，](https://zhuanlan.zhihu.com/p/77686118)
   2. 哪些场景下的分类问题不适用于交叉熵损失函数？
   3. 回归问题的损失函数都有哪些？从哪些角度设计一个合理的损失函数？
   4. 交叉熵损失函数，0-1分类的交叉熵损失函数的形式。什么是凸函数？0-1分类为什么用交叉熵而不是平方损失？
      1. 均方误差基于模型的输出误差满足高斯分布的前提。
   5. 常见损失函数汇总([知乎](https://zhuanlan.zhihu.com/p/58883095))：0-1损失，绝对值损失，对数损失，平方损失，指数损失，hinge损失max(0, 1-yf(x))，

7. 为什么梯度是函数变化最快的方向 https://zhuanlan.zhihu.com/p/24913912

8. LSTM的模型结构和公式

9. RNN为什么出现梯度消失及BPTT的推导

10. Wide &Deep的原理

11. 常见的激活函数有哪些？为什么通常需要零均值？

    1. 为什么要使用激活函数：激活函数为网络加入了非线性。
    2. 激活函数：sigmoid、relu、tanh
    3. 为什么要零均值：

12. early stop对参数有什么影响？

    

### Tensorflow

1. TensorFlow原理、流程图，session是啥？ 



### 常用的推荐算法

1. SVD及其变种
2. 基于物品的协同过滤、基于用户的协同过滤
3. 相似度算法优劣的比较



### 实践题

1. 如何验证新加入的特征是否有效
2. LR+GBDT和GBDT+FM如何结合
3. 如何对模型进行融合
4. KNN的使用场景 
5. 数据量很大，存储方案怎么设计 
6. 根据用户名快速检索出对应的手机号 
7. 如何保证用户名的唯一性 
8. 模拟高德地图或者百度地图中，求两地间的最短路径？
9. 怎么对模型效果进行评估的？发现模型性能下降有什么改进方法，从模型角度、数据角度分析。
10. 不均衡样本训练时有什么方法提高模型性能
    - 扩大数据集，修改损失函数(提高低比例样本在损失函数中的比重)，高比例样本欠采样，低比例样本重采样，修改评价指标多维度评价，尝试不同的算法



### ANN

1. minhash
   1. https://www.biaodianfu.com/minhash.html
2. simhash
3. lsh



### 大数据实践题

1. 给你一份乱序的100万个数字的文件，你如何来排序？



### Redis

1. 为什么redis快
   1. 基于内存
   2. 数据结构简单
   3. 单线程操作，避免并发环境下的线程切换
   4. 使用多路I/O复用模型，非阻塞IO
      1. 一般使用O(1)查询复杂度的方式。如：epoll，保定使用select
      2. 基于react设计模式监听I/O事件
   
2. 常用数据类型
   
   - String(二进制安全)、Hash、List、Set、Sorted Set、**HyperLogLog(计数)、Geo(地理位置)**
   
3. 读写分离的好处

4. 一致性hash算法

5. **如何查找指定前缀的所有key**
   
   1. 确定数据规模
   2. kyes会阻塞并返回所有匹配的key
   3. **scan不阻塞且只返回少量key(Scan执行的原理)**
   
6. **如何通过redis实现分布式锁**
   1. 什么是分布式锁？应该满足什么条件？
   2. SETNX key value (非原子性、不要使用)
      1. 如果不存在就创建value、时间复杂度为O(1)
      2. 设置过期时间来释放锁
   3. set [key] ex [time] nx 满足原子性，过期时间设置为随机，避免集中删除时造成reids卡顿
   
7. 如何通过redis实现异步队列
   1. 使用list作为队列，使用lpop消费，当队列为空时线程sleep
   2. 使用blpop消费，指定阻塞时间
   3. publish/subsribe：主题订阅者模式。消息是无状态的，无法保证消费者一定会受到
   
8. redis如何做持久化
   1. RDB（快照）持久化：保存某个时间点的全量数据快照
      1. save会阻塞
      2. bgsave会fork一个子进程来进行持久化
         1. 如果当前有save或bgsave执行，则提交的save和bgsave命令会被拒绝
         2. 系统调用fork实现了copy-on-write(copy-on-write原理)
      3. 数据量大是调使用RDB会有大量磁盘IO，降低系统性能，且最后一次持久化操作后的数据都会丢失
   2. AOF
      1. 保存数据库操作命令
      2. 日志重写解决AOF文件大小不断增加的问题
   3. RDB和AOF的优缺点
      1. RDB优点：全量数据快照，文件小，恢复快
      2. RDB缺点：无法保存最近一次快照后的数据
      3. AOF优点：可读性高，适合保存增量数据，数据不易丢失
      4. AOF缺点：文件体积大，恢复时间长
   4. RDB-AOF混合持久方式
   
9. redis中的pipeline
   
   1. 节省多次IO的往返时间
   
10. Redis的同步机制
    1. 全同步
       1. slave发送sync命令到master
       2. master启动后台进程，将redis中的数据写入到文件中
       3. master将保存数据快照期间接收到的写命令缓存起来
       4. master完成写文件操作后，将文件发送给slave
       5. slave使用新的RDB文件替换掉就得RDB文件，并更新自己
       6. Master将Salve更新期间的增量命令发送给slave
    2. 增量同步过程
       1. Master接收到用户命令，判断是否需要传播到Slave
       2. 将操作记录到AOF文件
       3. 将操作传播到Slave
    3. **redis sentinel(redis 哨兵机制)**
       1. 解决Master宕机后的主从切换问题
          - redis哨兵观测master节点、redis哨兵也会相互监听
          - 当哨兵发现某master节点超过一定的时间没有回复的时候就将其置为主管下线
          - 当一定数量的哨兵认为某一master节点主管下线时，就会发起一次投票确认
          - 需要一定的哨兵同一才会真的执行master下线
    4. 流言协议Gossip

11. Redis的集群原理
    1. 一致性hash算法：对2^32取模，将哈希值空间组成环
    2. 使用虚拟节点来避免数据倾斜
    
12. 数据类型的底层实现
    - String
      - C语言的简单动态字符串，len：字符串长度，free：未使用字节数，buf保存字符串。空间预分配、惰性释放。
    
13. Redis数据淘汰策略

    （1）volatile-lru：从已设置过期时间的数据集中挑选最近最少使用的数据淘汰。

    （2）volatile-ttl：从已设置过期时间的数据集中挑选将要过期的数据淘汰。

    （3）volatile-random：从已设置过期时间的数据集中任意选择数据淘汰。

    （4）volatile-lfu：从已设置过期时间的数据集挑选使用频率最低的数据淘汰。

    （5）allkeys-lru：从数据集中挑选最近最少使用的数据淘汰

    （6）allkeys-lfu：从数据集中挑选使用频率最低的数据淘汰。

    （7）allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰

    （8） no-enviction（驱逐）：禁止驱逐数据，这也是默认策略。意思是当内存不足以容纳新入数据时，新写入操作就会报错，请求可以继续进行，线上任务也不能持续进行，采用no-enviction策略可以保证数据不被丢失。

14. Redis如何对key加锁

15. 如何设置key永不过期 *persist* 命令

16. 缓存同步策略



### Linux

1. Linux体系结构
   1. 内核态和用户态
   2. 系统调用：内核的访问接口，是一种不可能再简化的操作
   3. 公用函数库：系统调用的组合拳
   4. Shell：命令解释器
2. find -iname不区分大小写
3. grep -e和-E的区别
4. sed -i "s/^/abc/efg/g" test.py，不加g只替换每行的第一个



### Hive

1. hive 中数据倾斜是否遇到过，如何解决

2. 什么是map join和reduce join

   

### Spark

1. spark角色架构图

   ![](img\SprkStructure.webp)

   Executor可以同时运行多个task，每个RDD的partition需要一个task执行。

   spark中的广播变量在每个executor上存一份。

2. spark中有哪些聚合算子：groupByKey，reduceByKey

3. spark中shuffle的实现

4. 如何处理spark中发生的数据倾斜：随机打散，加随机前缀

5. spark怎么划分stage，宽窄依赖，各包括哪些***作

6. 什么时候发生窄宽依赖：一个RDD被一个子RDD消费称为窄依赖，一个RDD被多个子RDD消费称为宽依赖。

   ![](img/NarrowDeps.webp)

7. 什么时候会划分stage：当出现宽依赖的时候会划分stage。

8. spark内存不够时会发生什么？

9. union是宽依赖还是窄依赖？

10. repartition过程

    - repartition随机分块（产生随机key用于shuffle、用于分区变多）、coalesce随机分块（用于分区变少）、partitionBy根据指定字段分区
    - **repartition和coalesce的区别？**
      - repartition会shuffle，coalesce不会shuffle

11. reduce和reduceByKey的区别

12. groupByKey和reduceByKey的区别

13. spark为什么比hadoop快？

14. spark有哪些角色？

15. zookeeper在spark中的作用？

16. spark如何对集群资源进行调度？



### Hadoop

1. NameNode，DataNode，Secondary NameNode
2. hdfs文件上传流程，hdfs的容错机制：客户端分块，向NameNode获取写入的DataNode，写入完成后NameNode自动复制，复制完成后更新元数据。
3. 适用场景，一次写入多次读取，适合存储大文件，不适合顺序写入。过多的小文件会对NameNode造成很大的压力。
4. NameNode挂了会怎么办？
   - 备用NameNode实现HA
5. NameNode的备份策略是怎样的？
6. Hadoop与yarn的关系
7. 通过校验和保证数据一致性



### Kafka

1. kafka和rabitMQ对比
   - rabbitMQ低吞吐，高可靠
   - kafka高吞吐
2. kafka如何保证高吞吐的
   - os cache +磁盘顺序写保证高效写入
   - zero copy避免数据在内存中的重复拷贝保证高效发送
3. 消息传递语义
   - 最多一次
   - 最少一次
   - 精确一次
4. kafka发送和消费消息时如何确定partition
   - 发送时默认轮训partition
   - 消费时独占一个或多个partition
5. 如何保证消息不丢失
   1. 注册发送回调函数，当发送失败时重试，设置较大的重试间隔
   2. 关闭消费端的自动commit，只有当消息完全被消费时才提交
   3. 设置acks=all，保证所有partition备份都能得到同步。设置备份分片>=3。min.insync.replicas > 1消息至少写入一个分片才算成功。并保证**replication.factor > min.insync.replicas**。
6. 消息分段
   - 指定时间
   - 指定大小
7. 过期数据清理：
   - 超过指定时间
   - 超过指定大小
8. zookeeper在kafka中的作用？
   - 消费者注册、消费offset记录、分区与消费者记录、消费者负载均衡、生产者负载均衡、topic和broker注册。
9. kafka中的leader选举
   1. leader的初始化选举
   2. leader挂掉之后的选举
      - kafka动态维护ISR列表，ISR保存了partition的分片
      - leader挂掉之后，ISR向zookeeper抢注leader
      - 如果isr全挂了，就进行脏选举
10. 发送消息的流程
    1. 序列化消息
    2. 用分区器对消息进行分区
    3. 指定分区的独立线程按批次发送消息
11. Controller
    1. controller的作用：
       - topic的创建和删除、broker的动态更新
    2. controller是一个特殊的broker，担任集群master的角色。采用zookeeper抢占式注册的方式选出。
12. 消息有序性
    1. 使用单个partition存在的问题：
       - 串行的读写，且只能由一个消费者消费，效率低下。
    2. 使用key + offset保证业务有序



### HBase

1. hbase中row key该怎么设计

   - 唯一性、有序性、散列原则
   - 如何分散rowkey
     - 反转key
     - 加随机前缀
     - 加哈希前缀
   - 长度10-100字节

2. 行存储与列存储
   - 行存储：
   - 列存储：

3. RegionServer的元数据存在哪

4. HBase如何实现底层的存储？

   - 一个region只能占有一个或多个列簇，当region大于一定值的时候会根据rowkey进行划分，rowkey是有序的。Store=列族
   - ![image-20200610123929144](img\RegionServerStructure.png)
   - HBase中的LSM存储思想

   - c0存在内存，c1存在磁盘。c0超过指定大小时会写入c1。c1是小文件所以也会定期对c1进行合并。
   - HLog + MemStore = Level0
   - StoreFile + HFile = Level1
   - region切分的过程是什么？
   - region会有自己的备份吗？没有。为什么？
   - HBaseWAL(预写日志) ---- 所有操作在写入之前都会记录WAL，如果写入日志失败，则认为操作失败。
     - ![image-20200610123929144](img\HBaseWAL.png)

5. HBase实战优化

   1. 数据表预分区
   2. 批量写入

6. region越多越好吗？

   1. 机器数量一致的情况下，region数量的增加会导致memstore的空间变小，从而会更平凡的吸入HFile和合并HFile，降低系统性能。
   2. region数量过小会导致region的频繁访问，降低系统效率。

7. 什么是OLAP



### ZooKeeper

1. 什么是zookeeper，好处是什么？

2. 如何实现分布式锁

3. zookeeper选举机制
   
   1. 选举时发送(myid，zxid)，初始情况下发送自己的相关id
   
4. zookeeper的watch机制
   
   1. 如何监控集群中主机的状态
   
5. zookeeper如何使用临时节点实现集群监控

   

### Yarn

1. 什么是yarn，解决了什么问题？
   1. yarn是一个分布式的资源协调框架，解决了分布式程序在集群上运行时资源分配的问题。
2. Yarn的调度算法？
   - FIFO：先进先出
   - Capacity Scheduler：
   - Fair Scheduler



### 分布式系统理论

1. CAP
   1. P的重要性：数据容灾
2. Paxos
3. ZAB
4. 什么是脑裂
   - 只有单一master节点的集群由于网络问题分裂为具有两个master节点的两个集群对外提供服务。
   - 如何解决脑裂？半数投票机制。
5. 如何实现分布式事务



### Thrift

1. 和其他rpc框架的区别。使用简单支持多语言，基于tcp。



### MyBatis

1. 一级缓存：处于Session级别，不受配置影响。
2. 二级缓存：。
3. 延迟加载
4. 一对多、多对多关联查询



### Tomcat

1. Servlect：java class

2. tomcat结构

   ![](img\TomcatStructure.png) 

3. Servlet的生命周期：

### 函数计算

1. 什么是函数计算，优缺点是什么？



### 设计模式

1. 工厂模式
   1. 简单工厂
   2. 抽象工程



### 操作系统

1. 僵尸进程：进程执行完成之后没有被及时回收，不占用内存但是会占用进程号，大量僵尸进程对进程号的占用将导致无法创建新的进程。
2. top中cache和buffer的区别
3. 操作系统swap
4. 操作系统的抢占式和非抢占式



### 数仓

1. 雪花模型
2. 星型模型



### 操作系统

1. 软中断和硬中断的区别
   - 硬中断是由外部事件触发的中断，处理事件长
   - 软中断是由程序指令触发，处理时间相对较短
2. 操作系统启动时发生了什么
3. 不同的进程怎么共享相同的一块内存？



### 计算机基础理论

1. utf-8和utf-16的区别？utf-8使用单个字节作为存储单位，utf-16使用两个字节作为存储单位。
2. 乐观锁和悲观锁的区别：悲观锁是线程独占，乐观锁不是线程独占但是同时更新操作的线程只有一个能成功
3. 布隆筛
   - 