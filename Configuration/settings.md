#### settings是MyBatis中最复杂的配置，他能深刻影响MyBatis底层的运行，但是大部分情况下使用默认值即可。
- 一些常用的设置
```java
     <settings>
        <setting name="cacheEnabled" value="true"/>   <!--影响所有映射器中配置缓存的全局开关-->
        <setting name="lazyLoadingEnabled" value="true"/> <!--延迟加载的全局开关-->
        <setting name="aggressiveLazyLoading" value="true"/><!--是带有延迟加载属性的对象完整加载  false按需-->
        <setting name="autoMappingBehavior" value="PARTIAL"/> <!--value值 none表示取消自动映射 PARTIAL表示只会自动映射 FULL会自动映射任意的结果集-->
        <setting name="mapUnderscoreToCamelCase" value="true"/> <!--是否开启自动驼峰命名规则的映射 A_COLUMN aColumnD的类型映射-->
        <setting name="defaultExecutorType" value="SIMPLE"/><!--配置默认的执行器 SIMPLE是普通的执行器;REUSE会重用预处理语句(prepared statements)
                                                               BATCH执行器将重用语句并执行批量更新-->
    </settings>

```
