# 笔记

## 1. 过期时间TTL

### 1.1 概念

过期时间TTL表示可以对**消息**设置预期的时间,在这个时间内都可以被消费者接收获取;过了之后消息将自动被删除,RabbitMQ可以通过**消息和队列**设置TTL;

目前有两种方法可以设置:

1. 第一种通过队列属性设置,队列中所有消息都是相同的过期时间
2. 第二种是对消息进行单独设置,每条消息TTL可以不同

**注意:**如果上述两种方法同时使用,则消息的过期的过期时间则以两者之间TTL较小的那个数值为准。

**注意**：消息在队列的生存时间一旦超过设置的TTL值，就称为dead Message被投递到死信队列,消息将无法再收到该消息



### 1.2 设置队列TTL

我们通过代码的方式设置队列中消息的过期时间

```java
@Component
public class OrderService {
    @Autowired
    private RabbitTemplate rabbitTemplate;

    public void makeOrder() {
        String orderId = UUID.randomUUID().toString();
        String exchangeName = "ttl_direct_order_exchange";
        String routingKey = "ttl";
        rabbitTemplate.convertAndSend(exchangeName, routingKey, orderId);
    }
}

//配置类:
Configuration
public class TTLRabbitMqConfig {
    @Bean
    public DirectExchange fanoutExchange() {
        return new DirectExchange("ttl_direct_order_exchange", true, false);
    }

    /**
     * 队列的过期时间
     *
     * @return {@link Queue}
     */
    @Bean
    @SuppressWarnings("all")
    public Queue ttlQueue() {
        //设置过期时间
        Map<String, Object> args = new HashMap<>();
        //设置过期时间为5秒,注意这里一定是一个int类型,如果是字符串那一定会报错
        args.put("x-message-ttl", 5000);
        return new Queue("ttl.direct.queue", true,false,false,args);
    }

    @Bean
    public Binding smsBinding() {
        return BindingBuilder.bind(ttlQueue()).to(fanoutExchange()).with("ttl");
    }
}
```

此时我们运行测试一下:

```java
@SpringBootTest
class SpringBootMqTtlApplicationTests {
    @Autowired
    private OrderService orderService;
    
    @Test
    void contextLoads() {
        orderService.makeOrder();
    }
}
```

此时我们去图形化界面查看:

![image-20211026154113214](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211026154113214.png)

这里就绪消息为0是因为我已经过了5秒钟了;

**注意**:参数 x-message-ttl 的值 必须是非负 32 位整数 (0 <= n <= 2^32-1) ，以毫秒为单位表示 TTL 的值。这样，值 6000 表示存在于 队列 中的当前 消息 将最多只存活 6 秒钟。

当然我们也可以点击这个队列详细信息里面去查看:

![image-20211026154206641](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211026154206641.png)

思考?我第一次声明创建队列设置了过期时间有标识,那么下次我往队列投递消息的时候用代码的方式不设置过期时间那么这个消息会不会过期呢?

答案是会发生异常信息,这是我修改的代码:

```java
@Bean
    @SuppressWarnings("all")
    public Queue ttlQueue() {
        //设置过期时间
        Map<String, Object> args = new HashMap<>();
        //设置过期时间为5秒,注意这里一定是一个int类型,如果是字符串那一定会报错
        args.put("x-message-ttl", 5000);
        return new Queue("ttl.direct.queue", true, false, false); //注意我没加过期时间参数
    }
```

然后此时看控制台:

![image-20211026154712417](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211026154712417.png)

说明了必须要设置过期时间并且必须跟第一次设置队列的过期时间保持一致,否则就会报错!也就是说队列一旦被创建如果你再次去修改添加参数它是不会去覆盖变更的而是会报错!!

> 注意我们设置过期时间的参数名不是乱取的,而是人家定义好的,这点要记住!并且设置队列TTL就表示了在这里面的消息都会在指定时间以后被移除!当然这也有一些应用场景,比如用户下单,如果用户在20分钟以内都没支付那么就会将订单进行移除,但是不是真正意义上移除而是通过死信队列来接收,这样就不会因为消息的过期而丢失而出现的隐患!

在图形化界面上在这里设置:

![image-20211026171024803](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211026171024803.png)

### 1.3 设置消息TTL

消息的过期时间只需要在发送消息(可以发送到任何队列,不管该队列是否属于某个交换机)的时候设置过期时间即可;

在测试类中编写如下方法发送消息并设置过期时间到队列:

```java
/**
     * ttl消息过期时间
     */
public void ttlMessage() {
    String orderId = UUID.randomUUID().toString();
    String exchangeName = "ttl_direct_order_exchange";
    String routingKey = "ttlMessage";

    //给消息设置过期时间
    MessagePostProcessor messageProcessor = new MessagePostProcessor() {
        @Override
        public Message postProcessMessage(Message message) throws AmqpException {
            //注意这里是字符串
            //设置过期时间
            message.getMessageProperties().setExpiration("5000");
            //还可以设置消息编码
            message.getMessageProperties().setContentEncoding("UTF-8");
            return message;
        }
    };
    //我们只需要将messageProcessor放入到后面进行方法的重载
    rabbitTemplate.convertAndSend(exchangeName, routingKey, orderId,messageProcessor);
}
```

然后我们看页面:

这是投递到消息后的队列:

![image-20211026161538698](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211026161538698.png)

这是过了5毫秒后的队列:

![image-20211026161625055](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211026161625055.png)

**注意**:`expiration` 字段以微秒为单位表示 TTL 值。且与 x-message-ttl 具有相同的约束条件。因为 expiration 字段必须为字符串类型，broker 将只会接受以字符串形式表达的数字;当同时指定了 queue 和 message 的 TTL 值，则两者中较小的那个才会起作用。

> 可以发现已经自动移除了这个过期消息;

总结一下两者的差异:

我们可以发现一个是过期队列一个是非过期队列,而如果我们给消息设置过期时间那么一旦到期就直接被移除了,而如果我们给队列设置过期时间,那么在未来会有一个死信队列,就是过期队列的消息就会把它过期后写入到死信队列中,而给消息设置过期是不会写入死信队列!



## 2. 死信队列(面)

### 2.1 概念:

​	DLX,全称为Dead-Letter-Exchange,可以称之为`死信交换机`,也有人称之为死信邮箱,当消息在一个队列中变成死信(dead message)之后,它能被重新发送到另一个交换机中,这个交换机就是DLX,绑定DLX的队列就称之为死信队列!

消息变成死信,可能是由于以下的原因(这也是一个面试题:"**什么样子的情况下的消息会进入到死信队列?**"):

1. 消息被拒绝
2. 消息过期(目前记住这个即可)
3. 队列达到最大长度

