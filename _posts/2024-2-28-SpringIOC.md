---
layout:     post
title:      "Spring框架重点概念"
date:       2024-2-28
author:     "Mtz"
catalog: true
tags:
    - Java
    - Java项目组件文档
---

# Spring IOC

1. 在Spring框架中，IOC（Inversion of Control）是一种设计模式，它反转了传统的控制流程，将对象的创建和依赖关系的管理交给了容器。简而言之，IOC实现了将对象的创建和组装从代码中解耦，由容器负责管理对象的生命周期和依赖关系。



2. 使用对象时候由主动new对象转换成由外部提供对象,此过程中对象的创建权由程序转移到外部，这种思想叫做控制反转
   Spring技术对此提供的实现
   Spring提供了一个容器，称为IOC容器，用来充当IOC思想中的外部
   IOC容器负责对象的创建、初始化等一系列工作，被创建或被管理的对象在IOC容器中统称为Bean。



3. 为什么需要IOC呢？使用IOC可以降低组件之间的耦合度，提高代码的可维护性和可测试性。通过IOC容器，对象的创建和依赖注入变得更加灵活，使得系统更容易扩展和修改。

```java
// 定义一个接口
public interface MessageService {
    String getMessage();
}

// 实现接口的一个类
public class HelloWorldMessageService implements MessageService {
    @Override
    public String getMessage() {
        re、turn "Hello, World!";
    }
}

// config类
@Configuration
@ComponentScan(basePackages = "com.ma.book")
public class AppConfig {

    // 定义一个MessageService的bean
    @Bean
    public MessageService messageService() {
        return new HelloWorldMessageService();
    }
}

// 在应用程序中使用IOC容器
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MyApp {
    public static void main(String[] args) {
        // 使用AnnotationConfigApplicationContext作为IOC容器
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

        // 从容器中获取MessageService实例
        MessageService messageService = context.getBean(MessageService.class);

        // 调用方法获取消息并输出
        System.out.println(messageService.getMessage());
    }
}

```

# SpringAop

* 概念

  

  1. 在Spring框架中，AOP（Aspect-Oriented Programming）是一种编程范式，它允许在程序运行时动态地将代码切入到现有类的方法中，而无需修改这些类的源代码。AOP通过在应用程序中横切关注点（cross-cutting concerns）的方式提供了一种更好的代码结构和模块化的方式。

  2. 关注点是指横跨应用程序多个模块的功能，例如日志记录、事务管理、性能监控等。AOP的主要目标是通过将这些关注点与核心业务逻辑进行分离，使代码更清晰、可维护，并避免在核心业务逻辑中散布这些横切关注点的代码。

  3. 在Spring中，AOP的实现依赖于代理模式。Spring AOP通过使用代理对象来织入切面（Aspect），切面定义了在何处以及如何横切关注点。切面通常包含了一系列通知（Advice），定义了在方法执行的不同阶段插入的代码。

  Spring框架提供了多种类型的通知，包括：

  1. **前置通知（Before advice）：** 在目标方法执行前执行的代码。
  2. **后置通知（After returning advice）：** 在目标方法成功执行后执行的代码。
  3. **异常通知（After throwing advice）：** 在目标方法抛出异常后执行的代码。
  4. **最终通知（After advice）：** 在目标方法执行完毕后无论是否发生异常都会执行的代码。
  5. **环绕通知（Around advice）：** 包围目标方法执行，可以在目标方法执行前后添加自定义逻辑。

  通过使用AOP，开发人员可以更好地组织和管理代码，将横切关注点从核心业务逻辑中分离出来，提高代码的可维护性和可读性。

* 代码示例

```java
      <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>
```

```java
public interface MessageService {
    String getMessage();
}

```

```java
// 实现接口的一个类
@Service
public class HelloWorldMessageService implements MessageService {
    @Override
    public String getMessage() {
        return "Hello, World!";
    }
}
```

```java
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

/**
 * 切面类用于日志记录的AOP
 */
@Aspect
@Component
public class LoggingAspect {

    /**
     * 在执行MessageService的方法前执行日志记录
     */
    @Before("execution(* com.ma.book.service.MessageService.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("在执行前记录日志: " + joinPoint.getSignature().getName());
    }

    /**
     * 在执行MessageService的方法后执行日志记录
     */
    @After("execution(* com.ma.book.service.MessageService.*(..))")
    public void logAfter(JoinPoint joinPoint) {
        System.out.println("在执行后记录日志: " + joinPoint.getSignature().getName());
    }
}

```

# SpringBean

* Spring Bean容器是Spring框架的核心组件之一，负责管理和组织应用中的对象（也称为Bean）

