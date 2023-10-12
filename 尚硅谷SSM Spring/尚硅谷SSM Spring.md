# Spring

### 需要了解的一些名词

单一架构：一个项目，一个工程，导出为war包，在一个tomcat上运行

单一架构，项目主要应用技术框架为：Spring，SpringMVC，Mybatis

<br/>

分布式架构：一个项目拆分成了很多个模块，每一个工程都是运行在自己的tomcat上。模块之间可以互相调用，。每个模块内部可以看成是一个单一架构的应用.

项目主要应用技术框架：SpringBoot（SSM），SpringCloud，中间件等 

<br/>

组件：可以复用的java对象

组件一定是对象，对象不一定是组件

组件一般是在程序中有业务功能的，而非组件一般没有

<br/>

## SpringFramework

 是Spring家族的基础

SpringFramework主要功能模块

![截图](f899a91485336ed85a89b02b1eef1f61.png)

<br/>

## IOC容器

### ioc容器继承关系

![截图](6e1895203ebb27d2aae865fd9bda31b1.png)

<br/>

<br/>

### IOC控制反转

控制反转：控制反转是针对对象的创建和调用控制而言的，对象的管理从程序员变成IOC容器，由IOC容器维护、管理对象

<br/>

<br/>

### DI依赖注入

在组件之间的传递依赖关系的过程中，将依赖关系在容器内部进行处理，这样就不必再应用程序代码中硬编码对象之间的依赖关系，实现了对象之间的解耦合。

它强调将依赖关系从一个对象传递给另一个对象，而不是让一个对象自己创建其依赖关系。在DI中，依赖关系是通过将依赖项注入到对象中而不是对象自己创建它们来实现的.

DI通过XML配置文件或注解方式实现，提供了三种形式的依赖注入：构造函数注入、setter方式注入和接口注入

<br/>

在传统的程序设计中，一个对象通常会负责创建它所依赖的其他对象，或者在需要的时候自己查找和实例化这些依赖项。这样的做法会导致对象之间的紧密耦合，降低了代码的可维护性和可测试性。当一个对象自己创建或查找依赖项时，它需要知道如何创建这些依赖项，这使得代码更加复杂，并且难以修改或扩展。

依赖注入的思想是将对象的依赖关系从对象本身分离出来，由外部的容器或框架负责创建和管理这些依赖项，然后注入（提供）给需要它们的对象。

所谓的依赖关系就是一个对象需要引用另一个对象，比如A对象需要B对象，那么DI就会把B对象实例给A使用

<br/>

### 使用xml方式管理对象

- 配置元数据（配置）
  - 使用xml配置元数据，标示需要ioc容器管理的类
    - 基于无参构造函数实例化组件 
      ```xml
      <bean id="..." class="..." />
      <!--id:组件的唯一标识，方便后期读取 class:组件的类的全限定符-->
      ```
    - 基于静态工厂方法实例化组件
      ```xml
      <bean id="..." class="..." factory-method="..." /> 
      <!--factory-method:指定静态方法 id:静态方法返回的对象的id（创建的组件）-->
      ```
    - 非静态工厂方法实例化组件
      ```xml
      <!--先在ioc容器中实例化工厂类-->
      <bean id="..." class="..." />
      
      <!--通过工厂类对象并指定工厂方法实例化组件-->
      <bean id="..." factory-bean="..." factory-method="..." /> 
      ```
  - di依赖注入，当实例的对象需要引用其它对象时，需要使用di注入依赖的对象
    - 构造函数注入
      
      1.单个构造参数注入
      ```xml
      <!--它们都需要存放在ioc容器中 比如A依赖B-->
      <bean id="Bid" class="B全限定符" />
      
      <bean id="Aid" class="A全限定符">
        <!--构造参数传值 di的配置
          <constructor-arg构造参数传值的di配置
            value =直接属性值 String name ="二狗子" int age = "18"
            ref=引用其他的bean beanId值 -->
        <constructor-arg ref="Bid" />
      </bean>
      ```
      
      2.多个构造参数注入
      ```xml
      <!-- C依赖B, 且C需要参数age和name-->
      <!-- 上面已经把B类放进了IOC容器，这里不再重复写-->
      <bean id="Cid" class="C全限定符">
        <constructor-arg name="name" value="张三" />
        <constructor-arg name="age" value="18" />
        <constructor-arg name="B" ref="Bid" />
      </bean>
      ```
    - 触发setter方法注入
      ```xml
      <!-- D依赖B，D中有属性的setter方法-->
      <bean id="Did" class="D全限定符">
        <!--D中有name属性，需要一个B对象
        有setName和setB方法-->
        <!-- name->属性名 setter方法的去set和首字母小写的值！调用set方法的名
      setMovieFinder -> movieFinder
      value / ref 二选一 value-"直接属性值"ref-"其他bean的Id"-->
        <property name="name" value="张三" />
        <property name="b" ref="Bid" />
      </bean>
      ```
