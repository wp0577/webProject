# Junit测试问题

{% embed url="https://blog.csdn.net/sunshinegyan/article/details/80481195" %}

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

