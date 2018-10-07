# HighCharts使用

## Highcharts简介 

Highcharts 是一个用纯JavaScript编写的一个图表库， 能够很简单便捷的在web网站或是web应用程序添加有交互性的图表，并且免费提供给个人学习、个人网站和非商业用途使用。HighCharts支持的图表类型有曲线图、区域图、柱状图、饼状图、散状点图和综合图表。

### 使用步骤

#### 第一步：在页面中引入相关js文件

```text
	<script src="https://code.highcharts.com/highcharts.js"></script>
	<script src="https://code.highcharts.com/modules/exporting.js"></script>
	<script src="https://code.highcharts.com/modules/export-data.js"></script>
	<script type="text/javascript" src="${pageContext.request.contextPath }/js/jquery-1.8.3.js"></script>
```

#### 第二步：在jsp页面中提供按钮，并提供div窗口，在这个窗口中展示图表

![](../.gitbook/assets/image%20%28261%29.png)

![](../.gitbook/assets/image%20%2812%29.png)

#### 第三步：定义function

```text
function doShowHighcharts(){
		$("#showSubareaWindow").window("open");
		//页面加载完成后，动态创建图表
		$.post("subareaAction_findSubareasGroupByProvince.action",function(data){
			$("#test").highcharts({
				title: {
		            text: '区域分区分布图'
		        },
		        series: [{
		            type: 'pie',
		            name: '区域分区分布图',
		            data: data
		        }]
			});
		});
	}

```

#### 第四步：在服务端Action中提供方法

![](../.gitbook/assets/image%20%28205%29.png)

Dao代码：

```text
@Repository
public class SubareaDaoImpl extends BaseDaoImpl<Subarea> implements ISubareaDao {
	public List<Object> findSubareasGroupByProvince() {
		String hql = "SELECT r.province ,count(*) FROM Subarea s LEFT OUTER JOIN s.region r Group BY r.province";
		return (List<Object>) this.getHibernateTemplate().find(hql);
	}
}

```

![](../.gitbook/assets/image%20%2874%29.png)

