1. 网络的端口服务测试  ok
3. 密码空间优化    ok
4. zip推荐  ok
5. 剩余时间---后续  ok
6. 下载相关离线包     ok
7. bug --- 密钥空间 ：因为后续计算密钥空间了！！，前面某些密钥空间是 0
8. bug---- .后缀 的 归档任务 详情无法打开。
9. bug --- 进度： ok
10. bug --- 文件删除问题：


TODO:
1. 运行 md5  破解。do ing  下载字典有点久
2. 整理包。 ---- 
3. 整理指令。————— TODO 

Factory::getTaskFactory()->set($task, Task::KEYSPACE, $keyspace);
      DServerLog::log(DServerLog::TRACE, "已保存 Keyspace", [$this->agent, $task]);
         $this->sendResponse(array(

        PResponseSendKeyspace::ACTION => PActions::SEND_KEYSPACE,

        PResponseSendKeyspace::RESPONSE => PValues::SUCCESS,

        PResponseSendKeyspace::KEYSPACE => PValues::OK

      )

    );
   $task->setKeyspace(Factory::getFileFactory()->get($fileId)->getLineCount());
Factory::getFileFactory()->get($fileId)->getLineCount(),


msgs = 
[
{MsgRequestVote 1 1 0 0 0 [] 0 <nil> false {} [] 0} {MsgRequestVote 1 1 0 0 0 [] 0 <nil> false {} [] 0} 
{MsgRequestVote 2 1 0 0 0 [] 0 <nil> false {} [] 0} {MsgRequestVote 2 1 0 0 0 [] 0 <nil> false {} [] 0} 
{MsgRequestVote 3 1 0 0 0 [] 0 <nil> false {} [] 0} {MsgRequestVote 3 1 0 0 0 [] 0 <nil> false {} [] 0}
],
want [
{MsgRequestVote 2 1 2 0 0 [] 0 <nil> false {} [] 0} {MsgRequestVote 3 1 2 0 0 [] 0 <nil> false {} [] 0}
]

