## Sentinel是什么？如何使用的？
Sentinel是流量治理组件，可以实现流量控制、流量整形、熔断降级等功能。
在项目中通过注解通过和OpenFeign结合使用。在配置文件中配置`feign.sentinel.enabled=true`
我们使用的是`1.8.0`以上版本。

## Sentinel和Hystrix的区别？
- 实现理念
    - Sentinel是以流量作为切入点。支持的粒度比Hystrix更细。可以细化到一个接口、一段代码。
    - Hystrix是以服务保护为基础。
- 隔离策略
    - Sentinel使用信号量
    - Hystrix使用线程池
- 熔断降级策略
    - Sentinel基于响应时间或失败比
    - Hystrix基于失败比
- 限流模式
    - Sentinel可以配置多种方式的限流


## Sentinel是怎么实现限流的？
在Sentinel中，所有的资源都对应一个资源名称和一个Entry。每个Entry创建时，也会创建一系列的功能插槽也就是slot chain。
有NodeSelectorSlot，负责收集资源的调用路径的。有StatisticSlot，用于记录，统计不同维度的runtime指标。
其中的FlowSlot，用于流量控制。Sentinel通过这个slot来实现限流功能的。
这里的流量控制主要有两种统计类型，一种是基于统计线程数的，一种是基于统计QPS的。

## Sentinel是怎么实现熔断降级的？
出现的场景：调用链中不稳定的资源可以进行熔断。例如第三方API（支付API等）
在Sentinel中，熔断降级是通过`BlockException`异常来实现的。当调用资源时，如果资源被熔断降级了，就会抛出`BlockException`异常。
熔断降级主要是在DegradeSlot中实现的。具体实现如下：
首先，每个Slot都有四个方法分别是：`entry`、`fireEntry`、`exit`、`fireExit`。<br/>
`entry`表示进入这个Slot，`fireEntry`表示执行完当前slot的entry方法，一般用来执行下一个slot的entry，`exit`表示退出这个Slot，`fireExit`表示执行完当前slot的exit，一般用来调用下一个slot的exit。
在DegradeSlot中的`entry`方法中，有一个`performChecking`的方法，该方法会依次判断配置的断路器的tryPass方法，该方法就是判断断路器的状态是不是close的，close就直接返回**True**。如果是open的，会判断是否到了retry的时间，并将状态改为半开状态返回**True**，否则就返回**False**。
那断路器的状态就是在在DegradeSlot中的`exit`方法中进行修改的。例如响应时间断路器（ResponseTimeCircuitBreaker）则是在请求完成后，判断响应时间是否大于设置的阈值，并累加到满响应的计数器中。如果在当慢响应的次数占到总响应的一定比例后，就会将断路器的状态改为open，来实现熔断降级的功能。

## Sentinel是怎么实现集群限流的？


## 限流算法有哪些？
在Sentinel中，限流实现的功能是在类`StatisticSlot`中实现的。
具体的算法有：
- 计数器算法
- 滑动窗口算法
- 漏桶算法
- 令牌桶算法


