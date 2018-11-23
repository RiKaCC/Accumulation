原文[https://redis.io/topics/replication]

翻译：Rika

# 复制
Redis通过使用配置master-slave来进行最基本的复制。slave实例可以可以精确的复制master实例的数据。如果断链，slave会自动重连，不论master发生任何情况，slave都会尝试去成为master的精确副本。

系统需要使用三台主要机器：

  1. 当master和slave之间的连接良好，当master端数据集发生变化，master会通过发送stream命令给slave来保证slave更新。这些变化包括：客户端写入，秘钥过期或已逐出，更改master数据集的任何其他操作。
