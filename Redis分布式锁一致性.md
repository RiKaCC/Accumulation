## 什么是分布式锁
在分布式系统中，不同进程使用同一个所去获取同一个资源。

## 分布式锁使用场景
多个进程需要互斥访问同一个资源。

## 分布式锁安全保证
1. 互斥保证，任何时刻只能有一个进程获取到锁
2. 无死锁：需要对所设置过期时间，不能因为获取到锁的进程挂了，而导致死锁的产生。

## 基于Redis的分布式锁解决方案
### 互斥保证：
使用redis的SETNX命令。SETNX的说明如下：
```
SETNX key value

将 key 的值设为 value ，当且仅当 key 不存在。

若给定的 key 已经存在，则 SETNX 不做任何动作。

SETNX 是『SET if Not eXists』(如果不存在，则 SET)的简写。

时间复杂度：
O(1)
返回值：
设置成功，返回 1 。
设置失败，返回 0 。
```

## 无死锁
通过使用SETNX能够保证了分布式锁的互斥，同一时间内只能有一个进程获取到该锁。但如果获取到锁的进程挂了，那么这个锁就会一直存在，导致其他进程永远获取不到该锁，而产生了死锁的情况。

为了避免这个问题，在获取到锁之后，需要为该锁增加一个expiretime。

这里需要注意一个问题，看下如下代码：
```
... ...
n := redis.SETNX(key)
if n == 1 {
  redis.expire(key, expiretime)
}
... ...
```
若果程序在redis执行完SETNX之后就挂了，之后的expire就没设置进去，那么还是会产生死锁。

这里我们需要使用多参数的set方法。
```
可选参数

从 Redis 2.6.12 版本开始， SET 命令的行为可以通过一系列参数来修改：

EX second ：设置键的过期时间为 second 秒。 SET key value EX second 效果等同于 SETEX key second value 。
PX millisecond ：设置键的过期时间为 millisecond 毫秒。 SET key value PX millisecond 效果等同于 PSETEX key millisecond value 。
NX ：只在键不存在时，才对键进行设置操作。 SET key value NX 效果等同于 SETNX key value 。
XX ：只在键已经存在时，才对键进行设置操作。
因为 SET 命令可以通过参数来实现和 SETNX 、 SETEX 和 PSETEX 三个命令的效果，所以将来的 Redis 版本可能会废弃并最终移除 SETNX 、 SETEX 和 PSETEX 这三个命令。
```
