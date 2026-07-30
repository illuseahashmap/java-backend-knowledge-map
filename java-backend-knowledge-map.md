---
markmap:
  autoFit: true
  colorFreezeLevel: 4
  initialExpandLevel: 4
  maxWidth: 500
  spacingHorizontal: 90
  spacingVertical: 10
---

# Java知识图谱

## 基础

## 并发

### AbstractQueuedSynchronizer

#### 结构

##### 一个 CAS 修改的 volatile 的 int 类型的 state

- park
- unpark

##### 一个 FIFO ==双向== 同步队列

- 线程异常时，方便移除
- 忙等时挂起（前驱已经在等待了，挂起当前）
- 支持从队尾开始判断是否在队列中
- 减少并发，源码很多从队尾遍历，猜测是减少队头的并发

##### Condition 条件队列

- 开发者显示调用 condition.signal() 放入同步队尾，而不是直接执行

##### Node

###### 被封装的线程

###### 前驱 / 后继

###### 共享模式 tryAcquireShared

- Semaphore
- CountDownLatch
- ReadLock

###### 独占模式 tryAcquire

- ReentrantLock


## JVM

## MySQL

## Redis

## 消息队列

### Kafka

#### 架构

##### Producer ： ack = 0/1/-1

##### Broker ：

- Topic 是逻辑分类，Partition 是 Topic 的分片，Partition 的 Leader / Follower 副本分布在不同 Broker 上
- ISR 旧 Follower落后条数，短期大量消息会导致Follower下线
- ISR 新 Follower落后条数 + 落后时间

##### Consumer ： 手动提交偏移量，无法依靠Kafka保证幂等；

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

##### Leader/Follower

- Partition 有 Leader 和 Follower 副本
- Producer 和 Consumer 主要访问 Leader
- Follower 从 Leader 拉取数据

##### ISR

- ISR 表示和 Leader 保持同步的副本集合
- 落后太多的 Follower 会被移出 ISR

##### 可靠性参数

- acks=all 表示等待 ISR 中副本确认
- min.insync.replicas 表示最少同步副本数
- acks=all + min.insync.replicas 可以提高消息可靠性

#### 顺序性

##### 分区内有序

- Kafka 只保证单个 Partition 内有序
- 不保证 Topic 全局有序
- 保证有序 ：Partition中只有一个Topic / 业务侧排序

##### 如何保证业务顺序

- 需要有序的业务使用相同 key
- 相同 key 会进入同一个 Partition
- 例如同一个 orderId 的创建、支付、发货消息进入同一分区

#### 重平衡

##### 触发条件

- Consumer数量变化
- Topic数量变化
- Partition数量变化

##### 具体步骤

- 暂停消费
- 重新计算
- 通知消费者
- 重新分区
- 恢复消费

#### 事务

- 业务处理后提交offset + 幂等

### RocketMQ

#### 架构

##### Producer

##### Broker（Topic（独立Queue，Queue，...），Topic，...）

##### Consumer

##### NameServer ： 维护Broker的元数据

#### 集群

- 单master
- 多master
- 多master，多slave

#### 消息模型

- 拉数据
- 基于拉的推数据

#### 可靠性保证

- 生产者选用同步推送，接收回调
- Broker使用集群部署，且改为同步刷盘
- 消费者Ack确认

#### 顺序性

- 同步发送
- 指定Queue
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

- 解决有没有到达Broker，不保证倒霉到达Queue，仍有可能丢失

#### 重复消费

- 正常流程：发送->确认->删除
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

##### RPC模式

- 分布式

#### 事务

- 主要是生产者的AMQP事务
- 开启事务 --> 提交消息 --> 提交事务 -->失败回滚
- 通常用Publisher Confirm替代
- 到达Broker靠确认，到达Queue靠开启默认退回

#### 死信队列

- 处理失败
- 过期
- 拒绝
- 无法路由

### ActiveMQ

## Agent开发

### 常见问题

#### 避免PDF中表格被切断

##### 页面解析

##### 判断文本/表格

##### 正文按段/表格按表

##### 注意事项

- 表格分片按行
- 均保留表头
- 表格转md而不是文本
- 添加元数据
- 分片器添加规则

#### 多模态检索分配权重

##### 固定权重

- 文字说明多/图表截图多/商品图片检索
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
- 是否包含“图/表/截图”
- chunk 类型：text/table/image
- 历史命中率

#### 工程和Prompt防止大模型幻觉

##### 工程为主，Prompt为辅

###### RAG只允许基于检索结果而非常识

###### 返回答案必须带引用，否则标记为推测

- 规定引用范围，不允许生成
- 引用做二次校验

###### 检索质量控制

- TopK召回 / 多路召回
- reranker重排
- 相似度阈值
- 主子分片
- 表格 / 图片结构化

###### 上下文完整性

###### 答案后校验

##### Prompt添加规则，Agent调用Prompt时检查

##### 实用组合

- 混合检索：BM25 + 向量
- reranker精排
- 相似度阈值
- 主子分片
- 答案引用，无证据不回答
- 结果校验，高风险工具确认

#### 长期画像/短期记忆存储

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

- message表
- Redis会话缓存
- 本地上下文窗口
- 对象存储归档
- (conversation_id, user_id, role, content, created_at, token_count, metadata)

#### 大模型生成json调用工具

##### 最好用填参数形式而非json

##### Prompt中明确需要json格式

##### 代码中做转换 / 校验

##### 做幂等校验

##### 缺失信息提示用户补充


## 问题排查

### 问题记录

#### 代码未生效 2026/07/29

##### 现象：代码已提交，Jenkins 中 Maven 成功生成新 JAR，但新代码未生效

##### 原因：之前调整过 JDK 17 拉取配置，与当前 Dockerfile 不匹配，导致镜像构建、推送失败。流水线未中断，继续部署了旧镜像。

##### 解决方案

- 统一 Jenkins 与 Dockerfile 使用的 JDK 17 基础镜像，建议使用公司 Harbor 中的固定镜像。
- 清理或修复 Jenkins 节点的 BuildKit/镜像代理缓存。
- Docker build或push失败时立即终止流水线，禁止继续部署。
- 新镜像推送成功后重新拉取并创建容器，确认镜像 digest 和运行 JAR 为最新版本。