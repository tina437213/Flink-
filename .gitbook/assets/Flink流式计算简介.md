在此系列文章中，我们将深入研究如何构建健壮的、有状态的流处理应用程序。但首先，我们需要了解Flink应用程序的基础知识。
<a name="N6jRW"></a>
# 有状态流处理
<a name="CM1GJ"></a>
## 传统系统应用
<a name="nhrg1"></a>
### 事务型应用（OLTP）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643510481396-6d8a137e-2c40-446d-a16e-8bd088d2b189.png#clientId=u152ff1b7-6b6c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=330&id=u21a4b2e5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=412&originWidth=495&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18851&status=done&style=none&taskId=u634c050a-3e00-4587-8507-bb108ebf2f4&title=&width=396)<br />特点：

- 通常是Request/Response模式
- 两层结构+事务型数据库
- 每次请求通常只涉及有限的记录数（几行等）

举例

- 会议预定系统
- 电商
- 客户关系管理系统CRM
- Web应用



<a name="CKYlJ"></a>
### 分析型应用（OLAP）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643510744396-2fc7131a-ab8c-4ba1-b1f2-c37959115899.png#clientId=u152ff1b7-6b6c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=276&id=uf94ef7d3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=503&originWidth=1216&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36355&status=done&style=none&taskId=uf83767a0-6a94-49f6-9e48-146ef6f3790&title=&width=667)<br />事务型应用和分析型应用一般是分开建设的：

- 面向行访问和面向列访问；
- 事务型应用通常要求低延迟，分析型通常对延迟相对不敏感。

分析型应用的访问方式一般有2种：

- 报表
- 即席查询
<a name="SPF0w"></a>
### 流式应用
<a name="aLVaH"></a>
#### 无状态流计算
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643510974584-57e459e9-ac90-42e0-b068-e38252bc79ec.png#clientId=u152ff1b7-6b6c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=162&id=u3cdc7bb1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=277&originWidth=1120&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21173&status=done&style=none&taskId=u4e6660e8-0cb0-4b00-a827-145f86d9ec1&title=&width=654)<br />持续的接收和处理消息，同时也不断的发送处理后的消息。
<a name="fNMWb"></a>
#### 有状态流计算
​

