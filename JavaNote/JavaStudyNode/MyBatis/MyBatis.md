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
##### 1MyBatis的增删改查的入门

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

4.配置src/resources/mybatis-config.xml文件(必须放在resources(main目录下)文件中)
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/><!--事务管理器-->
      
      <dataSource type="POOLED"><!--数据源-->
        <property name="driver" value="com.mysql.cj.jdbc.Driver"/><!--驱动-->
        <!--1.jdbc:mysql协议
	        2.127.0.0.1:3306:指定链接mysql的ip+port
	        3.mybatis: 链接的DB
	        4.useSSL=true 表示安全连接
	        5.&amp; 转义表示& 防止解析错误 
	        6.useUnicode=true: 使用unicode防止编码错误
	        7.characterEncoding=UTF-8 指定使用utf-8防止中文乱码
	        8.注意数据库和JDBC所在的时差问题-->
        <property name="url" value="jdbc:mysql://127.0.0.1:3306/数据库名?serverTimezone=Asia/Shanghai&amp;useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
        <property name="username" value="自己mysql的用户名"/>
        <property name="password" value="密码"/>
      </dataSource>
      
    </environment>
  </environments>
  <mappers>
    <mapper resource="mapper.xml的全路径"/>
  </mappers>
</configuration>
```
对config.xml配置文件的解析mapper.xml的全路径可以通过对mapper.xml文件右键->copy path->path from source root

5.创建一个javabean,   该javabean的属性一定要和表的字段形成对应关系

6.创建一个名位mapper的包然后 , 在该包下写增删改查的接口(Mapper接口)
 接口样例模版 xxx是javabean的类名
```java
public interface xxxMapper{
	public void addxxx(xxx xxx);
	public void delxxx(Integer id);
	public void updateMonster(Monster monster);
	public Monster getMonsterById(Integer id);
	public List<Monster> findAllMonster();
}
```
 接着在mapper包下创建xxx.xml文件, 然后引入以下代码
```xml
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="接口的全类名">
  <insert id="addxxx" parameterType="传入的类型(addxxx的参数)全类名"><!--每个操作不一样,去官方查-->
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
	 细节: **如果需要获得自增的key值,那么可以在标签中加seGeneratedKeys="true" keyProperty="id"**
	 如果需要简写javabean的全类名,需要在mybatis-config.xml配置
```xml
<typeAliases>  
    <typeAlias type="javabean全类名" alias="要被替代的别名"/>  
</typeAliases>
```


