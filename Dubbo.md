### 什么是dubbo?

> Dubbo是  阿里巴巴公司开源的一个**高性能优秀的服务框架**，使得应用可通过高性能的 **RPC** 实现服务的输出和输入功能，可以和   Spring框架无缝集成。

上面我们提到了RPC，现在我们来理解一下**RPC的一些相关概念**。之前学习过操作系统的同学在进程那一章也会接触到这个东西。

> RPC（Remote Procedure Call）—远程过程调用，它是一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议。RPC协议假定某些传输协议的存在，如TCP或UDP，为通信程序之间携带信息数据。在OSI网络通信模型中，RPC跨越了传输层和应用层。RPC使得开发包括网络分布式多程序在内的应用程序更加容易。 RPC采用客户机/服务器模式。请求程序就是一个客户机，而服务提供程序就是一个服务器。首先，客户机调用进程发送一个有进程参数的调用信息到服务进程，然后等待应答信息。在服务器端，进程保持睡眠状态直到调用信息到达为止。当一个调用信息到达，服务器获得进程参数，计算结果，发送答复信息，然后等待下一个调用信息，最后，客户端调用进程接收答复信息，获得进程结果，然后调用执行继续进行。

**既然有http请求为什么还要用rpc调用呢？？？**

> 良好的rpc调用是面向服务的封装，针对服务的可用性和效率等都做了优化。单纯使用http调用则缺少了这些特性。

### dubbo的一些相关资源

相信你看了dubbo的用户手册可能会明白dubbo被企业所喜爱的一部分原因，官方文档介绍的真的详细，很容易就可以学会如何简单的去使用dubbo到自己的项目中。

dubbo官网：[dubbo.incubator.apache.org/](https://link.juejin.im?target=http%3A%2F%2Fdubbo.incubator.apache.org%2F)

Dubbo Github地址：[github.com/apache/incu…](https://link.juejin.im?target=https%3A%2F%2Fgithub.com%2Fapache%2Fincubator-dubbo)

[Dubbo用户手册(中文)](https://link.juejin.im?target=https%3A%2F%2Fgithub.com%2Fapache%2Fincubator-dubbo) ：这篇文档详细讲解了dubbo的使用，基本涵盖dubbo的所有功能特性。如果你正依赖dubbo作为你业务工程的RPC通信框架，这里可以作为你的参考手册

[Dubbo开发手册(中文)](https://link.juejin.im?target=http%3A%2F%2Fdubbo.apache.org%2Fbooks%2Fdubbo-dev-book%2F)：这篇文档的目标读者是对 dubbo 源码、设计有兴趣的，或者有意愿加入 dubbo 开发的人群。主要涵盖了 dubbo 的框架设计、扩展机制、编码规范、版本管理、构建等话题。

### 为什么要用dubbo呢？？？

先来看一张普通电商的简易架构图 

![普通电商的简易架构图](https://user-gold-cdn.xitu.io/2018/4/9/162a875f8a6b5d01?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



当服务越来越多后，服务之间的依赖关系越来越复杂，服务 URL 配置管理变得非常困难另外还需要统计服务的调用量来进行分析，这些需求都可以使用dubbo来满足。

#### dubbo架构



![dubbo架构](https://user-gold-cdn.xitu.io/2018/4/9/162a875f8a7c4bb1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



**上述节点简单说明：**

- **Provider** 	暴露服务的服务提供方
- **Consumer** 	调用远程服务的服务消费方
- **Registry** 	服务注册与发现的注册中心
- **Monitor** 	统计服务的调用次数和调用时间的监控中心
- **Container** 	服务运行容器

**调用关系说明：**

1. 服务容器负责启动，加载，运行服务提供者。
2. 服务提供者在启动时，向注册中心注册自己提供的服务。
3. 服务消费者在启动时，向注册中心订阅自己所需的服务。
4. 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
5. 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
6. 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

### 注册中心

一个完整的dubbo应该是包括注册中心的。 **注册中心**用来注册服务和进行负载均衡，哪一个服务由哪一个机器来提供必需让调用者知道，简单来说就是IP地址和服务名称的对应关系。
 dubbo官方提供了**几种实现注册中心的方式**：

1. Multicast 注册中心
2. **Zookeeper 注册中心**
3. Redis 注册中心
4. Simple 注册中心

另外官方明确推荐使用**Zookeeper 注册中心**的方式。 

![Zookeeper 注册中心](https://user-gold-cdn.xitu.io/2018/4/9/162a875f8a48407a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

 装zookeeper的话，建议装在Linux机器上，我这里就不做讲解了。想要了解的可以去官网看看文档，因为dubbo的官方文档很详细了，建议看官方文档，大多数博客文章还是照着文档写的。



### dubbo的使用

[Dubbo用户手册(中文)](https://link.juejin.im?target=https%3A%2F%2Fgithub.com%2Fapache%2Fincubator-dubbo) 已经介绍的很详细了，所以这里我就不去班门弄斧了，想要了解的可以去看一下。 

![dubbo的使用](https://user-gold-cdn.xitu.io/2018/4/9/162a875f8a7cd50a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

作者：SnailClimb

链接：https://juejin.im/post/5acadeb1f265da2375072f9c

来源：掘金

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。