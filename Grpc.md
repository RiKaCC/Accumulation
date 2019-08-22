## 不会立即重连
grpc client连接后端失败后，并不会立即重连，而是会做一些退避策略。
```
不立即重连，就是怕太多的请求打到后端，增加网络负担。
```

几个参数：

INITIAL_BACKOFF (how long to wait after the first failure before retrying)

MULTIPLIER (factor with which to multiply backoff after a failed retry)

JITTER (by how much to randomize backoffs).

MAX_BACKOFF (upper bound on backoff)

MIN_CONNECT_TIMEOUT (minimum time we're willing to give a connection to complete)


## Stream Read
RecvMsg 会从流中读取完整的 gRPC 消息体，另外通过阅读源码可得知：

（1）RecvMsg 是阻塞等待的

（2）RecvMsg 当流成功/结束（调用了 Close）时，会返回 io.EOF

（3）RecvMsg 当流出现任何错误时，流会被中止，错误信息会包含 RPC 错误码。而在 RecvMsg 中可能出现如下错误：

io.EOF
io.ErrUnexpectedEOF
transport.ConnectionError
google.golang.org/grpc/codes
同时需要注意，默认的 MaxReceiveMessageSize 值为 1024 1024 4，建议不要超出

