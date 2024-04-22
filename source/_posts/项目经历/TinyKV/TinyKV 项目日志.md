
# Project 1
# Project 2
## Project 2 整体架构

## Project 2A
### Project 2A 整体架构
本部分实现基础的 Raft 算法，且不需要考虑快照操作。我们共需要实现三个模块，分别为 RawNode、Raft 和 RaftLog，分别对应文件 `rawnode.go`、`raft.go` 和 `log.go` ，这三个模块，共同构成一层，我将其称为 `raft 层`。结构图如下
![[image2.png]]
- **RawNode**：该模块用来接收上层传来的信息，将信息下传给 Raft 模块。比如，上层传递一个 Msg 给 RawNode，这个 Msg 可能是 心跳请求、日志提交请求、日志追加请求等等。然后 RawNode 收到这个 Msg 之后，将其交给 Raft 模块去处理。比如，上层交给 RawNode 一个日志提交请求，然后 RawNode 将其交给 Raft ，如果该节点是领导人，那么其就会追加这条日志，然后发送给其他节点进行同步。另外，RawNode 不仅用来接受请求然后传递给 Raft，还要用来收到 Raft 的同步结果然后传递给上层。RawNode 要负责检查 Raft 模块是否已有同步好的日志需要应用、是否有 Msg 需要发送、是否有日志需要持久化等等，然后把这些信息（Ready）交给上层，上层会据此进行处理。总的来说，RawNode 是 raft 层暴露给上层的一个模块，用来进行信息的传递。
- **Raft**：整个 raft 层最关键的就是它，它就是实现 Raft 算法的核心模块。其中，领导人选举、日志追加、角色转换等等均是在这里实现。Raft 模块会接受 RawNode 发来的信息，然后在进群中进行相关的同步操作，同步结果不需要主动返回给 RawNode，因为 RawNode 自己会进行检查。
- **RaftLog**：该模块用来暂存节点的日志条目，同时还要维护一些日志指针，如 committed、applied 等等。

总结一下，RawNode 是 raft 层中暴露在外面的模块，用于该层与上层的信息交互，Raft 模块是 raft 层中的核心模块，算法的核心逻辑均在该模块实现，RaftLog 用来暂存日志信息，并维护相关指针。
### Project 2 AA｜Project 2 AB
#### 消息类型以及基本逻辑
1. case pb.MessageType_MsgHup:
	1. 选举的本地消息，由 Tick() 触发。
	2. 如果发生选举超时，节点应将“MessageType_MsgHup”传递给其 Step 方法并开始新的选举，向其他所有节点发送 MsgRequestVote；
	3. 一旦收到了这个消息，说明就是要重新开始一次选举。
	
2. case pb.MessageType_MsgBeat:
	1. 本地心跳信息。表示当前节点的时钟前进，判断心跳超时和选举超时并进行相应的处理。
	2. 注意只有 Leader 需要处理这个消息。
	
3. case pb.MessageType_MsgPropose:
	1. 注意只有 Leader 会处理这个消息。 
	2. Leader 收到此消息需要将日志广播给其他节点。
		1. 发送时，如果要发送的 Index 已经被压缩了，转为发送快照。
		2. 否则发送 MsgAppend。
		3. 如果 MsgAppend 被接收者拒绝，Leader 会调整 next，重新进行前置判断，如果无需发快照，则按照新的 next 重新发送 MsgAppend。
	3. 发送方：由 Raft 层的上层构造此 msg 表示一次追加日志。此外，becomeLeader 时也会发送一个空 Entry 的 propose（也会被 广播）。
	
4. case pb.MessageType_MsgAppend:
	1. 发送方：如上，一般 propose 之后触发，由 Leader 发送给其他节点来同步日志条目。
	2. 接收方：
		1. 判断 Msg 的 Term 是否大于等于自己的 Term，是则更新，否则拒绝；
		2. 拒绝，如果 prevLogIndex > r.RaftLog.LastIndex() 说明中间有日志缺失。否则往下；
		3. 拒绝，如果接收者日志中没有包含这样一个条目：即该条目的 Term 在 prevLogIndex 上不能和 prevLogTerm 匹配上。否则往下；
		4. 追加新条目，同时删除冲突条目，冲突条目的判别方式和论文中的一致；
		5. 当前节点更新 committedIndex 时，要比较 Leader 已知已经提交的最高的日志条目的索引 m.Commit 或者是上一个新条目的索引，然后取两者的最小值。为
		6. 接受；
	
