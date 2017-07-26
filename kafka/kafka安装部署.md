[TOC]
## kafka安装配置
### 单机模式

### 伪分布模式
所谓伪分布式即在同一个服务器上启动多个kafka实例。需要先安装zookeeper。
copy两个副本
```
cp config/server.properties  config/server-1.properties
cp config/server.properties  config/server-2.properties
```
主要修改配置
server.properties
```
broker.id=0
port=9092
log.dir=/tmp/kafka-logs-0
host.name=cent
zookeeper.connect=cent:2181
```
server-1.properties
```
broker.id=1
port=9093
log.dir=/tmp/kafka-logs-1
host.name=cent
zookeeper.connect=cent:2181
```
server-2.properties
```
broker.id=2
port=9094
log.dir=/tmp/kafka-logs-2
host.name=cent
zookeeper.connect=cent:2181
```

先启动zookeeper
```
$zookeeper_home/bin/zkServer.sh start
```
然后启动三个broker
```
bin/kafka-server-start.sh config/server.properties &
bin/kafka-server-start.sh config/server-1.properties &
bin/kafka-server-start.sh config/server-2.properties &
```

### 常用命令
1. 创建一个拥有3个副本1个分区名叫test的topic
```
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 3 --partitions 1 --topic test
```

2. 查看主题信息
```
bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic test
```

3. 生产消息
```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
```
在停留的shell中随便发送，按`Enter`发送

4. 消费消息
```
bin/kafka-console-consumer.sh –zookeeper localhost:2181 –from-beginning –topic test
```
可以在shell中看到发送的消息