- 实例化ioc容器
  - 使用ClassPathXmlApplicationContext类创建通过指定xml配置获取ioc容器，需要管理的类就会被这个ioc容器自动管理
    ```java
    // 方式一：直接创建容器并指定配置文件（底层还是像方式二那样，需要刷新）
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-config.xml");
    
    // 方式二：先创建ioc容器对象，再指定配置文件，再刷新。
    // 源码的配置过程，创建容器（spring） 和 配置文件（自己指定）
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext(); //创建对象
    applicationContext.setConfigLocations("spring-config.xml"); //指定配置文件
    applicationContext.refresh(); //调用ioc和di流程
    ```
- 获取bean组件
  - 通过实例化的ioc容器来获取容器中的bean对象
    ```java
    //方案1：直接根据beanId获取 返回值类型是Object 需要强转（不推荐）
    A a = (A)applicationContext.getBean("Aid");
    
    //方案2：根据beanId，同事指定bean的类型Class
    B b = applicationContext.getBean("Bid", B.class);
    
    //方案3：直接根据类型获取
    //根据bean的类型获取，那么同一个类型，在ioc容器中只能有一个bean
    //如果ioc容器中存在多个同类型的bean 会出现：NoUniqueBeanDefinitionException （bean不唯一）
    //ioc的配置一定是实现类，但是可以根据接口类型获取bean 如果C是接口，接口C instanceof ioc容器的类型 == true 判断是否是C的判断实现
    C c = applicationContext.getBean(C.class);
    ```

#### 扩展组件周期方法

```xml
<bean id="Aid" class="A全限定符" init-method="init" destroy-method="destroy" />
<!-- 
    init-method="初始化方法名"
    destroy-method="销毁方法名"
    ioc容器会在对应的时间节点回调对应的方法
-->
<!-- 
    初始化方法必须是public void 无参
    命名随意
-->
```

#### 组件作用域配置（组件实例的创建方式）

*xml的配置信息在程序中是由BeanDefinition对象存储的，每条<bean>标签都有一个对应的BeanDefinition对象并且存储在ioc容器中，当需要创建对象时，ioc容器可以根据BeanDefinition对象来反射创建多个Bean对象实例*

```xml
<bean id="..." class="..." scope="..." />
<!-- 
    scope属性指定改bean的创建方式
    singleton 单例 IOC容器初始化时创建该对象（一个）默认值
    prototype 多例 在需要获取Bean时创建新对象（多个）
-->
```

#### 扩展FactoryBean接口的使用

继承FactoryBean接口就是表示该类是一个工厂类，当需要的对象实例化的过程比较复杂时，使用工厂类可以简化实例的过程，只需要在xml文件中配置工厂类就可以完成一系列的实例过程。FactoryBean多用于第三方框架

```java
public class JavaBeanFactoryBean implements FactoryBean<JavaBean> {
  //重写getObject() 和 getObjectType 方法
  //在getObject方法中写生成JavaBean对象的过程，并返回
}
```

```xml
<bean id="..." class="JavaBeanFactoryBean全限定符"></bean>
<!--
    与非静态工厂方法实例化组件不同
    继承了FactoryBean就无需先把工厂类放进ioc容器，再指定工厂方法
    ioc容器自动实例factorybean工厂并调用getObject方法实例组件
-->
```

<br/>

