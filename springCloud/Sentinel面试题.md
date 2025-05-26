## Sentinel是什么？如何使用的？
Sentinel是流量治理组件，可以实现流量控制、流量整形、熔断降级等功能。
在项目中通过注解通过和OpenFeign结合使用。在配置文件中配置`feign.sentinel.enabled=true`

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



## Sentinel是怎么实现集群限流的？


## 限流算法有哪些？



## Sentinel是怎么实现熔断降级的？