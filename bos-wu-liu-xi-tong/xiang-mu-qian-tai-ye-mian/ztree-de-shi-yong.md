---
description: 使用ztree插件来表现菜单栏
---

# ztree的使用

## 讲zTree代码放入accordion各title的代码块中：

```text
<div title="Title2" data-options="iconCls:'icon-reload'" style="padding:10px;">
	<ul id="ztree2" class="ztree"></ul>
		<script type="text/javascript">
            $(function(){
                //页面加载完成后，执行这段代码----动态创建ztree
                var setting2 = {
                    data: {
                        simpleData: {
                            enable: true//使用简单json数据构造ztree节点
                        }
                    }
                };
                //构造节点数据
                var zNodes2 = [
                    {"id":"1","pId":"0","name":"节点一"},//每个json对象表示一个节点数据
                    {"id":"2","pId":"1","name":"节点二"},
                    {"id":"3","pId":"2","name":"节点三"},
							{"id":"4","pId":"0","name":"tree4"},
							{"id":"5","pId":"4","name":"tree5"},
							{"id":"6","pId":"0","name":"tree6"},
							{"id":"7","pId":"6","name":"tree7"}
                ];
                //调用API初始化ztree
                $.fn.zTree.init($("#ztree2"), setting2, zNodes2);
            });
		</script>

</div>
```

里面用到的是简单json数据构造ztree节点。

## 使用Ajax获取json数据创建节点

```text
<%--使用ajax获得json数据形成菜单--%>
<script type="text/javascript">
	$(function () {
			var setting2 = {
			data: {
					simpleData: {
					enable: true//使用简单json数据构造ztree节点
					}
			}
			};
			$.post("${pageContext.request.contextPath}/json/menu.json", {},
			function(data){
			$.fn.zTree.init($("#ztree2"), setting2, data);
			}, "json");
		})

</script>
```

## 为ztree绑定事件

```text
callback: {
    //为ztree节点绑定单击事件
    onClick: function(event, treeId, treeNode){
        if(treeNode.page != undefined){
            //判断选项卡是否已经存在
            var e = $("#tt").tabs("exists",treeNode.name);
            if(e){
                //已经存在，选中
                $("#tt").tabs("select",treeNode.name);
            }else{
                //动态添加一个选项卡
                $("#tt").tabs("add",{
                    title:treeNode.name,
                    closable:true,
                    content:'<iframe frameborder="0" height="100%" width="100%" src="'+treeNode.page+'"></iframe>'
                });
            }
        }
    }
}
```

