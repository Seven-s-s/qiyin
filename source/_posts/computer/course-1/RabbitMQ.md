---
title: RabbitMQ
date: 2020/12/25
categories:
	- [计算机,框架]
tags:
	- RabbitMQ
---

# 安装RabbitMQ

1. 下载

> erlang-solutions-1.0-1.noarch.rpm,socat-1.7.3.3-2.el8.x86_64.rpm,rabbitmq-server-3.8.8-1.el8.noarch

2. 执行

> rpm -Uvh erlang-solutions-1.0-1.noarch.rpm

3. 出现epel-release问题执行以下命令

> yum -y install epel-release再执行上条命令

4. 安装

> sudo yum install erlang

5. 检验 

> erl

6. 查看安装路径 

> whereis erlang

7. 安装配置包 

> rpm – ivh socat-1.7.3.3-2.el8.x86_64.rpm

8. 安装rabbitmq

> rpm -ivh rabbitmq-server-3.8.8-1.el8.noarch

9. 修改配置文件

> 将配置文件rabbitmq-config-example复制到/etc/rabbitmq/rabbitmq.config下，并且修改配置文件，将%%{loopback_users, []}的注释取消掉

10. 启动rabbitmq的插件管理

> rabbitmq-plugins enable rabbitmq_management

11. rabbitmq

> 启动systemctl start rabbitmq-server
>
> 重启systemctl restart rabbitmq-server
>
> 停止systemctl stoprabbitmq-server
>
> 检查状态systemctl status rabbitmq-server

如果启动出现ERROR: epmd error for host 192: badarg (unknown POSIX error) 编辑：vi /etc/rabbitmq/rabbitmq-env.conf 添加一行NODENAME=rabbit@localhost

12. 关闭防火墙

> systemctl stop firefalld

13. 访问IP:15672

> 账号：guest
>
> 密码：guest

# 管理命令行

1. 启动服务相关

> systemctl start | restart | stop | status rabbotmq-server

2. 管理命令行 用来在不使用web管理界面情况下命令操作RabbitMQ

> Rabbitmqctl help 可以查看更多命令

3. 插件管理命令行

> Rabbitmqplugins enable | list | disable

# 搭建集群

## 普通集群

1. 克隆三台机器主机名和IP映射

> Vim /etc/hosts 加入：
>
> ​    ip1：mq1
>
> ​    ip2：mq2
>
> ​    ip3：mq3
>
>  node1: vim /etc/hostname 加入：mq1
>
>  node2: vim /etc/hostname 加入：mq2
>
>  node3: vim /etc/hostname 加入：mq3

2. 三个机器安装rabbitmq，并同步cokkie文件，在node1上执行

> scp  /var/lib/rabbitmq/.erlang.cookie root@mq2:/var/lib/rabbitmq/
>
> scp  /var/lib/rabbitmq/.erlang.cookie root@mq3:/var/lib/rabbitmq/

3. 查看cookie是否一致

> node1:  cat /var/lib/rabbitmq/.erlang.cookie
>
> node2:  cat /var/lib/rabbitmq/.erlang.cookie
>
> node3:  cat /var/lib/rabbitmq/.erlang.cookie

4. 后台启动rabbitmq所有节点执行如下命令，启动成功后访问管理界面

> rabbitmq-server -detached

5. 在node2和node3执行加入集群命令

* 关闭 

   rabbitmqctl stop_app

* 加入集群 

   rabbitmqctl join_cluster rabbit@mq1

* 启动服务

   rabbitmqctl start_app

6. 查看集群状态，任意节点执行

> rabbitmqctl cluster_status

7. 如果出现以下显示，集群搭建成功

> [{nodes,[{disc,[rabbit@mq1,rabbit@mq2,rabbit@mq3]}]}],…

8. 登录管理界面，在overview页面下面的nodes有三个mq

## 镜像集群

策略说明

