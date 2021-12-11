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

   **理解**: 我们首先定义一个普通交换机和两个队列（也是所谓的死信交换机和死信队列），然后在这里我们定义过期队列（正常普通的），然后定义参数，其中设置过期后就会移动到指定死信队列中，然后根据设置的死信交换机的routing key将消息发送给指定的死信队列！

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

### 5.1 概述

分布式事务指事务的操作位于不同的节点上,需要保证事务的ACID特性。

![image-20211029181040786](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211029181040786.png)

例如：

​	在下单的场景下，库存和订单如果不在同一个服务,有独立的JVM和数据库DB,现在我们想要变成一个整体那么就会涉及到数据的一致性,就好比这个我们需要完整的保存订单和库存的扣减保证整个闭环是正常的!理想状态下在保存订单的过程中库存服务发生了故障,那么就会让订单服务发生失败,让事务回滚!这就是事务的重要性了!而我们之前学习Spring提供了对数据库事务的支持,但是它们只能控制当前JVM自己级别范围的数据库,也就是说不能说这个JVM去控制另一个JVM,现在我们就需要来解决这个问题;	



**理解**:分布式事务它是指在我们项目发展到一定阶段把系统拆分成若干个子服务,服务之间都有独立的数据库和独立的JVM空间,如何保证垮数据库级别的数据一致性,那么就是我们现在即将来看到的分布式事务的问题;而所谓的分布式事务就是指在不同的系统之间如何保证事务的完整性的一种解决方案;

### 5.2 解决方案

在分布式系统中要实现分布式事务,无外乎就这几种:

#### 1. 2PC

两阶段提交(Two-phase Commit,2PC),通过引入`协调者`(Conrdinator)来协调参与者的行为,并最终决定这些参与者是否要真正执行事务;

**注**:两阶段提交(2PC)需要数据库厂商的支持,Java有组件atomikos等

> 准备阶段

协调者询问参与者事务是否正常执行成功,参与者发回事务执行结果

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy28c463a1-73a3-42a5-b498-265078248883.png)

> 提交阶段

如果事务在每个参与者上都执行成功,事务协调者发送通知让参与者提交事务,否则,协调者发送通知让参与者回滚事务。

**注意**：在准备阶段参与者执行了事务但是还未提交，只有在提交阶段接收到协调者发来的通知后，才进行提交或回滚

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy1227423e-49da-4823-bab5-9a1530403086.png)

**存在的问题**：

1. 同步阻塞
   - 所有事务参与者在等待其他参与者响应的时候都处于同步阻塞状态，无法进行其他操作；
2. 单点问题
   - 协调者在2PC中起到非常大的作用，但是如果发生故障那也会造成很大影响，特别是在第二阶段发生故障那么所有的参与者都会一直处于等待状态，无法完成其他操作；
3. 数据不一致
   - 在阶段二如果协调者只发送了部分Commit消息,此时网络发生异常,那么只有部分参与者接收到Commit消息,也就是说只有部分参与者提交了事务.使得系统数据不一致
4. 太过保守
   - 任意一个节点失败就会导致整个事务失败,没有完善的容错机制;

#### 2. TCC

TCC其实就是采用的补偿机制,其核心思想是:针对每个操作,都要注册一个与其对应的确认和补偿(撤销)操作,它分为三个阶段:

- Try阶段主要是对业务系统做检测及资源预留
- Confirm阶段主要是对业务系统做确认提交,Try阶段执行成功并开始执行Confirm阶段时,默认---Confirm阶段是不会出错的,即:只要Try成功,Confirm一定成功
- Cancel阶段主要在业务执行错误,需要回滚的状态下执行的业务取消,预留资源释放

```java
举个例子:假如A向B转账,大概思路是:我们有一个本地方法,里面依次调用
1. 首先在Try阶段,要先调用远程接口把A和B的钱给冻结起来;
2. 在Confirm阶段,执行远程调用的转账的操作,转账成功进行解冻;
3. 如果第2步执行成功,那么转账成功,如果第二步执行失败,则调用远程冻结接口对应的解冻方法(Cancel);
```

优点:

- 根2PC比起来,实现以及流程相对简单了些,但数据的一致性必2PC也要差一些

缺点:

- 比较明显,在2,3步中都有可能失败,TCC属于应用层的一种补偿方式,所以需要程序员在实现的时候多写很多补偿的代码,在一些场景中,一些业务流程可能用TCC不太好定义及处理.