5. case pb.MessageType_MsgAppendResponse:
	1. 只有 Leader 会处理该 Msg，其余角色直接忽略；
	2. 如果被 reject 了，那么重置 next。重置规则为将旧的 next --，然后比较 m.Index + 1，最后取小的那个赋值给 next，然后重新进行日志 / 快照追加；
	3. 如果没有 reject，则更新 match 和 next。next 赋值为 m.Index + 1，match 赋值为 next - 1 ；
	4. 按照论文的思路更新 commit。假设存在 N 满足N > commitIndex，使得大多数的 matchIndex[i] ≥ N以及log[N].term == currentTerm 成立，则令 commitIndex = N。为了快速更新，这里先将节点按照 match 进行了递增排序，这样就可以快速确定 N 的位置。

6. case pb.MessageType_MsgRequestVote:
	1. 判断 Msg 的 Term 是否大于等于自己的 Term，是则更新，否则拒绝；
	2. 如果 votedFor 不为空或者不等于 candidateID，则说明该节点以及投过票了，直接拒绝。否则往下；
	3. Candidate 的日志至少和自己一样新，那么就给其投票，否者拒绝。新旧判断逻辑如下：
	    - 如果两份日志最后的条目的任期号不同，那么任期号大的日志更加新
	    - 如果两份日志最后的条目任期号相同，那么日志比较长的那个就更加新；
	Candidate 会通过 r.votes 记录下都有哪些节点同意哪些节点拒绝，当同意的票数过半时，即可成为 Leader，当拒绝的票数过半时，则转变为 Follower。
		
7. case pb.MessageType_MsgRequestVoteResponse:
	1. 只有 Candidate 会处理该 Msg，其余节点收到后直接忽略；
	2. 根据 m.Reject 更新 r.votes[m.From]，即记录投票结果；
	3. 算出同意的票数 agrNum 和拒绝的票数 denNum；
	4. 如果同意的票数过半，那么直接成为 Leader；
	5. 如果拒绝的票数过半，那么直接成为 Follower
8. case pb.MessageType_MsgSnapshot:
	1. Project2C 中才会实现，所以该 Msg 在 Project2C 处
	
9. case pb.MessageType_MsgHeartbeat:
	1. 发送：
		1. 每当 Leader 的 heartbeatTimeout 达到时，就会给其余所有节点发送 MsgHeartbeat；
	2. 接收与处理：
		1. 判断 Msg 的 Term 是否大于等于自己的 Term，是则更新，否则拒绝；
		2. 重置选举计时 r.electionElapsed
		3. 发送 MsgHeartbeatResponse
		4. Commit:  min(r.RaftLog.committed, r.Prs[to].Match), 确保了心跳消息中的 Commit 字段不会超过已经在目标节点上提交的日志索引，使得心跳消息不需要推进 Commit。
	
10. case pb.MessageType_MsgHeartbeatResponse:
	1. 发送：
		1. 当节点收到 MsgHeartbeat 时，会相应的回复 MsgHeartbeatResponse；
	2. 接收与处理：
		1. 只有 Leader 会处理 MsgHeartbeatResponse，其余角色直接忽略；
		2. 通过 m.Commit 判断节点是否落后了，如果是，则进行日志追加；
	
11. case pb.MessageType_MsgTransferLeader:
	1. 用于上层请求转移 Leader，Project3 使用。
			
12. case pb.MessageType_MsgTimeoutNow:
	1. 节点收到后清空 r.electionElapsed，并即刻发起选举

#### 消息使用的字段

| MessageType\msg_nr  | MsgType | To  | From | Term | LogTerm | Index | Entries | Commit | Snapshot | Reject |
| ------------------- | ------- | --- | ---- | ---- | ------- | ----- | ------- | ------ | -------- | ------ |
| Hup                 | √       |     |      |      |         |       |         |        |          |        |
| Beat                | √       |     |      |      |         |       |         |        |          |        |
| Propose             | √       | √   |      |      |         |       | √       |        |          |        |
| RequestVote         | √       | √   | √    | √    | √       | √     |         |        |          |        |
| requestVoteResponse | √       | √   | √    | √    | 。       | 。     |         |        |          | √      |
| Heartbeat           | √       | √   | √    | √    |         |       |         | √      |          |        |
| HeartbeatResponse   | √       | √   | √    | √    |         |       |         |        |          |        |
| Append              | √       | √   | √    | √    | √       | √     | √       | √      |          |        |
| AppendResponse      | √       | √   | √    | √    |         | √     |         |        |          | √      |
| Snapshot            | √       | √   | √    | √    |         |       |         | √      | √        |        |
| TransferLeader      | √       | √   | √    |      |         |       |         |        |          |        |
| TimeoutNow          | √       | √   | √    |      |         |       |         |        |          |        |
注意点：
1. Hup 和 Beat 一定是本地消息，而 Propose 不一定是本地消息，可能是外部构造的不再 Raft 内部调用 Step 处理，所以：From 不使用但是 To 需要使用。
2. TimeoutNow 也是 本地消息，即刻发起选举。
3. 对于 MessageType_MsgAppend：
	1. LogTerm 为要发送的条目的前一个条目的 Term，即论文中的 prevLogTerm
	2. Index 为要发送的条目的前一个条目的 Index，即论文中的 prevLogIndex
	3. 当前节点 (Leader) 的 committedIndex
