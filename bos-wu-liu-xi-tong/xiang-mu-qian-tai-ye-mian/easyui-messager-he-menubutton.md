# easyUi messager和menuButton

## alert使用

![](../../.gitbook/assets/image%20%28229%29.png)

## confirm

![](../../.gitbook/assets/image%20%28257%29.png)

![](../../.gitbook/assets/image%20%28146%29.png)

## show

![](../../.gitbook/assets/image%20%28220%29.png)

![](../../.gitbook/assets/image%20%28228%29.png)

![](../../.gitbook/assets/image%20%28223%29.png)

## menuBotton菜单使用

```text
<!-- 制作菜单 -->
	<a data-options="iconCls:'icon-help',menu:'#mm'" class="easyui-menubutton">控制面板</a>
	
	<!-- 使用div元素制作下拉菜单 -->
	<div id="mm">
		<div onclick="alert(1111)" data-options="iconCls:'icon-edit'">修改密码</div>
		<div>联系管理员</div>
		<div class="menu-sep"></div>
		<div>退出系统</div>
	</div>

```

![](../../.gitbook/assets/image%20%28171%29.png)

