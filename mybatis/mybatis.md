## 注解

@Mapper：运行时，会自动生成该接口的实现类对象（代理对象），并交给IOC容器管理

@Select

@Insert

@Options(useGeneratedKeys = true, keyProperty = "<field>")：useGeneratedKeys 表示使用数据库生成的主键，keyProperty指定接收主键值的属性

@Param：给mapper方法的参数命名，使用情况：1.mapper方法需要给sql语句传入多个参数； 2.@Param注解javaBean对象
在**不使用**@Param注解声明参数的时候，mapper方法传参只能传一个值或对象，且必须使用的是#{}方式来取参数。

<br/>

---

<br/>

- #{parameter}：在sql语句中传递参数，执行sql时，会被替换为？占位符，不存在sql注入问题，在传递参数时都可以使用

![截图](b89c7db754c5deefc2558dfd5ddc99d2.png)

- ${field}：拼接sql。直接将参数拼接在sql语句中，**存在sql注入问题**，一般对表名、列表进行动态设置时可以使用，使用较少

<br/>

- 当需要模糊查询时 like ='%<>%'，不能使用#{}的形式传入参数，因为此时'%?%'是字符串类型的，占位符不起作用。需要使用concat函数用来拼接字符串

![截图](78ab7bc75b9a72df88c31d3d78989e2a.png)

<br/>

- 使用mabatis来管理sql语句，不仅可以使用注解，还可以使用XML映射文件来绑定sql语句。注解和XML映射文件都是有效方法，一般对于简单的sql语句使用注解，对于复杂的sql使用XML可以更好的管理sql语句。注解有的属性，在XML标签中都有相同的属性，比如useGeneratedKeys，keyProperty等

![截图](c5c1c6709700fcbd1187f4ba2f7e0cbc.png)

<br/>

## 动态SQL

- <if> & <where>

![截图](6213f26af0257c3a72a375bccc364ad7.png)

- <set>

![截图](8107390720d74cceda4bcc6d230074c1.png)

- <foreach>

![截图](30df7d06021c0a177cd66763034282fa.png)

- <sql> & <include>

![截图](2a663ce713a088484a70d2450ed7eb18.png)

## 一些没有见过的写法

获取emp对象的属性竟然不用emp.get属性。

这里是自动调用了这个属性的get方法

![截图](44e4d66ffee819a4939f4aa631a6bc92.png)
