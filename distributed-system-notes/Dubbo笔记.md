# Dubbo笔记

## 架构

### 核心组件

架构中有四个**核心组件**：生产者，消费者，注册中心和监控中心。

<img src="./img/Dubbo_Architecture.png">

#### 过程

0. 生产者(provider)初始化和启动服务。
1. 生产者到注册中心(registry)去注册服务。
   - 发布接口服务
   - 暴露host（IP地址）
2. 消费者到注册中心去订阅服务。
3. 注册中心通知(notify)服务给消费者。
4. 消费者调用生产者提供的服务
   - 调用dubbo协议的接口方法
5. 监控器(monitor)统计并监控服务调用。

## ToDo

1. dubbo负载均衡算法

   随机，轮询，最少活跃请求数，一致性hash

2. dubbo原理

3. 什么是dubbo？dubbo是干嘛的？

4. 什么是zookeeper？zookeeper是干嘛的？

5. 什么是RPC？有哪些RPC框架？你觉得实现一个RPC框架重点在什么？

6. 