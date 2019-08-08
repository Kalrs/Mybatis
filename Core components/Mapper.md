>映射器是MaBatis中最重要、最复杂的组件，它由一个接口和对应的XML文件或者注解组成。可以配置以下内容。
- 描述映射规则
- 提供SQL语句,并可以配置SQL参数类型,返回类型、缓存刷新等信息
- 配置缓存
- 提供动态SQL

### 使用XML实现映射器
- 用XML定义映射器分为两个部分:接口和XML。
```java
package mapper;

import entity.User;

public interface UserMapper {
    public User getStudnet(String id);
}
```
```java
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0 //EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="mapper.UserMapper">
    <select id="getUser" parameterType="long" resultType="entity.User">
        select id,role_name as rolename,note from t_role where id =#{id}
    </select>
</mapper>
```
#### 元素描述
- mapper:namespace所对应的是一个接口的全限定名,Mybatis上下文可以通过它找到对应的接口
- select:属性id标识这条SQL,属性parameterType="long"说明传递给SQL的是一个long型的参数，而resultType表示返回的是一个role类型的返回值，及前面mybits-config.xml配置的别名。
- 这条SQL中的#{id}表示传递进去的参数
**这里采用的是一种被称为自动映射的功能，MyBatis在默认情况下提供自动映射，只要SQL返回的列名能和POJO对应起来即可**
