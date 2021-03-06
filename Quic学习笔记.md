[Quic协议初探](http://ralphbupt.github.io/)

## 先谈前任Http2

>HTTP/2的一个主要特性是使用多路复用（multiplexing），因而它可以通过同一个TCP连接发送多个逻辑数据流。复用使得
很多事情变得更快更好，它带来更好的拥塞控制、更充分的带宽利用、更长久的TCP连接————这些都比以前更好了，链
路能更容易实现全速传输。标头压缩技术也减少了带宽的用量。

```
Q1: http2是怎么实现多路复用的？
A1: 可以参考下方的队首阻塞，Http2基于TCP的多路复用。

Q2: http2多路复用通过什么来区分每一路流？
Q3: http2的拥塞控制？
Q4: 什么是标头压缩？带来的好处是什么？
```

>采用HTTP/2后，浏览器对每个主机一般只需要 一个 TCP连接，而不是以前常见的 六个 连接。事实上，HTTP/2使用的连接
聚合（connection coalescing）和“去分片”（desharding）技术还可以进一步缩减连接数。

```
Q1: 什么是连接聚合？
Q2: 什么是去分片？
Q3: 连接聚合和区分片是怎么缩减连接数的？
Q4: 一般来说Http2需要几个连接？

```

>HTTP/2解决了HTTP的队头拥塞（head of line blocking）问题，客户端必须等待一个请求完成才能发送下一个请求的日子过
去了。

```
Q1: Http2是怎么解决队头拥塞的？
--- 多路复用？一个TCP连接可以承载多个传输数据流（目前学到这，我自己的理解）
```

### TCP队首阻塞
TCP连接的双方，只要有一方数据出现丢包，TCP就会进入等待的状态，丢失的数据包需要被重新传输。

可以把TCP的传输看成一个链式结构。

a,b,c均建立在同一个一个TCP连接上，可以简单用如下方式来表示传输：

```
----------------------------------------------------TCP-----------------------------------------------------
Client            a3->b3->c3->a2->b2->c2->a1->b1->c1         Server
----------------------------------------------------TCP-----------------------------------------------------
当任意一个数据包出现丢失，这里假设c2丢失，那么c2,b2,a2,c3,b3,a3都会继续等待，一直等待到c2重传成功。
```

下面简单画一下quic多路复用
```
---------------------------------quic----------------------------------------
                a3->a2->a1
Client          b3->b2->b1            Server
                c3->c2->c1
---------------------------------quic----------------------------------------
```

### Quic解决队首阻塞
Quic在一个连接上建立两个不同的流的时候，他们是相互独立的，也就是说，当一个流出现丢包，是不会影响第二个流的。

Quic使用基于UDP，当其中一个数据流包丢失的时候，其他的通道并不会因此阻塞等待丢失的数据包。
```
Q1: 怎么实现的独立？
---  Quic是基于UDP的，UDP是面向数据包的。
```

## Quic
Quic提供基于UDP的多路复用，有序，可靠的流传输。
Quic是和HTTP同一层的应用层协议，其核心就是将丢包控制工作转移到应用层。

```
Q1: Quic的多路复用实现？
A1: Quic的多路复用类似于Http2，但是Quic的优势在于，一个连接上的多个stream之间没有依赖，其实也就是独立的.

Q2: Quic是基于UDP的，是怎么做到有序，可靠的？
A1：Quic的每一个包都有一个Packet Number（类似于TCP的sequence number）,并且每一个Packet Number都单调递增。
    单调递增的好处就是，解决了重传的歧义。
    比如一个为N的包，出现了超时，发起了重传，然后客户端收到了ACK，由于序列号都是N，这时你并不知道这个ACK是原始数据包的ACK，还是重传包的ACK。
    Packet Number采用了单调递增，如果N发生了超时，这时会重传N+M，如果客户端收到的ACK N，那么就知道这时原始的请求N。
    如果收到了ACK N+M，那么就知道是发送的N+M，这样的好处是可以精确的计算出RTT。
    如果只是单纯的依靠递增的Packet Number也是无法保证数据的顺序性和可靠性的，如果N丢失了，重传了N+2，那么N+2是会放在N+1之后的。
    所以Quic还用了Stream Offset的概念。
    Packet N的Stream Offset为x, 而Packet N+1的Stream Offset为y, 如果Packet N丢失了，重传后Packet N+2，但是Packet N+2的Stream Offset还是x，
    这样哪怕Packet N+2在Packet N+1之后被收到，也能处理知道Packet N+2是Packet N+1之前的那个包。
```

### Quic的优点
#### 1. 精细的流量控制
依赖于三种关键帧
window_update帧，告诉对端自己“最多可以”接收的字节数的绝对偏移量。

blocked帧，告诉对端，流量控制被阻塞了，暂时无法发送。

stop_waiting帧，通知对端，不应该继续等待包小于特定值的包了。

#### 2. 无队头阻塞的多路复用
#### 3. 0RTT
在下面的Quic建立连接里有说。

```
Quic的连接直接将版本协商，握手，加密和传输交织在一起，这样减少了建立连接的延时。
```
#### 4. 连接迁移
```
Quic对于每一个连接，都有Connection ID,这样可以做到网络切换，Quic不断。
```
#### 5. 加密的认证报文
```
Quic对于每一个UDP都进行了TSL1.3的加密，对证书也有压缩优化。对于客户端的防盗链，盗播，劫持上有一定的好处。
```
#### 6. 向前纠错
```
Quic的每个数据包，不仅仅包含本身的数据，还会带有其他的数据包的部分数据，在少量丢包的情况下，可以使用其他数据包冗余的数据来完成数据的重装，这样就不需要重传。对于直播来说可以减少ARG、减少卡顿、提升秒开成功率。

Q1: Quic向前纠错是怎么实现的？

```
#### 7. 改进的拥塞控制

### 协议僵化
```
为了保证互联网之间的正常工作，我们需要在互联网各处搭建各种设备，这些设备需要支持某些协议（通过软件来支持），当某些协议有升级，有了新的特性，而这些设备并未更新，或者更新很慢。当收到这些更新后的协议时，设备会认为这是非法的，恶意的，设备会将这些流量扔掉。这就是协议僵化。

简单理解就是：协议更新，经过的设备没更新，更新后的协议通过设备，并不能被使用。
```

#### 解决协议僵化
```
尽可能将通信加密是对抗僵化的唯一有效手段，加密可以防止中间设备看到协议传输的绝大部分内容。
```

### Quic工作原理
#### 建连
```
quic连接是两个quic端点之间的单次会话。

quic建立连接的时候，加密算法的版本协商和传输层握手是合并完成的，以减小延时。

---  是否也可以理解为减少了RTT？

Quic连接的四个阶段：

启动
第一次连接
Client  ------CHLO--------> Server
        <--------REJ-------
       
  客户端发送一个空的hello包，也就是CHLO，服务端发送一个REJ包，在这个REJ包汇总发，包含有客户端需要的信息，包括token和服务端的证书（哈希表）。
  下次client再发送 CHLO包的时候，可以使用缓存的token与证书发送加密的请求给server
  
  客户端验证哈希表，如果验证失败
        ------发送包要求服务器发送完整的证书链(此时，进入2RTT)-------> 
       
正常来说，第一次连接一般为一个RTT，若第一次验证失败，则会有两个RTT。
验证成功，则客户端与服务端握手完毕。

再次连接
再次连接时，客户端会推测服务端仍会使用之前连接时已验证过的服务器证书，所以这时0RTT。
如果失败，则走第一次连接的流程，那么将会是1RTT或者2RTT。


稳定阶段
丢包
Client -------------------数据包-------------------> Server
       <--------否定确认包k(同时发送加密哈希表)--------- 
       ----客户端确认哈希表正确，并声明数据包k丢失，同时发送一个确认帧--->

客户端发送确认帧的目的：要求服务端停止否定确认包k。
客户端可以选择发送丢失包k中的数据到服务端，或不重传。
```

#### 连接ID（Connection ID）
```
每个连接都会有一个connection ID，该ID用来唯一标识这个连接 。

连接ID的基本功能：确保底层协议（UDP，IP以及其底层协议）的寻址变更不会使Quic连接传输的数据到达错误的端点。

比如：从4G切到 Wi-Fi的时候，quic不会因为这个改变而收到影响，因为连接ID并没有发生改变，而TCP就不行。
```

#### 端口号
```
QUIC基于UDP建立，因此使用16比特的UDP端口号字段来区分传入的不同连接。
```

#### 版本协商
```
客户端的QUIC连接请求会告知服务器所希望使用的QUIC协议版本，服务器端会回复一系列支持的版本供客户端选择。
```

#### 使用TLS的连接
```
在初始的数据包建立连接之后，连接发起者会马上发一个加密的帧以开始安全层握手。安全层使用TLS 1.3协议。
在QUIC中，没有方法或机制避免使用TLS连接。该设计旨在使中间设备难以篡改数据包，防止协议僵化。
```

#### 数据流
```
quic中有两种数据流类型：
从发起者到对等段(peer)的单向数据流
双向均可以发出数据的双向数据流

Q1: 那TCP的数据流呢？

数据流是可以被取消的。

通过quic发送数据需要建立一个或多个数据流。
```

#### 流量控制（Flow control）
```

```

## Quic与Http2的比较
### 相同
```
两者都提供数据流
两者都提供服务器推送
两者都有头部压缩，QPACK与HPACK的设计非常类似
两者都通过单一连接上的数据流提供复用
两者都提供数据流的优先度设置
```

### 不同
```
得益于QUIC的0-RTT握手，HTTP/3可以提供更好的早期数据支持，而TCP快速打开和TLS通常只能传输更少的数据，且经常存在问题。
得益于QUIC，HTTP/3的握手速度比TCP+TLS快得多。
HTTP/3不存在明文的不安全版本。尽管在互联网上很少见，HTTP/2还是可以不配合HTTPS来实现和使用。
通过ALPN拓展，HTTP/2可以直接在TLS握手时进行协商。HTTP/3基于QUIC，所以需要凭借响应中的 Alt-Svc: 头部来向客户端宣告。
```
