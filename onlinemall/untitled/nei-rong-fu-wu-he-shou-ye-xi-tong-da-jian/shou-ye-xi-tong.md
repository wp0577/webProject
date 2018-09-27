# 首页系统

### 1.1. 工程搭建

可以参考e3-manager-web工程搭建

### 1.2. 功能分析

请求的url：/index

Web.xml中的欢迎页配置：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)

[http://localhost:8082/index.html](http://localhost:8082/index.html)

参数：没有

返回值：String 逻辑视图

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Controller</p>
        <p><b>public</b>  <b>class</b> IndexController {</p>
        <p>@RequestMapping("/index")</p>
        <p> <b>public</b> String showIndex() {</p>
        <p> <b>return</b> "index";</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>## 2.  首页动态展示分析

内容信息要从数据库中获得

### 2.1. 动态展示分析

1、内容需要进行分类

2、分类下有子分类，需要动态管理。

3、分类下有内容列表

4、单点的内容信息

a\)       有图片

b\)      有链接

c\)       有标题

d\)      有价格

e\)       包含大文本类型，可以作为公告

需要一个内容分类表和一个内容表。内容分类和内容表是一对多的关系。

内容分类表，需要存储树形结构的数据。

内容分类表：tb\_content\_category

内容表：tb\_content

需要有后台来维护内容信息。Cms系统。

需要创建一个内容服务系统。可以参考e3-manager创建。

E3-content：聚合工程打包方式pom

\|--e3-content-interface jar

\|--e3-content-Service  war

