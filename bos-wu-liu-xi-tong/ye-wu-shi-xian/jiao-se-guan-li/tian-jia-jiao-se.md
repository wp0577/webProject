# 添加角色

## 1.页面部分

![](../../../.gitbook/assets/image%20%28254%29.png)

第一步：修改页面，使用ztree勾选效果（checkbox）

![](../../../.gitbook/assets/image%20%28224%29.png)

第二步：修改ajax方法的请求URL地址

![](../../../.gitbook/assets/image%20%28257%29.png)

第三步：为保存按钮绑定事件，提交表单

因为ztree中的check是假check，所以我们需要自己去获取数据

![](../../../.gitbook/assets/image%20%28103%29.png)

![](../../../.gitbook/assets/image%20%2873%29.png)

## 2.服务端实现

service代码

![](../../../.gitbook/assets/image%20%2818%29.png)

