`init`，`service`和`destroy`是servlet的生命周期方法，这些方法由web容器调用。

HttpServlet类扩展了`GenericServlet`类并实现了`Serializable`接口。它提供了http特定的方法，如:`doGet`，`doPost`，`doHead`，`doTrace`等。

工作流程：

> 服务器检查servlet**是否为第一次被请求**？
>
> 如果**是第一次**被请求，则 - 
>
> - 加载servlet类。
> - 实例化servlet类。
> - 调用`init`方法传递`ServletConfig`对象
>
> 如果**不是第一次**被请求，则 - 
>
> - 调用`service`方法传递请求和响应对象

传统的使用Servlet来开发web应，如果业务复杂，系统庞大，需要编写大量的Servlet类，这不是我们想要的。想要一个servlet搞定。

打造一个轻量级的Java Web框架，采用MVC模式、Ioc控制反转技术。通过框架来对应用进行初始化和管理，提高开发效率。

> - Model（模型）是应用程序中用于处理应用程序数据逻辑的部分。通常模型对象负责在数据库中存取数据。
> - View（视图）是应用程序中处理数据显示的部分。通常视图是依据模型数据创建的。
> - Controller（控制器）是应用程序中处理用户交互的部分。通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据。

准备配置文件/resources/config.properties，准备一个PropsUtil类loadProps方法getResourceAsStream对其进行加载，K-V存Properties对象里。

创建ConfigHelper(加载了配置文件，需要此类来获取配置文件属性)，利用PropsUtil加载的Properties对象来获取配置文件属性：应用基础包名、getAppAssetPath等

实现一个类加载器ClassUtil中的Class.forName(className, isInitialized, getDefaultClassLoader())来加载基础包下的所有的类。只有加载了这些类，框架才能对其进行初始化。

> 获取类加载器，获取指定包名下的所有类，根据类名对类进行加载和静态资源初始化。

定义注解：采用注解来标识Controller，Service类。控制器类上使用@Controller注解，在控制器类的方法上使用@RequestMapping注解，使用@Autowired注解将服务类依赖注入进来。服务类使用@Service注解。

> annotation包下定义这几种注解

Controller,Service类是框架需要管理的类，把他们统称为Bean类，放在bean包下。

前文已经通过PropsUtil加载了属性，ConfigHelper获取了属性，ClassUtil加载了基础包下所有类存在List中，现在要从这所有类里选出被@Controller和@Service注解的类。

> 创建ClassHelpr，ConfigHelper.getAppBasePackage然后ClassUtil.getClasses(basePackage)获取所有类，遍历，通过isAnnotationPresent(Service.class)和isAnnotationPresent(Controller.class)判断是否被@Service和@Controller注解，getBeanClasses是二者都可以。

当前我们只是获取了被@Controller和@Service注解的类。但是无法通过获取的类来实例化对象。因此需要一个反射工具，来实例化类。

> 创建一个BenaFactory，newInstance()通过类型创建实例，invokeMethod调用对象obj的方法method，injectField为对象obj的属性field注入value

在ClassHelper定义了方法getBeanClasses()来获取Bean容器需要管理的所有Controller,Service类，获取这些类以后，调用BeanFactory.newInstance(Class<?> clazz)方法来实例化类的对象，缓存在容器Map beanContainer中，需要随时获取。beanContainer使用类全名作为key，类的实例对象作为value值。

当某个Java实例（调用者）需要另一个Java实例（被调用者）时，在传统的程序设计过程中，通常由调用者来创建被调用者的实例。在依赖注入模式下，创建调用者的工作不再由调用者来完成而交由容器来完成，因此称为控制反转（Ioc）；创建被调用者实例的工作通常由Spring容器来完成，然后注入调用者，因此也成为依赖注入(DI)。

> 创建IocHelper类，获取刚刚由BeanContatiner创建的Map beanContainer。initIOC。
>
> initIOC的过程即，从Map中获取String className-Object beanInstance键值对，由className加载到对应类，获取到全部被声明的属性，遍历查看isAnnotationPresent(Autowired.class)，获得被Autowired注解的属性，获取其类名看看beanContainer里面有没有实例，有就injectField()为其注入。

被@Controller注解的类中带有@RequestMapping注解的方法用于处理特定URL请求。

问题：如何判断当前请求 URL的method 对应哪个@Controller的method？

> 通过反射可以获取被@Controller的类中带有@RequestMapping注解的方法，进而获得@RequestMapping注解中请求方法和请求路径。封装一个请求对象request与处理request对象handler，将request与handler建立一个映射关系。
>
> 请求对象request：包括请求路径path；请求方法method两个属性。
>
> 处理request对象handler：包括带有@Controller的类；带有@RequestMapping注解的方法method。

如何建立映射关系？

创建一个一个类ControllerHelper，找到controller类，定位到其中被RequestMapping注解的方法后获得具体的RequestMapping类，取得其中的请求方法和请求路径包装出一个Request实例和Handler实例，这就是这个controller类的method对应的request



框架基本搭建起来了，现在需要编写一个类来初始化应用。初始化步骤，编写HelperLoader:

- 加载ClassHelper类，通过这个类加载基础包名下所有的类。
- 加载BeanContainer类，将基础包名下所有Bean类，通过Bean工厂实例化保存在Bean容器。
- 加载IocHelper类，实例化Bean类，需要为Controller类中带有Autowired注解的属性赋值。
- 加载ControllerHelper类，将Controller类中带有RequestMapping注解的方法，建立与请求路径和请求方法的映射关系，这样框架才能找到处理请求对应的方法。

使用一个Request类封装从HttpServletRequest请求对象中获取所有的参数，然后传递给处理方法，处理方法将其封装成Param类中，传递给Controller的方法处理。



IOC实现过程

getResourceAsStream获取了properties文件中的基础包名，解析了包名下的所有类，通过Class.forName加载了所有类，还初始化了。