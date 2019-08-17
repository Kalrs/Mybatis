>databaseIdProvider元素主要是支持多种不同厂商的数据库,提供数据库的一致性
### 使用系统默认的databaseIdProvider
- 配置
```java
  <databaseIdProvider type="DB_VENDOR">
      <property name="Oracle" value="oracle"/>
      <property name="MySQL" value="mysql"/>
      <property name="DB2" value="db2"/>
  </databaseIdProvider>
```
- 改造映射器的SQL
```java
  <select id="getRole" parameterType="long"
    resultType="role" databaseId="oracle">
    select id from t_role where id =#{id}
  <select>
   <select id="getRole" parameterType="long"
    resultType="role" databaseId="oracle">
    select id from t_role where 1=1 and id =#{id}
  <select>
  /*
    通过打印日志可以看出SQL被标识为哪一个数据库的SQL
  */
```
** 当databaseIdProvidertype属性被配置时,系统优先取到和数据库一致的SQL.如果没有,则取没有databaseId的SQL,可以把它当作默认值.如果还是取不到,则会抛出异常,说明无法匹配到对应的SQL.
### 不使用系统规则
>也可以使用自定义的规则,但是必须实现MyBatis提供的接口DatabaseIdProvider
