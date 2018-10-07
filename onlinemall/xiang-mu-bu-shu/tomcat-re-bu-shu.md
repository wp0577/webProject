# tomcat热部署

可以使用maven实现tomcat热部署。Tomcat启动时 部署工程。

Tomcat有个后台管理功能，可以实现工程热部署。

配置方法：

第一步：需要修改tomcat的conf/tomcat-users.xml配置文件。添加用户名、密码、权限。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <role rolename="manager-gui" />
        </p>
        <p>
          <role rolename="manager-script" />
        </p>
        <p>
          <user username="tomcat" password="tomcat" roles="manager-gui, manager-script"
          />
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第二步：重新启动tomcat。

使用maven的tomcat插件实现热部署：

第一步：配置tomcat插件，需要修改工程的pom文件。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <build>
        </p>
        <p>
          <plugins>
        </p>
        <p>
          <!-- 配置Tomcat插件 -->
        </p>
        <p>
          <plugin>
        </p>
        <p>
          <groupId>org.apache.tomcat.maven</groupId>
        </p>
        <p>
          <artifactId>tomcat7-maven-plugin</artifactId>
        </p>
        <p>
          <configuration>
        </p>
        <p>
          <port>8081</port>
        </p>
        <p>
          <path>/</path>
        </p>
        <p>
          <url>http://192.168.25.135:8080/manager/text</url>
        </p>
        <p>
          <username>tomcat</username>
        </p>
        <p>
          <password>tomcat</password>
        </p>
        <p>
          </configuration>
        </p>
        <p>
          </plugin>
        </p>
        <p>
          </plugins>
        </p>
        <p>
          </build>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第二步：使用maven命令进行部署。

tomcat7:deploy

tomcat7:redeploy

部署的路径是“/”会把系统部署到webapps/ROOT目录下。

部署工程跳过测试：

clean tomcat7:redeploy -DskipTests

#### 1.1.1.    工程部署

每个工程运行在不同的tomcat上，修改tomcat的端口号。

