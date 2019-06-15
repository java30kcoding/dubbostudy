# Dubbo入门

## Dubbo是什么

​	一款高性能的Java RPC框架，由阿里贡献的开源RPC框架。

## Dubbo能做什么

1. 服务开发(rpc应用开发)
2. 服务软负载均衡
3. 服务依赖管理
4. 服务监控
5. 服务治理

## Dubbo的架构

![dubbo1](C:\Users\yuanyulou\Desktop\博客图片\dubbo\dubbo1.png)

| 节点      | 角色说明                               |
| --------- | -------------------------------------- |
| Provider  | 暴露服务的服务提供方                   |
| Consumer  | 调用远程服务的服务消费方               |
| Registry  | 服务注册与发现的注册中心               |
| Monitor   | 统计服务的调用次数和调用时间的监控中心 |
| Container | 服务运行容器                           |

## Dubbo架构具有的特点

官方文档<http://dubbo.apache.org/zh-cn/docs/user/preface/architecture.html

1. 连通性

- 注册中心负责服务地址的注册与查找，相当于目录服务，服务提供者和消费者只在启动时与注册中心交互，注册中心不转发请求，压力小。
- 监控中心负责统计各服务调用次数，调用时间等，统计在内存汇总然后每分钟发送到监控中心服务器，并以报表展示。
- 服务提供者向注册中心注册其提供的服务，并汇报调用时间到监控中心，此时间不包含网络开销。
- 服务消费者向注册中心获取服务提供者地址列表，并根据负载均衡算法直接调用提供者，同时汇报调用时间到监控中心，此时间包含网络开销。
- 注册中心，服务提供者，服务消费者三者之间均为长链接，监控中心除外。
- 注册中心通过长链接感知服务提供者的存在，服务提供者宕机，注册中心将立即推送事件通知消费者。
- 注册中心和监控中心全部宕机，不影响已经运行的提供者和消费者，消费者在本地缓存了提供者列表。
- 注册中心和监控中心都是可选的，服务消费者可以直连服务提供者(通过地址和协议)。

2. 健壮性

- 监控中心宕机不影响使用，只是丢失部分采样数据。
- 数据库宕机后，注册中心仍能通过缓存提供服务列表查询，但不能注册新服务。
- 注册中心对等集群，任意一台宕机，将自动切换到另一台。
- 注册中心全部宕机后，服务提供者和消费者仍能通过本地缓存继续通信。
- 服务提供者如果是无状态，任意一台宕机后，不影响使用。
- 服务提供者全部宕机后，服务消费者将无法使用，并无限次重连等待服务提供者恢复。

## Dubbo服务调用公作流程

![dubbo2](C:\Users\yuanyulou\Desktop\博客图片\dubbo\dubbo2.png)

## Dubbo使用

服务提供端：

- 独立的服务。
- 继承在应用中。

服务消费端：

- 在应用中调用远程服务。
- 也可以在服务提供者中调用远程服务。

## Dubbo使用步骤

1. 引入dubbo相关依赖

   ```
   <dependency>
   	<groupId>com.alibaba</groupId>
   	<artifactId>dubbo</artifactId>
   	<version>2.6.6</version>
   </dependency>
   <dependency>
   	<groupId>io.netty</groupId>
   	<artifactId>netty-all</artifactId>
   	<version>4.1.32.Final</version>
   </dependency>
   ```

   **Dubbo依赖说明**：<http://dubbo.apache.org/zh-cn/docs/user/dependencies.html>
   **务必学习，了解各项配置、配置属性**

2. 配置dubbo框架
   3种配置方式：

   - spring schema xml方式：适用spring应用。
   - 注解方式：使用spring应用，需要2.6.3及以上版本。
   - API方式：API方式使用范围说明：API仅用于OpenAPI，ESB，Test，Mock等系统集成。普通服务提供方或消费方，请采用XML配置方式使用dubbo。

3. 开发服务

4. 配置服务

5. 启动、调用
