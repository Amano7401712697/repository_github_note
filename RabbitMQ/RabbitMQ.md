# RabbitMQ 笔记

## 消息队列的简单介绍

​	“消息队列”是在消息的传输过程中保存消息的容器。消息被发送到队列中。“消息队列”是在消息的传输过程中保存消息的容器。消息队列管理器在将消息从它的源中继到它的目标时充当中间人。队列的主要目的是提供路由并保证消息的传递；如果发送消息时接收者不可用，消息队列会保留消息，直到可以成功地传递它。

## RabbitMQ 简介

### AMQP 介绍

​	AMQP(Advanced Message Queuing Protocol)，高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。消息中间件主要用于组件之间的解耦，消息的发送者无需知道消息使用者的存在，反之亦然。 AMQP的主要特征是面向消息、队列、路由（包括点对点和发布/订阅）、可靠性、安全。

### RabbitMQ 介绍

​	 RabbitMQ是一个开源的AMQP实现，服务器端用Erlang语言编写，支持多种客户端，如：Python、Ruby、.NET、Java、JMS、C、PHP、ActionScript、XMPP、STOMP等，支持AJAX。用于在分布式系统中存储转发消息，在易用性、扩展性、高可用性等方面表现不俗。 

## RabbitMQ 相关概念

​	**ConnectionFactory**、**Connection**、**Channel**都是RabbitMQ对外提供的API中最基本的对象。Connection是RabbitMQ的socket链接，它封装了socket协议相关部分逻辑。ConnectionFactory为Connection的制造工厂。 Channel是我们与RabbitMQ打交道的最重要的一个接口，我们大部分的业务操作是在Channel这个接口中完成的，包括定义Queue、定义Exchange、绑定Queue与Exchange、发布消息等。

### Queue

​	Queue（队列）是RabbitMQ的内部对象，用于存储消息。表示如下图:

![img](https://cdn.www.sojson.com/file/doc/8071183810)

​	RabbitMQ中的消息都只能存储在Queue中，生产者（下图中的P）生产消息并最终投递到Queue中，消费者（下图中的C）可以从Queue中获取消息并消费。

![img](https://cdn.www.sojson.com/file/doc/1972176598)

​	多个消费者可以订阅同一个Queue，这时Queue中的消息会被平均分摊给多个消费者进行处理，而不是每个消费者都收到所有的消息并处理。 

![img](https://cdn.www.sojson.com/file/doc/9323193930)

## Rabbit 环境搭建

1.  依赖安装

```shell
#erlang安装
wget https://www.rabbitmq.com/releases/erlang/erlang-19.0.4-1.el6.x86_64.rpm
yum install erlang-19.0.4-1.el6.x86_64.rpm
#epel安装
yum -y install epel-release
#socat安装
yum -y install socat
```

2.  软件包安装

```
wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.7.14/rabbitmq-server-3.7.14-1.el6.noarch.rpm
yum install rabbitmq-server-3.7.14-1.el6.noarch.rpm 
```

3.  启动和访问

```shell

```



