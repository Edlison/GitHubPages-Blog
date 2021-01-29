---
title: Java中的定时任务
categories:
- Notebook
tags:
- java
- springboot
---

# Schedule

只适合处理简单的计划任务，不能处理分布式计划任务。优势：是spring框架提供的计划任务，开发简单，执行效率比较高。且在计划任务数量太多的时候，可能出现阻塞，崩溃，延迟启动等问题。



例子

```java
@Configuration      //1.主要用于标记配置类，兼备Component的效果。
@EnableScheduling   // 2.开启定时任务
public class DynamicScheduleTask implements SchedulingConfigurer {
    /**
     * 执行定时任务.
     */
    @Override
    public void configureTasks(ScheduledTaskRegistrar taskRegistrar) {

        taskRegistrar.addTriggerTask(
                //1.添加任务内容(Runnable)
                () -> System.out.println("执行动态定时任务: " + LocalDateTime.now().toLocalTime()),
                //2.设置执行周期(Trigger)
                triggerContext -> {
                    //2.1 从数据库获取执行周期
                    String cron = cronMapper.getCron();
                    //2.2 合法性校验.
                    if (StringUtils.isEmpty(cron)) {
                        // Omitted Code ..
                    }
                    //2.3 返回执行周期(Date)
                    return new CronTrigger(cron).nextExecutionTime(triggerContext);
                }
        );
    }
}
```



## Cron表达式

Cron = "seconds minutes hours day month week"

或

cron = "seconds minutes hours day month week year"

| 位置 | 时间       | 允许值    | 允许的特殊字符  |
| ---- | ---------- | --------- | --------------- |
| 1    | 秒         | 0-59      | , - * /         |
| 2    | 分钟       | 0-59      | , - * /         |
| 3    | 小时       | 0-23      | , - * /         |
| 4    | 日         | 1-31      | , - * / L W C   |
| 5    | 月         | 1-12      | , - * /         |
| 6    | 星期       | 1-7       | , - * ? / L C # |
| 7    | 年（可选） | 1970-2099 | , - * /         |

> 星号(*)：可用在所有字段中，表示对应时间域的每一个时刻，例如，*在分钟字段时，表示“每分钟”；
>
> 问号（?）：该字符只在日期和星期字段中使用，它通常指定为“无意义的值”，相当于占位符；
>
> 减号(-)：表达一个范围，如在小时字段中使用“10-12”，则表示从10到12点，即10,11,12；
>
> 逗号(,)：表达一个列表值，如在星期字段中使用“MON,WED,FRI”，则表示星期一，星期三和星期五；
>
> 斜杠(/)：x/y表达一个等步长序列，x为起始值，y为增量步长值。如在秒数字段中使用0/15，则表示为0,15,30和45秒，而5/15在分钟字段中表示5,20,35,50，你也可以使用*/y，它等同于0/y；
>
> L：该字符只在日期和星期字段中使用，代表“Last”的意思，但它在两个字段中意思不同。L在日期字段中，表示这个月份的最后一天，如一月的31号，非闰年二月的28号；如果L用在星期中，则表示星期六，等同于7。但是，如果L出现在星期字段里，而且在前面有一个数值X，则表示“这个月的最后X天”，例如，6L表示该月的最后星期五；
>
> W：该字符只能出现在日期字段里，是对前导日期的修饰，表示离该日期最近的工作日。例如15W表示离该月15号最近的工作日，如果该月15号是星期六，则匹配14号星期五；如果15日是星期日，则匹配16号星期一；如果15号是星期二，那结果就是15号星期二。但必须注意关联的匹配日期不能够跨月，如你指定1W，如果1号是星期六，结果匹配的是3号星期一，而非上个月最后的那天。W字符串只能指定单一日期，而不能指定日期范围；
>
> LW组合：在日期字段可以组合使用LW，它的意思是当月的最后一个工作日；
>
> 井号(#)：该字符只能在星期字段中使用，表示当月某个工作日。如6#3表示当月的第三个星期五(6表示星期五，#3表示当前的第三个)，而4#5表示当月的第五个星期三，假设当月没有第五个星期三，忽略不触发；
>
> C：该字符只在日期和星期字段中使用，代表“Calendar”的意思。它的意思是计划所关联的日期，如果日期没有被关联，则相当于日历中所有日期。例如5C在日期字段中就相当于日历5日以后的第一天。1C在星期字段中相当于星期日后的第一天。

**Cron表达式对特殊字符的大小写不敏感，对代表星期的缩写英文大小写也不敏感。**

# Qurtz

Quartz是OpenSymphony开源组织在Job scheduling领域又一个开源项目，它可以与J2EE与J2SE应用程序相结合也可以单独使用。Quartz可以用来创建简单或为运行十个，百个，甚至是好几万个Jobs这样复杂的程序。

　　Quartz是一个完全由java编写的开源作业调度框架。不要让作业调度这个术语吓着你。尽管Quartz框架整合了许多额外功能， 但就其简易形式看，你会发现它易用得简直让人受不了！
在开发Quartz相关应用时，只要定义了Job（任务），Trigger（触发器）和Scheduler（调度器），即可实现一个定时调度能力。其中Scheduler是Quartz中的核心，Scheduler负责管理Quartz应用运行时环境，Scheduler不是靠自己完成所有的工作，是根据Trigger的触发标准，调用Job中的任务执行逻辑，来完成完整的定时任务调度。

- Job - 定时任务内容是什么。
- Trigger - 在什么时间上执行job。
- Scheduler - 维护定时任务环境，并让触发器生效。
- 在SpringBoot中应用Quartz，需要依赖下述资源：

1. 创建定时任务

```java
public class MyTask extends QuartzJobBean {
    @Override
    protected void executeInternal(JobExecutionContext jobExecutionContext) throws JobExecutionException {
        //TODO 这里写定时任务的执行逻辑
        System.out.println("简单的定时任务执行时间："+new Date().toLocaleString());
    }
}
```

2. 创建配置类将定时任务添加到调度中

```java
@Configuration
public class QuartzConfig {
	//指定具体的定时任务类
    @Bean
    public JobDetail uploadTaskDetail() {
        return JobBuilder.newJob(MyTask.class).withIdentity("MyTask").storeDurably().build();
    }

    @Bean
    public Trigger uploadTaskTrigger() {
        //TODO 这里设定执行方式
        CronScheduleBuilder scheduleBuilder = CronScheduleBuilder.cronSchedule("*/5 * * * * ?");
        // 返回任务触发器
        return TriggerBuilder.newTrigger().forJob(uploadTaskDetail())
                .withIdentity("MyTask")
                .withSchedule(scheduleBuilder)
                .build();
    }
}
```



**还可以对定时任务进行动态暂停，修改，启动，单次执行等操作。**

见文章：

https://cloud.tencent.com/developer/article/1640190









----

References



https://www.cnblogs.com/jing99/p/11546559.html

https://cloud.tencent.com/developer/article/1640190