4. 对于 MessageType_MsgAppendResponse：
	1. Index：r.RaftLog.LastIndex()；该字段用于 Leader 更快地去更新 next
5. Reject 两个功能，一个 投票一个 在 append
	不同的是，propose 的 entry 无论是不是一个空的，都会被当成一个新的 entry 进行添加，而 Appen 的 entry，则可能被拒绝或者截取部分。

#### 推进器 Step()
Step() 作为驱动器，用来接收上层发来的 Msg，然后根据不同的角色和不同的 MsgType 进行不同的处理。
#### 计时器 tick()
该函数起到计时器的作用，即逻辑时钟。每调用一次，就要增加节点的心跳计时（ r.electionElapsed），如果是 Leader，就要增加自己的选举计时（ r.heartbeatElapsed），然后，应按照角色进行对应的操作。

#### log.go

#### 关键点｜易错点
1. raft 的 id 是从 0 开始的，但是 0 是无效 id ：const RaftInvalidIndex uint64 = 0
2. entry 的 index 指的是 在 storage 中的 索引，而不是在数组中的下标。
	下标计算： index - entries\[0\].Index
3. 注意每一个 entry 都有一个自己的 **term**
4. Leader 不会自己发起一个新的选举。
5. 注意变成 Candidate 后发现，只有一个节点，再直接变成 Leader。
6. 注意某一个消息处理时如果遍历 Prs 且跳过了当前节点的 id ，需要额外考虑一下集群只有一个节点的情况。
7. becomeCandidate 和 发送 请求 投票要分开进行
8. allEntries() 描述中需要排除 排除虚拟条目，但是实际上，
	// if ent.Data != nil { // 如果 这里的 ent 的 Data 是空的,说明 entry 无效
	// }
	不需要排除，这个是在 测试中确定的。
9. handleRequestVoteResponse  除了统计同意的个数，还需要统计 拒绝的个数。
#### TODO
**随机选举时间问题**

- 如果 et 太小，会过早开始选举，导致 term 比测试预期大。如果太大，会很晚发生选举，导致 term 比测试预期小。而且如果按照etcd那样一直递增，最后时间会非常长，直接卡住。我最后把它限制在 10~20 之间，通过测试。

**leader 更新 committed 之后要告知 follower**

- eader 收到 appendResp 后，会相应的更新自己的 committed，但是更新之后一定要把更新结果告知`全部` follower，即另外发送一个 appendResp 。如果不这样的话，最后一轮完成后，集群的 committed 会不同步。并且为了防止死循环，leader 只能在 committed 发生变化的时候去通知 follower。

**测试要求 msg 是nil，而不是空切片**

- TestRawNodeRestartFromSnapshot2C 中，want 里的 msg 为 nil，即测试点预期 newRaft 处的 msg 应该为 nil，而不是 make 一个空切片。

### Project 2 AB
空空如也
### Project 2 AC
#### RawNode 部分整体工作流程
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
代码解释：
1. RawNode 是 Raft 的包装器，上层 ( peer 层) 会不停的调用 RawNode 的 tick() 函数，进一步触发 Raft 的 tick() 函数。 
2. 上层会定时从 RawNode 获取 Ready，首先上层通过 HasReady() 进行判断，如果有新的 Ready，上层会调用RawNode 的Ready()方法进行获取，RawNode 从 Raft 中 获取信息生成相应的 Ready 返回给上层应用，Raft 的信息则是存储在 RaftLog 之中。上层应用处理完 Ready 后，会调用 RawNode 的Advance() 方法进行推进，告诉 RawNode 之前的 Ready 已经被处理完成，然后你可以执行一些操作，比如修改 applied，stabled 等信息。 
3. 上层应用可以直接调用 RawNode 提供的 Propose(data []byte) ，Step(m pb.Message) 等方法，RawNode 会将这些请求统一包装成 Message，通过 Raft 提供的 Step(m pb.Message) 输入信息。

