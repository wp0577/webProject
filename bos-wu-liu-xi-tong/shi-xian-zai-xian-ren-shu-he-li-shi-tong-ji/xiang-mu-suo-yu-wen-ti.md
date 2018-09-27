# 项目所遇问题

## 远程tomcat运行项目报错

51错误是因为jdk版本问题

## Response乱码的解决方法

返回中文前一定要做如下处理：

```text
protected void doPost(HttpServletRequest request, HttpServletResponse response)      throws ServletException, IOException {	request.setCharacterEncoding("UTF-8");	response.setCharacterEncoding("UTF-8");	response.setContentType("text/html; charset=UTF-8");	PrintWriter out = response.getWriter();	out.print("收到了");}
```

进入方法后一定要优先设定request和response对象的编码格式out对象一定要在request和response设定完编码个时候再获取  


## idea将项目导出为war包

首先点击这里进入项目的配置页面

![](../../.gitbook/assets/image%20%2833%29.png)

  
在Artifacts栏里点击绿色加号，选择Web Applicant:Archive

![](../../.gitbook/assets/image%20%28214%29.png)

  
设置好名称和输出路径。Build on make选项可选可不选。如果选择了，那么每次在运行项目时都会生成war包。如果不勾选则可以在后续的步骤中手动生成war包。如果下面显示.MF file not found in Accept.war，那么要继续进行配置。很多教程上都到了这一步就结束了，说“哎呀你们运行项目就可以去设置好的路径下找war包啦”。

![](../../.gitbook/assets/image%20%28116%29.png)

  
点击绿色加号选择Directory Content，选择你当前项目的WebRoot目录，之后保存就可以啦。![](https://img-blog.csdn.net/20161119171221283)![](https://img-blog.csdn.net/20161119171227781)  
如果前面勾选了Build on make选项，可以在运行项目时生成war包。如果没有勾选，可以通过Build--&gt;Build Artifacts来生成war包。

![](../../.gitbook/assets/image%20%2894%29.png)

## scp使用方法

#### 1、获取远程服务器上的文件

> _scp -P 2222 root@www.vpser.net:/root/lnmp0.4.tar.gz /home/lnmp0.4.tar.gz_

上端口大写P 为参数，2222 表示更改SSH端口后的端口，如果没有更改SSH端口可以不用添加该参数。 root@www.vpser.net 表示使用root用户登录远程服务器www.vpser.net，:/root/lnmp0.4.tar.gz 表示远程服务器上的文件，最后面的/home/lnmp0.4.tar.gz表示保存在本地上的路径和文件名。还可能会用到p参数保持目录文件的权限访问时间等。

#### 2、获取远程服务器上的目录

> _scp -P 2222 -r root@www.vpser.net:/root/lnmp0.4/ /home/lnmp0.4/_

上端口大写P 为参数，2222 表示更改SSH端口后的端口，如果没有更改SSH端口可以不用添加该参数。-r 参数表示递归复制（即复制该目录下面的文件和目录）；root@www.vpser.net 表示使用root用户登录远程服务器www.vpser.net，:/root/lnmp0.4/ 表示远程服务器上的目录，最后面的/home/lnmp0.4/表示保存在本地上的路径。

#### 3、将本地文件上传到服务器上

> _scp -P 2222 /home/lnmp0.4.tar.gz root@www.vpser.net:/root/lnmp0.4.tar.gz_

上端口大写P 为参数，2222 表示更改SSH端口后的端口，如果没有更改SSH端口可以不用添加该参数。 /home/lnmp0.4.tar.gz表示本地上准备上传文件的路径和文件名。root@www.vpser.net 表示使用root用户登录远程服务器www.vpser.net，:/root/lnmp0.4.tar.gz 表示保存在远程服务器上目录和文件名。

#### 4、将本地目录上传到服务器上

> _scp -P 2222 -r /home/lnmp0.4/ root@www.vpser.net:/root/lnmp0.4/_

上 端口大写P 为参数，2222 表示更改SSH端口后的端口，如果没有更改SSH端口可以不用添加该参数。-r 参数表示递归复制（即复制该目录下面的文件和目录）；/home/lnmp0.4/表示准备要上传的目录，root@www.vpser.net 表示使用root用户登录远程服务器www.vpser.net，:/root/lnmp0.4/ 表示保存在远程服务器上的目录位置。

#### 5、可能有用的几个参数 :

-v 和大多数 linux 命令中的 -v 意思一样 , 用来显示进度 . 可以用来查看连接 , 认证 , 或是配置错误 .

-C 使能压缩选项 .

-4 强行使用 IPV4 地址 .

-6 强行使用 IPV6 地址 .

## [JQ和Js获取span标签的内容](https://www.cnblogs.com/anniey/p/6439021.html)

#### html:

```text
1 <span id="content">‘我是span标签的内容’</span>
```

#### javascript获取:

```text
1 var cont=document.getElementById("content");
2 console.log('innerText cont= '+ cont.innerText); 
3 console.log('innerHtml cont= '+ cont.innerHTML); 
4 //以上两条都能输出span标签的值‘我是span标签的内容’；
```

#### jquery获取:

```text
1 var cont=$("#content");
2 console.log(cont.val()); //输出  (无值);
3 console.log(cont.text()); //输出 ‘我是span标签的内容’;
4 console.log(cont.html()); //输出 ‘我是span标签的内容’;
```

#### 小知识：

**\*\*\***在JS中使用innerHTML时希望自己注意，不要写成了cont.innerHtml ，这样输出结果就是 undefined ；  
**\*\***在表单中习惯性的选用val\(\)来获取值，但是在span标签继续想用val\(\)方法的时候打印空值；  
**\***无奈想了一下，span这个容器需要用text\(\)来获取的呀，我泪奔了，逃走~~·