> 其实就是对我们每一个连接信息进行注册,然后过程中出现事务回滚那么做补偿,但是相较必2PC实现要简单些但数据一致性要差!

说明:其中这个机制有严选、阿里、蚂蚁金服;

#### 3. 本地消息表(异步确保)

如：支付宝、微信支付主动查询支付状态，对账单的形式

**概念**：本地消息表与业务数据表处于同一个数据库中，这样有利于本地事务来保证在对这两个表的操作满足事务特性，并且使用了消息队列来保证最终一致性；

- 在分布式事务操作的一方完成写业务数据的操作之后向本地消息表发送一个消息，本地事务能保证这个消息一定会写入本地消息表中；
- 之后将本地消息表中的消息转发到Kafka等消息队列中,如果转发成功则将消息从本地消息表中删除,否则继续重新转发;
- 在分布式事务操作的另一方从消息队列中读取一个消息,并执行消息中的操作;

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy316cc2e4-e23a-4874-a8a9-0ed14ba71989.png)

优点:一种非常经典的实现,避免了分布式事务,实现了最终一致性

缺点:消息表会耦合在业务系统中,如果没有封装好的解决方案会导致很多杂活需要处理。

> 理解：是用一种对战的机制，我们把数据的结果先写到DB然后两个事务之间进行状态的修改，可以用Kafka或者MQ来完成它们之间异步的确保,来进行对比对战一下,就是每次执行完都会通知RabbitMQ或者Kafka来对数据的对战,这样会保证数据的正常性,而如果在写入的过程中出现异常,那么就会同时广播,如果没有发生问题就会把事务提交,但凡一边出现了问题那么接下来就会失败;

#### 4. MQ事务消息

有一些第三方的MQ是支持事务消息的,比如RocketMQ,他们支持事务的消息的方式也是类似于采用的二阶段提交,但是市面上一些主流的MQ都是不支持事务消息的,比如Kafka不支持.

以阿里的RabbitMQ中间件为例,其思路大致为:

- 第一阶段Prepared消息,会拿到消息的地址。第二阶段执行本地事务，第三阶段通过第一阶段拿到的地址去访问消息，并修改状态
- 也就是说在业务方法内要想消息队列提交两次请求，一次发送消息和一次确认消息。如果确认消息发送失败了RabbitMQ会定期扫描消息集群中的事务消息,这时候发现了Prepared消息,它会向消息发送者确认,所以生产方需要实现一个check接口,RabbitMQ会根据发送端设置的策略来决定是回滚还是继续确认消息,这样就保证了消息发送与本地事务同时成功或同时失败。

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudye6732a40-44e5-491d-8e3d-7070fc943151.png)

优点： 实现了最终一致性，不需要依赖本地数据库事务。

缺点： 实现难度大，主流MQ不支持，RocketMQ事务消息部分代码也未开源。

> 这种异步场景，通用性比较强，拓展性比较高



### 5.3 具体实现

这里以一个美团来作为基础的原型来进行分析：

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy29ccc5e9-db2b-40ae-ae33-385e15519226.png)

**分析**：首先用户在APP交易平台上选购商品，选购完成以后会有一个配送平台，会把我们商品订单信息会配送到我们的物流系统，物流系统就会通知骑手进行一系列的到店取餐和送达客户手中。对于这种架构肯定就会用到分布式架构来进行开发，也就是说用户会在APP这一端下单接下来就会通知我们的配送平台，然后由通知中心通知骑手去取餐，那么这里的订单系统和配送中心肯定是两个独立的服务，服务与服务之间的传递肯定就会用到消息中间件完成，一旦用到消息中间件就会引发分布式事务的问题，也即是数据的最终一致性问题；

> 系统与系统之间的分布式事务问题

![image-20211029192323574](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211029192323574.png)

比如说小明在我们美团上下了一个订单点了一个外卖，然后就会把订单数据通过一些主流方式进行传输到配送中心，然后呢我们的订单就完成了一个流转；从这里可以看出这里订单服务和配送中心的数据库是不一致的是独立的数据库、独立的JVM、独立的服务体系。比如说我这里订单服务下名单那么有自己的订单数据库,而配送中心也会有自己独立的配送数据库,通过一些远程调用的方式来达到系统与系统之间的通讯以后,那么这里就会牵扯到两个数据库连接,连接过程中事务的管理只能控制自身的系统而无法去回滚我们的配送中心;主要是两者之间都有自己独立的事务,事务与事务之间是无法进行控制的;

