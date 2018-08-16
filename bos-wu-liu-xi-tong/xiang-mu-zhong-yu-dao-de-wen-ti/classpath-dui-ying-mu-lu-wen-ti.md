# classpath对应目录问题

首先 classpath是指 WEB-INF文件夹下的classes目录   
  
解释classes含义：   
1.存放各种资源配置文件 eg.init.properties log4j.properties struts.xml   
2.存放模板文件 eg.actionerror.ftl   
3.存放class文件 对应的是项目开发时的src目录编译文件   
总结：这是一个定位资源的入口   
  
如果你知道开发过程中有这么一句话：惯例大于配置 那么也许你会改变你的想法   
  
对于第二个问题   
这个涉及的是lib和classes下文件访问优先级的问题: lib&gt;classes   
对于性能的影响应该不在这个范畴   
  
classpath 和 classpath\* 区别：   
classpath：只会到你的class路径中查找找文件;   
classpath\*：不仅包含class路径，还包括jar文件中\(class路径\)进行查找.   


