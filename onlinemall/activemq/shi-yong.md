# 使用

## 1.  ActiveMQ的使用方法

![](../../.gitbook/assets/image%20%2841%29.png)

### 1.1. Queue

#### 1.1.1.                  Producer

生产者：生产消息，发送端。

把jar包添加到工程中。使用5.11.2版本的jar包。

![](../../.gitbook/assets/image%20%28195%29.png)

第一步：创建ConnectionFactory对象，需要指定服务端ip及端口号。

第二步：使用ConnectionFactory对象创建一个Connection对象。

第三步：开启连接，调用Connection对象的start方法。

第四步：使用Connection对象创建一个Session对象。

第五步：使用Session对象创建一个Destination对象（topic、queue），此处创建一个Queue对象。

第六步：使用Session对象创建一个Producer对象。

第七步：创建一个Message对象，创建一个TextMessage对象。

第八步：使用Producer对象发送消息。

第九步：关闭资源。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testQueueProducer() <b>throws</b> Exception {</p>
        <p>// 第一步：创建ConnectionFactory对象，需要指定服务端ip及端口号。</p>
        <p>//brokerURL服务器的ip及端口号</p>
        <p>ConnectionFactory connectionFactory = <b>new</b> ActiveMQConnectionFactory("tcp://192.168.25.168:61616");</p>
        <p>// 第二步：使用ConnectionFactory对象创建一个Connection对象。</p>
        <p>Connection connection = connectionFactory.createConnection();</p>
        <p>// 第三步：开启连接，调用Connection对象的start方法。</p>
        <p>connection.start();</p>
        <p>// 第四步：使用Connection对象创建一个Session对象。</p>
        <p>//第一个参数：是否开启事务。true：开启事务，第二个参数忽略。</p>
        <p>//第二个参数：当第一个参数为false时，才有意义。消息的应答模式。1、自动应答2、手动应答。一般是自动应答。</p>
        <p>Session session = connection.createSession(<b>false</b>, Session.<b>AUTO_ACKNOWLEDGE</b>);</p>
        <p>// 第五步：使用Session对象创建一个Destination对象（topic、queue），此处创建一个Queue对象。</p>
        <p>//参数：队列的名称。</p>
        <p>Queue queue = session.createQueue("test-queue");</p>
        <p>// 第六步：使用Session对象创建一个Producer对象。</p>
        <p>MessageProducer producer = session.createProducer(queue);</p>
        <p>// 第七步：创建一个Message对象，创建一个TextMessage对象。</p>
        <p>/*TextMessage message = new ActiveMQTextMessage();</p>
        <p>message.setText("hello activeMq,this is my first test.");*/</p>
        <p>TextMessage textMessage = session.createTextMessage("hello activeMq,this
          is my first test.");</p>
        <p>// 第八步：使用Producer对象发送消息。</p>
        <p>producer.send(textMessage);</p>
        <p>// 第九步：关闭资源。</p>
        <p>producer.close();</p>
        <p>session.close();</p>
        <p>connection.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.2.                  Consumer

消费者：接收消息。

第一步：创建一个ConnectionFactory对象。

第二步：从ConnectionFactory对象中获得一个Connection对象。

第三步：开启连接。调用Connection对象的start方法。

第四步：使用Connection对象创建一个Session对象。

第五步：使用Session对象创建一个Destination对象。和发送端保持一致queue，并且队列的名称一致。

第六步：使用Session对象创建一个Consumer对象。

第七步：接收消息。

第八步：打印消息。

第九步：关闭资源

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testQueueConsumer() <b>throws</b> Exception {</p>
        <p>// 第一步：创建一个ConnectionFactory对象。</p>
        <p>ConnectionFactory connectionFactory = <b>new</b> ActiveMQConnectionFactory("tcp://192.168.25.168:61616");</p>
        <p>// 第二步：从ConnectionFactory对象中获得一个Connection对象。</p>
        <p>Connection connection = connectionFactory.createConnection();</p>
        <p>// 第三步：开启连接。调用Connection对象的start方法。</p>
        <p>connection.start();</p>
        <p>// 第四步：使用Connection对象创建一个Session对象。</p>
        <p>Session session = connection.createSession(<b>false</b>, Session.<b>AUTO_ACKNOWLEDGE</b>);</p>
        <p>// 第五步：使用Session对象创建一个Destination对象。和发送端保持一致queue，并且队列的名称一致。</p>
        <p>Queue queue = session.createQueue("test-queue");</p>
        <p>// 第六步：使用Session对象创建一个Consumer对象。</p>
        <p>MessageConsumer consumer = session.createConsumer(queue);</p>
        <p>// 第七步：接收消息。</p>
        <p>consumer.setMessageListener(<b>new</b> MessageListener() {</p>
        <p>@Override</p>
        <p> <b>public</b>  <b>void</b> onMessage(Message message) {</p>
        <p> <b>try</b> {</p>
        <p>TextMessage textMessage = (TextMessage) message;</p>
        <p>String text = <b>null</b>;</p>
        <p>//取消息的内容</p>
        <p>text = textMessage.getText();</p>
        <p>// 第八步：打印消息。</p>
        <p>System.<b>out</b>.println(text);</p>
        <p>} <b>catch</b> (JMSException e) {</p>
        <p>e.printStackTrace();</p>
        <p>}</p>
        <p>}</p>
        <p>});</p>
        <p>//等待键盘输入</p>
        <p>System.<b>in</b>.read();</p>
        <p>// 第九步：关闭资源</p>
        <p>consumer.close();</p>
        <p>session.close();</p>
        <p>connection.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.2. Topic

#### 1.2.1.                  Producer

使用步骤：

第一步：创建ConnectionFactory对象，需要指定服务端ip及端口号。

第二步：使用ConnectionFactory对象创建一个Connection对象。

第三步：开启连接，调用Connection对象的start方法。

第四步：使用Connection对象创建一个Session对象。

第五步：使用Session对象创建一个Destination对象（topic、queue），此处创建一个Topic对象。

第六步：使用Session对象创建一个Producer对象。

第七步：创建一个Message对象，创建一个TextMessage对象。

第八步：使用Producer对象发送消息。

第九步：关闭资源。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testTopicProducer() <b>throws</b> Exception {</p>
        <p>// 第一步：创建ConnectionFactory对象，需要指定服务端ip及端口号。</p>
        <p>// brokerURL服务器的ip及端口号</p>
        <p>ConnectionFactory connectionFactory = <b>new</b> ActiveMQConnectionFactory("tcp://192.168.25.168:61616");</p>
        <p>// 第二步：使用ConnectionFactory对象创建一个Connection对象。</p>
        <p>Connection connection = connectionFactory.createConnection();</p>
        <p>// 第三步：开启连接，调用Connection对象的start方法。</p>
        <p>connection.start();</p>
        <p>// 第四步：使用Connection对象创建一个Session对象。</p>
        <p>// 第一个参数：是否开启事务。true：开启事务，第二个参数忽略。</p>
        <p>// 第二个参数：当第一个参数为false时，才有意义。消息的应答模式。1、自动应答2、手动应答。一般是自动应答。</p>
        <p>Session session = connection.createSession(<b>false</b>, Session.<b>AUTO_ACKNOWLEDGE</b>);</p>
        <p>// 第五步：使用Session对象创建一个Destination对象（topic、queue），此处创建一个topic对象。</p>
        <p>// 参数：话题的名称。</p>
        <p>Topic topic = session.createTopic("test-topic");</p>
        <p>// 第六步：使用Session对象创建一个Producer对象。</p>
        <p>MessageProducer producer = session.createProducer(topic);</p>
        <p>// 第七步：创建一个Message对象，创建一个TextMessage对象。</p>
        <p>/*</p>
        <p>* TextMessage message = new ActiveMQTextMessage(); message.setText(</p>
        <p>* "hello activeMq,this is my first test.");</p>
        <p>*/</p>
        <p>TextMessage textMessage = session.createTextMessage("hello activeMq,this
          is my topic test");</p>
        <p>// 第八步：使用Producer对象发送消息。</p>
        <p>producer.send(textMessage);</p>
        <p>// 第九步：关闭资源。</p>
        <p>producer.close();</p>
        <p>session.close();</p>
        <p>connection.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.2.2.                  Consumer

消费者：接收消息。

第一步：创建一个ConnectionFactory对象。

第二步：从ConnectionFactory对象中获得一个Connection对象。

第三步：开启连接。调用Connection对象的start方法。

第四步：使用Connection对象创建一个Session对象。

第五步：使用Session对象创建一个Destination对象。和发送端保持一致topic，并且话题的名称一致。

第六步：使用Session对象创建一个Consumer对象。

第七步：接收消息。

第八步：打印消息。

第九步：关闭资源

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testTopicConsumer() <b>throws</b> Exception {</p>
        <p>// 第一步：创建一个ConnectionFactory对象。</p>
        <p>ConnectionFactory connectionFactory = <b>new</b> ActiveMQConnectionFactory("tcp://192.168.25.168:61616");</p>
        <p>// 第二步：从ConnectionFactory对象中获得一个Connection对象。</p>
        <p>Connection connection = connectionFactory.createConnection();</p>
        <p>// 第三步：开启连接。调用Connection对象的start方法。</p>
        <p>connection.start();</p>
        <p>// 第四步：使用Connection对象创建一个Session对象。</p>
        <p>Session session = connection.createSession(<b>false</b>, Session.<b>AUTO_ACKNOWLEDGE</b>);</p>
        <p>// 第五步：使用Session对象创建一个Destination对象。和发送端保持一致topic，并且话题的名称一致。</p>
        <p>Topic topic = session.createTopic("test-topic");</p>
        <p>// 第六步：使用Session对象创建一个Consumer对象。</p>
        <p>MessageConsumer consumer = session.createConsumer(topic);</p>
        <p>// 第七步：接收消息。</p>
        <p>consumer.setMessageListener(<b>new</b> MessageListener() {</p>
        <p>@Override</p>
        <p> <b>public</b>  <b>void</b> onMessage(Message message) {</p>
        <p> <b>try</b> {</p>
        <p>TextMessage textMessage = (TextMessage) message;</p>
        <p>String text = <b>null</b>;</p>
        <p>// 取消息的内容</p>
        <p>text = textMessage.getText();</p>
        <p>// 第八步：打印消息。</p>
        <p>System.<b>out</b>.println(text);</p>
        <p>} <b>catch</b> (JMSException e) {</p>
        <p>e.printStackTrace();</p>
        <p>}</p>
        <p>}</p>
        <p>});</p>
        <p>System.<b>out</b>.println("topic的消费端03。。。。。");</p>
        <p>// 等待键盘输入</p>
        <p>System.<b>in</b>.read();</p>
        <p>// 第九步：关闭资源</p>
        <p>consumer.close();</p>
        <p>session.close();</p>
        <p>connection.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>