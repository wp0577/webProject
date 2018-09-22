---
description: 通过comboBox插件实现下拉菜单的数据显示，并提供字段检索
---

# comboBox

![](../../../../../.gitbook/assets/image%20%28157%29.png)

![](../../../../../.gitbook/assets/image%20%2860%29.png)

## 第一步：修改页面中combobox：

![](../../../../../.gitbook/assets/image%20%2893%29.png)

如果想要在选择框输入内容并通过内容检索的话可以将mode模式设置成remote如上图：

![](../../../../../.gitbook/assets/image%20%2823%29.png)

并且发送的请求属性为‘q‘，需要在action方法中得到q的值。



## 服务器端

![](../../../../../.gitbook/assets/image%20%28119%29.png)

### 在RegionDao中扩展方法

![](../../../../../.gitbook/assets/image%20%28118%29.png)

