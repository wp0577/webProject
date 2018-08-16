# 动态添加选项卡

![](../../.gitbook/assets/image%20%286%29.png)

###  选中指定的选项卡和判断某个选项卡是否存在

![](../../.gitbook/assets/image%20%2857%29.png)

```text
<a id="but1" class="easyui-linkbutton">添加一个选项卡</a>
<script type="text/javascript">
	$(function(){
		//页面加载完成后，为上面的按钮绑定事件
		$("#but1").click(function(){
			//判断“系统管理”选项卡是否存在
			var e = $("#mytabs").tabs("exists","系统管理");
			if(e){
				//已经存在，选中就可以
				$("#mytabs").tabs("select","系统管理");
			}else{
				//调用tabs对象的add方法动态添加一个选项卡
				$("#mytabs").tabs("add",{
					title:'系统管理',
					iconCls:'icon-edit',
					closable:true,
					content:'<iframe frameborder="0" height="100%" width="100%" src="https://www.baidu.com"></iframe>'
				});
			}
		});
	});
</script>

```