1. **什么是Bean？**
   - 在Spring中，Bean是指由Spring容器管理的对象。这些对象通常是应用中的组件，例如服务、数据访问对象、实体等。
2. **Spring Bean容器的作用：**
   - Spring Bean容器负责创建、管理和注入（或装配）应用中的Bean。它提供了一个环境，使得开发者可以通过配置文件或者注解的方式定义和组织Bean。
3. **Bean的生命周期：**
   - Bean的生命周期包括实例化、初始化、使用和销毁四个阶段。
   - Spring容器负责在适当的时机执行这些阶段的操作，例如通过构造函数实例化Bean、调用初始化方法、提供Bean给其他组件使用，最后在应用关闭时销毁Bean。
4. **Bean的作用域：**
   - Spring支持多种Bean的作用域，包括单例（Singleton）、原型（Prototype）、会话（Session）、请求（Request）等。
   - 每种作用域定义了Bean实例的生命周期和访问范围。
5. **Bean的装配：**
   - 装配是指将不同的Bean组装在一起，形成应用的组件关系。
   - Spring支持通过XML配置、注解和Java配置等方式进行Bean的装配。
6. **Bean的依赖注入：**
   - 依赖注入是Spring框架的一个关键特性，它通过将一个Bean的依赖关系通过构造函数、Setter方法或者接口注入到另一个Bean中。这样可以降低组件之间的耦合度。
7. **Bean的自动装配：**
   - Spring支持自动装配，通过指定 `@Autowired` 注解或者使用XML配置，Spring可以自动识别和满足Bean之间的依赖关系。
8. **ApplicationContext和BeanFactory：**
   - Spring提供了两个核心的容器接口，即`ApplicationContext`和`BeanFactory`。`ApplicationContext`是`BeanFactory`的子接口，提供了更丰富的功能，例如事件传播、AOP等。
9. **配置元数据：**
   - Bean的配置信息通常使用配置元数据来定义，可以使用XML文件、Java配置类或者注解。配置元数据包含了Bean的类型、依赖关系、作用域、初始化方法、销毁方法等信息。

总体而言，Spring Bean容器为开发者提供了一种松散耦合的方式来组织和管理应用中的组件，使得应用更加灵活、可维护和可测试。

# Spring注解

* 在Spring框架中，注解是一种简化配置的方式，用于定义和描述组件、依赖关系、作用域、生命周期等

1. **`@Component`：**
   - 用于标识一个类为Spring的组件（Bean）。被标注为`@Component`的类会被Spring自动扫描并注册为Bean。
2. **`@Repository`：**
   - 通常用于标识数据访问层（DAO）组件。被标注为`@Repository`的类通常用于持久化层，与数据库进行交互。
3. **`@Service`：**
   - 通常用于标识服务层组件。被标注为`@Service`的类表示业务逻辑的组件。
4. **`@Controller`：**
   - 通常用于标识控制器层组件。被标注为`@Controller`的类用于处理HTTP请求，通常与Spring MVC一起使用。
5. **`@Autowired`：**
   - 用于自动装配Bean。可以标注在构造函数、Setter方法、成员变量上，Spring会根据类型进行自动装配。
6. **`@Qualifier`：**
   - 与`@Autowired`一起使用，用于指定具体的Bean名称进行注入。
7. **`@Value`：**
   - 用于注入外部属性值。可以标注在成员变量、Setter方法上，通过`${}`表达式指定属性值。
8. **`@Configuration`：**
   - 用于定义Java配置类，替代XML配置文件。在这个类中可以定义Bean、配置Bean之间的依赖关系等。
9. **`@Bean`：**
   - 用于定义一个Bean，通常在`@Configuration`注解的类中使用。方法的返回值将成为一个Spring Bean。
10. **`@Scope`：**
    - 用于定义Bean的作用域，包括单例（`singleton`）、原型（`prototype`）、会话（`session`）等。
11. **`@PostConstruct`：**
    - 用于在Bean初始化时执行的方法，相当于XML配置中的`init-method`。
12. **`@PreDestroy`：**
    - 用于在Bean销毁时执行的方法，相当于XML配置中的`destroy-method`。
13. **`@RequestMapping`：**
    - 用于映射HTTP请求到控制器方法。通常用于Spring MVC中的控制器类。
14. **`@PathVariable`：**
    - 用于从URL中获取变量值，通常用于Spring MVC的控制器方法。
15. **`@RequestBody`：**
    - 用于将HTTP请求的内容（通常为JSON或XML）绑定到Java对象，通常用于Spring MVC的控制器方法。