DLX也是一个正常的交换机,和一般的交换就没有区别,它能在任何的队列上被指定,实际上就是设置某一个队列的属性,当这个队列中存在死信时,Rabbitmq就会自动地将这个消息重新发布到设置的DLX交换机上去,进而被路由到另一个队列,即死信队列!

要想使用死信队列,只需要在定义队列的时候设置队列参数,`x-dead-letter-exchange`指定交换机即可!

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy95eb209a-1bcd-487b-832a-e09d88da3beb.png)

**注意**:当然使用死信队列前提得是设置队列TTL,因为只有过期队列里面的消息过期才会被写入到死信队列中,而普通队列中消息过期是直接被移除不会进行这样的操作的! 

**解释:**DLX是一个交换机,但它终究是一个"备胎",也就是人家消息变成死信那么就会被他进行接盘!

 我有一个生产者,按照正常的流程将消息投递到我们的交换机里面,而在交换机里面就会绑定一个队列,我们给队列设置过期时间让其中消息都在5秒内过期,一旦过期就会把这个消息队列进行清空,那么接下来清空的消息交给谁呢?那么这个"接盘侠"来了也就是死信队列,当然我们知道队列不可能直接承载接收消息的,所以会将消息转移到死信交换机里面,而交换机就会把消息投递到死信队列中;

### 2.2 代码演示

1. 首先定义一个死信交换机和死信队列,类型随意

   ```java
   @Configuration
   public class DealRabbitMqConfig {
   
       //声明一个死信交换机
       @Bean
       public DirectExchange deadExchange() {
           return new DirectExchange("dead_direct_exchange", true, false);
       }
   
       /**
        * 这是一个死信队列,
        * 注意:如果消息过期了那么接下来就会把这个消息投递到我的这个交换机里面,而交换机就会把这个消息转发死信队列中去
        * 要记住消息之间的传递就是一个交换机的一个发送过程
        *
        * @return {@link Queue}
        */
       @Bean
       public Queue dealQueue() {
           return new Queue("dead.direct.queue", true);
       }
   
       @Bean
       public Binding deadBinding() {
           return BindingBuilder.bind(dealQueue()).to(deadExchange()).with("dead");
       }
   }
   ```

   从图中我们可以知道这里定义了一个正常的direct模式的交换机和一个队列,然后进行两者绑定配置了一个路由key,看起来没有什么好高大上的感觉!

2. 在设置过期的队列里设置"接盘侠"

   ```java
   /**
        * 队列的过期时间
        *
        * @return {@link Queue}
        */
       @Bean
       @SuppressWarnings("all")
       public Queue ttlQueue() {
           //设置过期时间
           Map<String, Object> args = new HashMap<>();
           //设置过期时间为5秒,注意这里一定是一个int类型,如果是字符串那一定会报错
           args.put("x-message-ttl", 5000);
           //设置死信交换机和队列,在消息到期后转移到指定的交换机中
           args.put("x-dead-letter-exchange", "dead_direct_exchange");
           //因为死信交换机设置的direct模式的,所以需要路由key,当然如果是fanout模式的,那么这里就不需要配置路由key!
           args.put("x-dead-letter-routing-key", "dead");
           return new Queue("ttl.direct.queue", true, false, false,args);
   }
   ```

   这个就是在个队列里面所有的消息在5秒过期了就会转移到刚刚上面定义的死信交换机里,然后被转发到死信队列里进行存储!

3. 我们运行测试给这个过期队列对应的交换机里发送消息,看看会发生什么?

   ![image-20211026173353495](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211026173353495.png)

这是死信交换机:

![image-20211026175049332](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211026175049332.png)

### 2.3 小结

上面介绍了消息过期后被转移到死信队列,那么还要一种失效场景就是`消息数量达到最大数量`,比如我们设置参数`x-max-length`,比如设置5,那么在第六次往这个队列投递消息那么就会被将之前的5个一起转移到死信队列中去!那么在原来的过期队列上可以看见清晰的标识`Lim"`,如:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudye74b63ff-ce96-4f09-8df4-a0ff45701375.png)

**总结**:其实所谓的死信队列没有很高大上的那种,它呢说白了就是找一个普通的交换机给另一个队列说你要是过期就要消息放我这就可以了!要注意的点还是说如果是消息过期那么一定是过期队列才可以进行消息的转移,因为这种死信的队列的本质上来讲其实就是过期了那么你就消息投递到我们定义的死信交换机,然后死信交换机就会把消息进行分发到我们的死信队列中去,而如果是过期消息进行投递那么是不可以的!所以才说是过期队列才可以!

来看一下这张图:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy4b80ded8-9524-4986-9485-aad2946124b2.png)

​	我们生产者将消息投递到交换机上,交换机经过路由key进行分发到队列中,而队列经过我们设置参数让其6秒过期或设置最大消息数量,而一旦超过这个峰值那么此时就会将消息投递我们自己定义的死信交换机上,然后经过分发到我们的指定死信队列中,--->那么就是这个的过程!

而看见上面的图也可以看出我们所谓死信队列死信交换机就是一个代名词而已,它就是一个普通的交换机和队列而已!!!只是起到了所谓的"接盘侠"的任务而已!

这种应用场景也比较多:比如在用户下订单,那么用户在20分钟内不支付那么就会被移除正式的订单队列中,转而被转移到我们自定义死信队列中,进而我们可以通过死信消费者读取消费信息,比如我们可以将过期的订单生成历史记录,也可以做一些其他事情....

> 再次声明:如果队列首次创建添加了参数那么后续修改是无法进行覆盖的!会报错异常,所以最好请先删除再试!

## 3. 内存磁盘的监控

这一部分应该属于运维的范围了!

### 3.1 内存警告

当内存使用超过配置的阈值或者磁盘空间剩余空间对于配置的阈值时,RabbitMQ会暂时阻塞客户端的连接,并且停止接收从客户端发来的消息,以此避免服务器的崩溃,客户端与服务端的心态检测机制也会失效。

如下图在`Connections`那一栏:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy414d826e-5cea-4caa-aba2-92cd30be34f4.png)

当出现`blocking`或`blocked`话说明到达了阈值和以及高负荷运行了。那么生产者生产者就无法把消息存储到消息队列中去了!说明现在的服务器以及内存不足,需要更换服务器内存!如果物理内存还很充足,那么你就需要去把这个参数值调大!如果不足就增加内存条之类的,

当然我们首先得去这里看我们的磁盘以及内存的使用状况:

![image-20211026184429751](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211026184429751.png)

