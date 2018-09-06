# java中servletContextListener、httpSessionListener和servletRequestListener使用整理



在java web应用中，listener监听器似乎是必不可少的，常常用来监听servletContext、httpSession、servletRequest等域对象的创建、销毁以及属性的变化等等，可以在这些事件动作前后进行一定的逻辑处理。   
比较常用的应用场景是利用监听器来初始化一些数据、统计在线人数、统计web应用浏览量等等。   
这里所说的监听器实际上是servlet规范中定义的一种特殊类，需要实现特定的接口。   
而我暂时先说其中三个用来监听域对象的，分别是servletContextListener、httpSessionListener、servletRequestListener。   
这三个接口写法上实际是差不多的，都有两个分别代表了该域对象创建时调用和销毁时调用的方法，据我的理解，这三个对象最大的区别应该就是作用域不一样。   
servletContext在整个应用启动到结束中生效，启动系统时创建这个对象，整个过程中这个对象是唯一的。   
httpSession则是在一个session会话中生效，在一个session被创建直到失效的过程中都起作用，不过一个启动的应用中httpSession对象可以有多个，比如同一台电脑两个浏览器访问，就会创建两个httpSession对象。   
而servletRequest是在一个request请求被创建和销毁的过程中生效，每发起一次请求就会创建一个新的servletRequest对象，比如刷新浏览器页面、点击应用的内链等等。   
这三个监听器的写法基本如下伪代码所示：   
首先创建一个监听器类实现相应的接口及方法：

```text
package packageName;
public class ListenerName implements *Listener {

    @Override
    public void 对象创建时被调用的方法(相应的事件 arg0) {

    }

    @Override
    public void 对象销毁时被调用的方法(相应的事件 arg0) {

    }
}
```

然后在web.xml中注册：

```text
 <listener>
    <listener-class>packageName.ListenerName</listener-class>
  </listener>123
```

到这里，基本上这个监听器在启动web服务器以后就可以正常跑了，只是有时候我们还会在注册监听器的时候配置一些其他的。   
比如配置servletContextListener的时候，可能会加上context-param参数：

```text
<context-param>
     <param-name>paramName</param-name>
     <param-value>paramValue</param-value>
</context-param>1234
```

配置httpSessionListener的时候，可能会加上session超时：

```text
<session-config>
     <session-timeout>1</session-timeout>
</session-config>123
```

我感觉经过这样一整理后，就会发现起始监听器写起来还是很简单的，于是便模拟简单的实现了在线用户统计和应用访问量，基本代码如下：   
首先是实现了servletContext

```text
package webTest;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.Date;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

public class ListenerTest1 implements ServletContextListener {

    @Override
    public void contextDestroyed(ServletContextEvent arg0) {

        System.out.println("contextDestroyed" + "," + new Date());
        Object count = arg0.getServletContext().getAttribute("count");
        File file = new File("count.txt");
        try {
            FileOutputStream fileOutputStream = new FileOutputStream(file);
            OutputStreamWriter outputStreamWriter = new OutputStreamWriter(fileOutputStream, "utf-8");
            BufferedWriter bufferedWriter = new BufferedWriter(outputStreamWriter);
            bufferedWriter.write(count.toString());
            bufferedWriter.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void contextInitialized(ServletContextEvent arg0) {
        System.out.println("contextInitialized" + "," + new Date());
        File file = new File("count.txt");
        if (file.exists()) {
            try {
                FileInputStream fileInputStream = new FileInputStream(file);
                InputStreamReader inputStreamReader = new InputStreamReader(fileInputStream, "utf-8");
                BufferedReader bReader = new BufferedReader(inputStreamReader);
                String count = bReader.readLine();
                System.out.println("历史访问次数：" + count);
                arg0.getServletContext().setAttribute("count", count);
                bReader.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

这里我把访问次数存在一个txt文件中，以便于持久化保存，当项目启动的时候，也就是创建servletContext对象的时候调用加载方法，从txt文件中读取历史访问量，然后使用setAttribute方法把这个数据存入到内存中。   
之后当应用被关闭，servletContext对象被销毁的时候把内存中新的访问量数据覆盖写入到txt文件，以便于下次启动应用后继续读取之前的访问量。   
然后使用servletRequestListener来实现web浏览量的变化，当然了，这里只是简单的实现，如果是要实现那种同一个用户刷新页面不增加浏览量的功能，还需要做更多的处理。

```text
package webTest;
import java.util.Date;
import javax.servlet.ServletRequestEvent;
import javax.servlet.ServletRequestListener;

public class ListenerTest3 implements ServletRequestListener {

    @Override
    public void requestDestroyed(ServletRequestEvent arg0) {
        System.out.println("requestDestroyed" + "," + new Date());
        System.out.println("当前访问次数：" + arg0.getServletContext().getAttribute("count"));
    }

    @Override
    public void requestInitialized(ServletRequestEvent arg0) {
        System.out.println("requestInitialized" + "," + new Date());
        Object count = arg0.getServletContext().getAttribute("count");
        Integer cInteger = 0;
        if (count != null) {
            cInteger = Integer.valueOf(count.toString());
        }
        System.out.println("历史访问次数：：" + count);
        cInteger++;
        arg0.getServletContext().setAttribute("count", cInteger);
    }

}123456789101112131415161718192021222324252627
```

这里同样是两个方法，在servletRequest对象被建立的时候调用初始化方法，从内存中读取servletContext对象的count属性，而后输出历史访问量。   
同时在此基础上加一重新设置servletContext对象的count属性的内容，当servletRequest对象被销毁的时候调用销毁时的方法打印出当前浏览量，这样就简单的实现了web浏览的量的累加计数。   
然后就是利用httpSessionListener来实现在线人数的统计：

```text
package webTest;
import java.util.Date;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

public class ListenerTest2 implements HttpSessionListener {

    @Override
    public void sessionCreated(HttpSessionEvent arg0) {
        System.out.println("sessionCreated" + "," + new Date());
        Object lineCount = arg0.getSession().getServletContext().getAttribute("lineCount");
        Integer count = 0;
        if (lineCount == null) {
            lineCount = "0";
        }
        count = Integer.valueOf(lineCount.toString());
        count++;
        System.out.println("新上线一人，历史在线人数：" + lineCount + "个,当前在线人数有： " + count + " 个");
        arg0.getSession().getServletContext().setAttribute("lineCount", count);
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent arg0) {
        System.out.println("sessionDestroyed" + "," + new Date());
        Object lineCount = arg0.getSession().getServletContext().getAttribute("lineCount");
        Integer count = Integer.valueOf(lineCount.toString());
        count--;
        System.out.println("一人下线，历史在线人数：" + lineCount + "个，当前在线人数: " + count + " 个");
        arg0.getSession().getServletContext().setAttribute("lineCount", count);
    }

}
```

