# 面试问题

### 1.1. 网站并发数：

1000-2000左右并发。

### 1.2. 人员配置

产品经理：3人，确定需求以及给出产品原型图。

项目经理：1人，项目管理。

前端团队：5人，根据产品经理给出的原型制作静态页面。

后端团队：20人，实现产品功能。

测试团队：5人，测试所有的功能。

运维团队：3人，项目的发布以及维护。

### 1.3. 开发周期

采用迭代开发的方式进行，一般一次迭代的周期为一个月左右。

### 1.4. Sku

最小库存量单位。

Sku==商品id

### 1.5. 电商活动倒计时方案：

1、确定一个基准时间。可以使用一个sql语句从数据库中取出一个当前时间。SELECT NOW\(\)；

2、活动开始的时间是固定的。

3、使用活动开始时间-基准时间可以计算出一个秒为单位的数值。

4、在redis中设置一个key（活动开始标识）。设置key的过期时间为第三步计算出来的时间。

5、展示页面的时候取出key的有效时间。Ttl命令。使用js倒计时。

6、一旦活动开始的key失效，说明活动开始。

7、需要在活动的逻辑中，先判断活动是否开始。

### 1.6. 秒杀方案：

1、把商品的数量放到redis中。

2、秒杀时使用decr命令对商品数量减一。如果不是负数说明抢到。

3、一旦返回数值变为0说明商品已售完。