在这里我们也可以看见内存与磁盘的过程,比如持久化机制,就是将内存中的数据写入到磁盘中去,一般默认的情况下MQ的默认极限大小为0.4,比如说我这里我电脑运行内存为16GB,然后16*0.4=6.4,再加上本身被瓜分走了85MiB,所以这里还剩余6.3GB的内存使用情况!而如果一旦出现报红的情况那么blocked就会全部关闭连接,不再接收消息!

官方文档里已经描述:

```properties
vm_memory_high_watermark.relative = 0.4  # 默认MQ极限阈值大小,这是相对值当然也可以写绝对值
```



### 3.2 内存控制

这是官方给的参考文档:https://www.rabbitmq.com/configure.html

当出现内存警告的时候我们就可以去修改和跳转阈值

> 命令的方式:

```properties
rabbitmqctl set_vm_memory_high_watermark <fraction>
rabbitmqctl set_vm_memory_high_watermark absolute 50MB
```

**注意**:我们修改阈值大小一个绝对和一个相对选其一即可!不用两个都写没有意义的!

fraction/value为内存阈值,默认情况是:0.4/2GB,代表含义是当RabbitMQ的内存超过40%,就会产生警告并且阻塞所有生产者的连接,通过此命令修改阈值在Broker重启以后将会失效,通过修改配置文件方式设置的阈值则不会随着重启而消失,但修改了配置文件一样要重启broker才会生效;

测试一下:

```
rabbitmqctl set_vm_memory_high_watermark absolute 50MB  // 输入这个命令
```

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy2a0177fc-9dd6-4285-8b26-1cc9cd0c6e35.png)

可以看见内存已经爆红,下面是48MB,当前已经使用了93MB那么已经触及到了极限!此时连接就应该全部挂起,不再连接:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy928db4f2-6860-470e-ba62-23e811eee586.png)

如果我们以后在生产环境中一旦看见这种警告或者内存报红,那么就要赶紧让老板去买内存条!



> 使用配置文件的方式-- rabbitmq.conf

```properties
#默认
#vm_memory_high_watermark.relative = 0.4
# 使用relative相对值进行设置fraction,建议取值在04~0.7之间，不建议超过0.7,因为给太大了电脑里其他服务比如MySQL等等都需要一定的内存,给太大就会造成其他服务宕机;
vm_memory_high_watermark.relative = 0.6
# 使用absolute的绝对值的方式，但是是KB,MB,GB对应的命令如下
vm_memory_high_watermark.absolute = 2GB
```



### 3.3 内存换页

在某个Broker节点及内存阻塞在生产者之前,它会尝试将队列中的消息换页到磁盘来释放内存空间,持久化和非持久化的消息都会写入磁盘中,其中持久化的消息本身就在磁盘中有一个副本,所以在转移的过程中持久化的消息会先从内存中清除掉;也就是说不能让内存占满了才去持久化消息转移,得在内存占用满之前进行相关的处理和机制!

> 默认情况下,内存到达的阈值是50%就会换页处理
>
> 也就是说,在默认情况下该内存的阈值是0.4的情况下,当内存超过0.4*0.5=0.2,会进行分页动作片;

举个例子:

比如说有1000MB内存,当内存的使用率达到了400MB,这时就已经达到了极限,但是因为配置的换页内存是0.5,所以在这个时候会在达到400MB之前,会把内存中的200MB进行转移到磁盘中,从而达到稳健的运行。

**理解**：1000MB的物理内存,到Broker节点就是:1000\*0.4=400MB内存,也就是现在使用的内存达到了400MB阈值,而换页的内存条件是Broker节点内存的50%,所以400*0.5=200MB,也就是说现在使用内存超过200MB,那么就会将内存中的200MB数据写入到磁盘中,从而达到一个运行稳健的状态!

可以通过设置`vm_memory_high_watermark_paging_ratio`来进行调整

```properties
# 这是物理内存相对于MQ中的内存阈值
vm_memory_high_watermark.relative = 0.4
# 这是MQ服务内存的阈值相对于的分页阈值
vm_memory_high_watermark_paging_ratio = 0.7（设置小于1的值）
```

**解释**:为什么设置小于1，以为你如果你设置为1的阈值。内存都已经达到了极限了。你在去换页意义不是很大了

核心就是:用磁盘空间换内存空间

### 3.4 磁盘报警

当磁盘的剩余空间低于确定的阈值时,RabbitMQ同样会阻塞生产者生产消息,这样避免因非持久化的消息持续换页而耗尽磁盘空间导致服务器崩溃

> 默认情况下:磁盘预警为50MB的时候会进行预警。表示当前磁盘空间第50MB的时候会阻塞生产者并且停止内存消息换页到磁盘的过程；
>
> 这个阈值可以减小，但是不能完全的消除因磁盘耗尽而导致崩溃的可能性，比如在两次磁盘空间的检查空隙内，第一次检查是：60MB，第二检查可能就是1MB，就会出现警告；

说白了就是可能因为内存消息换页,将内存中数据持久化到磁盘中,那么假如一直这样存,而磁盘也是有大小的,所以磁盘也有预警,到达阈值就会让生产者发生警告!!!默认是50MB

解决：

1. 通过命令方式修改如下：

   ```markdown
   rabbitmqctl set_disk_free_limit  <disk_limit>
   rabbitmqctl set_disk_free_limit memory_limit  <fraction>
   disk_limit：固定单位 KB MB GB
   fraction ：是相对阈值，建议范围在:1.0~2.0之间。（相对于内存）
   ```

2. 通过配置文件配置如下:

   ```properties
   disk_free_limit.relative = 3.0
   disk_free_limit.absolute = 50mb
   ```

   

## 4. 集群

### 4.1 概念

​	RabbitMQ这款消息队列中间件产品本身是基于Erlang编写,ErLang语言天生具备分布式特性(通过同步Erlang集群各节点的magic cookie来实现),因此,RabbitMQ天然支持Clustering（集群）,这使得RabbitMQ本身不需要像ActiveMQ、Kafka那样通过ZooKeeper分别来实现HA方案和保存集群的元数据,集群是保证可靠性第一种方式,同时可以通过水平扩展以达到增加消息吞吐量能力的目的。

主要参考官方文档：https://www.rabbitmq.com/clustering.html

### 4.2 搭建

> 这里的话就不搭建了，因为搭建的话就要重新安装mq重新部署，接下来我直接贴图，以后如果要搭集群那么就来看看：

配置的前提是你的rabbitmq可以运行起来，比如”ps aux|grep rabbitmq”你能看到相关进程，又比如运行“rabbitmqctl status”你可以看到类似如下信息，而不报错：

执行下面命令进行查看：