#### HasReady()
RawNode 通过 HasReady() 来判断 Raft 模块是否已经有同步完成并且需要上层处理的信息，包括：
- 是否有需要持久化的状态；
- 是否有需要持久化的条目；
- 是否有需要应用的快照；
- 是否有需要应用的条目；
- 是否有需要发送的 Msg

其中，最后一点的 Msg，就是 Raft 模块中节点间相互发送的 Msg。也就是说，节点间发送的 Msg 是通过 RawNode 先发送给上层，然后上层在将其发送给对应节点的。

如果 HasReady() 返回 true，那么上层就会调用 Ready() 来获取具体要做的事情，和上述 HasReady() 的判断一一对应。该方法直接调用 rn.newReady() 生成一个 Ready() 结构体然后返回即可。
#### Advance()
当上层处理完 Ready 后，调用 Advance() 来推进整个状态机。Advance() 的实现就按照 Ready 结构体一点点更改 RawNode 的状态即可，包括：

- prevHardSt 变更；
- stabled 指针变更；
- applied 指针变更；
- 清空 rn.Raft.msgs；
- 丢弃被压缩的暂存日志；
- 清空 pendingSnapshot；

#### 思考&总结
1. RawNode 是 Raft 的封装，保存 Raft 的一部分状态
### Project 2A  总结&思考
#### 一些思考
> 对于某个 Raft 变为 Candidate 提出选举，是当场给自己投一票 还是 同样构造一个 msg 发给自己处理？从 term 的角度思考。
	1. 假设发消息。可能导致当前 Raft 先收到其他 Raft 的投票请求，从而自己没有给自己投票。
	2. 因此当场给自己投一票。这样也减少了自己和自己的发消息多余操作。
	
> 如果当前的 Raft 就是自己本身，开始一个新的选举之后  term 要增加吗？
	增加。


> 如果超时，是先增加  term 还是先发送 MsgHup 消息？
	先增加 term。如果先发送消息，那么在消息队列中，这个消息前面的消息会认为这个 Raft 还是没有超时（实际上已经超时了）

> 先发心跳包等到响应再增加 Elapsed 还是 先增加？
	先增加。万一响应收不到了，Elapsed 还是要增加的。

> Message 中 Commit 字段的作用？
	表示提交索引（commit index）。在 Raft 一致性算法中，每个节点都会维护一个提交索引。提交索引表示在该索引之前的所有日志条目都已经被安全地复制到了大多数节点，并且可以被应用到状态机中。换句话说，提交索引是已经达成共识并可以执行的日志条目的最高索引。

> RaftLog 中 committed | applied | stabled 三个字段？
	committed：已知在多数节点上的稳定存储中的最高日志位置。
	applied：已经应用到状态机的最大 log 位置
	stabled： 已经被持久化存储的最大 log 位置。

> 持久化存储中到底要存储什么？
	1. "Save log entries to stable storage"（将日志条目保存到稳定存储）：在 Raft 中，每个节点会维护一个日志（log），其中包含按顺序记录的操作或状态变化。当节点接收到来自客户端的命令或其他节点的日志复制请求时，它将这些日志条目追加到自己的日志中。为了确保数据的持久性和可靠性，节点需要将这些日志条目保存到稳定存储介质（如磁盘）上，以便在节点重启或发生故障时能够恢复日志的状态。
	2. "Save hard state like the term, commit index, and vote to stable storage"（将任期、提交索引和投票信息等硬状态保存到稳定存储）：除了日志条目之外，Raft 还维护了一些重要的状态信息，称为硬状态。这些硬状态包括当前任期（term）、已提交的索引（commit index）和投票信息（如上一次投票的候选人ID等）。这些硬状态的变化需要被持久化保存，以确保在节点重启或发生故障时能够恢复到正确的状态。

