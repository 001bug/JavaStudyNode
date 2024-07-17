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
	 细节: 如果需要获得自增的key值,那么可以在标签中加seGeneratedKeys="true" keyProperty="id"
	 如果需要简写javabean的全类名,需要在mybatis-config.xml配置
```
<typeAliases>  
    <typeAlias type="javabean全类名" alias="要被替代的别名"/>  
</typeAliases>
```


7.写MyBatisUtils 工具类 ^ede5a4
```
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
```
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
```
<settings>
	<setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```
注意这个配置代码要放在前面.

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
Executor执行器是个接口,他被很多类实现了,其中重要是BaseExecutor,还有个带缓存的CachingExecutor执行器
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
```
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
```
InputStream stream = loader.getResourceAsStream(resource);
```
[resource是类路径的相对路径](../Java/类路径和Java相对路径.md)\

2.xml解析获取document根元素 ^e8bb23
```
//加载配置xxx.xml 获取到对应的InputStream  
InputStream stream = loader.getResourceAsStream(resource);  
//解析xxx.xml  => dom4j  
SAXReader reader = new SAXReader();  
Document document = reader.read(stream);  
//获取到xxx.xml 的根元素 <database>  
Element root = document.getRootElement();  
//解析root元素，返回Connection => 单独的编写一个方法
```

3.拿到根元素,然后去遍历,并且通过选择性的语句去获取xml中配置文件的信息(node就是root,)
```
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
```
Class.forName(driverClassName);  
connection = DriverManager.getConnection(url,username,password);
```
##### 3.写执行器Executor
源码是基于接口实现了很多类去实现sql的 .  这里直接开发一个简单的基于Executor实现的BaseExecutor

1.在此之前,学习一个**快速开发javabean**的包
相关pom依赖
```
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
```
public interface Executor {  
    //泛型方法  
    public <T> T query(String statement, Object parameter);  
}
```

3.基于接口实现BaseExecutor
* 查询操作,实现类没有采用反射机制(传入参数是 sql语句,和属性 parameter)
```
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

4.在SqlSession方法中让Executor和Configuration连接
```
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
```
public interface MonsterMapper {  
    public Monster getMonsterById(Integer id);  
}
```
对应的xxxMapper.xml文件
```
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
```
public class Function {  
    private String sqlType;  
    private String funcName;  
    private String sql;  
    private Object resultType;  
    private String parameterType;
}
```
* 写MapperBean(封装Mapper信息) (get和set方法省略了,要补全)
```
public class MapperBean {  
    private String interfaceName;//接口全类名  
    private List<Function> functions;//保存接口中的方法
}
```
* 封装MapperBean
```
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
            //resultType现在不知道他是什么类型,在运行的时候才能知道他是什么类型,所以这里可以死使用反射机制处理  
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
1. 这里先采用[xml解析获取mapper的配置信息](#^e8bb23) , 注意这里做了简化,我把xml配置信息默认放在类的类路径的[相对路径](../Java/类路径和Java相对路径.md)下,就不用像原生的MyBatis那样要通过解析全局配置文件,然后获得xxxmapper.xml的类路径的相对路径
2. 循环遍历,封装[Function](#^edb9fc) 然后方进一个集合中,因为可能有多个接口,以及xxxmapper.xml,最后返回一个具有多个mapper.xml信息和多个mapper接口信息的信息体. 
##### 5.实现[动态代理](设计模式)MapperProxy类
在一开始的Executor是直接通过JDBC调用MySQL , 这里进行改进,通过一个名为MapperProxy代理类,然后动态的生成代理对象,然后去调用**BaseExecutor中的方法**
```
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
            }  
        }  
    }  
    return null;  
}
```
首先,要实现动态代理, 目的是为了让接口的方法扩展功能(解析xml,实现mapper接口),用动态代理对象调用