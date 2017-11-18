### 背景：kafka集群从2.10-0.8.2.1升级到2.10-0.10.0.1后，发现原先使用的写日志到Kafka的API全部被标记成了deprecated的状态
- old API
> kafka_2.10-0.10.0.1.jar
kafka.javaapi.producer.Producer;
kafka.producer.KeyedMessage;
kafka.producer.ProducerConfig;

- new API, 用异步方式发送消息，性能更优
> kafka-clients-0.10.0.1.jar
org.apache.kafka.clients.producer.KafkaProducer;
org.apache.kafka.clients.producer.Producer;
org.apache.kafka.clients.producer.ProducerConfig;
org.apache.kafka.clients.producer.ProducerRecord;

- 若仍然想实现原来的同步发送消息，并监控每条消息是否发送成功，需要对每次调用send方法后返回的Future对象调用get方法。（get方法的调用会导致当前线程block，直到发送结果返回，不管是成功还是失败）
- 官方文档解释如下
> The send is asynchronous and this method will return immediately once the record has been stored in the buffer of records waiting to be sent. This allows sending many records in parallel without blocking to wait for the response after each one. Since the send call is asynchronous it returns a Future for the RecordMetadata that will be assigned to this record. Invoking get() on this future will block until the associated request completes and then return the metadata for the record or throw any exception that occurred while sending the record.

- 代码实现如下，1. 实现producer；2. 发送消息
 
```java
    public Producer<Integer, String> initKafkaProducer(String brokerList){
        Properties props = new Properties();
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, brokerList.substring(1));//格式：host1:port1,host2:port2,....
        props.put(ProducerConfig.BATCH_SIZE_CONFIG, 0);//a batch size of zero will disable batching entirely
        props.put(ProducerConfig.LINGER_MS_CONFIG, 0);//send message without delay
        props.put(ProducerConfig.ACKS_CONFIG, "1");//对应partition的leader写到本地后即返回成功。极端情况下，可能导致失败
        props.put("key.serializer", "org.apache.kafka.common.serialization.IntegerSerializer");
        props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        Producer<Integer, String> kafkaProducer = new KafkaProducer<Integer, String>(props);
        return kafkaProducer;
}``` 
```java
public void send2Kafka(Producer<Integer, String> kafkaProducer, String topic, List<String> lines) throws InterruptedException, ExecutionException{
    for(String line : lines) {
        ProducerRecord<Integer, String> message = new ProducerRecord<Integer, String>(topic, line);
        this.kafkaProducer.send(message).get();
    }
}
```


