kafka是一款消息队列处理框架，一般用于生产者和消费者之间的解耦，流量削峰，消息传递，日志收集等场景。<br>

kafka结构一般有三部分，分别是producer，broker，consumer，producer用于消息的生产，broker一般用于消息的分发，consumer用于消息的消费。<br>

broker也就是kafka服务器。对于一个消息来说，这个消息有自己专属的topic，这个topic只是一个概念，也就是消息要发送的地方，每个topic是有多个
partition组成的，对于每个partition来说，消息只能append到每个partition的尾部。<br>

为了防止消息丢失，所以kafka有备份的机制，这些备份的日志被称为replica（副本），这些副本是保存在broker上面的。同时对于同一个partition的多个
replica是不会保存在同一台broker上面的，因为如果一台broker挂了，那所有replica都不存在了。副本分为leader replica
以及follower replica，follower replica是不能提供服务给客户端的，如果leader挂了的话，会从剩下的follower当中选举出一个leader。注意并不是所有的
follower replica都有机会被选举为leader，只有那些在ISR（in-sync replica)当中的副本才会，这些集合当中的replica与leader保持着同步。

producer发送消息的流程是，首先构造消息对象，然后对其进行压缩（可选）以及序列化，然后选择发送到哪个分区，有专门的partitioner（分区器）进行设置。
发送。发送有两种方式，分别是同步以及异步发送，这两种方式是通过future实现的，前者
是通过future.get无限等待，而后者有future对象可以稍后获取结果，这就是回调机制。在发送完之后，broker可以返回结果给producer，producer可以设置参数为
不等待broker确认结果（acks=0），也可以是broker等到ISR当中所有副本写入之后再发（acks=all或者-1），也可以是只等到leader replica写进去就发送
（acks=1），因此不同的方式会影响吞吐率。






