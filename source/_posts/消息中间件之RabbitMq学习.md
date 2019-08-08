---
title: 消息中间件之RabbitMq学习
tags: RabbitMQ
categories: 消息中间件
abbrlink: efd55b48
date: 2019-07-27 16:40:40
---
消息中间件之RabbigMQ
<!--more-->
### RabbitMQ简介
    RabbitMQ是一个消息代理，用于接受消息和转发消息。
    RabbitMQ接受、存储和转发二进制数据块---即消息。
    尽管消息流经RabbitMQ，但他们只能存储在队列中。
    一个队列只受主机内存和磁盘限制约束，它本质上是一个很大的消息缓冲区
### 消息队列
   消息(Message)是指在应用间传送的数据。
   消息队列(Message Queue)是一种应用间通信的方式，消息发送后可以立即返回，由消息系统来确保消息的可靠传递。
   消息发布者只管把消息发布到MQ中而不用管谁来取，消息使用者只管从MQ中取消息而不管是谁发布的。
   因此，发布者和使用者都不用知道对方的存在。
   消息队列是一种应用间的异步协作机制。
   常见场景：广播、错峰流控、最终一致性、业务解耦
### RabbitMQ特点
RabbitMQ 是一个由 Erlang 语言开发的 AMQP 的开源实现。

AMQP ：Advanced Message Queue，高级消息队列协议。它是应用层协议的一个开放标准，为面向消息的中间件设计，基于此协议的客户端与消息中间件可传递消息，并不受产品、开发语言等条件的限制。

RabbitMQ 最初起源于金融系统，用于在分布式系统中存储转发消息，在易用性、扩展性、高可用性等方面表现不俗。具体特点包括：

（1）可靠性（Reliability）
RabbitMQ 使用一些机制来保证可靠性，如持久化、传输确认、发布确认。

（2）灵活的路由（Flexible Routing）
在消息进入队列之前，通过 Exchange 来路由消息的。对于典型的路由功能，RabbitMQ 已经提供了一些内置的 Exchange 来实现。针对更复杂的路由功能，可以将多个 Exchange 绑定在一起，也通过插件机制实现自己的 Exchange 。

（3）消息集群（Clustering）
多个 RabbitMQ 服务器可以组成一个集群，形成一个逻辑 Broker 。

（4）高可用（Highly Available Queues）
队列可以在集群中的机器上进行镜像，使得在部分节点出问题的情况下队列仍然可用。

（5）多种协议（Multi-protocol）
RabbitMQ 支持多种消息队列协议，比如 STOMP、MQTT 等等。

（6）多语言客户端（Many Clients）
RabbitMQ 几乎支持所有常用语言，比如 Java、.NET、Ruby 等等。

（7）管理界面（Management UI）
RabbitMQ 提供了一个易用的用户界面，使得用户可以监控和管理消息 Broker 的许多方面。

（8）跟踪机制（Tracing）
如果消息异常，RabbitMQ 提供了消息跟踪机制，使用者可以找出发生了什么。

（9）插件机制（Plugin System）
RabbitMQ 提供了许多插件，来从多方面进行扩展，也可以编写自己的插件。
### RabbitMQ中的概念模型--消息模型
所有 MQ 产品从模型抽象上来说都是一样的过程：
消费者（consumer）订阅某个队列。生产者（producer）创建消息，然后发布到队列（queue）中，最后将消息发送到监听的消费者。 
### RabbitMQ基本概念
（1）Message
消息，消息是不具名的，它由消息头和消息体组成。消息体是不透明的，而消息头则由一系列的可选属性组成，这些属性包括routing-key（路由键）、priority（相对于其他消息的优先权）、delivery-mode（指出该消息可能需要持久性存储）等。
（2）Publisher
消息的生产者，也是一个向交换器发布消息的客户端应用程序。
（3）Exchange
交换器，用来接收生产者发送的消息并将这些消息路由给服务器中的队列。
（4）Binding
绑定，用于消息队列和交换器之间的关联。一个绑定就是基于路由键将交换器和消息队列连接起来的路由规则，所以可以将交换器理解成一个由绑定构成的路由表。
（5）Queue
消息队列，用来保存消息直到发送给消费者。它是消息的容器，也是消息的终点。一个消息可投入一个或多个队列。消息一直在队列里面，等待消费者连接到这个队列将其取走。
（6）Connection
网络连接，比如一个TCP连接。
（7）Channel
信道，多路复用连接中的一条独立的双向数据流通道。信道是建立在真实的TCP连接内地虚拟连接，AMQP 命令都是通过信道发出去的，不管是发布消息、订阅队列还是接收消息，这些动作都是通过信道完成。因为对于操作系统来说建立和销毁 TCP 都是非常昂贵的开销，所以引入了信道的概念，以复用一条 TCP 连接。
（8）Consumer
消息的消费者，表示一个从消息队列中取得消息的客户端应用程序。
（9）Virtual Host
虚拟主机，表示一批交换器、消息队列和相关对象。虚拟主机是共享相同的身份认证和加密环境的独立服务器域。每个 vhost 本质上就是一个 mini 版的 RabbitMQ 服务器，拥有自己的队列、交换器、绑定和权限机制。vhost 是 AMQP 概念的基础，必须在连接时指定，RabbitMQ 默认的 vhost 是 / 。
（10）Broker
表示消息队列服务器实体。

### RabbitMQ消息持久性、确认机制、拒绝、预取数量、分配策略
   1. 消息的持久性
   为了保证消息的可靠性，需要对消息进行持久化。
为了保证RabbitMQ在重启、奔溃等异常情况下数据没有丢失，除了对消息本身持久化为，还需要将消息传输经过的队列(queue)，交互机进行持久化(exchange)，持久化以上元素后，消息才算真正RabbitMQ重启不会丢失。
   
    __消息持久化__
方法：
   ```
void basicPublish(String exchange, String routingKey, BasicProperties props, byte[] body) throws IOException;   
   ```
    第三个参数props：设置投递模式为持久化，如果此值是persistent ，则此消息存储在磁盘上。如果服务器重启，系统会保证收到的持久化消息未丢失，将消息以持久化方式发布时，会对性能造成一定的影响
消息持久化代码如下:
```
channel.basicPublish("", TASK_QUEUE_NAME,
                    MessageProperties.PERSISTENT_TEXT_PLAIN,
                    message.getBytes("UTF-8")); 

```
队列持久化：
```
Queue.DeclareOk queueDeclare(String queue, boolean durable, boolean exclusive, boolean autoDelete,
                                 Map<String, Object> arguments) throws IOException;
```
参数解释：
```
第二个参数 durable
是否持久化，如果true，则此种队列叫持久化队列（Durable queues）。此队列会被存储在磁盘上，当消息代理（broker）重启的时候，它依旧存在。没有被持久化的队列称作暂存队列（Transient queues）。
第三个参数 execulusive
表示此对应只能被当前创建的连接使用，而且当连接关闭后队列即被删除。此参考优先级高于durable
第四个参数 autoDelete
当没有生成者/消费者使用此队列时，此队列会被自动删除。
(即当最后一个消费者退订后即被删除)

```
代码：
```
channel.queueDeclare(TASK_QUEUE_NAME, true, false, false, null);
```

交换机持久化：
声明交换机方法
```
 Exchange.DeclareOk exchangeDeclare(String exchange, String type, boolean durable) throws IOException;   

```
方法参数说明：
 > 第三个参数durable:交换机是否持久化



__消息确认机制__
消费者在处理消息的时候偶尔会失败或者有时会直接崩溃掉。而且网络原因也有可能引起各种问题，对于此AMQP有两种处理方式：

    ○ 自动确认模式（automatic acknowledgement model）：当RabbbitMQ将消息发送给应用后，消费者端自动回送一个确认消息，此时RabbitMQ删除此消息。
    ○ 显式确认模式（explicit acknowledgement model）：消费者收到消息后，可以在执行一些逻辑后，消费者自己决定什么时候发送确认回执（acknowledgement），RabbitMQ收到回执后才删除消息，这样就保证消费端不会丢失消息

