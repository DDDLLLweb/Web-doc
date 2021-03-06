-- 消息中间件简介
    消息中间件是在传输过程中保存消息的容器。消息中间件再将消息从它的源中继到它的目标时充当中间人的作用。
    消息中间件的主要目的是提供路由并保证消息的传递；如果发送消息时接收者不可用，消息队列会保留消息，
    直到可以成功的传递它为止。当然消息队列保存消息也是有期限的
-- 消息中间件的特点
    1， 采用异步处理模式
    消息发送者可以发送一个消息而无需等待响应。消息发送者将消息发送到一条虚拟的通道(主题或队列上)，
    消息接收者则订阅或监听该通道。 一条消息可能最终转发给一个或者多个消息接收者，这些消息接收者都无需
    对消息发送者做出同步回应。 整个过程是异步的。
        比如： 用户信息注册，注册完成后过段时间发送邮件或者短信
    2，应用程序和应用程序调用关系为松耦合的关系
        发送者和接收者都无需了解对方，只需要确认消息。
        发送者和接受者不必同时在线
        比如： 在线交易系统为了保证数据一致，在支付系统处理完成后把支付结果放到消息中间件里，通知订单系统修改订单状态，
        两个系统通过消息中间件解耦
--  消息中间件的传递模型
    1. 点对点模型(PTP) 
        用于消息生产者与消息消费者之间点到点的通信。消息生产者将消息发动到由某个名字标识的特定的消费者。这个名字实际对应
        于一个队列(Queue),在消息传递给消费者之前它被存储在这个队列中，队列消息可以放在内存中也可以是持久的，以保证在消息
        服务出现故障时仍然能够传递消息
        特性：
         - 每个消息只有一个消费者
         - 发送者和接收者没有时间依赖
         - 接收者确认消息接收和处理成功
    2. 发布-订阅模型(Pub/Sub)
        支持向一个特定的消息主题生产消息。0或者多个订阅者可能对接受来自特定的消息主题的消息感兴趣。在这种模型下，发布和订阅者
        彼此不知道对方，这种模式好比匿名公告板。这种模式概括为：多个消费者可以获得消息，在发布者和订阅者之间存在时间依赖性，
        发布者需要建立一个订阅(subscription)，以便能够消费者订阅。订阅者必须要保持持续的活动状态以接收消息，除非订阅者
        建立持久的订阅，在这种情况下，在订阅者链接时发布的消息将在订阅者重新链接时重新发布。
        特性：
         - 每个消息可以有多个消费者
         - 发布者和订阅者有时间依赖：  客户端只有订阅后才能接收到消息
         - 持久订阅(订阅关系建立后，消息就不会消失，不管订阅者是否都在线)和非持久订阅(订阅者为了接收消息，必须一直在线，当只有一个订阅者时约等于点对点模式)
-- 消息中间件的应用场景
    1， 用户注册成功，注册成功后会过一会发送邮件确认或短信
    2， 日志分析使用 把日志进行集中收集，用于计算pv，用户行为分析
    3， 数据复制案例 将数据从源头复制到多个目的地，一般是要求顺序或者保证因果序列。用于跨机房数据传输，搜素，离线数据计算等。
    4， 延迟消息发送和暂存 把消息中间件当成可靠的消息暂存地 定时进行消息投递，比如模拟用户秒杀访问，进行系统性能压测。
    5， 消息广播 缓存数据同步更新 往应用推送数据
-- 消息中间件的分类
    1, (push) 推消息模型：消息生产者将消息发送给消息传递服务，消息传递服务又将消息推给消息消费者
        服务端： 消息存储，处理请求，保存推送轨迹，保存订阅关系，消费者负载均衡，集中式
        客户端： 处理和响应请求
        实时性： 较好，收到数据后立即发送给客户端
        消费者故障： 消费者在故障情况下，服务端堆积消息，重复推送耗费资源，保存推送轨迹压力过大
        其他： 对消息推送有更多控制，能够实现多样化的推送机制，当消费者数量增多的时候，推送的压力大，
            性能天花板。消费者处理能力差异，导致堆消息。
    2, (pull) 拉消息类型：消费者请求消息服务接受消息，消息服务从消息生产者拉取该消息。
       服务端： 消息存储，处理请求，分布式
       客户端： 处理和响应请求，保存pull状态，如拉取位置的偏移量offset，异常情况下的消息暂存和recover
       实时性： 取决于pull的间隔时间
       消费者故障：对服务端无影响
       其他：需要在客户端实现消息过滤，浪费资源，需要在不同的客户端之间协调，做负载均衡
