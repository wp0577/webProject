# Junit测试问题

{% embed data="{\"url\":\"https://blog.csdn.net/sunshinegyan/article/details/80481195\",\"type\":\"link\",\"title\":\"Spring Boot单元测试报错java.lang.IllegalStateException: Could not load TestContextBootstrapper \[null\] - CSDN博客\",\"description\":\"1 报错描述java.lang.IllegalStateException: Could not load TestContextBootstrapper \[null\]. Specify @BootstrapWith\'s \'value\' attribute or make the default bootstrapper class available.2分析原因spring-webmvc的版本应...\",\"icon\":{\"type\":\"icon\",\"url\":\"https://csdnimg.cn/public/favicon.ico\",\"aspectRatio\":0}}" %}

```text
import com.wp.service.mailJobs.MailJob;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.annotation.Resource;

/**
 * @program: bos-parent
 * @description:
 * @author: Pan wu
 * @create: 2018-08-31 16:56
 **/

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations={"classpath:applicationContext.xml"})
public class test {

    @Resource
    private MailJob mailJob;

    @Test
    public void test() {
        mailJob.execute();
    }


}

```

