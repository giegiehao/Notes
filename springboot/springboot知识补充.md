# springboot相关知识补充

[TOC]



## 理论——springboot底层相关理论知识

### 1.get请求参数是可以直接用对象去接收的

不需要注解，可以用来接收一些参数自由度比较高的请求。比如多条件查询，不确定用户会使用什么条件查询。



### 2. 统一时间Date格式

1. 在application文件中指定全局统一时间格式：

   ```yaml
   spring:
   	jackson:
   		date-format: yyyy-MM-dd HH:mm:ss
   		time-zone: GMT+8
   ```

2. 在相应的Date属性上使用`@JsonFormat`注解：

   ```java
   @JsonFormat(patten = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
   private Date time;
   ```



### 3. 全局异常处理

[Spring Boot REST API 最佳实践 - 第四章 - spring 中文网 (springdoc.cn)](https://springdoc.cn/spring-boot-rest-api-best-practices-part-4/)

同spring 3.2新增了@ControllerAdvice 注解，可以用于定义@ExceptionHandler、@InitBinder、@ModelAttribute，并应用到所有@RequestMapping中

使用@ExceptionHandler的方法会捕捉控制器中抛出的指定方法进行处理，并返回结果。

```java
@RestControllerAdvice
public class GlobalExceptionHandle {

    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    @ExceptionHandler(RuntimeException.class)
    public String err(Exception e) {
        System.out.println("全局异常管理器");
        return "服务器内部错误";
    }
}
```





## 问题——接触springboot项目过程中遇到的问题及解决方案

### 1.为什么使用`@Autowried`会警告

答：使用`@Autowried`注解，即表示使用spring的字段注入，在springboot2.4版本开始有个新规范：不推荐使用字段注入 推荐使用构造器注入，那么使用构造器注入又很麻烦，每次注入新对象都要在构造器中写，这时可以使用lombok注解`@AllArgsConstructor`。如果一个类需要的成员变量很多，而且不是都需要注入，此时可以使用`@RequiredArgsConstructor`，并在需要的成员变量上加上`final`。





## 创新——通过理解“理论”和“问题”所提出一点有想法的point