### 基于注解方式的ioc配置

#### 组件添加标记注解

```java
@Component
@Repository
@Service
@Controller
//没有本质区别，只是在三层架构中方便区分
//组件BeanName问题：默认组件id为类的首字母小写
```

==注解的Value属性可以自定义id == ==默认组件id为类的首字母小写==

#### 配置ioc容器注解扫描范围

1.基本扫描配置

```xml
<context:component-scan base-package="包名" />
<!--
  1.包要精准，提高性能，防止一个包被重复扫描
  2.会扫描指定的包和子包内容
  3，需要配置多个包可以在包名中使用","分隔
-->
```

2.指定排除组件

```xml
<context:component-scan base-package="包名">
  <!-- 指定排除这个包下的Controller组件-->
  <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller" />
  <!-- type属性：指定需要排除的类型-->
  <!-- expression属性：指定需要排除的包名-->
</context:component-scan>
```

3.指定扫描组件

<br/>

#### 扩展周期方法

@PostConstruct：初始方法

@PreDestroy：销毁方法

初始方法和销毁方法必须是public void 无参

<br/>

#### 扩展作用域(创建方法）

@Scope(scopeName = ConfigurableBeanFactory.**SCOPE_SINGLETON**)：单例 默认值

@Scope(scopeName = ConfigurableBeanFactory.**SCOPE_PROTOTYPE**)：多例

<br/>

#### 注解di依赖注入

==@Autowired==

场景1：ioc容器中只有一个实现类对象，会正常注入

<br/>

场景2：ioc容器中没有默认的类型对象

```
@Autowired 使用它进行装配，默认情况下要求至少有一个Bean，否则报错，可以指定佛系装配@Autowired(required = flase)，不报错但是不会对属性进行注入
```

<br/>

场景3：用一个类型有多个对应的组件也会报错！无法选择

```
1.用成员属性名匹配组件id，如果有相应的id就会注入

2.@Qualifier(value = "id") 使用该注解指定获取bean的id，不能单独使用必须配合@Autowired

3.@Autowired + @Qualifier(value = "id") = Resource(name = "id")
```

<br/>

==@Value==

该注解一般用于在配置文件中给属性注入值

```xml
<!-- 1.创建配置文件-->
<!-- 2.在xml配置文件中配置需要扫描这个配置文件的信息-->
<context:property-placeholder location="classpath:xx.property" />
```

```java
@Value("${username}")
private String name;
//把配置文件中username的值赋给name属性

@Value("${username:name}");
private String name;
//如果配置文件中没有对应的值，就给这个属性默认值name；
```

<br/>

#### 完全基于注解开发（取代xml配置文件的配置类）

创建ioc配置类

```java
@ComponentScan({"包路径"}) //扫描指定包中的注解配置
@PropertySource(value = "classpath:xx.properties") //导入配置文件
@Configuration //声明一个配置类
public class Configuration {
  
}
```

AnnotationConfigApplicationContext：使用该类实现基于配置类的ioc容器

<br/>

==@Bean==

表示该方法需要生成一个bean对象存放在ioc容器中，一般用于第三方bean的生成

```java
//引入参数的值
@Value("${jdbc.user}")
String usernmae;
@Value("${jdbc.password}")
String password;
@Value("${jdbc.url}")
String url;

//生成德鲁伊连接池对象
@Bean
public DruidDataSource dataSource() {
  //实例化过程
  DuridDataSource datasource = new DruidDataSource();
  ...
  return dataSource;
}

//方法发的返回值类型 == bean组件的类型或者它的接口和父类
//方法的名字 == bean id
//@Bean 会真正让配置类的方法创建的组件存储到ioc容器中

/**
 * 引入外部配置的参数
 * 1.在配置类中引入全局变量（如上）
 * 2.作为方法的局部变量 （如下）
*/
@Bean
public DruidDataSource dataSource(@Value("${jdbc.user}") String username, 
                                  @Value("${jdbc.password}") String password
                                  @Value("${jdbc.url}") String url) {
  //实例化过程
  DuridDataSource datasource = new DruidDataSource();
  ...
  return dataSource;
}
```

@Bean详解

