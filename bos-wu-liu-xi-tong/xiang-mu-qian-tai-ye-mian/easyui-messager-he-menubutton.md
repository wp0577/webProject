# easyUi messager和menuButton

## alert使用

![](../../.gitbook/assets/image%20%28138%29.png)

## confirm

![](../../.gitbook/assets/image%20%28154%29.png)

![](../../.gitbook/assets/image%20%2888%29.png)

## show

![](../../.gitbook/assets/image%20%28135%29.png)

![](../../.gitbook/assets/image%20%28137%29.png)

![](../../.gitbook/assets/image%20%28136%29.png)

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

![](../../.gitbook/assets/image%20%28105%29.png)

