---
markmap:
  autoFit: false
  colorFreezeLevel: 4
  initialExpandLevel: 3
  maxWidth: 500
  spacingHorizontal: 90
  spacingVertical: 10
---

# Java 知识图谱

## 基础

## 并发

### 进程和线程

#### 进程

##### 系统分配资源的最小单位

##### 进程崩溃不影响其他进程

##### 不共享内存，切换开销大

#### 线程

##### CPU 调度和执行的最小单位

##### 线程崩溃直接影响进程

##### 共享内存，切换开销小

#### 创建线程的方式

##### 实现 Runnable 接口

- 可以继承类
- 重写 run 方法，无返回值
- 不能抛出异常

##### 实现 Callable 接口

- 可以继承类
- 重写 call 方法，有返回值
- 可以抛出异常

##### 继承 Thread 类

- 不能继承类

#### 状态

##### 新建

- 创建，new Thread

##### 就绪

- 等待 CPU 执行

##### 运行

- 执行

##### 阻塞

- 等待阻塞 wait()
- 同步阻塞 synchronized
- 其他阻塞 sleep()

##### 死亡

- 执行完毕
- 异常

#### 死锁

##### 互斥

- 资源只能被一个线程占用
- 无法破坏，多线程必须共享资源

##### 请求与保持

- 获取不到资源时，不释放当前资源
- 破坏：一次申请全部资源

##### 不剥夺

- 不能剥夺其他线程资源
- 破坏：拿不到资源，释放自己的

##### 循环等待

- 循环等待资源
- 破坏：锁排序，指定获取锁的顺序，ReentrantLock.tryLock()

### 线程池

#### execute 和 submit 的区别

- `execute`：提交不需要返回的任务，无法判断是否成功
- `submit`：提交需要返回值的任务，返回 `Future` 对象

#### 核心参数

##### 核心线程大小 `corePoolSize`

- 线程池运行，核心线程不停止

##### 最大线程数量 `maximumPoolSize`

- 非核心 + 核心
- CPU 密集型：线程数约等于 CPU 核心数
- IO 密集型：线程数约等于 `CPU 核心数 / (1 - 阻塞系数)`

##### 非核心线程的心跳时间 `keepAliveTime`

##### 阻塞队列 `workQueue`

###### newCachedThreadPool

- `SynchronousQueue`：直接转发队列，无限线程
- 线程数量无限制
- 需要控制数量避免 OOM

###### newFixedThreadPool

- `LinkedBlockingQueue`：无界队列
- 提交任务创建线程
- 达到最大数量，任务存入队列

###### newSingleThreadExecutor

- `LinkedBlockingQueue`：无界队列
- 单线程的 `Executor`
- FIFO

###### newScheduleThreadPool

- `DelayedWorkQueue`：延时队列
- 定长，核心线程数量固定
- 定时
- 周期

##### 饱和策略 `defaultHandler`

- `AbortPolicy`：丢弃并直接报错，默认策略
- `DiscardPolicy`：丢弃但不报错
- `DiscardOldestPolicy`：丢弃队列首部任务
- `CallerRunsPolicy`：在线程池外直接调用 `run` 执行

##### 线程工厂 `threadFactory`

#### 怎么复用线程的

##### ThreadPoolExecutor

###### 内置对象 Worker

- while 死循环从对象中拉取数据
- 置换 `Worker` 中的 `Runnable` 对象
- 运行 `run` 方法

#### Executor 和 Executors 的区别

##### Executor

- 接口，ExecutorService 继承并扩展，获取任务状态 / 返回值

##### Executors

- 工具类，创建线程池

### ThreadLocal

#### 每个线程保存变量副本，每次只修改自己的副本

#### 应用

- 数据库连接上下文
- 用户上下文
- 会话

#### 原理

- 每个线程保存一个 ThreadLocalMap（Entry 数组）
- key 为 ThreadLocal 对象，value 为变量值

#### 内存泄漏

##### Entry 数组的 key 是弱引用

##### key 回收后 value 仍可能存在

##### 调用 remove() 方法及时回收

### ReentrantLock

#### 可重入锁 / 可公平锁

#### 依赖 AQS 实现，维护一个阻塞队列

#### 缺点：读写互斥时可能阻塞只读线程

##### ReadWriteLock 接口

##### ReentrantReadWriteLock 实现

- 读共享，写独占
- 原理：把 AQS 的 state 拆分为读计数 / 写计数

### AbstractQueuedSynchronizer

#### 结构

##### 一个 CAS 修改的 volatile int 类型的 state

- park
- unpark

##### 一个 FIFO ==双向== 同步队列

- 线程异常时，方便移除
- 忙等时挂起（前驱已经在等待了，挂起当前）
- 支持从队尾开始判断是否在队列中
- 减少并发，源码很多从==队尾==遍历，猜测是减少队头的并发

##### Condition 条件队列

- 开发者显示调用 Condition.signal() 放入同步队尾，而不是直接执行

##### Node

###### 被封装的线程

###### 前驱 / 后继

###### 共享模式 tryAcquireShared

