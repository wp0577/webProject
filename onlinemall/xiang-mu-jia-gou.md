# 项目架构

### 1.1. 功能列表

![](../.gitbook/assets/image%20%2884%29.png)

后台管理系统：管理商品、订单、类目、商品规格属性、用户管理以及内容发布等功能。

前台系统：用户可以在前台系统中进行注册、登录、浏览商品、首页、下单等操作。

会员系统：用户可以在该系统中查询已下的订单、收藏的商品、我的优惠券、团购等信息。

订单系统：提供下单、查询订单、修改订单状态、定时处理订单。

搜索系统：提供商品的搜索功能。

单点登录系统：为多个系统之间提供用户登录凭证以及查询登录用户的信息。  


### 1.2. 系统架构

#### 1.2.1.                  传统架构

![](../.gitbook/assets/image%20%28210%29.png)

#### 1.2.2.                  1000并发

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image003.png)![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.png)  

![](../.gitbook/assets/image%20%2881%29.png)

需要20台服务器做tomcat集群。当tomcat集群中节点数量增加，服务能力先增加后下降。

所以集群中节点数量不能太多，一般也就5个左右。  


#### 1.2.3.                  10000并发

需要按照功能点把系统拆分，拆分成独立的功能。单独为某一个节点添加服务器。需要系统之间配合才能完成整个业务逻辑。叫做分布式。

![](../.gitbook/assets/image%20%28150%29.png)

分布式架构：多个子系统相互协作才能完成业务流程。系统之间需要进行通信。

集群：同一个工程部署到多台服务器上。

分布式架构：

把系统按照模块拆分成多个子系统。

优点：

1、把模块拆分，使用接口通信，降低模块之间的耦合度。

2、把项目拆分成若干个子项目，不同的团队负责不同的子项目。

3、增加功能时只需要再增加一个子项目，调用其他系统的接口就可以。

4、可以灵活的进行分布式部署。

缺点：

1、系统之间交互需要使用远程通信，接口开发增加工作量。

2、各个模块有一些通用的业务逻辑无法共用。

#### 1.2.4.                  基于soa的架构

SOA：Service Oriented Architecture面向服务的架构。也就是把工程拆分成服务层、表现层两个工程。服务层中包含业务逻辑，只需要对外提供服务即可。表现层只需要处理和页面的交互，业务逻辑都是调用服务层的服务来实现。

![](../.gitbook/assets/image%20%28199%29.png)

#### 1.2.5.                  宜立方商城系统架构

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image009.png)

![](../.gitbook/assets/image%20%2883%29.png)

