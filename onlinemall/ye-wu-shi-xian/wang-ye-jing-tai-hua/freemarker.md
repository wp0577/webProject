# freemarker

### 1.1. 什么是freemarker

FreeMarker是一个用Java语言编写的模板引擎，它基于模板来生成文本输出。FreeMarker与Web容器无关，即在Web运行时，它并不知道Servlet或HTTP。它不仅可以用作表现层的实现技术，而且还可以用于生成XML，JSP或Java 等。

目前企业中:主要用Freemarker做静态页面或是页面展示

### 1.2. Freemarker的使用方法

把freemarker的jar包添加到工程中。

Maven工程添加依赖

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <dependency>
        </p>
        <p>
          <groupId>org.freemarker</groupId>
        </p>
        <p>
          <artifactId>freemarker</artifactId>
        </p>
        <p>
          <version>2.3.23</version>
        </p>
        <p>
          </dependency>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>原理：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)

使用步骤：

第一步：创建一个Configuration对象，直接new一个对象。构造方法的参数就是freemarker对于的版本号。

第二步：设置模板文件所在的路径。

第三步：设置模板文件使用的字符集。一般就是utf-8.

第四步：加载一个模板，创建一个模板对象。

第五步：创建一个模板使用的数据集，可以是pojo也可以是map。一般是Map。

第六步：创建一个Writer对象，一般创建一FileWriter对象，指定生成的文件名。

第七步：调用模板对象的process方法输出文件。

第八步：关闭流。

模板：

${hello}

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> genFile() <b>throws</b> Exception {</p>
        <p>// 第一步：创建一个Configuration对象，直接new一个对象。构造方法的参数就是freemarker对于的版本号。</p>
        <p>Configuration configuration = <b>new</b> Configuration(Configuration.getVersion());</p>
        <p>// 第二步：设置模板文件所在的路径。</p>
        <p>configuration.setDirectoryForTemplateLoading(<b>new</b> File("D:/workspaces-itcast/term197/e3-item-web/src/main/webapp/WEB-INF/ftl"));</p>
        <p>// 第三步：设置模板文件使用的字符集。一般就是utf-8.</p>
        <p>configuration.setDefaultEncoding("utf-8");</p>
        <p>// 第四步：加载一个模板，创建一个模板对象。</p>
        <p>Template template = configuration.getTemplate("hello.ftl");</p>
        <p>// 第五步：创建一个模板使用的数据集，可以是pojo也可以是map。一般是Map。</p>
        <p>Map dataModel = <b>new</b> HashMap
          <>();</p>
        <p>//向数据集中添加数据</p>
        <p>dataModel.put("hello", "this is my first freemarker test.");</p>
        <p>// 第六步：创建一个Writer对象，一般创建一FileWriter对象，指定生成的文件名。</p>
        <p>Writer out = <b>new</b> FileWriter(<b>new</b> File("D:/temp/term197/out/hello.html"));</p>
        <p>// 第七步：调用模板对象的process方法输出文件。</p>
        <p>template.process(dataModel, out);</p>
        <p>// 第八步：关闭流。</p>
        <p>out.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>