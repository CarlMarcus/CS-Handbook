Java热部署：https://www.jianshu.com/p/731bc8293365 https://www.jianshu.com/p/0bbd79661080 https://www.jianshu.com/p/6096bfe19e41

一个类加载器只能加载一个同名类，在Java默认的类加载器层面作了判断，如果已经有了该类，则不再重复加载。但不同的类加载器是可以加载同名的类的，加载完成之后，这两个类虽然同名，但不是同一个 Class 对象，无法进行转换。使用自定义的类加载器，加载一个类，当需要进行替换类的时候，我们就丢弃之前的类加载器和类，使用新的类加载器去加载新的 Class 文件，然后运行新对象的方法。热部署的方法1。缺点-----不够灵活。需要手动修改文件等操作。

在 JDK 1.5 中，Java 引入了 java.lang.Instrument 包，该包提供了一些工具帮助开发人员在 Java 程序运行时，动态修改系统中的 Class 类型。使用该软件包的一个关键组件就是 Java agent。实际上，他的功能像是一个Class 类型的转换器，他可以在运行时接受重新外部请求，对Class 类型进行修改。

参数 javaagent 可以用于指定一个 jar 包，并且对该 java 包有2个要求：

1. 这个 jar 包的MANIFEST.MF 文件必须指定 Premain-Class 项。
2. Premain-Class 指定的那个类必须实现 premain（）方法。

JVM 会先执行 premain 方法，大部分类加载都会通过该方法。注意：是大部分，不是所有。当然，遗漏的主要是系统类，因为很多系统类先于 agent 执行，而用户类的加载肯定是会被拦截的。也就是说，这个方法是在 main 方法启动前拦截大部分类的加载活动。premain 只能在类加载之前修改字节码，还需要在启动的时候增加参数，类加载之后无能为力，只能通过重新创建 ClassLoader 这种方式重新加载。





Jmx_exporter是以代理的形式收集目标应用的jmx指标，这样做的好处在于无需对目标应用做任何的改动。

-javaagent:/path/to/JavaAgent.jar=port(一般是7070):/config_path/JMX_config.yml

http://serverIP:port/metrics 可以看到metrics信息。

JMX是一个为应用程序植入管理功能的框架，我个人的理解是JMX让程序有被管理的功能

JMX的结构一共分为三层，1、基础层：主要是MBean，被管理的资源。2、适配层：MBeanServer，主要是提供对资源的注册和管理。3、接入层：提供远程访问的入口。 

创建xxxMBean接口，提供setter和getter，然后实现xxxxMBean接口，类名就是xxxx，再把它注册到MBeanServer中，进行管理。

jmx exporter过程：

agentArgument 是通过命令行传入的参数，从中解析出IP地址host，端口port，配置文件file

注册收集器，build info 用于收集 jxm_exporter的版本信息

collector 主要收集jmx_exporter.yaml文件中指定的对象的jmx指标。

收集的信息放在这个里 MetricFamilySamples。

这个类主要由以下几个属性：

1、	Name 指标名称
2、	Collector.Type 指标的类型：
a)	COUNTER
b)	GAUGE
c)	SUMMARY
d)	HISTOGRAM
e)	UNTYPED
BuildInfoCollector收集的指标类型是GAUGE
3、	Help 指标的说明
4、	List<Collector.MetricFamilySamples.Sample> samples 指标的样本集合
看看样本Sample的结构 **io.prometheus.client.Collector.MetricFamilySamples.Sample**

属性介绍：

- 1、	String name 样本名称
- 2、	List labelNames 标签名称
- 3、	List labelValues 标签值
- 4、	double value 样本值
- 5、	Long timestampMs 时间戳

jmx collector过程：

1首先判断配置文件是否有被修改，如果被修改就重新加载，可以看出jmx_exporter的配置是热加载的。
2等待开始收集jmx指标，等待的时间由配置startDelaySeconds指定。
3开始收集jmx指标，scraper.doScrape(); 这个方法会收集配置文件中指定的对象的信息，并且根据配置的正则表达式进行过滤：首先根据有户名，密码等认证信息获取jmx的链接beanConn；	根据配置中的白名单whitelistObjectNames，黑名单blacklistObjectNames确定需要获取的对象名单；挨个去获取名单中的对象信息scrapeBean(beanConn, objectName);



hbase性能指标：主要考虑各个regionServer是否负载均衡，有没有哪个服务器占用特别高或特别低，有没有卡死

[HDFS工作原理解析](https://mp.weixin.qq.com/s?__biz=MzIzMTE1ODkyNQ==&mid=2649411210&idx=1&sn=749f6a034d91ed3292a9f7167dee9c41&scene=19#wechat_redirect)      [HBase存储原理解析](https://mp.weixin.qq.com/s?__biz=MzIzMTE1ODkyNQ==&mid=2649411489&idx=1&sn=cc830718ac8af0287a0f2c4a880a87c9&scene=19#wechat_redirect)      [hbase指标收集](https://www.zybuluo.com/cyfcooler/note/1494848)       [Hbase 日常运维监控性能指标调优](https://blog.csdn.net/wangshuminjava/article/details/80575875)

[Hbase架构与原理](https://zhuanlan.zhihu.com/p/29674705)        [深入浅出hbase和bigtable](https://zhuanlan.zhihu.com/p/33990921)          [HBase原理-RegionServer宕机数据恢复](https://zhuanlan.zhihu.com/p/27885715)

对于region来说，一般要关注split、flush和compact，尤其是compact对性能影响很大，太多compact容易导致不可用，一般凌晨定时compact，对于regionserver来说，region数量、大小、文件本地化比例、请求数量、连接队列、GC时间等是主要的监控指标，都和负载均衡相关。blockCache也很重要，和查询性能关系很大。对于物理机来说，主要关注磁盘io，网络io，iowait、cpu占用、内存占用。

通常较少的region数量可使群集运行的更加平稳，官方指出每个RegionServer大约100个regions的时候效果最好。单个region最佳大小5-10GB。[2-Hbase最佳实战：Region数量与大小的重要影响](https://zhuanlan.zhihu.com/p/27800787) [HBase最佳实践－集群规划](http://hbasefly.com/2016/08/22/hbase-practise-cluster-planning/?jezmvi=zscvp1)

『MSLAB』MemStore-Local Allocation Buffer

每个store（相当于一个列族）都有一个memstore（LSM第一层），为了避免在大量数据写入，堆中产生很多碎片，导致stop-the-world GC出现，设置hbase.hregion.memstore.mslab.enabled，来预防此问题。即本地MemStore允许分配的内存大小。