如果一个消费者在尚未发送确认回执的情况下挂掉了，那么消息会被重新放入队列，并且在还有其他消费者存在于此队列的前提下，立即投递给另外一个消费者。如果当时没有可用的消费者了，消息代理会死等下一个注册到此队列的消费者，然后再次尝试投递。
RabbitMQ里的消息是不会过期。当消费者挂掉后，RabbitMQ会不断尝试重推。所有单个消息的推送可能花费很长的时间

是否开启自动确认模式由以下方法的autoAck属性决定
```
String basicConsume(String queue, boolean autoAck, Consumer callback) throws IOException;
```
代码实现

    a 自动确认模式：只需要设置此值为autoAck为true即可.
    b. 显示确认模式：见WorkQueuesRecv.java

WorkQueuesRecv有两个注意点：
a. channel.basicConsume()第二个参数autoAck值为false
b. 收到消息后，必须调用 channel.basicAck 向rabbitMQ发送确认回执

```
package com.hry.spring.rabbitmq.basic.workqueues;

import com.rabbitmq.client.*;

import java.io.IOException;
/**
 * Created by huangrongyou@yixin.im on 2018/1/9.
 */
public class WorkQueuesRecv {
    private static final String TASK_QUEUE_NAME = "task_queue";


    public static void execute(String host, String userName, String password, int id){
        // 配置连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost(host);
        // 需要在管理后台增加一个hry帐号
        factory.setUsername(userName);
        factory.setPassword(password);
        try {
            // 建立TCP连接
            final Connection connection = factory.newConnection();
            // 在TCP连接的基础上创建通道
            final Channel channel = connection.createChannel();
            // 声明一个队列
            channel.queueDeclare(TASK_QUEUE_NAME, true, false, false, null);
            System.out.println(" [WorkQueuesRecv-" +id+ "] Waiting for messages.");
            // 每个客户端每次最后获取N个消息
        //    channel.basicQos(1);
            // 默认消费者实现
            final Consumer consumer = new DefaultConsumer(channel) {
                @Override
                public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                    String message = new String(body, "UTF-8");

                    System.out.println(" [WorkQueuesRecv-" +id+ "] Received '" + message + "'");
                    try {
                        doWork(message);
                    } finally {
                        System.out.println(" [WorkQueuesRecv-" +id+ "] Done");

                        // 情况一：对处理好的消息进行应答
                        channel.basicAck(envelope.getDeliveryTag(), false);

                        // 情况二：对于id=0的消费者者正常应答消息，其它id=0，解决此消息并要求重发
//                        if(id == 0){
//                            // 对处理好的消息进行应答
//                            channel.basicAck(envelope.getDeliveryTag(), false);
//                        }else {
                            // 拒绝当前这条消息
                  //          channel.basicReject(envelope.getDeliveryTag(), true);
                            // 拒绝包含本条delivery_tag 所对应消息在内的所有比该值小的消息都被拒绝了（除了已经被 ack 的以外)
                            // channel.basicNack(envelope.getDeliveryTag(), false, false);
//                        }


                    }
                }
            };
            // 获取消息
            channel.basicConsume(TASK_QUEUE_NAME, false, consumer);
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    private static void doWork(String task) {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException _ignored) {
            Thread.currentThread().interrupt();
        }
    }
}
```
消息确认方法：
```
void basicAck(long deliveryTag, boolean multiple) throws IOException;
```
方法详细参数如下：
```
第一个参数deliveryTag：发布的每一条消息都会获得一个唯一的deliveryTag，(任何channel上发布的第一条消息的deliveryTag为1，此后的每一条消息都会加1)，deliveryTag在channel范围内是唯一的
第二个参数multiple：批量确认标志。如果值为true，则执行批量确认，此deliveryTag之前收到的消息全部进行确认; 如果值为false，则只对当前收到的消息进行确认
```
备注：
如果在获取消息时采用不自动应答，但是获取消息后不调用basicAck，则后果会很严重。RabbitMQ会认为消息没有投递成功，不仅所有的消息都会保留到内存中，而且在客户重新连接后，会将所有的消息重新投递一遍



__拒绝消息__

当消费者接收到某条消息后，处理过程有可能失败，这时消费者可以拒绝此消息。在拒绝消息时，消费者会告诉RabbitMQ如何处理这条消息：销毁它或者重新放入队列。
可以有两种方式拒绝此消息

a. channel.basicReject：只支持对一条消息进行拒绝
拒绝方法:
```
void basicReject(long deliveryTag, boolean requeue) throws IOException;
```
方法详细参数如下：

    第一个参数deliveryTag：发布的每一条消息都会获得一个唯一的deliveryTag，deliveryTag在channel范围内是唯一的
    第二个参数requeue：表示如何处理这条消息，如果值为true，则重新放入RabbitMQ的发送队列，如果值为false，则通知RabbitMQ销毁这条消息
代码如下：
```
channel.basicReject(envelope.getDeliveryTag(), true);

```
b. channel.basicNack
channel.basicNack是 channel.basicReject的补充，提供一次对多条消息进行拒绝的功能
方法如下：
```
void basicNack(long deliveryTag, boolean multiple, boolean requeue) throws IOException; 
```

方法参数：

第一个参数deliveryTag：发布的每一条消息都会获得一个唯一的deliveryTag，deliveryTag在channel范围内是唯一的

第二个参数multiple：批量确认标志。如果值为true，包含本条消息在内的、所有比该消息deliveryTag值小的 消息都被拒绝了（除了已经被 ack 的以外）;如果值为false，只拒绝三本条消息

第三个参数requeue：表示如何处理这条消息，如果值为true，则重新放入RabbitMQ的发送队列，如果值为false，则通知RabbitMQ销毁这条消息

代码如下：
```
channel.basicNack(envelope.getDeliveryTag(), false, false);
```
备注：
当此队列只有一个消费者时，请确认不要由于拒绝消息并且选择了重新放入队列的行为而引起消息在同一个消费者身上无限循环的情况发生。

__设置预取消息的数量__

默认情况下，RabbitMQ收到消息后，就向消费者送。但是如果消息过多，且消息的数量超过了消息者处理能力从而导致其崩溃。此时我们可以通过prefetchCount 限制每个消费者在收到下一个确认回执前一次可以最大接受多少条消息。即如果设置prefetchCount =1，RabbitMQ向这个消费者发送一个消息后，再这个消息的消费者对这个消息进行ack之前，RabbitMQ不会向这个消费者发送新的消息

代码如下：
```
// 每个客户端每次最后获取N个消息
channel.basicQos(1);
```

__消息分配策略__
多个消费者同时消费同一个队列，Rabbit的消息的分配策略是什么？
如果同一个队列，有多个消费者消费这个队列。RabbitMQ默认是按照轮询的策略发送消息，即发送的顺序是消费者1，消费者2，消费者1，消费者2…。所以平均下来，每个消费者消费的消息数量几乎相同。

