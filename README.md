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

```
2. Golang中常用的并发模型
channel
sync包的WaitGroupe
Context上下文（用来处理多个goroutine之间共享数据，以及管理多个goroutine ）
```

```
3.Golang GC过程
栈扫描（开始时STW）
第一次标记（并发）
第二次标记（STW）
清除（并发）

STW： Stop The World
```

```
主要是负责直播录制（点播）模块的设计与开发。


```