-- rabbit mq 简介
    rabbitMQ 是一个在AMQP协议标准基础上完成的，可复用的企业消息系统，它遵循Mozilla Public License 开源协议。
    采用Erlang实现的工业级的消息队列(MQ)服务器

    AMQP (Advanced Message Queuing Protocol 高级消息队列协议) 是一个异步消息传递所使用的应用层协议规范。
    做为线路层协议，而不是API(JMS), AMQP客户端可以无视消息的来源，任意发送和接收消息。

    rabbitMQ 的两大核心组件是Exchange和 Queue

    重要术语：
        1, Server(broker): 接收客户端连接，实现AMQP消息队列和路由功能的进程
        2, Virtual Host: 其实是一个虚拟概念，类似于权限控制组，一个Virtual Host里面可以有若干个Exchange和Queue,但是控制的最小颗粒度为Virtual Host
        3, Exchange: 接收生产者发送的消息，并根据Binding规则将消息路由给服务器中的队列。ExchangeType决定了Exchange路由消息的行为，例如，在rabbitMQ中，
        ExchangeType有redirect，Fanout和Topic三种。 不同类型的Exchange路由的行为是不一样的。
        4，Message: 由Header和Body组成，Header是由生产者添加的这种属性的集合，包括message是否被持久化，由哪个message queue接受，优先级是多少等。
        而body是真正需要传输的App数据。
        5，Message Queue: 消息队列，用于存储还未被消费者消费的数据
        6，BindingKey: 所谓绑定就是将一个特定的Exchange和一个特定的queue绑定起来，绑定的关键字成为BindingKey
    Exchange的三种方式：
        1，Direct Exchange - 直接交互式的处理路由键。需要将一个队列绑定到交换机上要求该消息与一个特定的路由键完全匹配。这是一个完整的匹配。如果一个队列绑定
        到该交换机上要求路由键“dog”,则只有被标记为“dog”的消息才被转发，不会转发"dog.puppy",也不会转发“dog.guard”。
        2，Fanout Exchange - 广播式路由键。 你只需要简单的将队列绑定到交换机上，一个发送到交换机的徐岙许都会被转发到与该交换机绑定的所有队列上，很像子网广播，
        每台子网内的主机都获得了一份复制的消息，Fanout转发消息是最快的
        3，Topic Exchange - 主题式交换器。通过消息的路由的关键字和绑定的关键字的模式匹配。将消息路由到被绑定的队列中这种路由器类型可以被使用支持用来经典的发布订阅消息模型。使用主题命名空间做为消息寻址模式。将消息传递给那些部分或者全部匹配主题模式的多个消费者。主题交换机类型的工作方式如下：绑定关键词用零个或者
        多个标记组成，每一个标记之间用“.” 分割。绑定关键字必须用这种形式明确说明，并支持通配符: "*" 匹配一个词组，“#”匹配零个或者多个词组。因此绑定关键字“*.stock.#” 匹配路由关键字“used.stock” 和“eur.stock.db”,但是不匹配“stcok.hshd”

-- rabbitMQ 配置
    环境变量的配置文件：rabbitmq.env.conf
    配置信息的配置文件：rabbitmq.config
    注意：这两个文件默认是没有的，需要自己创建
    1，rabbitmq.env.conf 这个文件的位置是确定不可变的，位于：/etc/rabbitmq目录下(这个目录需要自己创建)。
        RABBITMQ_NODE_IP_ADDRESS：指定ip地址
        RABBITMQ_NODE_PORT: 指定端口号，默认是5672
        RABBITMQ_CONFIG_FILE: 配置文件的路径，后缀名必须是.config
        RABBITMQ_LOG_BASE: 日志文件路径
    2，rabbitmq.config 这是一个标准的erlang配置文件。它必须符合erlang配置文件的标准。Erlang tuple,结构为{key,value},
    key 是atom类型，value是一个term,其中几个关键参数为:
        1,tcp_listerners设置rabbitmq的监听端口，默认为5672
        2,disk_free_limit 磁盘低水位线，若磁盘容量低于指定值则停止接收数据。
        3,vm_memory_high_watermark 设置内存低水位线，若低于该水位线，则开启流控机制，默认值是0.4，即内存总量的40%
-- rabbitMQ 命令介绍
    /etc/init.d/rabbitmq-server start | stop | restart | reload
    rabbitmqctl add_vhost vhostname 创建vhost
    rabbitmqctl delete_vhost vhostname 删除vhost
    rabbitmqctl list_vhosts 遍历所有虚拟主机信息
    rabbitmqctl add_user username password 添加用户及密码
    rabbitmqctl change_password username newpassword 修改用户密码
    rabbitmqctl set_permssions -p v_host user ".*"".*"".*"//绑定权限，并且具备读写的权限
    rabbitmqctl list_queues //显示所有队列

-- rabbitmq 安装
    1， 安装erlang依赖
     sudo yum install epel-release -y
     yum install erlang -y
-- 简介
    MQ为message queue 消息队列是应用程序之间的通信方法
    AMQP：即advanced Message Queuing Protocol一个提供统一消息服务的应用层标准高级消息队列协议，应用层协议的一个标准开放，为面向消息的中间件设计。
    rabbit 是一个开源的，在AMQP基础上完整的，可复用的企业消息系统
    支持主流的操作系统。
 