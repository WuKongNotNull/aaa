## 定时调度集成-Quartz 

在日常项目运行中，我们总会有需求在某一时间段周期性的执行某个动作。比如每天在某个时间段导出报表，或者每 隔多久统计一次现在在线的用户量等。

**Scheduled 比较 quartz**

在Spring Boot中有Java自带的java.util.Timer类，SpringBoot自带的Scheduled来实现,也有强大的调度器Quartz。 Scheduled 在Spring3.X 引入，默认SpringBoot自带该功能,使用起来也很简单，在启动类级别添加 @EnableScheduling注解即可引入定时任务环境。

但遗憾的是Scheduled 默认不支持分布式环境，这里主要讲解 Quartz 时钟调度框架与Spring Boot 集成。



**pom**

```xml
  <dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-quartz</artifactId>
</dependency>
```



**job类**

```java
public class MyJob implements Job {

    private final Logger logger = LoggerFactory.getLogger(MyJob.class);

    @Resource
    private UserService userService;

    @Override
    public void execute(JobExecutionContext jobExecutionContext) throws JobExecutionException {
        TriggerKey triggerKey =jobExecutionContext.getTrigger().getKey();

        User userById = userService.getUserById(1);
        logger.info("触发器:"+triggerKey.getName()+"-->所属组:"+triggerKey.getGroup()+"->"+userById.toString()+"-->"+"hello Spring Boot Quartz...");


        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        logger.info("触发器:"+triggerKey.getName()+"-->所属组:"+triggerKey.getGroup()+"->"+sdf.format(new Date())+"-->"+"hello Spring Boot Quartz...");
    }
}
```



**配置类**

```java
import com.wukong.job.MyJob;
import org.quartz.*;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class QuartzConfig {

    @Bean
    public JobDetail myJobDetail(){
        return JobBuilder.newJob(MyJob.class).storeDurably().build();
    }


    @Bean
    public Trigger trigger1(){

        SimpleScheduleBuilder simpleScheduleBuilder = SimpleScheduleBuilder.simpleSchedule()
         //每一秒执行一次              
        .withIntervalInSeconds(1)
        //永久重复，一直执行下去              
        .repeatForever();

        return TriggerBuilder.newTrigger()
                            .withIdentity("trigger1","group1")
                            .withSchedule(simpleScheduleBuilder)
                            .forJob(this.myJobDetail()).build();
    }

     // 每两秒触发一次任务    
     @Bean
    public Trigger trigger2(){

        CronScheduleBuilder cronScheduleBuilder=CronScheduleBuilder.cronSchedule("0/5 * * * * ? *");

        return TriggerBuilder.newTrigger()
                .withIdentity("trigger2", "group1")
                .withSchedule(cronScheduleBuilder)
                .forJob(this.myJobDetail()).build();
    }

    }
```



**启动项目，定时生成日志**