这里写一个DEMO:

```java
package com.xuexiangban.rabbitmq.service;
import com.xuexiangban.rabbitmq.dao.OrderDataBaseService;
import com.xuexiangban.rabbitmq.pojo.Order;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.client.SimpleClientHttpRequestFactory;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import org.springframework.web.client.RestTemplate;
@Service
public class OrderService {
    @Autowired
    private OrderDataBaseService orderDataBaseService;
    // 创建订单
    @Transactional(rollbackFor = Exception.class) // 订单创建整个方法添加事务
    public void createOrder(Order orderInfo) throws Exception {
        // 1: 订单信息--插入丁订单系统，订单数据库事务
        orderDataBaseService.saveOrder(orderInfo);
        // 2：通過Http接口发送订单信息到运单系统
        String result = dispatchHttpApi(orderInfo.getOrderId());
        if(!"success".equals(result)) {
            throw new Exception("订单创建失败,原因是运单接口调用失败!");
        }
    }
    /**
     *  模拟http请求接口发送，运单系统，将订单号传过去 springcloud
     * @return
     */
    private String dispatchHttpApi(String orderId) {
        SimpleClientHttpRequestFactory factory  = new SimpleClientHttpRequestFactory();
        // 链接超时 > 3秒
        factory.setConnectTimeout(3000);
        // 处理超时 > 2秒
        factory.setReadTimeout(2000);
        // 发送http请求
        String url = "http://localhost:9000/dispatch/order?orderId="+orderId;
        RestTemplate restTemplate = new RestTemplate(factory);//异常
        String result = restTemplate.getForObject(url, String.class);
        return result;
    }
}
```

此时就会发现系统间调用过程中事务回滚问题,也就是所谓的分布式事务问题,一旦遇到异常那么无法保证两个数据库之间数据的一致性!因为A事务无法去控制B事务...

并且可以看见这里使用的HTTP请求协议去调用接口来进行传输数据

> 基于MQ的分布式事务整体设计思路

![image-20211029195421230](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211029195421230.png)

可以发现订单服务与配送中心之间的数据我们可以用RabbitMQ的消息中间件进行可靠传输,我们会把消息服务丢给RabbitMQ,然后配送中心监听收到消息来完成数据的传递,但是还是不能解决分布式事务引发的数据的不一致问题!所以我们一系列的策略和手段再结合消息中间件一起来达成这件事情!

> 在实际应用中分布式事务所解决的问题是一个永恒的话题,想要完美的达到事务的一致性还是很难的,所以一般都说"我保证我的订单不丢失成功率达到99%,但是永远不敢说100%"

其实总的来说分布式事务其实就是指两个服务与服务之间事务都是独立的,而如何让这些数据库的事务之间达到最终一致性(ACID)都可以采用其他方式,比如MQ还是2PC还是TCC,只是消息队列在这方面比较完善和友好以及更加的高效;

### 5.3 MQ解决方案

#### 5.3.1 可靠生产:

我们用消息队列的目的就是为了数据的最终一致性问题:就是我的数据跑到另一个服务中报错了,那么我的数据通过我的策略和机制会达到最终一致性的目的,那么可以通过这张图来进行解释:

![image-20211029201205129](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211029201205129.png)

解释:首先我们的业务数据会往我们的生产者这边投递消息,然后就会把消息发送到交换机然后转发到队列中,而我们之前在业务数据投递的时候会有一个消息冗余字段之类的DB,因为可能消息中间件出现一些故障所以我们为了保证消息最终是一定投递到MQ的,会用到冗余机制,这个时候就会在本地数据库建立一个消息冗余表,说白了就是一个备机制,这个消息表不做任何事只用来存储跟订单信息一样,只是会去告诉消息服务是否会准确投递到消息队列中,会放一些状态用于判断你这消息是否确认收到了,那么就会给一个可靠回执,我们为了保证消息数据的不丢失正确的抵达,如果这个消息状态是0,那么进行重发机制,这种就可以防止MQ宕机网络故障而造成服务无法被写入的问题;然后就有一个定时器专门去判断你这个重发的次数,而一旦达到2次那么就说明这个消息有异常,而一旦有异常我们就把这个消息状态改为2,然后我们通过人工去解决排查;而说明有异常说明数据本身就是有问题,那么这就很好的排除一些问题了,很精准的定位到错误的问题; 这样很好的解决MQ宕机或者服务出现故障而数据的丢失,从而保证消息的可靠生产;



