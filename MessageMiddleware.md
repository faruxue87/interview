# rabbitMQ vs kafka

## 1.起源
RabbitMQ使用ErLang编写，是一种传统的消息中间件实现了多种消息协议。RabbitMQ最初设计是为了实现Advanced Message Queuing Protocol。随
着AMQP的到来，跨语言开源消息中间件成为了现实。

Kafka使用Scala编写，源于LinkedIn，用于连接内部不同的内部系统。LinkedIn的系统架构更加的分布式，对数据集成，流式计算能力要求更高，摆脱过
去单一的方式解决这些问题。Kafka被Apache生态下许多项目所采用，尤其在事件驱动架构。

## 2.架构设计
RabbitMQ设计为一般用途的消息中间件，实现了多种模式包括：点对点，请求响应，发布订阅的模式。设计初衷，是使得broker变得聪明，而consumer端
尽可能的傻瓜，多语言客户端支持，多功能插件支持。RabbitMQ通信可以是同步或者异步的。Publishers将消息推送至exchange，Consumers从Queue当
中获取消息。将Queue和Publisher进行解耦，减轻了Publisher选择routing的负担。RabbitMQ也支持分布式部署，联合集群部署且不需要外部服务支持。


# jms规范
## 消息模型
* 点对点(Queue)
    * 1对1
    * 无时间耦合(接收者不需要立即接收)
    * 接收者需要响应应答
* 发布订阅(Topic)
    * 1对N
    * 时间耦合性，先订阅，后接受到消息
    * 消息无需被每个订阅者确认消息
## 调用API
* ConnectionFactory
* Connection
* MessageProducer Send/MessageConsumer Receive
* Destination

# ActiveMQ持久化方式
* Database JDBC
* AMQ日志
* KahaDB(默认)AMQ基础上的改进
* LevelDB

# ActiveMQ集群高可用方式
## PureMasterSlave
同时运行两个MQ，一个Master，一个slave，Master挂了需要手动干预，很少用

## SharedFileSystem
共享文件系统(日志)，使用文件锁，获取文件锁的node成为master

## Database
类似共享文件系统，对表进行锁争夺

## Zookeeper
选举master

# 消息可靠性
* 消息重复
* 消息丢失

