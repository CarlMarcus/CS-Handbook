### Spring面试题

[15个经典的Spring面试常见问题](https://zhuanlan.zhihu.com/p/68191247)

> 1. 什么是Spring框架?
> 2. 列举一些重要的Spring模块？
> 3. 谈谈自己对于Spring IoC和AOP的理解
> 4. Spring AOP 和 AspectJ AOP 有什么区别？
> 5. Spring 中的bean的作用域有哪些?
> 6. Spring 中的bean生命周期?
> 7. Spring 中的单例 bean 的线程安全问题了解吗？
> 8. 说说自己对于Spring MVC了解?
> 9. SpringMVC 工作原理了解吗?
> 10. Spring 框架中用到了哪些设计模式？
> 11. @Component 和 @Bean 的区别是什么？
> 12. 将一个类声明为Spring的 bean 的注解有哪些?
> 13. Spring管理事务的方式有几种？
> 14. Spring事务中的隔离级别有哪几种?
> 15. Spring事务中哪几种事务传播行为?

[49个Spring经典面试题总结，附带答案! (一)](https://zhuanlan.zhihu.com/p/62905881)     [49个Spring经典面试题总结，附带答案! (二)](https://zhuanlan.zhihu.com/p/62906906)

> 1. 不同版本的 Spring Framework 有哪些主要功能？
>
> 2. 什么是 Spring Framework？
>
> 3. 列举 Spring Framework 的优点。
>
> 4. Spring Framework 有哪些不同的功能？
>
> 5. Spring Framework 中有多少个模块，它们分别是什么？
> 6. 什么是 Spring 配置文件？
> 7. Spring 应用程序有哪些不同组件？
> 8. 使用 Spring 有哪些方式？
> 9. 什么是 Spring IOC 容器？
> 10. 什么是依赖注入？	
> 11. 可以通过多少种方式完成依赖注入？
> 12. 区分构造函数注入和 setter 注入。
> 13. Spring 中有多少种 IOC 容器？
> 14. 区分 BeanFactory 和 ApplicationContext。
> 15. 列举 IoC 的一些好处。
> 16. Spring IoC 的实现机制。
> 17. 什么是 Spring bean？
> 18. Spring 提供了哪些配置方式？
> 19. Spring 支持集中 bean scope？
> 20. Spring bean 容器的生命周期是什么样的？
> 21. 什么是 Spring 的内部 bean？
> 22. 什么是 Spring 装配？
> 23. 自动装配有哪些方式？
> 24. 自动装配有什么局限？
> 25. 你用过哪些重要的 Spring 注解？
> 26. 如何在 Spring 中启动注解装配？
> 27. @Component, @Controller, @Repository, @Service 有何区别？
> 28. @Required 注解有什么用？
> 29. @Autowired 注解有什么用？
> 30. @Qualifier 注解有什么用？
> 31. @RequestMapping 注解有什么用？



**基础篇**

**Spring 概 述**

1. 什 么 是 spring?
2. 使 用 Spring 框 架 的 好 处 是 什 么 ？
3. Spring 由 哪 些 模 块 组 成?
4. 核 心 容 器 （ 应 用 上 下 文) 模 块 。
5. BeanFactory – BeanFactory 实 现 举 例 。
6. XMLBeanFactory
7. 解 释 AOP 模 块
8. 解 释 JDBC 抽 象 和 DAO 模 块 。
9. 解 释 对 象/关 系 映 射 集 成 模 块 。
10. 解 释 WEB 模 块 。
11. Spring 配 置 文 件
12. 什 么 是 Spring IOC 容 器 ？
13. IOC 的 优 点 是 什 么 ？
14. ApplicationContext 通 常 的 实 现 是 什 么?
15. Bean 工 厂 和 Application contexts 有 什 么 区别 ？
16. 一 个 Spring 的 应 用 看 起 来 象 什 么 ？
17. 什 么 是 Spring 的 依 赖 注 入 ？
18. 有 哪 些 不 同 类 型 的 IOC（ 依 赖 注 入 ） 方 式 ？
19. 哪 种 依 赖 注 入 方 式 你 建 议 使 用 ， 构 造 器 注 入 ， 还 是Setter 方 法 注入

**Spring Beans**

1. 什么是spring beans
2. 一 个 Spring Bean 定 义 包 含 什 么 ？
3. 如 何 给 Spring 容 器 提 供 配 置 元 数 据?
4. 你 怎 样 定 义 类 的 作 用 域?
5. 解 释 Spring 支 持 的 几 种 bean 的 作 用 域 。
6. Spring 框 架 中 的 单 例 bean 是 线 程 安 全 的 吗?
7. 解 释 Spring 框 架 中 bean 的 生 命 周 期 。
8. 哪 些 是 重 要 的 bean 生 命 周 期 方 法 ？ 你 能 重 载 它 们吗 ？
9. 什 么 是 Spring 的 内 部 bean？
10. 在 Spring 中 如 何 注 入 一 个 java 集 合 ？

**String 类 型 。**

1. 什 么 是 bean 装 配?
2. 什 么 是 bean 的 自 动 装 配 ？
3. 解 释 不 同 方 式 的 自 动 装 配 。
4. 自 动 装 配 有 哪 些 局 限 性 ?
5. 你 可 以 在 Spring 中 注 入 一 个 null 和 一 个 空 字 符 串吗 ？

**Spring 注 解**

1. 什 么 是 基 于 Java 的 Spring 注 解 配 置? 给 一 些 注 解的 例 子.
2. 什 么 是 基 于 注 解 的 容 器 配 置?
3. 怎 样 开 启 注 解 装 配 ？
4. @Required 注 解
5. @Autowired 注 解
6. @Qualifier 注 解

**Spring 数 据 访 问**

1. 在 Spring 框 架 中 如 何 更 有 效 地 使 用 JDBC?
2. JdbcTemplate
3. Spring 对 DAO 的 支 持
4. 使 用 Spring 通 过 什 么 方 式 访 问 Hibernate?
5. Spring 支 持 的 ORM
6. 如 何 通 过 HibernateDaoSupport 将 Spring 和
7. Spring 支 持 的 事 务 管 理 类 型
8. Spring 框 架 的 事 务 管 理 有 哪 些 优 点 ？
9. 你 更 倾 向 用 那 种 事 务 管 理 类 型 ？

**Spring 面 向 切 面 编 程 （AOP）**

1. 解 释 AOP
2. Aspect 切 面
3. 在 Spring AOP 中 ， 关 注 点 和 横 切 关 注 的 区 别 是 什么 ？
4. 连 接 点
5. 通 知
6. 切 点
7. 什 么 是 引 入?
8. 什 么 是 目 标 对 象?
9. 什 么 是 代 理?
10. 有 几 种 不 同 类 型 的 自 动 代 理 ？
11. 什 么 是 织 入 。 什 么 是 织 入 应 用 的 不 同 点 ？
12. 解 释 基 于 XML Schema 方 式 的 切 面 实 现 。
13. 解 释 基 于 注 解 的 切 面 实 现

**Spring 的 MVC**

1. 什 么 是 Spring 的 MVC 框 架 ？
2. DispatcherServlet
3. WebApplicationContext
4. 什 么 是 Spring MVC 框 架 的 控 制 器 ？
5. @Controller 注 解
6. @RequestMapping 注 解

**高级篇**

1、什么是 Spring 框架？Spring 框架有哪些主要模块？

2、使用 Spring 框架能带来哪些好处？

3、什么是控制反转(IOC)？什么是依赖注入？

4、请解释下 Spring 框架中的 IoC？

5、BeanFactory 和 ApplicationContext 有什么区别？

6、Spring 有几种配置方式？

7、如何用基于 XML 配置的方式配置 Spring？

8、如何用基于 Java 配置的方式配置 Spring？

9、怎样用注解的方式配置 Spring？

10、请解释 Spring Bean 的生命周期？

11、Spring Bean 的作用域之间有什么区别？

12、什么是 Spring inner beans？

13、Spring 框架中的单例 Beans 是线程安全的么？

14、请举例说明如何在 Spring 中注入一个 Java Collection？

15、如何向 Spring Bean 中注入一个 Java.util.Properties？

16、请解释 Spring Bean 的自动装配？

17、请解释自动装配模式的区别？

18、如何开启基于注解的自动装配？

19、请举例解释@Required 注解？

20、请举例解释@Autowired 注解？

21、请举例说明@Qualifier 注解？

22、构造方法注入和设值注入有什么区别？

23、Spring 框架中有哪些不同类型的事件？

24、FileSystemResource 和 ClassPathResource 有何区别？

25、Spring 框架中都用到了哪些设计模式？

**高级篇二**

1.谈谈你对 spring IOC 和 DI 的理解，它们有什么区别？

2.BeanFactory 接口和 ApplicationContext 接口有什么区别 ？

3.spring 配置 bean 实例化有哪些方式？

4.简单的说一下 spring 的生命周期？

5.请介绍一下 Spring 框架中 Bean 的生命周期和作用域

6.Bean 注入属性有哪几种方式？

7.什么是 AOP，AOP 的作用是什么？

8.Spring 的核心类有哪些，各有什么作用？

9.Spring 里面如何配置数据库驱动？

10.Spring 里面 applicationContext.xml 文件能不能改成其他文件名？

11.Spring 里面如何定义 hibernate mapping？

12.Spring 如何处理线程并发问题？

13 .介 绍 一 下 S p r i n g 的 事 物 管 理 事 务 就 是 对 一 系

14.解释一下 Spring AOP 里面的几个名词

15.通知有哪些类型？

[面试官会问关于spring的哪些问题？](https://www.zhihu.com/question/39814046)

[什么才叫懂Spring底层原理，这些面试题你都会吗](https://zhuanlan.zhihu.com/p/38484238)

[Spring系列之DI的原理及手动实现](https://zhuanlan.zhihu.com/p/68686091)

[Spring事务面试考点吐血整理](https://zhuanlan.zhihu.com/p/62457620)



### Mybatis

[面试官都会问的Mybatis面试题，你会这样回答吗？](https://zhuanlan.zhihu.com/p/67828781)

[Java面试---2018年MyBatis常见实用面试题整理](https://zhuanlan.zhihu.com/p/44464109)

> 1. **什么是Mybatis？ ** 
> 2. **Mybaits的优点？**
> 3. **MyBatis框架的缺点**
> 4. **MyBatis框架适用场合**
> 5. **MyBatis与Hibernate有哪些不同？**
> 6. **#{}和${}的区别是什么？**
> 7. **当实体类中的属性名和表中的字段名不一样 ，怎么办 ？**
> 8. **模糊查询like语句该怎么写?**
> 9. **通常一个Xml映射文件，都会写一个Dao接口与之对应，请问，这个Dao接口的工作原理是什么？Dao接口里的方法，参数不同时，方法能重载吗？**
> 10. **Mybatis是如何进行分页的？分页插件的原理是什么？**
> 11. **Mybatis是如何将sql执行结果封装为目标对象并返回的？都有哪些映射形式？**
> 12. **如何执行批量插入?**
> 13. **如何获取自动生成的(主)键值?**
> 14. **在mapper中如何传递多个参数?**
> 15. **Mybatis动态sql有什么用？执行原理？有哪些动态sql？**
> 16. **Xml映射文件中，除了常见的select|insert|updae|delete标签之外，还有哪些标签？**
> 17. **简述Mybatis的Xml映射文件和Mybatis内部数据结构之间的映射关系？**
> 18. **Mybatis的Xml映射文件中，不同的Xml映射文件，id是否可以重复？**
> 19. **什么是MyBatis的接口绑定,有什么好处？**
> 20. **接口绑定有几种实现方式,分别是怎么实现的?**
> 21. **什么情况下用注解绑定,什么情况下用xml绑定？**
> 22. **为什么说Mybatis是半自动ORM映射工具？它与全自动的区别在哪里？**
> 23. **当实体类中的属性名和表中的字段名不一样，如果将查询的结果封装到指定pojo？**
> 24. **一对一、一对多的关联查询 ？**
> 25. **MyBatis实现一对一有几种方式?具体怎么操作的？**
> 26. **MyBatis实现一对多有几种方式,怎么操作的？**
> 27. **Mybatis是否支持延迟加载？如果支持，它的实现原理是什么？**
> 28. **Mybatis的一级、二级缓存:**
> 29. **使用MyBatis的mapper接口调用时有哪些要求？**
> 30. **Mapper编写有哪几种方式？**
> 31. **简述Mybatis的插件运行原理，以及如何编写一个插件。**
> 32. **Mybatis中如何执行批处理？**
> 33. **Mybatis都有哪些Executor执行器？它们之间的区别是什么？**
> 34. **Mybatis中如何指定使用哪一种Executor执行器？**
> 35. **Mybatis是否可以映射Enum枚举类？**
> 36. **如何获取自动生成的(主)键值？**
> 37. **resultType resultMap的区别？**
> 38. **使用MyBatis的mapper接口调用时有哪些要求？**
> 39. **Mybatis比IBatis比较大的几个改进是什么？**
> 40. **IBatis和MyBatis在核心处理类分别叫什么？**
> 41. **IBatis和MyBatis在细节上的不同有哪些？**
