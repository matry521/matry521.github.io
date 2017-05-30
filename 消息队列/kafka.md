## kafka初识

### 什么是kafka
一个分布式、可分区、可复制的发布/订阅消息系统。主要设计目标如下:<br/>
+ 以时间复杂度为O(1)的方式提供消息持久化能力，即使对TB级以上数据也能保证常数时间复杂度的访问性能。<br/>
+ 高吞吐率。即使在非常廉价的商用机器上也能做到单机美妙100K以上消息的传输。<br/>
+ 支持Kafka Server间的消息分区，及分布式消费，同时保证每个Partition内的消息顺序传输。<br/>
+ 同时支持离线数据处理和实时数据处理。<br/>
+ Scala out: 支持在线水平扩展。<br/>

### kafka名词解释
+ **broker**: Kafka集群包含一个或多个服务器，这种服务器被称为**broker**。server.properties(broker.id=0)配置指定id，正整数唯一递增。<br/>
+ **topic** : 每条发布到Kafka集群的消息都有一个类别，这个类别被称为**topic**。(物理上不同Topic的消息分开存储，逻辑上一个Topic的消息虽然
保存于一个或多个broker上，但用户只需指定消息的Topic即可生产或消费数据，而不必关心数据存于何处)<br/>
+ **productor**: 消息生产者，负责发布消息到Kafka broker。<br/>
+ **consumer** : 消息消费者，从Kafka broker读取消息的客户端。<br/>
+ **offset**: 偏移量，唯一用来标记一条消息在分区中的位置。<br/>
+ **partition**: 是物理上的概念，每个Topic包含一个或多个Partition。(一般为Kafka节点数cpu的总核数)<br/>
+ **consumer group**: 每个consumer属于一个特定的consumer group(可为每个consumer指定group name，若不指定默认为group)<br/>
+ **replication-factor**: 创建topic时的副本数，提供冗余保证可用性。<br/>
+ **isr**: Kafka在zookeeper中动态维护了一个ISR(in-sync replicas) set，这个set里所有replica都跟上了leader，只要ISR里的成员才有被
选为leader的可能。<br/>
+ **zookeeper**: 用来保障分布式应用一致性的软件。<br/>

### 基本特性
#### **1.可扩展性**
+ 在不需要下线的情况下进行扩容<br/>
+ 数据流区(partition)存储在多个机器上<br/>
#### **2.高性能** 
+ 单个broker就能服务上千客户端<br/>
+ 单个broker每秒读/写可达几百兆字节<br/>
+ 多个brokers组成的集群将达到非常强的吞吐能力<br/>
+ 性能稳定，无论数据多大<br/>
+ Kafka在底层摒弃了Java堆缓存机制，采用了**操作系统级别的页缓存**，同时将随机写操作改为顺序写，再结合Zero-Copy的特性极大改善了IO性能。<br/>
#### **3.持久存储**
+ 存储在磁盘上<br/>
+ 冗余备份到其他服务器上以防止丢失<br/>

### Kafka拓扑结构
![Image](../images/kafka/kafka拓扑结构.png)
如上图，一个典型的Kafka集群中包含若干Producer，若干broker，若干consumer group以及一个zookeeper集群。通过zk管理集群配置，选举leader，
以及在consumer group发生变化时进行Rebalance。producer使用push模式将消息发布到broker，consumer使用pull模式从boker订阅并消费消息。<br/>

### 为何要使用消息系统

<!-- 1.kafka节点之间如何复制备份的？
kafka消息是否会丢失？为什么？
kafka最合理的配置是什么？
kafka的leader选举机制是什么？
kafka对硬件的配置有什么要求？
kafka的消息保证有几种方式？-->