- Semaphore：计数信号量，控制并发许可数
- CountDownLatch：倒计时门闩，等待多个线程完成
- ReadLock

###### 独占模式 tryAcquire

- ReentrantLock

### Compare And Swap

#### 保证原子性

##### 基于硬件的原子指令 cmpxchg

###### 锁定总线

###### CPU 禁止中断

###### 硬件实现

### CompletableFuture

#### 概念

##### Future

- 未来才会有结果，现在返回 Future

##### result

- Future 里存放的结果

##### CompletableFuture

- 相比 Future，get 结果后还能执行任务

##### Completion

- 后续任务

##### Stack

- 存放后续任务

#### 底层实现

##### result

- volatile
- null 未完成
- 正常值
- AltResult 包装异常 / null / 取消

##### stack

- Completion 依赖栈
- thenApply / thenAccept 注册回调
- 上游完成后触发下游

##### CAS

- 原子设置 result
- 只能完成一次
- 完成后 postComplete

##### Executor

- Async 提交线程池
- 默认 ForkJoinPool.commonPool
- 可自定义 Executor
- 非 Async 复用完成线程

### synchronized

#### 锁类型

##### 偏向锁 01

- 锁对象的 Mark Word 中存储 Thread ID

##### 轻量级锁 00

- 线程在自己的栈帧中创建 Lock Record
- 使用 CAS 将对象 Mark Word 替换为指向 Lock Record 的指针
- 如果是自己，再压一个空锁
- 不是自己，CAS 等待
- 举例：线程 A 有栈帧 A，想要获取 Object 的锁，就 CAS 把 Object 的 Mark Word 移到自己的栈帧中

##### 重量级锁 10

- Mark Word 存的是指向 ObjectMonitor 的指针
- ObjectMonitor（owner, WaitSet, EntryList）

##### 11 为 GC 标记，不参与 synchronized 锁

#### 锁升级

##### 偏向锁 --> 轻量级锁

###### 线程不一致升级

##### 轻量级锁 --> 重量级锁

###### 自旋获取，超次数升级

#### 锁降级

##### 锁状态检查

- STW 时，检查所有 Monitor

##### 确定降级对象

- Monitor 计数器
- 持有者

##### 降级操作

- 恢复锁对象的 Mark Word 对象头
- 重置 ObjectMonitor

#### 优化

##### 锁升级

##### 锁消除

##### 锁粗化

##### 自旋锁 / 自适应自旋锁

- 自旋锁：轻量级锁没获取到时，不让出 CPU，进行循环等待
- 自适应自旋锁，自旋次数由上一次的类似情况决定

#### 用法

##### 普通方法

###### 当前实例

##### 静态方法

###### 当前类

##### 代码块

###### 进入前要获取指定的锁

#### 特性

- 原子性
- 可见性
- 有序性

### ForkJoin - 分而治之

#### 概念

- compute：当前任务具体怎么执行
- fork：把子任务丢出去异步执行
- join：等待子任务执行完并拿结果

#### 适用场景

##### 大任务分解为小任务

##### 计算密集型

##### 异构任务并行处理

##### 递归算法并行化

##### 数据聚合任务

#### 优势

##### 可分解

- 天然适合快排
- 左 fork，右 compute，左 join

##### 并行减少处理时间

##### 可伸缩

- 默认根据CPU核心数初始化
- 可以手动指定
- ==managedBlock== 阻塞时会考虑是否增加线程

### 常见对比

#### run / start、wait / sleep、notify / notifyAll区别?

##### start / run

###### start

- 线程的入口
- 启动后会执行 run 方法

###### run

- 普通方法调用，不会启动新线程
- 由当前线程同步执行

##### wait / sleep

###### sleep

- 任何地方使用
- 不会释放锁
- 针对的是==线程==，所以定义在==Thread==中

###### wait

- 同步方法 / 同步代码块中使用
- 释放锁
- 针对的是==对象==，所以定义在==Object==中

##### notify / notifyAll

###### 针对的都是对象，定义在==Object==中

###### notify

- 唤醒一个

###### notifyAll

- 唤醒所有

## JVM

## MySQL

### 基础

#### 数据库三范式

##### 第一范式

- 每一列都是不可分割的原子项

##### 第二范式

- 每一列的属性都完全依赖于主属性
- 根据主属性能够完全确定

##### 第三范式

- 任何非主属性都不依赖于其他非主属性

#### 存储引擎

##### InnoDB

- 支持事务
- 支持外键
- 聚簇索引
- 不支持全文索引
- 不保存表的行数
- 支持行级锁，表级锁

##### MyISAM

- 不支持事务
- 不支持外键
- 非聚簇索引
- 全文索引
- 保存表的行数
- 表级锁

#### 键

##### 超键

###### 主键

- 唯一完整标识

###### 候选键

- 最小超键，没有冗余

##### 外键

- 另外一个表的主键

#### IN / EXISTS

##### IN

- 外表和内表做 Hash 连接
- ==子表大==的时候用
- NOT IN 没有用索引
- 新版本将 NOT IN / NOT EXISTS 优化成 Anti Join，可能使用索引

