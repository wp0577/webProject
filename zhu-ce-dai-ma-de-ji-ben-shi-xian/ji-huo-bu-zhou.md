# 激活步骤

```text
//发送激活邮件
			String emailMsg = "恭喜您注册成功，请点击下面的连接进行激活账户"
					+ "<a href='http://localhost:8080/HeimaShop/active?activeCode="+activeCode+"'>"
							+ "http://localhost:8080/HeimaShop/active?activeCode="+activeCode+"</a>";
			try {
				MailUtils.sendMail(user.getEmail(), emailMsg);
			} catch (MessagingException e) {
				e.printStackTrace();
			}
```

