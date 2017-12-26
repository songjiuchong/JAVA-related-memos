Struts2;


package prototype;

public class Struts {

	public static void main(String[] args) {

	}
}

/*

Struts;

1,什么是Struts;
Struts是基于MVC的轻量级框架;
1)基于MVC:实现了MVC;
2)轻量级:框架的侵入性有关,与侵入性成正比;
3)框架:基本代码的结构,减少一定的开发量,规范开发的代码,提升开发的效率;

mvc：
将软件分层;
model,view,controller;
历史:Jsp model1(Jsp+dao)->Jsp model2(mvc)->框架(Struts等);


2,为何用Struts;
1)健壮性:软件的稳定性;  v2.1.8;
2)易用性:好用,好学;
3)扩展性:针对未来而言,是未来发展的可能性; struts的扩展性尤为出色;
4)侵入性:与耦合度成正比;
	耦合:事物之间的关联程度,紧密程度;从某种角度来说耦合度越高(侵入性),越难以管理;

3,Struts历史;
Struts1:Apache;结构简单,灵活性高,20%+运用率;但是由于与servlet/jsp耦合紧密,发展遇到了瓶颈;

WebWork:技术更先进,核心是XWork;

Struts2:以WebWork为核心(收购了WebWork),进行发展;

题目:Struts1和Struts2的区别和联系？
答:Struts1与Struts2区别很大,不能理解为Struts1的升级版,而Struts2以XWork为核心,可以理解为WebWork的
升级版;

注意:struts2相当于将mvc中的controller做了更细致的分工(一个前端控制器,若干个action,原本的mvc模式中controller相当于需要管理所有的action组件),
而它本身其实是偏向于view层的,由前端控制器拦截客户端响应后分配action组件,而action组件来实现业务,调用DAO并且指明之后将前往哪个view,然后前端控制器将对应的视图页面请求发送给客户端;
struts2的控制机制在底层是通过servlet的过滤器来实现的,它会捕获所有向此web应用地址发送的请求然后转到org.apache.struts2.dispatcher.ng.filter这个定义好的filter类中
来处理这个请求,并且此类会根据struts.xml来对处理地址进行解析然后分配接下去的任务,初始化指定的action类并且调用其中的execute方法,再根据这个方法的返回值到struts.xml中查找对应的
jsp文件地址,然后将此地址发送给客户端显示;

注意:需要注意的是,当Struts2的底层filter在根据struts.xml配置文件找不到相应的请求路径对应的action时,将允许客户端跳过filter继续访问地址栏中的请求内容,由于这个机制,建议除了一些页面的样式文件,页面图片等(这些文件将会
在客户端生成html页面时要求向服务器获取,如果隐藏将无法获取),其他的jsp文件还是需要WEB-INF文件夹来限制访问的;

4,如何用Struts;

1)创建web项目;

2)导入Struts2的类库,放入lib;
基础jar包为:
xwork-core-2.1.6.jar,
struts2-core-2.1.8.jar,
ognl.jar,
freemarker.jar,
commons-fileupload.jar;

3)在web.xml中配置struts2前端控制器;

4)配置前端控制器所需要的配置文件struts.xml;

流程:
1)请求提交给前端控制器(在web.xml中配置)

web.xml文件:

<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" 
	xmlns="http://java.sun.com/xml/ns/javaee" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
	
<filter>
	<filter-name>Struts2</filter-name>
	//类搜索器使用:Ctrl+shift+t;或者在工具栏中的search(一个手电筒)选项使用;
	//找到StrutsPrepareAndExcuteFilter..打开后在左边的Package Explorer框中使用
	//Link with Editor功能在树中并锁定正在编辑的类,也就是刚才找到并打开的类,然后
	//右键选copy qualified name,放入下面的标签中;
	<filter-class>org.apache.struts2.dispatcher.ng.filter</filter-class>     
</filter>

<filter-mapping>
	<filter-name>Struts2</filter-name>
	<url-pattern>/*</url-pattern>  //拦截一切访问此web工程的请求并处理;
</filter-mapping>

</web-app>


2)前端控制器找到struts的配置文件struts.xml(相当于一个使用手册),
通过这个文件,来找具体处理业务的组件Action;



补充:
org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter类(前端控制器)其实就是一个大型的转发过滤器;
它的原理是将此项目相关的向服务器发送的所有请求拦截住,根据requestURI向struts.xml中查找相对应的标签,根据其包,类名(action类路径)
找到action类,实例化这个类并获得一个对象,将拦截器中获取的request中的parameter取出,利用action类中同名属性的setter方法将其赋值(这里还有一个类型转换的过程,
由于parameter中取出的值一定是String类型的,而action类中的属性不一定是String类型的),然后调用action类实例化对象的execute()方法,获得return返回值,再去struts.xml中找到对应的返回值,
再执行对应的动作:转发,重定向,或者json等;在真正执行这些动作前还有一些操作,比如转发前将把action类对象中所有的属性通过getter方法后再以setAttribute的形式放入request对象中,
此时还会实例化一个ValueStack对象,而整个action实例化对象将被同时放入ValueStack中(所谓的放入值栈其实就是类似于将这个action对象传入这个ValueStack对象中的一个具有栈功能,
也就是先进后出功能的Deque双端队列的顶部,之后在tomcat要求JVM根据.JSP页面信息解析JSP文件执行到OGNL表达式时会调用类似:Ognl.getValue("参数名",getValueStack对象.peek())来获取这个action实例,
这也是之后在JSP中可以通过OGNL/el表达式的形式获得这些参数的原因;而当解析到struts标签时同样会调用标签类中的类似方法获得这个对象加以操作,在迭代时还会将action实例中指定属性的迭代元素暂时放入队列顶端利用push放入元素,
然后依然利用Ognl.getValue("参数名",getValueStack对象.peek()),读取元素,最后pop弹出元素来完成一次迭代);如果是重定向那么浏览器将会重新发送一次请求,
也就会重新通过org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter过滤器;
还需注意的是如果根据请求地址在struts.xml中找不到相应的action对应,那么过滤器将执行xxx.doFilter（request,response）方法直接执行地址中的内容;


在src中建立一个struts.xml文件;
首先打开struts2.core.jar包,在其中找到struts-default.xml打开并将其中的版本信息以及
dtd(document type definition,文档类型定义)标签头复制到刚才创建的struts.xml文件中;
dtd:这个定义是为了检查文档创建的结构格式是否正确,元素和标签的定义是否符合规范;

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
"......"
"http://struts.apache.org/dtds/struts..."

<!--根元素-->
<struts>
	<!--
		1,name是包名,只是用来区分包的,根元素下可以有多个包,包名不能重复;
		2,extends是包继承,将struts2默认提供的组件(具有特定功能的代码)继承过来,避免了反复的copy;
		在struts-default.xml中找到内容为:package name="struts-default"..的标签,标签体中就是继承下来的内容;
		3,http://ip:port/appname/namespace/actionname.action
		如:http://localhost:8080/first/hello.action (action后缀可以省略);
		4,namespace其实就是访问这个包的一个域名,显然也是不能重复的;
		5,通常包名以模块命名,namespace以/模块命名;
	-->
	
	<package name="day01" namespace="/first" extends="struts-default"> 
		<!--
			1,name是用来找到Action的;
			2,class是具体要实例化并执行的类;
			3,method是要执行的类中的方法;
			4,每个包下可以有多个Action,他们之间不能重名;
			5,method可以省略不写,默认就是execute;
		-->
		<action name="hello" class="com.tarena.action.HelloStrutsAction" method="execute">
			<!--
				1,name是用来找到Result的;
				2,result元素中包含的信息时转发的去向页面;
				3,每个Action下可以有多个result,他们彼此不能重名;
			-->
			<result name="success">
				/WEB-INF/jsp/hello.jsp
			</result>
			<result name="error">
				/WEB-INF/jsp/error.jsp
			</result>
		</action>
	</package>
</struts>

在com.tarena.action包中建立HelloStrutsAction类;

package com.tarena.action;

public class HelloStrutsAction{
	//Action中将被执行的方法,他的返回值是struts.xml中result的name,必须是一个字符串;
	public String execute(){
		//返回success,将找到名称为success的result;
		return "success";
	}
}

在WEB-INF下建立一个jsp文件夹,然后创建一个hello.jsp;

<%@page pageEncoding="utf-8"%>
<html>
	<head></head>
	<body>
		<h1>Hello,Struts2.</h1>
	</body>
</html>

在WEB-INF下建立一个jsp文件夹,然后创建一个error.jsp;

<%@page pageEncoding="utf-8"%>
<html>
	<head></head>
	<body>
		<h1 style="color:red">Error.</h1>
	</body>
</html>


3)Action处理业务流程,调用DAO;
4)Action通知前端控制器,继续处理请求;
5)前端控制器将请求转发给JSP;



5,用Struts开发项目;

1)标准研发流程:需求调研->需求概要分析->需求分析->需求交底(具体需要实现的细节)->概要设计->详细设计
->单元自测->需求验证->功能测试->集成测试->上线测试;


*****NetCtoss项目(part1)*****

1,分析需求,简单设计:
模块名(关键字):cost;
功能动作命名:查询find,新增add,修改modify,删除delete;

2,设计好数据库表,建表;
eclipse中window->show view->others->MyEclipse Database->DB Browser->在B Browser这个控制台中右键->new...
执行sql语句:新建一个xxx.sql文件,连接所需的数据库,然后在.sql文件中执行sql语句;

3,新建web项目;

web.xml文件:

<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" 
	xmlns="http://java.sun.com/xml/ns/javaee" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
	
<filter>
	<filter-name>Struts2</filter-name>
	<filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>     
</filter>

<filter-mapping>
	<filter-name>Struts2</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>

4,导入Struts2类库;

5,配置前端控制器;

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
"......"
"http://struts.apache.org/dtds/struts..."

<struts>
	<package name="cost" namespace="/cost" extends="struts-default"> 
		<action name="findCost" class="com.netctoss.action.FindCostAction">
			<result name="success">
				/WEB-INF/cost/findCost.jsp
			</result>
		</action>
	</package>
</struts>
	

6,配置struts.xml

7,写Action;
package com.netctoss.action;

import java.util.List;

import com.netctoss.dao.DAOException;
import com.netctoss.dao.DAOFactory;
import com.netctoss.dao.ICostDao;
import com.netctoss.pojo.Cost;

//用来封装业务流程:数据+算法;
//根据输入来计算输出;
public class FindCostAction {
	
	//只需要这样声明一个需要发送给view层的"输出属性",并给出getter/setter方法,
	//struts2就会在execute方法返回后将这个"输出属性"发送给view层;
	private List<Cost> costList; 
	
	public List<Cost> getCostList() {
		return costList;
	}

	public void setCostList(List<Cost> costList) {
		this.costList = costList;
	}

	public String execute(){
		
		//1,调用DAO,查询出全部的数据;
		ICostDao dao = DAOFactory.getCostDAO();
		try {
		//2,把数据发送到页面; 
			costList= dao.findAll();  //此处struts2将自动把赋给costList的内容作为请求内容发送给view层;
		} catch (DAOException e) {
			e.printStackTrace();
			return "error";
		}
		
		//3,返回result的值,使得前端控制器可以通过result找到对应的jsp;
		return "success";
		
	}
}

8,写jsp;

F:\alternative lab\NetCtoss\NETCTOSS_V02\fee\fee_list.html;
将styles和images文件夹放在WebRoot下,由于WEB-INF文件夹相当于是隐藏的,所以在jsp文件中写styles/images
路径的时候直接跳过了这层结构;
将jstl.jar和standard.jar导入lib,准备将jsp用jstl标签优化;
在standard.jar的META-INF中找到c.tld,将其中的<uri>http://java.sun.com/jsp/jstl/core</uri>
中的内容放入:
<%@ page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
如果tomcat版本小于6则需要添加:
<%@ page isELIgnored="false" %>来启用el表达式;
......参考web project:NetCtoss->findCost.jsp;


9,设计util包;

在包中创建db.properties文件(也可以用xml);

user=...
password=...
url=jdbc:oracle:thin:@192.168.0.26:1521:tarena
driverName=oracle.jdbc.driver.OracleDriver


//数据库连接工具类;
package com.netctoss.util;

public class DBUtil{
	private static String user;
	private static String password;
	private static String driverName;
	private static ThreadLocal<Connection> tl = new ThreadLocal<Connection>();
	
	static{
		Properties p = new Properties();
		try{
		//通过加载DBUtil的类加载器和它加载此类的流读取db.properties文件,注意参数地址是从src中开始的;
		p.load(DBUtil.class.getClassLoader().getResourceAsStream("db.properties"));
		user = p.getProperty("user");
		password = p.getProperty("password");
		url = p.getProperty("url");
		driverName = p.getProperty("driverName");
		Class.forName(driverName);
		}catch(IOException e){
			e.printStackTrace();
			throw new RuntimeException("读取数据库配置文件失败");
		}
		
		//获取数据库连接,返回一个连接;
		public static Connection getConnection(){
			Connection con = tl.get();
			if(con == null){
				try{
				con = DriverManager.getConnection(url,user,password);
				tl.set(con);
				}catch(SQLException e){
					e.printStackTrace();
					throw new RuntimeException("创建数据库连接失败");
				}
			}
			return tl.get();
		}
		
		public static void closeConnection(){
			Connection con = tl.get();
			if(con !=null){
				try{
				con.close();
				tl.set(null);
				}catch(SQLException e){
					e.printStackTrace();
					throw new RuntimeException("关闭数据库连接失败");
				}
			}
		}
	}
}

//测试是否能获得一个连接;
public static void main(String[] args) throws Exception{
	System.out.println(getConnection());
}


10,设计pojo(plain ordinary java object)包(实体类包,也就是vo,value-object包):

package com.netctoss.pojo;

public class Cost{
	private Integer id;  //如果接收的数据可以是Null,这里就必须用封装类;
	private String name;
	private Integer baseDuration; 
	private Double baseCost;
	private Double unitCost;
	private String status;
	private String descr;
	private Date createTime;
	private Date startTime;
	getter/setter...
	
}

11,设计DAO;

1)定义一个接口;

package com.netctoss.dao;

import java.util.List;

import com.netctoss.pojo.Cost;

public interface ICostDao{
	List<Cost> findAll() throws DAOException;
}

2)自定义异常类;
package com.netctoss.dao;

public class DAOException extends Exception{

	public DAOException(String arg0,Throwable arg1){
		super(arg0,arg1);
	}
	
}

3)DAO类;

package com.netctoss.dao;

import java.util.List;

import com.netctoss.pojo.Cost;

public class CostDaoImpl implements ICostDao{

	private String findAllSql = "select * from COST";
	
	//查询所有Cost记录并返回一个cost对象列表(每行记录代表一个cost对象);
	public List<Cost> findAll() throws DAOException{
		 List<Cost> list = null;
		 Connection con = DBUtil.getConnection();
		 try{
		 	PreparedStatement ps = con.prepareStatement(findAllSql);
		 	ResultSet rs = ps.executeQuery();
		 	while(rs.next()){
		 		Cost c = createCost(rs);
		 		if(list==null)
		 		list = new ArrayList<Cost>();
		 		list.add(c);
		 	}
		 return list;
		 }catch(SQLException e){
		 	e.printStackTrace();
		 	throw new DAOException("查询全部的资费数据时发送错误!",e);
		 }finally{
		 	DBUtil.closeConnection();
		 }
	}
	
	private Cost createCost(ResultSet rs) throws SQLException{
		cost c = new Cost();
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
	
//单元测试;
public static void main(String[] args)throws Exception{
	ICostDao dao = new CostDaoImpl();
	List<Cost> costs = dao.findAll();
	for(Cost c:costs){
		System.out.println("id:"+c.getId()+" name:"+c.getName());
	}
}
}


package com.netctoss.dao;

//DAO工厂类;
public class DAOFactory {
	
	//为了节省资源不用每次在调用CostDaoImpl中方法时都创建一个新的对象,
	//这里利用单例方法静态声明了一个对象,以供调用;
	private static ICostDao costDao = new CostDaoImpl();
	
	//返回ICostDat接口的实例;
	public static ICostDao getCostDAO(){
		
		return costDao;
	}
}



完成NetCtoss项目的资费列表功能(分页);
1,为何要分页:
a,使页面看起来美观,便于查找;
b,处于效率考虑,不至于过多数据把浏览器卡死;

2,分页的形式:
a,假分页:一次请求就将所有的数据加载到页面,当翻页时,不再重新请求,只需要从得到的数据中找到那一页的数据展示即可;
优点:流畅,不会卡;
缺点:数据量大的时候,效率差,具有很大的局限性;

b,真分页:一次请求就从数据库中查找当前页要展示的数据,当翻页的时候,重新发请求;
优点:每次只查询一页的数据,对数据库造成的压力较小;
缺点:需要反复重新请求,对资源消耗较大;

3,如何分页;
前提:需要已知当前页(page),已知每页数据的容量(pageSize);
假设:12条数据,页容量是pageSize=5;page=2;
推演:第一页 1,2,3,4,5
	第二页 6,7,8,9,10
	第三页 11,12
结果:5<行数<11
计算:(page-1)*pageSize+1  //本页的第一条记录在数据库中所有记录的行数;
SQL中如何反映:oracle的rownum;
rownum计数器,必须从1开始;

select * from(
select c.*,rownum r from COST c
where rownum<11)
where r>5

页数totalPage,总行数rows
totalPage=rows/pageSize(如果有小数则进位);


*****NetCtoss项目(part2)*****

CostDaoImpl.java中更新:

	//分页查找记录;
	private String findByPageSql="select * from( "+
	"select c.*,rownum r from COST c "+
	"where rownum<?) where r>?";
	
	public List<Cost> findByPage(int page, int pageSize) throws DAOException {
		 List<Cost> list = null;
		 try{
			Connection con = DBUtil.getConnection();
		 	PreparedStatement ps = con.prepareStatement(findByPageSql);
		 	
		 	//下一页的最小行;
		 	int nextMin = page*pageSize + 1;
		 	//上一页的最大行;
		 	int lastMax = (page-1)*pageSize;
		 	
		 	ps.setInt(1,nextMin);
		 	ps.setInt(2,lastMax);
		 	ResultSet rs = ps.executeQuery();
		 	while(rs.next()){
		 		Cost c = createCost(rs);
		 		if(list==null)
		 		list = new ArrayList<Cost>();
		 		list.add(c);
		 	}
		 return list;
		 }catch(SQLException e){
		 	e.printStackTrace();
		 	throw new DAOException("分页查询数据时发生错误!",e);
		 }finally{
		 	DBUtil.closeConnection();
		 }
	}	

	//查询总页数;
	private String findTotalRowsSql = "select count(*) from cost";
	
	public int findTotalPage(int pageSize) throws DAOException {
		 try{
			Connection con = DBUtil.getConnection();
		 	PreparedStatement ps = con.prepareStatement(findTotalRowsSql);
		 	ResultSet rs = ps.executeQuery();
		 	rs.next();
		 	int rows = rs.getInt(1); //得到总行数;
		 	if(rows%pageSize==0){
		 		//如果整除,则返回相除的值;
		 		return rows/pageSize;
		 	}else{
		 		//有余数的情况下,进位取整;
		 		return rows/pageSize+1;
		 	}
		 }catch(SQLException e){
		 	e.printStackTrace();
		 	throw new DAOException("查询总页数时发生错误!",e);
		 }finally{
		 	DBUtil.closeConnection();
		 }	
	}


在ICostDao中更新;

	//查找所有记录;
	List<Cost> findAll() throws DAOException;
	
	//根据页码,每页容量来查询当前页的自费记录;
	//page,当前页码;pageSize,每页容量;
	List<Cost> findByPage(int page,int pageSize) throws DAOException;
	
	//根据页容量查询所有数据一共可以分为几页,返回总页数;
	int findTotalPage(int pageSize) throws DAOException;


在FindCostAction.java中更新:

package com.netctoss.action;

import java.util.List;

import com.netctoss.dao.DAOException;
import com.netctoss.dao.DAOFactory;
import com.netctoss.dao.ICostDao;
import com.netctoss.pojo.Cost;

//用来封装业务流程:数据+算法;
//根据输入来计算输出;
public class FindCostAction {
	//需要注意的是,一下的输入输出属性只是根据其基本功能来划分的,而实际上只要有getter/setter方法,这些
	//定义后的属性既是输入属性又是输出属性,也就是说从客户端传入的参数被Struts2接收赋值到action后,在
	//之后的发送给客户端的请求中也会将这个属性再次以输出属性发送过去;(参考ValueStack原理);
	
	//输入属性;
	private int page=1; //当前页;这里赋值为1是为了让第一次访问此页面时能够显示第一页的内容;
	
	private int pageSize=5; //页容量;
	

	public int getPage() {
		return page;
	}

	public void setPage(int page) {
		this.page = page;
	}
	
	public int getPageSize() {
		return pageSize;
	}

	public void setPageSize(int pageSize) {
		this.pageSize = pageSize;
	}


	//只需要这样声明一个需要发送给view层的"输出属性",并给出getter/setter方法,
	//struts2就会在execute方法返回后将这个"输出属性"发送给view层;
	private List<Cost> costList; 
	
	private int totalPage; //总页数;
	
	
	public List<Cost> getCostList() {
		return costList;
	}

	public void setCostList(List<Cost> costList) {
		this.costList = costList;
	}
	
	public int getTotalPage() {
		return totalPage;
	}

	public void setTotalPage(int totalPage) {
		this.totalPage = totalPage;
	}
	

	public String execute(){
		
		//1,调用DAO,查询出全部的数据;
		ICostDao dao = DAOFactory.getCostDAO();
		try {
		//2,把数据发送到页面; 
			//costList= dao.findAll();  //此处struts2将自动把赋给costList的内容作为请求内容发送给view层;
			costList = dao.findByPage(page, pageSize); //此处的page,pageSize属性Struts2已经从页面的请求中取得并赋值到了action类中的相应属性里,所以此处可以直接调用;
			totalPage = dao.findTotalPage(pageSize);
		} catch (DAOException e) {
			e.printStackTrace();
			return "error";
		}
		
		//3,返回result的值,使得前端控制器可以通过result找到对应的jsp;
		return "success";
		
	}
}

findCost.jsp中更新:

                    <table id="datalist">
                        <tr>
                            <th>资费ID</th>
                            <th class="width100">资费名称</th>
                            <th>基本时长</th>
                            <th>基本费用</th>
                            <th>单位费用</th>
                            <th>创建时间</th>
                            <th>开通时间</th>
                            <th class="width50">状态</th>
                            <th class="width200"></th>
                        </tr>    
                        <c:forEach items="${costList}" var="cost">              
	                        <tr>
	                            <td>${cost.id}</td>
	                            <td><a href="fee_detail.html">${cost.name}</a></td>
	                            <td>${cost.baseDuration}</td>
	                            <td>${cost.baseCost}</td>
	                            <td>${cost.unitCost}</td>
	                            <td>${cost.createTime}</td>
	                            <td>${cost.startTime}</td>
	                            <td>
									<c:if test="${cost.status==0}">开通</c:if>
									<c:if test="${cost.status==1}">暂停</c:if>
								</td>
	                            <td>                                
	                                <input type="button" value="启用" class="btn_start" onclick="startFee();" />
	                                <input type="button" value="修改" class="btn_modify" onclick="location.href='fee_modi.html';" />
	                                <input type="button" value="删除" class="btn_delete" onclick="deleteFee();" />
	                            </td>
	                        </tr>
                        </c:forEach>
                    </table>
       .......
                </div>
                <!--分页-->
                <div id="pages">
                	<c:choose>
                		<c:when test="${page==1}">
                			<a href="#">上一页</a>
                		</c:when>
                		<c:otherwise>
                			<a href="findCost.action?page=${page-1}">上一页</a>
                		</c:otherwise>
                	</c:choose>
        	        
        	        <c:forEach begin="1" end="${totalPage}" var="p">
        	        	<c:choose>
        	        		<c:when test="${p==page}">
        	        			<a href="findCost.action?page=${p}" class="current_page">${p}</a>
        	        		</c:when>
        	        		<c:otherwise>
        	        			<a href="findCost.action?page=${p}">${p}</a>
        	        		</c:otherwise>
        	        	</c:choose>
                    	
                    </c:forEach>
                    
                	<c:choose>
                		<c:when test="${page==totalPage}">
                			<a href="#">下一页</a>
                		</c:when>
                		<c:otherwise>
                			<a href="findCost.action?page=${page+1}">下一页</a>
                		</c:otherwise>
                	</c:choose>
                	
                </div>
            </form>
        </div>
        
        
        
OGNL表达式(Struts2自己的标签所使用的表达式);
1,概念:OGNL(Object graph navigation language)是一种功能强大的
表达语言;可以让用户使用简单的表达式访问对象层,方便用户使用或者构建自己的框架;
需要ognl.jar包来实现,其核心的类是Ognl;

2,原理:ognl表达式传递给ognl的解析器,解析器来解析这个表达式,如果表达式以#开头,则从
Map对象中取值,否则从root对象(JavaBean)中取值;

3,用法: Ognl.getValue("OGNL表达式",root对象);

例子:
public class OgnlTester{
	public static void main(String[] args)throws Exception{
		Foo foo = new Foo();
		//1,访问基本类型;
		System.out.println(Ognl.getValue("id",foo));
		System.out.println(Ognl.getValue("name",foo));
		
		//2,访问数组,集合;
		 System.out.println(Ognl.getValue("ary[0]",foo));
		 System.out.println(Ognl.getValue("list[1]",foo));
		 
		//3,访问Map;
		  System.out.println(Ognl.getValue("map.bbb",foo));
		   
		//4,访问时做运算;
		  System.out.println(Ognl.getValue("id+10",foo));
		  System.out.println(Ognl.getValue("'my name is'+name",foo));
		  
		//5,访问时调用方法;
		  System.out.println(Ognl.getValue("name.toUpperCase()",foo)); 
		 
		//6,访问时调用静态方法;
		  
		 //调用静态方法不需要实例化对象,所以这里第二个参数是null,但是需要将调用方法类的路径用@@声明;
		 System.out.println(Ognl.getValue("@com.tarena.test.Foo@talk()",null)); 
		 //相当于初始化foo,然后用它的属性name作为参数传入此方法;
		 System.out.println(Ognl.getValue("@com.tarena.test.Foo@talk(name)",foo)); 
		 
		//7,访问复杂的对象;
		  Dept dept = new Dept();
		  dept.setDeptName("science");
		  Emp e1 = new Emp("zhangsan",10000.00,dept);
		  Emp e2 = new Emp("lisi",20000.00,dept);
		  List<Emp> emps = new ArrayList<Emp>();
		  emps.add(e1);
		  emps.add(e2);
		  dept.setEmps(emps);
		  System.out.rpintln(Ognl.getValue("dept.deptName",e1));
		  System.out.rpintln(Ognl.getValue("emps[0].salary+emps[1].salary",dept));
	
		  //8,访问时直接创建集合对象;
		    
		   Object obj = Ognl.getValue("{1,2,3}",null);
		   System.out.println(obj);  // [1,2,3];
		   System.out.println(obj.getClass().getName());  // java.util.ArrayList;
		    
		   //9,访问时创建Map对象;
		   obj = Ognl.getValue("#{'aaa':'a','bbb':'b'}",null);
		   System.out.println(obj);  // {aaa=AAA,bbb=BBB};
		   System.out.println(obj.getClass().getName());  // java.util.LinkedHashMap;
		    
		    //10,可以从不同的对象中进行访问;
		    Map<String,String> ctx = new HashMap<String,String>();
		    ctx.put("name","tarena");
		    System.out.rpintln(Ognl.getValue("#name",ctx,foo)); //不加#从foo中取name对象,加上#从Map(ctx)中取;
		     
	}
}

package com.tarena.test.Foo;

public class Foo{
	private int id;
	private String name;
	private String[] ary;
	private List<String> list;
	private Map<String,String> map;
	
	getter/setter...
	
	public Foo(){
		id = 1;
		name = "zhangsan";
		ary = new String[]{"struts","hinernate","spring"};
		list = new ArrayList<String>();
		list.add("java");
		list.add("c#");
		list.add("php");
		map = new HashMap<String,String>();
		map.put("aaa","AAA");
		map.put("bbb","BBB");
		map.put("ccc","CCC");
	}
	
	public static String talk(){
		return "hello";
	}
	
	public static String talk(String name){
	return "hello,"+name;
	}
}

public class Dept{
	private String deptname;
	private List<Emp> emps;
	
	getter/setter;
	
}

public class Emp{
	private String name;
	private Double salary;
	private Dept dept;
	
	getter/setter;
	
	public Emp(String name,Double salary,String dept){
		this.name=name;
		this.salary=salary;
		this.dept=dept;
	}
}



ValueStack(介绍一些struts2的标签);
1,封装前后台交互的数据,可以用ognl表达式访问的一个容器;
2,工作原理:OGNL表达式传递给ValueStack的解析器,解析器会从栈顶向下依次查找数据,
对于栈里的每一个对象,都以它为root查找,如果表达式前有#,则不从栈中而是从Map对象中找;

struts2中ValueStack组件的工作流程:
1,当服务器发送请求给服务器,被前端控制器拦截;
2,通过struts.xml找到action类;
3,创建ValueStack栈容器(其实就是一个引擎,包括了一个解析器,声明了一块栈式空间,一个Map集合);
4,实例化action类(将服务器发送过来的request中参数赋值到action相应的属性中),并将它放入ValueStack中的栈顶;
5,执行execute方法(execute方法执行完后,将会对原本request对象中的参数进行一次同步),然后根据返回值在struts.xml中找到result后找到对应的jsp开始解析;
6,在解析jsp表达式的过程中,如果遇到struts标签和其中的OGNL表达式,OGNL引擎将表达式语句做合法性验证与初步解析后传入
ValueStack的解析器,然后解析器将会根据表达式内容从其栈顶开始取值,又由于action实例在之前被放入了栈顶,所以这里就能
从中取到相应的内容(当然如果表达式是#开头,解析器将会指向Map集合中去取内容),最后将解析完毕后的jsp发送给客户端;

注意:
1,ValueStack中的Map保存了request,session,application,action等对象,相当于一个全集,而ValueStack的栈是为了通过栈结构提高提取特定内容效率的考虑而实现的;
2,以上流程为一次请求和相应,每次新的请求会重新创建新的ValueStack,初始化新的action;
3,execute方法执行完后,将会对原本request对象中的参数进行一次同步赋值,保证之后从request中获取的数据将和action实例中相应参数的值保持同步;


例子:
struts.xml:
<struts>
	<package name="demo" namespace="/demo" extends="struts-default"> 
		<action name="valuestack" class="com.tarena.action.ValueStackAction">
			<result name="success">
				/WEB-INF/jsp/valuestack.jsp
			</result>
		</action>
	</package>
</struts>

package com.tarena.action;

这是一个验证ValueStack运行原理的例子;
public class ValueStackAction{
	//输入属性;
	private String name;
	private String[] langs;
	private List<Emp> emps;
	
	getter/setter;
	
	public String execute(){
		name="Tarena";
		langs = new Srting[]{"1","2","3"};
		emps = new ArrayList<Emp>();
		Emp e1 = new Emp("a",10000);
		Emp e2 = new Emp("b",20000);
		emps.add(e1);
		emps.add(e2);
		return "success";
	}
	 
}

public class Emp{
	private String name;
	private Double salary;
	
	getter/setter;
	
	constructive method...
	
}

valuestack.jsp;

<%@taglib uri="/struts-tags" prefix="s"%> //struts标签导入地址在struts2-core.jar中struts-tags.tld的<uri>/struts-tags</uri>中;
<html>
	<head></head>
    <body>
    	<h1>验证valuestack的例子</h1>
    	<h3>
    		<s:debug/> 
    	</h3>
    	<h3>
    		Name:
    		<s:property value="name"/> 
    	</h3>
     	<h3>
    		Lang:
    		<s:property value="langs[1]"/> 
    	</h3>   	
    	<h3>
    		薪资的和:
    		<s:property value="emps[0].salary+emps[1].salary"/> 
    	</h3>  
     	<h3>
    		Request:
    		<s:property value="#request"/> 
    	</h3> 
      	<h3>
    		Request:
    		<s:iterator value="emps"> 
    			<s:debug/>
    			<s:property value="name"/>
    		</s:iterator > 
    	</h3>       	  
     	<h3>
    		<s:iterator begin="1" end="3" var="p"> 
    			<s:debug/>
    			<s:property/>
    			<s:property value="#p"/>
    		</s:iterator> 
    	</h3>     	 
      	<h3>
    		<s:iterator begin="1" end="3" var="p"> 
    			<s:property value="#p"/>
    			<s:if test="#p==1">aaa</s:if>
    			<s:else>bbb</s:else>
    		</s:iterator> 
    	</h3>        		  	
    </body>
</html>



Struts2标签;

1,<s:debug/>:用来给开发人员调试的,展开之后能看见ValueStack里的内容;
注意:
1)调试完毕后需要删除,正式发布时不能有这个标签;
2)一个页面有多个debug标签时只会打开第一个debug对应的内容,所以需要保持只在
测试处保留一个标签;

2,<s:property value="OGNL表达式"/>:用于在界面上显示值,OGNL默认以ValueStack栈顶为root对象来取值的,
如果栈顶找不到想要的值,则依次向下从栈中找值,如果在栈底也为发现则返回空;
   <s:property/>:表示直接输出栈顶内容;
   
3,<s:iterator value="需要被遍历的集合或者数组"> </s:iterator> 
a,循环前ValueStack栈顶是action;
b,第一次循环将循环变量直接放到ValueStack栈顶,这样action就处于栈的第二个位置;
c,第n次循环中,ValueStack会移除栈顶数据,并将本次的循环数据置于栈顶,此时action依旧
处于栈的第二个位置;
d,最后一次循环结束后,ValueStack会移除栈顶数据,但此时不存在其他需要被循环的数据了,所以action重新
回到栈顶;

4,<s:iterator begin="" end="" var=""/></s:iterator>
在此循环中,ValueStack栈顶的变化与上一种用法相同,可以通过定义var临时变量来从Map中
找到这个变量表示的值(正在被迭代，处于栈顶的变量);

5,<s:if test="返回boolean值的OGNL表达式"></s:if>
   <s:elseif test=""><s:elseif>
   <s:else></s:else>

6,<s:date value="" format=""/>


补充:
当action将request,response对象转发给一个JSP文件后,tomcat容器将命令jvm去解析这个.jsp页面(其实就是解析成一个servlet类的文件),其中的标签会根据jsp的taglib指令中的
路径去查找(lib中所有的tld文件在工程运行时已经被加载)并根据tld文件中对应方法的信息将这些标签和表达式(EL,OGNL)翻译成若干个方法体,
其中例如iterator标签中根据其迭代对象已经将整个遍历过程展开,集合中的各个元素都已被直接展现在文本中的对应方法体内;
值得注意的是,在文本的最开始首先会根据固定方法获取一个pageContext对象,然后提供了包括request,response,out,session,application等
属性,并且通过pageContext对象的各种get方法将这些对象赋值,所以在JSP页面中可以直接使用这些属性名;
然后JVM会将此页面编译成.class文件,在tomcat中:\tomcat\apache-tomcat-5.5.23\work\Catalina\localhost\webappName\org\apache\jsp\
中可以找到想要JSP文件的.java和.class文件;最后JVM运行JSP的.class文件,执行了文件中生成的那些方法体,并通过输出流把信息通过tomcat中封装好
的通信层利用http标准协议发送给浏览器;


用struts2的标签重构NetCtoss的资费列表;

1,Struts2对EL表达式的支持;

对request进行了包装:
System.out.println(request.getClass().getName());  //StrutsRequestWrapper;

在包装的对象中重写了getAttribute()方法,大致是这样做的:
public Object getAttribute(String name){
	Object obj = request.getAttribute(name);
	if(obj==null){
		obj = 从ValueStack中的栈顶取值;
	}
	return obj;
}

原理:原本在未使用struts2时当客户端发送一个请求,request中将会保存所有的请求参数,
服务器端接收后若是转发给一个jsp页面(重定向的话将会声明新的request和response),
会把自己原本的request,response对象发送这个jsp页面,而发送前也可以在request对象
中添加参数;之后在解析jsp页面时,EL表达式将会从request对象中寻找相应的内容,无论是
浏览器发送的请求参数还是在action中添加的参数,都能被找到并调用;
但是当使用struts2时,客户端发送的一个请求,request中仍旧会有请求参数,request对象
先被过滤器接收后,它们会被赋值到相应action中的属性中,之后过滤器相当于会将请求再次
转发到相应的jsp页面中,所以此时用EL表达式仍旧可以从request中取得客户端发送的参数;
而如果使用OGNL表达式的情况下在execute方法执行完毕后所有action对象中的属性将被以
setAttribute()的方式放入request中,所以之后还是能够取得;但是如果在Struts标签中以迭代
的方式取得元素呢,比如:
<s:iterator value="costList">
	${"name"}
	<s:property value="name"/>
</s:iterator>
这样在pageContext,request,session,application中是找不到名为"name"的元素的,所以此时
EL表达式调用了被包装过的request对象直接去ValueStack栈顶对象中找"name"属性就能够得到与OGNL表达式相同的值了,
这就是Struts2要重写getAttribute()的原因了;


*****NetCtoss项目(part3)*****

用struts2的标签重构NetCtoss的资费列表;

                        <s:iterator value="costList">              
	                        <tr>
	                            <td><s:property value="id"/></td>
	                            <td><a href="fee_detail.html"><s:property value="name"/></a></td>
	                            <td><s:property value="baseDuration"/></td>
	                            <td><s:property value="baseCost"/></td>
	                            <td><s:property value="unitCost"/></td>
	                            <td><s:property value="createTime"/></td>
	                            <td><s:property value="startTime"/></td>
	                            <td>
									<s:if test="status==0">开通</s:if>
									<s:else>暂停</s:else>
								</td>
	                            <td>                                
	                                <input type="button" value="启用" class="btn_start" onclick="startFee();" />
	                                <input type="button" value="修改" class="btn_modify" onclick="location.href='fee_modi.html';" />
	                                <input type="button" value="删除" class="btn_delete" onclick="deleteFee();" />
	                            </td>
	                        </tr>
                        </s:iterator>
                    </table>
                    <p>业务说明：<br />
                    1、创建资费时，状态为暂停，记载创建时间；<br />
                    2、暂停状态下，可修改，可删除；<br />
                    3、开通后，记载开通时间，且开通后不能修改、不能再停用、也不能删除；<br />
                    4、业务账号修改资费时，在下月底统一触发，修改其关联的资费ID（此触发动作由程序处理）
                    </p>
                </div>
                <!--分页-->
                <div id="pages">
                	<s:if test="page==1">
                		<a href="#">上一页</a>
                	</s:if>
                	<s:else>
                		<a href="findCost.action?page=<s:property value="page-1"/>">上一页</a>
                	</s:else>
        	        <s:iterator begin="1" end="totalPage" var="p">
        	        	<s:if test="#p==page">
        	        		<a href="findCost.action?page=<s:property/>" class="current_page"><s:property/></a>
        	        	</s:if>
        	        	<s:else>
        	        		<a href="findCost.action?page=<s:property value="#p"/>"><s:property value="#p"/></a>
        	        	</s:else>
                    </s:iterator>
                	<s:if test="page==totalPage">
                		<a href="#">下一页</a>
                	</s:if>
                	<s:else>
                		<a href="findCost.action?page=<s:property value="page+1"/>">下一页</a>
                	</s:else>



Action基本原理:

1,Struts2有6大核心组件;

1) 前端控制器FC;
2) Action;
3) ValueStack;
4) 拦截器 Interceptor;
5) Result;
6) 标签tags;


2,工作原理;

请求提交前端控制器,
根据配置文件找到Action;
创建VS栈容器;
实例化Action放栈顶;
历经层层拦截器,调用Action得到输出;
根据方法返回值,调用Result输出;


3,在struts2中取得session;

1)通过struts2中的ServletActionContext取到request,然后通过request取到session;
HttpServletRequest request = ServletActionContext.getRequest();  //获得原始的request对象;
HttpSession session1 = request.getSession(); //原始的session对象;

2)通过ActionContext直接取到session,但是这个session是Map类型的;
ActionContext ctx = ActionContext.getContext();
Map<String,Object> session2 = ctx.getSession();  //Struts2包装后的request对象取得的session;
session2.put("user","zhangsan");
System.out.println(session2.get("user"));  //在包装后的session中获得参数对象;
System.out.println(session1.get("user"));  //在原始的session中也能获得同步后的对象;

3)在实例化Action类时当Struts2发现此Action类实现了SessionAware接口就会将包装后的session对象传入setSession方法并且利用Action对象调用此方法;
public class Action implements SessionAware{

	private Map<String,Object> session;
	
	public void setSession(Map<String,Object> ctx){
		session = ctx;
	}
	
}



*****NetCtoss项目(part4)*****

//建立一个通用的父Action类去实现SessionAware接口,在子类Action被实例化时会先实例化父Action类并获得此session对象,
之后子类Action就可以随意调用;为了防止此Action类被直接实例化并调用,此处可以声明为abstract的,其子类在实例化的时候仍旧
可以从这里继承到相应的内容,因为abstract类虽然自身不能直接被实例化,但是它是含有构造方法的,子类实例化时会先调用抽象父类
的构造方法,相当于间接实例化了父类;

public abstract class BaseAction implements SessionAware{

	protected Map<String,Object> session;
	
	public void setSession(Map<String,Object> ctx){
		session = ctx;
	}
	
}

public class Action extends BaseAction{
	//可以直接调用session对象;
}

struts.xml:

	<!--登录模块login-->
	<package name="login" namespace="/login" extends="struts-default"> 
	
		<!--跳转到登录页面的Action-->
		<action name="toLogin">
			<result name="success">
				/WEB-INF/main/login.jsp
			</result>
		</action>
		
		<!--登录验证的Action-->
		<action name="login" class="com.netctoss.action.LoginAction">
			<!--登录成功进入到项目的首页-->
			<result name="success">
				/WEB-INF/main/index.jsp
			</result>
			<!--登录失败停留在登录页面-->
			<result name="error">
				/WEB-INF/main/login.jsp
			</result>
		</action>
		
	</package>
	
注意:在action标签中可以不指定class,如果没有,则由struts2自动实例化一个ActionSupport类,
此类中只有一个默认的返回"success"的execute方法;
	
action
public class LoginAction...

pojo
public class Admin...

dao
public interface IAdminDao...
public class AdminDaoImpl implements IAdminDao ...

public class DAOFactory {
	
	//为了节省资源不用每次在调用CostDaoImpl中方法时都创建一个新的对象,
	//这里利用单例方法静态声明了一个对象,以供调用;
	private static ICostDao costDao = new CostDaoImpl();
	
	//返回ICostDat接口的实例;
	public static ICostDao getCostDAO(){
		return costDao;
	}
	
	private static IAdminDao adminDao = new AdminDaoImpl();
	
	public static IAdminDao getAdminDao(){
		return adminDao;
	}
}

public abstract class LoginAction extends BaseAction{

login.jsp

index.jsp

注意:不要忘记修改引入样式的地址;



Action属性的注入:
在action标记下增加子标记param,其属性name的值为action中要被赋值的属性名;
标记中间写属性的值;
Struts2在实例化action之后会自动根据配置将值赋给action中的属性;
注意:该属性一定要有set方法;

<action>
	<param name="action中的属性名">
	属性值
	</param>
</action>

比如:
		<action name="findCost" class="com.netctoss.action.FindCostAction">
			<!--通过Action属性注入的方式给action中的属性利用set方法赋值-->
			<param name="pageSize">5</param>	
			<result name="success">
				/WEB-INF/cost/findCost.jsp
			</result>
			<result name="error">
				/WEB-INF/cost/error.jsp
			</result>
		</action>



通配符:是用来配置struts.xml中的action的;
目的是为了简化action的配置的;
但是前提是需要给action相关的配置项(属性/子元素中的内容)规定一个格式;
可以达到用一个action标记来配置出多个action的目的;

例子:模拟一个模块,用户模块,其中包含(增加,修改,查询);
定义关键字:
用户User;
增加Add;
修改Update;
查询Find;

<!--这是一个模拟用户模块的例子,旨在张氏通配符的用法-->
<package name="user" namespace="/user" extends="struts-default">
	<!--增加action-->
	<action name="AddUser" class="com.tarena.action.AddUserAction">
		<result name="success">
			/WEB-INF/jsp/AddUser.jsp
		<result>
	</action>
	<!--修改action-->
	<action name="UpdateUser" class="com.tarena.action.UpdateUserAction">
		<result name="success">
			/WEB-INF/jsp/UpdateUser.jsp
		<result>
	</action>	
	<!--查询action-->
	<action name="FindUser" class="com.tarena.action.FindUserAction">
		<result name="success">
			/WEB-INF/jsp/FindUser.jsp
		<result>
	</action>	
</package>



<package name="user" namespace="/user" extends="struts-default">
	<!--action的名称,类名,jsp名称都有统一的规则,即:动作+模块 来命名的
	解释:action名称中有2个*,定义一个通用的表示法,第一个*代表动作,第二个*代表模块
	{1}是对第一个*的引用
	{2}是对第二个*的引用
	-->
	<action name="**" class="com.tarena.action.{1}{2}Action">
			<result name="success">
				/WEB-INF/jsp/{1}{2}.jsp
			</result>
	</action>
	
	当访问....../FindUser是第一个*就是Find,第二个*就是User,必须满足这样的格式;
	
public class AddUserAction{
	public String execute(){
		return "success";
	}
}
	
......

AddUser.jsp
UpdateUser.jsp
FindUser.jsp

......



Result原理;

Result是最终用来做输出的组件;
struts-default这个默认的包中定义了10种不同类型的Result;
每种Result对应一个类,但是他们都实现了同一个接口Result;
如果<result>标记中不写type属性,那么默认这个Result是dispatcher类型;

比较基础核心的几个:

1) dispatcher: 将请求转发给jsp的Result;

2) redirectAction: 将请求重定向给一个Action的Result;

3) stream: 将二进制数据(流)输出到界面的Result;

4) json: 将对象以json格式的字符串输出到界面的Result;


1,redirectAction

<action>
	<result name = "success" type="redirectAction">
		findCost
	</result>
</action>



*****NetCtoss项目(part5)*****

		<!--删除资费Action-->
		<action name="deleteCost" class="com.netctoss.action.DeleteCostAction">
			<result name="success" type="redirectAction">findCost</result>
			
		</action>
		
public class DeleteCostAction {



2,stream:这种类型的Result,可以将二进制数据,输出给界面;
<action>
	<result name = "success" type="stream">
		<param name="inputName">
			Action中的输出属性名,这个输出属性需要时InputStream类型;
		</param>
	</result>
</action>


*****NetCtoss项目(part6)*****

public final class ImageUtil {
public class ValidateCodeAction extends BaseAction {

login.jsp:
    	<script type="text/javascript" language="javascript">
    		function change(imgObj){
    			imgObj.src = "validateCode.action?date="+newDate().getTime();
    		}
    	</script>
    	
<td><img src="validateCode.action" alt="验证码" title="点击更换" onclick="change(this);"/></td>  

......



3,json:这种类型的Result也是直接输出的,输出的是json格式的字符串;
输出的是json格式的字符串,往往是结合页面脚本,ajax来用的;

注意:与之前的stream类型的result一样,这种类型的result直接将返回的结果传给调用者,
而不是转向某个页面,如果是浏览器发出的请求,则将返回结果直接显示在一个空的页面上,
如果是某个页面的组件发出的请求,则直接返回给那个组件,相当于ajax的异步更新;

需要导包:struts2-json-plugin...jar
在struts2-json-plugin...jar包中的配置文件中有:
 <package name="json-default" extends="struts-default">这样的声明;
 由于json-default继承了struts-default,所以在需要用到json类型的result时,它
 所在的package必须继承json-default文件,而不能只是struts-default,struts-default
 中并无json-default的信息;

<action>
	<result name="success" type="json">
		<!--如果不写param会将整个Action做成json字符串
		写了param,则把指定的输出属性作为json字符串-->
		<param name = "root">
			往往是Action中的某一个输出属性;
		</param>
	</result>
</action>



Struts常用的UI标记;
这些UI标签最终将被翻译为不同形式的html标签发送到浏览器中;

例子:
<head></head>
<body>
	<h1>展示UI标签用法的例子</h1>
	
	<!--form tag-->
	//注意:在不设置theme属性的情况下,struts的form标签会自动为其中的子标签添加一个table,并且整齐的将子标签放在每一个tr中;
	//不过,这样有可能会破坏原有美工完成了的也没排布,所以需要指定theme来取消这样的设置; 
	 
	<s:form action="#" method="post" theme="simple">
		
		<!--textfield tag-->
		<s:textfield name="name" label="姓名"></s:textfield>
		
		<!--password tag-->
		<s:password name="password" label="密码" showPassword="true"></s:password>
		
		<!--textarea tag-->
		<s:textarea name="desc" label="描述" cols="30" rows="5"></s:textarea>
		
		<!--submit tag-->
		<s:submit value="保存"></s:submit>
		
		<!--单选标签-->
		//注意1:单选框需要指定name,因为radio是以name来分别哪些radio是互斥的,如果不指定name则这个ognl
		//表达式将会翻译成多个不互斥的radio,以list中键值对的个数决定;
		//注意2:list中的键值对中key是作为参数传递给服务器的值,而其value则是在页面上显示标签时的名称;
		//注意3:list属性必须指定,其中可以是一个Map(ognl表达式创建一个Map需要以#开头),如下例,也可以是一个list集合;
		<s:radio name="sex" label="性别" list="#{'M':'男','F':'女'}"></s:radio>
		
		<!--单个的checkbox标签-->
		//对应action的属性为boolean类型;
		<s:checkbox name="marry" label="是否已婚" labelposition="left">
		
		<!--多个的checkbox标签-->
		//list属性是给定的集合,集合中的数据会生成相应的多个checkbox;
		//listKey:是指定以集合对象中的哪个属性做提交;
		// listValue:是指定集合对象中的哪个属性做显示;
		<s:checkboxlist name="travelCity" label="旅游过的城市" list="citys" listKey="code" listValue="name"></s:checkboxlist>
		
		<!--下拉选的标签-->
		//由于是多选项,用法基本和多个checkbox标签相同,但是一般select只会选择出一个值,所以name对应的action中的属性最好和radio中一样是
		单个的值(虽然select也有多选功能),而不是一个相应类型的数组;
		<s:select name ="homeland" label="家乡" list="citys" listKey="code" listValue="name">
		
	</s:form>
</body>

那么如果action中的输出属性与表单中那些元素的name对应起来,那么就会自动地向这些元素的value传入这些值;
因为这些struts标签中的name都是以ognl表达式的形式给出的,能读取valuestack中的值;

public class Action{
	private String name;
	private String password;
	private String desc;
	private String sex;
	private boolean marry;
	private String homeland;
	
	//多选选项的数组;
	private List<City> citys;
	private String[] travelCity;
	
	getter/setter...;
	
	public String execute(){
		name="Tarena";
		password = "123";
		desc="good";
		sex = "M";
		marry = true;
		City c1= new City("beijing","北京")
		City c2= new City("shanghai","上海")
		City c3= new City("guangzhou","广州")
		City c4= new City("shenzhen","深圳")
		City c5= new City("chongqing","重庆")
		citys = new ArrayList<City>();
		citys.add(c1);
		citys.add(c2);
		citys.add(c3);
		citys.add(c4);
		citys.add(c5);
		homeland="shanghai";
		
		travelCity = new String[]{"shanghai","北京"};
		return "success";
	}
}

public class City{
	private String code;
	private String name;
	
	getter/setter;
	
	public City(String code,String name){
		this.code=code;
		this.name=name;
	}
}



拦截器;Interceptor;
1)是对Action的一种扩展机制;
可以为一个Action定义多个拦截器;
2)作用:用于处理一些Action之间的通用的逻辑,如:记录日志,权限检查,控制事务等;
3)使用步骤:
a,定义一个java类,实现Interceptor接口;
b,在package(strutx.xml中的)标签中注册拦截器;
<package>
	<interceptors>
		<interceptor name="名称" class="拦截器对应的类">
		
		</interceptor>
	</interceptors>
</package>

c,在struts.xml中的Action下引用注册好的拦截器;

4)注意:
1)拦截器的interceptor方法有参数,类型为ActionInvocation,通过此参数可以访问对应的Action;
2)ActionInvocation.invoke();  //来调用action方法;而此方法也有一个返回值,就是action中的返回值;
3)拦截器返回的字符串是用来找Result的,但是如果拦截器在返回字符串之前调用了Action,
那么以Action的返回值优先来找Result,之后拦截器的返回值就无效了;

5)拦截器栈;
<interceptor-stack>
仅仅是对拦截器的一个打包,在引用拦截器时可以一个一个的单独引用拦截器;
也可以对这些拦截器进行打包,成拦截器栈,从而引用这个栈即可,达到了方便的目的;
<interceptor-stack name="拦截器栈名">
	<interceptor-ref name="拦截器名"></interceptor-ref>
</interceptor-stack>

例子:

struts.xml:
<struts>
	<!--这是一个拦截器的入门小例子-->
	<package name="demo" namespace="/demo" extends="struts=default">
		<!--注册拦截器-->
		<interceptors>
			<interceptor name="someInterceptor" class="...SomeInterceptor">
			
			</interceptor>
		</interceptors>		
		<action name="some" class="com.tarena.action.SomeAction">
			<!--引用拦截器-->
			<interceptor-ref name="someInterceptor">
			</interceptor-ref>
			<result name="success">
				/WEB-INF/jsp/ok.jsp
			</result>
			<result name="error">
				/WEB-INF/jsp/error.jsp
			</result>
		</action>
	</package>
</struts>

public class SomeAction{
	public String execute(){
		System.out.println("SomeAction.execute()...");
		return "success";
	}
}


ok.jsp/error.jsp...


public class SomeInterceptor implements Interceptor{

	public void destroy(){
	
	}

	public void init(){
	
	}
	
	public String intercept(ActionInvocation arg0) 
		throws Exception{
		
		System.out.println("SomeInterceptor.intercept()...");
		
		//执行了Action;
		arg0.invoke();  //如果没有这条语句则action不会被执行;
		
		System.out.println("after invoke()...");		
		
		return "error";
	}

}

//注意,最终的终端显示结果是:
 SomeInterceptor.intercept()...
 SomeAction.execute()...
 after invoke()...
 
 浏览器页面显示的是ok.jsp的内容,因为在action中首先返回了success,Interceptor中之后的语句会继续执行,
 但是前端控制器已经接收到了一个result结果,并且根据它去执行后面的内容,之后就不可逆了,不会再去struts.xml中
 寻找从Interceptor中返回的result信息了;
 
 
 
 上传文件:
 需要common-io-1.3.2.jar以及commons-fileupload-1.2.1.jar
 fileUpload拦截器原理;
 该拦截器首先会将客户端选中上传在表单中提交的文件保存到服务器临时目录下,之后
 将临时目录下的文件对象给action属性赋值,action在此时可以访问到这个文件目录;
 当action和result调用完毕后,拦截器会清除临时目录下的文件,因此在action业务方法中,需要做文件复制,
 将临时文件转移到目标目录中;
 
 struts.xml:
 <!--这是一个使用拦截器上传文件的例子-->
 <struts>
 
 //在struts2-core-2.1.8中的default.properties文件有一个struts.multipart.maxSize参数为:2097152(2M);
 //可以通过改变常量的形式直接在struts.xml中定义; 
 <constant name="struts.multipart.maxSize" value="5000000"></constant>
 
 <package name="demo" namespace="/demo" extends="struts-default">

 	<!--跳转到上传页面的action-->
 	<action name="toUpload">
 		<result name="success">
 			/WEB-INF/jsp/upload.jsp
 		</result>
 	</action>
	<!--上传表单提交的action-->
  	<action name="upload" class="com.tarena.action.UploadAction">
  	//为了为fileUpload拦截器添加参数,这里只能将其单独引用,但是这样就需要引用默认的拦截器了;
 	//这里不引用defaultStack的原因是defaultStack中以及包含了fileUpload,如果重复引用可能会发生参数指定无效的问题;
 	<interceptor-ref name="fileUpload">
 		//这里声明无效时因为虽然拦截器的上传限制被调整了,但是struts中有默认的对整个项目的全局限制,任何单独的限制不能
 		//超过全局的限制,所以需要在struts标签中直接重新定义这个全局参数;(见上) 
 		<param name="maximumSize">5000000</param> 
 	</interceptor-ref>
 	<interceptor-ref name="basicStack"></interceptor-ref>
 		<result name="success">
 			/WEB-INF/jsp/ok.jsp
 		</result>
 		<result name="error">
 			/WEB-INF/jsp/error.jsp
 		</result>
 	</action>
 </package>
 </struts>
 
public class UploadAction{
	//输入属性;
	private File  some;
	
	//someFileName这个变量名是固定的,(action中文件变量名+FileName),这样拦截器才能自动把原始文件名传到这个属性对象中;
	private String someFileName;
	
	getter/setter...
	
	public String execute(){
	
		//1,读取到文件的名字;
		some.getName(); //这样获得的文件对象的名称并不是上传的文件的原始名;
		 System.out.println("FileName:"+someFileName);
		 if(some==null){
		 	return "error";
		 }
		 
		//2,拼一个上传的名字;
		String name = "upload_"+someFileName;
		  
		//3,指定一个与服务器中项目的相对路径;
		//在webroot下建立文件夹upload;
		String path = "upload/"+name; 
		 
		//4,得到绝对路径;
		path=ServletActionContext.getServletContext().getRealPath(path);
		
		//5,将临时文件复制到这个绝对路径下;
		FileUtil.copy(some,new File(path));
		  
		//6,返回success;
		return "success";
	}
	  
}


upload.jsp:
......
<head></head>
<body>
	<h1>文件上传示例</h1>
	//上传时,method必须为post,且enctype属性需如下指定;
	<form action="upload" method="post" enctype="multipart/form-data">
		<input name="some" type="file"/><br/>
		<input type="submit" value="上传"/>
	</form>
</body>

ok/error.jsp...



补充:

一.Struts1框架内部核心类;

关于ActionServlet;
ActionServlet类是Struts1框架的内置核心控制器组件,它继承了javax.servlet.http.HttpServlet类,Struts的启动一般从加载ActionServlet开始,因此它在MVC模型中扮演中央控制器的角色;
ActionServlet是一个标准的Servlet,在web.xml文件中配置,该Servlet用于拦截所有的HTTP请求;因此,应将Servlet配置成自启动Servlet,即为该Servlet配置load-on-startup属性;
用户提交表单时,一个配置好的ActionForm对象被创建,并被填入表单相应的数据,ActionServlet根据Struts-config.xml文件配置好的设置决定是否需要表单验证,如果需要就调用ActionForm的Validate()验证后选择将请求发送到哪个Action,
如果Action不存在,ActionServlet会先创建这个对象,然后调用Action的execute()方法;Execute()从ActionForm对象中获取数据,完成业务逻辑,返回一个ActionForward对象,ActionServlet再把客户请求转发给ActionForward对象指定的jsp组件,
ActionForward对象指定的jsp生成动态的网页,返回给客户;

1.Struts1框架的初始化流程;
对于采用Struts1框架的Web应用,在Web应用启动时就会加载并初始化控制器ActionServlet,ActionServlet从struts-config.xml文件中读取配置信息,把它们存放到ActionMappings对象中;
在Struts framework中,Controller主要是ActionServlet,但是对于业务逻辑的操作则主要由Action、ActionMapping、ActionForward这几个组件协调完成(也许这几个组件,应该划分到模型中的业务逻辑一块);
其中,Action扮演了真正的控制逻辑的实现者,而ActionMapping和ActionForward则指定了不同业务逻辑或流程的运行方向;

2.Struts1框架响应客户请求的工作流程;
(1)检索和用户请求匹配的ActionMapping实例,如果不存在,就返回用户请求路径无效的信息;
(2)如果ActionForm实例不存在,就创建一个ActionForm对象,把客户提交的表单数据保存到ActionForm对象中;
(3)根据配置信息决定是否需要表单验证;如果需要验证,就调用ActionForm的validate()方法;
(4)如果ActionForm的validate()方法返回null或返回一个不包含ActionMessge的ActionErrors对象,就表示表单验证成功;
(5)ActionServlet根据AtionMapping实例包含的映射信息决定将请求转发给哪个Action;如果应的Action实例不存在,就先创建这个实例,然后调用Action的execute()方法;
(6)Action的execute()方法返回一个ActionForward对象,ActionServlet再把客户请求转发给ActionForward对象指向的JSP组件,JSP组件生成动态页面,返回给客户;


二.日期的显示:
如果页面上要显示的日期字段进行存储,那么注意一下几点:
1)数据库中的类型是日期类型;
2)实体对象中的类型为字符串;
3)查询语句中对日期类型的字段to_char;
4)插入/更新语句中对日期类型的字段to_date;



NetCtoss项目(day01):

整体需求:

NETCTOSS项目分成3个子系统,用于解决电信行业Unix服务器出租业务的,
共分为3个子系统:

1,DMS(采集端的部署); //负责对Unix服务器的日志进行分析的,从中采集出用户登录相关信息,
并去访问一个固定的公共服务器将这些信息存储到同一的数据库中,这个系统要分别部署到不同的Unix服务器上;

注意:利用DMS系统而不是直接让NetCTOSS去读取Unix服务器上日志信息的原因是:
1)出于安全考虑,不想让各个Unix服务器将日志公开到一个公共的FTP上供NetCTOSS访问;
2)出于效率的考虑,让NetCTOSS去访问一个固定的服务器效率更高,而不是将DMS分配
在固定的公共服务器上来抓取各个其他服务器的登录信息或是直接去访问各个服务器的日志信息;

2,NetCTOSS; //是个营业厅的营业员使用的,营业员可以通过这个系统创建Unix服务器用户,维护自费相关数据,
资费的标准,查看用户的消费记录,需要独立部署到一台应用服务器上,需要读取同一的数据库的数据;

3,PL/SQL程序; //根据DMS采集的用户登录时长,以及NetCTOSS web系统维护的
资费标准,自动地按时计算用户消费;


项目任务:
1,前提:资费模块;
2,目标:独立开发;
3,具体任务:
1)账务账号;
2)业务账号;
3)角色管理;
4)管理员;


技术架构:
1,所用技术:
Java+jdbc+struts2+Ajax/JQuery

2,开发思想:
基于MVC的分层思想,将代码分为以下层：

1)表现层: jsp页面,校验脚本,样式;
2)控制层: struts2;
3.1)业务层: Action中,业务控制代码;
3.2)数据访问层: jdbc+dao;


具体开发:
1,账务账号;
1)带条件的组合查询;
2)新增,修改;
3)删除; 并非物理删除,而是执行更新语句,将status字段改为2;
4)状态的维护;

2,带条件的组合查询;
1) 规划代码:
    a,页面: account/findAccount.jsp
	b,struts.xml: 配置账务账号模块,查询的Action;
	c,FindAccountAction;
	   输入,输出属性是相对页面而言的,当前操作需要页面传递什么参数,那么这些
	   参数就是输入属性;
	   当前操作需要返回给页面什么值,那么这些值就是输出属性;
	   
	d,IAccountDao;
	f, AccountDaoImpl;
	e,实体类Account;
	
2) 写代码:
	a,写实体类; (之后的Action,dao都要用到,需要先了解表结构)
	b,struts xml; (Action,jsp规划好)
	c,Action:  相对于需要给访问者提供怎样的html页面,页面中怎么返回给Action更新后的参数;
	定义输入属性; 需要jsp提供的参数;
	定义输出属性; 需要返回给jsp的值;
	d,dao:包括接口,实现类,工厂类;
	e,jsp页面,根据美工提供的静态页面来改;
	包括:struts标签,js脚本;
	
	
2,开通,暂停,删除状态管理;
a,当列表中某行的状态为开通时,显示暂停,修改,删除按钮;
b,当列表中某行的状态为暂停时,显示开通,修改,删除按钮;
c,当列表中某行的状态为删除时,不显示任何按钮;

开通;
操作暂停状态的数据,点击开通后,这行数据的状态置0,删除暂停时间;
重新开通账务账号时不需要将其下所属所有业务账号重新开通,需要按需求逐个重新开通;

暂停;
操作开通状态的数据,点击暂停后,这行数据的状态置1,记录暂停时间;
同时暂停其下属所有的业务账号(暂时不做,业务账号模块还未完成);

删除;
操作的是开通/暂停状态的数据,点击删除后,这行数据的状态置2,记录删除时间;
删除账务账号,同时删除其下所属的所有业务账号(暂时不做,业务账号模块还未完成);

状态维护的思路;
a,通过js脚本来提交请求(异步),包含id参数;
b,Action中请求找到对应的Action,Action,会调用对应的dao方法,根据id更新状态和日期的字段;
c,更新状态结束后,显示提示信息给用户;
d,更新当前的列表页面,将更新后的字段以及其按钮更新,停留在当前页,所以需要将page传出;


新增;
推荐人:填完推荐人身份证号之后,光标切换时,发送异步请求(传递推荐人身份证号参数),查询表中是否
存在该推荐人,如果存在返回id,将id记录在隐藏的dom中,其name="recommenderId";
提交时会将这个id给recommenderId属性;如果不存在提示此用户不存在;

生日:不可编辑,需要通过身份证号计算的来;当填写完身份证号时,光标切换时,从身份证号中截取出生日,
填给生日字段;

修改;
1.参考自费模块的修改功能;
2.页面的校验和新增一致;
3.修改密码:填写完旧密码,光标切换时,发异步请求(传递id和旧密码),检查当前输入的旧密码是否正确,
如果不正确不能修改,正确才能修改;


查看明细;
1.列表中,姓名上加超链接,URL指定为查看的Action(viewAccountAction),转到查看页面;
2.创建查看的页面(viewAccount.jsp);Action中根据输入属性id,查询这条记录(参考toUpdateCost的查询方法),
转发到viewAccount.jsp,并将查询结果输出给页面;明细的所有字段都是只读的,没有保存按钮,只有返回;



业务帐号;
业务说明:用于创建和维护登录Unix服务器的帐号密码,以及维护登录藏好的状态;

1,账务账号和业务帐号之间是1对多的关系,在业务装好中要记录账务账号信息;
2,一对多的关系体现在SERVICE表中的ACCOUNT_ID字段;
3,创建业务帐号时,要指定资费类型(从资费管理取),业务帐号表中存的是资费表的ID;

组合查询:
1,逻辑上和账务账号一致;
2,查询结果中不再是账务账号这一张表的数据,还包含业务帐号表中的身份证,姓名,以及资费表中的
资费名,资费描述,这些字段需要关联查询出来;
3,用VO来封装多张表联合查询的结果;view object,视图对象,用来获取所需视图的实体类;

状态维护:
1,开通,暂停,删除状态的维护和账务账号一致;
2,开通有区别;
在开通的时候,先判断对应的账务账号是否处于暂停/删除的状态,如果是的话,不能开通,并且提示用户;
在开通的Action的execute方法中,
//输入属性;
private Integer id;
private Integer accountId;
private boolean pass; //
privateString message; //提示信息;

public String execute(){
	拿到dao;
	//1,判断对应的账务账号是否暂停/删除;
	Account a = dao.findAccountById(accountId);
	if(a.getStatus().equals("1")||a.getStatus().equals("2")){
		pass = false;
		message = "对应的账务账号已暂停或删除,不能进行此项操作";
		return "success";
	}
	//调用dao的开通方法去开通...
	和账务账号开通方法相同;
}


新增:
1,基本思路和账务账号一样;
2,输入完身份证点击查询账务账号时,异步发请求,根据身份证查询账务账号表,得到账务账号的数据
返回页面,页面判断返回结果是否为空,若为空则提示重新输入,并清空隐藏的accountId和账务账号显示,
若不为空则设置隐藏的accountId和账务账号;
这里的账务账号对应的是账务账号表的登录名(loginName);
3,资费类型:
数据来源于资费列表中的所有数据,该下拉选的option的value对应资费表的id字段,option的显示
内容对应的是资费表的名称字段;


修改:
1,资费类型下拉选动态构建参考新增;
2,其他的参考账务账号修改;


业务说明:
1,点击站务账号暂停按钮,要将账务账号数据暂停;
之后,要根据当前的账务账号ID,从业务账号表中根据ACCOUNT_ID字段将相关的业务账号更新状态为暂停;
同时将暂停时间设置为当前系统时间;
2,点击账务账号删除按钮,要将账务账号数据删除,之后要根据当前的账务账号ID,
从业务账号表中根据ACCOUNT_ID字段将相关的业务账号更新状态为删除;
同时将删除时间设置为当前系统时间;

暂停下属业务账号;
1,写一个方法,根据账务账号ID,查询相关的业务账号数据;
2,循环这组业务账号数据,每次调用之前的根据业务账号ID更改相关状态的方法;
pauseService(int id);但是这种方法效率低,需要避免;
3,可以重载pauseService(int[] id);
4,重构pauseService(int id),改成pauseService(int[] id);
调用dao.pauseService(new int[]{id});


查看明细;
1,参考账务账号的查看明细,或者参考资费管理的修改;

补充:
如果页面上要显示的日期字段进行存储,那么注意一下几点:
1)数据库中的类型是日期类型;
2)实体对象中的类型为字符串;
3)查询语句中对日期类型的字段to_char;
4)插入/更新语句中对日期类型的字段to_date;

持久化日期字段(新增账务账号中根据身份证获得生日);
1,数据库中是日期类型;
2,实体类中是String类型;
3,insert into account values(to_date(?,'yyyy-mm-dd'));
4,select to_char(BIRTHDAY,'yyyy-mm-dd') birthday from ACCOUNT;

一般不在DAO中做事务处理,而是在业务层(action)做事务处理,之后在spring中会讲;


备注:
2个瓶颈:
1,杜绝在循环内发请求,因为前端发送请求非常耗费资源,并且无法预知循环次数;
2,尽量杜绝在循环内访问数据库,因为访问数据库也很耗资源,无法预知循环次数的时候,效率无法控制;



权限控制;

业务说明：
1，权限控制：软件具有若干功能（模块：一组功能的集合）；
在使用时，不是所有的用户都能使用其中所有的功能，是要给用户指定一定的权限，
该权限包含了他能访问的功能；
分配权限和权限控制的过程叫权限控制；

2，权限控制分为：
1）权限设置；
a,设置角色
b,设置角色能操作的功能
角色和功能之间是一对多的关系；
c,设置用户（关联角色，从而间接关联功能）；
当一个用户具有多个角色的时候，那么他具有的功能权限是这些角色所具有的功能的并集；

2）权限检查；
a,动态的构建导航栏；
b,给Action加拦截器，判断是否具有权限；

3，NETCTOSS权限设置体系：
由角色管理，管理员管理组成的；

角色管理：
1，表说明：
ROLE_INFO:角色表，只存角色信息；
ROLE_PRIVILEGE:角色权限表，存的是角色和权限的关联关系；
一个是角色表的ID，一个是privileges.xml中privilege的ID；
privileges.xml:存的是权限相关的数据，每个privilege描述了能够访问哪些功能；

2,新增:
1)新增角色信息;
2)从刚新增的角色信息中取得自动生成的ID;
3)根据取得的ID,以及传递进来的权限ID数组,批量的保存角色权限数据;

3,修改:
1)修改角色信息;
2)删除该角色所有的角色权限数据;
3)根据传入的角色权限数据,重新新增角色权限数据;


findRole 分页sql;

select * from ROLE r inner join ROLE_PRIVILEGE p on i.ID=p.ROLE_ID
where i.id in(
	.....
)



管理员模块:
1,用来维护登陆NETCTOSS系统用户的登陆信息;
包含组合查询,新增,修改,删除,密码重置等功能;
2,查询:带条件的组合查询;
1)角色查询条件改成下拉选,选择范围为所有的角色;

2)组合查询:
select * from ADMIN_INFO ai 
inner join ADMIN_ROLE ar on ai.ID = ar.ADMIN_ID
inner join ROLE ri on ri.ID = ar.ROLE_ID
inner join ROLE_PRIVILEGE rp on rp.ROLE_ID = ri.ID
where 1=1
and......


密码重置:
1)将选中的行,对应的用户的密码重置为默认密码(123456);
2)由于查询页面上没有显示密码列,那么操作完密码重置之后，就
没有必要刷新页面,因此需要异步的提交请求;
3)主要是JS操作;


新增:
1)在新增的页面上,通过<s:checkboxlist/>标记来生成角色的数据;
2)新增DAO的步骤
a,插入管理员的数据admin_info;
b,把上面插入数据自动生成的ID得到;
c,根据得到的ID以及传递进来的一组角色ID来插入管理员角色的数据;


修改:
1)将管理员信息更新
2)删除管理员角色信息(根据管理员ID)
3)重新将管理员角色信息加入;


删除:
参考;


项目面试相关:

项目描述;
1)NETCTOSS项目,这个项目汉族要用来针对电信Unix服务器出租业务中,出租账号,资费维护,
收取费用的相关业务的管理;
2)该项目分为3个子系统:

DMS:用来提取用户登录Unix服务器的信息的;
通过读取服务器的日志文件,将登录信息读取出来之后写入到NETCTOSS统一的数据库中,需要
每一台Unix服务器都单独部署DMS;

NETCTOSS WEB系统:用来维护登录Unix服务器的账户的,以及用来维护用户的资费标准,查看
用户的消费情况(账单管理,报表);
包含了如下模块:
角色管理,管理员,资费管理,账务账号,业务账号,账单管理,报表;
我主要负责开发其中的角色管理,管理员,资费管理,账务账号,业务账号;

该系统的技术架构:
Java+JDBC+Struts2+jsp+javascript/jquery

介绍模块,比如:
账务账号:该模块用于维护向用户收费的账号,该账号与业务账号具有一对多的关系,业务账号是客户
登录Unix服务器的账号;
具有新增,修改,删除,组合查询,状态维护,查看明细的功能;
重点阐述组合查询,状态维护,新增(页面校验);

数据库存储过程:
每月固定时间(月底)自动计算用户本月消费情况,根据DMS提取的登录时长,NETCTOSS系统中
维护的资费数据来进行计算;
这些PL/SQL的脚本需要部署到NETCTOSS的数据库上,并且为存储过程定义好任务(何时触发);
计算好的数据可以在NETCTOSS WEB系统中查看;



备注:
1,通过<include file=""></include>
的方式可以拆分一个大的struts.xml文件来重构该配置文件,使代码可读性提高;
 
*/