```
package com.hry.spring.rabbitmq.basic.workqueues;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.MessageProperties;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * Created by huangrongyou@yixin.im on 2018/1/9.
 */
public class WorkQueuesSend {
    private static final String TASK_QUEUE_NAME = "task_queue";

    public static void execute(String host, String userName, String password) {
        // 配置连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost(host);
        // 需要在管理后台增加一个hry帐号
        factory.setUsername(userName);
        factory.setPassword(password);
        Connection connection = null;
        Channel channel = null;
        try {
            // 建立TCP连接
            connection = factory.newConnection();
            // 在TCP连接的基础上创建通道
            channel = connection.createChannel();
            // queueDeclarePassive(String queue)可以用来检测一个queue是否已经存在
            // Queue.DeclareOk queueDeclarePassive(String queue) throws IOException
            // 声明一个队列
            channel.queueDeclare(TASK_QUEUE_NAME, true, false, false, null);

            String message = "Work Queues!" + System.currentTimeMillis();
            // 发送一个持久化消息
            channel.basicPublish("", TASK_QUEUE_NAME,
                    MessageProperties.PERSISTENT_TEXT_PLAIN,
                    message.getBytes("UTF-8"));
            System.out.println(" [WorkQueuesSend] Sent '" + message + "'");
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            try {
                channel.close();
                connection.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```

```
package com.hry.spring.rabbitmq.basic;

import com.hry.spring.rabbitmq.basic.helloworld.HelloworldRecv;
import com.hry.spring.rabbitmq.basic.helloworld.HelloworldSend;
import com.hry.spring.rabbitmq.basic.publishsubscribe.Publish;
import com.hry.spring.rabbitmq.basic.publishsubscribe.Subscribe;
import com.hry.spring.rabbitmq.basic.routing.RoutingRecv;
import com.hry.spring.rabbitmq.basic.routing.RoutingSend;
import com.hry.spring.rabbitmq.basic.rpc.RpcClient;
import com.hry.spring.rabbitmq.basic.rpc.RpcServer;
import com.hry.spring.rabbitmq.basic.topics.TopicsRecv;
import com.hry.spring.rabbitmq.basic.topics.TopicsSend;
import com.hry.spring.rabbitmq.basic.workqueues.WorkQueuesRecv;
import com.hry.spring.rabbitmq.basic.workqueues.WorkQueuesSend;
import org.junit.Test;

import java.util.Arrays;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * Created by huangrongyou@yixin.im on 2018/1/10.
 */
public class BasicTest {
    // 测试线程池
    private ExecutorService executorService = Executors.newFixedThreadPool(10);

    // rabbitmq的IP地址
    private final String rabbitmq_host = "10.240.80.147";
    // rabbitmq的用户名称
    private final String rabbitmq_user = "hry";
    // rabbitmq的用户密码
    private final String rabbitmq_pwd = "hry";

    @Test
    public void helloworld() throws InterruptedException {
        // 接收端
        executorService.submit(() -> {
            HelloworldRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
        });
        Thread.sleep(5* 100);

        // 发送端
        executorService.submit(() -> {
            HelloworldSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
        });
        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void workqueues() throws InterruptedException {
        // 接收端
        int recNum = 2;
        while(recNum-- > 0) {
            final int recId = recNum;
            executorService.submit(() -> {
                WorkQueuesRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, recId);
            });
        }
        Thread.sleep(5* 100);
        // 发送端
        int sendNum = 4;
        while(sendNum-- > 0){
            executorService.submit(() -> {
                WorkQueuesSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
            });
        }

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void publishsubscribe() throws InterruptedException {
        // 接收端
        int recNum = 2;
        while(recNum-- > 0) {
            final int recId = recNum;
            executorService.submit(() -> {
                Subscribe.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, recId);
            });
        }
        Thread.sleep(5* 100);
        // 发送端
        int sendNum = 2;
        while(sendNum-- > 0){
            executorService.submit(() -> {
                Publish.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
            });
        }

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void routing_1() throws InterruptedException {
        // 接收端 1：绑定 orange 值
        executorService.submit(() -> {
            String[] colours = {"orange"};
            RoutingRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, colours);
        });
        // 接收端 2：绑定 black、green 值
        executorService.submit(() -> {
            String[] colours = {"black","green"};
            RoutingRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, colours);
        });

        Thread.sleep(5* 100);
        // 发送端 ： 发送 black，只有接收端1收到
        executorService.submit(() -> {
            String routing = "orange";
            RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // 发送端 ： 发送 green、black，只有接收端2收到
        executorService.submit(() -> {
            String routing = "green";
            RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void routing_2() throws InterruptedException {

        // 接收端：同时创建两个接收端，同时绑定black
        int recNum = 2;
        while(recNum-- > 0) {
             // 接收端：绑定 black 值
            executorService.submit(() -> {
                String[] colours = {"black"};
                RoutingRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, colours);
            });
        }

        Thread.sleep(5* 100);
        // 发送端1 ： 发送 black，所有的接收端都会收到
        executorService.submit(() -> {
            String routing = "black";
            RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // 发送端2 ： 发送 green，所有的接收端都不会收到
        executorService.submit(() -> {
            String routing = "green";
            RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void topics() throws InterruptedException {

        // 消费者1：绑定 *.orange.* 值
        executorService.submit(() -> {
            String[] bindingKeys = {"*.orange.*"};
            TopicsRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, bindingKeys);
        });

        // 消费者2：绑定  "*.*.rabbit" 和 "lazy.#"值
        executorService.submit(() -> {
            String[] bindingKeys = {"*.*.rabbit", "lazy.#"};
            TopicsRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, bindingKeys);
        });

        Thread.sleep(5* 100);
        // 生产者1 ： 发送 black，所有的接收端都会收到
        executorService.submit(() -> {
            String routing = "quick.orange.rabbit";
            TopicsSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // 生产者2 ： 发送 green，所有的接收端都不会收到
        executorService.submit(() -> {
            String routing = "lazy.pink.rabbit";
            TopicsSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void rpc() throws InterruptedException {

        // rpc服务端
        executorService.submit(() -> {
            RpcServer.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
        });

        // rpc客户端
        executorService.submit(() -> {
            RpcClient.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, "rpc test");
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

}
```
### RabbitMQ之交换机的四种类型和属性
__交换机作用__
    在RabbitMQ中，生产者不是直接将消息发送给消费者，生成者根本不知道这个消息要传递给哪些队列。实际上，生产者只是将消息发送到交换机。交换机收到消息到，根据交换机的类型和配置来处理消息，有如下几种情况：

    将消息传送到特定的队列
    有可能发送到多个队列中
    也有可能丢弃消息

RabbitMQ各个组件的功能重新归纳一下如下：

    生产者：发送消息
    交换机：将收到的消息根据路由规则路由到特定队列
    队列：用于存储消息
    消费者：收到消息并消费

__交换机的类型__
 交换机主要包括如下4种类型：

    Direct exchange（直连交换机）
    Fanout exchange（扇型交换机）
    Topic exchange（主题交换机）
    Headers exchange（头交换机）

另外RabbitMQ默认定义一些交换机：

    默认交换机
    amq.* exchanges

还有一类特殊的交换机：Dead Letter Exchange（死信交换机）
Direct exchange（直连交换机）

__直连型交换机（direct exchange）__
 是根据消息携带的路由键（routing key）将消息投递给对应队列的，步骤如下：

    1.将一个队列绑定到某个交换机上，同时赋予该绑定一个路由键（routing key）
    2.当一个携带着路由值为R的消息被发送给直连交换机时，交换机会把它路由给绑定值同样为R的队列。

  有时我们不希望所有的消息都被所有队列接收，我们希望可以指定类型为a的消息只能被队列A接收，类型为b的消息只能被队列B,C接收。扇型交换机只能无脑地广播消息给所有的消费者，其实质是广播给所有关联的队列。
为了实现这个功能，一种是建立多个交换机，这种方式简单暴力但是不灵活。本节我们介绍使用单个直连交换机+路由实现以上功能。

单个绑定和多个绑定

__生产者代码：__

