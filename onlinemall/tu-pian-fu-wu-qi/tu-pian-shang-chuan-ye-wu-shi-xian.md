# 图片上传业务实现

### 1.1. 功能分析

![](../../.gitbook/assets/image%20%28128%29.png)

![](../../.gitbook/assets/image%20%28189%29.png)

![](../../.gitbook/assets/image%20%2873%29.png)

![](../../.gitbook/assets/image%20%28151%29.png)

使用的是KindEditor的多图片上传插件。

KindEditor 4.x 文档

[http://kindeditor.net/doc.php](http://kindeditor.net/doc.php)

请求的url：/pic/upload

参数：MultiPartFile uploadFile

返回值：  
 ![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image005.png)

![](../../.gitbook/assets/image%20%28163%29.png)

可以创建一个pojo对应返回值。可以使用map

业务逻辑：

1、接收页面传递的图片信息uploadFile

2、把图片上传到图片服务器。使用封装的工具类实现。需要取文件的内容和扩展名。

3、图片服务器返回图片的url

4、将图片的url补充完整，返回一个完整的url。

5、把返回结果封装到一个Map对象中返回。

1、需要把commons-io、fileupload 的jar包添加到工程中。

2、配置多媒体解析器。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <!-- 定义文件上传解析器 -->
        </p>
        <p>
          <bean id="multipartResolver" </p>
            <p>class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></p>
            <p>
              <!-- 设定默认编码 -->
            </p>
            <p>
              <property name="defaultEncoding" value="UTF-8"></property>
            </p>
            <p>
              <!-- 设定文件上传的最大值5MB，5*1024*1024 -->
            </p>
            <p>
              <property name="maxUploadSize" value="5242880"></property>
            </p>
            <p>
          </bean>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.2. Controller

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> PictureController {</p>
        <p>@Value("${IMAGE_SERVER_URL}")</p>
        <p> <b>private</b> String IMAGE_SERVER_URL;</p>
        <p>@RequestMapping("/pic/upload")</p>
        <p>@ResponseBody</p>
        <p> <b>public</b> Map fileUpload(MultipartFile uploadFile) {</p>
        <p> <b>try</b> {</p>
        <p>//1、取文件的扩展名</p>
        <p>String originalFilename = uploadFile.getOriginalFilename();</p>
        <p>String extName = originalFilename.substring(originalFilename.lastIndexOf(".")
          + 1);</p>
        <p>//2、创建一个FastDFS的客户端</p>
        <p>FastDFSClient fastDFSClient = <b>new</b> FastDFSClient("classpath:resource/client.conf");</p>
        <p>//3、执行上传处理</p>
        <p>String path = fastDFSClient.uploadFile(uploadFile.getBytes(), extName);</p>
        <p>//4、拼接返回的url和ip地址，拼装成完整的url</p>
        <p>String url = IMAGE_SERVER_URL + path;</p>
        <p>//5、返回map</p>
        <p>Map result = <b>new</b> HashMap
          <>();</p>
        <p>result.put("error", 0);</p>
        <p>result.put("url", url);</p>
        <p> <b>return</b> result;</p>
        <p>} <b>catch</b> (Exception e) {</p>
        <p>e.printStackTrace();</p>
        <p>//5、返回map</p>
        <p>Map result = <b>new</b> HashMap
          <>();</p>
        <p>result.put("error", 1);</p>
        <p>result.put("message", "图片上传失败");</p>
        <p> <b>return</b> result;</p>
        <p>}</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.3. 解决浏览器兼容性的问题

KindEditor的图片上传插件，对浏览器兼容性不好。

使用@ResponseBody注解返回java对象，

Content-Type:application/json;charset=UTF-8

返回字符串时：

Content-Type:text/plan;charset=UTF-8

![](../../.gitbook/assets/image%20%2823%29.png)

指定响应结果的content-type：

![](../../.gitbook/assets/image%20%28168%29.png)

KindEditor的多图片上传插件最后响应的content-type是text/plan格式的json字符串。兼容性是最好的。

![](../../.gitbook/assets/image%20%28188%29.png)