> MemoryStorage  中 ents\[i\] has raft log position i+snapshot.Metadata.Index 的含义？
	`ents[i]` 表示 Raft 日志中的第 `i` 个条目。该条目的 Raft 日志位置是通过 `i + snapshot.Metadata.Index` 来确定的。
	在 Raft 算法中，为了支持快照（snapshot）功能，可以将当前节点的状态和日志压缩成一个快照。快照包含了快照的元数据信息以及存储在快照中的状态和日志条目。
	在代码中的 `ents` 切片中，存储了一系列的日志条目。为了与快照中的日志条目对应起来，`ents[i]` 的 Raft 日志位置是通过 `i + snapshot.Metadata.Index` 计算得出的。
	具体来说，`snapshot.Metadata.Index` 表示快照元数据中的索引值，它指示了快照中第一个条目的索引。然后，对于 `ents` 切片中的第 `i` 个条目，其在 Raft 日志中的位置就是 `i + snapshot.Metadata.Index`。
	这样的计算方式可以确保在应用快照之后，`ents` 切片中的日志条目仍然与 Raft 日志中的对应位置保持一致。
	需要注意的是，`snapshot.Metadata.Index` 是快照元数据中的一个字段，用于表示快照中第一个条目的索引。在给出的代码中，`snapshot` 是一个 `Metadata` 类型的字段，可能是一个结构体或变量，用于存储快照的元数据信息。
	也就是说  0 号位置的 Index 就是 `snapshot.Metadata.Index`

> RaftLog 中的 entries  和 Storage 中的 ents 有什么区别？
	1. entries 是 从下标 1 开始作为有效记录；ents 是从下表 0 开始作为有效记录。
	2. .ents\[0\].Index 始终是有效的，用来作为基础索引

> RaftLog.storage.Snapshot() 和 RaftLog.pendingSnapshot 中的区别
	一个永久快照，一个临时快照。


> 心跳 heatbeat 时间和选举 election 时间的关系
	1. heatbeat 只有 Leader 发送；election 时间是非 leader  需要维护的信息，以便即使开始一个选举。

> 收到陌生 Raft 的信息，该怎么处理？
	1. 可能是一个新的节点，需要添加到已有结构中。
	2. 什么情况下添加？
		1. 首先只有这个新的 Raft 发送消息时才能被旧 Raft 知道，但是陌生 Raft 的工作消息直接处理可能会出问题，所以应该使用 heatbeat 作为新 Raft 的识别消息（谁收到了这个消息谁添加节点）。那么：
		2. 选举过程：
			1. Candidate 收到 heatbeat：多一个投票人，不会导致选举失败 。
			2. 其他角色 收到 heatbeat：更不会影响选举。
		3. 工作过程：
			1. Leader 收到 heatbeat：多一个工作者，不会导致工作错误 。
			2. 其他角色 收到 heatbeat：更不会影响工作（因为不会给 这个 Raft 发送工作消息）。
	3. 总结：任何情况下，收到 新 Raft 的 heatbeat 时，可将其添加到当前 Raft 的记录中（最好同时向这个新 Raft 同步一下当前系统的情况）。



sendAppendResponse 不需要 回复 ----嘛嘛嘛？？？？？？？


#### 总结一下 commmit|applied|stable 的修改时机

//	snapshot/first.....applied....committed....stabled.....last
//	--------|------------------------------------------------|
//	                          log entries
#### RaftLog.committed 修改时机总结
1. 在  newRaft() 中调用 newLog() 时被初始化为 storage 中 hardState 的 Commit
2. Leader 在 发送 MessageType_MsgAppend 时发送当前 commited
3. Raft 在收到 MessageType_MsgAppend 并更新 RaftLog.entries 之后，将 commited 置为以下两者的最小值
	1. Leader 的 committed： m.Commit
	2. Leader 以为的 此 Raft 的 Committed：m.Index+uint64(len(m.Entries))
4. Leader 在收到 AppendEntriesResponse 之后 将自己的 commited 置为 当前 Term 的Entry 被大多数 Peers 中 都 Match 的最小 Index。
5. Leader 在 发送 MessageType_MsgHeartbeat 时发送 min(r.RaftLog.committed, r.Prs[to].Match) （确保心跳消息中的 Commit 字段不会超过已经和目标j节点同步的日志索引，从而使得心跳消息不会推进 committed ）
6. Raft 收到 Lead 的  MessageType_MsgHeartbeat 之后，如果发现 m.Commit（Leader 认为的提交） >= r.RaftLog.committed （Raft 自认为的提交），说明当前 Raft 的 RaftLog.entries 中有一部分 Leader 认为还未提交（需要被覆盖）。修改 committed 值为
	min(m.Commit, r.RaftLog.LastIndex())
