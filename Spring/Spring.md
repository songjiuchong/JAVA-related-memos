Spring

package prototype;

public class Spring {
	public static void main(String [] args){
		
	}
}

/*

//Spring 框架;
1.Spring作用和优点;
Spring框架提供了一个开发平台,用于整合其他的开发技术,例如Struts,Hibernate等;
用于整合,相当于计算机主板的角色,如:将CPU(Struts)和硬盘/内存(Hibernate)整合起来,并且
具有较低的耦合性,可以较好地改善程序结构,便于扩展和更新;

2.Spring框架主要功能;
a.提供了整合其他技术的API;
b.Spring提供了对象创建功能(具有工厂功能的容器框架);
c.Spring提供了IOC和AOP机制,可以降低组件对象之间的耦合度;

3.Spring框架基本应用;
对象创建功能(具有工厂功能的容器框架);

1)使用方法:
a.引入spring-ioc开发包;
b.在src下添加applicationContext配置文件;
c.将Bean组件在配置文件中定义<bean id="标示符" class="组件类型"/>
d.实例化spring容器对象,调用getBean("标示符")获取Bean实例对象使用;

2)String对Bean组件的管理;
a.对象的创建模式;
可以使用<bean>中的scope属性控制创建模式;属性值如下:
singleton:默认,单例模式;
prototype:非单例;
(此外该属性应用在Web程序中,还可以扩展为request,session等)
b.对象创建时机;
scope=singleton时,bean对象和Spring容器一起实例化; 
(通过lazy-init="true"属性可以讲对象创建推迟到getBean方法调用时,一般不会使用);
scope=prototype时,bean对象在用getBean方法获取对象实例时才会去实例化;
c.指定对象创建和销毁方法;
init-method属性可以指定对象初始化方法,当对象创建之后自动执行该方法;
destroy-method属性只适用于scope=singleton的Bean对象;
为其指定销毁方法;
当调用Spring容器的close()时,会自动调用单例Bean对象指定destroy-method方法;


例子:
public interface CostDao{
	public void save();
	public void delete();
}

public class JdbcCostDao implements CostDao{
	public void delete(){
		System.out.println("采用JDBC技术删除资费");
	}
	
	public void save(){
		System.out.println("采用JDBC技术添加资费");
	}
	
	public void myinit(){
		System.out.println("对象初始化");
	}
	
	public void destroy(){
		System.out.println("对象资源释放");
	}
}

public class Test{
	//实例化Spring框架容器对象;
	public void test2(){
		String conf = "applicationContext.xml";
		ApplicationContext ac = new ClassPathXmlApplicationCtext(conf);
		//从容器获取id=costDao的Bean对象;
		CostDao costDao = (CostDao)ac.getBean("costDao"); //相当于Spring实现了DaoFactory的工作;
		costDao.delete();
		CostDao costDao2 = (CostDao)ac.getBean("costDao");
		System.out.println(costDao==costDao2); //singleton模式时true,prototype模式时返回false;
		//释放容器对象;
		ac.close();
	}
	
	public void test(){
		CostDao costDao = new JdbcCostDao();
		costDao.delete();
	}
}


将Spring jar包引入(commons-logging.jar,spring.jar);
在Spring配置文件中加入:
<!--声明定义Bean组件(默认是单例模式singleton,prototype是非单例)-->
<bean id="costDao" 
scope="prototype" 
init-method="myinit"
destroy-method="destroy"
class="org.tarena.dao.JdbcCostDao">
</bean>


3.IOC机制;
1)IOC概念;
Inverse of Controller称为反向控制或控制反转(控制转移action->spring);
组件之间调用,将组件对象的创建和关系的建立由第三方框架或容器负责(spring);
2)DI 依赖注入(即IOC实现原理); 
Dependency Injection;  (Action->DAO);
Spring框架采用DI技术实现了IOC控制思想;
当两个组件存在使用关系时,也就是存在依赖关系,依赖关系建立可以通过以下几种注入途径:
a.setter方式注入; (Action->DAO 通过setter方法将DAO传入);
--在Action中定义属性变量和set方法;
--在<Bean>配置时,指定注入关系<property name="属性名" ref="要注入的Bean类型id">

b.构造方式注入;  (Action->DAO 通过构造方法将DAO传入);
--在Action中定义属性变量和带参数的构造方法;
--在<bean>定义配置中,指定注入<constructor-arg index="参数索引" ref="要注入的bean类id">

例子:
public class AddCostAction{
	
	private CostDao costDAO;
	setter...

	public String execute(){
		System.out.println("添加资费");
		costDAO.save();
		return "success";
	}
}

<bean id="addCostAction" scope="prototype" class="org.tarena.action.AddCostAction" >
	<!--setter方式注入-->
	<property name="costDAO" ref="costDao"></property>
</bean>

public class DeleteCostAction{
	
	//构造注入;
	private CostDao costDAO;
	
	public DeleteCostAction(CostDao costDao){
		this.costDAO = costDAO;
	}
	
	public String execute(){
		System.out.println("开始处理删除资费请求");
		return "success";
	}
}

<bean id="deleteCostAction" scope="prototype" class="org.tarena.action.DeleteCostAction" >
	<!--构造方式注入,将costDao给构造方法第一个参数传入-->
	<constructor-arg index="0" ref="costDao"></constructor-arg>
</bean>

测试:
public class test(){
	String conf = "applicationContext.xml";
		ApplicationContext ac = new ClassPathXmlApplicationCtext(conf);
		AddCostAction addCostAction = (AddCostAction)ac.getBean("addCostAction"); 
		action.execute();
}


3.如何注入各种类型数据;
a.基本类型注入:字符串,数值,日期等;
<property name="属性名" value="属性值"></property>
b.Bean对象注入;
<property name="属性名" ref="要注入的Bean类型id"></property>
c.集合类型注入;
List,Set,Map,Properties


例子:
public class MessageBean{
	private String dir;
	private int maxSize;
	private List<String> types = new ArrayList<String>();
	private List<String> cities = new ArrayList<String>();
	private Set<String> friends = new HashSet<String>();
	private Map<Integer,String> costs = new HashMap<Integer,String>();
	private Properties params = new Properties();
	
	//接收一个"jpg,jpeg,gif"类型的字符串,传出一个字符串集合,但是这个setter方法不是自动生成的,是自定义的;
	public void setIncluseTypes(String type){
		String[] arr = type.split(",");
		for(String s:arr){
			types.add(s);
		}
	}
	
	setter;
	
	public void show(){
		System.out.println("路径:"+dir);
		System.out.println("允许上传最大值:"+dir);
		System.out.println("允许上传类型如下:");
		for(String t:types){
			System.out.println(t);
		}
		System.out.println("List集合信息:"+cities);
		System.out.println("Set集合信息:"+friends);
		System.out.println("Map集合信息:"+costs);
		System.out.println("Properties信息:");
		Set<Object> keys = params.keySet();
		for(Object key:keys){
			System.out.priontln(key+":"+params.getProperty(key.toString()));
		}
	}
}

<bean id="message" class="org.tarena.aciton.MessageBean">
	<!--通过value属性注入直接值-->
	<property name="dir" value="D:\\images"></property>
	<property name="maxSize" value="10240"></property>
	<property name="includeTypes" value="jpg,jpeg,gif"></property>
	<property name="cities">
		<list>
			<value>北京</value>
			<value>上海</value>
			<value>深圳</value>
		</list>
	</property>
	<property name="friends">
		<set>
			<value>tom</value>
			<value>jerry</value>
		</set>
	</property>
	<property name="costs">
		<map>
			<entry key="1" value="包月">
			</entry>
			<entry key="2" value="套餐">
			</entry>
		</map>
	</property>
	<property name="params">
		<props>
			<prop key="driver">com.jdbc.driver.Driver</prop>
			<prop key="username">root</prop>
			<prop key="password">root</prop>
		</props>
	</property>
</bean>

总结:
1.Spring创建Bean对象的使用方法;
2.Spring中IOC使用;
3.了解Spring框架作用和优点;
4.Spring中IOC和DI概念;
5.了解Spring中各种类型注入的配置写法;


1.Spring的AOP机制;

1)AOP概念;
Aspect Oriented Programming; 面向方面编程;
(Object Oriented Programming 面向对象编程);
AOP和OOP区别在于侧重点不同,OOP侧重点在于对象及其封装;
AOP侧重点在于方面,(即封装共通逻辑的组件),改善程序结构;
AOP编程是以OOP为基础的,用于改善共通处理和目标方法的耦合度;

2)AOP编程概念;
a.方面(Aspect)
封装共通处理部分的组件;
组件可以被切入(作用)到其他目标组件方法上;
b.切入点(Pointcut)
负责制定哪些组件方法调用方面(共通)处理;通过一个表达式指定;
c.连接点(JoinPoint)
切入点是连接点的集合;连接点包含了方面和目标方法关联的信息;
例如当前作用的目标组件类型和作用的方法名等信息;
d.通知(Advice)
负责制定方面处理和目标组件方法之间的作用关系;例如先执行方面处理,再执行目标处理;先执行
目标处理再执行方面处理等;
e.目标(Target)
启用方面处理的组件,即切入点指定的组件;
f.动态代理(AutoProxy)
Spring采用动态代理技术实现了AOP控制;
当采用了AOP切入之后,Spring容器返回的目标对象是采用了动态代理技术生成的一个类型对象;
该类型会重写原目标对象的方法,在其内部会调用原目标的方法和方面处理;
Spring采用了一下两种动态代理技术;
技术:
--CGLIB.jar技术;
(适用于目标组件没有接口的情况)
public class 类型 extends 原目标类型{}
--JDK Proxy技术;
(适用于目标组件有实现接口的情况)
public class 类型 implements 原目标接口{}

3)AOP编程主要步骤:
--引入aop开发包;
--编写方面组件;
--添加spring的AOP配置;


public String execute(){
	System.out.println("模拟记录操作日志");
	System.out.println("开始处理添加资费请求");
	costDAO.save();
	return "success";
}

public String execute(){
	System.out.println("模拟记录操作日志");
	System.out.println("开始处理删除资费请求");
	costDAO.delete();
	return "success";
}

这样大面积的共通功能在后期维护上会比较有问题;

AOP:
在src中添加一个包:Aspect,添加一个java文件:

//封装记录操作日志功能(共通)
public class LoggerBean{
	//环绕通知;<aop:around pointcut-ref="actionPointcut" method="log"/>
	//只有运用环绕通知才能将连接点参数传入方面方法;
	public Object log(ProceedingJoinPoint pjp){
		//获取目标组件类型名;
		String className = pjp.getTarget().getClass().getName();
		//获取当前执行的目标组件方法名;
		String methodName = pjp.getSingnature().getName();
		//根据className,methodName去opt.properties中获取操作名;
		//文件获取操作名:
		String key = className+"."+methodName;
		String val = PropertiesUtil.getValue(key);
		System.out.println(new Date()+"执行了"+val);
		Object obj =  pjp.proceed(); //执行目标方法;
		//追加后续处理;
		return obj;
	}
}

//将spring-aop相应的jar包导入:
aspectjrt.jar aspectjweaver.jar等4个;

在配置文件中加入:
<!--采用AOP将记录日志的LoggerBean作用到Action方法上-->
<Bean id="loggerBean" class="org.tarena.aspect.LoggerBean">
</Bean>

<aop:config>
	<!--切入点-->
	<aop:pointcut id="actionPointcut" expression="within(org.tarena.action..*)"/>
	<!--方面-->
	<aop:aspect id="loggerAspect" ref="loggerBean">
		<!--通知-->
		<aop:before pointcut-ref="actionPointcut" method="log"/>
	</aop:aspect>
</aop:config>

建立一个properties文件(opt.properties):
org.tarena.action.AddCostAction.execute=资费添加操作
org.tarena.action.DeleteCostAction.execute=资费删除操作

//注意:
properties文件中默认以iso8859-1来编码,可以事先在创建时规定其编码格式,或者利用JDK自带
工具:Java\jdk...\bin\native2ascii 也就是将中文编译成eclipse能够识别的unicode编码格式;

创建一个读取properties文档的工具类:

public class PropertiesUtil{

	private static Properties props = new Properties();
	
	static{
	try{
		props.load(PropertiesUtil.class.getClassLoader().getResourceAsStream("opt.properties"));
	}catch(IOException e){
		......
	}
	}
	
	public static String getValue(String key){
		String val = props.getProperty(key);
	}
}


4)通知类型:
通知负责制定方面组件和目标组件作用的关系;
Spring提供的通知类型如下:
a.前置通知(<aop:before>);
先执行方面组件,再执行目标方法;
通知方法定义如下:
public void xxx(){}

b.后置通知(<aop:after-returnning>);
先执行目标组件,如果没有抛出异常,再执行方面组件(如果有异常,则无法运行方面组件);
方法定义:
public void xxx(Object val){}
val用于获取目标方法返回值;

c.最终通知(<aop:after>);
先执行目标组件,无论有无异常,都会再执行方面组件;
方法定义:
public void xxx(){}

d.异常通知(<aop:after-throwing>);
执行目标组件,如果抛出异常,才会执行方面组件;
public void xxx(Exception e){}
e用于接收目标组件抛出的异常对象;

e.环绕通知(<aop:around>)(前置和后置的结合);
先执行方面组件,然后进入目标组件,最后再进入方面组件;
public Object xxx(ProceedingJoinPoint pjp){
	//aspect;
	Obect obj = pjp.proceed(); //执行目标组件;
	//aspect;
	return obj;
}

pjp代表连接点对象;
返回值Object;

通知在内部的实现:
try{
	//前置通知--执行方面组件;
	//执行目标组件;
	//后置通知--执行方面组件;
}catch(){
	//异常通知--执行方面组件;
}finally{
	//最终通知--执行方面组件;
}


5)切入点表达式;
负责制定目标组件及其方法;
a.方法限定表达式;
可以指定哪个组件方法启用方面组件处理;
语法格式如下:
execution(修饰符? 返回值 方法名(参数列表) throws 异常? )

例子1:匹配容器中Bean对象里以find开始的方法,返回值任意,参数可以是0到多个;
execution(* find*(..))

例子2:匹配CostDao对象中所有方法;
execution(* dao.CostDao.*(..))

例子3:匹配dao包下所有类的所有方法;
execution(* dao.*.*(..))

例子4:匹配dao包及其子包中所有类的所有方法;
execution(* dao..*.*(..))

b.类型限定表达式:
负责指定哪些类型的组件启用方面组件处理;
within(类型)

例子1:匹配CostDao对象的所有方法;
within(dao.CostDao)

例子2:匹配dao包下所有对象的所有方法;
within(dao.*)

例子3:匹配dao包及其子包下所有对象的所有方法;
within(dao..*)

c.Bean名称限定:
利用<Bean>定义的id进行限定;
bean(id值)

实例1:匹配id=costDao的Bean对象;
bean(costDao)

实例2:匹配id值以Action结尾的Bean对象;
bean(*Action)

d.参数限定表达式;
可以对方法参数限定;
args(参数类型)

例子:匹配容器中所有Bean对象中只有一个参数,且参数类型是String的方法;
args(java.lang.String)


注意:
上述表达式可以采用&&,||运算符链接;


6)采用Spring AOP实现异常处理,将异常信息写入文件;
--切入点(指定目标);
作用到能够抛出异常的组件上;
within(org.tarena.action..*)
--通知;
采用异常通知<aop:after-throwing>
--方面;
将异常信息写入文件;
public void xxx(Exception e){}

在aspect类中定义ExceptionBean类;
//记录异常信息;
public class ExceptionBean{
	Logger logger = Logger.getLogger(ExceptionBean.class);
	
	public void logException(Exception e){
		//将e异常信息写入文件;
		logger.error("记录异常信息"+e);
		StackTraceElement[] els = ex.getStackTrace();
		logger.error(els[0]);
	}
}

配置:
<bean id="exceptionBean" class="org.tarena.aspect.ExceotionBean">
</bean>
<aop:config>
......
<!--aspect 2-->
	<aop:aspect id="exceptionAspect" ref="exceptionBean">
		<!--throwing指定获取异常方法的参数变量名-->
		<aop:after-throwing method="logException" pointcut-ref="actionPointcut1" throwing="e"/>
	</aop:aspect>
</aop:config>


log4j工具;
Log4j是Apcache组织推出的一款日志工具;
他可以将消息分成不同级别,可以将消息以不同形式输出;

该工具主要由一下几部分构成:
a.日志器Logger;
负责提供各种方法,不同方法对应不同的消息级别;
b.输出器Appender;
负责提供输出形式;例如控制台,文件输出等;
c.布局器Layout;
负责格式化消息; (级别-内容);

例子:
public class test{
	Logger logger = Logger.getLogger(test.class);
	
	public static void main(String[] args){
		logger.debug("调试信息");
		logger.info("普通信息");
		logger.warn("警告信息");
		logger.error("异常信息");
		logger.fatal("致命错误");
		//级别从小到大;
	}
}

配置文件:
在src/log4j.properties添加文件;

#logger = level(大于等于所指定级别的都会被运行),appender
log4j.rootLogger=WARN,myconsole,myfile
#log4j.appender.[appenderName]
log4j.appender.myconcole=org.apache.log4j.ConsoleAppender //jar包中有关于appender的文件,此处也可以是FileAppender等
log4j.appender.myfile=org.apache.log4j.FileAppender
log4j.appender.myfile.File=D:\\log4j.html
#layout
log4j.appender.myconsole.layout=org.apache.log4j.SimpleLayout
log4j.appender.myconsole.layout=org.apache.log4j.HTMLLayout


1.注解配置方法;
从JDK5.0开始支持注解;
其他框架开始利用注解替代XML配置文件;
Spring框架从2.5开始可以使用注解配置;
注解配置好处:简单方便;
1)什么是注解;
注解就是在类定义,方法定义或者成员变量定义前追加的@标记;
2)Spring组件自动扫描技术,替代XML配置中的<bean>元素定义;
自动扫描技术,可以扫描指定package下的组件,如果发现组件类定义前有
一下指定标记,就会将该组件扫描到spring容器,等价于定义了一个<bean>元素;
扫描标记如下:
@Component: 其他组件;
@Repository:DAO
@Service: Service 业务层组件
@Controller: 控制层

标记后可以指定bean的id值,默认为首字母小写的原名;

在spring配置文件中加入:
<!--开启组件扫描功能,指定扫描包路径-->
<context:component-scan base-package="org.tarena.dao">
</context:component-scan>

在类前定义:
@Repository("costDao")
@Scope("prototype") //定义scope属性;
public class CostDao{
	@PostConstruct //等价于设置初始化方法属性;
	public void myinit(){
	}
	
	@PreDestroy //等价于destory-method属性
	public void destroy(){
	}
}

注入的定义:
public class AddCostAction{
	
	@Resource(name="hibernateCostDao") //默认会自动添加setter方法,可以不写,如果有多个符合CostDao类的类型会报错,需要指定注入名;
	private CostDao costDAO;
//	public void setCostDAO(CostDao costDAO){
//		this.costDAO = costDAO;
//	}
	
	public String execute(){
		......
	}
}

3)注入注解;
在Action的属性变量或set方法前,使用@Resource标记;Spring会采用类型匹配
规则注入;如果需要注入指定名称的Bean对象,可以使用@Resource(name="id");
@Resource标记原本是属于JDK本身的;
Spring提供了@Autowired标记,作用和@Resource一样,但是指定id名方法是:
@Qualifier("")

4)AOP使用注解添加;

在spring配置文件中加入:
<!--开启AOP注解配置-->
<aop:aspectj-autoproxy/>

@Component //将组建扫描到容器;
@Aspect //将该组件定义为方面组件;
public class LoggerBean{
		@Around("within(org.tarena.action..*)") //环绕通知;
		public Object log(ProceedingJoinPoint pjp){
		//获取目标组件类型名;
		String className = pjp.getTarget().getClass().getName();
		//获取当前执行的目标组件方法名;
		String methodName = pjp.getSingnature().getName();
		//根据className,methodName去opt.properties中获取操作名;
		//文件获取操作名:
		String key = className+"."+methodName;
		String val = PropertiesUtil.getValue(key);
		System.out.println(new Date()+"执行了"+val);
		Object obj =  pjp.proceed(); //执行目标方法;
		//追加后续处理;
		return obj;
	}
}


//记录异常信息;
@Component
@Aspect
public class ExceptionBean{
	Logger logger = Logger.getLogger(ExceptionBean.class);
	
	@AfterThrowing(pointcut="within(org.tarena.action..*)",throwing="e")
	public void logException(Exception e){
		//将e异常信息写入文件;
		logger.error("记录异常信息"+e);
		StackTraceElement[] els = ex.getStackTrace();
		logger.error(els[0]);
	}
}


2.Spring整合数据库访问技术;
Spring框架对数据库访问技术的支持如下:

a.提供了一致的异常处理层次;
DataAccessException;

public class Action{
	public String execute(){
		try{
			dao.findById();
		}catch(不同类型的Exception e){
			......
		}
	}
}

b.提供了编写DAO所需的DaoSupport和Template工具类;

c.提供了声明式事务管理(AOP);


1)Spring整合JDBC技术;

--引入Spring开发包,数据库驱动包;
--添加配置Spring配置文件;
--编写数据表对应的POJO;
--编写DAO接口及其实现类;
DAO实现类继承了JdbcDaoSupport,之后利用super.getJdbcTemplate()获取JdbcTemplate对象,完成增删改查操作;
update():执行insert,update,delete操作;
queryForObject:执行查询,最多返回一行;
query:执行查询,返回多条;
queryForInt:执行查询,返回int值; //如: select count(*) from ...

--将DAO配置到Spring框架容器中;
--定义DataSource组件,给DAO的dataSource属性注入;

例子:

public interface CostDao{
	public Cost findById(int id) throws DataAccessException; //利用自定义异常后,在DAO实现类中先捕获DataAccessException再抛出自定义异常;
	......
}

@Repository("costDao")
public class JdbCostDao 
	extends JdbcDaoSupport 
	implements CostDao{
	
	//此处无需再声明类似private DataSource dataSource;
	//setter...因为这些步骤都已经封装到JdbcDaoSupport父类中了;
	
	//如果使用注解配置,此处无法自动给父类注入dataSource对象,只能通过以下方法;
	@Resource //自动注入DataSource对象,此处是根据参数类型来匹配注入对应的对象;
	public void setMyDataSource(DataSource ds){
		//将dataSource给daoSupport传入;
		super.setDataSource(ds);
	}
	
	public void findById(int id) throw DataAccessException{
		String sql = "select * from COST where ID=? "
		Object[] params = {id};
		super.getJdbcTemplate().update(sql,params);
		RowMapper mapper = new CostMapper();
		Cost cost = (Cost)super.getJdbcTemplate().queryForObject(sql,params,mapper);
		return cost;
	}
	
		public void save(Cost cost) throw DataAccessException{
		String sql = String addCostSql = 
			"insert into "+
			"COST(ID,NAME,BASE_DURATION,BASE_COST,UNIT_COST,STATUS,DESCR,CREATIME,COST_TYPE) "+
			"values(cost_seq.nextval,?,?,?,?,'1',?,sysdate,?)"; 
		Object[] params = {cost.getName(),cost.getBaseDuration(),cost.getBaseCost(),...};
		super.getJdbcTemplate().update(sql,params);
		}
		
		public List<Cost> findAll() throws DataAccessException{
			String sql = "select * from COST";
			RowMapper mapper = new CostMapper();
			List<Cost> list = super.getJdbcTemplate().query(sql,mapper);
			return list;
		}
	......
}

在POJO包下建立;
public class CostMapper implements RowMapper{
	//自动调用该方法,将ResultSet中当前行记录封装成Cost对象;
	public Object mapRow(ResultSet rs,int index) throw DataAccessException
		//将之前DAO实体类中的createCost方法中逻辑引入:
		Cost c = new Cost();
		c.setId(rs.getInt("ID"));
		c.setName(rs.getString("NAME"));
		c.setBaseDuration(rs.getInt("BASE_DURATION"));
		c.setBaseCost(rs.getDouble("BASE_COST"));
		c.setUnitCost(rs.getDouble("UNIT_COST"));
		c.setStatus(rs.getString("STATUS"));
		c.setDescr(rs.getString("DESCR"));
		c.setCreateTime(rs.getDate("CREATIME"));
		c.setStartTime(rs.getDate("STARTIME"));
		c.setCostType(rs.getString("COST_TYPE"));
		return c;
	}
}


引入dbcp jar包;

在Spring配置中添加:

		<!--定义dbcp提供的连接池对象-->
		<bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource.class">
			<property name="url" value="jdbc:oracle:thin:@localhost:1521:song"></property>
			<property name="driverClassName" value="oracle.jdbc.driver.OracleDriver"></property>
			<property name="username" value="system"></property>
			<property name="password" value="songsong"></property>
			<!-- 指定连接池里的最大连接数 -->
			<property name="maxActive" value="20"></property>
			<!-- 初始连接数 -->
			<property name="initialSize" value="2"></property>
		</bean>

<bean id="costDao" class="org.tarena.dao.JdbCostDao">
	<!--注入dataSource资源为template提供连接-->
	<property name="dataSource" ref="myDataSource">
</bean>


连接池(dataSource)
用于存储和管理Connection对象的组件;
使用连接池好处如下:
a.可以控制connection对象的数量,不会将压力施加在数据库端;
保持在安全的数量范围之内;
b.池中Connection对象始终与数据库保持连接状态;
避免频繁创建和关闭连接过程,浪费了资源和时间;

连接池组件目前流行的有:dbcp,c3p0,proxool等;


Spring框架整合Hibernate;
--引入Spring开发包,Hibernate开发包,数据库驱动,dbcp驱动;
--添加Spring配置文件;
--根据表定义POJO和hbm.xml;
--定义DAO接口,编写DAO实现类;
DAO实现类继承HibernateDaoSupport类;
可以使用super.getHibernateTemplate()获得对象,实现增产改查操作;
--在Spring配置文件中定义DAO(可以采用XML或者注解扫描)
--在Spring中定义SessionFactory对象,然后给DAO对象注入;

public class HibernateCostDao 
	extends HibernateDaoSupport 
	implements CostDao throws DataAccessException{

	public void delete(Cost cost) throws DateAccessException{
		super.getHibernateTemplate().delete(cost);
	}
	
	public void save(Cost cost) throws DataAccessException{
		super.getHibernateTemplate().save(cost);
	}
	
	public update(Cost cost) throws DataAccessException{
		super.getHibernateTemplate().update(cost);
	}
	
	public Cost findById(int id) throws DataAccessException{
	
		//Spring封装的HibernateTemplate对象中已经包含了transaction
		//和session的获取和关闭,所以这里不能直接用.load()延迟加载;
		Cost cost = (Cost)super.getHibernateTemplate().get(Cost.class,id);
		return cost;
	}
	
	public List<Cost> findAll() throws DataAccessException{
		String hql = "from Cost ";
		List<Cost> list = super.getHibernateTemplate().find(hql);
		return list;
	}
	
	public int count() throws DataAccessException{
		String hql = "select count(*) from Cost";
		List list = super.getHibernateTemplate().find(hql);
		Long size = (Long)list.get(0);
		return size.intValue();
	}
	......
}


在Spring配置中添加:

<bean id="mySessionFactory" class="org.springframework.orm.hibernate3.LocalSessionFactoryBean">
	<!--注入hbm.xml文件-->
	<property name="mappingResources">
		<list>
			<value>org/tarena/pojo/Cost.hbm.xml</value>
		</list>
	</property>
	<!--hibernate框架参数-->
	<property name="hibernateProperties">
		<props>
			<prop key="hibernate.show_sql">true</prop>
			<prop key="hibernate.format_sql">true</prop>
			<prop key="hibernate.dialect">org.hibernate.dialect.OracleDialect</prop>
		</props>
	</property>
	<!--指定数据库连接基本参数-->
	<!--将之前声明好的连接池配置注入SessionFactory-->
	<property name="dataSource" ref="myDataSource">
	</property>
</bean>


<bean id="costDao" class="org.tarena.dao.HibernateCostDao">
	<!--给HibernateDaoSupport注入sessionFactory资源-->
	<property name="sessionFactory" ref="mySessionFactory">
</bean>


可以对Spring框架的内容进行再封装,达到简便的效果;
例如:

public class AnnotationDaoSupport extends JdbcDaoSupport{

	@Resource //自动注入DataSource对象,此处是根据参数类型来匹配注入对应的对象;
	public void setMyDataSource(DataSource ds){
		//将dataSource给daoSupport传入;
		super.setDataSource(ds);
	}
}

public class JdbCostDao 
	extends AnnotationDaoSupport 
	implements CostDao{
	......
}


1.Spring和Struts的整合;

例子:
struts2配置文件:
......
<struts>
	<package name="ss" extends="struts-default">
	<!--用spring整合struts2的jar包引入后,这里class属性中的路径不再是action文件的包名.类名,而是Spring配置中这个action的id属性名-->
	<action name="welcome" class="welcomeAction> 
		<result>
			/welcome.jsp
		</result>
	</action>
	</package>
</struts>

在webroot中建立welcom.jsp:
......
<head>
	<body>
		欢迎<br>
	</body>
</head>

src下建立:
WelcomeAction.java......

引入struts2-spring-plugin.jar包;
引入这个包后,struts会自动放弃对请求中action对象的创建,而是去Spring容器中寻找创建好的对象;

由于如果struts去spring容器中取得action对象,例如ac.getBean("action");首先需要实例化容器,
所以,这里需要在web.xml中创建一个监听器:

<!--为listener指定spring配置文件-->
<context-param>
	<param-name>contextConfigLocation</param-name>
	<!--利用classpath:指定路径为src下-->
	<param-value>classpath:applicationContext.xml</param-value>
</context-param>

<!--在启动服务器时,实例化Spring容器-->
<listener>
	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>

在Spring配置文件中定义action:
<bean id="welcomeAction" scope="prototype" class="org.tarena.action.WelcomAction">
</bean>

总结:
a.将Action定义到Spring容器,由Spring容器实例化;
b.引入struts-spring-plugin.jar包,再收到浏览器请求时,struts控制器会根据struts.xml中action的配置
,调用plugin.jar去Spring容器中寻找action对象;
c.将struts.xml中action的class属性与spring容器中的id属性需要保持一致;
d.在web.xml中定义listener组件,用于实例化spring容器对象;


*.action-->Filter(struts.xml)
-->Action对象(ObjectFactory)
-->引入struts-spring-plugin.jar;(在web工程加载时,这个jar包也被加载,其中的配置文件将Struts默认配置中的一些参数以constant标签的形式修改了);
-->提供了一个StrutsSpringObjectFactory可以访问Spring框架容器;
并且在struts-plugin.xml中将struts框架的struts.objectFactory参数指定成了StrutsSpringObjectFactory;
这样Struts再接收到Action请求时,会调用StrutsSpringObjectFactory获取Spring中action对象;

StrutsSpringObjectFactory获取Action对象的方法如下:

public Object getAction(){
	//获取Spring容器对象ac(之前在web.xml中设置的filter中已经实例化了);
	try{
	Object action = ac.getBean(className);
	return action;
	}catch(){
		//如果通过class找不到对象;(比如在Struts配置文件中定义action的class为action的包名.类名,而不是Spring中bean对象的id值);
		//利用class属性,采用反射机制实例化一个Action对象;
		Class c = class.for(className);
 		*Action action = (*Action)c.newInstance();
 		//采用名称匹配规则(需要保证action类中属性名称和Spring配置中bean对象id相同),将Spring容器中的id名与action中属性名相同的Bean对象给Action属性赋值注入;
 		//也可以在此属性声明前使用@Resource来让 StrutsSpringObjectFactory使用类型匹配注入;
 		return action;
	}
}

所以,如果不去Spring配置中定义action bean,在struts.xml中action对象的class属性
还是按照标准的包名.类名,而不是Spring配置中bean对象id,StrutsSpringObjectFactory组件
仍然能够正常的创建一个action对象并返回给struts继续调用;


2.SSH整合应用;(重构项目中的资费管理模块);
1)重构资费列表显示功能;
a.梳理原处理流程:
/cost/findCost-->FindCostAction-->CostDao-->/cost/findCost.jsp;
b.采用Spring+hibernate实现CostDao组件;
--引入Spring开发框架jar包和配置文件;
--检测POJO和hbm.xml;
--修改HibernateCostDao;
--采用HibernateTemplate完成;
--将DAO扫描到Spring容器;
--将sessionFactory给DAO注入;
引入dbcp连接池jar包;
定义sessionFactory,并且给HibernateDaoSupport注入;
--测试DAO;
c.FindCostAction采用注入方式使用DAO;
--将Action扫描到Spring容器;
--将DAO给Action注入;
d.修改Action的配置;
--引入struts-spring-plugin.jar
--将class属性指定为Spring中action组件的bean id;
e.在web.xml中添加Listener配置和context-param,实例化Spring容器;
f.ssh整体测试;


Spring事务管理;

a.当Action或DAO使用了Hibernate延迟加载操作,需要将session对象推迟到
view层解析之后;可以采用Spring框架提供的opensessioninview filter实现;
使用方法:
--在web.xml中配置该Filter,但是一定要放在Struts的filter之前,因为一旦进入了struts的filter,
就不会再进入其他web.xml中配置的filter了,这个是它的特殊性;
因为StrutsFilter内部如果发现是Action请求,就会解析struts.xml,再去创建/调用Action对象,执行
execute方法,再根据返回标识调用result(JSP),所以在此过程中没有去调用过chain.doFilter()方法,
也就无法进入web.xml中指定的下一个filter;

<!-- 注意:这里的opensessioninview filter必须在Struts2的filter之前 -->
<filter>
	<filter-name>opensession</filter-name>
	<filter-class>
		org.springframework.orm.hibernate3.support.OpenSessionInViewFilter
	</filter-class>
</filter>

<filter-mapping>
	<filter-name>opensession</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>

--在Spring容器中的SessionFactory组件,其id值一定要指定为sessionFactory;

在Spring框架中提供了两种事物管理方法;
--当一个请求需要对数据表执行2个或2个以上的增删改语句时,为了保证数据的完整和一致性,必须
将所有对数据表的修改放在一个事务中执行;
--事务可以在程序发生意外时,保护数据的完整性;

//当事务作用在action的execute时;
public class DeleteAction{
	public String execute(){
		ADAO.delete();
		BDAO.delete();
	}
}


1)声明式事务管理(基于AOP配置实现);
a.xml配置;

--定位使用事务的目标组件和方法(切入点);
<bean id="deleteAction" class=".."></bean>
<aop:config>
......
<aop:pointcut id="actionPoint" expression="within(....DeleteAction)"/>
......
</aop:config>

--定义事务管理功能的方面组件(使用Spring提供的API);
JDBC方面组件:
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
<property name="dataSource" ref="dataSource"/>
</bean>
Hibernate方面组件:
<bean id="txManager" class="org.springframework.orm.hibernate3.HibernateTransactionManager">
<property name="sessionFactory" ref="sessionFactory"/>
</bean>

--定义事务和目标组件作用的通知(使用Spring提供的tx标签库配置);
<tx:advice id="txAdvice" transaction-manager="txManger">
	<tx:attributes>
		<tx:method name="get*" read-only="true"/>
		<tx:methodname="*" propagation="REQUIRED"/>
	</tx:attributes>
</tx:advice>

<asop:config>
	<aop:pointcut id="actionPoint" expression="within(....DeleteAction)"/>
	<aop:advisor advice-ref="txAdvice" pointcut-ref="actionPoint"/>
</aop:config>

例子:
不使用多对多关系映射,通过事务中的关联修改来完成对admin_info表和admin_role
的关联更新;

POJO:
public class AdminRole implements Serializable{
	//主键类型;
	private AdminRoleKey key;
	setter/getter...
}

public class AdminRoleKey implements Serializable{
	private Integer adminId;
	private Integer roleId;
	setter/getter...
}

hbm:
<class name="com.netctoss.pojo.AdminRole" table="ADMIN_ROLE">
<!--联合主键设置-->
<composite-id name="key" class="com.netctoss.pojo.AdminRoleKey">
	<key-property name="adminId" type="integer" column="ADMIN_ID">
	</key-property>
	<key-property name="roleId" type="integer" column="ROLE_ID">
	</key-property>
</composite-id>

dao:
@Repository
public class HibernateAdminDao
extends HibernateDaoSupport
implements IAdminDao{
	
	@Resource
	public void setFactory(SessionFactory sf){
		super.setSessionFactory(sf);
	}
	
	public void addAdmin(Admin admin) throws DAOException{
		super.getHibernateTemplate().save(admin);
	}
	......
}

public interface IAdminRoleDao{
	public void addAdminRole(AdminRole ar);
}

@Repository
public class HibernateAdminRoleDao
extends HibernateDaoSupport
implements IAdminRoleDao{
	
	@Resource
	public void setFactory(SessionFactory sf){
		super.setSessionFactory(sf);
	}
	
	public void addAdminRole(AdminRole ar) throws DAOException{
		super.getHibernateTemplate().save(ar);
	}
}

action:
@Controller
public class AddAdminAction{
	private Admin admin;
	//injection;
	@Resource
	private IAdminDao adminDao;
	@Resource
	private IAdminRoleDao adminRoleDao; 
	
	public String execute() throw Exception{
		//添加admin_info信息;
		adminDao.addAdmin(admin);
		//添加admin_role信息;
		for(Integer roleId:admin.getRoleIds()){
			AdminRole ar = new AdminRole();
			ar.setKey(new AdminRoleKey(admin.getId(),roleId));
			adminRoleDao.addAdminRole(ar);
		 }
		return "success";
	}
}

spring配置中加入:
<!--AOP事务管理-->
<bean id="txManager" class="org.springframework.orm.hibernate3.HibnernateTransactionManager">
	<property name="sessionFactory" ref="sessionFactory"></property>
</bean>
<!--指定事务通知-->
<tx:advice id="txAdvice" transaction-manager="txManager">
	<tx:attributes>
		<tx:method name="execute" propagation="REQUIRED" />
		<tx:method name="find*" read-only="true" />
	</tx:attributes>
</tx:advice>
<!--将通知和目标组件结合-->
<aop:config>
	<aop:pointcut id="targetPoint" expression="within(com.netctoss.action..*)">
	<aop:advisor advice-ref="txAdvice" pointcut-ref="targetPoint"/>
</aop:config>


b.注解配置;
--在spring配置文件中定义事务管理bean,HibernateTransactionManager;
需要注入SessionFactory;
--在spring配置文件中开启事务注解配置;
--在目标组件类中,使用@Transational注解标记;
该标记可以定义在类前和方法前;
放在类定义前:指定所有方法采取的事务管理策略;


在spring配置中添加:
<!--开启事务注解配置-->
<tx:annotation-driven transaction-manager="txManager"/>

在action类前或execute方法前:
@Transactional(propagation=Propagation.REQUIRED)


2)编程式事务管理(基于java编码实现);


Spring的MVC实现;
1)Spring框架的MVC结构;
Spring MVC是MVC思想的一种实现;
实现如下:
--C(控制器):
DispatcherServlet-->filter
Controller-->action
HandlerMapping-->ActionMapper(ActionProxy:执行拦截器然后action中execute方法)
--M(模型):
ModelAndView,POJO-->ValueStack,POJO
--V(视图):
ViewResolver,JSP-->Result,JSP;

2)Spring框架的处理流程;
a.客户端发送一个请求,请求交给DispatchServlet主控制器;
b.主控制器会调用HandlerMapping(该组件负责维护请求和Controller对应关系)
c.根据HandlerMapping的映射信息,调用一个Controller组件处理请求;
(Controller可以调用DAO组件)
d.Controller处理完毕,会返回一个ModelAndView对象(该组件封装了视图名和要传递的模型数据);
e.主控制器调用ViewResolver组件,解析出视图名,定位View组件;并将模型数据传递给View;
f.将View组件(JSP)标签和表达式解析,形成HTML相应输出;

例子:
引包,spring-webmvc.jar...
toLogin.do-->ToLoginController-->ModelAndView-->/WEB-INF/jsp/login.jsp

web.xml:

<servlet>
	<servlet-name>springmvc</servlet-name>
	<servlet-class>
		org.springframework.web.servlet.DispatcherServlet
	</servlet-class>
	<!--指定配置文件名以及地址-->
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	</init-param>
</servlet>
<servlet-mapping>
	<servlet-name>springmvc</servlet-name>
	<url-pattern>*.do</url-pattern>
</servlet-mapping>

spring配置:

<!--定义handlermapping组件-->
<bean id="handlermapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
	<property name="mappings">
		<!--定义请求和Controller对应关系-->
		<props>
			<prop key="toLogin.do">toLoginController</prop>
		</props>
		<props>
			<prop key="login.do">LoginController</prop>
		</props>
	</property>
</bean>

<!--定义viewResolver组件-->
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<property name="prefix" value="/WEB-INF/jsp/">
	</property>
	<property name="suffix" value=".jsp">
	</property>
</bean>

<bean id="toLoginController" class="org.tarena.controller.ToLoginController">
</bean>

<bean id="loginController" class="org.tarena.controller.LoginController">
</bean>


WEB-INF中:
login.jsp:
......
<form action="">
	<font color="red" size="2">${error}</font>
	用户名:<input type="text" name="name"/></br>
	密码:<input type="password" name="pwd"></br>
	<input type="submit" value="login">
</form>

ok.jsp;

action:

public class ToLoginController implements Controller{
	//默认执行的处理方法,相当与execute方法;
	public ModelAndView handleRequest(HttpServletRequest arg0,HttpServletResponse arg1) throws Exception{
	//封装视图名返回;
	ModelAndView mav = new ModelAndView("login");
	return mav;
	}
}

public class LoginController implements Controller{
	public ModelAndView handleRequest(HttpServletRequest req,HttpServletResponse res) throws Exception{
	String name = req.getParameter("name");
	String pwd = req.getParameter("pwd");
	Map model = new ModelMap();
	if("scott".equals(name)&&"123".equals(pwd)){
		//将模型数据和视图标识传出,页面通过标签或表达式来接收;
		model.put("name",name)
		return new ModelAndView("ok",model);
	}
	model.put("error","用户名或密码错误")
	return new ModelAndView("login",model);
}


使用注解来实现spring mvc:

web.xml:

<servlet>
	<servlet-name>springmvc</servlet-name>
	<servlet-class>
		org.springframework.web.servlet.DispatcherServlet
	</servlet-class>
	<!--指定配置文件名以及地址-->
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	</init-param>
</servlet>
<servlet-mapping>
	<servlet-name>springmvc</servlet-name>
	<url-pattern>*.do</url-pattern>
</servlet-mapping>

spring配置:

<!--定义handlermapping组件,支持注解的hanlermapping-->
<bean id="handlermapping" class="org.springframework.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
</bean>

<!----定义viewResolver组件,添加前缀+视图名+后缀>
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<property name="prefix" value="/WEB-INF/jsp/">
	</property>
	<property name="suffix" value=".jsp">
	</property>
</bean>

<!--开启组件扫描-->
<context:component-scan base-package="org.tarena">
</context:component-scan>


action:
@Controller
@Scoper("prototype")
public class ToLoginController{
	@	RequestMapping(value="/toLogin.do")
	public String execute(){
		return "login"; //返回视图名;
	}
}

@Controller
@Scope("prototype")
public class LoginController{
	@RequestMapping(value="/login.do")
	//自动将表单信息封装到User中,model用于向jsp传值;
	public String execute(HttpServletResquest req,User user,Model model){
		if("tom".equals(user.getPwd()) && "123".equals(user.getPwd())){
			model.addAttribute("name",user.getName());
			return "ok"; //成功进入ok.jsp;
		}
		model.addAttribute("error","用户名或密码错误");
		return "login";
	}
}

POJO:
public class User implements Serializable{
	private String name; //该值与表单组件属性必须一致;
	private String pwd;
	setter/getter...
}


spring至此告一段落;

*/

