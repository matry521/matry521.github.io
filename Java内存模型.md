## Java内存模型

### 什么是Memory Barrier(内存屏障)

>内存屏障，又称内存栅栏，一个CPU指令：
>1.保证特定操作的执行顺序
>2.影响某些数据（或则是某条指令的执行结果）的内存可见性。
>>例如：当插入一条新的Memory Barrier时，这个时候不管什么指令都不能和这条Memory Barrier指令重排序。
>
>3.Memory Barrier会强制刷出各种CPU cache，volatile是基于Memory Barrier实现的。
>>存在private volatile int a; a写入之后会被JMM插入一个Write-Barrier指令，在a被读之前插入一个Read-Barrier指令。可以保证做到如下两点：
>>1.一个线程写入变量a，任何线程访问该变量拿到都是最新值
>>2.对a写入操作，更新数据对其他线程也是可见的。


>数据结构
>>树
>>>二叉树
>>>>平衡二叉树
>>>>>满二叉树