# datagrid编辑功能使用

### 数据表格编辑功能是以列为单位 

通过数据表格中的列属性定区具体那一列具有编辑功能：

![](../../../../.gitbook/assets/image%20%28131%29.png)

开始编辑：

![](../../../../.gitbook/assets/image%20%28135%29.png)

结束编辑：

![](../../../../.gitbook/assets/image%20%282%29.png)

 插入一行数据：

![](../../../../.gitbook/assets/image%20%28224%29.png)

 删除一行：

![](../../../../.gitbook/assets/image%20%28123%29.png)

 获得指定行对象的索引

![](../../../../.gitbook/assets/image%20%2867%29.png)

 数据表格提供的用于监听结束编辑事件

![](../../../../.gitbook/assets/image%20%28132%29.png)

```text
<table id="mytable"></table>
	<!-- 方式三：使用easyUI提供的API创建datagrid -->
	<script type="text/javascript">
		$(function(){
			var myIndex = -1;//全局变量，值为正在编辑行的索引
			//页面加载完成后，创建数据表格datagrid
			$("#mytable").datagrid({
				//定义标题行所有的列
				columns:[[
				          {title:'编号',field:'id',checkbox:true},
				          {width:150,title:'姓名',field:'name',editor:{
				        	  								type:'validatebox',
				        	  								options:{}
				          								   }},
				          {width:150,title:'年龄',field:'age',editor:{
								type:'numberbox',
  								options:{}
								   }},
				          {width:150,title:'日期',field:'address',editor:{
								type:'datebox',
  								options:{}
								   }}
				          ]],
				//指定数据表格发送ajax请求的地址
				url:'${pageContext.request.contextPath }/json/datagrid_data.json',
				rownumbers:true,
				singleSelect:true,
				//定义工具栏
				toolbar:[
				         {text:'添加',iconCls:'icon-add',
				        	 //为按钮绑定单击事件
				        	 handler:function(){
				        		 $("#mytable").datagrid("insertRow",{
				        			 index:0,//在第一行插入数据
				        			 row:{}//空行
				        		 });
				        		 $("#mytable").datagrid("beginEdit",0);
				        		 myIndex = 0;
				         	 }
				         },
				         {text:'删除',iconCls:'icon-remove',handler:function(){
				        	//获得选中的行对象
				        	 var rows = $("#mytable").datagrid("getSelections");
				        	 if(rows.length == 1){
				        		 var row = rows[0];
				        		 //获得指定行对象的索引
				        		 myIndex = $("#mytable").datagrid("getRowIndex",row);
				        	 }
				        	 $("#mytable").datagrid("deleteRow",myIndex);
				        	 //$.post();
				         }},
				         {text:'修改',iconCls:'icon-edit',handler:function(){
				        	 //获得选中的行对象
				        	 var rows = $("#mytable").datagrid("getSelections");
				        	 if(rows.length == 1){
				        		 var row = rows[0];
				        		 //获得指定行对象的索引
				        		 myIndex = $("#mytable").datagrid("getRowIndex",row);
				        	 }
				        	 $("#mytable").datagrid("beginEdit",myIndex);
				         }},
				         {text:'保存',iconCls:'icon-save',handler:function(){
				        	 $("#mytable").datagrid("endEdit",myIndex);
				         }}
				         ],
				//显示分页条
				pagination:true,
				pageList:[3,5,7,10],
				//数据表格提供的用于监听结束编辑事件
				onAfterEdit:function(index,data,changes){
					console.info(data);
					$.post();
				}
			});
		});
	</script>

```

