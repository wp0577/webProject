# mapper绑定异常

**原因**

![](../../.gitbook/assets/image%20%28166%29.png)

此异常的原因是由于mapper接口编译后在同一个目录下没有找到mapper映射文件而出现的。由于maven工程在默认情况下src/main/java目录下的mapper文件是不发布到target目录下的。

**解决方法**

在e3-manager-dao工程的pom文件中添加如下内容：

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <!-- 如果不添加此节点mybatis的mapper.xml文件都会被漏掉。 -->
        </p>
        <p>
          <build>
        </p>
        <p>
          <resources>
        </p>
        <p>
          <resource>
        </p>
        <p>
          <directory>src/main/java</directory>
        </p>
        <p>
          <includes>
        </p>
        <p>
          <include>**/*.properties</include>
        </p>
        <p>
          <include>**/*.xml</include>
        </p>
        <p>
          </includes>
        </p>
        <p>
          <filtering>false</filtering>
        </p>
        <p>
          </resource>
        </p>
        <p>
          </resources>
        </p>
        <p>
          </build>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>{% hint style="info" %}
还存在一个问题是，我用逆向工程创建时不小心创建了两次，xml中路径发生改变
{% endhint %}



