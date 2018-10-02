# 首页轮播图

### 1.1. 功能分析

根据分类id查询内容列表，把内容展示到首页。

内容分类id需要是固定的。可以配置到属性文件中。

展示首页之前，先查询内容列表，然后展示到首页。

### 1.2. Dao层

单表查询。可以使用逆向工程。

### 1.3. Service层

参数：内容分类id

返回值：List&lt;TbContent&gt;

业务逻辑：

根据分类id查询内容列表。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Override</p>
        <p> <b>public</b> List
          <TbContent>getContentList(<b>long</b> cid) {</p>
        <p>//根据分类id查询内容列表</p>
        <p>//设置查询条件</p>
        <p>TbContentExample example = <b>new</b> TbContentExample();</p>
        <p>Criteria criteria = example.createCriteria();</p>
        <p>criteria.andCategoryIdEqualTo(cid);</p>
        <p>//执行查询</p>
        <p>List
          <TbContent>list = contentMapper.selectByExample(example);</p>
        <p> <b>return</b> list;</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>需要把接口安装到本地仓库。

### 1.4. 表现层

引用服务：

![](../../.gitbook/assets/image%20%28174%29.png)

Springmvc.xml中添加引用：

![](../../.gitbook/assets/image%20%2845%29.png)

需要在展示首页的Controller中添加业务逻辑。