```bash
ps aux|grep rabbitmq
```

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy2b94c755-598f-42cc-ac4a-7672dcfe8e2a.png)

或者:

```bash
systemctl status rabbitmq-server
```

> 注意：确保RabbitMQ可以运行的，确保完成之后，把单机版的RabbitMQ服务停止，后台看不到RabbitMQ的进程为止

> 接下来是单机多实例搭建

**场景**:假设有两个rabbitmq节点，分别为rabbit-1, rabbit-2，rabbit-1作为主节点，rabbit-2作为从节点。

**启动命令**:RABBITMQ_NODE_PORT=5672 RABBITMQ_NODENAME=rabbit-1 rabbitmq-server -detached

**结束命令**:rabbitmqctl -n rabbit-1 stop

步骤:

1. 启动第一个节点rabbit-1

   ```bash
   > sudo RABBITMQ_NODE_PORT=5672 RABBITMQ_NODENAME=rabbit-1 rabbitmq-server start &
   ...............省略...................
     ##########  Logs: /var/log/rabbitmq/rabbit-1.log
     ######  ##        /var/log/rabbitmq/rabbit-1-sasl.log
     ##########
                 Starting broker...
    completed with 7 plugins.
   ```

   至此节点rabbit-1启动完成。

2. 启动我们第二个节点rabbit-2

   ```bash
   sudo RABBITMQ_NODE_PORT=5673 RABBITMQ_SERVER_START_ARGS="-rabbitmq_management listener [{port,15673}]" RABBITMQ_NODENAME=rabbit-2 rabbitmq-server start &
   ..............省略..................
     ##########  Logs: /var/log/rabbitmq/rabbit-2.log
     ######  ##        /var/log/rabbitmq/rabbit-2-sasl.log
     ##########
                 Starting broker...
    completed with 7 plugins.
   ```

   注意:web管理插件端口占用,所以还要指定其web插件占用的端口号,RABBITMQ_SERVER_START_ARGS=”-rabbitmq_management listener [{port,15673}]”

   至此节点rabbit-2启动完成

3. 验证启动 “ps aux|grep rabbitmq”

   ```bash
   rabbitmq  2022  2.7  0.4 5349380 77020 ?       Sl   11:03   0:06 /usr/lib/erlang/erts-9.2/bin/beam.smp -W w -A 128 -P 1048576 -t 5000000 -stbt db -zdbbl 128000 -K true -B i -- -root /usr/lib/erlang -progname erl -- -home /var/lib/rabbitmq -- -pa /usr/lib/rabbitmq/lib/rabbitmq_server-3.6.15/ebin -noshell -noinput -s rabbit boot -sname rabbit-1 -boot start_sasl -kernel inet_default_connect_options [{nodelay,true}] -rabbit tcp_listeners [{"auto",5672}] -sasl errlog_type error -sasl sasl_error_logger false -rabbit error_logger {file,"/var/log/rabbitmq/rabbit-1.log"} -rabbit sasl_error_logger {file,"/var/log/rabbitmq/rabbit-1-sasl.log"} -rabbit enabled_plugins_file "/etc/rabbitmq/enabled_plugins" -rabbit plugins_dir "/usr/lib/rabbitmq/plugins:/usr/lib/rabbitmq/lib/rabbitmq_server-3.6.15/plugins" -rabbit plugins_expand_dir "/var/lib/rabbitmq/mnesia/rabbit-1-plugins-expand" -os_mon start_cpu_sup false -os_mon start_disksup false -os_mon start_memsup false -mnesia dir "/var/lib/rabbitmq/mnesia/rabbit-1" -kernel inet_dist_listen_min 25672 -kernel inet_dist_listen_max 25672 start
   rabbitmq  2402  4.2  0.4 5352196 77196 ?       Sl   11:05   0:05 /usr/lib/erlang/erts-9.2/bin/beam.smp -W w -A 128 -P 1048576 -t 5000000 -stbt db -zdbbl 128000 -K true -B i -- -root /usr/lib/erlang -progname erl -- -home /var/lib/rabbitmq -- -pa /usr/lib/rabbitmq/lib/rabbitmq_server-3.6.15/ebin -noshell -noinput -s rabbit boot -sname rabbit-2 -boot start_sasl -kernel inet_default_connect_options [{nodelay,true}] -rabbit tcp_listeners [{"auto",5673}] -sasl errlog_type error -sasl sasl_error_logger false -rabbit error_logger {file,"/var/log/rabbitmq/rabbit-2.log"} -rabbit sasl_error_logger {file,"/var/log/rabbitmq/rabbit-2-sasl.log"} -rabbit enabled_plugins_file "/etc/rabbitmq/enabled_plugins" -rabbit plugins_dir "/usr/lib/rabbitmq/plugins:/usr/lib/rabbitmq/lib/rabbitmq_server-3.6.15/plugins" -rabbit plugins_expand_dir "/var/lib/rabbitmq/mnesia/rabbit-2-plugins-expand" -os_mon start_cpu_sup false -os_mon start_disksup false -os_mon start_memsup false -mnesia dir "/var/lib/rabbitmq/mnesia/rabbit-2" -rabbitmq_management listener [{port,15673}] -kernel inet_dist_listen_min 25673 -kernel inet_dist_listen_max 25673 start
   ```

4. rabbit-1操作作为主节点

   ```bash
   #停止应用
   > sudo rabbitmqctl -n rabbit-1 stop_app
   #目的是清除节点上的历史数据（如果不清除，无法将节点加入到集群）
   > sudo rabbitmqctl -n rabbit-1 reset
   #启动应用
   > sudo rabbitmqctl -n rabbit-1 start_app
   ```

5. rabbit2操作为从节点

   ```bash
   # 停止应用
   > sudo rabbitmqctl -n rabbit-2 stop_app
   # 目的是清除节点上的历史数据（如果不清除，无法将节点加入到集群）
   > sudo rabbitmqctl -n rabbit-2 reset
   # 将rabbit2节点加入到rabbit1（主节点）集群当中【Server-node服务器的主机名】
   > sudo rabbitmqctl -n rabbit-2 join_cluster rabbit-1@'Server-node'
   # 启动应用
   > sudo rabbitmqctl -n rabbit-2 start_app
   ```

6. 验证集群状态

   ```bash
   > sudo rabbitmqctl cluster_status -n rabbit-1
   //集群有两个节点：rabbit-1@Server-node、rabbit-2@Server-node
   [{nodes,[{disc,['rabbit-1@Server-node','rabbit-2@Server-node']}]},
    {running_nodes,['rabbit-2@Server-node','rabbit-1@Server-node']},
    {cluster_name,<<"rabbit-1@Server-node.localdomain">>},
    {partitions,[]},
    {alarms,[{'rabbit-2@Server-node',[]},{'rabbit-1@Server-node',[]}]}]
   ```

