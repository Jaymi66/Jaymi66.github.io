---
title: Spring 4.3.12.RELEASE Core Container Imporvements(核心加强)
date: 2017-12-17 15:05:43
tags: [java, spring]
---

[Spring 文档链接](https://docs.spring.io/spring/docs/4.3.14.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/#v4_2-Core-Container-Improvements)

<!-- more -->

### 1. Annotations such as `@Bean` get detected and processed on Java 8 default methods as well, allowing for composing a configuration class from interfaces with default `@Bean` methods.

> `@Bean` 注解可以检查和执行 Java 8 默认方法(as well), 允许为一个接口装载一个实例化对象(使用类内部的方法).

示例: 

```java
public interface UserService() {
  void findUserById(Integer id);
}
```

```java
public Main() {
  // 使用注解为 Service 装载实例 
  @Bean
  public UserService userServiceFindUserById() {
    return new UserService() {
      public void findUserById(Integer id) {
        System.out.println(id);
      }
    };
  }

  public static void main(String[] args) {
    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(MainTest.class);
    UserService userService = applicationContext.getBean(UserService.class);
    userService.findUserById(1);
  }
}
```


### 2. Configuration classes may declare @Import with regular component classes now, allowing for a mix of imported configuration classes and component classes.

> 配置类可使用`@Import`导入单个或多个常规的组件类, 允许一个导入混合的配置类和组件类.

示例: 
```java
@Component
public class ComponentClass {
  public void print() {
    System.out.println("ComponentClass");
  }
}
```
```java
@Configuration
@Import(value = {ComponentClass.class, ...})
public class MainTest {
  public static void main(String[] args) {
    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(MainTest.class);
    ComponentClass component = applicationContext.getBean(ComponentClass.class);
    component.print();
  }
}
```


### 3. Configuration classes may declare an `@Order` value, getting processed in a corresponding order (e.g. for overriding beans by name) even when detected through classpath scanning.

> 配置类可以声明一个`@Order`的值, 按照相对应的顺序进行处理(), 
支持对装载在诸如Lists和Arrays容器中的自动包装（auto-wired）组件的排序.

示例: 
```java
public interface RankService {
}
```
```java
@Component
@Order(2)
public class RankServiceImpl01 implements RankService {
  @Override
  public String toString() {
    return "RankServiceImpl01";
  }
}
```
```java
@Component
@Order(1)
public class RankServiceImpl02 implements RankService {
  @Override
  public String toString() {
    return "RankServiceImpl02";
  }
}
```
```java
@Component
@Order(3)
public class RankServiceImpl03 implements RankService {
  @Override
  public String toString() {
    return "RankServiceImpl03";
  }
}
```
```java
@Component
public class Results {

  @Autowired
  private List<RankService> rankService;

  @Override
  public String toString() {
    return rankService.toString();
  }
}
```
```java
@Configuration
@ComponentScan
public class MainTest {
  public static void main(String[] args) {
    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(MainTest.class);
    Results results = applicationContext.getBean(Results.class);
    // @Order 的值越小, 排序约靠前
    System.out.println(results); // [RankServiceImpl02, RankServiceImpl01, RankServiceImpl03]
  }
}
```