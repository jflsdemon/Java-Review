1. 安装工具brew install kafka 会自动安装依赖zookeeper

2. 安装配置文件位置 
> /usr/local/etc/kafka|zookeeper

3. 启动 zookeeper

> cd /usr/local/Cellar/kafka/0.11.0.1

> ./bin/zookeeper-server-start -daemon /usr/local/etc/kafka/zookeeper.properties &

- 最后出现绑定成功
> [2017-03-15 14:11:38,769] INFO binding to port 0.0.0.0/0.0.0.0:2181 (org.apache.zookeeper.server.NIOServerCnxnFactory)

4. 启动kafka服务

> ./bin/kafka-server-start /usr/local/etc/kafka/server.properties &

- 最后出现Kafka启动成功
> [2017-03-15 14:13:37,260] INFO [Kafka Server 0], started (kafka.server.KafkaServer)

5. 创建topic
> ./bin/kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
- --replication-factor 副本数
- --partitions 分区数
6. 查询topic
> ./bin/kafka-topics --list --zookeeper localhost:2181

7. 生产消息
> ./bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test 
- 在一个命令行窗口输入消息

8. 消费消息
> ./bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning
- 或者用老版本的消费方式
> ./bin/kafka-console-consumer --zookeeper localhost:2181 --topic test --from-beginning
- 在另一个命令行窗口自动接收消息


10. 删除broker
> kill -9 processId
10. 删除topic
>  ./bin/kafka-topics --zookeeper localhost:2181 --delete --topic test
### 现在应该能够输入消息到生产者终端，看到他们出现在消费者终端。

## 集群
- 修改server.properties中
    - broker.id
    - listener中的host.name:port
    - zookeeper.connect


### 查看硬盘上的消息
> sh /usr/local/Cellar/kafka/0.11.0.1/bin/kafka-run-class kafka.tools.DumpLogSegments --files /usr/local/var/lib/kafka-logs/first-0/00000000000000000000.log --print-data-log

- 利用自带的jar包中的DumpLogSegments类，来运行二进制log文件

作者：冰_茶
链接：http://www.jianshu.com/p/cc540f29e779
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。