# 添加定区

## 使用combobox展示取派员数据

![](../../../../.gitbook/assets/image%20%28110%29.png)

### 在StaffAction中提供listajax方法，查询所有未删除的取派员，返回json

```text
/**
	 * 查询所有未删除的取派员，返回json
	 */
	public String listajax(){
		List<Staff> list = staffService.findListNotDelete();
		this.java2Json(list, new String[]{"decidedzones"});
		return NONE;
	}

```

### 在BaseDao中扩展一个通用查询方法

![](../../../../.gitbook/assets/image%20%2844%29.png)

### StaffService中扩展方法，查询未删除的取派员

![](../../../../.gitbook/assets/image%20%28106%29.png)

## 使用datagrid展示分区数据

![](../../../../.gitbook/assets/image%20%2894%29.png)

### 在SubareaAction中提供listajax方法，查询所有未关联到定区的分区，返回json

![](../../../../.gitbook/assets/image%20%2872%29.png)

### 在SubareaService中扩展方法，查询未关联到定区的分区

![](../../../../.gitbook/assets/image%20%283%29.png)

## 保存分区

### 前台页面问题

![](../../../../.gitbook/assets/image%20%2866%29.png)

![](../../../../.gitbook/assets/image%20%28107%29.png)

{% hint style="info" %}
问题一：提交的表单中存在多个id参数

解决方案：将datagrid的filed由id改为subareaid
{% endhint %}

![](../../../../.gitbook/assets/image%20%28124%29.png)

{% hint style="info" %}
问题二：提交的表单中subareaid参数的值为null

解决方案：在分区类中提供getSubareaid方法
{% endhint %}

![](../../../../.gitbook/assets/image%20%2885%29.png)

![](../../../../.gitbook/assets/image%20%2839%29.png)

### 服务器端实现

![](../../../../.gitbook/assets/image%20%2819%29.png)

![](../../../../.gitbook/assets/image%20%2897%29.png)

{% hint style="info" %}
上图代码中，并没有保存Subarea对象的操作数据也得到了保存。这是因为得到的subarea对象是一个Hibernate的持久状态。此时该对象的改变在事务提交后也会更新到数据库中。
{% endhint %}