不可靠生产问题:

![image-20211029200851622](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211029200851622.png)

如果这个时候MQ服务器出现了异常和故障，那么消息是无法获取到回执信息。怎么解决呢？

![image-20211029203108391](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211029203108391.png)

```
解释:
	用户下了一个订单然后发送消息,而这个消息可能会存在一个不可靠比如MQ可能出现宕机,我们为了保证数据一定会发生在MQ中,在同一个事务中会增加一个冗余表的来记录订单的信息和是否发送成功的状态,默认状态为0,如果发送成功以后那么就会利用MQ的确认机制来调用数据的回执修改,然后将状态标识为1,这个消息就是一个正常的消息,而如果这个MQ出现故障那么你的订单消息会进行一个冗余,我们会用一个定时器进行定时重发来保证这个闭环!也就是保证了一个可靠生产的问题;
```

#### 5.3.2 可靠消费(面)

我们已经把基于MQ分布式事务可靠生产的问题已经解决了,可靠生产问题我们可以通过消息冗余的方式和定时任务的方式来处理和解决,利用的是MQ中提供的消息确认机制和发布与订阅机制来完成消息的可靠生产,其实可靠生产就是把消息投递到消息队列,消息队列就会给你一个回执,告诉你成功把消息成功投递到消息队列的过程!



**概念**:可靠消费的问题就是在可靠生产的前提下你的消息能正常放到MQ中,而MQ则将消息投递到运单中心,而运单就开始一系列的操作:

![image-20211029210022476](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211029210022476.png)

面试题:"消费者在消息过程出现异常会发生什么情况":

​	如果在运单中心的过程中国发生了故障异常,那么我们就会使用手动ACK机制,因为出现故障的话就会出现死循环,因为默认情况下消费者在消费消息的过程中出现了异常和故障,那么就会触发重发MQ中的重试机制,而这个重试机制就会引发死循环!然后磁盘和内存都会消耗殆尽,直到我们的程序宕机为止;

解决消息重试的方案:

1. 控制重发的次数
2. try+catch+手动ACK
3. try+catch+手动ACK+死信队列处理

yaml配置:

```yaml
spring:
	rabbitmq:
		....
		listener:
        	simple:
        		acknowledge-mode: manual #这里是开启手动ack，让程序去控制MQ的消息的重发和删除和转移
        		retry:
        			enabled: true # 开启重试
        			max-attempts: 10 # 最大重试次数
        			initial-interval: 2000ms # 重试间隔时间
```

```java
//手动应答
channel.basicNack(tag,false,true)
```



> 基于MQ的分布式事务消息的消息重发

![image-20211030115344388](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211030115344388.png)



> 基于MQ的分布式事务消息的死信队列消息转移+人工转移

![image-20211030115427927](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211030115427927.png)

> 如果死信队列报错就进行人工处理

![image-20211030115455903](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211030115455903.png)

### 5.4 总结

分布式事务就是指两个独立的服务之间如何解决数据的一致性问题!解决方案有很多推荐还是使用MQ消息来实现,MQ作为一个单独的消息服务可以很好的搭建起两个服务之间的消息桥梁,但是单纯的使用MQ是无法达成和很好的处理,我们就得必须要借助一些MQ为我们提供的机制如:

​	消息确认机制,而这个确认机制就能很好的帮我们解决订单是否正常抵达到MQ的一个解决方案,我们可以利用这个机制在本地建立一个消息冗余表来标记我的消息是否正常存储到下消息队列中里面去,如果正常存储一旦有消息到交换机那么交换机就会给我们一个回执,也就是到消息队列中会进行一个手动应答然后修改本地冗余表中状态,这样就代表是一个可靠投递和可靠生产的闭环,但是要注意我们得在配置文件中开启ACK手动应答,开启成功才会出现我们的消息确认机制;

​	还有我们的可靠消费,也就是说可能消费者在消费消息过程中出现了故障问题,消息不能得到一个正常的消费,这个时候就会陷入一个死循环的一个过程,因为MQ默认会不停的重试分发消息!这个时候我们就要一些应对机制:

1.控制重发的次数+死信队列 

