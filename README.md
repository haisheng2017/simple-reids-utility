# simple-redis-utility
Simple utility based on redis

# distributed-id 发号器
很快，线程安全，多进程下安全（但不保证全局一定连续，仅能保证ID呈现单调增趋势）

# message-queue
适用场景：
- 小流量，并发可能性适中
- 写少读多
- 对数据可靠性没有那么敏感

以List数据结构实现
- 只要有消费，不会引起堆积内存
- 能够做到 QoS=0
- 能够做到消费者负载均衡
- 只能连接到主节点
- 无法保证消息的完全有序（例如未Ack的消息无法按照顺序恢复到在队列时的原始顺序
- 无法多播
- 有可能丢消息

以Stream数据结构实现
- 可能引起内存堆积
  - 不会主动删除消息，除非调用XDEL（但这也是逻辑上删除，只有删除最后一条数据时，才会物理删除所有数据
  - 需要配合 MAXLEN 参数控制Stream的容量
- 能够做到 QoS=0
- 能够做到消费者负载均衡
- 能够支持多播
- 只能连接到主节点
- 能够保证消息有序
- 有可能丢消息