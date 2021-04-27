# JVM

## 基本概念

1. **JRE, JDK, JVM。**

   JVM: Java virtual machine, Java虚拟机。 位于在操作系统之上，负责**执行 字节码中指令**。Java语言的可执行性正是建立在JVM之上，任何平台只要装了**针对该平台的JVM**，那么就能**执行经过编译的字节码文件**。这就是“一次编译，多次运行”。

   > Java程序(.Java)    编译--》 Java字节码(.class, .jar)    解释----》 JVM   执行----》 操作系统。
   >
   > Java语言是编译+解释语言，Java程序由Javac编译成字节码后，用JVM解释。

   JRE: Java runtime environment Java运行时环境。JRE =  JVM + JVM工作所需要的一些类库

   JDK: Java development kit，Java开发工具包。JDK = JRE + 一堆Java开发工具 + 一些基础类库

   > JDK是给开发人员使用，JRE和JVM是给普通用户使用。

   <img src = "./img/JDK层次结构图.png" width = 40% height = 40%>

   <img src = "./img/JDK层次结构图.png">

## 内存区域划分

<img src = "img\JVM内存区域划分.png">

1. 类加载过程
2. 双亲委派
3. JVM内存划分