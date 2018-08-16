# datagrid使用方法

 

### 1.1   将静态HTML渲染为datagrid样式

&lt;!-- 方式一：将静态HTML渲染为datagrid样式 --&gt;

                  &lt;table class="easyui-datagrid"&gt;

                                    &lt;thead&gt;

                                                      &lt;tr&gt;

                                                                        &lt;th data-options="field:'id'"&gt;编号&lt;/th&gt;

                                                                        &lt;th data-options="field:'name'"&gt;姓名&lt;/th&gt;

                                                                        &lt;th data-options="field:'age'"&gt;年龄&lt;/th&gt;

                                                      &lt;/tr&gt;

                                    &lt;/thead&gt;

                                    &lt;tbody&gt;

                                                      &lt;tr&gt;

                                                                        &lt;td&gt;001&lt;/td&gt;

                                                                        &lt;td&gt;小明&lt;/td&gt;

                                                                        &lt;td&gt;90&lt;/td&gt;

                                                      &lt;/tr&gt;

                                                      &lt;tr&gt;

                                                                        &lt;td&gt;002&lt;/td&gt;

                                                                        &lt;td&gt;老王&lt;/td&gt;

                                                                        &lt;td&gt;3&lt;/td&gt;

                                                      &lt;/tr&gt;

                                    &lt;/tbody&gt;

                  &lt;/table&gt;

### 1.2   发送ajax请求获取json数据创建datagrid

提供json文件：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.jpg)

&lt;!-- 方式二：发送ajax请求获取json数据创建datagrid --&gt;

                  &lt;table data-options="url:'${pageContext.request.contextPath }/json/datagrid\_data.json'"

                                                      class="easyui-datagrid"&gt;

                                    &lt;thead&gt;

                                                      &lt;tr&gt;

                                                                        &lt;th data-options="field:'id'"&gt;编号&lt;/th&gt;

                                                                        &lt;th data-options="field:'name'"&gt;姓名&lt;/th&gt;

                                                                        &lt;th data-options="field:'age'"&gt;年龄&lt;/th&gt;

                                                      &lt;/tr&gt;

                                    &lt;/thead&gt;

                  &lt;/table&gt;

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.jpg)

### 1.3   使用easyUI提供的API创建datagrid（掌握）

                  &lt;!-- 方式三：使用easyUI提供的API创建datagrid --&gt;

                  &lt;script type="text/javascript"&gt;

                                    $\(**function**\(\){

                                                      //页面加载完成后，创建数据表格datagrid

                                                      $\("\#mytable"\).datagrid\({

                                                                        //定义标题行所有的列

                                                                        columns:\[\[

                                                                                  {title:'编号',field:'id',checkbox:**true**},

                                                                                  {title:'姓名',field:'name'},

                                                                                  {title:'年龄',field:'age'},

                                                                                  {title:'地址',field:'address'}

                                                                                  \]\],

                                                                        //指定数据表格发送ajax请求的地址

                                                                        url:'${pageContext.request.contextPath }/json/datagrid\_data.json',

                                                                        rownumbers:**true**,

                                                                        singleSelect:**true**,

                                                                        //定义工具栏

                                                                        toolbar:\[

                                                                                 {text:'添加',iconCls:'icon-add',

                                                                                           //为按钮绑定单击事件

                                                                                           handler:**function**\(\){

                                                                                                            alert\('add...'\);

                                                                                           }

                                                                                 },

                                                                                 {text:'删除',iconCls:'icon-remove'},

                                                                                 {text:'修改',iconCls:'icon-edit'},

                                                                                 {text:'查询',iconCls:'icon-search'}

                                                                                 \],

                                                                        //显示分页条

                                                                        pagination:**true**

                                                      }\);

                                    }\);

                  &lt;/script&gt;

如果数据表格中使用了分页条，要求服务端响应的json变为：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image006.jpg)

请求：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image008.jpg)

响应：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image010.jpg)