主要业务逻辑如下：
```
1. 配置连接工厂
2. 建立TCP连接
3. 在TCP连接的基础上创建通道
4. 声明一个direct交换机 
5. 发送消息，并配置消息的路由键

```
第四、五步代码如下：
```
// 声明一个direct交换机
channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.DIRECT);
String message = "RoutingSend-" + System.currentTimeMillis();
// 发送消息，并配置消息的路由键
channel.basicPublish(EXCHANGE_NAME, routingKey, null, message.getBytes("UTF-8"));
```
RoutingSend.java
```
package com.hry.spring.rabbitmq.basic.routing;

import com.rabbitmq.client.BuiltinExchangeType;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

public class RoutingSend {
    private static final String EXCHANGE_NAME = "direct_logs";

    public static void execute(String host, String userName, String password, String routingKey) {
        // 配置连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost(host);
        // 需要在管理后台增加一个hry帐号
        factory.setUsername(userName);
        factory.setPassword(password);

        Connection connection = null;
        Channel channel = null;
        try {
            // 建立TCP连接
            connection = factory.newConnection();
            // 在TCP连接的基础上创建通道
            channel = connection.createChannel();
            // 声明一个direct交换机
            channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.DIRECT);
            String message = "RoutingSend-" + System.currentTimeMillis();
            // 发送消息，并配置消息的路由键
            channel.basicPublish(EXCHANGE_NAME, routingKey, null, message.getBytes("UTF-8"));
            System.out.println(" [RoutingSend] Sent '" + routingKey + "':'" + message + "'");
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            try {
                // 空值判断，为了代码简洁略
                channel.close();
                connection.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        }
}
```
__消费者的代码__

主要业务逻辑如下：
1. 配置连接工厂
2. 建立TCP连接
3. 在TCP连接的基础上创建通道
4. 声明一个direct交换机
5. 声明一个临时队列
6. 将临时队列绑定到交换机上，并在队列上绑定多个绑定值
7. 接收消息并处理

第4,5,6步代码如下：
```
// 声明一个direct交换机
channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.DIRECT);
// 声明一个临时队列
String queueName = channel.queueDeclare().getQueue();
// 绑定路由，同一个队列可以绑定多个值
for (String colour : colours) {
    channel.queueBind(queueName, EXCHANGE_NAME, colour);
}
```
RoutingRecv.java
```
package com.hry.spring.rabbitmq.basic.routing;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.Arrays;

public class RoutingRecv {
    private static final String EXCHANGE_NAME = "direct_logs";

    public static void execute(String host, String userName, String password, String[] colours){
        // 配置连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost(host);
        // 需要在管理后台增加一个hry帐号
        factory.setUsername(userName);
        factory.setPassword(password);

        Connection connection = null;
        Channel channel = null;
        try {
            // 建立TCP连接
            connection = factory.newConnection();
            // 在TCP连接的基础上创建通道
            channel = connection.createChannel();
            // 声明一个direct交换机
            channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.DIRECT);
            // 声明一个临时队列
            String queueName = channel.queueDeclare().getQueue();
            // 绑定路由，同一个队列可以绑定多个值
            for (String colour : colours) {
                channel.queueBind(queueName, EXCHANGE_NAME, colour);
            }
            System.out.println(" [RoutingRecv-" + Arrays.toString(colours) + "] Waiting for messages.");
            // 定义消息的回调处理类
            Consumer consumer = new DefaultConsumer(channel) {
                @Override
                public void handleDelivery(String consumerTag, Envelope envelope,
                                           AMQP.BasicProperties properties, byte[] body) throws IOException {
                    String message = new String(body, "UTF-8");
                    System.out.println(" [RoutingRecv-" + Arrays.toString(colours) + "] Received '" + envelope.getRoutingKey() + "':'" + message + "'");
                }
            };
            // 接收消息
            channel.basicConsume(queueName, true, consumer);
        }catch (Exception e){
            e.printStackTrace();
        }finally {
        }
    }
}
```
测试：
1. 模拟上文中”单个绑定”的场景：

BasicTest.java
```
@Test
public void routing_1() throws InterruptedException {
    // 接收端 1：绑定 orange 值
    executorService.submit(() -> {
        String[] colours = {"orange"};
        RoutingRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, colours);
    });
    // 接收端 2：绑定 black、green 值
    executorService.submit(() -> {
        String[] colours = {"black","green"};
        RoutingRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, colours);
    });

    Thread.sleep(5* 100);
    // 发送端1 ： 发送 black，只有接收端1收到
    executorService.submit(() -> {
        String routing = "orange";
        RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
    });

    // 发送端2 ： 发送 green、black，只有接收端2收到
    executorService.submit(() -> {
        String routing = "green";
        RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
    });

    // sleep 10s
    Thread.sleep(10 * 1000);
}

```
以上代码启动2个消费者，消费者1绑定orange，消费者2绑定black、green;
启动2个生产者，生产者1发送消息的路由键为orange，此消息只被消费者1接收
生产者2发送消息的路由键为green，此消息只被消费者2接收，符合之前的分析
```
 [RoutingRecv-[orange]] Waiting for messages.
 [RoutingSend] Sent 'orange':'RoutingSend-1516003763435'
 [RoutingSend] Sent 'green':'RoutingSend-1516003763444'
 // 生产者1发送消息的路由键为orange，此消息只被消费者1接收
 [RoutingRecv-[orange]] Received 'orange':'RoutingSend-1516003763435'
 // 生产者2发送消息的路由键为green，此消息只被消费者2接收
 [RoutingRecv-[black, green]] Waiting for messages.
```

2. 模拟上文中”多个绑定”的场景

BasicTest.java
```
@Test
public void routing_2() throws InterruptedException {

    // 接收端：同时创建两个接收端，同时绑定black
    int recNum = 2;
    while(recNum-- > 0) {
         // 接收端：绑定 black 值
        executorService.submit(() -> {
            String[] colours = {"black"};
            RoutingRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, colours);
        });
    }

    Thread.sleep(5* 100);
    // 发送端1 ： 发送 black，所有的接收端都会收到
    executorService.submit(() -> {
        String routing = "black";
        RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
    });

    // 发送端2 ： 发送 green，所有的接收端都不会收到
    executorService.submit(() -> {
        String routing = "green";
        RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
    });

    // sleep 10s
    Thread.sleep(10 * 1000);
}
```
以上代码启动2个消费者，都绑定black
启动2个生产者，生产者1发送消息的路由键为black，此消息被2个消费者都接收
生产者2发送消息的路由键为green，此消息被丢失，符合之前的分析

测试输出如下:
```
 [RoutingRecv-[black]] Waiting for messages.
 [RoutingRecv-[black]] Waiting for messages.
 // 生产者1发送消息的路由键为black，此消息被2个消费者都接收
 [RoutingSend] Sent 'black':'RoutingSend-1516003663034'
 [RoutingRecv-[black]] Received 'black':'RoutingSend-1516003663034'
 [RoutingRecv-[black]] Received 'black':'RoutingSend-1516003663034'
 // 生产者2发送消息的路由键为green，此消息被丢失
 [RoutingSend] Sent 'green':'RoutingSend-1516003663045'
```