> Rabbitmqctl set_policy [-p <vhost>] [--priority <priority>] [--apply-to <apply-to>] <name> <pattern> <definition>
>
> -p Vhost：可选参数，针对指定的vhost下的queue进行设置
>
> Priority：可选参数，policy的优先级
>
> Name：policy的名称
>
> Pattren：queue的匹配模式(正则表达式)
>
> Definition：镜像定义，包括三个部分ha-mode，ha-params，ha-sync-mode.
>
> * Ha-mode：指明镜像队列的模式，有效值为all/exactly/nodes
>
>   * All:表示在集群中所有的节点上进行镜像
>
>   * Exactly:表示在指定个数的节点上进行镜像，节点的个数由ha-params指定
>
>   * Nodes：表示在指定的节点上进行镜像，节点名称通过ha-params指定
>
> * Ha-params：ha-mode模式需要用到的参数
>
> * Ha-sync-mode：进行队列中消息的同步方式，有效值为automatic和manual

1. 查看当前策略

> Rabbitmqctl list_policies

2. 添加策略

> Rabbitmqctl set_policy ha-all ‘^hello’ ‘{“ha-mode”:”all”,”ha-sync-mode”:”sutomatic”}’
>
> 说明：策略正则表达式为 ”^”：匹配所有队列, ^hello：匹配jello开头队列

3. 删除策略

> Rabbitmq clear_policy ha-all

# Java实现RabbitMQ

## 搭建项目

1. 启动RabbitMQ
2. 进入管理界面，在Admin下面添加一个User

![1](/assets/RabbitMQ.assets/1.png)

3. 添加一个Virtual Hosts

![2](/assets/RabbitMQ.assets/2.png)

4. 设置权限

![3](/assets/RabbitMQ.assets/3.png)

![4](/assets/RabbitMQ.assets/4.png)

5. 新建一个Maven项目

6. 导入pom依赖

```xml
<!--junit测试-->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.11</version>
</dependency>
<!--rabbitmq-->
<dependency>
    <groupId>com.rabbitmq</groupId>
    <artifactId>amqp-client</artifactId>
    <version>5.8.0</version>
</dependency>
```

## 简单队列

![helloworld](/assets/RabbitMQ.assets/helloworld.png)

1. 创建一个生产者Provider类

```java
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import org.junit.Test;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

public class Provider {

    //生产消息
    @Test
    public void sendMessage() throws IOException, TimeoutException {
        //创建连接mq的连接工厂对象
        ConnectionFactory connectionFactory = new ConnectionFactory();
        //设置连接rabbitmq主机
        connectionFactory.setHost("主机ip");
        //设置端口号
        connectionFactory.setPort(5672);
        //设置连接那个虚拟主机
        connectionFactory.setVirtualHost("/qiyin");
        //设置访问虚拟主机的用户和密码
        connectionFactory.setUsername("qiyin");
        connectionFactory.setPassword("qiyin");

        //获取连接对象
        Connection connection = connectionFactory.newConnection();

        //获取连接中的通道
        Channel channel = connection.createChannel();
        //通道绑定对应的消息队列
        //参数1:队列名称 如果队列不存在就自动创建
        //参数2:用来定义队列特性是否持久化
        //参数3:是否独占队列
        //参数4:是否在消费完成后自动删除队列
        //参数5:额外附加参数
        channel.queueDeclare("hello", false, false, false, null);

        //发布消息
        //参数1:交换机名称 参数2:队列名称 参数3:传递消息额外设置 参数4:消息的具体内容
        channel.basicPublish("","hello",null,"hello rabbitmq".getBytes());

        channel.close();
        connection.close();
    }
}
```

> 运行之后在管理界面Queues有一条消息

![5](/assets/RabbitMQ.assets/5.png)

2. 创建一个消费者Consumer类

