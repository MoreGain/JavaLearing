# RabbitMQ

### 传递模式

点对点模式

发布/订阅模式

### 应用场景

解耦

削锋

异步

### 生产者和消费者

Producer: 生产者

Consumer: 消费者

Broker: 消息中间件的服务节点

### 队列

消息只能存放在队列中

### 交换器、路由器、绑定

###### Exchange: 交换器

生产者将消息发送到 Exchange，由交换器将消息路由到一个或多个队列

###### RoutingKey: 路由键

生产者将消息发送给交换器时，一般会指定一个 RoutingKey，用来指定消息的路由规则，和交换器与绑定建联合使用

###### Binding: 绑定

通过绑定将交换器与队列联系起来，绑定时一般会指定绑定建(BindingKey)，当 RoutingKey 和 BindingKey 向匹配时，消息会被路由到相应的队列中

###### 交换器类型

- fanout

  把发送到该交换器的消息路由到与该交换器绑定的队列中

- direct

  把消息路由到 BindingKey 与 RoutingKey 完全匹配的队列中

- topic

  它在匹配规则上进行了扩展，它与 direct 类型的交换器相似，将消息路由到 BindingKey 和 RoutingKey 相匹配的队列中，不过有不同的匹配规则，约定：RoutingKey 和 BindingKey 都是一个 “.” 分割的字符串，BindingKey 中可以存在两种特殊的字符串 “\*” “#” ，“\*” 匹配一个单词，“#” 匹配一个或多个单词，如 RoutingKey 为 `com.rabbitmq.client` ，BindingKey 为 `*.rabbitmq.*`

- headers

  不依赖于路由键的匹配规则来路由消息，而是根据发送的消息内容中的 headers 属性进行匹配；性能较差，不实用

- 还有 System 和自定义类型的交换器