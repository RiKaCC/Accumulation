## Topic
topic就是kafka消息的类别。

发送到kafka的消息都需要制定topic。

### partition
一个topic可以分为多个partition。topic是一个逻辑上概念，而partition是物理概念。

partition可以分布在多台broker上。如果只有一台broker，那么会有多个partition目录。（partition实现了水平扩展）。

每个partition内消息的顺序是有序的，新来的消息都顺序的写到partition的队尾（kafka在写消息的时候，是顺序写入磁盘，这样大幅度提高了效率）。

在partition中，都有一个offset来记录该partition消费的位置。

### 使用partition的好处：

  - 能处理更多的消息，不受单台机器的限制。（分布式）
  - partition能作为并行处理的单元。（并发）
  - 由消费者控制offset，可以做到单partition灵活消费。broker是无状态的。（灵活消费，互不影响）
  - 同一个partition内有序消费。（有序消费）
  - 消息一般来说可以均匀的写入partition中，可以实现负载均衡。（负载均衡）

### partition消息存储
存储在partition中的消息，是通过offset来控制消费的。

可以去配置消息的保存策略，比如保存两天，那么这个消息在发布的两天之内都是可以被消费的。

offset是consumer来维护的，一般来说随着consumer不断的读取消息，offset是不断增长的，但其实consumer可以通过修改offset来任意读取消息。

#### segment
在一个partition中，消息是顺序写到partition队尾的，我们可以通过设置segment大小，来将文件切割成小段（segment）。

partition可以理解为一个文件夹，而segment就是这个文件夹下的各个文件。

Segment file组成：由2大部分组成，分别为index file和data file，此2个文件一一对应，成对出现，后缀”.index”和“.log”分别表示为segment索引文件、数据文件。

每个index文件名字就是offset，表示他对应的log文件从这个offset开始。如下例子，00000000000000000000.log这个文件就记录着offset0到137199的消息。

通过这个index文件，我们就能很快索引到需要查询的消息（二分查找）。
```
00000000000000000000.index
00000000000000000000.log

00000000000000137200.index
00000000000000137200.log

00000000000000271600.index
00000000000000271600.log

00000000000000406000.index
00000000000000406000.log
```

### partition复制
