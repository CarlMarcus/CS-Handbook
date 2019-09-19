### MyBatis面试题

#### 1 什么是Mybatis？

> 答：Mybatis是一个orm类型的半自动框架，执行了对JDBC的封装，是一个持久层框架，它可以通过XML文件或者注解来配置原生信息，不在需要去做更多繁琐重复的过程，如创建连接，加载驱动！

#### 2 说一下Mybaits的优缺点和使用场合

> 答：
> 优点：基于SQL语句编译，相当灵活，与JDBC相比，减少了50%的代码，很好的与各种数据库兼容，能够与Spring很好的集成，提供映射标签，支持对象关系组件维护！
> 缺点：SQL语句的编写工作量较大，尤其字段多，关联表多时，对开发人员编写SQL语句的功底有一定要求！
> SQL语句依赖于数据库，导致数据库移植性差，不能随意更换数据库！
>
> 适用场合：MyBatis专注于SQL本身，是一个足够灵活的DAO层解决方案，对性能要求很高，或者需求变化较多的项目，如互联网项目！

#### 3 #{}和${}的区别是什么？

> 答：#{}：是预编译处理。
> ${}：是字符串替换。
> Mybatis在处理#{}时，
>
> 会将sql中的#{}替换为？号，调用PreparedStatement的set方法来赋值；mybatis在处理${}时，就是将${}替换成变量的值.
> 使用#{}可以有效的防止SQL注入，提高系统的安全性！

#### 4 当实体类中的属性名和表中的字段名不一样 ，怎么办 ？

> 答：1.通过在查询的sql语句中定义字段名的别名，让字段名的别名和实体类的属性名一致！
> 2.通过<resultMap>来映射字段名和实体类属性名的一一对应关系！

#### 5 通常一个Xml映射文件，都会写一个Dao接口与之对应，请问，这个Dao接口的工作原理是什么？Dao接口里的方法，参数不同时，方法能重载吗？

> 答：Dao接口即Mapper接口，接口的全限名，就是映射文件的namespace的值，接口的方法名，就是映射文件中Mapper的Statement的id值，接口方法内的参数，就是传递个sql的参数！因为mapper接口是没有实现类的，所以在调用方法时，需要拿全限定路径名称加上方法名作为key值！
>
> 不能重载

#### 6 说一下resultMap和resultType

> 答：resultmap是手动提交，人为提交，resulttype是自动提交
> MyBatis中在查询进行select映射的时候，返回类型可以用resultType，也可以用resultMap，resultType是直接表示返回类型的，而resultMap则是对外部ResultMap的引用，但是resultType跟resultMap不能同时存在。
> 在MyBatis进行查询映射时，其实查询出来的每一个属性都是放在一个对应的Map里面的，其中键是属性名，值则是其对应的值。
> 1.当提供的返回类型属性是resultType时，MyBatis会将Map里面的键值对取出赋给resultType所指定的对象对应的属性。所以其实MyBatis的每一个查询映射的返回类型都是ResultMap，只是当提供的返回类型属性是resultType的时候，MyBatis对自动的给把对应的值赋给resultType所指定对象的属性。
> 2.当提供的返回类型是resultMap时，因为Map不能很好表示领域模型，就需要自己再进一步的把它转化为对应的对象，这常常在复杂查询中很有作用。

#### 7 如何在在mapper中如何传递多个参数?

答：多个参数封装成map

> \1. 映射文件的命名空间，SQL片段的ID，就可以调用对应的映射文件的SQL，
> \2. 由于我们的参数超过两个，而方法只有一个Object参数收集，因此我们使用Map集合来装载我们的参数！

#### 8 Mybatis动态sql有什么用？执行原理？有哪些动态sql？

> 答：有九种动态sql标签：trim,where,set,foreach,if,choose,when,bind,otherwise
> Mybatis的动态sql可以在xml映射文件内，以标签的形式编写动态sql，执行原理是根据表达式的值，完成逻辑判断并动态拼接sql的功能！

#### 9 Xml映射文件中，除了常见的select|insert|updae|delete标签之外，还有哪些标签？

> 答：sql，selectkey，resulMap，parameterMap，include

#### 10 Mybatis全局配置文件中有哪些标签?分别代表什么意思?

> 答：
> configuration 配置
> properties 属性:可以加载properties配置文件的信息
> settings 设置：可以设置mybatis的全局属性
> typeAliases 类型命名
> typeHandlers 类型处理器
> objectFactory 对象工厂
> plugins 插件
> environments 环境
> environment 环境变量
> transactionManager 事务管理器
> dataSource 数据源
> mappers 映射器

#### 11 Mybatis的Xml映射文件中，不同的Xml映射文件，id是否可以重复？

> 答：看不同情况对待，不同的xml配置文件，如果配置了namespace，那么id重复，如果没有配置namespace，那么id不能重复！

#### 12 一对一关联查询使用什么标签?一对多关联查询使用什么标签?

> 答：MyBatis中使用collection标签来解决一对多的关联查询，collection标签可用的属性如下：
> property:指的是集合属性的值.
> ofType:指的是集合中元素的类型.
> column:所对应的外键字段名称.
> select:使用另一个查询封装的结果.
>
> MyBatis中使用association标签来解决一对一的关联查询，association标签可用的属性如下：
> property:对象属性的名称.
> javaType:对象属性的类型.
> column:所对应的外键字段名称.
> select:使用另一个查询封装的结果！

#### 13 什么是Mybatis的一级、二级缓存,如何开启?什么样的数据适合缓存?

> 答：一级缓存是基于PerpetualCache的hashmap本地缓存，其存储作用域为Session，当Session flush后，默认打开一级缓存！
> 二级缓存和一级缓存的机制是相同的，默认也是采用PerpetualCache的hashmap本地缓存，不过他的储存作用于在Mapper，而且可自定义存储源，要开启二级缓存，需要使用二级缓存属性类实现Serializable序列化的接口，可在它的映射文件中配置<cache/>
>
> 缓存数据的更新机制，当某一个作用域（一级缓存session/二级缓存namespace）的进行了c/u/d操作后，默认该作用域下所有select中的缓存将被clear

#### 14 什么是MyBatis的接口绑定？有哪些实现方式？

> 答：接口绑定就是在mybatis中任意定义接口，然后把接口里面的方法和sql语句绑定，我们直接调用接口方法就可以，这样比起原来sqlsession提供的方法我们可以有更加灵活的选择和设置！
> 有两种实现方式：
> \1. 在接口的方法上面加上@select，@update等注解，里面包含sql语句来绑定
> \2. 通过xml里面写sql语句来绑定，在这种情况下，要指定xml映射文件里面的namespace必须为接口的全路径名，当sql语句比较简单的时候，用注解绑定，当sql语句比较复杂的时候，用xml绑定，一般使用xml绑定的比较多！

#### 15 使用MyBatis的mapper接口调用时有哪些要求？

> 答：1.接口实现类继承sqlsessionDaosupport，需要编写mapper接口，mapper接口实现类，mapper.xml文件！
> mapper.xml中的namespace为mapper接口的地址
> mapper接口中的方法名和mapper.xml中的定义的statement的id保持一致
> Spring中定义
> 2.使用mapper扫描器

#### 16 MyBatis你只写了接口为啥就能执行SQL啊？

动态代理：MapperProxy类实现了InvocationHandler接口

https://www.cnblogs.com/demingblog/p/9544774.html

https://blog.csdn.net/lu930124/article/details/50991899