```java
import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

public class Consumer {
    public static void main(String[] args) throws IOException, TimeoutException {
        //创建连接mq的连接工厂对象
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("192.168.78.128");
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("/qiyin");
        connectionFactory.setUsername("qiyin");
        connectionFactory.setPassword("qiyin");

        //获取连接对象
        Connection connection = connectionFactory.newConnection();

        //获取连接中通道
        Channel channel = connection.createChannel();

        //通道绑定消息队列
        channel.queueDeclare("hello", false, false, false, null);

        //消费消息
        //参数1:消费那个队列的消息 消息名称
        //参数2:开始消息的自动确认机制
        //参数3:消费时的回调函数
        channel.basicConsume("hello", true, new DefaultConsumer(channel) {
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("----" + new String(body) + "----");
            }
        });
    }
}
```

> 运行之后，消息被消费
>
> 输出----hello rabbitmq----

![6](/assets/RabbitMQ.assets/6.png)

## work模式

![work](/assets/RabbitMQ.assets/work.png)

1. 创建一个连接关闭工具类

```java
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

public class RabbitMQUtils {

    private static ConnectionFactory connectionFactory;

    static {
        connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("主机ip");
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("/qiyin");
        connectionFactory.setUsername("qiyin");
        connectionFactory.setPassword("qiyin");
    }

    public static Connection getConnection(){
        try {
            return connectionFactory.newConnection();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }

    public static void closeConnectionAndChannel(Channel channel,Connection connection){
        try {
            if (channel != null) channel.close();
            if (connection != null) connection.close();
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

2. 创建一个生产者

```java
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;

import java.io.IOException;

public class Provider {
    public static void main(String[] args) throws IOException {
        //获取连接
        Connection connection = RabbitMQUtils.getConnection();
        //获取通道
        Channel channel = connection.createChannel();
        channel.queueDeclare("work",true,false,false,null);
        for (int i = 1; i <= 20; i++) {
            channel.basicPublish("","work",null,(i+" hello work queue").getBytes());
        }
        RabbitMQUtils.closeConnectionAndChannel(channel,connection);
    }
}
```

3. 创建两个消费者

```java
import com.rabbitmq.client.*;

import java.io.IOException;

public class Consumer_1 {
    public static void main(String[] args) throws IOException {
        Connection connection = RabbitMQUtils.getConnection();
        Channel channel = connection.createChannel();
        channel.queueDeclare("work",true,false,false,null);
        //第二个参数自动确认机制，设置为true，每个消费者都会收到相同数量的消息
        channel.basicConsume("work",true, new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println(new String(body));
            }
        });
    }
}
```

```java
import com.rabbitmq.client.*;

import java.io.IOException;

public class Consumer_2 {
    public static void main(String[] args) throws IOException {
        Connection connection = RabbitMQUtils.getConnection();
        Channel channel = connection.createChannel();
        channel.queueDeclare("work",true,false,false,null);
        channel.basicConsume("work",true,new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println(new String(body));
            }
        });
    }
}
```

> 先将两个消费者运行起来，然后在运行生产者
>
> 消费者1运行结果
> 1 hello work queue
> 3 hello work queue
> 5 hello work queue
> ...
> 19 hello work queue
>
> 消费者2运行结果
> 2 hello work queue
> 4 hello work queue
> 6 hello work queue
> ...
> 20 hello work queue

4.再创建两个消费者

```java
import com.rabbitmq.client.*;

import java.io.IOException;

public class Consumer_3 {
    public static void main(String[] args) throws IOException {
        Connection connection = RabbitMQUtils.getConnection();
        final Channel channel = connection.createChannel();
        //一次只接受一条未确认的消息
        channel.basicQos(1);
        channel.queueDeclare("work",true,false,false,null);
        //参数2:关闭自动确认机制
        channel.basicConsume("work",false, new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(new String(body));
                //手动确认消息
                channel.basicAck(envelope.getDeliveryTag(),false);
            }
        });
    }
}
```

```java
import com.rabbitmq.client.*;

import java.io.IOException;

