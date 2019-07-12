HSF
--

+ HSF是使用diamond来实现规则的存储和通知,对于注册中心使用的是ConfigServer,阿里自己的一个产品，堪比zookeeper.
+ hsf使用ServiceLoader来实现隐藏实现细节，完成服务实例加载。
+ hsf通过export进行服务的发布，unexport进行取消发布，refer进行服务订阅，unrefer取消订阅。发布时启动HSFServer，生成ServiceURL，订阅服务时返回能进行远程调用的InvocationHandler。

