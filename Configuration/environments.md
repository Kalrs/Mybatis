>运行环境主要的作用就是配置数据库信息,可以配置多个数据库.其下分为两个可配置的元素:事务管理器和数据源.实际工作中大部分情况下会采用sping对数据源和数据库的事务进行管理
### transactionManager(事务管理器)
- Mybatis当中,transactionManager提供了两个实现类,需要实现接口Transaction.
```java
public interface Transaction {
    Connection getConnection() throws SQLException;

    void commit() throws SQLException;

    void rollback() throws SQLException;

    void close() throws SQLException;

    Integer getTimeout() throws SQLException;
}
```
> 工作就是提交(commit),回滚(rollback),关闭(close)数据库的事务.Mybatis提供的两个实现类:jdbcTransaction和ManagedTransaction.对应两个工厂,实现TransactionFactory
之后,生成对应的Transaction对象,所以事务管理器配置有两种方式
```java
<transactionManager type="JDBC"/>
<transactionManagrt type="MANAGED"/>
```
- JDBC使用JdbcTransactionFactory生成的JdbcTransaction对象实现.它是以JDBC的方式对数据库的提交和回滚进行操作.
- MANAGED使用ManagedTransationFactory生成的ManagedTransaction对象实现.它的提交和回滚方法不用任何操作,而是把事务交给容器处理.在默认情况下,它会关闭连接
一些容器并不希望这样,因此需要将closeConnection属性设置为false来阻止它默认的关闭行为.
#### 自定义事务工厂
- 也可以自定义事务工厂,需要实现TransactionFactory
  - 配置
    ```java
    <transactionManager type="类的权限定命"/>
    ```
  - 代码
    ```java
      public class MyTransactionFactory implements TransactionFactory{
      @Override
      public void setProperties(Properties props){}
      @Override
      public Transaction newTransaction(Connection conn){
         return new MyTransaction(conn);
      }
      @Override
      public Transaction newTransaction(DataSource dataSource,TransactionIsolationLevel level,boolean autoCommit){
        return new MyTransaction(dataSource,level,autoCommit);
      }
      }
    ```
- 实现工厂方法后,还需要事务实现类MyTransaction,用于实现Transaction接口.
### 数据源环境
>数据库通过PooledDataSourceFactory.UbpooledDataSourceFactory和JndiDataSourceFactory三个工厂类来提供,前两者产生对应的类对象,而JndiDataSourceFactory
则会根据JNDI的信息拿到外部容器实现的数据库连接对象.三个工厂类,最后生成的产品都会是一个实现了DataSource接口的数据库连接对象.
- 配置方法
```java
  <dataSource type="UNPOOLED"/>
  <dataSource type="POOLED"/>
  <dataSource type="JNDI"/>
```
- UNPOOLED
>采用非数据库池的管理方式,每次请求会打开一个新的数据库的连接,创建比较慢,对一些性能没有很高要求的场合可以使用它.
  - driver 数据库驱动名
  - url 连接数据库的URL
  - username 用户名
  - password 密码名
  - default TransactionIsolationLevel 默认的连接事务隔离级别
- POOLED
>利用"池"的概念将JDBC的Connection对象阻止起来,它开始会有一些空置,并且已经连接好的数据库连接,所以请求时,无需再建立和验证,省去一些时间.还能控制最大连接数,避免过多的连接导致系统瓶颈
  - 除了UNPOOLED下的属性,会有更多属性来配置POOLED的数据源
  - poolMaximumActiceConneions 在任意时间都存在的活动(正在使用)连接数量,默认值是10
  - poolMaximumIdleConnections 在任意时间可能存在的空闲连接数
  - poolMaximumCheckoutTime 在被强制返回之前,池中连接被检出的时间,默认值20s
  - poolTimeToWati 一个底层设置,如果获取连接花费相当长时间,它会给连接池打印状态日志,并重新获取一个链接,默认值20s
  - poolPingQuery 为发送到数据库的侦测查询,用来检验链接是否处在正常工作秩序中,并准备接受请求,默认是"NO PING QUERY SET"这会导致多数数据库驱动失败时带有一个恰当的错误信息.
  - poolPingEnabled 为是否启用侦测查询.若开启,也必须使用一个可执行的SQL语句设置poolPingQuery(最好是一个非常快的SQL),默认值是false.
  - poolPingConnectionsNotUsedFor 配置poolPingQuery使用频度,默认值为0(所有连接每一时刻都被侦测--仅当poolPingEnabled为True时适用)
- JNDI
>它的实现是为了在如EJB或应用服务器这类容器中使用,容器可以集中或在外部配置数据源,然后放置一个JNDI上下文的引用.
  - inirial_context 用来在InitialContext中寻找上下文,如果忽略,那么data_source属性将会直接从InitialContext中寻找.
  - data_source 是引用数据源实例位置上下文的路径.提供inirial_context会在其返回的上下文中查找,没有,则从InitialContext查找
**也支持自定义数据源工厂**