- 达到一定的重试次数那么就把消息推到死信队列中

2:try+catch+手动ack 

- 手动ACK应答机制来进行不重试,但是要注意如果使用了try+catch之后那么配置文件中的重试次数就变得毫无意义!也就是失效了;

  ```bash
  # 参数1:消息的tag 
  # 参数2:false 多条处理 
  # 参数3:request 重发 
  #  		true:默认会死循环的重发,建议使用try+catch否则就会造成死循环
  # 		false:不会重发,会把消息打入到死信队列中!
  finalChannel.basicAck(delivery.getEnvelope().getDeliveryTag(), false, false);
  ```

  通过try+catch将获取的异常消息打入到死信队列中,然后通过死信消费者去监听获取进行一些单独的消息异常处理!

3:try+catch+手动应答+死信队列处理+人工干预;



> 基于MQ的分布式事务解决方案优点:

1. 通用性强
2. 拓展方便
3. 耦合度低,方案也比较成熟

> 基于MQ的分布式事务解决方案缺点:

1. 基于消息中间件,只适合异步场景
2. 消息会延迟处理,需要业务上能够容忍
   - 因为不是单体架构所以消息在投递到消费者的时候有一定的网络波动等原因!

> 建议

1. 尽量避免分布式事务
2. 尽量将非核心业务做成异步

理解:比如订单和支付就可能会出现分布式事务,那么我们尽量的去把他们放在一个系统中不要去做拆分,能够整合的就不要去拆分,放在一个事务中去控制!同时在一个系统服务中存放也会减少我们的服务器成本问题,这也是解决分布式事务的另一种解决办法!

而非核心业务做成异步,比如像发消息、发邮件、发短信等不重要的东西就可以用MQ异步的方式,因为哪怕是网络波动延迟等原因也不会造成太大的影响！但是终究还是尽量的去规避分布式事务，因为分布式事务至今也没有一个完美的解决方案！

## 6. 消息确认机制的配置

消息确认就是判断一个消息是否正常存储到消息队列的一个确认机制!

首先yaml配置:

```yaml
# 服务端口
server:
  port: 8080
# 配置rabbitmq服务
spring:
  rabbitmq:
    username: admin
    password: admin
    virtual-host: /
    host: 127.0.0.1
    port: 5672
    publisher-confirm-type: correlated
```

**注意**:

1. none值是禁用发布确认模式，是默认值
2. correlated值是发布消息成功到交换器后会触发回调方法
3. SIMPLE值经测试有两种效果:
   - 其一效果和correlated值一样会触发回调方法
   - 其二在发布消息成功后使用rabbitTemplate调用waitForConfirms或waitForConfirmsOrDie方法等待broker节点返回发送结果，根据返回结果来判定下一步的逻辑，要注意的点是waitForConfirmsOrDie方法如果返回false则会关闭channel，则接下来无法发送消息到broker;

实际DEMO:

```java
package com.xuexiangban.rabbitmq.springbootorderrabbitmqproducer.callback;
import org.springframework.amqp.rabbit.connection.CorrelationData;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.stereotype.Component;
/**
 * @description:
 * @author: xuke
 * @time: 2021/3/5 23:25
 */
public class MessageConfirmCallback implements RabbitTemplate.ConfirmCallback {
    @Override
    public void confirm(CorrelationData correlationData, boolean ack, String cause) {
        if(ack){
            System.out.println("消息确认成功!!!!");
        }else{
            System.out.println("消息确认失败!!!!");
        }
    }
}
```

```java
/**
     * @Author xuke
     * @Description 模拟用户购买商品下单的业务
     * @Date 22:26 2021/3/5
     * @Param [userId, productId, num]
     * @return void
     **/
    public void makeOrderTopic(String userId,String productId,int num){
        // 1: 根据商品id查询库存是否充足
        // 2: 保存订单
        String orderId = UUID.randomUUID().toString();
        System.out.println("保存订单成功：id是：" + orderId);
        // 3: 发送消息
        //com.#  duanxin
        //#.email.* email
        //#.sms.# sms
        // 设置消息确认机制
        rabbitTemplate.setConfirmCallback(new MessageConfirmCallback());
        rabbitTemplate.convertAndSend("topic_order_ex","com.email.sms.xxx",orderId);
    }
```

## 7.总结

