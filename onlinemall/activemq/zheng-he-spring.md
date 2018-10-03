# 整合Spring

### 1.1. 使用方法

第一步：引用相关的jar包。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <dependency>
        </p>
        <p>
          <groupId>org.springframework</groupId>
        </p>
        <p>
          <artifactId>spring-jms</artifactId>
        </p>
        <p>
          </dependency>
        </p>
        <p>
          <dependency>
        </p>
        <p>
          <groupId>org.springframework</groupId>
        </p>
        <p>
          <artifactId>spring-context-support</artifactId>
        </p>
        <p>
          </dependency>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第二步：配置Activemq整合spring。配置ConnectionFactory

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <?xml version="1.0" encoding="UTF-8" ?>
        </p>
        <p>
          <beans xmlns="http://www.springframework.org/schema/beans" </p>
            <p>xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"</p>
            <p>xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"</p>
            <p>xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"</p>
            <p>xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd</p>
            <p>http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd</p>
            <p>http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd</p>
            <p>http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd</p>
            <p>http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd"></p>
            <p>
              <!-- 真正可以产生Connection的ConnectionFactory，由对应的 JMS服务厂商提供 -->
            </p>
            <p>
              <bean id="targetConnectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory">
            </p>
            <p>
              <property name="brokerURL" value="tcp://192.168.25.168:61616" />
            </p>
            <p>
              </bean>
            </p>
            <p>
              <!-- Spring用于管理真正的ConnectionFactory的ConnectionFactory -->
            </p>
            <p>
              <bean id="connectionFactory" </p>
                <p>class="org.springframework.jms.connection.SingleConnectionFactory"></p>
                <p>
                  <!-- 目标ConnectionFactory对应真实的可以产生JMS Connection的ConnectionFactory -->
                </p>
                <p>
                  <property name="targetConnectionFactory" ref="targetConnectionFactory"
                  />
                </p>
                <p>
              </bean>
              </p>
              <p>
          </beans>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第三步：配置生产者。

使用JMSTemplate对象。发送消息。

