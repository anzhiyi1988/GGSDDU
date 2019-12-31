### JVM内存结构简介

----



![img](../image/jvmstructure.png) 



### JVM参数配置

#### 内存参数

##### -Xms

设置 **`堆`** 容量的最小值，单位M。

##### -Xmx
设置**`堆`**容量的最大值，单位M，xms和xmx可以设置为一样，这样可以避免自动扩展。

##### -Xss128k 
设计**`虚拟机栈`**的大小 128k。

##### -Xoss128k
设置**`本地方法栈`**的大小，但是Hotspot并不区分虚拟机栈和本地方法栈，对Hotspot来说这个参数无效。

##### -XX:PermSize=10M
设置**`永久代`**的容量 ，单位M。

##### -XX:MaxPermSize=10M
设置**`永久代`**最大容量，单位M，默认64M。

##### -Xnoclassgc 
关闭JVM对类的回收。

##### -XX:+TraceClassLoading
查看类的加载信息。

##### -XX:+TraceClassUnLoading 
查看类的卸载信息。

##### -XX:NewRatio=4
设置**`年轻代`**的占比，目前年轻代占总堆的1/5。

##### -XX:SurvivorRatio=8
设置**`sruvivor区`**占比，这个参数没看懂。

##### -Xmn20M
设置**`年轻代`**大小。 

##### -XX:+HeapDumpOnOutOfMemoryError
JVM出现内存溢出异常时，dump出当前的堆内存转储快照，快照命名规则java_pidxxxx.hprof。

查看工具

eclipsemat

IBM heap ana:java -Xmx1600 -jar ha396.jar

##### -XX:+PrintGCDetails

控制台打印GC具体细节。

##### -XX:+PrintGC

控制台打印GC信息。

##### -XX:PretenureSizeThreshold=3145728

对象大于配置的字节时，直接进入**`老年代`**，单位字节。

##### -XX:MaxTenuringThreshold=1

对象年龄大于1，直接进入**`老年代`**。

##### -XX:ComplieThreshold=1000

一个方法被调用1000次后，会被认为是热代码，并触发及时编译。

##### -XX:PrintHeapAtGC

打印每次GC前后的**`堆`**内存分布。

##### -XX:+PrintTLAB

打印TLAB的使用情况。

##### -XX:+UseSpining

开启自旋锁。

##### -XX:PreBlockSpin

设置自旋锁自旋次数，使用这个参数必须开启自旋锁。



#### -XX:-UseGCOverheadLimit

默认启用(+)，限制GC的运行时间，如果GC耗时过长，就抛OOM。

关闭限制(-)

过长的定义是，超过98%的时间用来做GC并且回收了不到2%的堆内存。实践中发现这个策略不能拯救你的应用，但是可以在应用挂掉之前做最后的挣扎，比如数据保存或者保存现场（Heap Dump）。一般都停用(-)。

#####  



#### 垃圾收集器的设置

----

##### -XX:+UseG1GC

使用G1垃圾收集器。

##### -XX:+UseParallelGC

选择垃圾收集器为并行收集器。此配置仅对年轻代有效。可以同时并行多个垃圾收集线程，但此时用户线程必须停止。

指 定在 New Generation 使用 parallel collector, 并行收集 , 暂停 app threads, 同时启动多个垃圾回收 thread, 不能和 CMS gc 一起使用 . 系统吨吐量优先 , 但是会有较长长时间的 app pause, 后台系统任务可以使用此 gc 

##### -XX:+UseParNewGC

设置年轻代为多线程收集。可与CMS收集同时使用。在serial基础上实现的多线程收集器。

指定在 New Generation 使用 parallel collector, 是 UseParallelGC 的 gc 的升级版本 , 有更好的性能或者优点 , 可以和 CMS gc 一起使用 

##### -XX:+UseConcMarkSweepGC

设置年老代为并发收集