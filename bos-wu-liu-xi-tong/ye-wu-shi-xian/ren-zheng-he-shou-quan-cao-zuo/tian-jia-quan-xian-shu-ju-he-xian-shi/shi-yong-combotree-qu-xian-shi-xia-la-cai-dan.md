# 使用Combotree去显示下拉菜单

## 显示效果

![](../../../../.gitbook/assets/image%20%28173%29.png)

## 前端配置

![](../../../../.gitbook/assets/image%20%2859%29.png)

## 后端配置

![](../../../../.gitbook/assets/image%20%2878%29.png)

### 修改function.java内容

![](../../../../.gitbook/assets/image%20%28179%29.png)

因为combotree需要Id，和text两个参数，而如果不增加getText的话，只会得到id

### 重写IFunctionDaoImpl内容

![](../../../../.gitbook/assets/image%20%283%29.png)

我们只需要得到function中父节点为null的数据，因为这些数据的children就会包含我们需要的节点。