##### EXISTS

- 对外表做 Loop 循环
- ==子表小==的时候用
- NOT EXISTS 用索引了

#### MySQL 执行查询过程

- 连接：TCP 请求到 MySQL 服务器
- 鉴权：权限验证，连接资源分配
- 缓存：查询完全相同的 SQL 缓存
- 分析：预处理器进行语法分析
- 优化：是否使用索引，生成执行计划
- 执行：结果保存到结果集，逐步同步缓存，返回给客户端

### 索引

#### 索引分类

##### 应用层次

###### 普通索引

- 只包含单个列

###### 唯一索引

- 索引值唯一
- 可以有多个 NULL

###### 复合索引

- 多个列组成

##### 聚簇 / 非聚簇

###### 聚簇索引

- 索引和值存在一起
- 一般使用主键作为聚簇索引

###### 非聚簇索引

- 索引和地址放在一起
- 需要二次查询

##### 存储结构

###### Hash 索引

- 为所有的索引列建立哈希码
- 精确匹配索引所有列的查询才生效

###### B-Tree

- 数据直接在树的各个节点上

###### B+Tree

- 数据在叶子节点上
- 增加顺序访问指针
- 叶子节点为聚簇索引，非叶子节点为非聚簇索引

#### 最左前缀

- WHERE 中最频繁的一列放在最左边
- 一直向右匹配直到范围查询
- = 和 IN 可以==乱序==

#### 索引下推

- 5.6 版本引入，默认开启
- 减少回表次数
- 只对二级索引有效
- 没有使用会把数据返回到服务端进行过滤
- 使用后会在==服务端==对==索引==进行过滤

#### EXPLAIN

##### id

- 每个 SELECT 关键字映射成一个 id
- 如果两个有相同的 id，说明把子查询优化成连接查询了

##### select_type

- SIMPLE
- PRIMARY
- SUBQUERY
- DEPENDENT
- UNION

##### table

##### ==type==

- system
- const
- eq_ref
- ref
- range
- index
- all

##### possible_keys

- 可能用到的 key

##### key

- 实际用到的 key

##### filtered

- 查询器预测满足下一次查询条件的百分比

##### ==rows==

- 估算需要扫描的行数

##### extra

- 额外信息
- 如 Using where、Start temporary、End temporary、Using temporary 等

#### 创建索引需要注意

##### 非空

- 很难进行查询优化，难以统计
- 使用 0、空串占位

##### 离散值大

- count 查看字段差异值

##### 索引字段越小越好

#### 建索引原则

- 最左前缀匹配
- = 和 IN 可以乱序
- 区分度高
- 索引列不参与计算
- 尽可能扩展，不要新建

#### 索引失效

- != 或者 <>
- 类型不一致
- 函数
- 运算符
- OR
- 模糊查询
- NOT EXISTS

### 事务

#### 四个特征

##### 原子性

- 所有操作要么都做，要么都不做

##### 一致性

- 从一个状态到另一个状态

##### 隔离性

- 一个事务不能影响另一个事务

##### 持久性

- 事务提交，数据库影响是永久的

#### 隔离级别

##### 读未提交 RU

###### 会读到其他未提交事务修改的内容

###### ==脏读==

- 读到了其他事务未提交的数据

##### 读已提交 RC

###### 会读到其他事务提交修改的内容

###### ==不可重复读==

- 其他事务多次提交，导致每次读取到的不一样

###### 每条==SQL==创建一个 Read View

###### 大多数数据库默认级别

##### 可重复读 RR

###### 同一事务处理过程中读到的是一致的

###### ==幻读==

- 修改后某一行没有修改过来，像幻觉一样
- 侧重于插入 / 删除

###### 每个==事务==创建一个 Read View

###### MySQL 默认隔离级别

##### 可串行化 Serializable

###### 强制事务排序

###### 加共享读锁

###### 分布式事务

#### MVCC

##### 流程

- 执行 `UPDATE balance = 200`
- 对记录加排他锁
- 将数据页从磁盘加载到 Buffer Pool
- 将旧版本 `balance = 100` 写入 Buffer Pool 中的 Undo Page
- 修改 Buffer Pool 中当前数据页的记录为 `balance = 200`
- 当前记录通过回滚指针关联 Undo Log
- 将本次修改写入 Redo Log Buffer
- 其他事务在 RC/RR 的普通快照读中，根据 Read View 判断可见性：
  - 看不到未提交的 `200`
  - 沿 Undo Log 读取旧版本 `100`
- 事务提交时，将 Redo Log Buffer 刷入磁盘上的 Redo Log
- Redo Log 持久化成功后，事务提交具备崩溃恢复能力
- Buffer Pool 中被修改的数据页和 Undo Page 标记为脏页
- 脏页之后由后台线程异步刷入数据文件和 Undo Tablespace

#### binlog

##### statement

###### 基于 SQL 语句的模式

##### row

###### 基于行的模式，记录的是行的变化，很安全

##### mixed

###### 根据语句来选用是 statement 还是 row 模式