public class Consumer_4 {
    public static void main(String[] args) throws IOException {
        Connection connection = RabbitMQUtils.getConnection();
        final Channel channel = connection.createChannel();
        channel.basicQos(1);
        channel.queueDeclare("work",true,false,false,null);
        channel.basicConsume("work",false,new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                try {
                    Thread.sleep(400);
                }catch (Exception e){
                    e.printStackTrace();
                }
                System.out.println(new String(body));
                channel.basicAck(envelope.getDeliveryTag(),false);
            }
        });
    }
}
```

> 先将两个消费者运行起来，然后在运行生产者
>
> 消费者3运行结果
> 1 hello work queue
> 5 hello work queue
> 8 hello work queue
> 12 hello work queue
> 15 hello work queue
> 19 hello work queue
>
> 消费者4运行结果
> 2 hello work queue
> 3 hello work queue
> 4 hello work queue
> 6 hello work queue
> 7 hello work queue
> 9 hello work queue
> 10 hello work queue
> 11 hello work queue
> 13 hello work queue
> 14 hello work queue
> 16 hello work queue
> 17 hello work queue
> 18 hello work queue
> 20 hello work queue

## 订阅模式

![fanout](/assets/RabbitMQ.assets/fanout.png)

1. 创建一个生产者

```java
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;

import java.io.IOException;

public class Provider {
    public static void main(String[] args) throws IOException {
        Connection connection = RabbitMQUtils.getConnection();
        Channel channel = connection.createChannel();
        //声明交换机
        //参数1:交换机的名称 参数2:交换机的类型 fanout 广播类型
        channel.exchangeDeclare("logs","fanout");
        //发生资源
        channel.basicPublish("logs","",null,"fanout type message".getBytes());
        //释放资源
        RabbitMQUtils.closeConnectionAndChannel(channel,connection);
    }
}
```

2. 创建三个消费者

```java
import com.rabbitmq.client.*;

import java.io.IOException;

public class Consumer_1 {
    public static void main(String[] args) throws IOException {
        //获取连接对象
        Connection connection = RabbitMQUtils.getConnection();
        Channel channel = connection.createChannel();

        //通道绑定交换机
        channel.exchangeDeclare("logs","fanout");

        //临时队列
        String queue = channel.queueDeclare().getQueue();

        //绑定交换机和队列
        channel.queueBind(queue,"logs","");

        //消费消息
        channel.basicConsume(queue,true,new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("消费者-1："+new String(body));
            }
        });
    }
}
```

```java
import com.rabbitmq.client.*;

import java.io.IOException;

public class Consumer_2 {
    public static void main(String[] args) throws IOException {
        ...
        //消费消息
        channel.basicConsume(queue,true,new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("消费者-2："+new String(body));
            }
        });
    }
}
```

```java
import com.rabbitmq.client.*;

import java.io.IOException;

public class Consumer_3 {
    public static void main(String[] args) throws IOException {
        ...
        //消费消息
        channel.basicConsume(queue,true,new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("消费者-3："+new String(body));
            }
        });
    }
}
```

3. 运行三个消费者，再运行生产者

> 消费者1运行结果
> 消费者-1：fanout type message
>
> 消费者2运行结果
> 消费者-2：fanout type message
>
> 消费者3运行结果
> 消费者-3：fanout type message

## 路由模式

![direct](/assets/RabbitMQ.assets/direct.png)

1. 创建一个生产者

```java
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;

import java.io.IOException;

public class Provider {
    public static void main(String[] args) throws IOException {
        //获取连接
        Connection connection = RabbitMQUtils.getConnection();
        Channel channel = connection.createChannel();

        //交换机名称
        String exchangeName = "logs_direct";

        //通过通道声明交换机
        //参数1:交换机名称 参数2:direct 路由模式
        channel.exchangeDeclare(exchangeName,"direct");

        //发送消息
        String routingkey = "info";
        channel.basicPublish(exchangeName,routingkey,null,("这是direct模型发布的基于rout key:"+routingkey+"的消息").getBytes());

        //关闭资源
        RabbitMQUtils.closeConnectionAndChannel(channel,connection);
    }
}
```

2. 创建两个消费者

```java
import com.rabbitmq.client.*;

import java.io.IOException;