BasicTest.java
```
package com.hry.spring.rabbitmq.basic;

import com.hry.spring.rabbitmq.basic.header.HeaderRecv;
import com.hry.spring.rabbitmq.basic.header.HeaderSend;
import com.hry.spring.rabbitmq.basic.helloworld.HelloworldRecv;
import com.hry.spring.rabbitmq.basic.helloworld.HelloworldSend;
import com.hry.spring.rabbitmq.basic.publishsubscribe.Publish;
import com.hry.spring.rabbitmq.basic.publishsubscribe.Subscribe;
import com.hry.spring.rabbitmq.basic.routing.RoutingRecv;
import com.hry.spring.rabbitmq.basic.routing.RoutingSend;
import com.hry.spring.rabbitmq.basic.rpc.RpcClient;
import com.hry.spring.rabbitmq.basic.rpc.RpcServer;
import com.hry.spring.rabbitmq.basic.topics.TopicsRecv;
import com.hry.spring.rabbitmq.basic.topics.TopicsSend;
import com.hry.spring.rabbitmq.basic.workqueues.WorkQueuesRecv;
import com.hry.spring.rabbitmq.basic.workqueues.WorkQueuesSend;
import org.junit.Test;

import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * Created by huangrongyou@yixin.im on 2018/1/10.
 */
public class BasicTest {
    // 测试线程池
    private ExecutorService executorService = Executors.newFixedThreadPool(10);

    // rabbitmq的IP地址
    private final String rabbitmq_host = "10.240.80.147";
    // rabbitmq的用户名称
    private final String rabbitmq_user = "hry";
    // rabbitmq的用户密码
    private final String rabbitmq_pwd = "hry";

    @Test
    public void helloworld() throws InterruptedException {
        // 接收端
        executorService.submit(() -> {
            HelloworldRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
        });
        Thread.sleep(5* 100);

        // 发送端
        executorService.submit(() -> {
            HelloworldSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
        });
        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void workqueues() throws InterruptedException {
        // 接收端
        int recNum = 2;
        while(recNum-- > 0) {
            final int recId = recNum;
            executorService.submit(() -> {
                WorkQueuesRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, recId);
            });
        }
        Thread.sleep(5* 100);
        // 发送端
        int sendNum = 4;
        while(sendNum-- > 0){
            executorService.submit(() -> {
                WorkQueuesSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
            });
        }

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void publishsubscribe() throws InterruptedException {
        // 接收端
        int recNum = 2;
        while(recNum-- > 0) {
            final int recId = recNum;
            executorService.submit(() -> {
                Subscribe.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, recId);
            });
        }
        Thread.sleep(5* 100);
        // 发送端
        int sendNum = 2;
        while(sendNum-- > 0){
            executorService.submit(() -> {
                Publish.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
            });
        }

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void routing_1() throws InterruptedException {
        // 接收端 1：绑定 orange 值
        executorService.submit(() -> {
            String[] colours = {"orange"};
            RoutingRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, colours);
        });
        // 接收端 2：绑定 black、green 值
        executorService.submit(() -> {
            String[] colours = {"black","green"};
            RoutingRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, colours);
        });

        Thread.sleep(5* 100);
        // 发送端 ： 发送 black，只有接收端1收到
        executorService.submit(() -> {
            String routing = "orange";
            RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // 发送端 ： 发送 green、black，只有接收端2收到
        executorService.submit(() -> {
            String routing = "green";
            RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void routing_2() throws InterruptedException {

        // 接收端：同时创建两个接收端，同时绑定black
        int recNum = 2;
        while(recNum-- > 0) {
             // 接收端：绑定 black 值
            executorService.submit(() -> {
                String[] colours = {"black"};
                RoutingRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, colours);
            });
        }

        Thread.sleep(5* 100);
        // 发送端1 ： 发送 black，所有的接收端都会收到
        executorService.submit(() -> {
            String routing = "black";
            RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // 发送端2 ： 发送 green，所有的接收端都不会收到
        executorService.submit(() -> {
            String routing = "green";
            RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void topics() throws InterruptedException {

        // 消费者1：绑定 *.orange.* 值
        executorService.submit(() -> {
            String[] bindingKeys = {"*.orange.*"};
            TopicsRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, bindingKeys);
        });

        // 消费者2：绑定  "*.*.rabbit" 和 "lazy.#"值
        executorService.submit(() -> {
            String[] bindingKeys = {"*.*.rabbit", "lazy.#"};
            TopicsRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, bindingKeys);
        });

        Thread.sleep(5* 100);
        // 生产者1 ： 发送 black，所有的接收端都会收到
        executorService.submit(() -> {
            String routing = "quick.orange.rabbit";
            TopicsSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // 生产者2 ： 发送 green，所有的接收端都不会收到
        executorService.submit(() -> {
            String routing = "lazy.pink.rabbit";
            TopicsSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void rpc() throws InterruptedException {

        // rpc服务端
        executorService.submit(() -> {
            RpcServer.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
        });

        // rpc客户端
        executorService.submit(() -> {
            RpcClient.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, "rpc test");
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void header() throws InterruptedException {

        // 消费者1：绑定 format=pdf,type=report
        executorService.submit(() -> {
            Map<String,Object> headers = new HashMap();
            headers.put("format","pdf");
            headers.put("type","report");
            headers.put("x-match","all");
            HeaderRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, headers);
        });

        // 消费者2：绑定  format=pdf,type=log
        executorService.submit(() -> {
            Map<String,Object> headers = new HashMap();
            headers.put("format","pdf");
            headers.put("type","log");
            headers.put("x-match","any");
            HeaderRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, headers);
        });

        // 消费者3：绑定  format=zip,type=report
        executorService.submit(() -> {
            Map<String,Object> headers = new HashMap();
            headers.put("format","zip");
            headers.put("type","report");
            headers.put("x-match","all");
         //   headers.put("x-match","any");
            HeaderRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, headers);
        });

        Thread.sleep(2* 1000);
        System.out.println("=============消息1===================");
        // 生产者1 ： format=pdf,type=reprot,x-match=all
        executorService.submit(() -> {
            Map<String,Object> headers = new HashMap();
            headers.put("format","pdf");
            headers.put("type","report");
       //     headers.put("x-match","all");
            HeaderSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, headers);
        });

        Thread.sleep(5* 100);
        System.out.println("=============消息2===================");
        // 生产者2 ： format=pdf,x-match=any
        executorService.submit(() -> {
            Map<String,Object> headers = new HashMap();
            headers.put("format","pdf");
       //     headers.put("x-match","any");
            HeaderSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, headers);
        });

        Thread.sleep(5* 100);
        System.out.println("=============消息3===================");
        // 生产者1 ： format=zip,type=log,x-match=all
        executorService.submit(() -> {
            Map<String,Object> headers = new HashMap();
            headers.put("format","zip");
            headers.put("type","log");
      //      headers.put("x-match","all");
            HeaderSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, headers);
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }
}

```

__Fanout exchange（扇型交换机)__

扇型交换机（funout exchange）将消息路由给绑定到它身上的所有队列。不同于直连交换机，路由键在此类型上不启任务作用。如果N个队列绑定到某个扇型交换机上，当有消息发送给此扇型交换机时，交换机会将消息的发送给这所有的N个队列
   目的： 实现生产者发送一个消息，这个消息同时被传送给所有消费者。
    __生产者代码：__
Publish.java
```
1. 配置连接工厂
2. 建立TCP连接
3. 在TCP连接的基础上创建通道
4. 声明一个fanout交换机 (而不是声明一个队列)
5. 发送消息
这里表明我们不使用默认交换机，在消费端需要执行队列和交换机的绑定操作
```
声明交换机方法：
```
Exchange.DeclareOk exchangeDeclare(String exchange, String type, boolean durable, boolean autoDelete,
                                       Map<String, Object> arguments) throws IOException

```