7.写MyBatisUtils 工具类 ^ede5a4
```java
public class MyBatisUtils {
	private static SqlSessionFactory sqlSessionFactory;
	static{
		try{
			String resource = "mybatis-config.xml";
		   InputStream resourceAsStream = Resources.getResourceAsStream(resource);
		   sqlSessionFactory = new SqlSessionFactoryBuilder().build(
					resourceAsStream);//反射+io	
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

8.使用MyBatis , 模版样例 ^95c0bd
```java
public class 类名{
	private SqlSession sqlSession;//这些都是必备的
	private xxxMapper xxxMapper;//
	@Before //Before注解能够让类进行实例化的时候先执行这个语句
	public void init(){
		sqlSession = MyBatisUtils.getSqlSession();
		xxxMapper = sqlSession.getMapper(xxxMapper.class);
	}
	public void addxxx() {//添加实例,然后删除,更新操作都差不多  
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

##### 2MyBatis日志输出
* 我们先理解一下,什么是日志, 日志用于记录 SQL 语句的执行情况、参数、执行时间以及执行过程中可能发生的错误等信息。**可以查看MyBatis底层的原生的sql**

日志文档:https://mybatis.org/mybatis-3/zh/logging.html
配置文档:https://mybatis.org/mybatis-3/zh/configuration.html#settings

在使用配置之前要在resource目录下的mybatis-config.xml加入配置设置
```xml
<settings>
	<setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```
注意这个配置代码要放在前面.
* 加入日志功能


### 实现MyBatis底层机制
#### 1.底层组件的调用关系
![](assest/Pasted%20image%2020240716110728.png)
[对MyBatis结构图的解释](#^ede5a4),这个链接是实际代码,应对SqlSession的获得,需要解析xml配置文件
1.mybatis的核心配置文件: mybatis-config.xml 全局配置文件,全局只能有一个这样的配置文件
xxxMapper.xml 配置SQL , 可以有多个
2.通过mybatis-config.xml得到SqlSessionFactory的
3.SqlSession通过SqlSessionFactory得到, ==SqlSession的底层是Executor(执行器)==
Executor(执行器源码)
![](assest/Pasted%20image%2020240716142118.png)
Executor执行器是个接口,他被很多类实现了,其中重要是BaseExecutor,还有个带**缓存**的CachingExecutor执行器
查看Executor接口,发现有很多相应实现sql的接口
![](assest/屏幕截图%202024-07-16%20142447.png)
然后再查看BaseExecutor
![](assest/屏幕截图%202024-07-16%20142506.png)
接下来要看MappedStaement怎么实现
![](assest/Pasted%20image%2020240716143427.png)
**看源码就会发现,MappedStatement封装了很多的在配置文件中标签的参数**
#### 2.MyBatis的底层设计思路
![](assest/Pasted%20image%2020240716150132.png)
[此流程的对应模版](#^ede5a4) 这里打算在Session 中获取全局配置文件的信息, 源码是在SqlFactorySession中获取配置信息
#### 3.代码实现(只显示重要代码片段)
##### 1.自定义xml配置文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<database>  
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>  
    <property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?serverTimezone=Asia/Shanghai&amp;useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>  
    <property name="username" value="root"/>  
    <property name="password" value="hsp"/>  
</database>
```
要非常小心时区问题
##### 2.写读取xml配置文件的方法
1.因为要读取文件,那么一定会用到io方面的,但因为xml文件的工作目录随随着用户不同会变化,所以要考虑通过类加载器动态的获取相应的xml的io(一般工作的时候,xml文件会放到类路径的相对路径下面)
```java
InputStream stream = loader.getResourceAsStream(resource);
```
[resource是类路径的相对路径](../Java/类路径和Java相对路径.md)\

2.xml解析获取document根元素(build) ^e8bb23
```java
//加载配置xxx.xml 获取到对应的InputStream  
InputStream stream = loader.getResourceAsStream(resource);  
//解析xxx.xml  => dom4j  
SAXReader reader = new SAXReader();  
Document document = reader.read(stream);  
//获取到xxx.xml 的根元素 <database>  
Element root = document.getRootElement();  
//解析root元素，返回Connection => 单独的编写一个方法
```

3.拿到根元素,然后去遍历,并且通过选择性的语句去获取xml中配置文件的信息(node就是root,)(evalDataSource)
```java
for (Object item : node.elements("property")) {  
    Element i = (Element) item;//i 就是 对应property节点  
    String name = i.attributeValue("name");  
    String value = i.attributeValue("value");  
  
    //判断是否得到name 和 value  
    if (name == null || value == null) {  
        throw new RuntimeException("property 节点没有设置name或者value属性");  
    }  
    switch (name) {  
        case "url":  
            url = value;  
            break;  
        case "username":  
            username = value;  
            break;  
        case "driverClassName":  
            driverClassName = value;  
            break;  
        case "password":  
            password = value;  
            break;  
        default:  
            throw new RuntimeException("属性名没有匹配到...");  
    }  
}
```

4.通过第三步获得的信息,然后用jdbc进行连接数据库,这里MyBatis有用数据库连接池.
```java  
connection = DriverManager.getConnection(url,username,password);
```
##### 3.写执行器Executor
源码是基于接口实现了很多类去实现sql的 .  这里直接开发一个简单的基于Executor实现的BaseExecutor

1.在此之前,学习一个**快速开发javabean**的包
相关pom依赖
```java
<dependency>  
    <groupId>org.projectlombok</groupId>  
    <artifactId>lombok</artifactId>  
    <version>1.18.4</version>  
</dependency
```
* @Getter 就会给所有属性 生成对应的getter  
* @Setter 就会给所有属性 生成对应的setter  
* @ToString 生成 toString...  
* @NoArgsConstructor 生成无参构造器  
* @AllArgsConstructor 生成要给全参构造器  
* ==@Data 注解==

2.接口设计采用泛型编程 , 增强扩展性
```java
public interface Executor {  
    //泛型方法  
    public <T> T query(String statement, Object parameter);  
}
```

3.基于接口实现BaseExecutor
* 查询操作,实现类没有采用反射机制(传入参数是 sql语句,和属性 parameter)
```java
try {  
    pre = connection.prepareStatement(sql);  
    //设置参数, 如果参数多, 可以使用数组处理.  
    pre.setString(1, parameter.toString());  
    set = pre.executeQuery();//set是JDBC中ResultSet的变量名  
    //把set数据封装到对象-monster  
    //这里简化操作,默认返回一条记录  
    //完善的写法是一套反射机制.  
    Monster monster = new Monster();  
      
    //遍历结果集, 把数据封装到monster对象  
    while (set.next()) {  
        monster.setId(set.getInt("id"));  
        monster.setName(set.getString("name"));  
        monster.setEmail(set.getString("email"));  
        monster.setAge(set.getInt("age"));  
        monster.setGender(set.getInt("gender"));  
        monster.setBirthday(set.getDate("birthday"));  
        monster.setSalary(set.getDouble("salary"));  
    }  
    return (T) monster;  
  
} catch (Exception throwables) {  
    throwables.printStackTrace();  
} finally {  
    try {  
        if (set != null) {  
            set.close();  
        }  
        if (pre != null) {  
            pre.close();  
        }  
        if (connection != null) {  
            connection.close();  
        }  
    } catch (Exception throwables) {  
        throwables.printStackTrace();  
    }  
}
```
源码是有缓存机制的.这里跳过了

4.在SqlSession方法中让Executor跟参数建立联系
```java
public <T> T selectOne (String statement,Object parameter){  
    return executor.query(statement, parameter);//A是executor  
}
```
连接方式,就是独立出一个类 , 然后**申明两个变量(A和B)** , 用一个方法去结合他们
![](assest/屏幕截图%202024-07-16%20201117.png)
selectOne被覆盖了很多种,这里采用String , Obeject这一种
![](assest/屏幕截图%202024-07-16%20201704.png)
这里和写的源码一样,大意也是在SqlSession让configlation和executor连接

##### 4.实现Mapper机制
思路示意图
![](assest/Pasted%20image%2020240716202625.png)
实现Mapper接口声明方法,然后对应的xml文件会自动的把接口方法给实例化了
在实现之间声明一下,这里的mapper.xml文件放在resources文件下,因为resource文件下的文件直接被加载到类的相对路径之下. 原本的MyBatis是在全局配置文件配置一下,然后解析,就能知道mapper.xml的相对类路径(省很多代码) 

1.xxx接口方法
```java
public interface MonsterMapper {  
    public Monster getMonsterById(Integer id);  
}
```
对应的xxxMapper.xml文件
```java
<?xml version="1.0" encoding="UTF-8" ?>  
<mapper namespace="com.hspedu.entity.Monster">  
    <select id="getMonsterById" resultType="com.hspedu.entity.Monster">  
        select * from monster where id=?  
    </select>  
</mapper>
```

2.开发MapperBean
接下来是重中之重, 声明的接口怎么和xml相互连接映射起来
![](assest/Pasted%20image%2020240716204717.png)

**如图可知它是通过一个名为MapperBean来联系接口和xml的.JavaBean封装接口方法的信息,xxxmapper.xml的各种信息** ^edb9fc

?传统Java开发模式通过实现类来继承接口并实现方法.
?MyBatis通过MapperBean和XML文件配置实现接口与方法的映射和调用。MapperBean作为连接接口和XML配置的关键对象，**记录接口方法信息以支持运行时调用**。
最终目的可以让1.MyBatis减少样板代码2. SQL 与 Java 代码分离
* 写MapperBean中的Function(get和set方法省略了,要补全)
```java
public class Function {  
    private String sqlType;  
    private String funcName;  
    private String sql;  
    private Object resultType;  
    private String parameterType;
}
```
* 写MapperBean(封装Mapper信息) (get和set方法省略了,要补全)
```java
public class MapperBean {  
    private String interfaceName;//接口全类名  
    private List<Function> functions;//保存接口中的方法
}
```
* 封装MapperBean
```java
public MapperBean readMapper(String path){  
    MapperBean mapperBean=new MapperBean();  
    //获取到mapper.xml对应的流  
    try {  
        InputStream stream = loader.getResourceAsStream(path);  
        SAXReader reader = new SAXReader();  
        Document document = reader.read(stream);  
        Element root = document.getRootElement();  
        String namespace=root.attributeValue("namespace").trim();  
        mapperBean.setInterfaceName(namespace);//设置interface接口全路径  
        Iterator rootIterator=root.elementIterator();  
        List<Function> list = new ArrayList<>();//保存接口下所有的function的方法信息  
        while(rootIterator.hasNext()){  
            Element e=(Element) rootIterator;  
            Function function = new Function();  
            String sqlType=e.getName().trim();  
            String funcName=e.attributeValue("id").trim();  
            String resultType=e.attributeValue("resultType").trim();  
            String sql=e.getText().trim();  
            //开始封装  
            function.setSql(sql);  
            function.setFuncName(funcName);  
            function.setSqlType(sqlType);  
            //resultType现在不知道他是什么类型,在运行的时候才能知道他是什么类型,所以这里可以使用反射机制处理  
            Object newInstance = Class.forName(resultType).newInstance();  
            function.setResultType(newInstance);  
            list.add(function);//将封装好的信息放入集合中  
        }  
        mapperBean.setFunctions(list);//要将所有的方法信息传入进去  
    } catch (Exception e) {  
        e.printStackTrace();  
    }  
    return mapperBean;  
}
```
1. 这里先采用[xml解析获取mapper的配置信息](#^e8bb23) , 注意这里做了简化,我把Mapper映射文件默认放在类路径的[相对路径](../Java/类路径和Java相对路径.md)下,就不用像原生的MyBatis那样要通过解析全局配置文件,然后获得xxxmapper.xml的类路径的相对路径
2. 循环遍历,封装[Function](#^edb9fc) 然后方进一个集合中,因为可能有多个接口,以及xxxmapper.xml,最后返回一个具有多个mapper.xml信息和多个mapper接口信息的信息体. 
##### 5.实现[动态代理](设计模式)MapperProxy类
在一开始的Executor是直接通过JDBC调用MySQL , 这里进行改进,通过一个名为MapperProxy代理类,然后动态的生成代理对象,然后去调用**Executor中的方法**
```java
public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  
    MapperBean mapperBean = hspConfiguration.readMapper(this.mapperFile);  
    //判断是否是xml对应的接口  
    if(!method.getDeclaringClass().getName().equals(mapperBean.getInterfaceName())){  
        return null;  
    }  
    //取出mapperBean的functions  
    List<Function> functions=mapperBean.getFunctions();  
    //判断mapperBean解析对应的接口中的方法是否有方法  
    if(null!=functions&&0!=functions.size()){  
        for(Function function: functions){  
            if(method.getName().equals(function.getFuncName())){  
                //如果我们当前的function 要执行的sqlType  
                //我们就去执行selectOne  
                //这里做了简化,如果要执行的select,就对应执行selectone  
                //在这里,HspSqlsession只写了一个selectone,实际的源码有很多执行select的方法  
                if("select".equalsIgnoreCase(function.getSqlType())){  
                    return hspSqlSession.selectOne(function.getSql(),String.valueOf(args[0]));  
                }
                //源码这里写了很多不同的语句  
            }  
        }  
    }  
    return null;  
}
```
首先,要实现动态代理, 目的是为了让接口的方法扩展功能(解析xml,实现mapper接口),用动态代理对象调用
```java
public <T> T getMapper(Class<T> clazz){  
    return (T) Proxy.newProxyInstance(clazz.getClassLoader(),new Class[]{clazz},new HspMapperProxy(this, hspConfiguration,clazz));  
}
```
动态代理的使用
### 原生API的调用

```java
int insert = sqlSession.insert(xxx.xxx.xx,数据格式);
int delete = sqlSession.delete(一个方法的全类名,参数);
int update = slqSession.update(方法的全类名,参数);
//selsect也差不多,但是它有太多的方法,返回值可能不太同,但参数是差不多的
```
如果是增删改,需要加下面的代码
```java
if(sqlSession!=null){
	sqlSession.commit();
	sqlSession.close();
}
```
这些方法被用来执行mapper接口中的方法。返回的都是影响行数,如果是增删改,需要提交事务,括号中的形参很接口中的形参保持一致,总的来说,最终还是要到mapper.xml读里面的配置sql语句

### 注解的方式进行crud
1.还是要创建一个xxxmpper包,然后省去了配置xml文件,也是需要xxxmapper接口
xxx.interface
```java Xxx.interface
@Insert("INSERT INTO `表名` () VALUES (#{javabean的字段名})")
public void addXxx(Xxx xxx);
@Delete("sql")
public void delXxx(参数);
``` 
2.需要在全局配置文件配置
```xml
<mappers>
	<mapper class="xxx.interface的全类名"/>
</mappers>
```
3.使用,还是要拿到SqlSession,本质要通过它拿到实例化接口(动态代理)
```java
public class 类名{
	private SqlSession sqlSession;//这些都是必备的
	private xxxMapper xxxMapper;//
	@Before //Before注解能够让类进行实例化的时候先执行这个语句
	public void init(){
		sqlSession = MyBatisUtils.getSqlSession();
		xxxMapper = sqlSession.getMapper(xxxMapper.class);
	}
	public void addxxx() {//添加实例,然后删除,更新操作都差不多  
		...实例化对象  
	    xxxMapper.addxxx(xxx xxx);
	    ...实例化对象.getId();//需要在配置中设置或者注释中写
	    //如果是增删改,则需要下面的代码进行确认
	    if(sqlSession!=null){
		    sqlSession.commit();
		    sqlSession.close();
	    }
	}
}
```
在接口方法上写以下注释
`@Options(useGeneratedKeys=true,keyProperty="id",keyColumn="id")`
1.useGeneratedKeys=true表示返回自增的值
2.keyProperty="id" 自增值对应的对象属性是哪一个
3.keyColum="id" 自增值对应表的字段 总的来说要看表的字段和javabean中的字段
### 配置文件和XML映射器
##### 配置文件
1.properties
这些属性可以在外部进行配置，并可以进行动态替换。简单点说就是在点xml文件中使用`<properties>`标签可以获取外部.properties中的配置信息
* 例如可以将红框里的properties的value值改成`"${driver}",${url}...`
![](assest/屏幕截图%202024-07-18%20091634.png)
* 然后在resource文件下写一个jdbc.properties文件
```properties jdbc.properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://127.0.0.1:3306/数据库名?serverTimezone=Asia/Shanghai&amp;useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8
jdbc.username=
jdbc.password=
```
* 最后在`<configuration>`下直接引入`<properties resource="jdbc.properties"/>` resource表示放在类路径的相对路径根路径下

2.setting(全局参数)
这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。
在`configuration`直属目录下添加![](assest/Pasted%20image%2020240718093838.png)

3.类型别名（typeAliases）
![](assest/Pasted%20image%2020240718094715.png)
这样配置,那么在xxxmapper接口中要用到javabean的就不用写全类名了

4.类型处理器（typeHandlers）
MyBatis 在设置预处理语句（PreparedStatement）中的参数或从结果集中取出一个值时， 都会用类型处理器将获取到的值以合适的方式转换成 Java 类型。MyBatis已经有默认的类型处理器,符合大部分场景

5.环境配置（environments）

6映射器（mappers）
告诉 MyBatis 到哪里去找到sql语句
```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
```
```xml
<!-- 使用完全限定资源定位符（URL） -->
<mappers>
  <mapper url="file:///var/mappers/AuthorMapper.xml"/>
  <mapper url="file:///var/mappers/BlogMapper.xml"/>
  <mapper url="file:///var/mappers/PostMapper.xml"/>
</mappers>
```
```xml
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
```
```xml
<!-- 将包内的映射器接口实现全部注册为映射器sql语句中的id直接写类名,也可以在Mapper.xml的namespace中写接口的全类名-->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```
注意,无论是注释的方式还是xml的方式,都需要配置映射器,而且最常用的是包的形式
##### Mapper映射文件
1.参数类型（`parameterType`）和 `resultType`
* 传入简单类型
* 传入POJO类型(多数据), 比如查询时有多个筛选条件
* 传入String , 只能用`${}`来接受参数`#{}`
```sql
SELECT * FROM `monster` WHERE `name` LIKE `%牛%`--模糊查询的sql
```
然后配置sql语句
```xml
<select id=`findMonsterByName` parameterType="String" resultType="Monster">
	SELECT * FROM `monster` WHERE `name` LIKE `${name}`
</select>
```
POJO类型
![](assest/Pasted%20image%2020240718104700.png)
 Monster 类型的参数对象传递到了语句中，会自动查找 id、name 属性，然后将它们的值传入**预处理语句的参数**中
 * 传入HashMap(重点)
1.先在Mapper接口添加方法,参数改为map
```java
public List<Monster> FindMonsterByMap(Map<String,Object> map);
```
2.然后在xml配置文件中参数类型(parameterTyep)改为map
```xml
<select id="相应方法的全类名" parameterType="map" resultType="返回结果">
	SELECT * FROM `表名` WHERE `id` = #{id}
</select>
```
解释,如果传入的是map,那么预处理语句中的参数`id`,在map中也要有对应的key值为`id`, 还有可能遇到的**是返回的传入和返回的结果都是map**
首先要改一下Mapper接口中的方法
```java 
public List<Map<String,Object>> FindPrameterMap_Returnmap(Map<String,Object> map);
```
配置xml(parameter是map和return也是map的情况)
```xml
<select id="(上面)方法的全类名" parameterType="map" resultType="map">
	SELECT * FROM `表名` WHERE `id` = #{id}
</select>
```

2.resultMap(结果集映射)
介绍:当**数据库列**和java属性不一致时, 通过resultMap进行映射, 从而解决实体类属性和表字段不一致的问题,实体类是上面提到过的javabean ^24d624
```xml
<resutlMap id="随意" type="结果映射类型">
	<result property="实体类的属性名" column="表的字段名" />
</resultMap>
```
然后再把xml中sql语句的配置的resultType属性改成resultMap , 并且select id改成resultMap的id
```xml
<select id="方法的全类名"  resutlMap="上面resultMap的id">
	SELECT * FROM `表名`
</select>
```
`type="xxx"` 叫结果映射类型 `resultMap id`叫结果映射id `property`java对象的属性名
`column`数据库表的字段名

重要的标签`<association>`用于将查询结果中的字段映射到嵌套的对象属性。这通常用于[处理一对一或多对一关系的复杂映射。](#^f82f88)
- `property`：指定 Java 对象中的属性名，这个属性是另一个对象。
- `javaType`：指定嵌套对象的类类型（可选）。
- `resultMap`：引用一个 `resultMap`，用于嵌套对象的字段映射。

==可以使用MySQL的别名来处理实体类的属性名和表的字段名不一致的问题,但是复用性太差.不推荐使用==
##### 映射关系
映射关系的定义:映射关系的定义指的是 Java 对象与数据库表之间的映射规则。下面所介绍的都是级联查询的背景下的映射关系 
```sql 级联查询的表背景
CREATE TABLE person(
	`id` INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(32) NOT NULL DEFAULT '',
	card_id INT ,
	FOREIGN KEY(card_id) REFENGES idencard(id)
);
CREATE TABLE idencard(
	id INT PRIMARY KEY AUTO_INCREMENT,
	card_sn VARCHAR(32) NOT NULL DEFAULT ''
)
```
IdenCard.java
```java IdenCard.java
public class IdenCard{
	private Integer id;
	private String card_sn;//
	...
}
```
Person.java
```java
public class Person{
	private Integer id;
	private String name;
	private IdenCard card;//这里一定要写上
	...
}
```
IdenCardMapper.interface
```java
public interface IdenCardMapper{
	//通过id查找身份,注意,id 和身份在同一个表
	public IdenCard getIdenCardById(Integer id);
}
```
IdenCardMapper.xml配置文件
```xml
<mapper namespace ="接口名"><!--加了这个Mapper方法ID就不用写方法全名-->
	<select id="getIdenCardById" parameterType="Integer" resultType="IdenCard">
	SELECT * FROM `idencard` WHERE `id` = #{id}
	</select>
</mapper>
```
创建PersonMapper接口
```java
public interface PersonMapper{
	//通过Person的id获取到Person,包括这个Person关联的IdenCard对象[级联查询]
	public Person getPersonById(Integer id);//不在一个表上
}
```
创建PersonMapper.xml配置文件
```xml
<mapper namespace ="接口名"><!--加了这个Mapper方法ID就不用写方法全名-->
	<select id="getPersonById" parameterType="Integer" resultType="Person">
	SELECT * FROM `idencard` WHERE `id` = #{id}
	</select>
</mapper>
```
结果类型(resultType)很容易想到要填Person,但在调用Person getPersonById()返回的结果card是空的
**映射关系一对一**(assonciation),级联映射
实现一对一的关系的方法(举个例子,我们要在上面背景下,通过id查找身份证)
创建mapper.xml和接口

解决方案一 ^f82f88
* sql语句上使用多表查询,自定义[resultMap](#^24d624),解决返回结果的封装
```xml
<resultMap id="PersonResultMap" type="Person">
	<result property="id" column="id"/>
	<result property="name" column="name"/>
	<!--association,一个复杂类型的关联-->
	<association property="card" javaType="IdenCard">
		<result property="id" column="id"/>
		<result property="card_sn" column = "card_sn"/>
	</association>
</resultMap>
<select id="getPersonById" parameterType="Integer" resultMap="PersonResultMap">
	SELECT * FROM `person`,`idencard` WHERE `person`.id=#{id} AND `person`.card_id=`idencard`.id	
</select>
```
注意:结果映射类型(type)还是填Person,因为它是映射getPersonById方法的

方案二(推荐,代码复用性好)
```xml
<resultMap id="PersonResultMap2" type="Person">
	<id property="id" column="id"/>
	<result property="name" column="name"/>
	<association property="card" column="card_id" select="全类名.getPersonById"/>
<!--select中sql返回的列作为入参-->
</resultMap>
<select id="getPersonById2" parameterType="Integer" resultMap="PersonResultMap2">
	SELECT * FROM `person` WHERE `id` = #{id}
</select>
<select id="getPersonById" parameterType="Integer" resultType="IdenCard">
	SELECT * FROM idencard where id=#{id}
</select>
```
核心思想就是把多表查询转变为单表查询,先调用etPersonById2,然后进入,PersonResultMap2,封装第一调用得到的列,然后再调用getPersonById封装card对象
**注解的方式太过于耦合,不适合使用**

**映射关系一对多cllection**
基本介绍
![](assest/Pasted%20image%2020240719211013.png)
1.应用实例场景创建
![](assest/Pasted%20image%2020240719211431.png)
2.查询结果(多对一的表)
![](assest/Pasted%20image%2020240719211952.png)
3.然后映射表的基本结构(就是写javabean)
User
```java
private Integer id;
private String name;
private List<Pet> pets;//通过这种对象封装的形式来跟另一个类建立联系
```
Pet
```java
private Integer id;
private String nickname;
private User user;
```
4.创建Mapper接口
![](assest/Pasted%20image%2020240719214551.png)
5.创建Mapper映射文件(Mapper.xml)
```xml
<mapper namespace="Mapper接口的全类名">
	<resultMap id="UserResultMap" type="User">
		<id property="id" column="id"/>
		<id property="name" column="name"/>
		<collection property="pets" column="id" ofType  = "Pet(集合里面存什么就写什么)" select="xxx.getPetByUserId">
	</resultMap>
	<select id="getUserById" parameterType="Integer" resultMap="UserResultMap">
		SELECT * FROM `mybatis_user` WHERE `id` = #{id}
	</select>
	<resultMap id="PetResultMap" type="Pet">
		<id property="id" column="id"/>
		<result property="nickname" column="nickname"/>
		<association property="user" column="user_id" resultMap="xxx.getUserById"/>
	</resultMap>
	<select id="getPetByUserId" parameterType="Integer" resultMap="PetResultMap">
		SELECT * FROM `mybatis_pet` WHERE `user_id` = #{userId}
	</select>
</mapper>
```
完整的思路分析
1.先通过getUserById返回User对象,2.然后发现有个集合,需要查找Pet,那就调用getPetByUserId方法,然后配置getPetByUserId对应的Mapper映射配置
### 动态SQL
介绍:动态SQL是MyBatis的强大特性,使用JDBC时,根据不同条件拼接SQL语句非常麻烦,例如不能忘记添加空格,动态SQL语句解决了这个问题,很多情景都是用于where查询
##### if标签
使用动态 SQL 最常见情景是根据条件包含 where 子句的一部分。比如：
```xml
<select id="方法的全类名" resultType="javabean的全类名">
  SELECT * FROM BLOG
  WHERE state = ‘ACTIVE’
  <if test= "判断表达式"><!--如果真,下面的语句会执行-->
    AND title like #{title}<!--这一节在sql语句后面接上去-->
  </if>
</select>
```
这里要新学一个方法,在判断表达式中取参数,`@Param`,在接口方法中使用
![](assest/Pasted%20image%2020240718163733.png)
##### where标签

^1e356c

**自动添加 `WHERE` 关键字**：当有一个或多个条件时，`<where>` 标签会自动添加 `WHERE` 关键字。如果没有条件，则不会添加 `WHERE`。
```xml
<select id="方法的全类名" resultType="javabean的全类名" parameterType>
  SELECT * FROM BLOG
  <where>
	  <if test="判断表达式">
		  AND sql     <!--这一节+上WHERE在sql语句后面接上去-->
	  </if>
	  <if test="判断表达式">
		  AND sql <!--如果有多余的and会自动去掉-->
	  </if>
  </where>
</select>
```
只要有一个if标签成立,where就会自动加上去
##### choose标签
不想使用所有的条件，而只是想从**多个条件中选择一个使用**。
```xml
<select id="方法的全类名" resultType="javabean的全类名" prarameterType="入参">
  SELECT * FROM BLOG
  <choose>
	  <when test="where中的判断表达式"><!--如果真,则添加下面语句-->
		  WHERE `name`=#{name}
	  </when>
	  <when test ="xxx">
		  WHERE `id` = #{id}
	  </when>
	  <otherwise>
		  WHERE `salary`>#{id}<!--默认条件-->
	  </otherwise>
  </choose>
</select>
```
##### foreach
动态 SQL 的另一个常见使用场景是对**集合进行遍历**（尤其在构建 IN 条件语句的时候）
比如查看id 为12 ,13 ,14的语句,因为是查询语句,所以有入参(入参的map有ids的key值)
```xml
<select id="方法的全类名" resultType="javabean的全类名" parameterType="入参">
  SELECT * FROM BLOG
  <if test="ids !=null and ids!=''">
	  <where>
		  id IN 
		  <foreach collection = "ids" item="id" open="(" separator=",", close=")">
		  #{id}
		  </foreach>
	  </where>
  </if>
</select>
```
这个`#{id}`是什么?
ids对应入参map的名ids的key,那个item就是在遍历ids集合时,每次取出的值,对应的变量id,所以`#{id}`就是变量id(跟items关系)
##### trim(应用少)
**trim概述**
trim能够为词取别名,_where_ 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，_where_ 元素也会将它们去除。如果 _where_ 元素与你期望的不太一样，你也可以通过自定义 trim 元素来定制 _where_ 元素的功能。
就是说,在where多个标签下,如果有多个if 判断条件作为字句,那么关键字AND,[where标签能够自动去除多余的AND](#^1e356c). 假如有特殊的情况,那就是AND换成其他的关键字,那么就需要trim来解决
```xml
<select id="方法的全类名" resultType="javabean的全类名" parameterType>
  SELECT * FROM BLOG
  <trim prefix="WHERE" prefixOverrides="and|or|其它">
	  <if test="判断表达式">
		  AND sql     <!--这一节+上WHERE在sql语句后面接上去-->
	  </if>
	  <if test="判断表达式">
		  AND sql <!--如果有多余的and会自动去掉-->
	  </if>
  </trim>
</select>
```
`<trim prefix="WHERE" prefixOverrides="and|or|其它">`:如果发现字句开头有and|or|其它,那就去掉这些词
##### set标签
`<set>` 标签主要用于构建动态 SQL 语句，特别是在更新操作中。`<set>` 标签用来包裹一组 `SET` **子句**，以便动态地生成 SQL 语句中的 `SET` 部分。
简单点说,就是如果要进行update操作,set组合就会有很多种,特别不方便进行`update`操作
比如
```sql
update 'monster' set age=1, email='yy@sjs.com',name='狼人' where id=1
```
如果要修改指定的列,那么不改变的列的值就要先找出来,然后修改的时候保持让他们不变,太过于麻烦.`<set>`标签就能解决
配置update的接口中的方法,加入方法中的形参是map
```xml
<update id = "方法全类名" parameterType="map">
	UPDATE `表名` SET ....
	<set>
		<if test="age!=null and age!= ''">
			`age`=#{age} ,
		</if>
		<if test="判断表达式">
			要设定的值 ,
		</if>
	</set>
	WHERE id = #{id}
</update>
```
每个if 标签下多余的`,`会自动除去, 注意后面要加where,如果不加,回修改所有的值
### 缓存
![](assest/Pasted%20image%2020240720143831.png)
在实际中的效果,两次查询相同的数据,只有第一次会发出sql语句,第二次不会,这就是本地缓存造成的结果
##### 一级缓存
一级缓存的概述:一级缓存（Local Cache）是 MyBatis 中的一种缓存机制，它在 **SQLSession 范围内**工作。一级缓存默认是启用的，且无法关闭。通过同一个SqlSession查询的数据会被缓存，下次查询相同的数据，就会从缓存中直接获取，不会从数据库重新访问
1.一级缓存的debug过程
![](assest/Pasted%20image%2020240720191247.png)
然后一直set into 找到cache,然后会发现它并不会执行 Cache cache=ms.getCache();因为这个方法是二级缓存的,一直set into然后要注意到localCache.getObject(key)这是执行二级缓存的,查看有没有缓存..然后执行localCache.putObject(key,list);把查询到的结果放入缓存中

2.一级缓存失效
- 使一级缓存失效的四种情况：  
	1. 不同的SqlSession对应不同的一级缓存简而言之就是被关闭过一次  
	2. 同一个SqlSession但是查询条件不同
	3. 同一个SqlSession两次查询期间执行了任何一次增删改操作
	4. 同一个SqlSession两次查询期间手动清空了缓存(sqlSession.clearCache())
##### 二级缓存
![](assest/Pasted%20image%2020240720200755.png)
二级缓存（Second-Level Cache）是全局性的缓存机制,通过同一个SqlSessionFactory创建的SqlSession查询的结果会被缓存；此后若再次执行相同的查询语句，结果就会从缓存中获取  
- 二级缓存开启的条件
	1. 在核心配置文件中，设置全局配置属性cacheEnabled="true"，默认为true，不需要设置
```xml
<settings>
	<setting name="cacheEnabled" value="true"/>
</settings>
```
	2. 在映射文件中设置标签<cache />
```xml
<cache
	eviction="FIFO" 
	flushInterval="60000"
	size="512"
	readOnly="false"/>
```
eviction;更新机制 , flushInterval更新时间  size可以存多少条, readOnly是否只能读一个默认是false 但是如果只是读的操作,建议改成true,可以提高效率
具体看文档.
	2. 二级缓存必须在SqlSession关闭或提交之后有效
	3. 查询的数据所转换的实体类类型必须实现序列化的接口
缓存失效的原因:两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效
二级缓存的debug过程差不多 , 但是在Cache cache=ms.getCache()这里会进入的,因为有二级缓存
```java
public class MySqlSession {  
    private Executor executor=new MyExecutor();  
    private MyConfiguration myConfiguration= new MyConfiguration();  
    //编写方法SelectOne 返回一条记录  
    public <T> T selectOne (String statement,Object parameter){  
        return executor.query(statement, parameter);  
    }  
    public <T> T getMapper(Class<T> clazz){  
        return (T) Proxy.newProxyInstance(clazz.getClassLoader(),new Class[]{clazz},new MyMapperProxy(this, myConfiguration,clazz));  
    }  
}
```