![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643511028722-76505f95-196e-49bc-8479-2f0073468be2.png#clientId=u152ff1b7-6b6c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=195&id=u3fe13d60&margin=%5Bobject%20Object%5D&name=image.png&originHeight=327&originWidth=1107&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27205&status=done&style=none&taskId=u1dacb4c7-cab2-4963-a923-c997ae87ebf&title=&width=659)<br />一般来说，大部分的应用都是需要保持状态的。比如计数器、1分钟窗口内的最高max、最低值min。还有些更复杂的情况，状态用来计算一些异常检测、欺诈检测模型中的特征。FLink采用本地化保存状态模式，即保存在消息/事件被处理的机器上。这个状态存储在本地内存或者嵌入式数据库中。
<a name="GjRrs"></a>
# 应用案例
<a name="Jj5Xn"></a>
## streaming ETL
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643511173144-00aaa72e-0957-4124-b346-628f9d408a59.png#clientId=u152ff1b7-6b6c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=271&id=u3cd5dcbb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=642&originWidth=1656&originalType=binary&ratio=1&rotation=0&showTitle=false&size=54663&status=done&style=none&taskId=u16681fa1-958c-4775-a1f8-f65bebe0a26&title=&width=700)<br />流式数据经过流式ETL从一个位置搬到另一个位置，是一个非常典型的应用。
<a name="tekw2"></a>
## streaming analytics
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643511289644-af074268-5f58-4373-b532-c7c7cc80657c.png#clientId=u152ff1b7-6b6c-4&crop=0&crop=0.068&crop=1&crop=1&from=paste&height=305&id=u595b7f80&margin=%5Bobject%20Object%5D&name=image.png&originHeight=381&originWidth=1611&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36211&status=done&style=none&taskId=u379c0364-bc02-4597-b6b8-d577af4c973&title=&width=1289)<br />流式分析应用也是一个典型的应用场景，前提是需要对状态管理和时间管理有比较好的抽象。
<a name="Sg3jT"></a>
## 机器学习模型服务
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643511799897-1e4d750f-9f43-47fd-a6e0-555a21cb9c99.png#clientId=u152ff1b7-6b6c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=523&id=u06bfc18d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=654&originWidth=1662&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46819&status=done&style=none&taskId=ueb12c2a7-61aa-458f-8f4a-b4da5abb7ec&title=&width=1329.6)<br />Flink 还经常用于将机器学习模型（例如分类器）应用于实时事件流的应用程序中。如上图，我们沿着横轴来看，实时流消息被接收、特征被计算、模型被用来做预测。同时，同时，离线批处理过程会根据数据仓库中收集的数据定期重新训练模型。
<a name="rRheT"></a>
## 常见应用场景
供应链管理<br />个性化推荐<br />异常检测、欺诈检测<br />时间响应<br />

<a name="CXJSo"></a>
# Flink项目简介
<a name="lSoeB"></a>
## 什么是Flink？
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643533117473-8406a552-69ea-44c8-a096-da35866f2d45.png#clientId=u543d6f2d-cbb0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=344&id=uef6e2448&margin=%5Bobject%20Object%5D&name=image.png&originHeight=430&originWidth=991&originalType=binary&ratio=1&rotation=0&showTitle=false&size=50745&status=done&style=none&taskId=u3e8ef3df-6e02-437a-91ec-c1caf91637d&title=&width=792.8)<br />Flink的运行时是基于并行数据流引擎，支持对数据流的有状态计算。Flink支持一系列输入、输出连接器（connectors）。连接器包括消息队列、文件系统、数据库。除了最通用的流应用，低阶的Data Stream API以外，针对2种常见应用场景，我们开发出专属API（高阶API）：

1. streaming analytics：Flink关系型API，Flink SQL/Table API
1. event-driven applications：状态函数API
<a name="Kpeun"></a>
# Flink核心概念
<a name="QjYay"></a>
## Streaming的基石

- 事件流（event streams）
   - 实时、事后聪明
- 实践事件
   - 考虑无序和晚到数据下的一致性
- 状态
   - 复杂业务逻辑
- 快照
   - 容错
   - 版本管理
   - 时点数据（time-travel）

上面是Flink的核心概念。掌握了上面这些核心概念，就会很容易理解当前的API设计。
<a name="PHN1X"></a>
## 一切皆流
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643544102047-7aa1e038-93d4-459e-bad4-fe721f474e20.png#clientId=u543d6f2d-cbb0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=162&id=u33180f47&margin=%5Bobject%20Object%5D&name=image.png&originHeight=226&originWidth=971&originalType=binary&ratio=1&rotation=0&showTitle=false&size=52893&status=done&style=none&taskId=u048e2006-0e63-4da0-8611-4975eae4c24&title=&width=698)<br />在Flink社区，一直有个说法，就是批是流的一种特殊形式。数据源本质上是无界的。我们将有限数据集看做是我们选择了一些原本无序的数据集进行分析。
<a name="ztFEd"></a>
## 查询和数据，哪一个变化更快？
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643544599907-09017ee4-37f1-4b49-8fd0-0ef002b0352c.png#clientId=u543d6f2d-cbb0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=225&id=uac54db0e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=316&originWidth=926&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47991&status=done&style=none&taskId=u49e7de80-50ed-4568-8048-c1a23a6a6b7&title=&width=658.7999877929688)<br />批（Batch）

- 快速扫描数月/数年的历史
- 使用大规模并行无序读取优化处理
- 吞吐量最重要

流（Streaming）

- 保持实时处理，并且具备流发生中断后能继续追赶上的能力
- 大致按生产顺序接收数据
- 延迟敏感

​<br />
<a name="svcF5"></a>
## JobGraph
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643545644279-72013d64-40ac-436b-8996-423febdab048.png#clientId=u543d6f2d-cbb0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=181&id=u01961097&margin=%5Bobject%20Object%5D&name=image.png&originHeight=289&originWidth=1022&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21301&status=done&style=none&taskId=uec4ad904-6e25-44c8-ae1a-72a4e377edd&title=&width=640)<br />事件流在JobGraph的各个node间流动。<br />我们管这些图节点叫算子（operators）。一个运行的应用程序对应的节点集合就是一个JobGraph。<br />​<br />
<a name="t1k7N"></a>
## ExecutionGraph
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643545960549-3bf0c027-607d-42e6-ac49-d4270dd07322.png#clientId=u543d6f2d-cbb0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=267&id=ucbd84ea3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=459&originWidth=1008&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30816&status=done&style=none&taskId=u8aa2d166-fa92-4a82-a602-d1e8b4c38f2&title=&width=586)<br />这些 JobGraph 是并行运行的。我们来看下Job的Physical Graph或 ExecutionGraph。<br />​

Flink 对并行度提供了非常精细的控制，可以为整个Job设置，也可以为每个operator设置。<br />​

上图展示了这个Job中大部分operator都是2个并行度，但是sink是单线程的。<br />​

这个ExecutionGraph的第2层（filter）和第3层（read/write）算子是完全连接的。在这里，我们在worker之间执行一种完全连接的数据交换，有时称为network shuffle。数据会围绕着某种规则重新分区，分区后将同一个key的数据拉到同一个状态节点进行处理。例如，我们可能正在处理来自某移动端程序的事件流，我们可能会按用户 ID 对流进行分区或键控，以便我们可以在一个节点上收集有关每个用户的统计信息。
<a name="Hyq88"></a>
## Event time vs. processing time
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643547915519-bcb4b7ce-8da1-4487-b9c8-187e3cfde84f.png#clientId=u543d6f2d-cbb0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=288&id=uc8b8877c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=418&originWidth=741&originalType=binary&ratio=1&rotation=0&showTitle=false&size=158674&status=done&style=none&taskId=u599a5652-58fc-4a93-89d6-e5ac2e5a624&title=&width=510.79998779296875)<br />Flink 包含强大的 API 支持不同模式的时间管理（time management）。事件时间戳是由事件携带的，描述事件发生的时间，而处理处理时间描述事件被处理的时刻，一般相对是延后一点的，可能在数据中心处理。请注意，事件时间是事件的不可变特征，而处理时间是由处理事件的行为产生的非确定性、不可重现的副作用。如果重新处理一个事件，事件时间将相同，但处理时间不会！<br />​<br />
<a name="RDEAj"></a>
## (Stateful) stream processing
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643550483892-480c2024-ba03-4ca5-9720-5c2b4ce8218b.png#clientId=u543d6f2d-cbb0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=244&id=ubbf37ff6&margin=%5Bobject%20Object%5D&name=image.png&originHeight=327&originWidth=837&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26118&status=done&style=none&taskId=u9236aeb6-434e-4cc4-bd73-ab9b62215f9&title=&width=623.6000366210938)<br />Flink 的一些算子，比如做过滤和转换的算子，希望你以用户函数的形式提供业务逻辑。其中一些用户函数是有状态的。整个流计算应用都是在处理每一个到来的事件。这也意味着有时候我们需要记录一些事件的信息，他可能会影响后面结果的产生。<br />​<br />
<a name="gUTXr"></a>
## Stateful streaming snapshots
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643551085438-149a78ec-d05b-454a-8a04-752842d38fe8.png#clientId=u543d6f2d-cbb0-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=276&id=uf6a44bd7&margin=%5Bobject%20Object%5D&name=image.png&originHeight=345&originWidth=705&originalType=binary&ratio=1&rotation=0&showTitle=false&size=55221&status=done&style=none&taskId=ub18851d8-5cb4-421b-a0f2-e9f92222834&title=&width=564)<br />Flink是一个分布式系统，可以支持扩展到1000个节点，7*24运转。在这个规模上，发生局部故障是并不少见，所以必须有一个容错和恢复的解决方案。<br />​

每个key(e.g. user)的状态是存储到接收这个key对应的消息的node本地。你可以将此keyed state视为分片键值存储。这种设计对于 Flink 能够扩展到数千个节点并同时实现低延迟和高吞吐量的能力起着至关重要的作用。Flink 会定期扫描整个集群，并将所有这些checkpoints到一个持久的分布式文件系统。checkpoint工作会在后台完成，不会中断正在进行的流处理。<br />​<br />
<a name="ROY8O"></a>
## Recover by rolling back
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643552715574-53e6b2f0-74e5-4f37-bbbe-009f7b82bfc3.png#clientId=u543d6f2d-cbb0-4&crop=0.0161&crop=0.0303&crop=1&crop=0.973&from=paste&height=267&id=u6395bfe6&margin=%5Bobject%20Object%5D&name=image.png&originHeight=371&originWidth=778&originalType=binary&ratio=1&rotation=0&showTitle=false&size=33975&status=done&style=none&taskId=u07f9b7e9-74ea-417c-bab9-93370112ac1&title=&width=560)<br />如果发生故障，Flink 通过从最近的checkpoints重新加载状态来恢复。同时，输入流被重置为与checkpoints对应的偏移量，并继续处理。这是整个集群的全局回滚。
<a name="yk98V"></a>
## 举个例子：某视频网站

- 视频开始播放前，用户可能已经评论或点赞过了
- ML 模型需要导致播放事件的评价事件相关的信息
- 这种场景下的流式应用join需要事件时间和状态。

![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643594510899-66b6b54f-2716-4dc2-ab0f-66cb23b0cbb8.png#clientId=ue29bb0a5-9882-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=205&id=ue9bfa87f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=256&originWidth=663&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24583&status=done&style=none&taskId=ud94882f8-a5eb-4a0d-943f-1dfc2d1658f&title=&width=530.4)<br />上述视频网站例子是一个综合了事件流、事件时间、状态等的实际应用场景。该场景希望能join播放事件流和在此之前的评价，而不是之后的评价。因此需要将每部电影的评价存储起来，以userId和movieId为索引，方便播放事件发生时来查找这些评价。同时，需要注意在关联使用时需关注两个事件流中的时间戳的处理方式。
<a name="Asdbg"></a>
## 传统分层架构和本地状态（local state）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643595523736-a12bbe9c-46a8-4158-aa2b-d2cd779dde15.png#clientId=ue29bb0a5-9882-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=198&id=R2O6y&margin=%5Bobject%20Object%5D&name=image.png&originHeight=247&originWidth=226&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7692&status=done&style=none&taskId=ue1a6fff3-9fe8-44ff-b949-63a4a2ed2a8&title=&width=180.8)                     ![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643595538185-ef1413af-0149-497b-8824-9a890ccc3975.png#clientId=ue29bb0a5-9882-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=129&id=Gm63g&margin=%5Bobject%20Object%5D&name=image.png&originHeight=161&originWidth=275&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5929&status=done&style=none&taskId=u47e927b4-958c-4112-851f-9064beb1537&title=&width=220)<br />Flink 管理和扩展状态的方法与传统的应用系统中管理和扩展状态的差异：

- 可扩展性
   - DB可能成为瓶颈，需要随着应用做扩展
   - 计算和存储协同共存，并行扩展
- 性能
   - 跨层边界读写
   - 本地状态，加上大块的异步写入以实现持久性
- 操作简便
   - 部署新服务时需要考虑管理另一个数据库
   - 只需要额外的备份存储
- 一致性
   - 分布式事务，通常是低隔离性和一致性
   - 使用Flink的机制，每个key精准一次
<a name="REG0I"></a>
# Flink 的 API 和运行时架构
<a name="u184R"></a>
## Flink 1.9以前 API 
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643596661613-95cc3a4b-0bb4-4a55-8c39-01528b41bac2.png#clientId=ue29bb0a5-9882-4&crop=0&crop=0&crop=1&crop=0.8553&from=paste&height=256&id=u4144543c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=443&originWidth=996&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28289&status=done&style=none&taskId=ucb5112d8-e15f-4263-b2d8-17db16d45b1&title=&width=575)<br />在这个版本里，我们还能看到独立的流和批的API，即DataStream and DataSet APIs。在他们的上层是Table/SQL API。根据用户的使用需要，Table/SQL 库会将查询转换为 DataSet 或 DataStream 作业。
<a name="UMvwF"></a>
## Flink 2.0 API 
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643596639331-3c5a10f0-747f-4c06-ba8a-7ec479b66d1a.png#clientId=ue29bb0a5-9882-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=248&id=ud410cc52&margin=%5Bobject%20Object%5D&name=image.png&originHeight=441&originWidth=1032&originalType=binary&ratio=1&rotation=0&showTitle=false&size=35830&status=done&style=none&taskId=u820f9bff-29aa-4370-b2b6-614f5b230b5&title=&width=581)<br />在 Flink 1.9 中，我们开始向这种新分层过渡。<br />​

从用户的角度来看，DataStream 和 Table/SQL API 基本上与以前一样。并且它们都继续在系统中发挥相同的作用，即描述和生成数据流图。然而，关系 API 已经使用更强大、更低级别的抽象进行了重建，这使得实现更多优化成为可能。请注意，DataSet API 不再出现在此图中。最终，DataSet API 提供的所有操作和优化都将可用于使用 DataStream API 的有界流，并且 Flink 将不再需要单独的 API 来进行批处理。<br />​

在 Flink 1.11 中，两个 Table/SQL 运行时仍然可用，但此处描述的版本（“blink”规划器）现在是默认版本。一旦对有界流的支持完全与 DataSet API 一样强大，关系 API 的旧运行时将被删除。<br />​

至于 stateful functions API，这个 API  95% 左右的实现使用了 public DataStream API；它仅有限地使用内部操作。<br />​

如需深入了解 Flink 2.0 中批处理和流式处理的统一，请参阅 Aljoscha Krettek 在 Flink Forward Europe 2019 上的[Towards Flink 2.0: Unified Batch&Stream Processing](https://www.youtube.com/watch?v=WLlkQApBz4Y)
<a name="CZCWE"></a>
## DataStream API and execution
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643600237107-e6d7dd60-5c29-459d-ad22-908753c5a202.png#clientId=ue29bb0a5-9882-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=329&id=u2caabb1f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=411&originWidth=556&originalType=binary&ratio=1&rotation=0&showTitle=false&size=31607&status=done&style=none&taskId=u36455269-d356-4d24-9227-d62947d916d&title=&width=444.8)<br />要点：当使用 DataStream API 时，编写的代码描述了一个流式数据流作业图dataflow job graph。当应用程序的 main() 方法运行时，它会构建 job graph，然后由 Flink 集群执行。上图说明了代码的不同部分如何描述处理Data Pipeline的不同阶段。
<a name="NsNmk"></a>
## Apache Flink’s Relational APIs
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643600604717-c35c2171-f6b8-4315-b0a3-ce217cee14f2.png#clientId=ue29bb0a5-9882-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=142&id=u1806337c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=189&originWidth=904&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24346&status=done&style=none&taskId=u8bf2b203-14e8-41bb-a943-582336ad1e0&title=&width=677.2000122070312)

- 对批和流而言统一的API
- 对于一个查询而言，无论输入静态的批数据还是流数据，都产生一样的结果。
<a name="IbQP5"></a>
## Flink’s Stateful Functions API
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2494971/1643600911330-4b722048-fcbe-4d0d-ba23-acf8dc514579.png#clientId=ue29bb0a5-9882-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=188&id=u4b55b49a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=235&originWidth=496&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17754&status=done&style=none&taskId=ua8da7ced-3cbe-4732-b6d9-4235a6a7e33&title=&width=396.8)<br />状态API使得构建分布式有状态应用变得容易很多。

- 云原生
   - 专为无服务器架构设计的运行时/内核
- 构建块：远程函数
   - 表示实体的小块逻辑
- 多语言支持
   - 可以用任何处理 HTTP 请求的语言来实现
- 动态消息
   - 函数间随意通信
- 一致性状态，不需要数据库
   - 每个函数有持久的本地状态
   - 全局一致，精准一次保证
<a name="nJqTn"></a>
# 总结
Flink是一个流处理器：

- 容错的、精准一次的状态算子
- 实时和历史数据的基于事件时间的处理
- 高可扩展性和能快速处理的内核
- 各层次的API（表达能力与易用性）
- 丰富的连接器生态
- 灵活的部署模式
- 易于管理的有状态应用程序升级
- 高效运行批处理作业的能力
