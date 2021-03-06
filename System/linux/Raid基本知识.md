## 一、Raid基本原理

RAID(Redundant Array of Independent Disks)独立的磁盘冗余阵列，简称磁盘阵列。

## 二、常见的RAID类型

### 1. RAID 0

把连续的数据分散到多个磁盘上存取，系统有数据请求就可以被多个磁盘并行执行，每个磁盘读取自己那部分数据。做raid 0，服务器至少需要两块硬盘，读写是一块的两倍。如果有n快硬盘，读写就是一块的n倍。读写提高，但是没有备份，安全性低。

![img](..\..\image\raid0.jpg)

### 2. RAID 1

通过磁盘数据镜像实现数据冗余，在成对的独立磁盘上产生互为备份的数据。当原始数据繁忙时，可以直接读取镜像中数据。做raid 1，至少两块盘，一块读写，一块备份。安全高，磁盘使用率低。

![img](..\..\image\raid1.jpg)

### 3. RAID 2

纠错海明码磁盘阵列，海明码是一种在原始数据中加入若干校验码来进行错误检测和纠正的编码技术，其中第2^n位(1,2,4,8,...)是校验码，其他位置是数据码。

### 4. RAID 3

复杂

### 5. RAID 4

复杂

### 6. RAID 5

RAID 5把数据和与其相对应的奇偶校验信息存储到组成RAID5的各个磁盘上，并且奇偶校验信息和相对应的数据分别存储于不同的磁盘上。当其中一个磁盘（最多一个）数据损坏后，利用剩下的数据和相应的奇偶校验信息去恢复被损坏的数据。RAID 5具有和RAID 0相近似的数据读取速度，且磁盘空间利用率要比RAID 1高，存储成本相对较低，是目前运用较多的一种解决方案。用户选择做RAID 5的话，至少需要三块硬盘。

![img](..\..\image\raid5.jpg)



### 7. RAID 01 和 RAID 10

RAID01 是先做条带化再作镜像，本质是对物理磁盘实现镜像；

RAID10 是先做镜像再作条带化，是对虚拟磁盘实现镜像。

相同的配置下，通常 RAID 1+0 比 RAID 0+1 具有更好的容错能力，原理如图 



![img](..\..\image\raid01.png)

![img](..\..\image\raid10.png)

1+0 ,先两两做raid1（镜像），然后在做raid0（并行读写）。一般都推荐 1+0 ，盘数必须是偶数。





## 参考

http://www.hack520.com/169.html

http://baijiahao.baidu.com/s?id=1599790534545648124&wfr=spider&for=pc