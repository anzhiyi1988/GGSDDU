# NAME

vmstat - Report virtual memory statistics

# SYNOPSIS

    vmstat [options] [delay [count]]

DESCRIPTION

vmstat reports information about processes, memory, paging, block IO, traps, disks and CPU activity.

The first report produced gives averages since the last reboot.  Additional reports give information on a sampling period of length delay.  The process and memory reports are instantaneous in either case.

第一份报告是从重启到现在的平均值，附加reposts是抽样的信息，在这两个种情况下，进程和内存的report都是即时的。

# OPTIONS

##### delay

The delay between updates in seconds.  If no delay is specified, only one report is printed with the average values since boot.

##### count

Number of updates.  In absence of count, when delay is defined, default is infinite(无限的).

##### -a, --active

Display active and  inactive memory, given a 2.5.41 kernel or better.

##### -f, --forks

The -f switch displays the number of forks since boot.  This includes the fork, vfork, and clone system calls, and is equivalent to the total number of tasks created.  Each process  is represented by one or more tasks, depending on thread usage.  This display does not repeat.

f开关显示自引导以来的叉数。这包括fork、vfork和clone系统调用，相当于创建的任务总数。根据线程使用情况，每个进程由一个或多个任务表示。此显示不重复显示，就一条数据。

##### -m, --slabs

Displays slabinfo.

##### -n, --one-header

Display the header only once rather than periodically.

只显示一次标题，而不是定期显示，说的是换页的时候，应该有标题。

##### -s, --stats

Displays a table of various event counters and memory statistics.  This display does not repeat.

显示各种事件计数器和内存统计信息的表。此显示不重复，就一条数据。

##### -d, --disk

Report disk statistics (2.5.70 or above required).

不停的汇报磁盘的IO

##### -D, --disk-sum

Report some summary statistics about disk activity.此显示不重复，就一条数据。

##### -p, --partition device

Detailed statistics about partition (2.5.70 or above required). 显示磁盘分区。

##### -S, --unit character

Switches outputs between 1000 (k), 1024 (K), 1000000 (m), or 1048576 (M) bytes.  Note this does not change the swap (si/so) or block (bi/bo) fields.

显示的时候采用的是精确的进制还是模糊的。

##### -t, --timestamp

Append timestamp to each line，每行增加一个时间戳。

##### -w, --wide

Wide  output  mode  (useful  for systems with higher amount of memory, where the default output mode suffers from unwanted column breakage).  The output is wider than 80 characters per line.



# VM MODE

#####    Procs

       r: The number of runnable processes (running or waiting for run time).
       b: The number of processes in uninterruptible sleep.

#####    Memory

       swpd: the amount of virtual memory used.
       free: the amount of idle memory.
       buff: the amount of memory used as buffers.
       cache: the amount of memory used as cache.
       inact: the amount of inactive memory.  (-a option)
       active: the amount of active memory.  (-a option)

#####    Swap

       si: Amount of memory swapped in from disk (/s).
       so: Amount of memory swapped to disk (/s).

#####    IO

       bi: Blocks received from a block device (blocks/s).
       bo: Blocks sent to a block device (blocks/s).

#####    System

       in: The number of interrupts per second, including the clock.
       cs: The number of context switches per second.

#####    CPU

       These are percentages of total CPU time.
       us: Time spent running non-kernel code.  (user time, including nice time)
       sy: Time spent running kernel code.  (system time)
       id: Time spent idle.  Prior to Linux 2.5.41, this includes IO-wait time.
       wa: Time spent waiting for IO.  Prior to Linux 2.5.41, included in idle.
       st: Time stolen from a virtual machine.  Prior to Linux 2.6.11, unknown.

# DISK MODE

#####    Reads

       total: Total reads completed successfully
       merged: grouped reads (resulting in one I/O)
       sectors: Sectors read successfully
       ms: milliseconds spent reading

#####    Writes

       total: Total writes completed successfully
       merged: grouped writes (resulting in one I/O)
       sectors: Sectors written successfully
       ms: milliseconds spent writing

#####    IO

       cur: I/O in progress
       s: seconds spent for I/O

# DISK PARTITION MODE

       reads: Total number of reads issued to this partition
       read sectors: Total read sectors for partition
       writes : Total number of writes issued to this partition
       requested writes: Total number of write requests made for partition

# SLAB MODE

       cache: Cache name
       num: Number of currently active objects
       total: Total number of available objects
       size: Size of each object
       pages: Number of pages with at least one active object

# NOTES

   vmstat does not require special permissions.

   These reports are intended to help identify system bottlenecks.  Linux vmstat does not count itself as a running process.

   All linux blocks are currently 1024 bytes.  Old kernels may report blocks as 512 bytes, 2048 bytes, or 4096 bytes.

   Since procps 3.1.9, vmstat lets you choose units (k, K, m, M).  Default is K (1024 bytes) in the default mode.

   vmstat uses slabinfo 1.1

# FILES

   /proc/meminfo

   /proc/stat

   /proc/*/stat



SEE ALSO

   free(1), iostat(1), mpstat(1), ps(1), sar(1), top(1)

BUGS

   Does not tabulate the block io per device or count the number of system calls.

AUTHORS

   Written by Henry Ware ⟨al172@yfn.ysu.edu⟩.

   Fabian Frédérick ⟨ffrederick@users.sourceforge.net⟩ (diskstat, slab, partitions...)

REPORTING BUGS

   Please send bug reports to ⟨procps@freelists.org⟩