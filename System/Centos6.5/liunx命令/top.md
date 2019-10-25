



#### 简介

------

```
top -hv|-bcHiOSs -d secs -n max -u|U user -p pid -o fld -w [cols]
```



当使用top时，有两个重要的键：帮助（h）和退出（q），首次使用top，主要 分三个区域：1.摘要区；2.字段/列标题；3.任务区。列的宽度是可变的，也受命令行输入时-w选项的影响。显示的内容可以通过上下左右键移动。如果没有任何配置和自定义修改，top启动时的默认和显示有关的参数值如下，*的值可以在命令行指定：

```
Global-defaults
   A - Alt display      Off (full-screen)            -- 全屏显示默认关闭
 * d - Delay time       1.5 seconds                  -- 刷新频率1.5s
 * H - Threads mode     Off (summarize as tasks)     -- 线程模式默认关闭
   I - Irix mode        On  (no, `solaris' smp)      -- ？？？
 * p - PID monitoring   Off (show all processes)     -- 在不指定PID的情况下，默认显示所有
 * s - Secure mode      Off (unsecured)              -- 安全模式默认关闭
   B - Bold enable      On  (yes, bold globally)     -- 默认全局粗体
Summary-Area-defaults
   l - Load Avg/Uptime  On  (thus program name)      -- 显示load状态
   t - Task/Cpu states  On  (1+1 lines)              -- 显示任务和cpu状态，两行
   m - Mem/Swap usage   On  (2 lines worth)          -- 显示内存状态，两行
   1 - Single Cpu       Off (thus multiple cpus)     --？？？
Task-Area-defaults
   b - Bold hilite      Off (use `reverse')
 * c - Command line     Off (name, not cmdline)      -- 详细命令关闭，默认显示命令名字
 * i - Idle tasks       On  (show all tasks)         -- 空闲开启，所以显示所有任务
   J - Num align right  On  (not left justify)       -- 数字默认右对齐
   j - Str align right  Off (not right justify)      -- 字符默认左对齐
   R - Reverse sort     On  (pids high-to-low)       -- 默认按照pid高到底排序
 * S - Cumulative time  Off (no, dead children)      -- 累计时间关闭？？？？
 * u - User filter      Off (show euid only)         -- 用户过滤关？？？
 * U - User filter      Off (show any uid)           -- ？？？
   V - Forest view      On  (show as branches)       -- ？？？
   x - Column hilite    Off (no, sort field)         -- 高亮列关闭
   y - Row hilite       On  (yes, running tasks)     -- 高亮行开启，正在运行的任务高亮
   z - color/mono       On  (show colors)            -- 颜色开？？？
```



#### 选项说明

------

```
-hv|-bcHiOSs -d secs -n max -u|U user -p pid -o fld -w [cols]
```

##### -h | -v  :Help/Version

Show library version and the usage prompt, then quit.

##### -b  :Batch-mode operation

采用批处理模式打开top命令，这对向其他程序和文件发送输出结果很有用，此模式下top不接收任何输入（交互模式不能用了），直到达到通过`-n`指定的运行次数或被kill为止。

##### -c  :Command-line/Program-name toggle

控制command列是显示命令还是命令名，在输入top时指定，也开在启动top后（不是`-b`模式）直接键入‘c’。

##### -d  :Delay-time interval as:  -d ss.t (secs.tenths)

控制显示的输出频率，在输入top时指定，也可在启动top后（不是`-b`模式）直接键入‘d’或者‘s’。

##### -H  :Threads-mode operation

控制以线程模式显示，如果不加此参数，则显示的是进程中所有线程的总和，在输入top时指定，也可在启动top后（不是`-b`模式）直接键入‘H’。

##### -i  :Idle-process toggle

控制显示和上次更新比是否空闲任务，在输入top时指定，也可在启动top后（不是`-b`模式）直接键入‘i’。

##### -n  :Number-of-iterations limit as:  -n number

指定刷新多少次结束，在输入top时指定（-n 10），**不支持启动后键入**。

##### -o  :Override-sort-field as:  -o fieldname

指定按照某列排序，在列名前加‘+’和‘-’标识升序和降序，本参数主要支持批处理和自动化模式。

##### -O  :Output-field-names

在批处理/自动化模式是输出的内容是否包含字段列头。

#####  -p  :Monitor-PIDs mode as:  -pN1 -pN2 ...  or  -pN1,N2,N3 ...

监视指定的进程，最多支持指定20个线程，**不支持启动后键入**。启动后，如果要恢复所有进程，可以键入‘=’、‘u’或者‘U’。

> ***注意：p u U三个参数互斥的。***

##### -s  :Secure-mode operation

控制安全模式，不懂？？？

##### -S  :Cumulative-time toggle

开启累计时间后，每个进程和其子进程使用的cpu时间将列出，在输入top时指定，也可在启动top后（不是`-b`模式）直接键入‘S’。

##### -u | -U  :User-filter-mode as:  -u | -U number or name

显示指定用户的进程，‘u’选项是有效用户，‘U’是所有用户不管是否有效。 在用户前加‘！’会显示除了此用户外的其他线程。。

> ***注意：p u U三个参数互斥的。***

##### -w  :Output-width-override as:  -w [ number ]

批处理模式下，如果启动top时不指定参数，则结果会采用‘COLUMNS’和‘LINES’环境变量中的值作为输出格式。

使用参数后，宽度可以自定义，行数默认无限。，**不支持启动后键入**



#### 显示区说明

------

##### 摘要区

----

###### UPTIME and LOAD Averages

用户数、1/5/15分钟的平均负载



###### TASK and CPU States

线程概要

```
running
sleeping
stopped
zombie
```

cpu概要，

```
us, user    : time running un-niced user processes   -- 运行非独立用户进程的时间
sy, system  : time running kernel processes          -- 运行内核进程的时间
ni, nice    : time running niced user processes      -- 运行良好的用户进程的时间
id, idle    : time spent in the kernel idle handler  -- 在内核空闲处理程序中花费的时间
wa, IO-wait : time waiting for I/O completion        -- 等待I/O完成的时间
hi : time spent servicing hardware interrupts        -- 服务硬件中断所花费的时间
si : time spent servicing software interrupts        -- 服务于软件中断的时间
st : time stolen from this vm by the hypervisor      -- 虚拟机管理程序从此虚拟机窃取的时间
```

可以通过加入‘t’看到其他信息

![image-20191025190028494](../../../image/image-20191025190028494.png)

```
a) is the combined us and ni percentage; 
b) is the sy percentage; 
c) is the total; and 
d) is one of two visual graphs of those representations
```



###### MEMORY Usage









##### 字段/列标题区

----







##### 任务区

----





```
top -bcn 1
```

-b ：批处理，一屏一屏显示。
-c：是否显示完整的命令行信息
-n ：指定显示多少批。





