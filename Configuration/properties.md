#### properties可以给系统配置一些运行参数，可以放在XML文件或者peoperties文件中，而不是放在java编码中，这样的好处在于方便参数修改，不回引起代码的重新编译。
提供三种方式使用:
- property子元素
```java
    <properties>
        <property name="database.driver" value="com.mysql.jdbc.Driver"/>
        <property name="database.url" value="jdbc:mysql://localhost:3306/login"/>
        <property name="database.username" value="root"/>
        <property name="database.password" value="wj5201314"/>
    </properties>
        <!--数据库环境-->
     <environments default="development">
         <environment id="development">
             <transactionManager type="JDBC"/>
             <dataSource type="POOLED">
                <property name="driver" value="${database.driver}"/>
                 <property name="url" value=${database.url}"/>
                 <property name="username" value=${database.username}"/>
                 <property name="password" value=${database.password}"/>
             </dataSource>
         </environment>
     </environments>
```
- 使用properties文件
  - jdbc.properties 
    ```java
        database.driver=com.mysql.jdbc.Driver
        database.url=jdbc:mysql://localhost:3306/login
        database.username=root
        database.password=wj5201314
    ```
  - 引入MyBatis文件
     ```java
      <properties resource="jdbc.properties"/>
     ```
- 使用程序传递方式传递参数
>对已经加密的用户名和密码解密后的字符串重置到properties属性中，最后使用SqlSessionFactoryBuilder的build方法，传递多个peoperties参数来完成。
