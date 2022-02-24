---
layout: post
title: SpringAop
tag: 
  - java
  - Spring AOP
  - Spring
---



## 准备开发阶段

### 1. 导入依赖

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aspects</artifactId>
  <version>4.3.0.RELEASE</version>
</dependency>
```

### 2.编辑Spring上下文文件

	1. 开启组件扫描
	1. 开启aspect自动代理

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:aop="http://www.springframework.org/schema/aop"
  xsi:schemaLocation="
  http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd
  http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
  <context:component-scan base-package="com.oneamd.test"/>
  <aop:aspectj-autoproxy />
</beans>
```

### 3.注解方式—（配置注入点） 创建自己的MyAspect文件

```java
package com.oneamd.test;
import java.util.HashMap;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.reflect.CodeSignature;
import org.springframework.stereotype.Component;
@Component
@Aspect //告诉Spring容器这是一个Aspect类
public class MyAspect {
  @Around("execution(* com.oneamd.test.*.*(..))")
  public Object around(ProceedingJoinPoint point) {
    Object[] paramsValues = point.getArgs();
    String[] parameterNames = ((CodeSignature) point.getSignature()).getParameterNames();
    HashMap<String, Object> mapper = new HashMap<>();
    for (int i = 0; i < parameterNames.length; i++) {
      System.out.println(parameterNames[i] + ":" + paramsValues[i]);
    }
    Object object = null;
    try {
      object = point.proceed();
    } catch (Throwable e) {
      e.printStackTrace();
    }
    return object;
  }
}
```

### 4.XML文件方式配置

```xml
<aop:config>
  <aop:aspect ref="myAspect">
    <aop:around method="around" pointcut="execution(* com.oneamd.test.*.*(..))" />
  </aop:aspect>
</aop:config>
<bean class="com.oneamd.test.MyAspect" id="myAspect"/>
```

### 4.测试是否注入成功

```java
import com.oneamd.test.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
//必须加载Spring上下文配置文件
@ContextConfiguration("classpath:applicationContext.xml")
public class AopTest {

  @Autowired
  private User user;

  @Test
  public  void test() {

    user.login("oneadm","aaa1024");
  }
}
```

## SpringAop的主要应用场景

- 日志记录
- 权限校验
- 事务回滚

###  切点表达式:  

- execution(访问修饰符 包名.类名.方法名(参数列表))

- ```
  例子: * 匹配全部 ..匹配所有 包括无参数
  execution(* com.oneamd.test.*.*(..))
  ```



## @Before

>1. @before 在方法前注入切点 
>
>   

## @After

> 1. @After在方法后注入切面

## @Around （重点）

> 1. 环绕注入 使用 ProceedingJoinPoint
> 2. 注意点： 
>    - ProceedingJoinPoint只能在Around中使用 不能在before after中使用
> 3. 代码
>
> ```java
>  public Object around(ProceedingJoinPoint point) {
> 	//获取注入的参数值
>      Object[] paramsValues = point.getArgs();
>      //获取注入的参数名
>     String[] parameterNames = ((CodeSignature) point.getSignature()).getParameterNames();
>     for (int i = 0; i < parameterNames.length; i++) {
>       System.out.println(parameterNames[i] + ":" + paramsValues[i]);
>     }
>     Object object = null;
>     try {
>       //这里执行原来方法
>       object = point.proceed();
>     } catch (Throwable e) {
>       e.printStackTrace();
>     }
> 	//返回执行方法的对象
>     return object;
>   }
> 
> ```
>
> 