- beanName问题
  - 默认：方法名
  - 指定：name/value属性给beanid，覆盖方法名
- 周期方法如何指定
  - 原有注解方案：@PostConstruct + @PreDestroy 注解指定方法
  - bean属性指定：initMethod / destroyMethod指定
- 作用域（bean的创建形式）
  - 同用@Scope注解
- 引用其他ioc组件
  - 直接调用对方的bean方法即可（少用）
  - 直接形参变量进行注入
    - 如果ioc中没有对应类型的对象，报错
    - 如果ioc中有多个对应类型的对象，形参的变量名需要指定beanid

```java
@Bean(name = "dao1") //覆盖方法名给指定id
public StudentDao studentDao() {
  StudentDao dao = new Student();
  return dao;
}

@Bean(name = "dao2") //覆盖方法名给指定id
public StudentDao studentDao() {
  StudentDao dao = new Student();
  return dao;
}

@Bean(initMethod = "", destroyMethod = "") //指定周期方法
public StudentService studentService(StudentDao dao1) { //使用形参的方法，ioc容器会自动注入相应类型的对象，这里ioc中有两个dao对象，通过形参名dao1匹配beanid为dao1的对象进行注入
  StudentService service = new StudentService();
  service.setDao(dao1);
  return service;
}
```

<br/>

==@Import==

用于加载类

如果有多个配置文件，可以先创建一个配置类的根类，用@Import注解引入其它配置类

```java
@Import(value = {..., ...})
@Configuration
public class SpringConfiguration {
  
}
//加载多个类/配置类
```

<br/>

#### 整合Spring5-Test5搭建测试环境

在测试环境中不需要自己创建ioc容器对象，可以享受自动装配

相关依赖：junit-jupiter-api、spring-test

```java
@SpringJUnitConfig(value = 配置类 / location = "配置文件xml")
public class Test {
  @Test
  public void test1() {
    
  }
}
```

<br/>

<br/>

### 代理模式

代理模式可以解决附加功能代码干扰核心代码和不方便统一维护的问题！

将附加功能代码提取到代理中执行，不干扰目标核心代码！

![截图](269f26bab63646f6bbdf1757ee66ec4d.png)

代理模式分为静态代理和动态代理

#### 静态代理

每个目标方法都需要有个代理方法

静态代理确实实现了解耦，但是由于代码都写死了，完全不具备任何的灵活性。

多个静态代理类的声明，还是会产生大量的重复代码，没有统一管理

#### 动态代理

动态代理对象由代理工厂生成，在代理工厂写代理需要扩展的功能，代理工厂通过被代理对象产生相应的代理对象，这就是动态代理。

于静态代理的区别是不需要为每一个被代理对象都手动生成代理对象，而是由工厂生成。

#### 动态代理技术分类

- JDK动态代理：JDK原生的实现方式（javase），需要被代理的目标类==必须实现接口==！他会根据目标类的接口动态生成一个代理对象！代理对 象和目标对象由相同的接口！

- cglib：通过继承被代理的目标类实现代理，所以目标类可以==不需要实现接口==！

 888

<br/>

### AOP面向切面编程

OOP（面向对象编程）：纵向编程思维，继承关系，子类需要用到父类方法，一或是完全使用父类的方法，抑或是完全重写父类的方法，缺点就是不能对父类方法进行插入修改局部。

AOP：AOP是OOP的完善和补充，AOP是面向切面编程，横向的编程思维。将代码中重复的非核心业务提取到一个公共模块，最终再利用动态代理技术横向插入到各个方法中，针对非核心代码冗余问题！	

#### 主要应用场景

- 日志记录
- 事务处理
- 安全控制
- 性能监控
- 异常处理
- 缓存控制
- 动态代理

<br/>

#### AOP八个核心名词理解

横切关注点、通知（增强）、连接点、切入点、切面、目标、代理、织入

![屏幕截图 2023-10-09 231113.png](b471166239c48639a5628e7ceb579721.png)

<br/>

#### 关系梳理

<img src="78798e974226dd45ffbc589377e728c5.png" alt="屏幕截图 2023-10-09 232547.png" style="zoom:50%;" />
