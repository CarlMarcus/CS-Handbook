## javaagent



​	jdk1.5以后引入了javaAgent技术，是运行方法之前的拦截器，开发者可以通过修改字节码实现在类运行的时候动态修改类代码。它就是一个 jar 包，只不过启动方式不是跟普通 jar 包不一样，并不是类的静态 main 函数。javaagent 的 jar 包不能单独启动，必须依附于 JVM 进程才可以。

​	Javaagent 是运行在 main 方法之前的拦截器，它内定的方法名叫 premain ，也就是说先执行 premain 方法然后再执行 main 方法。我们也可以很方便地在运行过程中动态地设置加载代理类，以达到 instrumentation 的目的。 Instrumentation 的最大作用就是类定义动态改变和操作。

**需求场景：**

​	微服务场景中无侵入的采集java程序的各种运行信息(方法调用流量信息、异常栈信息、监控指标性能等)，Spring系程序员自然想到的是**Spring** **AOP**在方法前、中、抛异常时采集相关信息。用Spring AOP的方法确实可以采集信息，但是无法做到无侵入，必须预先写好代码-编译-发布后才能正常运行与采集。而且现有微服务们正在运行，不可能大批量的修改代码再进行重启发布。我们的需求呼之欲出：**无侵入+运行时仍能生效**。- javaagent

​	javaagent 的主要功能如下：

- 可以在加载 class 文件之前做拦截，对字节码做修改
- 可以在运行期对已加载类的字节码做变更，但是这种情况下会有很多的限制，后面会详细说
- 还有其他一些小众的功能
  > - 获取所有已经加载过的类
  > - 获取所有已经初始化过的类（执行过 clinit 方法，是上面的一个子集）
  > - 获取某个对象的大小
  > - 将某个 jar 加入到 bootstrap classpath 里作为高优先级被 bootstrapClassloader 加载
  > - 将某个 jar 加入到 classpath 里供 AppClassloard 去加载
  > - 设置某些 native 方法的前缀，主要在查找 native 方法的时候做规则匹配

<u>*说明*：</u>

**javaagent**：通常可理解为一个“插件”，本质是一个精心提供的jar文件，我们精心的编码在其中描写需要进行的操作，这些操作通过java.lang.Instrument包提供的API进行Java应用程序的Instrument(这里我理解为用Java代码装备，用于装备的Java代码可以进行任何和规矩的操作)。

**java.lang.instrument**：JDK1.5之后提供的用于装备Java应用程序的工具API，允许JavaAgent程序Instrument(装备)在JVM上运行的应用程序，通常的做法是提供方法用于在字节码中插入要执行的附加代码。JDK1.6后提供两种实现：命令行(-javaagent)形式在应用程序启动前处理(premain方式)；在应用程序启动后的某个时机处理(agentmain方式)。

> instrument包的用途很多，主要体现在对代码侵入低的优点上，例如一些监控不方便修改业务代码，但是可以使用这种方式在方法中植入特定逻辑，这种方式能够直接修改JVM中加载的字节码的内容，而不需要在像Spring AOP实现中创建新的代理类，所以在底层侵入更高，但是对开发者更透明。用于自动添加getter/setter方法的工具lombok就使用了这一技术。另外btrace和housemd等动态诊断工具也是用了instrument技术。

**Instrumentation**：此类提供能够Instrument(装备)Java代码的服务方法。启动Agent机制时，Instrumentation对象会被传递给premain或者agentmain方法。

**ClassFileTransformer**：JavaAgent的代码中需要提供一个它的实现类，以进行自定义的字节码转换。

**Javaassist**：它是一个处理Java字节码类库。能允许在Java程序运行时定义类，并能在JVM加载类时修改类文件。重要的是其提供两种级别的API：源代码级别和字节码级别。使用源代码级别的API可以向编写Java源码一样修改类文件，Javaassist会进行即时编译。

instrument的使用方式

> 1. 通过在启动命令上添加`-javaagent:xxx.jar`的方式加载一个称为agent的jar包，jar包的META-INF/MANIFEST.MF中应当声明Premain-Class或Main-Class。启动时JVM会寻找这个类中的`public static void premain(String agentArgs, Instrumentation instrumentation)`, instrumentation对象中可以添加自己的类修改逻辑进行字节码修改。
> 2. 另外当通过attach到一个运行中的JVM的方式时，可以调用agentmain方法来获取Instrument对象进行类的重定义。

实现一个javaagent：类里写一个 public static void premain(String args, Instrumentation inst) ｛｝

```java
public static void premain(String agentArgs, Instrumentation inst)
public static void premain(String agentArgs)
```

JVM首先尝试在代理中调用签名为1的方法，如果代理类没有实现签名为1的方法，JVM尝试调用签名为2的方法。（实际测试没有签名1只有签名2会报错。。。）

代理类可以有一个`agentmain`函数，函数会在JVM启动完成之后调用。如果，使用命令行启动代理，`agentmain`方式不会被调用。

代理的所有参数被当作一个字符串通过`agentArgs`变量传递，代理负责解析参数字符串。如果代理因为代理类无法被加载、代理类未实现`premain`方法或抛出了未被捕获的异常，JVM将会退出。

javaagent的启动不要求实现一定提供命令行的方式，如果，实现支持通过命令行启动，实现必须支持在命令行中通过指定*-javaagent*参数启动。*-javaagent*可以在命令行中使用多次，启动多个代理。*premain*函数的调用顺序和命令行中指定的顺序一致，多个代理可以使用相同*<jarpath>*.

没有一个严格模型来定义`premain`函数的工作范围，任何`main`函数可以做的工作，比如创建线程，在`premain`函数中都是合法的。

```java
package com.github;
import java.lang.instrument.Instrumentation;

public class PreAgent
{
    public static void premain(String agentOps, Instrumentation inst) {
        System.out.println("=========premain方法执行1========");
        System.out.println(agentOps);
    }

    public static void premain(String agentOps) {
        System.out.println("=========premain方法执行2========");
        System.out.println(agentOps);
    }
}
```

参考：

> [javaagent-刘正阳](https://liuzhengyang.github.io/2017/03/15/javaagent/)
>
> [Java探针-Java Agent技术-阿里面试题](https://www.cnblogs.com/aspirant/p/8796974.html)
>
> [Javaagent技术初探](https://www.jianshu.com/p/b2d09a78678d)
>
> [动手写一个javaagent](https://www.jianshu.com/p/1ca23d884e7f)
>
> [JVM源码分析之javaagent原理完全解读](https://www.ctolib.com/topics-35664.html)
>
> [构建自己的监测器【2】-javaagent参数使用](https://blog.csdn.net/qyongkang/article/details/7799603)
>
> [SkyWalking apm-sniffer原理学习与插件编写](http://skywalking.apache.org/zh/blog/2018-12-21-SkyWalking-apm-sniffer-beginning.html)

