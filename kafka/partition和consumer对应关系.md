[TOC]
### kafka中partition和consumer对应关系
1个partition只能被同组的一个consumer消费，同组的consumer则起到均衡效果

#### consumer多于partition
同一个partition内的消息只能被同一个组中的一个consumer消费。当消费者数量多于partition的数量时，多余的消费者空闲。也就是说如果只有一个partition你在同一组启动多少个consumer都没用，partition的数量决定了此topic在同一组中被可被均衡的程度

#### 消费者少于和等于partition
其中的多个partition会对应一个消费者

#### 多个消费者组
启动多个组，则会使同一个消息被消费多次，不同组之间会重复消费。