### 主从同步 / 读写分离

#### Binary log / Relay log

##### Binary log

- 主数据库的二进制日志
- update -> binlog cache -> 准备提交 -> cache 刷盘 -> 提交完成

##### Relay log

- 从服务器的中继日志

##### 格式

- `STATEMENT`：记录 SQL 语句
- `ROW`：记录具体行的前后变化
- `MIXED`：由 MySQL 根据情况选择 `STATEMENT` 或 `ROW`

#### 主从同步过程

##### 主库执行事务

- 主库将变更记录为 Binlog Event
- 事务提交时，将事务事件写入 Binlog

##### 从库接收日志

- 从库 I/O 线程连接主库
- 拉取主库 Binlog Event
- 将事件写入从库 Relay Log

##### 从库应用日志

- 从库 Applier 线程读取 Relay Log
- 解析 Binlog Event
- 根据 `STATEMENT` 或 `ROW` 格式执行变更
- 更新已执行位置或 GTID

## Redis

## 消息队列

### 总结

#### Kafka

- 偏事件流 / 日志流，核心是 Partition Log、offset、长期保留、回放和大数据生态。

#### RocketMQ

- 偏业务消息，核心是事务消息、延迟消息、顺序消息、重试、死信和消息轨迹。

#### RabbitMQ

- 偏灵活路由和任务队列，核心是 Exchange、Binding、RoutingKey、Queue、ACK / Confirm。

### Kafka

#### 一句话概括

- 分布式提交日志

#### 架构

##### Producer：ack = 0 / 1 / -1

##### Broker：

- Topic 是逻辑分类，Partition 是 Topic 的分片，Partition 的 Leader / Follower 副本分布在不同 Broker 上
- ISR 旧 Follower 落后条数，短期大量消息会导致 Follower 下线
- ISR 新 Follower 落后条数 + 落后时间

##### Consumer：手动提交偏移量，无法依靠 Kafka 保证幂等；

#### Kafka 元数据管理

##### ZooKeeper 模式

- Broker 注册临时节点
- 临时节点依赖 session
- session 超时后节点删除
- Controller 监听 ZK 感知 Broker 上下线
- 元数据存储在 ZooKeeper

##### KRaft 模式

- Kafka 自管理元数据
- Controller Quorum 基于 Raft
- Controller Leader 由多数派选举
- 元数据变更由多数派提交
- 不再依赖 ZooKeeper

#### 消息模型

##### 发布-订阅模型

- Kafka 基于 Topic 发布消息
- 不同 Consumer Group 可以各自消费同一条消息
- 适合日志采集、事件通知、数据分发等场景

##### 点对点消费语义

- Kafka 底层不是传统 Queue 模型
- 但可以通过 Consumer Group 实现类似点对点效果
- 同一个 Consumer Group 内，一条消息只会被其中一个 Consumer 消费
- 不同 Consumer Group 之间互不影响，各自维护 offset

##### 拉数据

#### 高吞吐量设计

##### 磁盘顺序写

- Partition 追加写：Producer -> Broker -> Partition Log -> Segment File
- 避免随机 IO，顺序写性能高

##### Page Cache

- 写入优先进入操作系统页缓存
- 读取命中 Page Cache 时不需要真实读磁盘
- 依赖操作系统管理缓存和刷盘

##### 零拷贝

- Consumer 拉取消息时减少用户态和内核态之间的数据复制
- 降低 CPU 开销，提高网络发送效率

##### 批量处理

- Producer 批量发送
- Broker 批量追加写入
- Consumer 批量拉取
- 减少网络请求、系统调用和 IO 次数

##### 分区并行

- Topic 拆成多个 Partition
- 不同 Partition 可以分布在不同 Broker
- Producer 可并行写多个 Partition
- Consumer Group 可并行消费多个 Partition
- 分区内有序，分区间并行

##### 存储结构简单

- 追加日志 + offset
- 主要维护 segment、index、time index
- Consumer 消费进度通过 offset 管理

#### 副本同步

##### Leader / Follower

- Partition 有 Leader 和 Follower 副本
- Producer 和 Consumer 主要访问 Leader
- Follower 从 Leader 拉取数据

##### ISR

- ISR 表示和 Leader 保持同步的副本集合
- 落后太多的 Follower 会被移出 ISR

##### 可靠性参数

- acks = all 表示等待 ISR 中副本确认
- min.insync.replicas 表示最少同步副本数
- acks = all + min.insync.replicas 可以提高消息可靠性

#### 顺序性

##### 分区内有序

- Kafka 只保证单个 Partition 内有序
- 不保证 Topic 全局有序
- 保证有序：Partition 中只有一个 Topic / 业务侧排序

##### 如何保证业务顺序

- 需要有序的业务使用相同 key
- 相同 key 会进入同一个 Partition
- 例如同一个 orderId 的创建、支付、发货消息进入同一分区

#### 重平衡

##### 触发条件

- Consumer 数量变化
- Topic 数量变化
- Partition 数量变化

##### 具体步骤