7. 查看web监控

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy38859ae4-0723-45b7-ac1f-32095b2e28da.png)

注意在访问的时候：web结面的管理需要给15672 node-1 和15673的node-2 设置用户名和密码。如下:

```bash
rabbitmqctl -n rabbit-1 add_user admin admin
rabbitmqctl -n rabbit-1 set_user_tags admin administrator
rabbitmqctl -n rabbit-1 set_permissions -p / admin ".*" ".*" ".*"
rabbitmqctl -n rabbit-2 add_user admin admin
rabbitmqctl -n rabbit-2 set_user_tags admin administrator
rabbitmqctl -n rabbit-2 set_permissions -p / admin ".*" ".*" ".*"
```

小结:

如果采用多机部署方式，需读取其中一个节点的cookie, 并复制到其他节点（节点之间通过cookie确定相互是否可通信）。cookie存放在/var/lib/rabbitmq/.erlang.cookie。例如：主机名分别为rabbit-1、rabbit-2

1. 逐个启动各节点
2. 配置各节点的hosts文件( vim /etc/hosts)

 ip1：rabbit-1
 ip2：rabbit-2
其它步骤雷同单机部署方式

### 4.3 SpringBoot整合MQ集群配置

1. 引入starter

   ```xml-dtd
   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>2.2.6.RELEASE</version>
       <relativePath/> <!-- lookup parent from repository -->
   </parent>
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-amqp</artifactId>
   </dependency>
   ```

2. 详细配置如下:

   ```yaml
    rabbitmq:
       addresses: 127.0.0.1:6605,127.0.0.1:6606,127.0.0.1:6705 #指定client连接到的server的地址，多个以逗号分隔(优先取addresses，然后再取host)
   #    port:
       ##集群配置 addresses之间用逗号隔开
       # addresses: ip:port,ip:port
       password: admin
       username: 123456
       virtual-host: / # 连接到rabbitMQ的vhost
       requested-heartbeat: #指定心跳超时，单位秒，0为不指定；默认60s
       publisher-confirms: #是否启用 发布确认
       publisher-reurns: # 是否启用发布返回
       connection-timeout: #连接超时，单位毫秒，0表示无穷大，不超时
       cache:
         channel.size: # 缓存中保持的channel数量
         channel.checkout-timeout: # 当缓存数量被设置时，从缓存中获取一个channel的超时时间，单位毫秒；如果为0，则总是创建一个新channel
         connection.size: # 缓存的连接数，只有是CONNECTION模式时生效
         connection.mode: # 连接工厂缓存模式：CHANNEL 和 CONNECTION
       listener:
         simple.auto-startup: # 是否启动时自动启动容器
         simple.acknowledge-mode: # 表示消息确认方式，其有三种配置方式，分别是none、manual和auto；默认auto
         simple.concurrency: # 最小的消费者数量
         simple.max-concurrency: # 最大的消费者数量
         simple.prefetch: # 指定一个请求能处理多少个消息，如果有事务的话，必须大于等于transaction数量.
         simple.transaction-size: # 指定一个事务处理的消息数量，最好是小于等于prefetch的数量.
         simple.default-requeue-rejected: # 决定被拒绝的消息是否重新入队；默认是true（与参数acknowledge-mode有关系）
         simple.idle-event-interval: # 多少长时间发布空闲容器时间，单位毫秒
         simple.retry.enabled: # 监听重试是否可用
         simple.retry.max-attempts: # 最大重试次数
         simple.retry.initial-interval: # 第一次和第二次尝试发布或传递消息之间的间隔
         simple.retry.multiplier: # 应用于上一重试间隔的乘数
         simple.retry.max-interval: # 最大重试时间间隔
         simple.retry.stateless: # 重试是有状态or无状态
       template:
         mandatory: # 启用强制信息；默认false
         receive-timeout: # receive() 操作的超时时间
         reply-timeout: # sendAndReceive() 操作的超时时间
         retry.enabled: # 发送重试是否可用
         retry.max-attempts: # 最大重试次数
         retry.initial-interval: # 第一次和第二次尝试发布或传递消息之间的间隔
         retry.multiplier: # 应用于上一重试间隔的乘数
         retry.max-interval: #最大重试时间间隔
   ```

   对于发送方而言，需要做以下配置：
   1 配置CachingConnectionFactory
   2 配置Exchange/Queue/Binding
   3 配置RabbitAdmin创建上一步的Exchange/Queue/Binding
   4 配置RabbitTemplate用于发送消息，RabbitTemplate通过CachingConnectionFactory获取到Connection，然后想指定Exchange发送
   对于消费方而言，需要做以下配置：
   1 配置CachingConnectionFactory
   2 配置Exchange/Queue/Binding
   3 配置RabbitAdmin创建上一步的Exchange/Queue/Binding
   4 配置RabbitListenerContainerFactory
   5 配置[@RabbitListener](https://github.com/RabbitListener)/[@RabbitHandler](https://github.com/RabbitHandler)用于接收消息
   在默认情况下主要的配置如下：

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy1433dcb6-80f9-4592-a57e-2cbb15863fb8.png)