详细参数：
```
第一个参数exchange：交换机的名称
第二个参数type：交换机的类型
第三个参数durable：是否持久化，如果true，则当前RabbitMQ重启的时候，它依旧存在
第四个参数autoDelete：当没有生成者/消费者使用此交换机时，此交换机会被自动删除。
第五个参数arguments：其它的扩展属性
```
代码如下：
```
// 声明一个fanout交换机
channel.exchangeDeclare(EXCHANGE_NAME,BuiltinExchangeType.FANOUT, false, false, null);
// 发送消息
channel.basicPublish(EXCHANGE_NAME, "", null, message.getBytes("UTF-8"));
```
Publish.java
```
package com.hry.spring.rabbitmq.basic.publishsubscribe;

import com.rabbitmq.client.BuiltinExchangeType;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.Channel;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

public class Publish {
    private static final String EXCHANGE_NAME = "logs";

    public static void execute(String host, String userName, String password) {
        // 配置连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost(host);
        // 需要在管理后台增加一个hry帐号
        factory.setUsername(userName);
        factory.setPassword(password);
        Connection connection = null;
        Channel channel = null;
        try {
            // 建立TCP连接
            connection = factory.newConnection();
            // 在TCP连接的基础上创建通道
            channel = connection.createChannel();
            // 声明一个fanout交换机
           // channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.FANOUT);
            channel.exchangeDeclare(EXCHANGE_NAME,BuiltinExchangeType.FANOUT, false, false, null);
            String message = "Publish-" + System.nanoTime();
            // 发送消息
            channel.basicPublish(EXCHANGE_NAME, "", null, message.getBytes("UTF-8"));
            System.out.println(" [Publish] Sent '" + message + "'");
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            try {
                // 空值判断，为了代码简洁略
                channel.close();
                connection.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```
消费者代码：
```
1. 配置连接工厂
2. 建立TCP连接
3. 在TCP连接的基础上创建通道
4. 声明一个fanout交换机 
5. 声明一个临时队列
6. 将临时队列绑定到交换机上
7. 接收消息并处理
这里表明我们不使用默认交换机，在消费端需要执行队列和交换机的绑定操作
```
临时队列：
在本例子中，我们使用临时队列。临时队列有以下特点：
1. 消费者每次连接时，都会创建一个新的队列，队列名称随机生成
2. 当消费者断开连接时，队列会自动变删除

在RabbitMQ中，随机使用的队列名称类似amq.gen-**

声明方法:
```
Queue.DeclareOk queueDeclare() throws IOException;  

此方法等价于创建一个
Queue.DeclareOk queueDeclare(String queue="自动生成类似amq.gen-******的随机名称", boolean durable=false, boolean exclusive=true, boolean autoDelete=true,  Map<String, Object> arguments=null) throws IOException;
```
代码：创建临时队列，得到队列名称，用于后面的绑定使用
```
String queueName = channel.queueDeclare().getQueue();
```
   __绑定(Bindings)__
在创建交换机、队列后，要实现将发送到特定的交换机X的消息路由到我们创建的队列，还需要做绑定操作。如果绑定成功后，交换机会将带特定路由键的消息路由到绑定的队列
方法：将队列绑定到交换机上
```
Queue.BindOk queueBind(String queue, String exchange, String routingKey) throws IOException;
```
详细参数如下：

    第一个参数queue：要绑定的队列
    第二个参数exchange：要绑定的交换机
    第三个参数routingKey：绑定使用的路由键

代码：将队列绑定到交换机上，因为我们这个demo是使用扇形交换机，routingKey是没有意义的，所以这里使用的routingKey为空字符串
```
String queueName = channel.queueDeclare().getQueue();
channel.queueBind(queueName, EXCHANGE_NAME, "");
```
Subscribe.java
```
package com.hry.spring.rabbitmq.basic.publishsubscribe;

import com.rabbitmq.client.*;

import java.io.IOException;

public class Subscribe {
    private static final String EXCHANGE_NAME = "logs";

    public static void execute(String host, String userName, String password, int id){
        // 配置连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost(host);
        // 需要在管理后台增加一个hry帐号
        factory.setUsername(userName);
        factory.setPassword(password);
        try {
            // 建立TCP连接
            Connection connection = factory.newConnection();
            // 在TCP连接的基础上创建通道
            Channel channel = connection.createChannel();
            // 声明一个fanout交换机
            channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.FANOUT);
            // 声明一个临时队列
            String queueName = channel.queueDeclare().getQueue();
            // 将临时队列绑定到交换机上
            channel.queueBind(queueName, EXCHANGE_NAME, "");

            System.out.println(" [Subscribe-"+id+"] Waiting for messages.");

            Consumer consumer = new DefaultConsumer(channel) {
                @Override
                public void handleDelivery(String consumerTag, Envelope envelope,
                                           AMQP.BasicProperties properties, byte[] body) throws IOException {
                    String message = new String(body, "UTF-8");
                    System.out.println(" [Subscribe-"+id+"] Received '" + message + "'");
                }
            };
            // 接收消费消息
            channel.basicConsume(queueName, true, consumer);
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```
测试代码：
```
public class BasicTest {
    // 测试线程池
    private ExecutorService executorService = Executors.newFixedThreadPool(10);

    // rabbitmq的IP地址
    private final String rabbitmq_host = "10.240.80.147";
    // rabbitmq的用户名称
    private final String rabbitmq_user = "hry";
    // rabbitmq的用户密码
    private final String rabbitmq_pwd = "hry";

    @Test
    public void publishsubscribe() throws InterruptedException {
        // 接收端
        int recNum = 2;
        while(recNum-- > 0) {
            final int recId = recNum;
            executorService.submit(() -> {
                Subscribe.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, recId);
            });
        }
        Thread.sleep(5* 100);
        // 发送端
        int sendNum = 2;
        while(sendNum-- > 0){
            executorService.submit(() -> {
                Publish.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
            });
        }

        // sleep 10s
        Thread.sleep(10 * 1000);
    }
```
执行测试，启动两个消费者，发送者发送2条记录，这些记录同时被2个消费者消费:
```
 [Subscribe-0] Waiting for messages.
 [Subscribe-1] Waiting for messages.
 // 发送两条记录
 [Publish] Sent 'Publish-366028801770803'
 [Publish] Sent 'Publish-366028803809517'
 // 消费者1收到两条记录
 [Subscribe-0] Received 'Publish-366028801770803'
 [Subscribe-0] Received 'Publish-366028803809517'
 // 消费者2收到两条记录
 [Subscribe-1] Received 'Publish-366028801770803'
 [Subscribe-1] Received 'Publish-366028803809517'
```

__Topic exchange（主题交换机）__

主题交换机（topic exchanges）中，队列通过路由键绑定到交换机上，然后，交换机根据消息里的路由值，将消息路由给一个或多个绑定队列。

扇型交换机和主题交换机异同：

    对于扇型交换机路由键是没有意义的，只要有消息，它都发送到它绑定的所有队列上
    对于主题交换机，路由规则由路由键决定，只有满足路由键的规则，消息才可以路由到对应的队列上


我们通过direct exchange（直连交换机）可以根据路由键进行路由，但是还是不够灵活，它只能进行完全匹配。这节我们引入Topic exchange(主题交换机)，支持对路由键的模糊匹配
上篇文章实现生产者发送一个消息，这个消息同时被传送给所有队列。但是有时我们不希望所有的消息都被所有队列接收，我们希望可以指定类型为a的消息只能被队列A接收，类型为b的消息只能被队列B,C接收。扇型交换机只能无脑地广播消息给所有的消费者，实质是广播给所有关联的队列。
为了实现这个功能，一种是建立多个交换机，这种方式简单暴力但是不灵活。本节我们介绍使用单个直连交换机+路由实现以上功能

路由键中特殊匹配字符说明

    *（星号）可以代替一个字。
    ＃（散列）可以代替零个或多个单词。

Topic Exchange的路由键条件：

    必须是由英文单词列表组成，单词之间使用”.”分隔
    路由键的长度最大255字节

    生产者代码

主要业务逻辑如下：

1. 配置连接工厂
2. 建立TCP连接
3. 在TCP连接的基础上创建通道
4. 声明一个topic交换机 
5. 发送消息，并配置消息的路由键

此代码和上一篇的代码基本相同，最大的不同是声明了topic交换机

