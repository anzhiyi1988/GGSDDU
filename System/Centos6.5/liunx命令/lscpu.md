\#lscpu

Architecture:          i686                            #架构686

CPU(s):                2                                   #逻辑cpu颗数是2

Thread(s) per core:    1                           #每个核心线程数是1                 

Core(s) per socket:    2                           #每个cpu插槽核数/每颗物理cpu核数是2

CPU socket(s):         1                            #cpu插槽数是1

Vendor ID:             GenuineIntel           #cpu厂商ID是GenuineIntel

CPU family:            6                              #cpu系列是6

Model:                 23                                #型号23

Stepping:              10                              #步进是10

CPU MHz:               800.000                 #cpu主频是800MHz

Virtualization:        VT-x                         #cpu支持的虚拟化技术VT-x

L1d cache:             32K                         #一级缓存32K（google了下，这具体表示表示cpu的L1数据缓存为32k）

L1i cache:             32K                          #一级缓存32K（具体为L1指令缓存为32K）

L2 cache:              3072K                      #二级缓存3072K



On-line CPU(s) list:   0-79           # 在线的cpu数量 有些时候为了省电或者过热的时候，某些CPU会停止运行

Off-line CPU(s) list:                     # 并不是每台服务器都有，这是不在线cpu，也就是未使用的cpu，如果有参数 便是不在线的 cpu号 （这部分是vios中经过多线程技术，未分配给本分区的cup数量。）



 

Cat  /proc/cpuinfo