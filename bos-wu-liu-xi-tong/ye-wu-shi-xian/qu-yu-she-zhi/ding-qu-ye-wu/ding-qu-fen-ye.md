# 定区分页

## 第一步：修改定区jsp页面中datagrid的URL地址

![](../../../../.gitbook/assets/image%20%28296%29.png)

## 第二步：在定区Action中提供pageQuery方法

![](../../../../.gitbook/assets/image%20%28144%29.png)

## 第三步：在Decidedzone.hbm.xml中修改，查询定区对象时需要立即加载关联的取派员对象

![](../../../../.gitbook/assets/image%20%2826%29.png)

