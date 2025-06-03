## Spring Cloud Gateway是什么？
网关作为流量的入口，常用的功能包括路由转发，权限校验，限流等。
Spring Cloud Gateway是Spring Cloud官方推出的第二代网关框架，定位于取代Nextifix Zuul。相比于Zuul来说，Spring Cloud Gateway提供了更优秀的性能，更强大的功能。
Spring Cloud Gateway是由WebFlux+Netty+Reactor实现的响应式的API网关。它不能在传统的servlet容器中工作，也不能构建成war包。


## Spring Cloud Gateway有哪些核心概念？
- 路由 Route
路由是网关中最基础的部分，路由信息包括一个ID、一个目的URI、一组断言工厂、一组Filter组成。如果断言为真，则说明请求的URL和配置的路由匹配。
- 断言 predicate
Java8中的断言函数，SpringCloud Gateway中的断言函数类型是Spring5.0框架中的ServerWebExchange。断言函数允许开发者去定义匹配Http request中的任何信息，比如请求头和参数等。
- 过滤器 filter
SpringCloud Gateway中的filter分为Gateway FilIer和Global Filter。Filter可以对请求和响应进行处理。

## Spring Cloud GateWay的工作流程是什么？



