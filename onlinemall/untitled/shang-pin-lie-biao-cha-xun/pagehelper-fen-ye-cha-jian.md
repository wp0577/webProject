# pageHelper分页插件

## 1.1.1.                  分页处理

逆向工程生成的代码是不支持分页处理的，如果想进行分页需要自己编写mapper，这样就失去逆向工程的意义了。为了提高开发效率可以使用mybatis的分页插件PageHelper。

### 1.2. 分页插件PageHelper

#### 1.2.1.                  Mybatis分页插件 - PageHelper说明

如果你也在用Mybatis，建议尝试该分页插件，这个一定是最方便使用的分页插件。

该插件目前支持Oracle,Mysql,MariaDB,SQLite,Hsqldb,PostgreSQL六种数据库分页。

#### 1.2.2.                  使用方法

第一步：把PageHelper依赖的jar包添加到工程中。官方提供的代码对逆向工程支持的不好，使用参考资料中的pagehelper-fix。

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)

第二步：在Mybatis配置xml中配置拦截器插件:

&lt;plugins&gt;

    &lt;!-- com.github.pagehelper为PageHelper类所在包名 --&gt;

    &lt;plugin interceptor="com.github.pagehelper.PageHelper"&gt;

        &lt;!-- 设置数据库类型 Oracle,Mysql,MariaDB,SQLite,Hsqldb,PostgreSQL六种数据库--&gt;       

        &lt;property name="dialect" value="mysql"/&gt;

    &lt;/plugin&gt;

&lt;/plugins&gt;

第二步：在代码中使用

1、设置分页信息：

    //获取第1页，10条内容，默认查询总数count

    PageHelper.startPage\(1, 10\);

    //紧跟着的第一个select方法会被分页

List&lt;Country&gt; list = countryMapper.selectIf\(1\);

2、取分页信息

//分页后，实际返回的结果list类型是Page&lt;E&gt;，如果想取出分页信息，需要强制转换为Page&lt;E&gt;，

Page&lt;Country&gt; listCountry = \(Page&lt;Country&gt;\)list;

listCountry.getTotal\(\);

3、取分页信息的第二种方法

//获取第1页，10条内容，默认查询总数count

PageHelper.startPage\(1, 10\);

List&lt;Country&gt; list = countryMapper.selectAll\(\);

//用PageInfo对结果进行包装

PageInfo page = new PageInfo\(list\);

//测试PageInfo全部属性

//PageInfo包含了非常全面的分页属性

assertEquals\(1, page.getPageNum\(\)\);

assertEquals\(10, page.getPageSize\(\)\);

assertEquals\(1, page.getStartRow\(\)\);

assertEquals\(10, page.getEndRow\(\)\);

assertEquals\(183, page.getTotal\(\)\);

assertEquals\(19, page.getPages\(\)\);

assertEquals\(1, page.getFirstPage\(\)\);

assertEquals\(8, page.getLastPage\(\)\);

assertEquals\(true, page.isFirstPage\(\)\);

assertEquals\(false, page.isLastPage\(\)\);

assertEquals\(false, page.isHasPreviousPage\(\)\);

assertEquals\(true, page.isHasNextPage\(\)\);

#### 1.2.3.                  分页测试

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testPageHelper() <b>throws</b> Exception {</p>
        <p>//初始化spring容器</p>
        <p>ApplicationContext applicationContext = <b>new</b> ClassPathXmlApplicationContext("classpath:spring/applicationContext-*.xml");</p>
        <p>//获得Mapper的代理对象</p>
        <p>TbItemMapper itemMapper = applicationContext.getBean(TbItemMapper.<b>class</b>);</p>
        <p>//设置分页信息</p>
        <p>PageHelper.startPage(1, 30);</p>
        <p>//执行查询</p>
        <p>TbItemExample example = <b>new</b> TbItemExample();</p>
        <p>List
          <TbItem>list = itemMapper.selectByExample(example);</p>
        <p>//取分页信息</p>
        <p>PageInfo
          <TbItem>pageInfo = <b>new</b> PageInfo
            <>(list);</p>
        <p>System.<b>out</b>.println(pageInfo.getTotal());</p>
        <p>System.<b>out</b>.println(pageInfo.getPages());</p>
        <p>System.<b>out</b>.println(pageInfo.getPageNum());</p>
        <p>System.<b>out</b>.println(pageInfo.getPageSize());</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>