public class Consumer_1 {
    public static void main(String[] args) throws IOException {
        //获取连接
        Connection connection = RabbitMQUtils.getConnection();
        Channel channel = connection.createChannel();

        //通过通道声明交换机以及交换机的类型
        channel.exchangeDeclare("log_direct","direct");

        //创建一个临时队列
        String queue = channel.queueDeclare().getQueue();

        //基于rout key绑定交换机和队列
        channel.queueBind(queue,"logs_direct","error");

        //获取消费的消息
        channel.basicConsume(queue,true,new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("消费者-1："+new String(body));
            }
        });

    }
}
```

```java
import com.rabbitmq.client.*;

import java.io.IOException;

public class Consumer_2 {
    public static void main(String[] args) throws IOException {
        //获取连接
        Connection connection = RabbitMQUtils.getConnection();
        Channel channel = connection.createChannel();

        //定义交换机名称
        String exchangeName = "logs_direct";

        //通道声明交换机的名称和类型
        channel.exchangeDeclare(exchangeName,"direct");

        //创建一个临时队列
        String queue = channel.queueDeclare().getQueue();

        //基于rout key绑定交换机和队列
        channel.queueBind(queue,exchangeName,"info");
        channel.queueBind(queue,exchangeName,"error");
        channel.queueBind(queue,exchangeName,"warning");

        //获取消费的消息
        channel.basicConsume(queue,true,new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("消费者2："+new String(body));
            }
        });
    }
}
```

3. 运行两个消费者，运行生产者，将routingkey改为error，再运行一次生产者

> 消费者1输出结果
> 消费者-1：这是direct模型发布的基于rout key:error的消息
>
> 消费者2输出消息
> 消费者2：这是direct模型发布的基于rout key:info的消息
> 消费者2：这是direct模型发布的基于rout key:error的消息

## 主题模式

![topic](/assets/RabbitMQ.assets/topic.png)

1. 创建一个生产者

```java
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;

import java.io.IOException;

public class Provider {
    public static void main(String[] args) throws IOException {
        //创建连接
        Connection connection = RabbitMQUtils.getConnection();
        Channel channel = connection.createChannel();

        //声明交换机以及交换机类型 topic 动态路由
        channel.exchangeDeclare("topics","topic");

        //发布消息
        String routekey = "user";
        channel.basicPublish("topics",routekey,null,("这里是topic动态路由模型,routekey："+routekey+"的消息").getBytes());

        //释放资源
        RabbitMQUtils.closeConnectionAndChannel(channel,connection);
    }
}
```

2. 创建两个消费者

```java
import com.rabbitmq.client.*;

import java.io.IOException;

public class Consumer_1 {
    public static void main(String[] args) throws IOException {
        //创建连接
        Connection connection = RabbitMQUtils.getConnection();
        Channel channel = connection.createChannel();

        //声明交换机的名称和类型
        channel.exchangeDeclare("topics","topic");

        //定义一个临时的队列
        String queue = channel.queueDeclare().getQueue();

        //绑定交换机和队列
        // * 匹配一个
        // # 匹配零个或多个
        channel.queueBind(queue,"topics","user.*");

        //消费消息
        channel.basicConsume(queue,true,new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("消费者1："+new String(body));
            }
        });
    }
}
```

```java
import com.rabbitmq.client.*;

import java.io.IOException;

public class Consumer_2 {
    public static void main(String[] args) throws IOException {
    	...
        //绑定队列和交换机
        channel.queueBind(queue,"topics","user.#");

        //消费消息
        channel.basicConsume(queue,true,new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("消费者2："+new String(body));
            }
        });
    }
}
```

3. 运行两个消费者，运行生产者，将routekey改为user.user，再运行一次生产者

> 消费者1输出结果
> 消费者1：这里是topic动态路由模型,routekey：user.user的消息
>
> 消费者2输出结果
> 消费者2：这里是topic动态路由模型,routekey：user的消息
> 消费者2：这里是topic动态路由模型,routekey：user.user的消息

# SpringBoot实现RabbitMQ

...