- 暂停消费
- 重新计算
- 通知消费者
- 重新分区
- 恢复消费

#### 事务

- 业务处理后提交 offset + 幂等

### RocketMQ

#### 一句话概括

- 面向业务事件的消息中间件

#### 架构

##### Producer

##### Broker（Topic（独立 Queue，Queue，...），Topic，...）

##### Consumer

##### NameServer：维护 Broker 的元数据

#### 集群

- 单 Master
- 多 Master
- 多 Master，多 Slave

#### 消息模型

- 拉数据
- 基于拉的推数据

#### 可靠性保证

- 生产者选用同步推送，接收回调
- Broker 使用集群部署，且改为同步刷盘
- 消费者 ACK 确认

#### 顺序性

- 同步发送
- 指定 Queue
- 有序消费模式（另一种并发消费模式）

#### 事务

- 提交半消息，保存成功，全消息

#### 消费模式

##### 集群消费

- 默认模式
- 同组 Consumer 分摊消息
- 一条消息同组内只消费一次
- 支持水平扩容
- 适合异步任务和业务处理

##### 广播消费

- 同组每个 Consumer 都消费一遍
- 不做负载均衡分摊
- 适合配置通知、本地缓存刷新
- 不适合扣库存、加积分等业务

#### 延时消息

- Timer
- 时间轮

### RabbitMQ

#### 一句话概括

- 基于 Exchange 的灵活路由消息队列

#### 架构

- Producer
- Broker（Exchange <-- Binding --> Queue）
- Consumer

#### 可靠性保证

##### 持久化交换机

##### 持久化队列

##### 消费者确认

##### 默认生产者不会等待回执

##### Publisher Confirm

- 解决有没有到达 Broker，不保证倒霉到达 Queue，仍有可能丢失

#### 重复消费

- 正常流程：发送 -> 确认 -> 删除
- 消费者：一锁二判三更新

#### 可靠性保证

##### 普通集群模式

- 元数据所有实例共享
- 数据本质分片

##### 镜像模式

- 所有实例一样
- 需同步

#### 消息模型

##### 简单模式

- 同步
- 单生产者 / 单消费者

##### 工作队列模式

- 单生产者 / 多消费者

##### 发布订阅模式

##### 路由模式

##### 主题模式

##### RPC 模式

- 分布式

#### 事务

- 主要是生产者的 AMQP 事务
- 开启事务 -> 提交消息 -> 提交事务 -> 失败回滚
- 通常用 Publisher Confirm 替代
- 到达 Broker 靠确认，到达 Queue 靠开启默认退回

#### 死信队列

- 处理失败
- 过期
- 拒绝
- 无法路由

### ActiveMQ

## Agent 开发

### 常见问题

#### 避免 PDF 中表格被切断

##### 页面解析

##### 判断文本 / 表格

##### 正文按段 / 表格按表

##### 注意事项

- 表格分片按行
- 均保留表头
- 表格转 MD 而不是文本
- 添加元数据
- 分片器添加规则

#### 多模态检索分配权重

##### 固定权重

- 文字说明多 / 图表截图多 / 商品图片检索
- 归一化

##### 按问题类型分配

- 这个图片里的异常点在哪里？ text 0.3 + image 0.7
- 这个参数是什么意思？ text 0.8 + image 0.2
- 按关键词规则判断

##### 两路召回再重排

- 文本向量召回 TopK
- 图片向量召回 TopK
- 合并去重
- 用 reranker / 多模态模型重排

##### 学习型权重最终得分

- text_score
- image_score
- query_type
- 是否包含“图 / 表 / 截图”
- chunk 类型：text / table / image
- 历史命中率

#### 工程和 Prompt 防止大模型幻觉

##### 产生原因

- 预测补全
- 上下文不足
- 上下文腐化
- 检索召回错误
- 没有 reranker
- 没有对工具结果进行校验
- Prompt 约束不足

##### 工程为主，Prompt 为辅

###### RAG 只允许基于检索结果而非常识

###### 返回答案必须带引用，否则标记为推测

- 规定引用范围，不允许生成
- 引用做二次校验

###### 检索质量控制

- TopK 召回 / 多路召回
- reranker 重排
- 相似度阈值
- 主子分片
- 表格 / 图片结构化

###### 上下文完整性

###### 答案后校验

##### Prompt 添加规则，Agent 调用 Prompt 时检查

##### 实用组合

- 混合检索：BM25 + 向量
- reranker 精排
- 相似度阈值
- 主子分片
- 答案引用，无证据不回答
- 结果校验，高风险工具确认

#### 长期画像 / 短期记忆存储

##### 长期画像：跨会话保留

- MySQL / PostgreSQL 结构化画像
- Redis 高频短期缓存

```json
{
  "user_id": "u123",
  "profile": {
    "role": "Java 后端开发",
    "experience_years": 3,
    "language": "zh-CN",
    "goals": ["准备面试", "构建 Java 后端知识图谱"],
    "preferences": ["解释要具体", "先讲原理再讲面试话术"]
  },
  "updated_at": "2026-07-29"
}
```

