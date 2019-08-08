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
- [Sql Mapper(映射器)](Core%20components/SqlMapper.md)
