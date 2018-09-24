# 在bos系统中配置shiro

## 第一步：引入shiro框架相关的jar

![](../../../.gitbook/assets/image%20%28151%29.png)

## 第二步：在web.xml中配置spring框架提供的用于整合shiro框架的过滤器

![](../../../.gitbook/assets/image%20%28159%29.png)

启动tomcat服务器，抛出异常：spring工厂中不存在一个名称为“shiroFilter”的bean对象

## 第三步：在spring配置文件中配置bean，id为shiroFilter

![](../../../.gitbook/assets/image%20%28126%29.png)

框架提供的过滤器：

![](../../../.gitbook/assets/image%20%2868%29.png)

## 第四步：配置安全管理器

![](../../../.gitbook/assets/image%20%2854%29.png)

## 第五步：修改UserAction中的login方法，使用shiro提供的方式进行认证操作

![](../../../.gitbook/assets/image%20%2849%29.png)

## 第六步：自定义realm，并注入给安全管理器

```text
public class BOSRealm extends AuthorizingRealm{
	@Autowired
	private IUserDao userDao;
	
	//认证方法
	protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
		System.out.println("realm中的认证方法执行了。。。。");
		UsernamePasswordToken mytoken = (UsernamePasswordToken)token;
		String username = mytoken.getUsername();
		//根据用户名查询数据库中的密码
		User user = userDao.findUserByUserName(username);
		if(user == null){
			//用户名不存在
			return null;
		}
		//如果能查询到，再由框架比对数据库中查询到的密码和页面提交的密码是否一致
		AuthenticationInfo info = new SimpleAuthenticationInfo(user, user.getPassword(), this.getName());
		return info;
	}

	//授权方法
	protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
		// TODO Auto-generated method stub
		return null;
	}


```

![](../../../.gitbook/assets/image%20%2862%29.png)

