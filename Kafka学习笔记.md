## Topic
topic就是kafka消息的类别。

发送到kafka的消息都需要制定topic。

### partition
一个topic可以分为多个partition。topic是一个逻辑上概念，而partition是物理概念。

partition可以分布在多台broker上。如果只有一台broker，那么会有多个partition目录。（partition实现了水平扩展）
