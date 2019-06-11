```
1. Golang的并发机制和CSP并发模型

要点：
并发模型通过Goroutine和channel来实现
通过通信来共享内存，可以不用加锁
goroutine
MPG模型：
M->可以理解为线程，真正干活的对象
P->G和M的调度对象，可以理解为CPU
G->Goroutine
```
