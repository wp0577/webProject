# 职业批量删除

## 页面配置

数据表格datagrid提供的方法，用于获取所有选中的行：

![](../../../.gitbook/assets/image%20%2837%29.png)

### 修改删除按钮绑定的事件：

```text
function doDelete(){
		//获取数据表格中所有选中的行，返回数组对象
		var rows = $("#grid").datagrid("getSelections");
		if(rows.length == 0){
			//没有选中记录，弹出提示
			$.messager.alert("提示信息","请选择需要删除的取派员！","warning");
		}else{
			//选中了取派员,弹出确认框
			$.messager.confirm("删除确认","你确定要删除选中的取派员吗？",function(r){
				if(r){
					
					var array = new Array();
					//确定,发送请求
					//获取所有选中的取派员的id
					for(var i=0;i<rows.length;i++){
						var staff = rows[i];//json对象
						var id = staff.id;
						array.push(id);
					}
					var ids = array.join(",");//1,2,3,4,5
					location.href = "staffAction_deleteBatch.action?ids="+ids;
				}
			});
		}
	}

```

## 服务端配置

```java
//属性驱动，接收页面提交的ids参数
	private String ids;
	
	/**
	 * 取派员批量删除
	 */
	public String deleteBatch(){
		staffService.deleteBatch(ids);
		return LIST;
	}

```

```text
/**
	 * 取派员批量删除
	 * 逻辑删除，将deltag改为1
	 */
	public void deleteBatch(String ids) {//1,2,3,4
		if(StringUtils.isNotBlank(ids)){
			String[] staffIds = ids.split(",");
			for (String id : staffIds) {
				staffDao.executeUpdate("staff.delete", id);
			}
		}
	}

```



