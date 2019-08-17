>在typeHandler中,分为jdbcType(定义数据库类型)和javaType(定义java类型),typeHandler承担jdbcType和javaType之间的相互转换.和别名一样,存在系统定义和自定义typeHanler
系统提供的typeHandler能覆盖大部分场景的要求,但有些情况是不够的,比如枚举类.
### 系统定义的typeHandler
>大部分情况下,无需显示地声明jdbcType和javaType,或者用typeHandler去制定对应的typeHandler,Mybatis系统会自己探测.typeHandler都需要实现接口org.apache.ibatis.type.TypeHandler.
#### TypeHandler.java
```java
  public interface TypeHandler<T> {
    void setParameter(PreparedStatement var1, int var2, T var3, JdbcType var4) throws SQLException;

    T getResult(ResultSet var1, String var2) throws SQLException;

    T getResult(ResultSet var1, int var2) throws SQLException;

    T getResult(CallableStatement var1, int var2) throws SQLException;
}
```
- setParameter方法,是使用typeHandler通过PrepareStatement对象进行设置SQL参数的时候使用的具体方法.var2是参数在SQL的下标,var3是参数,var4是数据库类型

