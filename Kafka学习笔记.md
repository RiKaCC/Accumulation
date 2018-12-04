## Topic
topic就是kafka消息的类别。

发送到kafka的消息都需要制定topic。

### partition
一个topic可以分为多个partition。topic是一个逻辑上概念，而partition是物理概念。

partition可以分布在多台broker上。如果只有一台broker，那么会有多个partition目录。（partition实现了水平扩展）。

每个partition内消息的顺序是有序的，新来的消息都顺序的写到partition的队尾（kafka在写消息的时候，是顺序写入磁盘，这样大幅度提高了效率）。

在partition中，都有一个offset来记录该partition消费的位置。

### 使用partition的好处：

  - 能处理更多的消息，不受单台机器的限制。
  - partition能作为并行处理的单元，并发。
  - 由消费者控制offset，可以做到单partition灵活消费。broker是无状态的。
  - 同一个partition内有序消费。
