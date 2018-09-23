# 用户添加

![](../../../.gitbook/assets/image%20%286%29.png)

## 页面调整

```text
第一步：发送ajax请求，获取角色数据，在回调函数中动态展示角色数据，展示为checkbox
<tr>
<td>选择角色:</td>
<td colspan="3" id="roleTD">
	<script type="text/javascript">
		$(function(){
			//页面加载完成后，发送ajax请求，获取所有的角色数据
			$.post('roleAction_listajax.action',function(data){
				//在ajax回调函数中，解析json数据，展示为checkbox
				for(var i=0;i<data.length;i++){
					var id = data[i].id;
					var name = data[i].name;
					$("#roleTD").append('<input id="'+id+'" type="checkbox" name="roleIds" value="'+id+'"><label for="'+id+'">'+name+'</label>');
				}
			});
		});
	</script>
</td>
</tr>

```

第二步：在RoleAction中提供listajax方法，查询所有角色，返回json数据

![](../../../.gitbook/assets/image%20%2818%29.png)

第三步：为保存按钮绑定事件，提交表单

![](../../../.gitbook/assets/image%20%287%29.png)

## 服务端实现

![](../../../.gitbook/assets/image%20%2810%29.png)

![](../../../.gitbook/assets/image%20%2869%29.png)

