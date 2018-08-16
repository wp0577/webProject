# BeanUtil的使用

```java
 Map<String, String[]> parameterMap = request.getParameterMap();
        User user = new User();
        try {
            //自己指定一个类型转换器（将String转成Date）
            ConvertUtils.register(new Converter() {
                @Override
                public Object convert(Class clazz, Object value) {
                    //将string转成date
                    SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
                    Date parse = null;
                    try {
                        parse = format.parse(value.toString());
                    } catch (ParseException e) {
                        e.printStackTrace();
                    }
                    return parse;
                }
            }, Date.class);
            //映射封装
            BeanUtils.populate(user, parameterMap);
        } catch (IllegalAccessException | InvocationTargetException e) {
            e.printStackTrace();
        }
        //将没有的参数自己设置
        user.setUid(CommonUtils.getUUID());
        user.setTelephone(null);
        user.setCode(CommonUtils.getUUID());
        user.setState(0);
        //调用service层代码去处理业务逻辑
```

Apache Common BeanUtil是一个常用的在对象之间复制数据的工具类，著名的web开发框架struts就是依赖于它进行ActionForm的创建。

BeanUtil最常用的类是org.apache.commons.beanutils.BeanUtils。

BeanUtils最常用的方法为：

1. public void copyProperties\(java.lang.Object dest, java.lang.Object orig\)

把orig中的值copy到dest中. 

2. public java.util.Map describe\(java.lang.Object bean\) 把Bean的属性值放入到一个Map里面。 

3. public void populate\(java.lang.Object bean, java.util.Map properties\) 把properties里面的值放入bean中。 

4. public void setProperty\(java.lang.Object bean, java.lang.String name, java.lang.Object value\) 设置Bean对象的名称为name的property的值为value. 

 5. public String getProperty\(java.lang.Object bean, java.lang.String name\) 取得bean对象中名为name的属性的值。 详细的使用方法可以参见官方网站： [http://jakarta.apache.org/commons/beanutils/](http://jakarta.apache.org/commons/beanutils/) Apache Common BeanUtil的常见使用场景。

1. 同类之间不同对象要求进行数据复制。

User user1 = …; 

User user2 = …; 

BeanUtils. copyProperties\(user2,user1\); 

2. 不同类不同对象之间的数据复制。

UserForm userForm = …; User user = …; BeanUtils. copyProperties\(user, userForm\);

相信经常使用struts的人，一定会很熟悉上面的代码。

 这是一个典型把页面的value object数据复制到domain object的例子。 

3. 对象数据和Map之间互相转化。 

User user = …; 

Map userMap = BeanUtils.describe\(user\);

 Map userMap = …; 

User user = …; BeanUtils.populate\(user,userMap\); 

Map可以看成一个动态数据容器，作为VO很适合在不同层之间传播数据，作为PO也可以动态存储字段信息， 合理运用可以减少程序很多修改和维护工作。所以让bean和map之间方便的进行数据填充，非常必要。

