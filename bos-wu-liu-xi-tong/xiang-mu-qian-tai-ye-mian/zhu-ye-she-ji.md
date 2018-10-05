# 主页设计

## 1.页面布局

```text
<body class="easyui-layout">
	<!-- 使用div元素描述每个区域 -->
	<div style="height: 100px" data-options="region:'north'">北部区域</div>
	<div style="width: 200px" data-options="region:'west'">西部区域</div>
	<div data-options="region:'center'">中心区域</div>
	<div style="width: 100px" data-options="region:'east'">东部区域</div>
	<div style="height: 50px" data-options="region:'south'">南部区域</div>
</body>

```

![](../../.gitbook/assets/image%20%2811%29.png)

## 2 accordion折叠面板

```text
<!-- 制作accordion折叠面板 
			fit:true----自适应(填充父容器)
		-->
		<div class="easyui-accordion" data-options="fit:true">
			<!-- 使用子div表示每个面板 -->
			<div data-options="iconCls:'icon-cut'" title="面板一">1111</div>
			<div title="面板二">2222</div>
			<div title="面板三">3333</div>
		</div>
```

![](../../.gitbook/assets/image%20%286%29.png)

## 3.tabs选项卡面板

```text
<!-- 制作一个tabs选项卡面板 -->
		<div class="easyui-tabs" data-options="fit:true">
			<!-- 使用子div表示每个面板 -->
			<div data-options="iconCls:'icon-cut'" title="面板一">1111</div>
			<div data-options="closable:true" title="面板二">2222</div>
			<div title="面板三">3333</div>
		</div>
```

![](../../.gitbook/assets/image%20%28113%29.png)

