# 实现方法

## 实现原理

因为当浏览器第一次向服务器发送请求时，服务器会为该client创建一个随机的sessionId并将sessionId通过cookie传给client，在该session结束前（可以通过主动的invalidate\(\)方法，或者设置最长空闲时间来设定session过期），客户端向服务器请求都是通过该相同的sessionID。

所以通过这个相同的session对话监听，可以进行访问人数的统计。

当createSession方法被调用的时候让count++；destroySession调用时count--并且将保存在servletContext中的userList（在request监听器中通过sessionId保存user）中跟该sessionId相等的session删除；

在request中通过对sessionId的对比，将没有SessionId的user保存到userlist中并放到servletContext，而已经存在的直接跳过。

## 实现步骤

### 编写两个监听器

{% code-tabs %}
{% code-tabs-item title="sessionListener" %}
```text
import Domain.LoginInfo;
import util.SessionUtil;

import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;
import java.util.ArrayList;

public class SessionCountListener implements HttpSessionListener {


    //在线用户数
    private int number;
    @Override
    public void sessionCreated(HttpSessionEvent se) {
        System.out.println("进入session————————————————");
/*
        ILoginInfoDao iLoginInfoDao=(ILoginInfoDao) SessionUtil.getObjectFromApplication(se.getSession().getServletContext(), "ILoginInfoDaoImpl");
        List<LoginInfo> all = iLoginInfoDao.getAll();
        List<LoginInfo> userList = (List<LoginInfo>) se.getSession().getServletContext().getAttribute("userList");
*/
        //设置session失效时间，为调试用
        //se.getSession().setMaxInactiveInterval(60*1);
        ArrayList<LoginInfo> userList = (ArrayList<LoginInfo>) se.getSession().getServletContext().getAttribute("userList");
        if(userList==null || SessionUtil.getUserBySessionId(userList, se.getSession().getId())==null) {
            number++;
        }
        //在线用户的数量存储到域对象ServletContext的number中
        se.getSession().getServletContext().setAttribute("number", number);
        System.out.println("number =" +number);
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        //list是存储在域对象ServletContext中，用于记录用户的的日志信息
        ArrayList<LoginInfo> list=
                (ArrayList<LoginInfo>) se.getSession().getServletContext().getAttribute("userList");
        //根据域中的session取得number
        number = Integer.valueOf(se.getSession().getServletContext().getAttribute("number").toString());
        //根据list去删session中的number
        //根据sessionid删除将要推出的用户信息
        if(SessionUtil.remove(list,se.getSession().getId())){
            //if(list!=null || list.size()>0) number=(number<=0)?0:number-1;
            number=(number<=0)?0:number-1;
            se.getSession().getServletContext().setAttribute("number", number);
        }
        se.getSession().getServletContext().setAttribute("userList", list);
        System.out.println("after = "+ number);
    }
}


```
{% endcode-tabs-item %}

{% code-tabs-item title="requestListner" %}
```
package listener;

import Domain.LoginInfo;
import dao.LoginInfoDao;
import util.NetworkUtil;
import util.SessionUtil;

import javax.servlet.ServletRequestEvent;
import javax.servlet.ServletRequestListener;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;
import java.sql.SQLException;
import java.sql.Timestamp;
import java.util.ArrayList;
import java.util.Date;


public class RequestListener implements ServletRequestListener {


    private ArrayList<LoginInfo> userList=new ArrayList<LoginInfo>();  //在线用户Id

    @Override
    public void requestInitialized(ServletRequestEvent sre) {
        System.out.println("进入request ---------------");
        HttpServletRequest request = (HttpServletRequest) sre.getServletRequest();
        String remoteAddr = null;
        LoginInfo u= null;
        try {
            //获得ip地址
            remoteAddr = NetworkUtil.getIpAddress(request);
            System.out.println("ip address = "+remoteAddr);
            //if(remoteAddr.equals("0:0:0:0:0:0:0:1")) return;
            u = LoginInfoDao.getLoginByIp(remoteAddr);
        } catch (Exception e) {
            e.printStackTrace();
        }

        //创建用户list在servlectcontext域对象中
        userList=(ArrayList<LoginInfo>) sre.getServletContext().getAttribute("userList");
        if(userList==null){
            userList=new ArrayList<LoginInfo>();
        }

        //获得用户信息之sessionid
        String id=request.getSession().getId();
        if (SessionUtil.getUserBySessionId(userList, id) == null) {
            System.out.println("number ="+sre.getServletContext().getAttribute("number")+"Ip = "+remoteAddr);
            if (u == null) {
                u = new LoginInfo();
                u.setIpString(request.getRemoteAddr());
                u.setTotalCount(1);
                u.setFirstTimeString(new Timestamp(new Date().getTime()));
                LoginInfoDao.save(u);
            } else {
                //设置最后登陆时间
                u.setLastTimeString(new Timestamp(new Date().getTime()));
                //如果已经存在ip地址，则更新total
                u.setTotalCount(u.getTotalCount() + 1);
                LoginInfoDao.update(u);
            }
            //无论是否存在都存在sessionId作为判重的要求
            u.setSessionIdString(id);
            userList.add(u);
            //iLoginInfoService.save(u);
        }
        sre.getServletContext().setAttribute("userList", userList);
        System.out.println("结束"+id+"request ---------------");
    }

    @Override
    public void requestDestroyed(ServletRequestEvent sre) {

    }



}


```
{% endcode-tabs-item %}
{% endcode-tabs %}

### web.xml配置

```text
 <!--session应该放在request前面-->
    <listener>
        <listener-class>listener.SessionCountListener</listener-class>
    </listener>
    <listener>
        <listener-class>listener.RequestListener</listener-class>
    </listener>
```



