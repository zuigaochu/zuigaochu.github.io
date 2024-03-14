---
layout:     post
title:      "SpringBoot限制字段为空的方法"
date:       2024-3-14
author:     "Mtz"
catalog: true
tags:
    - Java
    - Java项目组件文档
---







# 基本约束注解

* 在Java的Bean Validation API中，有多种注解可用于对实体类的字段进行各种限制。下面列出了一些最常用的注解及其用例，都加上了中文注释，帮助理解每个注解的用途。

```java
import javax.validation.constraints.*;

public class User {
    // 系统生成的主键自增id用 @Null
    
    @NotNull(message = "用户ID不能为空") // 确保字段不为null
    private Integer id;

    @NotEmpty(message = "用户名不能为空") // 确保字符串非null且长度大于0
    private String username;

    @Size(min = 6, max = 14, message = "密码长度必须在6到14字符之间") // 确保字符串长度在min和max之间
    private String password;

    @Email(message = "邮箱格式不正确") // 确保字段是一个格式正确的电子邮件地址
    private String email;

    @Positive(message = "年龄必须是正数") // 确保数值是正数
    private Integer age;

    @NegativeOrZero(message = "账户余额必须是负数或零") // 确保数值是负数或0
    private BigDecimal balance;

    @Past(message = "生日必须是过去的日期") // 确保日期在当前时间之前
    private LocalDate birthday;

    @Future(message = "到期日必须是将来的日期") // 确保日期在当前时间之后
    private LocalDate expireDate;

    @Pattern(regexp = "^(.+)@(.+)$", message = "电子邮件地址必须匹配正则表达式") // 确保字符串匹配正则表达式
    private String emailPattern;

    @DecimalMin(value = "0.1", inclusive = true, message = "评分必须大于或等于0.1") // 确保数字值大于或等于指定的最小值
    private BigDecimal score;

    @DecimalMax(value = "5.0", message = "评分必须小于或等于5.0") // 确保数字值小于或等于指定的最大值
    private BigDecimal rating;

    @Min(value = 18, message = "年龄必须大于或等于18") // 确保数值大于等于指定的最小值
    private int minimumAge;

    @Max(value = 30, message = "年龄必须小于或等于30") // 确保数值小于等于指定的最大值
    private int maximumAge;

    @NotBlank(message = "个人简介不能为空") // 确保字符串非null，且去除两端空白字符后长度大于0
    private String bio;

    @Digits(integer = 3, fraction = 2, message = "工资只能有3位整数和2位小数") // 确保数字的值有指定的最大整数位数和小数位数
    private BigDecimal salary;

    // Getter和Setter省略
}

```

# 控制层示例

```java
import org.springframework.web.bind.annotation.*;

import javax.validation.Valid;

@RestController
@RequestMapping("/api/users")
public class UserController {

    @PostMapping("/")
    public String createUser(@Valid @RequestBody User user) {
        // 创建用户的逻辑
        return "用户创建成功";
    }
    
    // 其他控制器方法
}

```

# 注解叠加使用案例

```java
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;

public class Product {

    @NotNull(message = "产品名称不能为空")
    @Size(min = 2, max = 100, message = "产品名称长度必须在2到100字符之间")
    private String name;

    // 构造函数、getter和setter方法省略
}

```



# @NotEmpty`和`@NotBlank区别

* `@NotEmpty`验证的是字符串非空，不考虑其中是否包含空格，只要字符串长度大于0即认为有效。

* `@NotBlank`验证的是字符串非空白字符，即至少包含一个非空白字符，字符串中可以包含空格，但至少要有一个非空白字符才算有效。

* 我还是不能理解，比如说我有个username字段，前端输入了几个空格，我需要限制，不能为空，也不能为空格，用什么注解？

  * ```java
    import javax.validation.constraints.NotBlank;
    
    public class User {
    
        @NotBlank(message = "用户名不能为空")
        private String username;
    
        // 其他字段和方法
    }
    
    ```

* @NotEmpty什么时候用？

  * `@NotEmpty`注解主要用于验证集合、数组或者字符串等对象不为空，并且至少包含一个元素。因此，它适用于需要确保对象不为空且至少包含一个元素的情况

    ```java
    @NotEmpty(message = "订单列表不能为空")
    private List<Order> orders;
    ```

  * **验证字符串不为空**：尽管`@NotBlank`注解更适合验证字符串不为空且至少包含一个非空白字符，但有时你可能需要确保字符串非空且长度大于0，而不关心其中是否包含空白字符。在这种情况下，

  * ```hava
    @NotEmpty(message = "用户名不能为空")
    private String username;
    ```

  * **验证Map不为空**：当你需要确保一个Map对象不为空，并且至少包含一个键值对时，可以使用`@NotEmpty`注解。

  * ```java
    @NotEmpty(message = "用户信息不能为空")
    private Map<String, String> userInfo;
    ```

    





