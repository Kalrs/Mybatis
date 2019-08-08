### 使用Mybatis首先是去生产SqlSessionFactory，可以通过读取配置文件XML的形式生成,也可以通过java代码的形式去生成
>SqlSessionFactory是一个接口,在MyBatis中它存在两个实现类:SqlSesionManager和DefaultSqlSessionFactory。一般而言,具体是由DefalutSqlSessionFactory去实现而SqlSession使用在多线程的环境中,它的具体实现依靠DefaultSqlSessionFactory。
- SqlSessionFactory唯一的作用就是生产Mybatis的核心接口对象SqlSession，责任唯一，往往采用单例模式处理
### 使用XML构建SqlSessionFactory
#### 分类
- 基础配置文件：通常只有一个，主要是配置一些最基本的上下文参数和运行环境
- 映射文件：配置映射关系、SQL、参数等信息
### 基础配置文件
![image](config.png)
#### 基础配置文件描述
- typeAlias:定义一个别名role，代表一个类，上下文中就可以使用别名去代替全限定名
- environment:描述的是数据库,它里面的transactionManager元素是配置事务管理器,图片中采用的是MyBatis的JDBC管理方式。然后采用dataSource元素配置数据库,其中属性type="POOLED"代表采用Mybatis内部提供的连接池方式,最后定义一些关于JDBC的属性信息
- mapper元素代表引入的那些映射器。
### 使用Java代码创建SqlSessionFactory