##### 短期聊天

- message 表
- Redis 会话缓存
- 本地上下文窗口
- 对象存储归档
- (conversation_id, user_id, role, content, created_at, token_count, metadata)

##### 压缩上下文

###### 长期记忆

- 长期目标
- 项目背景
- 反复出现的薄弱点
- 稳定决策

###### 短期记忆

- 最新目标
- 未解决问题
- 已确认结论
- 去除冲突后可转入长期记忆

###### 避免腐化

- 加时间戳，新覆盖旧
- 加置信度
- 可失效
- 避免推测
- 用户纠正优先

#### 大模型生成 JSON 调用工具

##### 最好用填参数形式而非 JSON

##### Prompt 中明确需要 JSON 格式

##### 代码中做转换 / 校验

##### 做幂等校验

##### 缺失信息提示用户补充

#### 检索策略

##### 问题理解

- 判断问题类型：事实、步骤、故障、对比、表格、图片
- 识别关键词：型号、字段、故障码、时间、版本

##### 多路召回

- 向量召回：语义相似
- BM25 / 关键词召回：精确匹配
- 表格 / OCR / 图片召回：结构化和多模态内容
- 多路结果合并去重

##### 过滤

- 权限过滤
- 版本过滤
- 时间过滤
- 文档类型过滤
- 业务域过滤

##### 重排

- Reranker 精排候选 chunk
- 解决语义相似但答非所问
- TopK 召回后取最相关 TopN

##### 上下文组装

- 命中小 chunk，回填 parent chunk
- 表格保留表头和标题
- 段落保留章节路径和相邻上下文
- 注入 citation_id

##### 置信度判断

- 分数低：拒答或追问
- 证据冲突：列出冲突来源
- 证据充分：回答并引用

#### Agent

##### Agent 自主规划工具使用

- 规划受 Skill 影响
- 理解目标
- 判断缺少信息
- 选择工具
- 执行工具
- 观察结果
- 更新计划
- 继续 / 结束

##### 主子 Agent 通信链路设计

###### 主 Agent

- 目标拆解
- 任务分派
- 结果校验
- 最终决策

###### 子 Agent

- 单一任务执行
- 返回结构化结果

###### 调度层

- 通信 / 状态 / 重试 / 日志 / 权限

###### 共享存储

- 保存任务上下文、文件与中间产物、工具结果、任务状态、Agent 通信消息、检索证据和引用
- 按 `task_id`、`tenant_id` 隔离数据
- 大文件只传 `artifact_id`，不直接复制内容
- Redis 保存短期状态，数据库保存任务记录，对象存储保存文件

###### 子任务挂了返回空怎么办？

- 格式化空结果让主任务能够识别
- 主任务采用状态机而不是猜
- 提供重试策略
- 主任务提供降级子任务
- 返回结果透明

###### 子任务返回错误数据怎么办？

- 子任务返回结构化数据
- 结构校验
- 证据校验
- 业务校验
- 一致性校验
- 决策处理

##### 能力复用 / Skill 管理

###### 能力分层

- Skill 流程模板
- Tool 工具调用
- Prompt 角色和约束
- Memory 用户记忆和长期偏好
- Policy 权限和安全

#### Prompt

##### System Prompt（规范）

- 定义角色、边界和安全规则
- 约束回答风格和可用能力
- 优先级高于用户输入

##### Few Shot（如样例 BPMN）

- 给模型提供输入 / 输出示例
- 稳定回答格式
- 降低理解偏差
- 适合分类、抽取、JSON 输出、复杂格式任务

##### Chain of Thought（mini 版本 Skill）

- 引导模型分步分析
- 适合复杂推理、规划、诊断
- 生产环境不一定展示完整推理
- 可改为输出简短理由或结构化步骤

#### RAG

##### 增量文本 RAG

###### 给文档和 chunk 建立稳定 ID（内容 hash）

- doc / chunk ID
- hash

###### 文档变化重新分片做 diff

- hash 相同：不变
- 新 chunk：新增
- 更新 chunk：更新
- 删除已删除的 chunk

###### 查询注意版本一致性

##### 检索召回不准

###### 检查分块

- chunk 是否能表达完整语义
- 表格是否切断
- chunk 是否过长或过短
- chunk 是否保存了元数据
- 标题和正文是否一起保存了

###### 检查 Embedding 和查询改写

- 问题和检索是否使用相同 Embedding 模型
- 中英文、数字、专业词汇是否支持良好
- 是否需要问题改写
- 是否需要补充同义词和关键词

###### 加混合检索（解决语义和关键词匹配问题）

- 向量检索
- BM25 关键词检索
- 合并去重

###### 优化 Reranker

- Reranker 模型
- TopK 候选数量
- TopN 截断数量
- 问题和 Chunk 拼接格式
- 分数预支

##### 优化推荐顺序

- 建立评测集
- 检测正确 Chunk 是否进入 TopK
- 优化分块
- 优化 Embedding 和查询改写
- 增加混合检索
- 优化 Reranker
- 优化上下文组装

#### Skill 过多无法命中

