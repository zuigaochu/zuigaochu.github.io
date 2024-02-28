---
layout:     post
title:      "SpringBoot注解"
date:       2024-2-28
author:     "Mtz"
catalog: true
tags:
    - Java
    - Java项目组件文档
---





# 常用注解

1. **`@SpringBootApplication`：**
   - 主要注解，用于标记主应用类。它包括了`@Configuration`、`@EnableAutoConfiguration`、`@ComponentScan`三个注解，是一个复合注解。
2. **`@RestController`：**
   - 用于标识控制器类，表示该类的所有方法都以JSON格式返回数据，不需要额外的`@ResponseBody`注解。
3. **`@Controller`：**
   - 用于标识控制器类，类似于Spring MVC中的`@Controller`注解。
4. **`@Service`：**
   - 用于标识服务类，通常用于业务逻辑层。
5. **`@Repository`：**
   - 用于标识数据访问层（DAO）组件。
6. **`@Configuration`：**
   - 用于定义配置类，通常与`@Bean`注解一起使用，替代XML配置文件。
7. **`@EnableAutoConfiguration`：**
   - 开启Spring Boot的自动配置特性，根据项目的依赖自动配置Spring应用上下文。
8. **`@ComponentScan`：**
   - 用于指定组件扫描的基础包，Spring Boot会扫描该包及其子包下的组件。
9. **`@Autowired`：**
   - 用于进行依赖注入，将Bean注入到另一个Bean中。
10. **`@Value`：**
    - 用于注入外部属性值，可以在配置文件中定义属性值，然后通过`@Value`注解注入到变量中。
11. **`@PathVariable`：**
    - 用于从URL中获取路径变量的值，通常用于RESTful风格的控制器方法中。
12. **`@RequestParam`：**
    - 用于从HTTP请求中获取请求参数的值。
13. **`@GetMapping`、`@PostMapping`、`@RequestMapping`等：**
    - 用于映射HTTP请求到控制器方法，简化了常见的RESTful API的定义。
14. **`@ConfigurationProperties`：**
    - 用于将配置文件中的属性值映射到Java对象的字段，方便配置的统一管理。
15. **`@SpringBootTest`：**
    - 用于指定Spring Boot的测试类，通常与JUnit一起使用，用于测试Spring Boot应用。

# 注解2

1. **`@ConditionalOnClass`：**
   - 仅当类路径上存在指定的类时，才会启用标注了该注解的配置。
2. **`@ConditionalOnBean`：**
   - 仅当容器中存在指定的Bean时，才会启用标注了该注解的配置。
3. **`@ConditionalOnMissingBean`：**
   - 仅当容器中不存在指定的Bean时，才会启用标注了该注解的配置。
4. **`@ConditionalOnProperty`：**
   - 仅当指定的属性存在于配置文件中且具有特定的值时，才会启用标注了该注解的配置。
5. **`@EnableAsync`：**
   - 启用异步方法执行的支持，配合`@Async`注解一起使用。
6. **`@EnableScheduling`：**
   - 启用定时任务的支持，配合`@Scheduled`注解一起使用。
7. **`@EnableTransactionManagement`：**
   - 启用事务管理的支持。
8. **`@EnableCaching`：**
   - 启用Spring的缓存支持。
9. **`@SpringBootApplication`中的`exclude`属性：**
   - 用于排除某些自动配置类，可以通过`exclude`属性指定不需要的自动配置类。
10. **`@ConfigurationPropertiesScan`：**
    - 用于扫描@ConfigurationProperties注解的类，并将其注册为Spring的`@ConfigurationProperties` Bean。
11. **`@EnableConfigurationProperties`：**
    - 用于启用@ConfigurationProperties注解类的支持，使其成为Bean并注入到其他组件中。