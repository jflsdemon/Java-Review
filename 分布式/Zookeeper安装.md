- zookeeper 位置/usr/local/zppkeeper-3.4.10

### 服务端
- bin目录下常用的脚本解释

    - zkCleanup　　清理Zookeeper历史数据，包括食物日志文件和快照数据文件

    - zkCli　　　　  Zookeeper的一个简易客户端

    - zkEnv　　　　设置Zookeeper的环境变量

    - zkServer　　   Zookeeper服务器的启动、停止、和重启脚本

- 运行服务

    - 进入bin目录，使用zkServer.sh start启动服务

　　

    - 使用jps命令查看，存在QuorumPeerMain进程，表示Zookeeper已经启动

　　

- 停止服务

    - 在bin目录下，使用zkServer.sh stop停止服务

　　

    - 使用jps命令查看，QuorumPeerMain进程已不存在，表示Zookeeper已经关闭

　　