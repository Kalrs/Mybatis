### 配置信息
- mybatis-config.xml
```java
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0 //EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--类别名-->
     <typeAliases>
         <typeAlias alias="role" type="Lee.role.Role"/>
     </typeAliases>
     <!--数据库环境-->
     <environments default="development">
         <environment id="development">
             <transactionManager type="JDBC"/>
             <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                 <property name="url" value="jdbc:mysql://localhost:3306/login"/>
                 <property name="username" value="root"/>
                 <property name="password" value="wj5201314"/>
             </dataSource>
         </environment>
     </environments>
    <mappers>
        <mapper resource="mapper/Mapper.xml"/>
    </mappers>
</configuration>
```
- Mapper.xml
```java
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0 //EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="mapper.RoleMapper">
        <insert id="insertRole" parameterType="role">
            insert into t_role(role_name,note)values (#{roleName},#{note})
        </insert>
        <delete id="deleteRole" parameterType="long">
            delete from t_role where id = #{id}
        </delete>
        <update id="updateRole" parameterType="role">
            update t_role set role_name=#{roleName},note=#{note} where id= #{id}
        </update>
        <select id="getRole" parameterType="long" resultType="role">
            select id,role_name as roleName,note from t_role where id=#{id}
        </select>
        <select id="findRoles" parameterType="string" resultType="role">
            select id ,role_name as roleName,note from t_role where
            role_name like concat('%',#{roleName},'%')
        </select>
</mapper>
```
- log4j.properties
```java
log4j.rootLogger=DEBUG,stdout
log4j.logger.org.mybatis=DEBUG
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p %d %C: %m%n
```
### POJO类及接口
- Role
```java
public class Role {
    private Long id;
    private  String roleName;
    private  String note;
    /**getter and setter**/
    /** constructor **/
}
```
- RoleMapper
```java
public interface RoleMapper {
        public int  insertRole(Role role);
        public int  deleteRole(Long id);
        public int  updateRole(Role role);
        public Role getRole(Long id);
        public List<Role> findRoles(String roleName);
}

```
- SqlSessionFactory
```java
package Lee.SqlSesFac;
/**
 *  构建SQLSessionFactory。
 *
 */
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import java.io.IOException;
import java.io.InputStream;

public class SqlSessionFactoryUtils{
    private final static Class<SqlSessionFactoryUtils> Lock=SqlSessionFactoryUtils.class;
    private static SqlSessionFactory sqlSessionFactory=null;
    private SqlSessionFactoryUtils(){
    }
    public  static SqlSessionFactory getSqlSessionFactory(){
        synchronized (Lock){
            if(sqlSessionFactory !=null){
                return sqlSessionFactory;
            }
            String resource="mybatis-config.xml";
            InputStream inputStream;
            try{
                inputStream =Resources.getResourceAsStream(resource);
                sqlSessionFactory=new SqlSessionFactoryBuilder().build(inputStream);
            }catch (IOException e){
                e.printStackTrace();
                return null;
            }
            return sqlSessionFactory;
        }
    }
    public static SqlSession openSqlSession(){
        if(sqlSessionFactory==null){
            getSqlSessionFactory();
        }
        return sqlSessionFactory.openSession();
    }
}
```
- Main
```java
package main;
import org.apache.ibatis.session.SqlSession;
import mapper.RoleMapper;
import Lee.role.Role;
import Lee.SqlSesFac.SqlSessionFactoryUtils;
import org.apache.log4j.Logger;
public class ChapterMain {
    public static void main(String[] args) {
        Logger logger=Logger.getLogger(ChapterMain.class);
        SqlSession sqlSession=null;
        try {
            sqlSession=SqlSessionFactoryUtils.openSqlSession();
            RoleMapper roleMapper=sqlSession.getMapper(RoleMapper.class);
            Role role =roleMapper.getRole(1L);
            logger.info(roleMapper.insertRole(new Role(2L,"asd","asda")));
            logger.info(role.toString());
            sqlSession.commit();
        }catch (NullPointerException e){
            e.printStackTrace();
        }finally {
            if (sqlSession!=null){
                sqlSession.close();
            }
        }
    }
}

```