- 修改 description，说明什么时候调用
- 添加负向样例，说明什么时候不要调用
- Skill 分层，先找大的，后找小的
- 召回 + 重排

#### Function Calling 模型如何决定调用工具

##### 流程

- HTTPS
- JSON API 请求 / 响应
- 模型输出结构化 tool call
- 应用代码执行工具

##### 坑

###### JSON 格式错误

- 代码解析
- 失败重试

###### 参数类型错误

- JSON Schema 校验 / 业务校验

###### 参数缺失或多余

- 后端严格限制格式

###### 工具选错

- 工具白名单
- 用户权限校验
- 高风险操作二次确认

###### 重复调用

- 业务幂等
- 唯一键
- 状态查询

###### 并行 Tool Call

- 是否允许并发
- 是否需要串行
- 是否存在依赖
- 是否共享事务

###### 上下文过大

- 分页
- 摘要
- 字段过滤
- 结果引用

##### 安全推荐流程

- 模型输出 tool call
- 解析 JSON
- 校验 tool name 白名单
- 校验 JSON Schema
- 校验用户权限 / 租户
- 校验业务规则
- 幂等检查
- 执行工具
- 校验结果
- 返回 Tool Result

##### 工具调用出错

###### Tool 层 **工具可靠**

- 参数校验
- 业务字段校验
- 返回结果校验
- 超时
- 限流
- 幂等
- 错误码转换
- 脱敏

###### Agent 层 **重试降级**

- Schema 是否正确
- 数据是否完整
- 证据是否可信
- 业务规则是否满足

###### System 层 **统一治理**

- 超时 / 重试 / 熔断 / 限流
- 并发控制
- 权限校验
- 审计日志
- Trace 链路
- 结果校验
- 秘钥管理
- 任务取消

#### MCP 工具如何被发现、描述和连接

##### MCP 能不能返回流式

###### 支持 HTTP / SSE 流式传输

- HTTP + SSE 已不受 Codex 支持

###### 支持新版 Streamable HTTP

###### 但标准 tool call 通常返回完整 result

##### MCP 如何保证并发

###### 协议层

- 基于 JSON-RPC 2.0，每个 request 有不同的 id

###### 服务层

- HTTP Server 使用异步框架
- 每个 request 分配独立 task
- 工具调用使用线程池
- 长任务异步

###### Tool 层

- 无共享状态 / 加锁

#### Lang

##### LangChain 组件库：搭建 Agent / 工具调用 / Prompt / 模型调用链

###### RAG 问答

- Retriever
- PromptTemplate
- LLM Model

###### 简单工具调用

- Tool
- OutputParser

###### 文档加载 / 分片

- DocumentLoader
- TextSplitter

###### 向量库接入

- Embedding Model
- VectorStore

###### Prompt 编排

- PromptTemplate
- Chain

###### 简单链式流程

- Chain

##### LangGraph 流程编排框架：构建有状态 / 多步骤 / 可循环 / 可中断的 Agent 工作流

###### 状态 / 节点 / 边 / 条件跳转

###### 循环 / 中断 / 恢复 / checkpoint

###### 多 Agent 协作

#### 验证准确性

##### 构建评测集分层识别

###### 意图识别

###### 实体抽取

- 人工标注问题里的关键实体
- 判断是否抽对

###### 检索召回

- 人工标注
- 系统召回
- 看目标 chunk 是否在 TopK 里

###### 重排

###### 答案

###### 引用

###### 工具调用

#### 大模型推理高并发优化手段

##### 请求调度

###### 批处理

- 已完成立即移出
- 新请求动态加入当前批次

###### 请求队列

- 排队削峰
- 限制并发数，防止打满

###### 优先级调度

- VIP 优先，实时请求优先
- 长文本限制并发，避免单个大请求阻塞

##### 缓存

###### KV Cache

- 缓存 Transformer 已计算的 Key / Value
- 避免重复计算
- 降低 Decode 阶段计算量

###### Prefix Cache

- 缓存请求前缀
- 比如相同 System Prompt 不重复计算

###### KV Cache 管理

- 分页管理缓存
- 按需分配和回收
- 避免碎片

##### 批处理与并行

###### Dynamic Batching

- 将多个请求合并成一个 Batch
- 减少 GPU Kernel 调度次数

###### Tensor Parallel

- 一个模型拆到多个 GPU
- 适合模型太大无法放入单卡

###### Data Parallel

- 每张 GPU 部署完整模型副本
- 请求按负载分发
- 适合提高整体吞吐量

###### Pipeline Parallel

- 模型不同层分布到不同 GPU
- 适合超大模型
- 但可能增加通信和流水线空隙

##### 模型压缩

###### 量化

- FP16/BF16 -> INT8/INT4
- 降低显存占用
- 提高并发和推理速度
- 可能损失部分精度

###### 蒸馏

- 用大模型训练小模型
- 降低单次推理成本

###### 剪枝

- 删除部分不重要参数或结构
- 实际收益取决于硬件和推理框架

##### 减少计算量

###### 控制上下文长度

