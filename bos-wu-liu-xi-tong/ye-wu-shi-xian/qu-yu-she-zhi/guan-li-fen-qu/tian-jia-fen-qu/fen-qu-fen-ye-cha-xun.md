# 分区分页查询

## 分区分页查询（没有过滤条件）

![](../../../../../.gitbook/assets/image%20%28220%29.png)

![](../../../../../.gitbook/assets/image%20%28152%29.png)

{% hint style="info" %}
因为懒加载，在string to json的环节中会出现region并未实例化而是代理对象，而string2json会同样将代理对象转化，而产生死循环。
{% endhint %}

![](../../../../../.gitbook/assets/image%20%28216%29.png)

## 分区分页查询（有过滤条件）

![](../../../../../.gitbook/assets/image%20%28157%29.png)

datagrid提供的方法：用于重新发送ajax请求，并且可以提交参数

![](../../../../../.gitbook/assets/image%20%28191%29.png)

{% hint style="info" %}
用这种方法发送ajax请求而不是重新提交表单的好处是，发送的请求数据会被页面吧保存下来，这样分页的时候不会丢失掉关键字查询。

而且通过load返回的数据会直接在datagrid中显示 而不是刷新。
{% endhint %}

### Action方法修改

```text
/**
	 * 分页查询
	 */
	public String pageQuery(){
		DetachedCriteria dc = pageBean.getDetachedCriteria();
		//动态添加过滤条件
		String addresskey = model.getAddresskey();
		if(StringUtils.isNotBlank(addresskey)){
			//添加过滤条件，根据地址关键字模糊查询
			dc.add(Restrictions.like("addresskey", "%"+addresskey+"%"));
		}
		
		Region region = model.getRegion();
		if(region != null){
			String province = region.getProvince();
			String city = region.getCity();
			String district = region.getDistrict();
			dc.createAlias("region", "r");
			if(StringUtils.isNotBlank(province)){
				//添加过滤条件，根据省份模糊查询-----多表关联查询，使用别名方式实现
				//参数一：分区对象中关联的区域对象属性名称
				//参数二：别名，可以任意
				dc.add(Restrictions.like("r.province", "%"+province+"%"));
			}
			if(StringUtils.isNotBlank(city)){
				//添加过滤条件，根据市模糊查询-----多表关联查询，使用别名方式实现
				//参数一：分区对象中关联的区域对象属性名称
				//参数二：别名，可以任意
				dc.add(Restrictions.like("r.city", "%"+city+"%"));
			}
			if(StringUtils.isNotBlank(district)){
				//添加过滤条件，根据区模糊查询-----多表关联查询，使用别名方式实现
				//参数一：分区对象中关联的区域对象属性名称
				//参数二：别名，可以任意
				dc.add(Restrictions.like("r.district", "%"+district+"%"));
			}
		}
		subareaService.pageQuery(pageBean);
		this.java2Json(pageBean, new String[]{"currentPage","detachedCriteria","pageSize",
						"decidedzone","subareas"});
		return NONE;
	}

```

**因为通过dc的join方法hibernate封装回来的格式会改变，region中又含有了subarea，所以我们需要改回subarea（region）的格式。**

![](../../../../../.gitbook/assets/image%20%28171%29.png)

