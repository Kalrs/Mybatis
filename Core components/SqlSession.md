>在MyBatis中,SqlSession是其核心接口。在MyBatis中有两个实现类,DefaultSqlSession(单线程使用)和SqlSessionManage(多线程使用)。SqlSession的作用类似与一个JDBC中的Connection对象，代表一个连接资源的启用。
#### 作用
- 获取Mapper接口
- 发送Sql给数据库
- 控制数据库事务