#### RaftLog.applied 修改时机总结
1. 在  newRaft() 中调用 newLog() 时被初始化为 storage 中 的 ent[0].Index 或者 初始化为 0，表示初始没有被 applied 的 entry。
2. 在 Advance() 中第一次修改。

#### RaftLog.stabled 修改时机总结
1. 在  newRaft() 中调用 newLog() 时被初始化为 storage 中 ent 最后一个 entry 的 Index。
2. r.RaftLog.stabled = m.Index
3. rn.Raft.RaftLog.stabled = rd.Entries[len(rd.Entries)-1].Index




#### r.Prs 的 Match & Next
1. 收到 Propose 时 if lastIndex == 0 Propose


## Project 2B
### Project 2B 整体流程
Project2B 实现了 rawNode 之上的上层应用，即真正开始多线程集群操作，引入了 peer 和 region 的概念。同时，除了实现上层的调用，Project2B 还需要通过调用 RaftStorage 中的接口真正实现写落盘。 store、peer、region 三者的关系如下：
![[Pasted image 20240420190155.png]]
1. Store：每一个节点叫做一个 store，也就是一个节点上面只有一个 Store。代码里面叫 RaftStore，后面统一使用 RaftStore 代称。 
2. Peer：一个 RaftStore 里面会包含多个 peer，一个 RaftStore 里面的所有 peer 公用同一个底层存储，也就是多个 peer 公用同一个 badger 实例。 
3. Region：一个 Region 叫做一个 Raft group，即同属一个 raft 集群，一个 region 包含多个 peer，这些 peer 散落在不同的 RaftStore 上。


这里将 Rawnode-Raft-RaftLog 统称为 raft 层，把要实现的部分称为 peer 层。peer 层首先接收来自 client 的 RaftCmdRequest，其中包含着不同的`命令请求`，接着它会把这些请求逐一以 entry 的形式传递给 raft 层，当然，这个 peer 应该是 Leader，不然 client 会找下一个 peer 继续试。raft 层收到条目后，会在集群内部进行同步，这就是 project2a 的内容。同步的过程中，peer 层会不时询问 raft 层有哪些已经同步好的 entry 可以拿来应用（执行）？哪些 entry 需要持久化？有没有快照需要应用？等等。三层的交互如下图所示：
![[Pasted image 20240421125735.png]]
此模块要完善两个文件，分别为 `peer_msg_handler.go` 和 `peer_storage.go`



https://github.com/sakura-ysy/TinyKV-2022-doc/blob/main/doc/project2.md?spm=a2c6h.13046898.publish-article.80.37936ffauECnFr&file=project2.md



### TODO
type peer struct 中为什么// Instance of the Raft module
    是 RaftGroup \*raft.RawNode  
一个 region 包含多个 peer 怎么理解







## Project 2C
Project 2C 整体流程



### RawNode 中 hardState  和 

# Project 3
https://www.inlighting.org/archives/tinykv-project3


# 技巧
## 日志输出
```go
log.Infof("Raft init with config={len(peers)=%d}, "+
        "peerid=%d, log commitid=%d, applied=%d, stabled=%d",
        len(c.peers),
        rsp.id, rsp.RaftLog.committed, rsp.RaftLog.applied, rsp.RaftLog.stabled)
        
log.Fatal(err)

log.Infof("peerid = %d becomes leader", r.id)
```
# 总结



## TODO

// TODO 这里的 stabled 在 newlog 的时候赋值过一次，应该不会是 0 ，为什么会判断一次stabled
    if l.stabled == 0 {
        return l.entries
        // log.Infof("stabled = %d", l.stabled)
    }
 
// TODO 什么时候 修改 committed 
> sendAppend 时没有修改
> handle AppendEntriesRespinse 时修改了。
https://github.com/LX-676655103/Tinykv-2021/blob/course/raft/log.go


// TODO 怎么处理 新 leader 中未 commit 的部分
？从当前的逻辑来看，只有 当前 term 也产生 log 时 才会顺便更新前面的log


// TODO 第一次收到 Lead 的消息时，需要进怎么样的处理

// TODO noop Entry 会发送吗，

// TODO r.Prs[r.id].Match 是  handlePropose 时修改。


TODO  搞清楚  Next 和 Match 之间的关系。
TODO 快照的同步逻辑
# 参考

https://github.com/LX-676655103/Tinykv-2021/blob/course/raft/raft.go#L967
https://github.com/sakura-ysy/TinyKV-2022-doc/


https://chenyunong.com/2021/08/04/TinyKV-Project2/
https://www.inlighting.org/archives/tinykv-project2