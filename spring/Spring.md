# Spring

1. AOP（面向切面编程）

   针对横切逻辑代码，AOP提出横向抽取机制，将横切逻辑代码和业务逻辑代码分离，横切逻辑代码一般指**日志，权限，事务，性能监测等功能代码**，区分于纵向的业务代码。

   <img src = ".\img\spring\AOP图解.jpg" width = 60% height = 60%>

1. 依赖注入（DI）和IOC

   IOC: Inversion of control控制反转，是一种设计思想。

   - 传统开发：A依赖B，那么就在类A中手动通过 new 出一个B的对象出来。

   - IOC：不是自己手动new对象，而是向IOC容器要对象，IOC容器帮助我们实例化对象。

   在这个过程中，我们也就“失去了”管理对象的权力，这个权力交给了spring的IOC容器，所以控制反转了。对于spring框架来说，spring来负责控制对象的生命周期和对象间的关系。

   **IOC的好处**

   - 降低了对象与对象间的耦合度

   **IOC如何实现**

   - spring启动时读取XML配置文件中提供的bean配置信息，并且在spring容器中生成一份相应的bean配置注册表。
   - 利用 Java 语言的**反射功能**实例化 Bean 并建立 Bean 之间的依赖关系
   - IOC容易底层是一个map，key就是beanId, value就是类。

   **Java反射**

   - 定义：对于任何一个类，都能知道这个类的所有属性和方法；

     ​           对于任何一个对象，都能调用它的任意一个方法和属性。