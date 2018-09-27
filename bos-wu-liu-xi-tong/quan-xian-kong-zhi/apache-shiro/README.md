# apache shiro

[https://juejin.im/post/5ab1b969f265da239376f1a6](https://juejin.im/post/5ab1b969f265da239376f1a6)

## shiro框架的核心功能： 

认证 授权 会话管理 加密

## shiro框架认证流程

![](../../../.gitbook/assets/image%20%28172%29.png)

Application Code：应用程序代码，由开发人员负责开发的 

Subject：框架提供的接口，代表当前用户对象 

SecurityManager：框架提供的接口，代表安全管理器对象

Realm：可以开发人员编写，框架也提供一些，类似于ＤＡＯ，用于访问权限数据

![](../../../.gitbook/assets/image%20%28100%29.png)