// 声明一个topic交换机
```
channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.TOPIC);
```
TopicsSend.java
```
package com.hry.spring.rabbitmq.basic.topics;

import com.rabbitmq.client.BuiltinExchangeType;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.Channel;

public class TopicsSend {
    private static final String EXCHANGE_NAME = "topic_logs";

    public static void execute(String host, String userName, String password, String routingKey) {
        // 配置连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost(host);
        // 需要在管理后台增加一个hry帐号
        factory.setUsername(userName);
        factory.setPassword(password);

        Connection connection = null;
        Channel channel = null;
        try {
            // 建立TCP连接
            connection = factory.newConnection();
            // 在TCP连接的基础上创建通道
            channel = connection.createChannel();
            // 声明一个topic交换机
            channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.TOPIC);
            String message = "Topics-" + System.currentTimeMillis();
            // 发送消息，并配置消息的路由键
            channel.basicPublish(EXCHANGE_NAME, routingKey, null, message.getBytes("UTF-8"));
            System.out.println(" [HeaderSend] Sent '" + routingKey + "':'" + message + "'");
        }
        catch  (Exception e) {
            e.printStackTrace();
        }
        finally {
            if (connection != null) {
                try {
                    connection.close();
                }
                catch (Exception ignore) {}
            }
        }
    }

    private static String getRouting(String[] strings){
        if (strings.length < 1)
            return "anonymous.info";
        return strings[0];
    }

    private static String getMessage(String[] strings){
        if (strings.length < 2)
            return "Hello World!";
        return joinStrings(strings, " ", 1);
    }

    private static String joinStrings(String[] strings, String delimiter, int startIndex) {
        int length = strings.length;
        if (length == 0 ) return "";
        if (length < startIndex ) return "";
        StringBuilder words = new StringBuilder(strings[startIndex]);
        for (int i = startIndex + 1; i < length; i++) {
            words.append(delimiter).append(strings[i]);
        }
        return words.toString();
    }
}
```

接收者代码

主要业务逻辑如下：

1. 配置连接工厂
2. 建立TCP连接
3. 在TCP连接的基础上创建通道
4. 声明一个topic交换机 
5. 声明一个临时队列
6. 将临时队列绑定到交换机上，并在队列上绑定多个绑定值
7. 接收消息并处理


此代码和上一篇的代码基本相同，最大的不同是声明了topic交换机
```
// 声明一个topic交换机
channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.TOPIC);

```

