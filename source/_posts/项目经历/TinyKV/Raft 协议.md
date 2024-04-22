
# Raft 协议
参考：[Raft-4：Raft 协议的基础工作流程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1nu4y167WM?p=5&spm_id_from=pageDriver&vd_source=59461060c1867e9bf731e467ae6f00b5)
知识点：Paxos
核心原理：在多个副本之间进行投票和决策，以确保数据的一致性。
两个阶段：
1. prepare: 提案者提出一个提议，发送给其他所有参与者，每个参与者都参与投票。如果大多数参与者同意这个协议，则进入 accept 阶段。
2. accept：提案者再次向所有参与者发送一个包含提议编号和提议内容的消息，如果大多数参与者再次同意这个提议，则该提议被视为有效并被接受。
关键特性：
	去中心化设计：没有单一的领导者节点负责做出最终决策，相反，任何节点都可以成为提案者或者接收者。
Raft：
基本流程：
1. 多个 server 共同选举产生一个 Leader，负责响应客户端的请求。
2. Leader 通过一致性协议，将客户端的指令发送到所有节点上。
3. 每个节点将客户端的指令以 Entry的形式保存到自己的 Log 中，此时 Entry 状态为 uncommited 。
4. 当有多数节点共同保存了 Entry 后，就可以执行 Entry 中的客户端指令，提交到 State 状态机中，此时 Entry 更新为 commited 状态。
![[Pasted image 20240320211915.png]]
理解  Raft 中的 Term：
1. Raft 将时间线分割为多个 term （任期）；
2. 一个任期有一个 id ，分两个阶段：
	1. 选举 election：选举出一个 leader。如果选举失败，此 term（任期）快速结束。
	2. 操作：进行执行任务。
![[Pasted image 20240320211822.png]]


理解 Raft 中的 状态机制
Raft 协议会为每一个服务器记录一个状态，状态有三种：
	1. Follower：负责同步 Leader 的操作日志。
	2. Candidate
	3. Leader：
1. 所有节点都是从 Follower 开始，Follower 负责同步 Leader 的操作日志。
2. Leader 会给 Follower 发送心跳。
3. Follower 收到心跳超时（随机值），转入 Candidate 状态，发起选举，如果成功，则变成 Leader；
4. 新的 Leader 会向所有其他节点发送心跳。
5. 此时其他节点无论是什么状态，都会变成 Follower
![[Pasted image 20240320211748.png]]

Raft 协议会为每一个服务器记录两个超时：
1. 选举超时。
2. 

Raft 和 Paxos 类似，但是更容易理解，也更容易实现。
Raft 主要是用来竞选主节点。
单个 Candidate 的竞选
有三种节点：Follower、Candidate 和 Leader。Leader 会周期性的发送心跳包给 Follower。每个 Follower 都设置了一个随机的竞选超时时间，一般为 150ms~300ms，如果在这个时间内没有收到 Leader 的心跳包，就会变成 Candidate，进入竞选阶段。

- 下图表示一个分布式系统的最初阶段，此时只有 Follower，没有 Leader。Follower A 等待一个随机的竞选超时时间之后，没收到 Leader 发来的心跳包，因此进入竞选阶段。
  

- 此时 A 发送投票请求给其它所有节点。

- 其它节点会对请求进行回复，如果超过一半的节点回复了，那么该 Candidate 就会变成 Leader。

- 之后 Leader 会周期性地发送心跳包给 Follower，Follower 接收到心跳包，会重新开始计时。

## 多个 Candidate 竞选

- 如果有多个 Follower 成为 Candidate，并且所获得票数相同，那么就需要重新开始投票，例如下图中 Candidate B 和 Candidate D 都获得两票，因此需要重新开始投票。


- 当重新开始投票时，由于每个节点设置的随机竞选超时时间不同，因此能下一次再次出现多个 Candidate 并获得同样票数的概率很低。

## 日志复制

- 来自客户端的修改都会被传入 Leader。注意该修改还未被提交，只是写入日志中。



- Leader 会把修改复制到所有 Follower。

  

- Leader 会等待大多数的 Follower 也进行了修改，然后才将修改提交。

- 此时 Leader 会通知的所有 Follower 让它们也提交修改，此时所有节点的值达成一致
