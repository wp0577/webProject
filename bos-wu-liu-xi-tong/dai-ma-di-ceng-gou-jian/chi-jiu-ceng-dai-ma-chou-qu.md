# 持久层代码抽取

![](../../.gitbook/assets/image%20%28220%29.png)

{% code-tabs %}
{% code-tabs-item title="IBaseDao" %}
```text
package com.wp.dao;

import java.io.Serializable;
import java.util.List;

public interface IBaseDao<T> {
    public void save(T t);
    public void update(T t);
    public void delete(T t);
    public List<T> getAll();
    public T getById(Serializable id);
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="IBaseDaoImp" %}
```text
package com.wp.dao.imp;

import com.wp.dao.IBaseDao;
import org.hibernate.SessionFactory;
import org.springframework.orm.hibernate5.support.HibernateDaoSupport;

import javax.annotation.Resource;
import java.io.Serializable;
import java.lang.reflect.ParameterizedType;
import java.lang.reflect.Type;
import java.util.List;

public class IBaseDaoImp<T> extends HibernateDaoSupport implements IBaseDao<T>  {

    private Class entityClass;
	//在父类（BaseDaoImpl）的构造方法中动态获得entityClass
    public IBaseDaoImp() {
        ParameterizedType parameterizedType = (ParameterizedType) this.getClass().getGenericSuperclass();
        Type[] actualTypeArguments = parameterizedType.getActualTypeArguments();
        entityClass = (Class<T>) actualTypeArguments[0];
    }
    //因为只通过注解的方式的话，我们不像以前一样在xml为每个dao注入sessionfactory
    //所以我们只能自己注入
    @Resource
    public void setMySessionFactory(SessionFactory sessionFactory) {
        super.setSessionFactory(sessionFactory);
    }

    @Override
    public void save(T t) {
        getHibernateTemplate().save(t);
    }

    @Override
    public void update(T t) {
        getHibernateTemplate().update(t);
    }

    @Override
    public void delete(T t) {
        getHibernateTemplate().delete(t);
    }

    @Override
    public List<T> getAll() {
        String hql = "from" + entityClass.getSimpleName();;
        return (List<T>) getHibernateTemplate().find(hql);
    }

    @Override
    public T getById(Serializable id) {
        return (T) getHibernateTemplate().get(entityClass, id);
    }
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

