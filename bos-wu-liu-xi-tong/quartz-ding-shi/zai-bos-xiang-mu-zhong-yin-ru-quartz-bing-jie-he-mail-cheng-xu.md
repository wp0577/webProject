# 在BOS项目中引入quartz并结合mail程序

## 第一步：在pom.xml中引入quartz和JavaMail的依赖

![](../../.gitbook/assets/image%20%28140%29.png)

## 第二步：提供一个作业类，用于为系统管理员发送邮件

```java
package com.wp.service.mailJobs;

import java.util.List;
import java.util.Properties;

import javax.annotation.Resource;
import javax.mail.Authenticator;
import javax.mail.Message.RecipientType;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

import com.wp.dao.IWorkBillDao;
import com.wp.domain.Workbill;

/**
 * 发送邮件的作业
 * @author wp
 *
 */
public class MailJob {

	@Resource
	private IWorkBillDao workBillDao;

	private String username;
	private String password;
	private String smtpServer;

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	public void execute() {
		System.out.println("要发邮件了。。。");
		try {
			//查询工单类型为新单的所有工单
			List<Workbill> list = workBillDao.getAll();
			if(null != list && list.size() > 0){
				final Properties mailProps = new Properties();
				mailProps.put("mail.smtp.host", this.getSmtpServer());
				mailProps.put("mail.smtp.auth", "true");
				mailProps.put("mail.username", this.getUsername());
				mailProps.put("mail.password", this.getPassword());
				mailProps.put("mail.smtp.starttls.enable", "true");
				mailProps.put("mail.smtp.port", "465");
				mailProps.put("mail.smtp.socketFactory.class", "javax.net.ssl.SSLSocketFactory");
				// 构建授权信息，用于进行SMTP进行身份验证
				Authenticator authenticator = new Authenticator() {
					protected PasswordAuthentication getPasswordAuthentication() {
						// 用户名、密码
						String userName = mailProps.getProperty("mail.username");
						String password = mailProps.getProperty("mail.password");
						return new PasswordAuthentication(userName, password);
					}
				};
				// 使用环境属性和授权信息，创建邮件会话
				Session mailSession = Session.getInstance(mailProps, authenticator);
				for(Workbill workbill : list){
					// 创建邮件消息
					MimeMessage message = new MimeMessage(mailSession);
					// 设置发件人
					InternetAddress from = new InternetAddress(mailProps.getProperty("mail.username"));
					message.setFrom(from);
					// 设置收件人
					InternetAddress to = new InternetAddress("wu.pa@husky.neu.edu");
					message.setRecipient(RecipientType.TO, to);
					// 设置邮件标题
					message.setSubject("系统邮件：新单通知");
					// 设置邮件的内容体
					message.setContent(workbill.toString(), "text/html;charset=UTF-8");
					// 发送邮件
					Transport.send(message);
				}
			}
		} catch (Exception ex) {
			ex.printStackTrace();
		}
	}

	public String getSmtpServer() {
		return smtpServer;
	}

	public void setSmtpServer(String smtpServer) {
		this.smtpServer = smtpServer;
	}
}

```

## 第三步：在spring配置文件中配置

```text
<!-- 注册自定义作业类 -->
	<bean id="myJob" class="com.itheima.jobs.MailJob">
		<property name="username" value="itcast_server@126.com"/>
		<property name="password" value="147963qP"/>
		<property name="smtpServer" value="smtp.126.com"/>
	</bean>
	
	<!-- 配置JobDetail -->
	<bean id="jobDetail" class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">
		<!-- 注入目标对象 -->
		<property name="targetObject" ref="myJob"/>
		<!-- 注入目标方法 -->
		<property name="targetMethod" value="execute"/>
	</bean>
	
	<!-- 配置触发器 -->
	<bean id="myTrigger" class="org.springframework.scheduling.quartz.CronTriggerFactoryBean">
		<!-- 注入任务详情对象 -->
		<property name="jobDetail" ref="jobDetail"/>
		<!-- 注入cron表达式，通过这个表达式指定触发的时间点 -->
		<property name="cronExpression">
			<value>0/5 * * * * ?</value>
		</property>
	</bean>
	
	<!-- 配置调度工厂 -->
	<bean id="schedulerFactoryBean" class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
		<!-- 注入触发器 -->
		<property name="triggers">
			<list>
				<ref bean="myTrigger"/>
			</list>
		</property>
	</bean>

```

