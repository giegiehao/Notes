# MybatisPlus

[MyBatis-Plus 🚀 为简化开发而生](https://baomidou.com/)

简化了简单sql的书写，通过继承BaseMapper泛型类，反射获得泛型属性，为mapper接口提供简单sql方法

```java
public interface UserMapper extends BaseMapper<User> {
    
}
```

自动生成的sql方法的前提是实体类与数据表之间的正确映射关系：

| 数据表     | 实体类      |
| ---------- | ----------- |
| 下划线命名 | 驼峰命名    |
| 表名       | @TableName  |
| 主键       | @TableId    |
| 普通字段   | @TableField |

更多注解和属性查看官网[注解配置 | MyBatis-Plus](https://baomidou.com/reference/annotation/)

坑：如果数据表中字段“is_xxx"，实体类boolean类型属性名“isXxx”，属性名“isXxx”方法的lombok getter方法是"isXxx()"，反射处理时把方法中的"is"去掉后作为变量名，就导致字段无法正确映射，不建议使用is开头的属性名，使用@TableField表明字段名。

#### 使用配置

看官网[使用配置 | MyBatis-Plus](https://baomidou.com/reference/#spring-boot-配置)

#### 持久层接口（Service）

封装了常见的 CRUD 操作，包括插入、删除、查询和分页等。通过继承 IService 接口，可以快速实现对数据库的基本操作。

```java
//service接口继承Iservice
public interface UserService extends IService<User> {
    
}

//service实现类，继承ServiceImpl，实现了IService接口中方法
public class UserServiceImpl extends ServiceImpl<UserDao, User> implements UserService {
    
}
```

> 问题：为什么UserServiceImpl继承了ServiceImpl（ServiceImpl实现了IService），为什么UserService接口还要继承IService接口？
>
> 答：疑惑这个问题我真的是少写代码了，答案很简单，使用spring的自动装配一般都是用接口来接，如果不继承IService，多态对象就没法调用IService接口的方法。

#### 条件构造器（Wrapper）

简单来说就是用对象来构造sql语句
官网介绍：用于构建复杂的数据库查询条件。Wrapper 类允许开发者以链式调用的方式构造查询条件，无需编写繁琐的 SQL 语句，从而提高开发效率并减少 SQL 注入的风险

Wrapper支持拼接到mapper文件的sql中，满足设计上的要求或更复杂的sql

#### lambda条件构造器

一个基于 Lambda 表达式的查询条件构造器，它通过 Lambda 表达式来引用实体类的属性，从而避免了硬编码字段名。

#### service上的chainWrapper链式条件构造器

在lambda条件构造器的基础上多了个“提交”的方法，在service中不需要调用this或者bashMapper方法

#### 以上三个构造器区别

```java
//UpdateWrapper
UpdateWrapper<User> updateWrapper = new UpdateWrapper<User>()
    .set("password", "123")
    .eq("id", 1);
update(updateWrapper); //执行

//lambdaUpdateWrapper
LambdaUpdateWrapper<User> lambdaUpdateWrapper = new LambdaUpdateWrapper<User>()
    .set(User::getUserPassword, "123")
    .eq(User::getUserId, 1);
update(lambdaUpdateWrapper); //执行

//LambdaUpdateChainWrapper
lambdaUpdate()
    .set(User::getUserPassword, "123")
    .eq(User::getUserId, 1)
    .update(); //链式执行
```

#### 大量数据批处理速度优化

- 普通for循环逐条插入速度极差，不推荐（多次发送，多次处理）
- MP的批量新增(Batch)，基于预编译的批处理，性能不错（多条sql，一次处理）
- 配置jdbc参数，开rewriteBatchedStatements,性能最好（一条sql，一次处理）

#### 静态工具Db

无需注入service或mapper，数据库操作



