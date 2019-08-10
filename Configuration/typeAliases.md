> 由于类的全限定名称很长，需要大量的使用的时候，总写那么长的名称不方便。MyBatis允许定义一个简写来代表这个类.别名又分为系统定义别名和自定义别名。在MyBatis中
别名有类TypeAliasRegistry去定义。Mybatis中别名不区分大小写
### 系统定义别名
- TypeAliasRegistry初始化别名
```java
 public TypeAliasRegistry() {
        this.registerAlias("string", String.class);
        this.registerAlias("byte", Byte.class);
        this.registerAlias("long", Long.class);
        this.registerAlias("short", Short.class);
        this.registerAlias("int", Integer.class);
        this.registerAlias("boolean", Boolean.class);
        this.registerAlias("byte[]", Byte[].class);
        this.registerAlias("long[]", Long[].class);
        this.registerAlias("short[]", Short[].class);
        this.registerAlias("int[]", Integer[].class);
        this.registerAlias("integer[]", Integer[].class);
        this.registerAlias("double[]", Double[].class);
        this.registerAlias("float[]", Float[].class);
       /**
          使用registerAlias可以注册别名，一般是通过Configuration获取TypeAliasRegistry类的对象
       
       */
       
 }
```
- Configuration对象对一些常用的配置项的别名
```java
   public Configuration() {
      //事务方式    
      this.typeAliasRegistry.registerAlias("JDBC", JdbcTransactionFactory.class);
      this.typeAliasRegistry.registerAlias("MANAGED", ManagedTransactionFactory.class);
      //数据源类型  
      this.typeAliasRegistry.registerAlias("JNDI", JndiDataSourceFactory.class);
      this.typeAliasRegistry.registerAlias("POOLED", PooledDataSourceFactory.class);
      this.typeAliasRegistry.registerAlias("UNPOOLED", UnpooledDataSourceFactory.class);      
      //缓存策略别名
      this.typeAliasRegistry.registerAlias("PERPETUAL", PerpetualCache.class);
      this.typeAliasRegistry.registerAlias("FIFO", FifoCache.class);
      this.typeAliasRegistry.registerAlias("LRU", LruCache.class);
      this.typeAliasRegistry.registerAlias("SOFT", SoftCache.class);
      this.typeAliasRegistry.registerAlias("WEAK", WeakCache.class);    
      //语言驱动类别名
      this.typeAliasRegistry.registerAlias("XML", XMLLanguageDriver.class);
      this.typeAliasRegistry.registerAlias("RAW", RawLanguageDriver.class);
      //日志类别名
      this.typeAliasRegistry.registerAlias("SLF4J", Slf4jImpl.class);
      this.typeAliasRegistry.registerAlias("COMMONS_LOGGING", JakartaCommonsLoggingImpl.class);
      this.typeAliasRegistry.registerAlias("LOG4J", Log4jImpl.class);
      this.typeAliasRegistry.registerAlias("LOG4J2", Log4j2Impl.class);
      this.typeAliasRegistry.registerAlias("JDK_LOGGING", Jdk14LoggingImpl.class);
      this.typeAliasRegistry.registerAlias("STDOUT_LOGGING", StdOutImpl.class);
      this.typeAliasRegistry.registerAlias("NO_LOGGING", NoLoggingImpl.class);
      //动态代理别名
      this.typeAliasRegistry.registerAlias("CGLIB", CglibProxyFactory.class);
      this.typeAliasRegistry.registerAlias("JAVASSIST", JavassistProxyFactory.class);
 }
```
### 自定义别名
> 通过TypeAliasesRegistry类的register方法注册，也可以采用配置文件或者扫描的方式来自定义它。
- 通过配置文件
```java
   <typeAliases>
  <typeAlias alias="role" type="Lee.pojo.Role"/>
  <typeAlias alias="User" type="Lee.pojo.User"/>
  <typeAliases/>
```
- 通过扫描
```java
     <typeAliases>
       <package name="Lee.pojo"/>
     <typeAliases/>
     <!--
      MyBati会扫描此包中的类，将其第一个字母变为小写作为其别名，比如Role变为role
      当扫描两个包，可能会产生重名，可以使用Mybatis提供的注解@Alias("User3")进行区分
     -->
```