3. 使用：

   通过配置类加载的方式：

   ```java
   package com.yd.demo.config;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.amqp.core.AcknowledgeMode;
   import org.springframework.amqp.core.Binding;
   import org.springframework.amqp.core.BindingBuilder;
   import org.springframework.amqp.core.DirectExchange;
   import org.springframework.amqp.core.Queue;
   import org.springframework.amqp.core.TopicExchange;
   import org.springframework.amqp.rabbit.config.SimpleRabbitListenerContainerFactory;
   import org.springframework.amqp.rabbit.connection.CachingConnectionFactory;
   import org.springframework.amqp.rabbit.connection.ConnectionFactory;
   import org.springframework.amqp.rabbit.core.RabbitAdmin;
   import org.springframework.amqp.rabbit.core.RabbitTemplate;
   import org.springframework.amqp.rabbit.listener.RabbitListenerContainerFactory;
   import org.springframework.amqp.support.converter.Jackson2JsonMessageConverter;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import java.util.HashMap;
   import java.util.Map;
   @Configuration
   public class RabbitConfig {
       private static final Logger logger = LoggerFactory.getLogger(RabbitConfig.class);
       public static final String RECEIVEDLXEXCHANGE="spring-ex";
       public static final String RECEIVEDLXQUEUE="spring-qu1";
       public static final String RECEIVEDLXROUTINGKEY="aa";
       public static final String DIRECTEXCHANGE="spring-ex";
       public static final String MDMQUEUE="mdmQueue";
       public static final String TOPICEXCHANGE="spring-top";
       @Value("${spring.rabbitmq.addresses}")
       private String hosts;
       @Value("${spring.rabbitmq.username}")
       private String userName;
       @Value("${spring.rabbitmq.password}")
       private String password;
       @Value("${spring.rabbitmq.virtual-host}")
       private String virtualHost;
    /*   @Value("${rabbit.channelCacheSize}")
       private int channelCacheSize;*/
   //    @Value("${rabbit.port}")
   //    private int port;
   /*    @Autowired
       private ConfirmCallBackListener confirmCallBackListener;
       @Autowired
       private ReturnCallBackListener returnCallBackListener;*/
       @Bean
       public ConnectionFactory connectionFactory(){
           CachingConnectionFactory cachingConnectionFactory = new CachingConnectionFactory();
           cachingConnectionFactory.setAddresses(hosts);
           cachingConnectionFactory.setUsername(userName);
           cachingConnectionFactory.setPassword(password);
   //        cachingConnectionFactory.setChannelCacheSize(channelCacheSize);
           //cachingConnectionFactory.setPort(port);
           cachingConnectionFactory.setVirtualHost(virtualHost);
           //设置连接工厂缓存模式：
           cachingConnectionFactory.setCacheMode(CachingConnectionFactory.CacheMode.CONNECTION);
           //缓存连接数
           cachingConnectionFactory.setConnectionCacheSize(3);
           //设置连接限制
           cachingConnectionFactory.setConnectionLimit(6);
           logger.info("连接工厂设置完成，连接地址{}"+hosts);
           logger.info("连接工厂设置完成，连接用户{}"+userName);
           return cachingConnectionFactory;
       }
       @Bean
       public RabbitAdmin rabbitAdmin(){
           RabbitAdmin rabbitAdmin = new RabbitAdmin(connectionFactory());
           rabbitAdmin.setAutoStartup(true);
           rabbitAdmin.setIgnoreDeclarationExceptions(true);
           rabbitAdmin.declareBinding(bindingMdmQueue());
           //声明topic交换器
           rabbitAdmin.declareExchange(directExchange());
           logger.info("管理员设置完成");
           return rabbitAdmin;
       }
       @Bean
       public RabbitListenerContainerFactory listenerContainerFactory() {
           SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
           factory.setConnectionFactory(connectionFactory());
           factory.setMessageConverter(new Jackson2JsonMessageConverter());
           //最小消费者数量
           factory.setConcurrentConsumers(10);
           //最大消费者数量
           factory.setMaxConcurrentConsumers(10);
           //一个请求最大处理的消息数量
           factory.setPrefetchCount(10);
           //
           factory.setChannelTransacted(true);
           //默认不排队
           factory.setDefaultRequeueRejected(true);
           //手动确认接收到了消息
           factory.setAcknowledgeMode(AcknowledgeMode.MANUAL);
           logger.info("监听者设置完成");
           return factory;
       }
       @Bean
       public DirectExchange directExchange(){
           return new DirectExchange(DIRECTEXCHANGE,true,false);
       }
       @Bean
       public Queue mdmQueue(){
           Map arguments = new HashMap<>();
           // 绑定该队列到私信交换机
           arguments.put("x-dead-letter-exchange",RECEIVEDLXEXCHANGE);
           arguments.put("x-dead-letter-routing-key",RECEIVEDLXROUTINGKEY);
           logger.info("队列交换机绑定完成");
           return new Queue(RECEIVEDLXQUEUE,true,false,false,arguments);
       }
       @Bean
       Binding bindingMdmQueue() {
           return BindingBuilder.bind(mdmQueue()).to(directExchange()).with("");
       }
       @Bean
       public RabbitTemplate rabbitTemplate(){
           RabbitTemplate rabbitTemplate = new RabbitTemplate(connectionFactory());
           rabbitTemplate.setMandatory(true);
           //发布确认
   //        rabbitTemplate.setConfirmCallback(confirmCallBackListener);
           // 启用发布返回
   //        rabbitTemplate.setReturnCallback(returnCallBackListener);
           logger.info("连接模板设置完成");
           return rabbitTemplate;
       }
     /*  @Bean
       public TopicExchange topicExchange(){
           return new TopicExchange(TOPICEXCHANGE,true,false);
       }*/
     /*
   *//**
        * @return DirectExchange
        *//*
       @Bean
       public DirectExchange dlxExchange() {
           return new DirectExchange(RECEIVEDLXEXCHANGE,true,false);
       }
   *//*
   *
        * @return Queue
   *//*
       @Bean
       public Queue dlxQueue() {
           return new Queue(RECEIVEDLXQUEUE,true);
       }
   *//*
        * @return Binding
        *//*
       @Bean
       public Binding binding() {
           return BindingBuilder.bind(dlxQueue()).to(dlxExchange()).with(RECEIVEDLXROUTINGKEY);
       }*/
   }
   ```

   通过两种方式加载
   1 通过配置文件
   2 通过配置类
   说明：上面是通过配置文件与配置类的方式去加载,常用的配置如上所示。实际使用中要生产方与消费方要分开配置，相关配置也会有小变动，大体配置不变。更多信息可查看官网配置。

### 4.4 集群监控

> 管理界面监控

管理界面监控需要我们开启对应的插件(rabbitmq-plugins enable rabbitmq_management) 然后访问[http://ip:15672](http://ip:15672/)

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudyefd35cc0-3b07-4298-9aac-afa7086350e6.png)

在管理控制台我们就可以直观的看到集群中的每一个节点是否正常,如果为红色则表示节点挂掉了,同时可以很方便的查看到各个节点的内存、磁盘等相关的信息，使用起来也是非常方便的。但是遗憾的该功能做的比较简陋,没有告警等一些列的个性化设置,同时如果想把他接入到公司其他的监控系统统一管理也是很难做到的,所以扩展性不强，一般在小型企业的小集群中使用;

> tracing日志监控

​	对于企业级的应用开发来讲,我们通常都会比较关注我们的消息,甚至很多的场景把消息的可靠性放在第一位,但是我们的MQ集群难免会出现消息异常丢失或者客户端无法发送消息等异常情况,此时为了帮助开发人员快速的定位问题,我们就可以对消息的投递和消费过程进行监控,而tracing日志监控插件帮我们很好的实现了该功能
​	消息中心的消息追踪需要使用Trace实现，Trace是Rabbitmq用于记录每一次发送的消息，方便使用Rabbitmq的开发者调试、排错。可通过插件形式提供可视化界面。Trace启动后会自动创建系统Exchange：amq.rabbitmq.trace ,每个队列会自动绑定该Exchange，绑定后发送到队列的消息都会记录到Trace日志