- 限制输入 Token 数
- 压缩历史对话
- 只检索相关 RAG 片段

###### 限制输出长度

- 设置 max_tokens
- 避免模型无意义地生成过长答案

###### Speculative Decoding

- 小模型先预测多个 Token
- 大模型批量验证
- 预测正确时可以加速生成

###### 路由小模型

- 简单问题交给小模型
- 复杂问题才使用大模型

#### LLM 生成 SQL

##### 生成过程

###### LLM 生成结构化查询意图

###### LLM 生成 SQL

###### SQL 解析成 AST

- 是否是 SELECT
- 访问的表是否在白名单
- 访问的字段是否允许
- 是否存在 DELETE / DROP
- 是否有 LIMIT
- WHERE 条件是否包含 tenant_id

###### 安全规则校验

###### 业务规则校验

###### EXPLAIN / 预检查

###### 只读数据库执行

###### 脱敏返回

##### 三层 RAG

###### DDL / Schema 检索

- 按表为单位
- 基本信息、枚举信息、索引信息、关联关系
- 检索部分，返回时补充元数据

###### 业务规则检索

- 一条规则一个 chunk
- 规则过多时，处理方式与 Skill 类似，进行分层检索

###### Few-shot 示例

- 问题 + SQL + 解释为一个完整样例

#### Redis 在 Agent 系统作用

##### 作用

###### 会话上下文

###### 短期记忆

###### 任务状态

###### 幂等键

###### 分布式锁

###### 限流

###### 缓存 RAG 结果

###### 缓存 Tool 结果

###### Stream 任务队列

##### 流程

- 读取 summary
- 读取最近消息
- 读取当前任务状态
- 检索长期记忆
- 组装 Prompt

##### 注意问题

- A 和 B 同时修改会话导致消息顺序错乱
- session + version
- WATCH / MULTI / EXEC
- Redis Stream
- 会话级分布式锁

#### 评估 Agent 效果

##### 组件评估

- 意图识别：Accuracy / F1
- 召回：Recall@K
- 重排：MRR / nDCG
- 工具选择：Tool Accuracy
- 参数生成：Schema Accuracy
- 结构化输出：JSON Valid Rate

##### 链路评估

- 问题
- 意图识别
- RAG 检索
- 工具调用
- 结果组装
- 最终回答

##### 端到端任务评估

###### 有真实用户

- 用户目标是否真正完成

###### 无真实用户

- 构造评测集
- 运行 Agent
- 记录完整轨迹
- 自动计算指标
- 抽检异常案例
- 分析失败原因
- 修改 Prompt、RAG、Skill 或工具
- 重新评测

###### 关键指标

- 任务成功率
- 答案准确率
- 引用是否真正支持答案
- 正确知识是否出现在前 K 个召回结果中
- 工具选择准确率
- 工具参数准确率
- 结构化输出可解析比例
- 无依据结论比例
- 证据不足时正确拒答比例
- P95 延迟
- 单任务成本

#### 定位 Agent 问题

##### 给每次请求建立 Trace

- 意图识别
- RAG 召回
- Reranker
- 上下文组装
- 模型生成
- Tool 调用
- 最终回答

##### 每个阶段记录 Span

```text
Trace t-001
+-- intent_detection     80ms   SUCCESS
+-- retrieval            220ms  SUCCESS
|   +-- vector_recall    100ms
|   +-- bm25_recall      50ms
|   +-- reranker         70ms
+-- context_assembly     30ms   SUCCESS
+-- llm_generation       900ms  SUCCESS
+-- tool_call            300ms  FAILED
```

##### 保留中间产物

- 意图识别结果
- 实体提取结果
- 召回文档 ID 和分数
- Reranker 分数
- 最终注入模型的上下文
- 模型输出
- 工具名和参数
- 工具返回结果
- 最终答案和引用

##### 每一步使用结构化状态

- `SUCCESS`：继续
- `EMPTY`：扩大召回或换检索方式
- `TIMEOUT`：重试或降级
- `SCHEMA_ERROR`：修复输出
- `PERMISSION_DENIED`：直接拒绝

##### 可重放机制

- 问题
- Prompt 版本
- 模型版本
- Skill 版本
- 检索结果
- 工具参数
- 工具结果
- 配置版本

## 问题排查

### 问题记录

#### 代码未生效 2026/07/29

##### 现象：代码已提交，Jenkins 中 Maven 成功生成新 JAR，但新代码未生效

##### 原因：之前调整过 JDK 17 拉取配置，与当前 Dockerfile 不匹配，导致镜像构建、推送失败。流水线未中断，继续部署了旧镜像。

##### 解决方案

- 统一 Jenkins 与 Dockerfile 使用的 JDK 17 基础镜像，建议使用公司 Harbor 中的固定镜像。
- 清理或修复 Jenkins 节点的 BuildKit/镜像代理缓存。
- Docker build 或 push 失败时立即终止流水线，禁止继续部署。
- 新镜像推送成功后重新拉取并创建容器，确认镜像 digest 和运行 JAR 为最新版本。

## 场景题
