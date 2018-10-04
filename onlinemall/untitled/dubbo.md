# dubbo

#### 1.1.1.                  什么是dubbo

随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及流动计算架构势在必行，亟需一个治理系统确保架构有条不紊的演进。

![](../../.gitbook/assets/image%20%2847%29.png)

·       **单一应用架构**

·       当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。

·       此时，用于简化增删改查工作量的 **数据访问框架\(ORM\)** 是关键。

·       **垂直应用架构**

·       当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，将应用拆成互不相干的几个应用，以提升效率。

·       此时，用于加速前端页面开发的 **Web框架\(MVC\)** 是关键。

·       **分布式服务架构**

·       当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。

·       此时，用于提高业务复用及整合的 **分布式服务框架\(RPC\)** 是关键。

·       **流动计算架构**

·       当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。

·       此时，用于提高机器利用率的 **资源调度和治理中心\(SOA\)** 是关键。

Dubbo就是**资源调度和治理中心的管理工具。**

#### 1.1.2.                  Dubbo的架构

![](../../.gitbook/assets/image%20%28103%29.png)

**节点角色说明：**

·       **Provider:** 暴露服务的服务提供方。

·       **Consumer:** 调用远程服务的服务消费方。

·       **Registry:** 服务注册与发现的注册中心。

·       **Monitor:** 统计服务的调用次调和调用时间的监控中心。

·       **Container:** 服务运行容器。

**调用关系说明：**

·       0. 服务容器负责启动，加载，运行服务提供者。

·       1. 服务提供者在启动时，向注册中心注册自己提供的服务。

·       2. 服务消费者在启动时，向注册中心订阅自己所需的服务。

·       3. 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。

·       4. 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

·       5. 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

#### 1.1.3.                  使用方法

Dubbo采用全Spring配置方式，透明化接入应用，对应用没有任何API侵入，只需用Spring加载Dubbo的配置即可，Dubbo基于Spring的Schema扩展进行加载。

**单一工程中spring的配置**

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <bean id="xxxService" class="com.xxx.XxxServiceImpl" />
        </p>
        <p>
          <bean id="xxxAction" class="com.xxx.XxxAction">
        </p>
        <p>
          <property name="xxxService" ref="xxxService" />
        </p>
        <p>
          </bean>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>**远程服务：**

在本地服务的基础上，只需做简单配置，即可完成远程化：

将上面的local.xml配置拆分成两份，将服务定义部分放在服务提供方remote-provider.xml，将服务引用部分放在服务消费方remote-consumer.xml。

并在提供方增加暴露服务配置&lt;dubbo:service&gt;，在消费方增加引用服务配置&lt;dubbo:reference&gt;。

发布服务：

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <!-- 和本地服务一样实现远程服务 -->
        </p>
        <p>
          <bean id="xxxService" class="com.xxx.XxxServiceImpl" />
        </p>
        <p>
          <!-- 增加暴露远程服务配置 -->
        </p>
        <p>
          <dubbo:service interface="com.xxx.XxxService" ref="xxxService" />
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>调用服务：

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <!-- 增加引用远程服务配置 -->
        </p>
        <p>
          <dubbo:reference id="xxxService" interface="com.xxx.XxxService" />
        </p>
        <p>
          <!-- 和本地服务一样使用远程服务 -->
        </p>
        <p>
          <bean id="xxxAction" class="com.xxx.XxxAction">
        </p>
        <p>
          <property name="xxxService" ref="xxxService" />
        </p>
        <p>
          </bean>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>