**消息的追踪启用与查看:**

以下是trace的相关命令和使用（要使用需要先rabbitmq启用插件，再打开开关才能使用）：

|                    命令集                     |                     描述                     |
| :-------------------------------------------: | :------------------------------------------: |
|             rabbitmq-plugins list             |                 查看插件列表                 |
|   rabbitmq-plugins enable rabbitmq_tracing    |            rabbitmq启用trace插件             |
|             rabbitmqctl trace_on              |               打开trace的开关                |
|        rabbitmqctl trace_on -p itcast         | 打开trace的开关(itcast为需要日志追踪的vhost) |
|             rabbitmqctl trace_off             |               关闭trace的开关                |
|   rabbitmq-plugins disable rabbitmq_tracing   |            rabbitmq关闭Trace插件             |
| rabbitmqctl set_user_tags heima administrator |   只有administrator的角色才能查看日志界面    |

安装插件并开启 trace_on 之后，会发现多个 exchange：amq.rabbitmq.trace ，类型为：topic。

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudyd39998d5-9bc6-4acb-a379-08a576623b40.png)

> 日志追踪:

1. 发送消息

   ```java
   rabbitTemplate.convertAndSend("spring_queue", "只发队列spring_queue的消息--01。");
   ```

2. 查看trace

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy14b7aa6d-0816-453b-96d1-5e63cc0a711e.png)

3. 点击Tracing查看Trace log files

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy3b05dced-e525-40ef-a0b1-59888679f9a5.png)

4. 点击xuexiangban-trace.log确认消息轨迹正确性

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy265d609c-788a-4219-95eb-51c859cd9f26.png)

> 定制自己的监控系统

RabbitMQ提供了很丰富的restful风格的api接口,我们可以通过这些接口得到对应的集群数据,此时我们就可以定制我们的监控系统。

|             HTTP API URL              |  HTTP请求类型  |                           接口含义                           |
| :-----------------------------------: | :------------: | :----------------------------------------------------------: |
|           /api/connections            |      GET       |             获取当前RabbitMQ集群下所有打开的连接             |
|              /api/nodes               |      GET       |         获取当前RabbitMQ集群下所有节点实例的状态信息         |
|    /api/vhosts/{vhost}/connections    |      GET       |       获取某一个虚拟机主机下的所有打开的connection连接       |
|   /api/connections/{name}/channels    |      GET       |                获取某一个连接下所有的管道信息                |
|     /api/vhosts/{vhost}/channels      |      GET       |               获取某一个虚拟机主机下的管道信息               |
|        /api/consumers/{vhost}         |      GET       |            获取某一个虚拟机主机下的所有消费者信息            |
|        /api/exchanges/{vhost}         |      GET       |           获取某一个虚拟机主机下面的所有交换器信息           |
|          /api/queues/{vhost}          |      GET       |             获取某一个虚拟机主机下的所有队列信息             |
|              /api/users               |      GET       |                   获取集群中所有的用户信息                   |
|           /api/users/{name}           | GET/PUT/DELETE |                  获取/更新/删除指定用户信息                  |
|     /api/users/{user}/permissions     |      GET       |                获取当前指定用户的所有权限信息                |
|    /api/permissions/{vhost}/{user}    | GET/PUT/DELETE |          获取/更新/删除指定虚拟主机下特定用户的权限          |
| /api/exchanges/{vhost}/{name}/publish |      POST      |           在指定的虚拟机主机和交换器上发布一个消息           |
|    /api/queues/{vhost}/{name}/get     |      POST      | 在指定虚拟机主机和队列名中获取消息，同时该动作会修改队列状态 |
|     /api/healthchecks/node/{node}     |      GET       |                  获取指定节点的健康检查状态                  |

更多API的相关信息和描述可以访问http://ip:15672/api/

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy21c5efec-827c-4b3f-86d2-99704d5dee0f.png)

接下来我们使用RabbitMQ Http API接口来获取集群监控数据:

1. HttpClient以及Jackson的相关Jar

   ```xml
   <dependency>
       <groupId>org.apache.httpcomponents</groupId>
       <artifactId>httpclient</artifactId>
       <version>4.3.6</version>
   </dependency>
   <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.7.4</version>
   </dependency>
   <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.7.4</version>
   </dependency>
   <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.7.4</version>
   </dependency>
   ```

