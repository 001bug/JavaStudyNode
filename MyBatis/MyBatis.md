### MyBatis简单介绍
**官方文档:https://mybatis.net.cn/** 
Maven仓库: https://mvnrepository.com/  搜索mybatis然后点进去,查看坐标
中文手册: https://mybatis.org/mybatis-3/zh/index.html

**工作流程**
传统crud工作流程
![](assest/Pasted%20image%2020240715112115.png)
MyBatis工作流程
![](assest/Pasted%20image%2020240715112347.png)
先看mybatis框架,然后看java操作右边部分

### MyBatis的简单使用
##### MyBatis的增删改查的入门

1.项目创建,新的maven项目管理方式,有父项目和子项目
创建的时候不要去选择模块,而是直接创建然后删除src文件
![](assest/Pasted%20image%2020240715150232.png)

2.在父项目引入依赖
```
<dependencies>
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>版本号自己选</version>
	</dependency>
	<dependency>
		<groupId>org.mybatis</groupId>
		<artifactId>mybatis</artifactId>
		<version>?</version>
	</dependency>
	<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.12</version>
	</dependency>
</dependencies>
```
 还要引入处理项目导到目标文件下的依赖
```
 <build>  
    <resources>  
        <resource>  
            <directory>src/main/java</directory>  
            <includes>  
                <include>**/*.xml</include>  
            </includes>  
        </resource>  
        <resource>  
            <directory>src/main/resources</directory>  
            <includes>  
                <include>**/*.xml</include>  
                <include>**/*.properties</include>  
            </includes>  
        </resource>  
    </resources>  
</build>
```

3.创建子项目,过程中就修改个项目名字,然后啥都不用点

4.配置src/resources/mybatis-config.xml文件(必须放在resources文件中)
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/><!--事务管理器-->
      
      <dataSource type="POOLED"><!--数据源-->
        <property name="driver" value="com.mysql.jdbc.Driver"/><!--驱动-->
        <!--1.jdbc:mysql协议
	        2.127.0.0.1:3306:指定链接mysql的ip+port
	        3.mybatis: 链接的DB
	        4.useSSL=true 表示安全连接
	        5.&amp; 转义表示& 防止解析错误 
	        6.useUnicode=true: 使用unicode防止编码错误
	        7.characterEncoding=UTF-8 指定使用utf-8防止中文乱码-->
        <property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
        <property name="username" value="自己mysql的用户名"/>
        <property name="password" value="密码"/>
      </dataSource>
      
    </environment>
  </environments>
  <mappers>
    <mapper resource="mapper类的路径"/>
  </mappers>
</configuration>
```
对config.xml配置文件的解析mapper类的全路径可以通过对mapper文件右键->copy path->path from source root

5.创建一个javabean,   该javabean的属性一定要和表的字段形成对应关系

6.创建一个名位mapper的包然后 , 在该包下写增删改查的接口
 接口样例模版 xxx是javabean的类名
```
public interface xxxMapper{
	public void addxxx(xxx xxx)
}
```
 接着在mapper包下创建xxx.xml文件, 然后引入以下代码
```
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="接口的全类名">
  <insert id="addxxx" parameterType="传入的类型(addxxx的参数)"><!--每个操作不一样,去官方查-->
	  <!--insert的sql语句,非关键字带上``,防止解析错误--> 
	  INSERT INTO `表名` () VALUES (#{javabean的字段名})
  </insert>
  <delete id="delxxx" parameterType="传入的类型(对象)">
	  DELETE FROM `monster` WHERE id = #{id}
  </delete>
  <update id="updatexxx" parameterType="传入的类型">  
	  update sql语句
  </update>
  <select id="getxxx" resultType="传入类型">  
    SELECT * FROM `monster` WHERE id = #{id}
  </select>
  <select id="findAllMonster" resultType="Monster">  
    SELECT * FROM `monster`
  </select>
</mapper>
```
对该文件的解释
	该文件可以去实现对应的接口的方法
	 namespace 指的是xml文件和接口对应,这个接口是在mapper包下的接口
	 细节: 如果需要获得自增的key值,那么可以在标签中加seGeneratedKeys="true" keyProperty="id"

7.写MyBatisUtils 工具类
```
public class MyBatisUtils {
	static{
		try{
			String resource = "mybatis-config.xml";
		   InputStream resourceAsStream = Resources.getResourceAsStream(resource);
		   sqlSessionFactory = new SqlSessionFactoryBuilder().build(
					resourceAsStream);	
		} catch (IOException e){
			e.printStackTrace();
		}
	}
	public static SqlSession getSqlSession() {  
	    return sqlSessionFactory.openSession();  
	}
}
```
工具类的注意事项
Resources一定要是来自mybatis的 , 涉及到io要看看收否加载到目标文件目录下

8.使用MyBatis , 模版样例
```
public class 类名{
	private SqlSession sqlSession;//这些都是必备的
	private MonsterMapper monsterMapper;//
	@Before //Before注解能够让类进行实例化的时候先执行这个语句
	public void init(){
		sqlSession = MyBatisUtils.getSqlSession();
		monsterMapper = sqlSession.getMapper(MonsterMapper.class);
	}
	public void addMonster() {//添加实例,然后删除,更新操作都差不多  
		...实例化对象  
	    xxxMapper.addxxx(xxx xxx);
	    //如果是增删改,则需要下面的代码进行确认
	    if(sqlSession!=null){
		    sqlSession.commit();
		    sqlSession.close();
	    }
	}
}
```

