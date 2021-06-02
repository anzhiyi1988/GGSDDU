# StringBuffer VS StringBuilder



String 不可变，线程安全

buffer 有同步锁 线程安全

builder 非线程安全



如果不是多线程，builder高于buffer



# ArrayList VS Linkedlist

linkedlist占用空间更大，因为每个节点要维护前后两个节点的地址。

但是如果arraylist存储的数量超过其默认的临时值，则空间就扩大了，这时有空间浪费。但是序列化时多余的空间不会被序列化，因为有transient关键字修饰。