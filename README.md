## Mybatis简介
>  MyBatis 是一款优秀的持久层框架，它支持定制化 SQL、存储过程以及高级映射。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 POJOs(Plain Ordinary Java Object,普通的 Java对象)映射成数据库中的记录。
## Mybatis核心组件
- SqlSessionFactoryBuilder(构造器):它会根据配置或者代码来生成SqlSessionFactory,采用的是分步构建的Builder模式
- SqlSessionFactory(工厂接口):依靠它来生成SqlSession，使用的是工厂模式
- SqlSession(会话):一个既可以发送Sql执行结果，也可以获取Mapper的接口。在现有的技术中,一般我们会让其在业务逻辑代码中“消失”，而使用的是Mybatis提供的Sql接口编程技术，提高代码的可读性和可维护性。
- SQL Mapper(映射器):MyBatis新设计存在的组件，由一个Java接口和XML文件(或注解)构成,需要给出对应的SQL和映射规则。负责发送SQL去执行,并返回结果。
## 组件介绍
- [SqlSessionFactory(工厂接口)](Core%20components/SqlSessionFactory.md)
- [SqlSession(会话)](Core%20components/SqlSession.md)
- [Sql Mapper(映射器)](Core%20components/Mapper.md)
## SQL发送方式
### SQLSession发送SQL
- 有了映射器就可以通过SQlSessiom发送SQL了。
```java
  Role role =(Role)sqlSession.selectOne("com.Lee.Learn.RoleMapper.getRole",1L);
  /**
  selectOne方法表示使用查询并且只返回一个对象,而参数则是一个String对象和一个Object对象。
  String对象由命名空间加SQL id组合而成，完全定位一条SQL，当id唯一时，可以简写。
  *
```
### 用Mapper接口发送SQL
- SqlSession可以获取Maooer接口，在同股票Mapper接口发送SQL。
```java
   RoleMapper roleMapper =sqlSession.getMapper(RoleMapper.class);
   Role role=roleMapper.getRole(1L);
```
### 对比两种方式
- 使用Mapper接口编程可以消除SqlSession带来的功能性代码，提高可读性，而SqlSession发送SQL，需要一个SQL id去匹配SQl。使用Mapper接口，类似roleMapper.getRole(1L)则是完全面向对象的语言，更能体现业务的逻辑。
- 使用Mapper.getRole(1L)，IDE会提示错误和校验，而使用sqlSession.selectOne("getRole",1L)语言，只有在运行中才能知道是否会产生误会。
## 生命周期
- SqlSessionFactoryBuilder:只能存在于创建SqlSessionFactory的方法中.
- SqlSessionFactory:可以被认为是数据库的连接池，在使用Mybatis应用时一直存在。以单例的方式存在。
- SqlSession:存活在一个业务请求中，处理完整个请求后，关闭连接，规划给SQLSessionFactory。
- Mapper:代表一个业务处理步骤,随着SqlSession的关闭而废弃。
## 实例
- 一个简单的插入删除修改等的[demo](Core%20components/Demo.md)
## MyBatis配置
- 配置文件元素
```java
    <configuration/><!--配置-->
    <properties/> <!--属性-->
    <settings/><!--设置-->
    <typeAliases/><!--类型命名-->
    <typeHandlers/><!--类型处理器-->
    <plugins/><!--插件-->
    <environments default=""><!--配置环境-->
        <environment id=""><!--环境变量-->
            <transactionManager type=""/><!--事务管理器-->
            <dataSource type=""/><!--数据源-->
        </environment>
    </environments>
    <databaseIdProvider type=""/><!--s数据库厂商标识-->
    <mappers/> <!--映射器-->
```
- [properties属性](Configuration/properties.md)
- [setings设置](Configuration/settings.md)
- [typeAliases别名](Configuration/typeAliases.md)
