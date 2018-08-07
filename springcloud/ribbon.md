### Ribbon 是什么
    Ribbon 是netfix公司开源的一个负载均衡项目，是一个负载均衡器，运行在客户端上，
    它是一个经过了云端测试IPC库，可以很好的控制http和tcp客户端的一些行为。
    Fegin已经默认使用了Ribbon。
    
    - 负载均衡
    - 容错
    - 多协议(tcp,http,udp)支持异步和反应模型
    - 缓存和批处理

### Ribbon 核心类
    LoadBalancerClient: 负载均衡的客户端。它是一个接口，继承ServiceInstanceChooser，
      它的实现类是RibbonLoadBalancerClient(最终的负载均衡的请求处理)。
    DynamicServerListLoadBalancer: 它的继承类为BaseLoadBalancer，内部实现负载均衡配置信息
    IClientConfig: 用于对客户端或者负载均衡的配置，它的默认实现类为DefaultClientConfigImpl。
    IRule: 用于复杂均衡的策略
        IRule有很多默认的实现类，这些实现类根据不同的算法和逻辑来处理负载均衡。
        BestAvailableRule 选择最小请求数
        ClientConfigEnabledRoundRobinRule 轮询
        RandomRule 随机选择一个server
        RoundRobinRule 轮询选择server
        RetryRule 根据轮询的方式重试
        WeightedResponseTimeRule 根据响应时间去分配一个weight ，weight越低，被选择的可能性就越低
        ZoneAvoidanceRule 根据server的zone区域和可用性来轮询选择(默认)
    IPing: 用来想server发生”ping”，来判断该server是否有响应，从而判断该server是否可用。
    LoadBalancerInterceptor: 用于实时拦截，在LoadBalancerInterceptor这里实现来负载均衡

### 总结
      综上所述，Ribbon的负载均衡，主要通过LoadBalancerClient来实现的，而LoadBalancerClient具体交给了ILoadBalancer来处理，ILoadBalancer通过配置IRule、IPing等信息，并向EurekaClient获取注册列表的信息，并默认10秒一次向EurekaClient发送“ping”,进而检查是否更新服务列表，最后，得到注册列表后，ILoadBalancer根据IRule的策略进行负载均衡。
      而RestTemplate 被@LoadBalance注解后，能通过用负载均衡，主要是维护了一个被@LoadBalance注解的RestTemplate列表，并给列表中的RestTemplate添加拦截器，进而交给负载均衡器去处理