2. 创建MonitorRabbitMQ类实现具体的代码

   ```java
   package com.xuexiangban.rabbitmq;
   import com.fasterxml.jackson.databind.DeserializationFeature;
   import com.fasterxml.jackson.databind.JsonNode;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import org.apache.http.HttpEntity;
   import org.apache.http.auth.UsernamePasswordCredentials;
   import org.apache.http.client.methods.CloseableHttpResponse;
   import org.apache.http.client.methods.HttpGet;
   import org.apache.http.impl.auth.BasicScheme;
   import org.apache.http.impl.client.CloseableHttpClient;
   import org.apache.http.impl.client.HttpClients;
   import org.apache.http.util.EntityUtils;
   import java.io.IOException;
   import java.util.HashMap;
   import java.util.Iterator;
   import java.util.Map;
   /**
    * RabbitMQ的监控
    */
   public class MonitorRabbitMQ {
       //RabbitMQ的HTTP API——获取集群各个实例的状态信息，ip替换为自己部署相应实例的
       private static String RABBIT_NODES_STATUS_REST_URL = "http://192.168.13.111:15672/api/nodes";
       //RabbitMQ的HTTP API——获取集群用户信息，ip替换为自己部署相应实例的
       private static String RABBIT_USERS_REST_URL = "http://192.168.13.111:15672/api/users";
       //rabbitmq的用户名
       private static String RABBIT_USER_NAME = "guest";
       //rabbitmq的密码
       private static String RABBIT_USER_PWD = "guest";
       public static void main(String[] args) {
           try {
               //step1.获取rabbitmq集群各个节点实例的状态信息
               Map<String, ClusterStatus> clusterMap =
                       fetchRabbtMQClusterStatus(RABBIT_NODES_STATUS_REST_URL, RABBIT_USER_NAME, RABBIT_USER_PWD);
               //step2.打印输出各个节点实例的状态信息
               for (Map.Entry entry : clusterMap.entrySet()) {
                   System.out.println(entry.getKey() + " : " + entry.getValue());
               }
               //step3.获取rabbitmq集群用户信息
               Map<String, User> userMap =
                       fetchRabbtMQUsers(RABBIT_USERS_REST_URL, RABBIT_USER_NAME, RABBIT_USER_PWD);
               //step4.打印输出rabbitmq集群用户信息
               for (Map.Entry entry : userMap.entrySet()) {
                   System.out.println(entry.getKey() + " : " + entry.getValue());
               }
           } catch (IOException e) {
               e.printStackTrace();
           }
       }
       public static Map<String, ClusterStatus> fetchRabbtMQClusterStatus(String url, String username, String password) throws IOException {
           Map<String, ClusterStatus> clusterStatusMap = new HashMap<String, ClusterStatus>();
           String nodeData = getData(url, username, password);
           JsonNode jsonNode = null;
           try {
               jsonNode = JsonUtil.toJsonNode(nodeData);
           } catch (IOException e) {
               e.printStackTrace();
           }
           Iterator<JsonNode> iterator = jsonNode.iterator();
           while (iterator.hasNext()) {
               JsonNode next = iterator.next();
               ClusterStatus status = new ClusterStatus();
               status.setDiskFree(next.get("disk_free").asLong());
               status.setFdUsed(next.get("fd_used").asLong());
               status.setMemoryUsed(next.get("mem_used").asLong());
               status.setProcUsed(next.get("proc_used").asLong());
               status.setSocketUsed(next.get("sockets_used").asLong());
               clusterStatusMap.put(next.get("name").asText(), status);
           }
           return clusterStatusMap;
       }
       public static Map<String, User> fetchRabbtMQUsers(String url, String username, String password) throws IOException {
           Map<String, User> userMap = new HashMap<String, User>();
           String nodeData = getData(url, username, password);
           JsonNode jsonNode = null;
           try {
               jsonNode = JsonUtil.toJsonNode(nodeData);
           } catch (IOException e) {
               e.printStackTrace();
           }
           Iterator<JsonNode> iterator = jsonNode.iterator();
           while (iterator.hasNext()) {
               JsonNode next = iterator.next();
               User user = new User();
               user.setName(next.get("name").asText());
               user.setTags(next.get("tags").asText());
               userMap.put(next.get("name").asText(), user);
           }
           return userMap;
       }
       public static String getData(String url, String username, String password) throws IOException {
           CloseableHttpClient httpClient = HttpClients.createDefault();
           UsernamePasswordCredentials creds = new UsernamePasswordCredentials(username, password);
           HttpGet httpGet = new HttpGet(url);
           httpGet.addHeader(BasicScheme.authenticate(creds, "UTF-8", false));
           httpGet.setHeader("Content-Type", "application/json");
           CloseableHttpResponse response = httpClient.execute(httpGet);
           try {
               if (response.getStatusLine().getStatusCode() != 200) {
                   System.out.println("call http api to get rabbitmq data return code: " + response.getStatusLine().getStatusCode() + ", url: " + url);
               }
               HttpEntity entity = response.getEntity();
               if (entity != null) {
                   return EntityUtils.toString(entity);
               }
           } finally {
               response.close();
           }
           return null;
       }
       public static class JsonUtil {
           private static ObjectMapper objectMapper = new ObjectMapper();
           static {
               objectMapper.disable(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);
               //objectMapper.disable(SerializationFeature.FAIL_ON_EMPTY_BEANS);
           }
           public static JsonNode toJsonNode(String jsonString) throws IOException {
               return objectMapper.readTree(jsonString);
           }
       }
       public static class User {
           private String name;
           private String tags;
           @Override
           public String toString() {
               return "User{" +
                       "name=" + name +
                       ", tags=" + tags +
                       '}';
           }
           //GET/SET方法省略
           public String getName() {
               return name;
           }
           public void setName(String name) {
               this.name = name;
           }
           public String getTags() {
               return tags;
           }
           public void setTags(String tags) {
               this.tags = tags;
           }
       }
       public static class ClusterStatus {
           private long diskFree;
           private long diskLimit;
           private long fdUsed;
           private long fdTotal;
           private long socketUsed;
           private long socketTotal;
           private long memoryUsed;
           private long memoryLimit;
           private long procUsed;
           private long procTotal;
           // 此处省略了Getter和Setter方法
           public long getDiskFree() {
               return diskFree;
           }
           public void setDiskFree(long diskFree) {
               this.diskFree = diskFree;
           }
           public long getDiskLimit() {
               return diskLimit;
           }
           public void setDiskLimit(long diskLimit) {
               this.diskLimit = diskLimit;
           }
           public long getFdUsed() {
               return fdUsed;
           }
           public void setFdUsed(long fdUsed) {
               this.fdUsed = fdUsed;
           }
           public long getFdTotal() {
               return fdTotal;
           }
           public void setFdTotal(long fdTotal) {
               this.fdTotal = fdTotal;
           }
           public long getSocketUsed() {
               return socketUsed;
           }
           public void setSocketUsed(long socketUsed) {
               this.socketUsed = socketUsed;
           }
           public long getSocketTotal() {
               return socketTotal;
           }
           public void setSocketTotal(long socketTotal) {
               this.socketTotal = socketTotal;
           }
           public long getMemoryUsed() {
               return memoryUsed;
           }
           public void setMemoryUsed(long memoryUsed) {
               this.memoryUsed = memoryUsed;
           }
           public long getMemoryLimit() {
               return memoryLimit;
           }
           public void setMemoryLimit(long memoryLimit) {
               this.memoryLimit = memoryLimit;
           }
           public long getProcUsed() {
               return procUsed;
           }
           public void setProcUsed(long procUsed) {
               this.procUsed = procUsed;
           }
           public long getProcTotal() {
               return procTotal;
           }
           public void setProcTotal(long procTotal) {
               this.procTotal = procTotal;
           }
           @Override
           public String toString() {
               return "ClusterStatus{" +
                       "diskFree=" + diskFree +
                       ", diskLimit=" + diskLimit +
                       ", fdUsed=" + fdUsed +
                       ", fdTotal=" + fdTotal +
                       ", socketUsed=" + socketUsed +
                       ", socketTotal=" + socketTotal +
                       ", memoryUsed=" + memoryUsed +
                       ", memoryLimit=" + memoryLimit +
                       ", procUsed=" + procUsed +
                       ", procTotal=" + procTotal +
                       '}';
           }
       }
   }
   ```

3. 启动测试

   ![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy163bbaea-9c8e-4cc3-95ca-e1e44115894f.png)

> Zabbix 监控RabbitMQ

Zabbix是一个基于WEB界面提供分布式系统监视以及网络监视功能的企业级开源解决方案,他也可以帮助我们搭建一个MQ集群的监控系统,同时提供预警等功能，但是由于其搭建配置要求比较高一般都是由运维人员负责搭建,可以访问https://www.zabbix.com/ 官网进行了解学习。

## 5.分布式事务