TopicsRecv.java
```
package com.hry.spring.rabbitmq.basic.topics;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.Arrays;

public class TopicsRecv {
    private static final String EXCHANGE_NAME = "topic_logs";

    public static void execute(String host, String userName, String password, String[] bindingKeys){
        // 配置连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost(host);
        // 需要在管理后台增加一个hry帐号
        factory.setUsername(userName);
        factory.setPassword(password);

        Connection connection = null;
        Channel channel = null;
        try {
            // 建立TCP连接
            connection = factory.newConnection();
            // 在TCP连接的基础上创建通道
            channel = connection.createChannel();
            // 声明一个topic交换机
            channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.TOPIC);
            // 声明一个临时队列
            String queueName = channel.queueDeclare().getQueue();
            // 绑定路由，同一个队列可以绑定多个值
            for (String bindingKey : bindingKeys) {
                channel.queueBind(queueName, EXCHANGE_NAME, bindingKey);
            }

            System.out.println(" [HeaderRecv "+ Arrays.toString(bindingKeys)+"] Waiting for messages.");

            Consumer consumer = new DefaultConsumer(channel) {
                @Override
                public void handleDelivery(String consumerTag, Envelope envelope,
                                           AMQP.BasicProperties properties, byte[] body) throws IOException {
                    String message = new String(body, "UTF-8");
                    System.out.println(" [HeaderRecv "+ Arrays.toString(bindingKeys) + " ] Received '" + envelope.getRoutingKey() + "':'" + message + "'");
                }
            };
            // 接收消息
            channel.basicConsume(queueName, true, consumer);
        }catch (Exception e){
            e.printStackTrace();
        }finally {
        }
    }
}
```
__测试__
BasicTest
```
@Test
public void topics() throws InterruptedException {

   // 消费者1：绑定 *.orange.* 值
   executorService.submit(() -> {
       String[] bindingKeys = {"*.orange.*"};
       TopicsRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, bindingKeys);
   });

   // 消费者2：绑定  "*.*.rabbit" 和 "lazy.#"值
   executorService.submit(() -> {
       String[] bindingKeys = {"*.*.rabbit", "lazy.#"};
       TopicsRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, bindingKeys);
   });

   Thread.sleep(5* 100);
   // 生产者1 ： 发送 black，所有的接收端都会收到
   executorService.submit(() -> {
       String routing = "quick.orange.rabbit";
       TopicsSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
   });

   // 生产者2 ： 发送 green，所有的接收端都不会收到
   executorService.submit(() -> {
       String routing = "lazy.pink.rabbit";
       TopicsSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
   });

   // sleep 10s
   Thread.sleep(10 * 1000);
}
```
以上代码启动2个消费者，消费者1绑定”*.orange.*” ，消费者2个绑定”*.*.rabbit” 和 “lazy.#”;
启动2个生产者，生产者1发送消息的路由键为 “quick.orange.rabbit”，此消息同时被2个消费者接收
生产者2发送消息的路由键为”lazy.pink.rabbit”，此消息只被消费者2接收，符合之前的分析
```
// 启动2个消费者，消费者1绑定"*.orange.*" ，消费者2个绑定"*.*.rabbit" 和 "lazy.#"
[TopicsRecv [*.orange.*]] Waiting for messages.
[TopicsRecv [*.*.rabbit, lazy.#]] Waiting for messages.
// 生产者1发送消息的路由键为 "quick.orange.rabbit"，此消息同时被2个消费者接收
[TopicsSend] Sent 'quick.orange.rabbit':'Topics-1516008766983'
[TopicsRecv [*.*.rabbit, lazy.#] ] Received 'quick.orange.rabbit':'Topics-1516008766983'
[TopicsRecv [*.orange.*] ] Received 'quick.orange.rabbit':'Topics-1516008766983'
// 生产者2发送消息的路由键为"lazy.pink.rabbit"，此消息只被消费者2接收
[TopicsSend] Sent 'lazy.pink.rabbit':'Topics-1516008767012'
[TopicsRecv [*.*.rabbit, lazy.#] ] Received 'lazy.pink.rabbit':'Topics-1516008767012'
```
BasicTest.java
```
package com.hry.spring.rabbitmq.basic;

import com.hry.spring.rabbitmq.basic.header.HeaderRecv;
import com.hry.spring.rabbitmq.basic.header.HeaderSend;
import com.hry.spring.rabbitmq.basic.helloworld.HelloworldRecv;
import com.hry.spring.rabbitmq.basic.helloworld.HelloworldSend;
import com.hry.spring.rabbitmq.basic.publishsubscribe.Publish;
import com.hry.spring.rabbitmq.basic.publishsubscribe.Subscribe;
import com.hry.spring.rabbitmq.basic.routing.RoutingRecv;
import com.hry.spring.rabbitmq.basic.routing.RoutingSend;
import com.hry.spring.rabbitmq.basic.rpc.RpcClient;
import com.hry.spring.rabbitmq.basic.rpc.RpcServer;
import com.hry.spring.rabbitmq.basic.topics.TopicsRecv;
import com.hry.spring.rabbitmq.basic.topics.TopicsSend;
import com.hry.spring.rabbitmq.basic.workqueues.WorkQueuesRecv;
import com.hry.spring.rabbitmq.basic.workqueues.WorkQueuesSend;
import org.junit.Test;

import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * Created by huangrongyou@yixin.im on 2018/1/10.
 */
public class BasicTest {
    // 测试线程池
    private ExecutorService executorService = Executors.newFixedThreadPool(10);

    // rabbitmq的IP地址
    private final String rabbitmq_host = "10.240.80.147";
    // rabbitmq的用户名称
    private final String rabbitmq_user = "hry";
    // rabbitmq的用户密码
    private final String rabbitmq_pwd = "hry";

    @Test
    public void helloworld() throws InterruptedException {
        // 接收端
        executorService.submit(() -> {
            HelloworldRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
        });
        Thread.sleep(5* 100);

        // 发送端
        executorService.submit(() -> {
            HelloworldSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
        });
        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void workqueues() throws InterruptedException {
        // 接收端
        int recNum = 2;
        while(recNum-- > 0) {
            final int recId = recNum;
            executorService.submit(() -> {
                WorkQueuesRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, recId);
            });
        }
        Thread.sleep(5* 100);
        // 发送端
        int sendNum = 4;
        while(sendNum-- > 0){
            executorService.submit(() -> {
                WorkQueuesSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
            });
        }

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void publishsubscribe() throws InterruptedException {
        // 接收端
        int recNum = 2;
        while(recNum-- > 0) {
            final int recId = recNum;
            executorService.submit(() -> {
                Subscribe.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, recId);
            });
        }
        Thread.sleep(5* 100);
        // 发送端
        int sendNum = 2;
        while(sendNum-- > 0){
            executorService.submit(() -> {
                Publish.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
            });
        }

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void routing_1() throws InterruptedException {
        // 接收端 1：绑定 orange 值
        executorService.submit(() -> {
            String[] colours = {"orange"};
            RoutingRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, colours);
        });
        // 接收端 2：绑定 black、green 值
        executorService.submit(() -> {
            String[] colours = {"black","green"};
            RoutingRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, colours);
        });

        Thread.sleep(5* 100);
        // 发送端 ： 发送 black，只有接收端1收到
        executorService.submit(() -> {
            String routing = "orange";
            RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // 发送端 ： 发送 green、black，只有接收端2收到
        executorService.submit(() -> {
            String routing = "green";
            RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void routing_2() throws InterruptedException {

        // 接收端：同时创建两个接收端，同时绑定black
        int recNum = 2;
        while(recNum-- > 0) {
             // 接收端：绑定 black 值
            executorService.submit(() -> {
                String[] colours = {"black"};
                RoutingRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, colours);
            });
        }

        Thread.sleep(5* 100);
        // 发送端1 ： 发送 black，所有的接收端都会收到
        executorService.submit(() -> {
            String routing = "black";
            RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // 发送端2 ： 发送 green，所有的接收端都不会收到
        executorService.submit(() -> {
            String routing = "green";
            RoutingSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void topics() throws InterruptedException {

        // 消费者1：绑定 *.orange.* 值
        executorService.submit(() -> {
            String[] bindingKeys = {"*.orange.*"};
            TopicsRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, bindingKeys);
        });

        // 消费者2：绑定  "*.*.rabbit" 和 "lazy.#"值
        executorService.submit(() -> {
            String[] bindingKeys = {"*.*.rabbit", "lazy.#"};
            TopicsRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, bindingKeys);
        });

        Thread.sleep(5* 100);
        // 生产者1 ： 发送 black，所有的接收端都会收到
        executorService.submit(() -> {
            String routing = "quick.orange.rabbit";
            TopicsSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // 生产者2 ： 发送 green，所有的接收端都不会收到
        executorService.submit(() -> {
            String routing = "lazy.pink.rabbit";
            TopicsSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, routing);
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void rpc() throws InterruptedException {

        // rpc服务端
        executorService.submit(() -> {
            RpcServer.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd);
        });

        // rpc客户端
        executorService.submit(() -> {
            RpcClient.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, "rpc test");
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }

    @Test
    public void header() throws InterruptedException {

        // 消费者1：绑定 format=pdf,type=report
        executorService.submit(() -> {
            Map<String,Object> headers = new HashMap();
            headers.put("format","pdf");
            headers.put("type","report");
            headers.put("x-match","all");
            HeaderRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, headers);
        });

        // 消费者2：绑定  format=pdf,type=log
        executorService.submit(() -> {
            Map<String,Object> headers = new HashMap();
            headers.put("format","pdf");
            headers.put("type","log");
            headers.put("x-match","any");
            HeaderRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, headers);
        });

        // 消费者3：绑定  format=zip,type=report
        executorService.submit(() -> {
            Map<String,Object> headers = new HashMap();
            headers.put("format","zip");
            headers.put("type","report");
            headers.put("x-match","all");
         //   headers.put("x-match","any");
            HeaderRecv.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, headers);
        });

        Thread.sleep(2* 1000);
        System.out.println("=============消息1===================");
        // 生产者1 ： format=pdf,type=reprot,x-match=all
        executorService.submit(() -> {
            Map<String,Object> headers = new HashMap();
            headers.put("format","pdf");
            headers.put("type","report");
       //     headers.put("x-match","all");
            HeaderSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, headers);
        });

        Thread.sleep(5* 100);
        System.out.println("=============消息2===================");
        // 生产者2 ： format=pdf,x-match=any
        executorService.submit(() -> {
            Map<String,Object> headers = new HashMap();
            headers.put("format","pdf");
       //     headers.put("x-match","any");
            HeaderSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, headers);
        });

        Thread.sleep(5* 100);
        System.out.println("=============消息3===================");
        // 生产者1 ： format=zip,type=log,x-match=all
        executorService.submit(() -> {
            Map<String,Object> headers = new HashMap();
            headers.put("format","zip");
            headers.put("type","log");
      //      headers.put("x-match","all");
            HeaderSend.execute(rabbitmq_host, rabbitmq_user, rabbitmq_pwd, headers);
        });

        // sleep 10s
        Thread.sleep(10 * 1000);
    }
}
```

__Headers exchange)(头交换机)__

类似主题交换机，但是头交换机使用多个消息属性来代替路由键建立路由规则。通过判断消息头的值能否与指定的绑定相匹配来确立路由规则。
此交换机有个重要参数：”x-match”

    当”x-match”为“any”时，消息头的任意一个值被匹配就可以满足条件
    当”x-match”设置为“all”的时候，就需要消息头的所有值都匹配成功
RabbitMQ默认定义一些交换机

在RabbitMQ默认定义一些交换机，主要如下：

__默认交换机__
默认交换机（default exchange）实际上是一个由RabbitMQ预先声明好的名字为空字符串的直连交换机（direct exchange）。它有一个特殊的属性使得它对于简单应用特别有用处：那就是每个新建队列（queue）都会自动绑定到默认交换机上，绑定的路由键（routing key）名称与队列名称相同。

如：当你声明了一个名为”hello”的队列，RabbitMQ会自动将其绑定到默认交换机上，绑定（binding）的路由键名称也是为”hello”。因此，当携带着名为”hello”的路由键的消息被发送到默认交换机的时候，此消息会被默认交换机路由至名为”hello”的队列中。即默认交换机看起来貌似能够直接将消息投递给队列.
__Dead Letter Exchange（死信交换机）__

在默认情况，如果消息在投递到交换机时，交换机发现此消息没有匹配的队列，则这个消息将被悄悄丢弃。为了解决这个问题，RabbitMQ中有一种交换机叫死信交换机。当消费者不能处理接收到的消息时，将这个消息重新发布到另外一个队列中，等待重试或者人工干预。这个过程中的exchange和queue就是所谓的”Dead Letter Exchange 和 Queue”

__交换机的属性__

除交换机类型外，在声明交换机时还可以附带许多其他的属性，其中最重要的几个分别是：

    Name：交换机名称
    Durability：是否持久化。如果持久性，则RabbitMQ重启后，交换机还存在
    Auto-delete：当所有与之绑定的消息队列都完成了对此交换机的使用后，删掉它
    Arguments：扩展参数


