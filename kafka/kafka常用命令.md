[TOC]
### kafak常用命令

#### Create a topic
```shell
kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
```

#### List topic
```
kafka-topics.sh --list --zookeeper localhost:2181
```

#### 查看topic消费进度
这个会显示出consumer group的offset情况， 必须参数为--group， 不指定--topic，默认为所有topic

```
bin/kafka-run-class.sh kafka.tools.ConsumerOffsetChecker
```

#### 查看topic某分区偏移量最大（小）值
```
bin/kafka-run-class.sh kafka.tools.GetOffsetShell --topic topic-name --time -1 --broker-list localhost:9092 --partitions 0
```
注： time为-1时表示最大值，time为-2时表示最小值


#### zkCli.sh工具
可以方便查看每一个partition的offset。
```
bin/zkCli.sh -server localhost:2181
```
使用`ls /`可以看到一个列表，内部就是以tree的方式显示具体内容。
