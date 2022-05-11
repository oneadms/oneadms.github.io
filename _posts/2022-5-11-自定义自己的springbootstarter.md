---
layout: post
title: 自定义自己的Springboot-starter
---



## 自定义自己的springboot-starter

#### step01

###### 编写TimeLog注解类

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface TimeLog {

}
```

#### step02

###### 编写TimeAspect类

```java
@Aspect
@Component
// 当属性满足条件时候，这个bean 生效
//prefix+name 组成配置完整名称 havingvalue是期待的配置值 如果为true 则条件 满足  matchifmissing 如果用户没有配置 time.log.enable属性 默认为true 开启切面

public class TimeLogAspect {

 private final Logger logger = LoggerFactory.getLogger(TimeLogAspect.class);
  @Pointcut("@annotation(com.example.log.springboot.starter.ano.TimeLog)")
  public void pc() {
  }
  @Around("pc()")
  public Object around(ProceedingJoinPoint pjp) {
    Object proceed = null;
    try {
      long startTime = System.currentTimeMillis();
      proceed = pjp.proceed();
      long endTime = System.currentTimeMillis();
      MethodSignature signature = (MethodSignature) pjp.getSignature();
      logger.info("{} 方法执行耗时{}毫秒",signature.getMethod().getName(),endTime-startTime);
    } catch (Throwable e) {
      e.printStackTrace();
    }
    return proceed;
  }

}
```

### step03

编写TimeLogAutoConfiguration类

```java
@Configuration
@ConditionalOnProperty(prefix = "time.log",name = "enable",havingValue = "true",matchIfMissing = true)
@ComponentScan(basePackages = "com.example.log.springboot.starter")
public class TimeLogAutoConfiguration {
}
```

#### step04

###### 编写spring.factories配置文件 让Spring容器扫描自动配置类

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.example.log.springboot.starter.config.TimeLogAutoConfiguration
```

#### step05

###### 安装到本地maven仓库

![image-20220511204748538](https://raw.githubusercontent.com/oneadms/blog_picture/main/image-20220511204748538.png)

##### 完结·撒花