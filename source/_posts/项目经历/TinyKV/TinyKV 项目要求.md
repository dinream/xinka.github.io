# 参考文档
[Smith-Cruise/TinyKV-White-Paper: Tutorial for TinyKV project in Talent Plan. (github.com)](https://github.com/Smith-Cruise/TinyKV-White-Paper)
[源码解析](https://cn.pingcap.com/blog/the-design-and-implementation-of-multi-raft/#raftstore)
# Project2 RaftKV
## 背景知识
### Raft
[[Raft 协议]]
### Raft GC
Raft GC 是指在 Raft 一致性算法中用于清理日志条目的过程。在 Raft 中，每个节点都会维护一个日志，用于记录系统状态的变化。当日志变得庞大时，执行一致性操作的成本也会增加。为了限制日志的大小并提高性能，Raft 使用 Raft GC 机制来清理已经提交（committed）的日志条目。

Raft GC 的基本原则是，一旦某个日志条目已经被提交并应用到状态机，就可以安全地删除该条目之前的所有条目。删除旧的日志条目可以释放存储空间，并减少后续一致性操作的开销。Raft GC 通常通过一些策略（例如基于索引或时间）来判断哪些日志条目可以安全删除。
### Snapshot（快照）
Raft 中的快照是一种机制，用于在节点的状态机状态较新的情况下压缩和清理日志。当日志变得很大时，将整个日志传输给新加入的节点可能会很耗时和耗带宽。为了解决这个问题，Raft 使用快照机制来捕获节点的状态，并将其保存为一个快照文件。

快照包含了节点在某个特定时间点的状态机状态。当新节点加入集群时，它可以通过获取最新的快照来快速将自己带到当前状态，并只追加日志中从快照之后的新条目。这样可以大大减少传输和恢复的开销。

快照通常是通过在节点的状态机达到某个预定阈值时触发，或者通过节点之间的协商来创建。创建快照后，旧的日志条目可以被截断和删除，只保留最新的日志条目和快照文件。
## 任务目标
1. 实现基本的 raft 算法。
2. 在 Raft 上构建容错 KV 服务器。
3. 添加 Raft GC 和 snapshot 的支持。
### Part A
#### 代码
 1. 代码位于 raft/
 2. 使用逻辑时钟实现，选举和超时时间
 3. 粗略阅读一下 eraftpb.proto ， 消息接收和发送的定义
 4. 注意，这里与标准 Raft 协议不同，将 HeartBeat 和 AppendEntries 分成不同的消息。
 5. 大致步骤：
	 1. Leader election
	 2. Log replication
	 3. Raw node interface

#### 实现 Raft 算法
1. `Raft/Raft.go` 提供了 Raft 算法的核心，包括消息处理、驱动逻辑时钟等。
##### Leader 选举 
1. `raft.Raft.tick()` 可以将内部时钟前移一个
2. 发送消息时仅将消息推入`raft.Raft.msgs`，因为测试代码会从这个里面的消息然后使用 `raft.Raft.Step()` 处理回应消息
3. make project2aa 测试。
##### 日志复制
1. 先在发送方和接收方实现处理 “MsgAppend” 和 “MsgAppendResponse” 。
2. `raft/log.go` 中的 `raft.RaftLog` 是关键结构。
3. 需要通过`raft/Storage.go`中定义的`Storage `接口与上层应用程序交互，以获取日志条目和快照等持久化数据。
4. make project2ab 进行测试。

#### 实现 Rawnode 接口
 `raft/rawnode.go` 中的 `raft.RawNode` 是与上层应用交互的接口。
 1. 包含 `raft.Raft` ，封装了时钟相关： `RawNode.Tick()`and `RawNode.Step()`，日志 ：`RawNode.Propose()`
 2. 包含一个新的 结构 `Ready`，处理消息或推进逻辑时钟时，`raft.Raft` 可能需要与上层应用程序交互，例如：
	- 向其他对等方发送消息
	- 将日志项保存到稳定的存储中
	- 保存硬状态（如术语），提交索引，并投票到稳定存储
	- 将提交的日志条目应用于状态机
	- 等
	交互不会立即发生，相反，它们被封装在 “Ready” 中，并由 “RawNode” 返回。`Ready（）`到上层应用程序。这取决于上层应用程序何时调用 `Ready（）`并进行处理。在处理完返回的 Ready 之后，上层应用程序还需要调用一些函数，如 `RawNode.Advance（）` 更新 `raft.Raft` 的内部状态，如应用索引、稳定日志索引等。

3. “makeproject2ac” 来测试实现

也可以运行 “make project2a” 来测试整个A部分。

> 提示。
>
> - 需要时添加任何状态到 `raft.Raft`, `raft.RaftLog`, `raft.RawNode` 和消息，在`eraftpb.proto` 中
> - 测试中假设初始的 term 0
> - 测试中假设新选举的 Leader 应该在任期内添加一个 noop 条目。 
> - 测试中假设一旦领导者推进其提交索引，它将通过 “MessageType_MsgAppend” 消息广播提交索引。
> - 测试中不会为本地 Message，`MessageType_MsgHup`, `MessageType_MsgBeat` 和 `MessageType_MsgPropose` 添加 term 号, 
> - 领导者和非领导者的日志条目附加有很大不同，有不同的来源，检查和处理，请小心。
> - 不要忘记，peers 之间的选举超时时间应该不同。
> - rawnode.go 中的一些包装函数可以通过 raft.Step(local message) 来实现
> - 当启动一个新的 raft 时，从`Storage`中获取最后的稳定状态来初始化`raft.Raft`和`raft.Raft Log` 

### Part B
使用 part A 中 实现的 Raft 模块构建一个容错键值存储服务。
1. 键/值服务将是一个复制状态机，由多个使用 Raft 进行复制的键/值服务器组成。
2. 只要大多数服务器处于活动状态并且可以通信，尽管存在其他故障或网络分区，您的键/值服务就应该继续处理客户端请求。
> 三个术语：“Store”、“Peer” 和 “Region”（定义在 “proto/proto/metapb.proto” 中）
> - Store 代表tinykv-server 的一个实例
> - Peer 代表在 Store 上运行的 Raft 节点
 >- Region是 Peers 的集合，也称为 Raft group (现在不需要考虑Region的范围。 项目3中将进一步引入多个区域。)

#### 代码
查看 `kv/storage/raft_storage/raft_server.go` 中的 `RaftStorage` ：实现了 `Storage` 接口（与 Project1 中的单机存储不同），首先将每个写入和读取请求发送到 Raft，然后在 Raft 提交请求后才对底层引擎进行实际的写入和读取。
`RaftStorage` 创建一个 `Raftstore` 来驱动 Raft。 当调用 `Reader` 或 `Write` 函数时，它实际上通过通道（ 通道是“raftWorker”的“raftCh”），并在 Raft 提交并应用命令后返回响应。 Reader 和 Write 函数的 kvrpc.Context 参数现在很有用，它从客户端的角度携带 Region 信息，并作为 RaftCmdRequest 的 header 传递。 这些信息可能不正确或过时，因此 raftstore 需要检查它们并决定是否提出请求。
接下来，就到了TinyKV的核心——raftstore。 结构有点复杂，阅读 TiKV 参考资料可以更好地理解设计：

- <https://pingcap.com/blog-cn/the-design-and-implementation-of-multi-raft/#raftstore>（中文版）
- <https://pingcap.com/blog/design-and-implementation-of-multi-raft/#raftstore>（英文版）

raftstore 的入口是 `Raftstore`，参见`kv/raftstore/raftstore.go`。 它启动一些工作线程异步处理特定任务，其中大多数现在不使用，因此您可以忽略它们。 你需要关注的是`raftWorker`。(kv/raftstore/raft_worker.go)

整个过程分为两部分：raftworker 轮询 “raftCh” 以获取消息，包括驱动 Raft 模块的基本 tick 和建议作为 Raft 条目的 Raft 命令； 它从 Raft 模块获取并处理就绪，包括发送 raft 消息、持久化状态、将提交的条目应用到状态机。 申请后，将响应返回给客户。

#### 实现对等存储 peer storage
对等存储是您通过 A 部分中的 “Storage”  接口进行交互的内容，但除了 raft 日志之外，对等存储还管理其他持久化元数据，这对于重启后恢复一致状态机非常重要
> `proto/proto/raft_serverpb.proto` 中定义了三个重要的状态：

- RaftLocalState：用于存储当前 Raft 的 HardState 和最后一个 Log Index。
- RaftApplyState：用于存储 Raft 最后应用的 Log 索引以及一些被截断的 Log 信息。
- RegionLocalState：用于存储 Region 信息以及该 Store 上对应的 Peer 状态。 Normal 表示该 Peer 正常，Tombstone 表示该 Peer 已从 Region 中移除，无法加入 Raft Group。
这些状态存储在两个 badger 实例中：raftdb 和 kvdb：
- raftdb 存储 raft 日志和 `RaftLocalState`
- kvdb 将键值数据存储在不同的列族 “RegionLocalState” 和 “RaftApplyState” 中。 可以把kvdb看成Raft论文中提到的状态机
> why?
> 实际上，可以只使用一个 badger 来存储 raft 日志和状态机数据。 分成两个实例只是为了与 TiKV 设计保持一致。

| Key               | KeyFormat                        | Value             | DB   |
| :---------------- | :------------------------------- | :---------------- | :--- |
| raft_log_key      | 0x01 0x02 region_id 0x01 log_idx | Entry             | raft |
| raft_state_key    | 0x01 0x02 region_id 0x02         | RaftLocalState    | raft |
| apply_state_key   | 0x01 0x02 region_id 0x03         | RaftApplyState    | kv   |
| region_state_key  | 0x01 0x03 region_id 0x01         | RegionLocalState  | kv   |
这些元数据应该在 “PeerStorage” 中创建和更新。 创建 PeerStorage 时，请参阅 “kv/raftstore/peer_storage.go”。 它会初始化该 Peer 的RaftLocalState、RaftApplyState，或者重启时从底层引擎获取之前的值。 注意，RAFT_INIT_LOG_TERM 和 RAFT_INIT_LOG_INDEX 的值都是 5（只要大于 1），而不是 0。之所以不设置为 0，是为了与 conf 更改后被动创建 Peer 的情况区别。 你现在可能还不太明白，所以请记住这一点，当你实现 conf 更改时，详细信息将在project3b 中描述。

这部分需要实现的代码只有一个函数：`PeerStorage.SaveReadyState`，该函数的作用是将 `raft.Ready` 中的数据保存到 badger 中，包括追加日志条目和保存 Raft 硬状态。

>硬状态：更新 `RaftLocalState.HardState` 并保存。

要追加日志条目，只需将 “raft.Ready.Entries” 中的所有日志条目保存到 raftdb 并删除任何以前追加的永远不会提交的日志条目。 另外，更新对等存储的 “RaftLocalState”并将其保存到 raftdb。
> 提示：
> - 使用 `WriteBatch` 立即保存这些状态。
> - 有关如何读取和写入这些状态的信息，请参阅 `peer_storage.go` 中的其他函数。
> - 设置环境变量 LOG_LEVEL=debug 这可以帮助您进行调试，另请参阅所有可用的[日志级别](../log/log.go)。

### 实施 Raft Ready 流程
在 Part A 部分，构建了一个基于 tick 的 Raft 模块。 现在您需要编写外部进程来驱动它。 大部分代码已经在 `kv/raftstore/peer_msg_handler.go` 和 `kv/raftstore/peer.go` 下实现。 所以你需要学习代码并完成 `proposeRaftCommand` 和 `HandleRaftReady` 的逻辑。 以下是对该框架的一些解释。

Raft “RawNode” 已使用 “PeerStorage” 创建并存储在 “peer” 中。 在 Raft Worker 中，您可以看到它采用 “peer” 并通过 “peerMsgHandler” 包装它。  `peerMsgHandler` 主要有两个功能：一是 `HandleMsg` ，另一个是 `HandleRaftReady`。

`HandleMsg` 处理从 raftCh 接收到的所有消息，包括 `MsgTypeTick` 调用 `RawNode.Tick()` 来驱动 Raft，`MsgTypeRaftCmd` 包装来自客户端的请求，以及 `MsgTypeRaftMessage` 是 Raft 对等点之间传输的消息 。 所有消息类型都在 `kv/raftstore/message/msg.go` 中定义。 具体大家可以查看一下，其中一些会在后面的部分用到。

消息处理完毕后，Raft 节点应该有一些状态更新。 因此 `HandleRaftReady` 应该从 Raft 模块中做好准备并执行相应的操作，例如持久化日志条目、应用提交的条目
并通过网络向其他对等点发送 raft 消息。

在伪代码中，raftstore 使用 Raft，如下所示：

``` go
for {
  select {
  case <-s.Ticker:
    Node.Tick()
  default:
    if Node.HasReady() {
      rd := Node.Ready()
      saveToStorage(rd.State, rd.Entries, rd.Snapshot)
      send(rd.Messages)
      for _, entry := range rd.CommittedEntries {
        process(entry)
      }
      s.Node.Advance(rd)
    }
}
```   

之后读取或写入的整个过程将是这样的：

- 客户端调用RPC RawGet/RawPut/RawDelete/RawScan
- RPC处理程序调用`RaftStorage`相关方法
- `RaftStorage` 向 raftstore 发送 Raft 命令请求，并等待响应
- `RaftStore` 将 Raft 命令请求作为 Raft 日志提出
- Raft模块追加日志，并通过`PeerStorage`持久化
- Raft模块提交日志
- Raft Worker在Raft准备就绪时执行Raft命令，并通过回调返回响应
- `RaftStorage` 接收回调的响应并返回到 RPC 处理程序
- RPC 处理程序执行一些操作并将 RPC 响应返回给客户端。

您应该运行“make project2b”来通过所有测试。 整个测试运行一个模拟集群，包括多个带有模拟网络的 TinyKV 实例。 它执行一些读写操作并检查返回值是否符合预期。

需要注意的是，错误处理是通过测试的重要组成部分。 您可能已经注意到“proto/proto/errorpb.proto”中定义了一些错误，并且错误是 gRPC 响应的一个字段。 另外，实现了 error 接口的相应错误定义在 kv/raftstore/util/error.go 中，因此您可以将它们用作函数的返回值。

这些错误主要与Region有关。 所以它也是`RaftCmdResponse`的`RaftResponseHeader`的成员。 当提出请求或应用命令时，可能会出现一些错误。 如果是这样，您应该返回带有错误的 raft 命令响应，然后错误将进一步传递给 gRPC 响应。 当返回带有错误的响应时，您可以使用 kv/raftstore/cmd_resp.go 中提供的 BindRespError 将这些错误转换为 errorpb.proto 中定义的错误。

在这个阶段，你可能会考虑这些错误，其他的将在 project3 中处理：

- ErrNotLeader：在跟随者上提议 raft 命令。 所以用它来让客户端尝试其他对等点。
- ErrStaleCommand：可能由于领导者更改，某些日志未提交并被新领导者的日志覆盖。 但客户并不知道这一点，仍在等待回复。 因此，您应该返回此信息以让客户端知道并再次重试该命令。


# TODO
> why
RAFT_INIT_LOG_TERM 和 RAFT_INIT_LOG_INDEX 的值都是 5（只要大于 1）

> Config.Applied uint64 干嘛用的