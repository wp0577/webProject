# 区域分页

## 重构分区代码

分页代码与staff相同

可以将重复的分页代码提取到baseAction中

{% code-tabs %}
{% code-tabs-item title="分页代码在子action中" %}
```text
public String list() {
        iRegionService.getPage(pageBean);
        this.String2Json(pageBean, new String[]{"currentPage","detachedCriteria","pageSize"});
        return NONE;
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="BaseAction" %}
```text
protected PageBean pageBean = new PageBean();
    DetachedCriteria detachedCriteria = null;

    //在构造方法中动态获取实体类型，通过反射创建model对象
    public BaseAction() {
        ParameterizedType parameterizedType = (ParameterizedType) this.getClass().getGenericSuperclass();
        Type[] actualTypeArguments = parameterizedType.getActualTypeArguments();
        Class<T> classes = (Class<T>) actualTypeArguments[0];
        detachedCriteria = DetachedCriteria.forClass(classes);
        pageBean.setDetachedCriteria(detachedCriteria);
        try {
            model = classes.newInstance();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }

    }
    
    public void setPage(int page) {
        pageBean.setCurrentPage(page);
    }

    public void setRows(int row) {
        pageBean.setPageSize(row);
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 将String转json的代码和将结果返回到response中

```text
   public void String2Json(PageBean o, String[] excludes) {
        //将page转化成json格式并传回
        //使用Jsonlib工具类
        //使用json-lib将PageBean对象转为json，通过输出流写回页面中
        //JSONObject---将单一对象转为json
        //JSONArray----将数组或者集合对象转为json
        JsonConfig jsonConfig = new JsonConfig();
        //指定哪些属性不需要转json
        //new String[]{"currentPage","detachedCriteria","pageSize"}
        jsonConfig.setExcludes(excludes);
        String jsonObject = JSONObject.fromObject(o,jsonConfig).toString();
        ServletActionContext.getResponse().setContentType("text/json;charset=utf-8");
        try {
            ServletActionContext.getResponse().getWriter().write(jsonObject);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

