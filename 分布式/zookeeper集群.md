### 集群
- 在conf/zoo.cfg文件最下方添加
> server.1=192.168.100.109:2888:2999
server.2=192.168.100.109:3888:3999
server.3=192.168.100.109:4888:4999

- 代表开启三台zookeeper服务器，每台服务器的zookeeper配置文件中都是这么配，后面的第一个端口号是leader用来跟flower通信，第二个使用来选举。

- 在配置文件中的dataDir下，创建myid文件，文件内容只要写配置文件server.id号的id号，如1或2或3。

- 启动三个zookeeper服务器

- 用./bin/zkServer.sh status查看zokeeper的状
态

- 用kill -9 pid 杀死leader，发现其他服务器有一台变成了leader

- 当存活的服务器数小于n/2+1时，集群无法服务了

### 应用场景

- 集群配置一致性
- HA（high availability） 主备动态切换
- pub/sub
- naming service
- load balance
- 分布式锁
