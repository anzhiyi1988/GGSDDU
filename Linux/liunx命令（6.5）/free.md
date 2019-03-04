|      |                      | 1        | 2        | 3        | 4      | 5       | 6        |
| ---- | -------------------- | -------- | -------- | -------- | ------ | ------- | -------- |
| 1    |                      | Total    | Used     | Free     | Shared | Buffers | cached   |
| 2    | Mem:                 | 24677460 | 23276064 | 1401396  | 0      | 870540  | 12084008 |
| 3    | -/+   buffers/cache: |          | 10321516 | 14355944 |        |         |          |
| 4    | Swap:                | 25151484 | 224188   | 24927296 |        |         |          |

第二行解析

从系统的角度看内存

共有FO[2][1]KB的内存，已经使用了FO[2][2]KB，还有FO[2][3]可以使用。

公式FO[2][1] = FO[2][2] + FO[2][3] 。

FO[2][4]表示被几个进程共享的内存的，现在已经deprecated，其值总是0（当然在一些系统上也可能不是0，主要取决于free命令是怎么实现的）。

FO[2][5]被系统buffer住的内存 。

FO[2][6]被系统cache的内存。

A buffer is something that has yet to be "written" to disk. 

A cache is something that has been "read" from the disk and stored for later use.

也就是说buffer是用于存放要输出到disk（块设备）的数据的，而cache是存放从disk上读出的数据。这二者是为了提高IO性能的，并由OS管理。

第三行解析

应用程序的角度看系统内存的使用情况

对于FO[3][2]，即-buffers/cache，表示一个应用程序认为系统被用掉多少内存

对于FO[3][3]，即+buffers/cache，表示一个应用程序认为系统还有多少内存

FO[3][2] = FO[2][2] - FO[2][5] - FO[2][6]

FO[3][3] = FO[2][3] + FO[2][5] + FO[2][6]

第四行解析

交换区的信息，分别是交换的总量（total），使用量（used）和有多少空闲的交换区（free），这个比较清楚，不说太多。

 

**手动释放方法** 

**2.1 使用free查看一下当前内存使用情况（可略过）：** 

[root@*** ~]# free -m
 total       used       free     shared    buffers     cached
 Mem:           512        488         23          0         57        157
 -/+ buffers/cache:        273        238 Swap:         1055          0       1055

**2.2** **执行****sync****同步数据**

[root@*** ~]# sync

**2.3 清理cache**

[root@*** ~]#echo 3 > /proc/sys/vm/drop_caches

**2.4 drop_cache的详细文档如下，以便查阅**

Writing to this will cause the kernel to drop clean caches, dentries and inodes from memory, causing that memory to become free.
 To free pagecache:
 \* echo 1 > /proc/sys/vm/drop_caches
 To free dentries and inodes:
 \* echo 2 > /proc/sys/vm/drop_caches
 To free pagecache, dentries and inodes:
 \* echo 3 > /proc/sys/vm/drop_caches
 As this is a non-destructive operation, and dirty objects are notfreeable, the user should run "sync" first in order to make sure allcached objects are freed.
 This tunable was added in 2.6.16.

 

源文档 <<http://m.blog.csdn.net/blog/miaomiaodmiaomiao/24733799>> 

 

 