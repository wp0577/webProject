# Java设计模式之代理模式\(Proxy\)

参考[http://blog.csdn.net/jianghuxiaoxiami/article/details/3403924](http://blog.csdn.net/jianghuxiaoxiami/article/details/3403924)



类似于在Spring框架中，为service添加了@Transactional注解，这就是为service生成代理对象。  


**1.代理模式**

代理模式的作用是：为其他对象提供一种代理以控制对这个对象的访问。在某些情况下，一个客户不想或者不能直接引用另一个对象，而代理对象可以在客户端和目标对象之间起到中介的作用。

代理模式一般涉及到的角色有：

抽象角色：声明真实对象和代理对象的共同接口；

代理角色：代理对象角色内部含有对真实对象的引用，从而可以操作真实对象，同时代理对象提供与真实对象相同的接口以便在任何时刻都能代替真实对象。同时，代理对象可以在执行真实对象操作时，附加其他的操作，相当于对真实对象进行封装。

真实角色：代理角色所代表的真实对象，是我们最终要引用的对象。

以下是《Java与模式》中的示例为例：

**1.1 静态代理**  


```text
package staticproxy;
 
/**
 * 抽象角色
 */
public abstract class Subject {
	public abstract void request();
}




package staticproxy;
 
/**
 * 真实的角色
 */
public class RealSubject extends Subject {
 
	@Override
	public void request() {
		// TODO Auto-generated method stub
 
	}
 
}




package staticproxy;
 
/**
 * 静态代理，对具体真实对象直接引用
 * 代理角色，代理角色需要有对真实角色的引用，
 * 代理做真实角色想做的事情
 */
public class ProxySubject extends Subject {
	
	private RealSubject realSubject = null;
	
	/**
	 * 除了代理真实角色做该做的事情，代理角色也可以提供附加操作，
	 * 如：preRequest()和postRequest()
	 */
	@Override
	public void request() {
		preRequest();  //真实角色操作前的附加操作
		
		if(realSubject == null){
			realSubject =  new RealSubject();
		}
		realSubject.request();
		
		postRequest();  //真实角色操作后的附加操作
	}
 
	/**
	 *	真实角色操作前的附加操作
	 */
	private void postRequest() {
		// TODO Auto-generated method stub
		
	}
 
	/**
	 *	真实角色操作后的附加操作
	 */
	private void preRequest() {
		// TODO Auto-generated method stub
		
	}
 
}




package staticproxy;
 
/**
 *	客户端调用 
 */
public class Main {
	public static void main(String[] args) {
		Subject subject = new ProxySubject();
		subject.request();  //代理者代替真实者做事情
	}
}



```

以卖房\(Subject的request方法\)为例，卖房者\(RealSubject\)想卖房，但是不想搞那么多麻烦事，不想天天招买房的骚扰，就委托中介\(ProxySubject\)去帮忙卖房，中介帮忙卖房，当然不会简单的卖房，可能还需要收取中介费\(preRequest\(\)和postRequest\(\)方法\)等等附加收费，最终买房者\(客户端Main\)买房，找到的是中介，不会与房主接触，与房主接触的是中介，买房者最终跟中介买房，完成操作。

这个是静态代理。真实角色必须是事先已经存在的，并将其作为代理对象的内部属性。但是实际使用时，一个真实角色必须对应一个代理角色，如果大量使用会导致类的急剧膨胀；此外，如果事先并不知道真实角色，该如何使用代理呢？这个问题可以通过Java的动态代理类来解决。

动态代理，就相当于代理者不仅仅只是代理一个真实对象，也可以代理很多对象，而且对象是动态指定的。

**1.2 动态代理**

Java动态代理类位于Java.lang.reflect包下，一般主要涉及到以下两个类：

\(1\). Interface InvocationHandler：该接口中仅定义了一个方法Object：invoke\(Objectobj,Method method, Object\[\] args\)。在实际使用时，第一个参数obj一般是指代理类，method是被代理的方法，如上例中的request\(\)，args为该方法的参数数组。这个抽象方法在代理类中动态实现。

\(2\).Proxy：该类即为动态代理类，作用类似于上例中的ProxySubject，其中主要包含以下内容：

Protected Proxy\(InvocationHandler h\)：构造函数，估计用于给内部的h赋值。

Static Class getProxyClass \(ClassLoaderloader, Class\[\] interfaces\)：获得一个代理类，其中loader是类装载器，interfaces是真实类所拥有的全部接口的数组。

Static Object newProxyInstance\(ClassLoaderloader, Class\[\] interfaces, InvocationHandler h\)：返回代理类的一个实例，返回后的代理类可以当作被代理类使用\(可使用被代理类的在Subject接口中声明过的方法\)。

所谓Dynamic Proxy是这样一种class：它是在运行时生成的class，在生成它时你必须提供一组interface给它，然后该class就宣称它实现了这些interface。你当然可以把该class的实例当作这些interface中的任何一个来用。当然啦，这个DynamicProxy其实就是一个Proxy，它不会替你作实质性的工作，在生成它的实例时你必须提供一个handler，由它接管实际的工作。\(参见文献3\)

在使用动态代理类时，我们必须实现InvocationHandler接口

```text
package dynamicproxy;
 
/**
 * 抽象角色
 * 这里应改为接口
 */
public interface Subject {
	void request();
}


package dynamicproxy;
 
/**
 * 真实的角色
 * 实现接口
 */
public class RealSubject implements Subject {
 
	@Override
	public void request() {
		// TODO Auto-generated method stub
 
	}
 
}




package dynamicproxy;
 
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
 
/**
 * 动态代理， 它是在运行时生成的class，在生成它时你必须提供一组interface给它， 然后该class就宣称它实现了这些interface。
 * 你当然可以把该class的实例当作这些interface中的任何一个来用。 当然啦，这个Dynamic
 * Proxy其实就是一个Proxy，它不会替你作实质性的工作， 在生成它的实例时你必须提供一个handler，由它接管实际的工作。
 */
public class DynamicSubject implements InvocationHandler {
 
	private Object sub; // 真实对象的引用
 
	public DynamicSubject(Object sub) {
		this.sub = sub;
	}
 
	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		System.out.println("before calling " + method); 
        method.invoke(sub,args); 
        System.out.println("after calling " + method); 
        return null; 
	}
 
}






package dynamicproxy;
 
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Proxy;
 
public class Main {
	public static void main(String[] args) throws Throwable {
		RealSubject rs = new RealSubject();
		InvocationHandler handler = new DynamicSubject(rs);
		Class cls = rs.getClass();
		//以下是分解步骤
		/*
		Class c = Proxy.getProxyClass(cls.getClassLoader(), cls.getInterfaces());
		Constructor ct = c.getConstructor(new Class[]{InvocationHandler.class});
		Subject subject =(Subject) ct.newInstance(new Object[]{handler});
		*/
		
		//以下是一次性生成
		Subject subject = (Subject)Proxy.newProxyInstance(cls.getClassLoader(),cls.getInterfaces(), handler);
		subject.request();
	}
}

```

通过这种方式，被代理的对象\(RealSubject\)可以在运行时动态改变，需要控制的接口\(Subject接口\)可以在运行时改变，控制的方式\(DynamicSubject类\)也可以动态改变，从而实现了非常灵活的动态代理关系

**1.3.代理模式使用原因和应用方面**

（1）授权机制不同级别的用户对同一对象拥有不同的访问权利,如Jive论坛系统中,就使用Proxy进行授权机制控制,访问论坛有两种人:注册用户和游客\(未注册用户\),Jive中就通过类似ForumProxy这样的代理来控制这两种用户对论坛的访问权限.

（2）某个客户端不能直接操作到某个对象,但又必须和那个对象有所互动.

     举例两个具体情况:

     如果那个对象是一个是很大的图片,需要花费很长时间才能显示出来,那么当这个图片包含在文档中时,使用编辑器或浏览器打开这个文档,打开文档必须很迅速,不能等待大图片处理完成,这时需要做个图片Proxy来代替真正的图片.

     如果那个对象在Internet的某个远端服务器上,直接操作这个对象因为网络速度原因可能比较慢,那我们可以先用Proxy来代替那个对象.

     总之原则是,对于开销很大的对象,只有在使用它时才创建,这个原则可以为我们节省很多宝贵的Java内存. 所以,有些人认为Java耗费资源内存,我以为这和程序编制思路也有一定的关系.

（3）现实中，Proxy应用范围很广,现在流行的分布计算方式RMI和Corba等都是Proxy模式的应用