第四步：在spring容器中配置Destination。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <?xml version="1.0" encoding="UTF-8" ?>
        </p>
        <p>
          <beans xmlns="http://www.springframework.org/schema/beans" </p>
            <p>xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"</p>
            <p>xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"</p>
            <p>xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"</p>
            <p>xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd</p>
            <p>http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd</p>
            <p>http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd
              http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd</p>
            <p>http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd"></p>
            <p>
              <!-- 真正可以产生Connection的ConnectionFactory，由对应的 JMS服务厂商提供 -->
            </p>
            <p>
              <bean id="targetConnectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory">
            </p>
            <p>
              <property name="brokerURL" value="tcp://192.168.25.168:61616" />
            </p>
            <p>
              </bean>
            </p>
            <p>
              <!-- Spring用于管理真正的ConnectionFactory的ConnectionFactory -->
            </p>
            <p>
              <bean id="connectionFactory" </p>
                <p>class="org.springframework.jms.connection.SingleConnectionFactory"></p>
                <p>
                  <!-- 目标ConnectionFactory对应真实的可以产生JMS Connection的ConnectionFactory -->
                </p>
                <p>
                  <property name="targetConnectionFactory" ref="targetConnectionFactory"
                  />
                </p>
                <p>
              </bean>
              </p>
              <p>
                <!-- 配置生产者 -->
              </p>
              <p>
                <!-- Spring提供的JMS工具类，它可以进行消息发送、接收等 -->
              </p>
              <p>
                <bean id="jmsTemplate" class="org.springframework.jms.core.JmsTemplate">
              </p>
              <p>
                <!-- 这个connectionFactory对应的是我们定义的Spring提供的那个ConnectionFactory对象 -->
              </p>
              <p>
                <property name="connectionFactory" ref="connectionFactory" />
              </p>
              <p>
                </bean>
              </p>
              <p>
                <!--这个是队列目的地，点对点的 -->
              </p>
              <p>
                <bean id="queueDestination" class="org.apache.activemq.command.ActiveMQQueue">
              </p>
              <p>
                <constructor-arg>
              </p>
              <p>
                <value>spring-queue</value>
              </p>
              <p>
                </constructor-arg>
              </p>
              <p>
                </bean>
              </p>
              <p>
                <!--这个是主题目的地，一对多的 -->
              </p>
              <p>
                <bean id="topicDestination" class="org.apache.activemq.command.ActiveMQTopic">
              </p>
              <p>
                <constructor-arg value="topic" />
              </p>
              <p>
                </bean>
              </p>
              <p>
          </beans>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第五步：代码测试

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testSpringActiveMq() <b>throws</b> Exception {</p>
        <p>//初始化spring容器</p>
        <p>ApplicationContext applicationContext = <b>new</b> ClassPathXmlApplicationContext("classpath:spring/applicationContext-activemq.xml");</p>
        <p>//从spring容器中获得JmsTemplate对象</p>
        <p>JmsTemplate jmsTemplate = applicationContext.getBean(JmsTemplate.<b>class</b>);</p>
        <p>//从spring容器中取Destination对象</p>
        <p>Destination destination = (Destination) applicationContext.getBean("queueDestination");</p>
        <p>//使用JmsTemplate对象发送消息。</p>
        <p>jmsTemplate.send(destination, <b>new</b> MessageCreator() {</p>
        <p>@Override</p>
        <p> <b>public</b> Message createMessage(Session session) <b>throws</b> JMSException
          {</p>
        <p>//创建一个消息对象并返回</p>
        <p>TextMessage textMessage = session.createTextMessage("spring activemq queue
          message");</p>
        <p> <b>return</b> textMessage;</p>
        <p>}</p>
        <p>});</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.2. 代码测试

#### 1.2.1.                  发送消息

第一步：初始化一个spring容器

第二步：从容器中获得JMSTemplate对象。

第三步：从容器中获得一个Destination对象

第四步：使用JMSTemplate对象发送消息，需要知道Destination

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testQueueProducer() <b>throws</b> Exception {</p>
        <p>// 第一步：初始化一个spring容器</p>
        <p>ApplicationContext applicationContext = <b>new</b> ClassPathXmlApplicationContext("classpath:spring/applicationContext-activemq.xml");</p>
        <p>// 第二步：从容器中获得JMSTemplate对象。</p>
        <p>JmsTemplate jmsTemplate = applicationContext.getBean(JmsTemplate.<b>class</b>);</p>
        <p>// 第三步：从容器中获得一个Destination对象</p>
        <p>Queue queue = (Queue) applicationContext.getBean("queueDestination");</p>
        <p>// 第四步：使用JMSTemplate对象发送消息，需要知道Destination</p>
        <p>jmsTemplate.send(queue, <b>new</b> MessageCreator() {</p>
        <p>@Override</p>
        <p> <b>public</b> Message createMessage(Session session) <b>throws</b> JMSException
          {</p>
        <p>TextMessage textMessage = session.createTextMessage("spring activemq test");</p>
        <p> <b>return</b> textMessage;</p>
        <p>}</p>
        <p>});</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.2.2.                  接收消息

e3-search-Service中接收消息。

第一步：把Activemq相关的jar包添加到工程中

第二步：创建一个MessageListener的实现类。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b>  <b>class</b> MyMessageListener <b>implements</b> MessageListener
          {</p>
        <p>@Override</p>
        <p> <b>public</b>  <b>void</b> onMessage(Message message) {</p>
        <p> <b>try</b> {</p>
        <p>TextMessage textMessage = (TextMessage) message;</p>
        <p>//取消息内容</p>
        <p>String text = textMessage.getText();</p>
        <p>System.<b>out</b>.println(text);</p>
        <p>} <b>catch</b> (JMSException e) {</p>
        <p>e.printStackTrace();</p>
        <p>}</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第三步：配置spring和Activemq整合。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <?xml version="1.0" encoding="UTF-8" ?>
        </p>
        <p>
          <beans xmlns="http://www.springframework.org/schema/beans" </p>
            <p>xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"</p>
            <p>xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"</p>
            <p>xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"</p>
            <p>xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd</p>
            <p>http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd</p>
            <p>http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd
              http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd</p>
            <p>http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd"></p>
            <p>
              <!-- 真正可以产生Connection的ConnectionFactory，由对应的 JMS服务厂商提供 -->
            </p>
            <p>
              <bean id="targetConnectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory">
            </p>
            <p>
              <property name="brokerURL" value="tcp://192.168.25.168:61616" />
            </p>
            <p>
              </bean>
            </p>
            <p>
              <!-- Spring用于管理真正的ConnectionFactory的ConnectionFactory -->
            </p>
            <p>
              <bean id="connectionFactory" </p>
                <p>class="org.springframework.jms.connection.SingleConnectionFactory"></p>
                <p>
                  <!-- 目标ConnectionFactory对应真实的可以产生JMS Connection的ConnectionFactory -->
                </p>
                <p>
                  <property name="targetConnectionFactory" ref="targetConnectionFactory"
                  />
                </p>
                <p>
              </bean>
              </p>
              <p>
                <!--这个是队列目的地，点对点的 -->
              </p>
              <p>
                <bean id="queueDestination" class="org.apache.activemq.command.ActiveMQQueue">
              </p>
              <p>
                <constructor-arg>
              </p>
              <p>
                <value>spring-queue</value>
              </p>
              <p>
                </constructor-arg>
              </p>
              <p>
                </bean>
              </p>
              <p>
                <!--这个是主题目的地，一对多的 -->
              </p>
              <p>
                <bean id="topicDestination" class="org.apache.activemq.command.ActiveMQTopic">
              </p>
              <p>
                <constructor-arg value="topic" />
              </p>
              <p>
                </bean>
              </p>
              <p>
                <!-- 接收消息 -->
              </p>
              <p>
                <!-- 配置监听器 -->
              </p>
              <p>
                <bean id="myMessageListener" class="cn.e3mall.search.listener.MyMessageListener"
                />
              </p>
              <p>
                <!-- 消息监听容器 -->
              </p>
              <p>
                <bean class="org.springframework.jms.listener.DefaultMessageListenerContainer">
              </p>
              <p>
                <property name="connectionFactory" ref="connectionFactory" />
              </p>
              <p>
                <property name="destination" ref="queueDestination" />
              </p>
              <p>
                <property name="messageListener" ref="myMessageListener" />
              </p>
              <p>
                </bean>
              </p>
              <p>
          </beans>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第四步：测试代码。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testQueueConsumer() <b>throws</b> Exception {</p>
        <p>//初始化spring容器</p>
        <p>ApplicationContext applicationContext = <b>new</b> ClassPathXmlApplicationContext("classpath:spring/applicationContext-activemq.xml");</p>
        <p>//等待</p>
        <p>System.<b>in</b>.read();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>