​	我们学习了那么多知识,我们也知道什么是中间件、中间件具有哪些特征和行为，比如说中间件必须要具备协议，还要具备持久化，因为我们的未来的消息要投递到这些中间件里面去，那么这些中间件是否要具备存盘的功能，还有消息的分发机制，并且还要具有一种高可靠、高可用这些特征;

高可靠其实就体现了持久化和容错处理上,高可用其实就体现了是否支持集群的功能;

> RabbitMQ是一个遵循AMQP协议的基于ErLang语言开发的高可靠的,高性能的,高可用的开源免费特征的消息中间件

这是RabbitMQ的核心组成部分:

![img](https://gitee.com/miawei/pic-go-img/raw/master/imgs/kuangstudy62a1f9e3-027d-408a-8fb4-a176bd184d23.png)

​	看这个首先你得有一个服务,这个服务我们统称为Broker,在这个服务上一层构建的话必须要有一个虚拟机也可理解为一个根节点也就是相当于我们电脑里面的C盘D盘一样的概念,这些概念构成以后在这里面就有一个交换机,因为ErLang语言本身就是基于交换机的一种语言编写的方式所以它是专门编写交换机的,然后有了交换机有了队列两者之间形成了一种绑定关系,所以就有了Bindings,如果消费者有很多我们要进行过滤所以我们就有了路由keyRoutingKey来进一步的过滤和筛选;然后接下来就有生产者和消费者,其中生产者也就是客户端负责开辟连接通道,通道呢负责将消息投递到交换机里面,而交换机必须明确一个概念:在没有指定交换机的队列的话,那么它一定会走一个默认的交换机,也就是说要记住一点消息的投递永远投放在交换机上;

​	在我们在代码程序里实现的时候,如果写路由key的时候没有指定路由key而是指定的是队列名,那么一定是走的默认交换机,而默认交换机的类型是direct路由模式;然后我们要知道消息投递到交换机以后,其中投放消息的能力和处理消息的能力全部都是基于通道,其中声明和注册;



在交换机的分发策略中有一个work模式:

1. 轮询分发

   - 是一种按均分配;无论消费者之间的消息处理数据的能力的快与慢都不会影响消息的均发规则

2. 公平分发

   - 更多讲的是多劳多得,也就是一种按劳分配,谁快谁就多得,但是要记住一点要完成公平分发的话就要手动ACK!qos一定要暴露出来,同时ack机制要改为手动应答false;



在使用场景中要知道MQ是一种异步的消息队列,其核心就是多线程的分发机制,也就是处理消息的时间都是并行的,所以它具有削峰异步、解耦这些特点,并且它具有高内聚低耦合的优势;场景比如有派发订单的流程给指定服务发送消息,如:发邮件、发消息、微信通知等等;



​	我们在使用的过程中要注意生产者在往一个不存在的交换机里面去投递这个消息会出现异常的!这个时候就会有一个声明和注册的问题!

有三种:

1. 图形化界面去手动的将交换机和队列进行绑定,在代码里面就不需要去绑定和声明关系,-----不推荐
2. 在代码里面通过一个配置类的方式去声明和绑定 ----推荐,可扩展性强
3. 在代码里面通过一个注解的方式去声明和绑定 ----- 在分布式环境中对于过期时间的配置不太友好 - 不推荐



消息变为死信队列有三种原因:

1. 消息被拒绝
   - 被拒绝也就是在调用NACK,在我们学习的分布式事务中就用到了消息的NACK,一旦被拒绝就会把消息打入到死信队列中!
2. 消息过期
   - 给消息设置一个过期时间,一旦过期就会打入到死信队列中
3. 队列达到最大长度
   - 队列中消息最大数量,一旦达到指定数量就会打入到死信队列中!

当然要要打入死信队列我们还需要在定义队列的时候设置队列参数`x-dead-letter-exchange`指定交换机即可!

注意:既然是打入到死信队列中那么我们需要指定一个死信消费者来进行监听,进而对其中的消息进行消费!



磁盘空间:

默认占用内存相对为:0.4,也就是我们物理内存的0.4倍,一旦超过0.4那么就会出现红色blocked也就意味了磁盘空间已经不足了!那么这个时候就无法使用生产者生产消息进行投递了!



消息中间件它的分布式事务解决的方案就是为了解决数据的最终一致性的问题;分布式事务就是两个不同的服务要保持数据的一致性和整体性!要完成同时成功和同时失败!





