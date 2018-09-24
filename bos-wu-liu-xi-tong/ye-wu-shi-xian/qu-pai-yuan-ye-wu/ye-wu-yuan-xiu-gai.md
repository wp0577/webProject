# 业务员修改

## 前端

### 第一步：为数据表格绑定双击事件

![](../../../.gitbook/assets/image%20%28120%29.png)

![](../../../.gitbook/assets/image%20%2825%29.png)

### 第二步：复制页面中添加取派员窗口，获得修改取派员窗口

![](../../../.gitbook/assets/image%20%2878%29.png)

### 第三步：定义function

```text
//数据表格绑定的双击行事件对应的函数
	function doDblClickRow(rowIndex, rowData){
		//打开修改取派员窗口
		$('#editStaffWindow').window("open");
		//使用form表单对象的load方法回显数据
		$("#editStaffForm").form("load",rowData);
	}

```

## 服务器端

{% code-tabs %}
{% code-tabs-item title="staffAction" %}
```text
public String edit() {
        //不能直接通过model对象去更新，因为edit表单传来的字段并不一定覆盖了model所有的字段
        //这样更新的时候数据库原本的model其他的字段会被设置为null
        //先通过id查到staff对象
        Staff staff = (Staff) iStaffService.getById(model.getId());
        //将model对象里赋值的属性赋值给staff
        staff.setTelephone(model.getTelephone());
        staff.setHaspda(model.getHaspda());
        staff.setName(model.getName());
        staff.setStation(model.getStation());
        staff.setHaspda(model.getHaspda());

        iStaffService.save(staff);

        return "list";
    }

```
{% endcode-tabs-item %}
{% endcode-tabs %}



