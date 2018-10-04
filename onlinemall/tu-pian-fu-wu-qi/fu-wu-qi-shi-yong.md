# 服务器使用

### 1.1. Java客户端：

![](../../.gitbook/assets/image%20%28212%29.png)

Maven环境：

![](../../.gitbook/assets/image%20%28115%29.png)

### 1.2. 上传图片

#### 1.2.1.                  上传步骤

1、加载配置文件，配置文件中的内容就是tracker服务的地址。

配置文件内容：tracker\_server=192.168.25.133:22122

2、创建一个TrackerClient对象。直接new一个。

3、使用TrackerClient对象创建连接，获得一个TrackerServer对象。

4、创建一个StorageServer的引用，值为null

5、创建一个StorageClient对象，需要两个参数TrackerServer对象、StorageServer的引用

6、使用StorageClient对象上传图片。

7、返回数组。包含组名和图片的路径。

#### 1.2.2.                  代码

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>public</b>  <b>class</b> FastDFSTest {</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testFileUpload() <b>throws</b> Exception {</p>
        <p>// 1、加载配置文件，配置文件中的内容就是tracker服务的地址。</p>
        <p>ClientGlobal.init("D:/workspaces-itcast/e3-manager-web/src/main/resources/resource/client.conf");</p>
        <p>// 2、创建一个TrackerClient对象。直接new一个。</p>
        <p>TrackerClient trackerClient = <b>new</b> TrackerClient();</p>
        <p>// 3、使用TrackerClient对象创建连接，获得一个TrackerServer对象。</p>
        <p>TrackerServer trackerServer = trackerClient.getConnection();</p>
        <p>// 4、创建一个StorageServer的引用，值为null</p>
        <p>StorageServer storageServer = <b>null</b>;</p>
        <p>// 5、创建一个StorageClient对象，需要两个参数TrackerServer对象、StorageServer的引用</p>
        <p>StorageClient storageClient = <b>new</b> StorageClient(trackerServer, storageServer);</p>
        <p>// 6、使用StorageClient对象上传图片。</p>
        <p>//扩展名不带“.”</p>
        <p>String[] strings = storageClient.upload_file("D:/Documents/Pictures/images/200811281555127886.jpg",
          "jpg", <b>null</b>);</p>
        <p>// 7、返回数组。包含组名和图片的路径。</p>
        <p> <b>for</b> (String string : strings) {</p>
        <p>System.<b>out</b>.println(string);</p>
        <p>}</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.3. 使用工具类上传

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testFastDfsClient() <b>throws</b> Exception {</p>
        <p>FastDFSClient fastDFSClient = <b>new</b> FastDFSClient("D:/workspaces-itcast/e3-manager-web/src/main/resources/resource/client.conf");</p>
        <p>String file = fastDFSClient.uploadFile("D:/Documents/Pictures/images/2f2eb938943d.jpg");</p>
        <p>System.<b>out</b>.println(file);</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>