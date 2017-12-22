JSP


package prototype;

public class JSP {

	public static void main(String[] args) {

	}
}

/*

jsp(java server page -- java服务器端页面技术);
(1)jsp
sun公司制定的一种服务器端动态页面生成技术规范;
jsp起始就是一个以".jsp"为后缀的文件,在文件当中,主要是html(css,javascript)和少量的
java代码;容器会将jsp文件转换成一个对应的servlet然后去执行;
转化后的servlet源代码可以从:
tomcat\apache-tomcat-5.5.23\work\Catalina\localhost\工程名\org\apache\jsp\jsp名称.java;

(2)如何写一个jsp文件?
	step1,创建一个以".jsp"为后缀的文件;
	step2,在该文件中可以添加如下的内容:
		1)html(css,javascript); //直接写,不用再利用out.print()语句;
		2)java代码;
		  a,java代码片段;
		     比如:
		     <% int i = 100; %>
		  b,jsp表达式;
		  比如:
		  	 <%=new Date()%>
		
        3)指令;
   		  a,什么是指令;
                          告诉容器,在将jsp文件转换成servlet类时做一些额外的处理,比如导包;
   		  b,语法;
   			<%@指令的名称 属性=值%>	     
   		  c,page指令;
   	      import属性;
   	   		如:
   			<%@page import="java.util.*,java.text.*"%>
   		  contentType属性: 设置response.setContentType的内容,比如:
   		  	<%@page contentType="text/html;charset=utf-8"%>
   	
   		4)隐含对象;
   		a,什么是隐含对象?
        	在jsp文件里面可以直接使用的对象; (out)
   		b,可以直接使用隐含对象的原因;
        	因为容器会自动添加获得这些对象的代码;
   		c,有哪些隐含对象?
   			out,request,response...
   
   
例子:
在WebRoot下创建一个hello.jsp;

<%@page import="java.util.*" contentType="text/html;charset=utf-8" pageEncoding="utf-8"%>
<html>
	<head></head>
	<body style="font-size:30px;font-style:italic;">
		current time:
		<%
			Date date = new Date();
			SimpleDateFormate sdf = new SimpleDateFormat("yyyy-MM-dd");
			out.println(sdf.format(date));
		%>
		<br/>
		当前系统时间:<%=sdf.format(date)%><br/>
		<%
			for(int i = 0; i<100; i++){
		%>
				hello world<br/>
		<%
			}
		%>
	</body>
</html>


(3)JSP是如何运行的?
step1,容器要将jsp文件转换成一个servlet类;
	html(css,javascript)---> service方法,用out.write()输出;
	<%   %>--->service方法,照搬;
	<%=  %>--->service方法,使用out.print方法输出;
	
注意:out.print()和out.write()的区别;
如在jsp中:
String test = null;
out.print(test); //在页面显示null;
out.write(test); //在页面显示为“”--空字符串;

int test2 = 97;
out.print(test); //在页面显示97;
out.write(test); //在页面显示a;



例子:利用JSP来实现之前用servlet完成的员工列表;

<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8"%>
<%@page import="dao.*,entity.*,java.util.*"%>
<html>
	<head>
		<style>
			row1{
				background-color:#fff8dc;
			}
				row2{
				background-color:yello;
			}
		</style>
	</head>
	<body style="font-size:30px;font-style:italic;">
		<table cellpadding="0" cellspacing="0" border="1" width="60%">
			<caption>员工列表</caption>
			<tr>
				<td>ID</td><td>姓名</td>
				<td>薪水</td><td>年龄</td>
			</tr>
			<%
				EmployeeDAO dao = new EmployeeDAO();
				List<Employee> employees = dao.findAll();
				
				for(int i=0; i<employees.size(); i++){
					Employee e = employees.get(i);
			%>
				<tr class="row<%(i%2+1)%">
					<td><%=e.getId()%></td>
					<td><%=e.getEname()%></td>
					<td><%=e.getSalary()%></td>
					<td><%=e.getAge()%></td>
					<td>
						<a href="del.do?id=<%e.getId()%>" onclick="return confirm('确定删除<%=e.getEname()%>吗?');">删除</a>
						<a href="load.do?id=<%e.getId()%>">修改</a>
					</td>
				</tr>
			<%
				}
			%>
		</table>
	</body>
</html>



转发;
	(1)什么是转发?
		一个web组件(servlet/jsp)将未完成的处理交给另外一个web组件继续完成;
		常见的情况:
		一个servlet获得需要展现的数据之后,转发给一个jsp来将这些数据展现出来;
	(2)编程;
		step1,绑定数据到request对象上;
		request.setAttribute(String name,Object obj);
		name:绑定名称; obj:绑定值;
		Object request.getAttribute(String name); //根据绑定名,获得绑定值,如果
		没有对应的值,返回null;
		
		step2,获得转发器;
		RequestDispatcher rd = request.getRequestDispatcher(String url); //url是转发的目的地;
		
		step3,转发;
		rd.forward(request,response); //jsp与servlet共享request,response对象;
	(3)需要注意的两个问题;
		a,转发之前,不能调用out.close();
		b,转发之前,容器会将response对象上的缓存数据清空;
	(4)特点;
		a,转发的目的地必须是同一个应用内部某个组件的地址;
		b,转发之后,浏览器地址栏的地址是不变的;



练习:利用转发实现员工列表;
addEmp.jsp:
直接将美工做完的.html拷贝到.jsp中;

<%@page pageEncoding="utf-8" contentType="text/html;charset=utg-8" %>

<html>
......
	<form action="add.do" method="post">
	

......
</html>


ActionServlet:
......
else if(action equals("/add")){
	String ename = request.getParameter("ename");
	double salary = Double.parseDouble(request.getParameter("salary"));
	int age = Integer.parseInt(request.getParameter("age"));
	EmployeeDAO dao = new EmployeeDAO();
	Employee e = new Employee();
	e.setEname(ename);
	e.setSalary(salary);
	e.setAge(age);
	try{
	dao.save(e);
	response.sendRedirect("list.do");
	}catch(SQLException e1){
		e1.printStackTrace();
		request.getRequestDispatcher("error.jsp").forward(request,response);
	}
}else if(action.equals("/del")){
	int id = Integer.parseInt(request.getParameter("id"));
	EmployeeDAO dao = new EmployeeDAO();
	try{
	dao.delete(id);
	response.sendRedirect("list.do");
	}catch(SQLException e1){
		e1.printStackTrace();
		request.getRequestDispatcher("error.jsp").forward(request,response);
	}
}else if(action.equals("/load")){
	int id = Integer.parseInt(request.getParameter("id"));
	EmployeeDAO dao = new EmployeeDAO();
	try{
	Employee employee = dao.findById(id);
	//转发给updateEmp.jsp来生成一个动态页面表单;
	request.setAttribute("e",employee);
	request.getRequestDispatcher("updateEmp.jsp").forward(request,response);
	}catch(SQLException e1){
		e1.printStackTrace();
		request.getRequestDispatcher("error.jsp").forward(request,response);
	}
}else if(action.equals("/modify")){
	int id = Integer.parseInt(request.getParameter("id"));
	String ename = request.getParameter("ename");
	double salary = Double.parseDouble(request.getParameter("salary"));
	int age = Integer.parseInt(request.getParameter("age"));
	Employee e = new Employee();
	e.setId(id);
	e.setEname(ename);
	e.setSalary(salary);
	e.setAge(age);
	try{
	dao.modify(e);
	response.sendRedirect("list.do");
	}catch(SQLException e1){
		e1.printStackTrace();
		request.getRequestDispatcher("error.jsp").forward(request,response);
	}
}


updateEmp.jsp:
直接将美工做完的.html拷贝到.jsp中;

<%@page pageEncoding="utf-8" contentType="text/html;charset=utg-8" %>
<%@page import="entity.*" %>

<html>
......
	<%
		Employee e = (Employee)request.getAttribute("e");
	%>
	<form action="modify.do?id=<%=e.getId()%>" method="post">
	......
	id:
	<%=e.getId()%>
	name:
	<input type="test" class=... name="ename" value="<%=e.getEname()%>"/>
	salary:
	<input type="test" class=... name="salary" value="<%=e.getSalary()%>"/>
	age:
	<input type="test" class=... name="age" value="<%=e.getAge()%>"/>
	
......
</html>



include指令;
(1)告诉容器在将jsp文件转换成servlet类时,将文件的内容直接插入到指令所在的位置;
(2)用法:
	<%@include file="head.jsp"%>
	<%@include file="footer.jsp"%>
可以将html页面中相同的内容(比如页头和页脚)提取出来单独成为.jsp文件,然后在主jsp文件中
利用include指令在正确位置将其加入,便于修改和管理;


让容器来处理异常;

step1.将异常抛给容器;
throw new ServletException(e);

step2,在web.xml文件中配置错误处理页面,当容器捕获到对应的异常后,会调用相应的页面;:

......
</servlet-mapping>
<!--配置错误处理页面-->
<error-page>
	<exception-type>javax.servlet.ServletException</exception-type>
	<location>/error.jsp</location>
</error-page>

一般来说,对于系统异常(不是由于程序的问题,是因为程序在运行过程中依赖的资源出了问题,
比如访问不了数据库),可以将异常交给容器来处理;


路径问题;
链接地址,表单提交,重定向,转发;

<a href="addEmp.jsp"></a>
<form action="add.do">
response.sendRedirect("list.do")
request.getRequestDispatcher("emplist.jsp")

(1)相对路径;
	不以"/"开头的路径;
(2)绝对路径;
	以"/"开头的路径;
(3)如何写绝对路径;
	链接地址,表单提交,重定向从应用名开始写;
	转发从应用名之后开始写;(转发本身就只能向同一个应用中的目标转发)
注意:
应用名不要直接写在路径里面,应该使用String request.getContextPath()来获得应用名;


如:在web工程中的WebRoot文件夹中建立一个app01的文件夹,并在其中建立一个a1.jsp;
并在WebRoot中新建一个a2.jsp;
http://localhost:8080/song/app01/a1.jsp
http://localhost:8080/song/a2.jsp

a1.jsp:

<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8"%>
<html>
	<head></head>
	<body style="font-size:30px;font-style:italic;">
		a1的内容...<br/>
		<a href="../a2.jsp">访问a2.jsp</a>
		<br/>
		使用绝对路径<br/>
		<a href="/song/a2.jsp">访问a2.jsp</a> //要避免工程名的硬编码;
		<a href="<%=request.getContextPath()%>/a2.jsp">访问a2.jsp</a>
	</body>
</html>

response.sendRedirect(request.getContextPath()+"/app01/a1.jsp"); //重定向;

注意:request.getContextPath()得到的是一个类似:"/工程名"这样的字符串,虽然是绝对路径,但是这个绝对是相对服务器而言的,就是说重定向
的"/"开头的路径在真正转向时还是在如同"http://localhost:8080/"之后的,想要访问服务器以外的站点需要将"https://www.baidu.com/"这样
的路径写入参数;而相对路径是相对request保存的浏览器访问地址而言的;

request.getRequestDispatcher("/app01/a1.jsp").forward(request,response); //转发;


转发与重定向的区别;

(1)转发所涉及的各个web组件(servlet/jsp)共享同一个request对象和response对象;
 而重定向不能共享;因为request对象和response对象的生存周期为一次请求与响应(当容器收到
 请求后,会立即创建这两个对象,只要容器将响应发送出来,就立即销毁这两个对象);

(2)转发的目的地只能是同一个应用内部某个组件的地址;重定向是任意的地址;

(3)转发之后浏览器地址栏的地址不变,重定向会变;

(4)转发是一件事未做完,而重定向是一件事已经做完然后调用另外一个组件做另外一件事;


关于out流的关闭:
在service方法的最后,out流会自动被关闭,response对象也被销毁了(因为一次请求和响应已经结束);
而当主动调用flush()或者close()时服务器会把请求头部分以及现成的其他response体发送到客户端,这里response将被置为commited,
不能再改变其请求头,也不能再通过此out流发送信息,而response.sendRedirect("/index.jsp")请求需要改变location请求头,所以不能在之前调用out.close();
同样,response.getRequestDispatcher.forward(request,response);也需要保持response状态为未提交,
这样在执行forward时才能正常将request中的信息转发到另一个地址(response对象此时已被刷新),之后才能将信息通过response对象再发送给浏览器;




状态管理;

(1)什么是状态管理;
   将浏览器与web服务器之间多次交互(一次请求一次响应)当成一个整体来看待,并且将多次交互所设计的
   数据(状态)保存下来;

(2)如何进行状态管理;
   第一类是客户端的状态管理技术;将状态保存在浏览器;
   第二类是服务器端的状态管理技术,将状态保存在服务器上;
   
(3)cookie;
   1)什么是cookie?
   	a,cookie属于客户端的状态管理技术;
   	b,浏览器在访问服务器时,服务器会将少量的数据以set-cookie消息头的方式发送给浏览器;
   	浏览器会将这些数据保存下来,当浏览器再次访问服务器时,会将之前保存的数据以cookie消息头
   	的方式发送给服务器;
   	
   2)如何创建一个cookie?
   	Cookie c = new Cookie(String name,String value);
   	name:cookie的名称;
   	value:cookie的值;
   	response.addCookie(c);

如:火狐浏览器->编辑(工具)选项->隐私->Firefox将会,下拉列表->使用自定义设置历史记录->显示cookie;

例子:创建一个servlet,测试cookie的用法;
......
PrintWriter out = response.getWriter();
Cookie c =new Cookie("uname","eric");
response.addCookie(c);
Cookie c2 =new Cookie("pwd","123");
response.addCookie(c2);
out.println("添加了一个cookie");
out.close();
......


	3)查询cookie;
	  Cookie[] request.getCookies();
	      注意:
	      如果浏览器没有发送任何的cookie,则给方法返回Null;
	  
	  String cookie.getName();
	  String cookie.getValue();
	  
获取cookie对象;

......
	Cookie[] cookies = request.getCookies();
	if(cookies!=null){
		for(int i=0li<cookies.length;i++){
			Cookie cookie = cookies[i];
			String name = cookie.getName();
			String value = cookie.getValue();
			out.println(name+" "+value+"<br/>");
		}
	}else{
		out.println("没有找到cookie");
	}
......
	  
	 4)生存周期;
	   指的是浏览器会将cookie保存多久,保存在哪儿?
	   默认情况下,浏览器会将cookie保存在内存,只要浏览器关闭,进程消亡,cookie就不存在了;
	 cookie.setMaxAge(int seconds);
	   注意:
	 a,单位是秒;
	 b,seconds > 0; 浏览器会将cookie保存在硬盘上;超过指定的时间,会被删除;
	   seconds < 0; 缺省值;浏览器会将cookie保存在内存中;
	   seconds = 0; 立即删除cookie;
	   比如,要删除一个名称为uid的cookie;
	 Cookie c =new Cookie("uid","");
	 c.setMaxAge(0);
	 response.addCookie(c);
	  
练习:
	写一个Find_AddCookieServle,该servlet先查看有没有一个名叫uid的cookie,有的话,就显示该cookie的值,
	没有,则创建;


public class Find_AddCookieServle extends HttpServlet{
	public void service(HttpServletRequest request,HttpServletResponse response)
	throws ServletException,IOException{
	PrintWriter out = response.getWriter();
	Cookie[] cookies = request.getCookies();
	boolean found = false;
	if(cookies!=null){
		for(int i=0;i<cookies.length;i++){
			String name = cookies[i].getName();
			if(name.equals("uid")){
			out.println("uid="+name+"<br/>");
			found=true;
			out.println("a cookie that already exist has been found.");
			break;
			}
		}
	}
	if(!found){
		Cookie c =new Cookie("uid","eric");
		response.addCookie(c);
		out.println("a new cookie has been received.");
	}
	out.close();
	}
}


	5)编码问题;
	cookie只能保存合法的ascii字符,如果要保存中文,需要编码(将中文转换成合法的ascii字符);
	
	如:
	public void service(HttpServletRequest request,HttpServletResponse response)
	throws ServletException,IOException{
	response.setContentType("text/html;charset=utf-8");
	PrintWriter out = response.getWriter();
	Cookie c = new Cookie("uname",URLEncoder.encode("米奇","utf-8"));
	response.addCookie(c);
	out.close();
	}
	
	......
	Cookie[] cookies = request.getCookies();
	if(cookies!=null){
		for(int i = 0;i<cookies.length;i++){
		Cookie c = cookies[i];
		String name = c.getName();
		String value = c.getValue();
		out.println(name+" "+URLDecoder.decode(value,"utf-8")+"<br/>");
		}
	}
	......
	
	在保存cookie时,使用URLEncoder.encode(String str,String code);
	在读取cookie时,使用URLDecoder.decode(String str,String code);
	
	
	6)路径问题;
	
	a,什么是路径问题;
	浏览器在访问服务器上的某个地址时,会比较cookie的路径是否于这个地址匹配,如果匹配才会将
	该cookie发送出去;
	
	b,匹配规则;
	要访问的地址必须是cookie的路径或者其子路径,浏览器才会发送该cookie;
	cookie有一个默认的路径,其值等于创建该cookie的组件的路径;
	可以调用cookie.setPath(String path)来修改cookie的路径;
	如:cookie.setPath("/appname");
	
	比如:
	1.在某个web工程的WebRoot文件夹下创建一个新的文件夹app01,其中新建一个a1.jsp文件;
	
	a1.jsp:
	......
	<%
		Cookie c =new Cookie("company","tarena");
		response.addCookie(c);
	%>
	......
	
	访问地址:localhost:8080/song/app01/a1.jsp
	
	访问后,本地浏览器中保存的相关cookie其属性中:路径为/song/app01/
	
	
	2.在WebRoot文件夹下创建一个a2.jsp;
	
	a2.jsp:
	......
	<%
		Cookie[] cookies = request.getCookies();
		if(cokies != null){
			for(int i=0;i<cookies.length;i++){
			Cookie c = cookies[i];
			out.println(c.getName() + " " + c.getValue() + "<br/>");
			}
		}else{
			out.println("no cookie");
		}
	%>
	......
	
	访问地址:localhost:8080/song/a2.jsp
	
	并未读取到之前a1.jsp访问浏览器所保存在其中的cookie;
	
	
	3.在WebRoot文件夹下的app01文件夹中创建一个a3.jsp,其内容与a2.jsp相同;
	
	访问地址:localhost:8080/song/app01/a2.jsp
	
	访问到了之前a1.jsp在浏览器中保存的cookie;
	
	
	4.在app01中建立一个子文件夹sub,在其中创建一个a4.jsp,其内容与a2相同;
	
	访问地址:localhost:8080/song/app01/sub/a4.jsp
	
	访问到了之前a1.jsp在浏览器中保存的cookie;
	
	
	a1.jsp:/song/app01/    original
	
	a2.jsp:song/           error
	
	a3.jsp:song/app01/     ok
	
	a4.jsp:song/app01/sub/ ok
	
	
	5.修改a1.jsp;
	
	......
	c.setPath("/song"); //发送时将缺省路径名改变;
	response.addCookie(c);
	......
	
	
	7)cookie的限制;
	
	a,cookie不安全,对于敏感数据(比如密码等)需要加密;
	
	b,cookie可以被用户禁止;
	
	c,cookie能够保存的数据大小有限制(4k左右);
	
	d,浏览其能够保存的cookie的个数也有限制(300左右);
	
	e,cookie只能保存字符串;
	
	

服务器端的状态管理技术:
session;
	(1)什么是session?
	
		a,服务器端的状态管理技术;
		
		b,浏览器在访问服务器时,服务器会创建一个session对象(该对象有一个唯一的id,一般
		称之为sessionId),在默认情况下,服务器会使用cookie的方式(以set-cookie消息头的方式)
		将sessionId发送给浏览器;当浏览器再次访问服务器时,会将sessionId发送给服务器;
		服务器依据sessionId找到之前创建的session对象;
	
	(2)如何获得session对象;
		1)方式一:HttpSession session = request.getSession(boolean flag);
		执行原理:
		当flag=true时,先查看请求当中有没有sessionId,如果没有,则创建一个session对象;
		如果有,则依据sessionId查找对应的session对象,找到后返回对象,找不到则创建一个新的session对象;
		当flag=false时,先查看请求当中有没有sessionId,如果没有,返回null;
		如果有,则依据sessionId查找对应的session对象,找到后返回对象,找不到返回null;
		
		2)方式二:HttpSession session = request.getSession();
		执行原理:等价于request.getSession(true);
	
	(3)session对象常用的方法;
	String getId();
	setAttribute(String name,Object obj);
	//如果name对应的值不存在,返回null;
	Object getAttribute(String name);
	//解除绑定;
	.removeAttribute(String name);
	
	(4)session的超时;
		1)什么是session超时;
		web服务器会将空闲时间过长的session对象删除;
		大部分服务器缺省的超时时间限制是30分钟,
		
		2)如何修改session超时;
		
		方式一:改配置文件;
		a,改整个web服务器的配置文件;(不推荐,修改覆盖面太大)
		如:对于tomcat,可以修改,conf/web.xml
		<session-config>
			<session-timeout>30</session-timeout>
		</session-config>
		
		b,也可以修改某个具体应用的配置文件;
		如:web工程下的web.xml文件;
		<session-config>
			<session-timeout>30</session-timeout>
		</session-config>
		这样只对此工程有效;
		
		方式二:编程;
		session.setMaxInactiveInterval(int seconds);
	
	(5)删除session对象;
		invalidate();
	
	

例子:记录同一个用户访问服务器的次数;

public void service(HttpServletRequest request,HttpServletResponse response)
	throws ServletException,IOException{
	response.setContentType("text/html;charset=utf-8");
	PrintWriter out = response.getWriter();
	HttpSession session = request.getSession();
	//获得sessionId
	String sessionId = session.getId();
	System.out.println("sessionId:"+sessionId);
	Integer count = (Integer)session.getAttribute("count");
	if(count == null){
		//如果是第一次访问;
		count = 1;
	}else{
		//不是第一次访问;
		count ++ ;
	}
	session.setAttribute("count",count);
	out.println("你是第"+count+"次访问");
	
	out.close();
}



实例:

session验证;

	1)登录模块;
	
	step1,数据库建表;
	create table t_user(
		id int primary key auto_increment,
		username varchar(50) unique,
		pwd varchar(30),
		age int)type=innodb;
		
	insert into t_user(username,pwd,age) values('eric','test',33);

	step2,User类;
	建立一个包:entity;其中新建一个java文件User;
	将之前的dbutil复制过来,并将驱动放入lib文件夹中;
	
	public class User{
		......
	}
	
	step3,UserDAO;
	public class UserDAO{
		public User findByUserName(String username){
			User user = null;
			PreparedStatement prep = null;
			ResultSet rst = null;
			try{
			Connection conn = DBUtil.getConnection();
			prep = conn.prepareStatement("select*from t_user where username=?");
			prep.setString(1,username);
			rst = prep.executeQuery();
			if(rst.next()){
				user = new User();
				user.setId(rst.getInt("id"));
				user.setUsername(username);
				user.setPwd(rst.getString("pwd"));
				user.setAge(rst.getInt("age"));
			}
			}catch(SQLException e){
				e.printStackTrace();
				throw e;
			}finally{
				if(rst!=null){
					rst.close;
				}
				if(prep!=null){
					prep.close;
				}
				DBUtil.close();
			}
			return user;
		}
	}
	
	step4,创建servlet,xml;
	
	public class ActionServlet extends HttpServlet{
		public void service(HttpServletRequest request,HttpServletResponse response)
		throws ServletException,IOException{
			request.setCharacterEncodeing("utf-8");
			String uri = request.getRequestURI();
			String action = uri.substring(uri.lastIndexOf("/"),uri.lastIndexOf("."));
			if(action.equals("/login")){
				//先看验证码是否正确;
				//number1:用户提交的验证码;
				String number = request.getParameter("number");
				//number2:session对象上绑定的验证码;
				HttpSession session = request.getSession();
				String number2 = (String)session.getAttribute("number");
				if(!number1.equalsIgnoreCase(number2)){
					//如果验证码错误,则提示用户重新填写;
					request.setAttribute("number_error","验证码错误");
					request.getRequestDispatcher("login.jsp").forward(request,response);
					return;
				}
				//只有验证码正确的情况下,才会比较用户名和密码是否正确;
				String username = request.getParameter("username");
				String pwd = request.getParameter("pwd");
				UserDAO dao = new UserDAO();
				try{
				User user = dao.findByUserName(username);
				if(user!=null&&user.getPwd().equals(pwd){
					//绑定数据到session对象上,用于验证;
					session.setAttribute("user",user);
					//登录成功,跳转到主功能页面;
					response.sendRedirect("main.jsp");
				}else{
					//登录失败;
					request.setAttribuite("login_error","用户名或密码错误");
					request.getRequestDispatcher("login.jsp").forward(request,response);
				}
				}catch(SQLException e){
					e.printStackTrace();
					//将系统异常扔给容器来处理;
					throw new ServletException(e);
				}
			}
		}
	}

	web.xml:
	......
		<servlet-mapping>
			<servlet-name>ActionServlet</servlet-name>
			<url-pattern>*.do</url-pattern>
		<servlet-mapping>
		<errpr-page>
			<exception-type>javax.servlet.ServletException</exception-type>
			<location>/error.jsp</location>
		</errpr-page>
	......


	创建一个叫error的jsp；
	<body>
		系统繁忙,稍后重试;
	</body>
	
	
	login.jsp;
	......
		<form action="login.do" method="post">
		<fieldset>
			<legend>登录</legend>
			用户名:<input name = "username"/><br/>
			<%
				String msg = (String)request.getAttribute("login_error");
			%>
			<span style="color:red;"><%=(msg==null?"":msg)%></span>
			密码:<input type = "password" name = "pwd"/><br/>
			验证码:<input name="number"/>
			<img src="checkcode" onclick="this.src='checkcode?'+Math.random();"/>
			<%
				String msg2 = (String)request.getAttribute("number_error");
			%>
			<span style="color:red;"><%=(msg==null?"":msg)%></span>
			<input type="submit" value="提交"/>
		</fieldset>
	......
	
	
	main.jsp;
	......
	<%
		//session验证代码;
		Object obj = session.getAttribute("user");  //这里的session是第四个隐藏对象; 
	  	if(obj == null){
	  		//没有登陆成功;
	  		response.sendRedirect("login.jsp");
	  		return;
	  	}
	%>
	主功能页面;
	<%
		System.out.println("登陆后才能执行的java代码");
	%>
	......
	
	
	
	2)如何进行session验证;
		step1,在登录成功以后,在session对象上绑定一些数据,比如:session.setAttribute("user",user);
		step2,对于需要保护的资源(必须登录之后才能访问的地址),添加session验证代码:
		Object obj = session.getAttribute("user");
		if(obj == null){
			response.sendRedirect("login.jsp");
		}
	
	
	
	1.摘要加密算法;
	(1)特点:
		a,不可逆性:即知道了摘要(密文),无法反推出明文(不存在密钥);
		b,唯一性:不同的明文,有不同的摘要;
		
		如:
		public class MD5Util{
			//依据明文str生成一个摘要(密文);
			public static String encrypt(String str){
				MessageDigest md = MessageDigest.getInstance("md5");
				byte[] buf = md.digest(str.getBytes());
				BASE64Encoder encoder = new BASE64Encoder();
				String str2 = encoder.encode(buf);
				return str2;
			}
			
			String str = "I love you";
			MessageDigest md = MessageDigest.getInstance("md5");
			//digest方法:对字节数组按照指定的算法,md5算法生成一个摘要(密文);
			byte[] buf = md.digest(str.getBytes());
			//将字节数组转换成一个字符串;
			//可以使用BASE64Encoder来完成任意的一个字节数组转换成一个字符串;
			BASE64Encoder encoder = new BASE64Encoder();
			String str2 = encoder.encode(buf);
			System.out.println(str2);
		}
	
		所以,无论是注册后保存在数据库端还是之后的登录验证,都必须先经过加密后再做对比验证等
		操作,而且如果客户遗忘密码,此密码无法找回,无论是客户还是服务器端都不知道明文,只有暗文,
		且此类加密无法反推,所以只能使用新的密码;
		
		
	
	session案例;
	
	验证码;
	
	test.jsp:
	
	//在工程下的webroot中创建一个images的文件夹,其中放了若干的验证图片;
	
	<%@page pageEncoding="utf-8" contentType="text/html;charset=utg-8" %>
	<html>
		<head></head>
		<body style="font-size:30px;font-style:italic;">
			<img id="img1" src="images/man1.jpg" onclick="this.src='images/t2.jpg'"/> 
			<a href="javascript:;" onclick="document.getElementById('img1').src='images/t1.jpg'">看不清,换一个</a>
		</body>
	</html>
	
	
	test2.jsp:
	<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>

	<html>
		<head></head>
		<body style="font-size:30px;font-style:italic;">
			......
			验证码:<input name="number"/>
			<img src="checkcode" onclick="this.src='checkcode?'+Math.random();"/> //为了使得src地址与onclick之前不同(否则不会去改变src),
			......																  //必须"欺骗"程序,使用?加不发生重复的参数来实现;
		</body>
	</html>
	
	
	CheckcodeServlet.java:
	
	public class CheckcodeServlet extends HttpServlet{
		private int WIDTH = 80;
		private int HEIGHT = 30;
		public void service(HttpServletRequest request,HttpServletResponse response)
		throws ServletException,IOException{
		
		//一,绘图;
		
		//step1,创建一个内存映像对象(画布);
		BufferedImage image = new BufferedImage(WIDTH,HEIGHT,BufferedImage.TYPE_INT_RGB);
		
		//step2,获得画笔;
		Graphics g = image.getGraphics();
		
		//step3,给画笔上一个随机的颜色;
		Random r = new Random();
		g.setColor(new Color(r.nextInt(255),r.nextInt(255),r.nextInt(255)));
		
		//step4,填充画布;
		g.fillRect(0,0,WIDTH,HEIGHT); //坐标+宽+高;
		
		//step5,设置字体的大小,风格,并在画布上画图;
		String number="";
		for(int i=0;i<5;i++){
			g.setColor(new Color(r.nextInt(255),r.nextInt(255),r.nextInt(255)));
			int h = (int)(HEIGHT*0.3+(HEIGHT*0.6)*r.nextDouble());
			g.setFont(new Font(null,Font.BOLD|Font.ITALIC,h));
			String str = getNumber(1);
			number+=str;
			g.drawString(str,i*WIDTH/5,h);
		}
		
		//将number保存到session对象上;
		HttpSession session = request.getSession();
		session.setAttribute("number",number);
		System.out.println("number:"+number);
		 
		//step6,加一些干扰线;
		for(int i=0;i<5;i++){
			g.setColor(new Color(r.nextInt(255),r.nextInt(255),r.nextInt(255)));
			g.drawLine(r.nextInt(WIDTH),r.next(HEIGHT),r.nextInt(WIDTH),r.next(HEIGHT));
		}
		
		
		//二,压缩图片,输出;
//		response.setContentType("text/html;charset=utf-8");
//		PrintWriter out = response.getWriter();  //字符输出流;
  
 		response.setContentType("image/jpeg");
		OutputStream ops = response.getOutputStream();  //字节输出流
		//write方法:将原始图片使用指定的压缩算法进行压缩,然后使用指定的
		//输出流输出;
		javax.imageio.ImageIo.write(image,"jpeg",ops);
		ops.close();
		 
		}
		//返回一个指定长度的字符串,要求从A-Z,0-9中随机选取字符;
		private String getNumber(int size){
		Random r = new Random();
			String strs = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"+"0123456789";
			String number="";
			for(int i=0;i<size;i++){
				number+=strs.charAt(r.nextInt(strs.length()));
			}
			return number;
		}
	}
	
	
	web.xml:
	......
	<servlet-mapping>
		<servlet-name>CheckcodeServlet</servlet-name>
		<url-pattern>/checkcode</url-pattern>
	<servlet-mapping>
	......
	
	
	
	购物车案例;
	
	(1)商品列表;
	
		Step1,建表;
		
		create table t_product(
			id int primary key auto_increment,
			model varchar(50),
			pic varchar(50),
			prodDesc varchar(255);
			price double
		)type=innodb;
	
		insert into t_product(model,pic,prodDesc,price) 
		values('x200','x200.jpg','good',2000);
		insert into t_product(model,pic,prodDesc,price) 
		values('x500','x500.jpg','good',3000);
		insert into t_product(model,pic,prodDesc,price) 
		values('x600','x600.jpg','good',5000);
		
		
		step2,Product类(实体类,VO,value object);
		
		package entity;
		public class Product{
			private int id;
			private String model;
			private String pic;
			private String prodDesc;
			private double price;
			get/set......
		}
		
		step3,ProductDAO;
		package DAO;
		public class ProductDAO{
			List<Product> findAll() throws SQLException{
				List<Product> products = new ArrayList<Product>();
			PreparedStatement prep = null;
			ResultSet rst = null;
			try{
				Connection conn = DBUtil.getConnection();
				prep = conn.prepareStatement("select * from t_product");
				rst = prep.executeQuery();
				while(rst.next()){
					Product p = new Product();
					p.setId(rst.getInt("id"));
					p.setModel(rst.getString("model"));
					p.setPic(rst.getString("pic"));
					p.setProdDesc(rst.getString("prodDesc"));
					p.setPrice(rst.getDouble("price"));
					products.add(p);
				}
			}catch(SQLException){
				e.printStackTGrace();
				throw e;
			}finally{
				......
				if(conn!=null){
				conn.close();
				......
			}
			
			}
		}
			
			
		step4,ActionServlet;
			该servlet调用ProductDAO的findAll方法,然后转发给prodList.jsp;
		
		public class ActionServlet extends HttpServlet{
			public void service(HttpServletRequest request,HttpServletResponse response)throws ServletException,IOException{
				String uri = request.getRequestURI();
				String action = uri.substring(uri.lastIndexOf("/"),uri.lastIndexOf("."));
				if(action.equals("/list")){
					ProductDAO dao = new ProductDAO();
					try{
					List<Product> products = dao.findAll();
					request.setAttribute("products",products);
					request.getRequestDispatcher("prodList.jsp").forward(request,response);
					}catch(SQLException e){
						e.printStackTrace();
						throw new ServletException(e);
					}
				}
				
			}
		}
		
		
		step5,prodList.jsp;
		根据所给的静态页面;将picture和css文件夹放入webroot文件夹下;
		......
		<%
			List<Product> products = (List<Product>)request.getAttribute("products");
			for(int i = 0;i<products.size();i++){
				Porduct p =products.get(i);
		%>
			<tr>
			......
				<td>
					......<%=p.getModel()%>

				</td>
				<td>
					......￥<%=p.getPrice()%>
				</td>
				<td>
					<a href="buy.do?id=<%=p.getId()%>">购买</a>
					<%
						String msg = (String)request.getAttribute("buy_msg"+p.getId());
					%>
					<span style="color:red;"><%=(msg==null?"":msg)%></span>
				</td>
			......
			</tr>
		<%
			}
		%>
		......
		
		
		为了方便将中文内容通过sql语句存放到数据库中,可以使用数据库访问工具;
		myeclipse中访问数据库的工具;
		window->open perspective->myeclipse database explorer
		DB Brower:右键->new->
		Driver templete->MySql/oracle
		Driver name ......
		Connection URL 同JDBC中;
		User name......
		password ......
		Add JARs: 同JDBC,驱动类载入;
		save password 选中
		->next->Display the selected schemes 选中;
		add->ok;
		选中数据库名称->ok->finish;
		
		在DB Brower中选中数据库名称,右键open connection：
		展开table->选中需要编辑的表;
		在Table/Object Info中有关于此表的可视化信息;
		->preview;
		
		右键选中工程名,new->file->***.sql文件->finish;
		把脚本复制进***.sql文件中:
		
		//避免表格已经存在造成报错,加入一条语句:
		drop table if exists t_product; 
		create table t_product(
			id int primary key auto_increment,
			model varchar(50),
			pic varchar(50),
			prodDesc varchar(255);
			price double
		)type=innodb;
	
		insert into t_product(model,pic,prodDesc,price) 
		values('x200','x200.jpg','性价比不错,适合学生使用',2000);
		insert into t_product(model,pic,prodDesc,price) 
		values('x500','x500.jpg','适合商务,配置高档',3000);
		insert into t_product(model,pic,prodDesc,price) 
		values('x600','x600.jpg','性能卓越',5000);
		
		
		然后在名为connection的下拉列表中选中数据库名,执行;
		到视图中检查是否已经将数据导入数据库;
		
		
		package bean;
		
		public class Cart{
			//items:用于保存用户所购买的商品;
			private List<CartItem> items = new ArrayList<CartItam>;
			
			//将商品添加到购物车;
			public boolean add(CartItem item){
				for(int i=0;i<items.size();i++){
					CartItam curr = iems.get(i);
					if(curr.getP().getId()==item.getP().getId()){
						//说明已经购买过此item;
						return false;
					}
					//第一次购买此item商品;
					items.add(item);
					return true;
				}
			}
		
		//修改商品的数量;
		public void modify(int id,int qtu){
			for(int i=0;i<items.size();i++){
				CartItem curr = items.get(i);
				if(curr.getP().getId()==id){
					if(qty==0){
						items.remove(curr);
					}else{
					curr.setQty(qty);
					}
				}
			}
		}
		
		//删除商品;
		public void delete(int id){
			for(int i=0;i<items.size();i++){
				CartItem curr = items.get(i);
				if(curr.getP().getId() == id){
					items.remove(curr);
				}
			}
		}
		  
		//清空;
		public void clear(){
			items.clear();
		}
		
		//计价;
		public double cost(){
			double total = 0;
			for(int i=0;i<items.size();i++){
				CartItem curr = items.get(i);
				total+=curr.getQty()*curr.getP().getPrice();
			}
			return total;
		}
		
		//将购物车中的所有商品条目返回后由JSP以html形式给浏览器;
		public List<CartItem> list(){
			return items;
		}
	}
	
		
		//商品条目类,为了方便实现购物车的功能而设计;
		public class CartItem{
			private Product p;
			privateint qty;
			get/set......
			
		}
	
		
		public class ActionServlet extends HttpServlet{
			public void service(HttpServletRequest request,HttpServletResponse response)throws ServletException,IOException{
				String uri = request.getRequestURI();
				String action = uri.substring(uri.lastIndexOf("/"),uri.lastIndexOf("."));
				if(action.equals("/list")){
					ProductDAO dao = new ProductDAO();
					try{
					List<Product> products = dao.findAll();
					request.setAttribute("products",products);
					request.getRequestDispatcher("prodList.jsp").forward(request,response);
					}catch(SQLException e){
						e.printStackTrace();
						throw new ServletException(e);
					}
				}else if(action.equals("/buy")){
					int id = Integer.parseInt(request.getParameter("id"));
					HttpSession session =request.getSession();
					Cart cart = (Cart)session.getAttribute("cart");
					if(cart == null){
						//第一次购买;
						cart = new Cart();
						session.setAttribute("cart",cart);
					}
					ProductDAO dao = newProductDAO();
					try{
						Product p =dao.findById();
						//将商品封装成一个商品条目对象(CartItem);
						CartItem item = newCartItem();
						item.setP(p);
						item.setQty(1);
						boolean flag = cart.add(item);
						if(flag){
							//添加成功;
							request.setAttribute("buy_msg_"+p.getId(),"已经将商品添加到购物车");
						}else{
							//已经购买过;
							request.setAttribute("buy_msg_"+p.getId(),"已经购买过该商品,请到购物车中修改数量");
						}
						request.getRequestDispatcher("list.do").forward(request,response);
					}catch(SQLException e){
						e.printStackTrace();
						throw new ServletException(e);
					}
				}else if(action.equals("/modify")){
					int id = Integer.parseInt(request.getParameter("id"));
					int qty = Integer.parseInt(request.getParameter("qty"));
					HttpSession session = request.getSession();
					Cart cart = (Cart)session.getAttribute("cart");
					cart.modify(id,qty);
					response.sendRedirect("cart.jsp");
				}else if(action.equals("/delete")){
					int id = Integer.parseInt(request.getParameter("id"));
					HttpSession session = request.getSession();
					Cart cart = (Cart)session.getAttribute("cart");
					cart.delete(id);
					response.sendRedirect("cart.jsp");
				}else if(action.equals("/clear")){
					HttpSession session = request.getSession();
					Cart cart = (Cart)session.getAttribute("cart");
					cart.clear();
					response.sendRedirect("cart.jsp");
				}
				
			}
		}
		
		
		//点击查看购物车后:
		......
		<input class="button" type="button" value="查看购物车" name="settingsubmit" onclick="location = 'cart.jsp';">
		......
		
		cart.jsp:
		将静态页面复制入jsp;
		在webroot文件夹下建立一个js文件夹,将prototype.js放入;
		
		<%@page import="bean.*,java.util.*"%>
		<html>
			<head>
				......
				<script type="text/javascript" src="<%=request.getContextPath()%>/js/prototype-1.6.0.3.js">
				<script type="text/javascript">
					function modify(id,qty){
						//检查数量是否为空;
						if(qty.strip().length==0){
							alert('数量必须输入');
							return;
						}
						//检查是否为数字;
						var reg = /^[0-9]+$/;
						if(!reg.test(qty.strip())){
							alert('必须是数字');
							return;
						}
						location='modify.do?id='+id+'&qty='+qty.strip();
						
					}
				</script>
			</head>
		<%
			Cart cart = (Cart)session.getAttribute("cart");
			if(cart!=null&&cart.list().size>0){
				//显示所有购买的商品;
				List<CartItem> items = cart.list();
				for(int i=0;i<items.size();i++){
		%>
					......
					<td class="">
						<%=item.getP().getModel()%>
					</td>
					<td class="" colspan="6">
						<b><%=cart.cost()%></b>
					</td>
					<td class="">
						<input type="text" size="3" value="" id="p_<%=item.getP().getId()%>"/>
					</td>
					<td class="">
						<%//<a href="javascript:;" onclick="location='modify.do?id=<%=item.getP().getId()%>&qty='+document.getElementById('p_<%=item.getP().getId()%>').value;">更改数量</a>%>
						
						<a href="javascript:;" onclick="modify(<%=item.getP().getId()%>,document.getElementById('p_<%=item.getP().getId()%>').value);">更改数量</a>
					
					</td>
					<td class="">
						<a href="javascript:;" onclick="location='delete.do?id=<%=item.getP().getId()%>'">删除</a>
					</td>
		<%
				}
		%>
				总价格:
		<%
				cart.cost();
			}else{
				//显示还没有选购商品;
		%>
				......
				<b>还没有选购商品</b>
				......
		<%
			}
		%>
				</br>
				<input class="button" type="button" value="返回商品列表" name="settingsubmit" onclick="location='list.do';">
				</br>
				<input class="button" type="button" value="清空购物车" name="settingsubmit" onclick="location='clear.do';">
		......
		</html>
		
		
		
prototype.js 框架的简单使用;

a.$(id):document.getElementById(id);
b.$F(id):$(id).value;
c.$(id1,id2,id3...):依据参数查找对应的节点并返回由这些节点组成的数组;
d.strip():去掉字符串两端的空格;
e.evalJSON():将json字符串转换成一个javascript对象;
		
		
		
过滤器;
(1)什么是过滤器;
servlet规范当中定义的一种特殊的组件,可以拦截servlet容器调用的过程,并进行相应的处理;

(2)编程;
	step1,写一个java类,实现Filter接口;
	step2,在doFilter方法中实现处理逻辑;
	step3,配置过滤器(web.xml);
	
(3)过滤器的优先级;
	当有多个过滤器都满足过滤的条件,则容器依据<filter-mapping>的先后顺序来调用;
	
(4)初始化参数;
	step1,使用<init-param>来配置初始化参数;(与servlet的初始化参数配置方式完全一样)
	step2,使用FilterConfig提供的方法:String getInitParameter(String paramname);
	

	如:
	comment.jsp
	<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>

	<html>
		<head></head>
		<body style="font-size:30px;font-style:italic;">
			<form action="comment" method="post">
				请输入你的评论:<input name="content"/>
				<input type="submit" value="提交"/>
			</form>
		</body>
	</html>
		
	
	comment.java
	......
	public class CommentServlet extends HttpServlet{
		public void service(HttpServletRequest request,HttpServletResponse response)throws ServletException,IOException{
			response.setContentType("text/html;charset=utf-8");
			PrintWriter out = response.getWriter();
			request.setCharacterEncoding("utf-8");
			String content = request.getParameter("content");
			out.println("<h1>你的评论是:"+content+"</h1>");
			out.close();
	}
	
	
	CommentFilterA.java
	
	import javax.servlet.Filter;
	public class CommentFilterA implements Filter{
	
		private FilterConfig config;
		
		//容器在删除过滤器实例之前会调用该方法,只会执行一次;
		public void destroy(){
		
		}
		
		//容器收到请求后,会调用doFilter方法来处理请求,并发事先创建好的request,response对象传给doFilter;
		//FilterChina对象有一个doFilter方法,调用了该方法,表示让容器继续调用后续的过滤器或者是servlet;
		//如果没有调用doFilter方法,表示请求处理完毕,不再调用后续的过滤器或者servlet;
		public void doFilter(ServletRequest arg0,ServletResponse arg1,FilterChain arg2) throws ServletException{
			HttpServletRequest request = (HttpServletRequest)arg0;
			HttpServletResponse response = (HttpServletResponse)arg1;
			request.setCharacterEncoding("utf-8");
			String content = request.getParameter("content");
			response.setContentType("text/html;charset=utf-8");
			PrintWriter out = response.getWriter();
			
			//检查是否有敏感字;
			String sensitiveChar = config.getInitParameter("sensitiveChar");
			if(content.indexOf(sensitiveChar)！=-1){
				out.println("<h1>评论包含非法字符</h1>");
			}else{
				//调用后续的过滤器或者servlet;
				arg2.doFilter(arg0,arg1);
			}
		}
		
		//容器启动的时候,就会创建过滤器实例(只有一个实例),接下来容器会调用init方法来完成初始化操作;
		//容器会实现创建好一个FilterConfig对象,该对象提供了一个getInitParameter方法,来访问过滤器的
		//初始化参数;init方法只会执行一次;
		public void init(FilterConfig arg0) throws ServletException{
			config= arg0;
		}
		
	}
	
	
	web.xml
	......
		<!--全局的初始化参数-->
		<context-param>
			<param-name>campany</param-name>
			<param-value>tarena</param-value>
		</context-param>
		<!--过滤器A-->
		<filter>
			<filter-name>filterA</filter-name>	
			<filter-class>web.CommentFilterA</filter-class>	
			<init-param>
				<param-name>sensitiveChar</param-name>
				<param-value>裆</param-value>
			</init-param>
		</filter>
		<!--过滤器B-->
		<filter>
			<filter-name>filterB</filter-name>
			<filter-class>web.CommentFilterB</filter-class>	
			<init-param>
				<param-name>size</param-name>
				<param-value>10</param-value>context-param
			</init-param>
		</filter>
		
		<filter-mapping>
			<filter-name>filterA</filter-name>	
			<url-pattern>/comment</url-pattern>	
		</filter-mapping>
		<filter-mapping>
			<filter-name>filterB</filter-name>	
			<url-pattern>/comment</url-pattern>	
		</filter-mapping>
		
		<!--监听器-->
		<listener>
			<listener-class>web.CountListener</listener-class>
		</listener>
		
		<servlet>
			<servlet-name>CommentServelt</servlet-name>
			<url-class>web.CommentServlet</url-class>
		<servlet>
		<servlet-mapping>
			<servlet-name>CommentServelt</servlet-name>
			<url-pattern>/comment</url-pattern>
		<servlet-mapping>
	......
	
	
	
例子:控制评论字数的过滤器;

public class CommentFilterB implements Filter{

	private FilterConfig config;

	public void destroy(){
		
	}
		
	public void init(FilterConfig arg0) throws ServletException{
		//将FilterConfig对象的引用保存下来;
		config = arg0;
	}
		
	public void doFilter(ServletRequest arg0,ServletResponse arg1,FilterChain arg2) throws ServletException{
		HttpServletRequest request = (HttpServletRequest)arg0;
		HttpServletResponse response = (HttpServletResponse)arg1;
		request.setCharacterEncoding("utf-8");
		String content = request.getParameter("content");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		
		int size = Integer.parseInt(config.getInitParameter("size"));
		if(content.length()>size){
		out.println("<h1>评论的字数过多</h1>");
		}else{
			arg2.doFilter(arg0,arg1);
		}
	}
		
}



监听器;
(1)什么是监听器;
	servlet规范当中定义的一种特殊的组件,用来监听servlet容器产生的事件,并进行相应的处理;
	主要是监听容器产生的两大类事件;
	第一类:生命周期相关的事件,容器创建或者销毁request,session,servletContext对象时产生的事件;
	第二类:绑定相关事件,指的是调用了以上三个对象的setAttribute,removeAttribute方法产生的事件;

(2)ServletContext接口;
	1)容器在启动的时候,会为每一个应用创建唯一的一个符合ServletContext接口的实例,一般将这个实例
	称之为servlet上下文;servlet上下文会一直存在,除非容器关闭,或者应用被移除;
	2)如何获得servlet上下文;
		a,GenericServlet提供的getServletContext方法;
		b,Httpsession提供的getServletContext方法;
		c,ServletConfig提供的getServletContext方法;
		d,FilterConfig提供的getServletContext方法;
	
(3)作用;

	a,绑定的数据可以被同一个应用的所有的组件访问,并且可以随时访问;
		注意:request,session,ServletContext都有setAttribute,getAttribute,removeAttribute
		方法,它们的区别是:
		
		1.生命周期: request<session<ServletContext
		request:一次请求与响应期间;
		session:多次请求与相应期间;
		ServletContext:只要容器存在,就会一直存在;
		一般来说,如果以上三个对象都可以解决绑定时的问题,应该优先使用生命周期短的,
		以节省资源;
		
		2.以上三个对象被访问的范围不同,ServletContext>session>request;
		request对象:只有同一个请求当中所涉及的组件可以访问;
		session:同一个会话(未关闭的同一个浏览器/同一个浏览器进程)所涉及的组件可以访问;
		ServletContext:同一个应用中的所有组件;
		
	b,访问全局初始化参数;
	同一个应用中,可以被servlet,filter共享的初始化参数;
	
	step1:使用如下配置全局的初始化参数;
	<context-param>
		<param-name>address</param-name>
		<param-value>北京海淀区</param-value>
	</context-param>
	step2:使用servlet上下文提供的
	String getInitParameter(String paramname);
	
	c,依据逻辑路径获得实际部署时的物理路径;
		String getRealPath(String path);
		
	
如:

servlet1:

	public void service(HttpServletRequest request,HttpServletResponse response)
	throws ServletException,IOException{
	response.setContentType("text/html;charset=utf-8");
	PrintWriter out = response.getWriter();
	
	//将数据绑定到servletContext
	ServletContext sc = getServletContext();
	sc.setAttribute("company","tarena");
	out.println("<h1>数据已绑定到servletContext</h1>");
	String address = sc.getInitParameter("address");
	out.println("address:"+address);
	out.close();


servlet2:

	public void service(HttpServletRequest request,HttpServletResponse response)
	throws ServletException,IOException{
	response.setContentType("text/html;charset=utf-8");
	PrintWriter out = response.getWriter();
	ServletContext sc = getServletContext(); //这里可以直接调用getServletContext方法是因为HttpServlet继承了抽象类GenericServlet中的此方法,
					                         //而GenericServlet中的此方法也是通过getServletConfig().getServletContext()调用了ServletConfig接口中的此方法;
	String company = (String)sc.getAttribute("company");
	out.println("<h1>"+company+"</h1>");
	String address = sc.getInitParameter("address");
	out.println("address:"+address);
	out.close();
	
	
	
监听器的编程;

监听器在容器启动的时候,会创建监听器实例;且仅有一个实例;

step1,写一个java类,实习监听器接口;

比如监听session的创建和销毁,需要实现HttpSessionListener接口;

step2,实现监听器接口中的方法;

step3,注册(web.xml);


当浏览器地址栏仅有应用名,而没有具体转向的页面时,就会访问webroot文件夹下的这个index.jsp;
<welcome-file-list>
	<welcome-file>/index.jsp</welcome-file>
</welcome-file-list>


例子:统计在线人数;

index.jsp:

	<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>

	<html>
		<head></head>
		<body style="font-size:30px;font-style:italic;">
			这是首页<br/>
			当前系统在线人数是:
			<%=application.getAttribute("count")%> //application是jsp的又一个隐含对象,相当于session.getServletContext(),也就是此应用的servlet上下文;
		
			<a href="logout.jsp">退出系统</a>
		
		</body>
	</html>


logout.jsp:

	<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>

	<html>
		<head>
		</head>
		<body style="font-size:30px;font-style:italic;">
			<%
				session.invalidate(); //立即销毁session对象--session destroyed;
			%>
		
		</body>
	</html>


CountListerner.java:

public class CountListener implements HttpSessionListener{
	
	private int count = 0; //计数器;
	
	//当容器创建了一个session对象,会调用sessionCreated方法,HttpSessionEvent是一个事件对象;
	public void sessionCreated(HttpSessionEvent arg0){
		System.out.println("sessionCreated...")
		count++;
		HttpSession session = arg0.getSession();
		ServletContext sc = session.getServletContext();
		sc.setAttribute("count",count);
	}

	//当容器销毁了一个session对象,会调用sessionDestroyed方法,HttpSessionEvent是一个事件对象;
	public void sessionDestroyed(HttpSessionEvent arg0){
		System.out.println("sessionDestroyed...")
		count--;
		HttpSession session = arg0.getSession();
		ServletContext sc = session.getServletContext();
		sc.setAttribute("count",count);
	}

}


注意:
1.如果需要模拟多个session被创建的过程来测试监听器,不能同时开启同一个浏览器多次,因为同一个浏览其共享一块内存空间,其sessionId是相同的,
也就是说同一个浏览器是属于同一个进程的,所以需要打开不同的浏览器来测试session监听器;
2.关闭浏览其并不会导致相关的session被销毁,而是需要等到服务器端的session对象超时;	

	
	
例子:容器启动之后,执行一个定时的任务,每隔2秒钟,输出当前系统时间;
写一个监听器,监听servlet上下文,只要servlet上下文被创建,则启动这个定时任务,一旦其被销毁则
取消这个任务;


TimerListerner.java:

import java.util.Timer;
import java.util.TimerTask;
import java.util.Date;
import java.text.SimpleDateFormat;

public class TimerListerner implements ServletContextListener{
	
	Timer timer;
	SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
	
	TimerTask task = new TimerTask(){
		public void run(){
			System.out.println("系统当前时间:");
			System.out.println(sdf.format(new Date()));
		}
	};
	
	public void contextInitialized(ServletContextEvent sce){
		timer=new Timer();
		timer.scheduleAtFixedRate(task,new Date(),2000);
	}
	
	public void contextDestroyed(ServletContextEvent sce){
		timer.cancel();
	}


}

web.xml

	<!--监听器-->
	<listener>
		<listener-class>web.TimerListerner</listener-class>
	</listener>


注意:在一些情况下myeclipse在通过tomcat运行时,如上例会发生中文在控制台输出为问号或者乱码的情况,而在浏览器中的输出与正常myeclipse输出都无问题,这种情况下需要在开启server的configure server中选中所使用的tomcat版本,然后是Launch->create launch configuration->arguments->VM arguments的最后加上-Dfile.encoding=UTF-8,然后apply,之后run新创建的tomcat就可以了;

	
扩展实例:上传文件;
	
step1:给表单添加上传文件域名,并且设置表单的enctype的属性值为"multipart/form-data";
表单的提交方式必须为"post";
<input type="file"/>
	
step2:在服务器端,要使用InputStream request.getInputStream获得一个流,通过分析该流来获得相应的数据;
一般使用一些工具(比如:apache提供的commoms-fileupload.jar)来分析该流;


form1.jsp:

	<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>

	<html>
		<head></head>
		<body style="font-size:30px;font-style:italic;">
			<form action="fileupload" method="post" enctype="multipart/form-data">
				username:<input name="username"/><br/>
				photo:<input type="file" name="file1"/><br/>
				<input type="submit" value="Confirm"/>
			</form>
		</body>
	</html>


	web.xml:
	......
	<servlet-mapping>
		<servlet-name>FileupdpadServlet</servlet-name>
		<url-pattern>/fileupload</url-pattern>
	<servlet-mapping>
	......


//将commoms-fileupload.jar等jar文件放入lib;
//在webroot中创建一个upload的文件夹;
FileupdpadServlet.java

public class FileupdpadServlet extends HttpServlet{
	public void service(HttpServletRequest request,HttpServletResponse response)
	throws ServletException,IOException{
		String username = request.getParameter("username"); //这里获取的username为null,因为一旦在form中使用了
													        //enctype="multipart/form-data"服务器就不会再对参
													        //数做解析,需要自己解析;
		InputStream ips = request.getInputStream();   //过于简单原始的获取方式,未达到解析效果;
		
		//factory对象为解析器提供解析时缺省配置;
		FileItemFactory factory = new DiskFileItemFactory();
		//创建一个解析器;
		ServletFileUpload sfu = new ServletFileUpload(factory);
		//使用解析器;
		try{
		//解析器将解析的结果封装到FileItem对象上,一个表单域对应一个FileItem对象;
		//只需调用FileItem对象提供的方法即可获得相应表单域的数据;
		List<FileItem> items = sfu.parseRequest(request);
		for(int i=0;i<items.size();i++){
			FileItem item = items.get(i);
			if(item.isFormField()){
			//普通表单域;
				String username = item.getString();
				Ststem.out.println("username:"+username);
			}else{
			//上传文件域;
			ServletContext sc = getServletContext();
			//依据逻辑路径(upload)获得实际部署时的物理路径;
			String path = sc.getRealPath("upload");
			//获得文件名;
			String filename = item.getName();
			File file = new File(path+"//"+filename);
			item.write(file);
			}
		}
		}catch(Exception e){
			e.printStackTrace();
		}
		
	}
}
	
	

servlet线程安全问题;

(1)servlet线程安全问题产生的原因;
	a,容器在默认情况下,只会为一个servlet创建一个实例;
	b,当容器收到多个请求的时候,会启动多个线程来处理,如果多个线程同时访问某个servlet实例,
	就需要考虑线程安全问题(比如修改servlet中的属性);

(2)解决方法;
	a,加锁,即使用synchronized对方法或者代码块加锁;
	b,让servlet实现SingleThreadModel接口,容器会为实现了该接口的servlet创建多个实例;
	如果请求过多,容器会产生过多的servlet实例,所以不建议使用;与Serializable接口一样,
	SingleThreadModel接口也属于标识接口,其中没有任何内容;
	
	
例子:

public class SomeServlet extends HttpServlet{

	private int count = 0;
	
	public void service(HttpServletRequest request,HttpServletResponse response)
	throws ServletException,IOException{
	synchronized(this){
	count++;
	try{
		Thread.sleep(1000);
	}catch(InterruptedException e){
		e.printStackTrace();
	}
	System.out.println(Thread.currentThread().getName()+":"+count);
	}
  	}

}
	
	
	
禁止cookie后,如何继续使用session?

(1)解决方式;
	可以使用url重写机制来解决;

(2)什么是url重写;
	在访问服务器上的某个地址时,需要使用服务器生成的地址(该地址会添加sessionId);

(3)编程;
	//用于连接和表单提交;
	String response.encodeURL(String url);
	如:
	<a href="<%=response.encodeURL("some")%>"></a>
	查看浏览器端源代码时,这里转换为:<a href="some;jsessionid=4C5F94AB3...">
	相当于服务器端把客户端发出请求后创建的sessionId通过response.encodeURL()方法传给了浏览器端,
	之后浏览器端再通过连接向服务器端发送请求时也会将此sessionId传给服务器端;
	
	<form action="<%=response.encodeURL("some")%>"></foem>
	
	//用于重定向;
	String response.encodeRedirectURL(String url);
	如:
	response.sendRedirect(response.encodeRedirectURL("cart.jsp"));
	
	注意:转发不用考虑,不存在session问题;


例子:

	<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>

	<html>
		<head></head>
		<body style="font-size:30px;font-style:italic;">
			<a href="<%=response.encodeURL("some")%>">点这里,访问SomeDervlet</a>
		</body>
	</html>



对JSP的补充;

1)html:直接写;
2)java代码:
	a,java代码片段; <% %>
	b,jsp表达式;    <%= %>
	c,jsp声明;	    <%! %>
	
3)指令;
a,page指令;
	pageEncoding属性:告诉容器jsp文件的编码格式;
	contentType属性:设置response.setContentType()的内容;
	import属性:导包;
	session属性:true(defalut)/false,当值为false时,容器不再添加获得session对象的代码了;
	即不能再使用session这个隐含对象了;
	isErrorPage属性; true/false(defalut); 当为true时,表示这是一个错误处理页面(专门来处理别的页面的异常);
	errorPage属性;	指定一个错误处理页面;
	
b,include指令;
file属性:讲一个文件的内容插入到指令所在的位置;、

c,tablib指令;
引入jsp标签;


4)隐含对象;
out,request,response,session,application,exception:当errorPage属性为true时,才能
使用这个对象,可以通过该对象获得一个异常信息;

如:
a3.jsp:
<%@page errorPage="a4.jsp"%>
......
<%
	String number = request.getParameter("number");
	int i = Integer.parseInt(number);
	out.println(i+100);
%>
......

a4.jsp:
<%@page isErrorPage="true"%>
......
<%
	<%=exception.getMessage()%>
%>
......


config:ServletConfig实例; 可以把jsp当成一个servlet去配置(配置初始化参数);

如:

a5.jsp:
......
company:<%=config.getInitParameter("company")%>
......

web.xml:
<servlet>
	<servlet-name>a5.jsp</servlet-name>
	<jsp-file>/a5.jsp</jsp-file>
	<init-param>
		<param-name>company</param-name>
		<param-value>tarena</param-value>
	</init-param>
</servlet>
<servlet-mapping>
	<servlet-name>a5.jsp</servlet-name>
	<url-pattern>/abc.html</url-pattern>
<servlet-mapping>


pageContext:PageContext实例; 容器会为每一个jsp实例创建唯一的一个符合PageContext接口要求的实例,称之为PageContext；
作用1:绑定数据; 绑定的数据只有对应的jsp实例可以访问;
作用2:提供了相应的方法来找到其它8个隐藏对象;

如:

a6.jsp:
......
	<%
		pageContext.setAttribute("ename","zs");
	%>
	ename:<%=pageContext.getAttribute("ename")%>
......

page:相当于this,jsp实例本身(jsp对应的servlet实例);


对象的获取:

当一个servlet访问,就会生成一个HttpServlet对象,以及包含了这个servlet各种信息,参数的ServletConfig对象,
Filter中也有类似的FilterConfig对象,可以通过覆写带参数的init方法来获取;而由于HttpServlet抽象类继承了GenericServlet抽象类(实现了Servlet接口),
在GenericServlet中已经封装好了类似:
ServletConfig getServletConfig();	   //这是通过init方法中传入的ServletConfig对象参数后封装的方法;
ServletContext getServletContext();    //通过ServletConfig对象来获得ServletContext的方法;
String getInitParameter(String name);  //通过ServletConfig对象来获得参数的方法;

所以在继承了HttpServlet的servlet类中的service方法里可以直接调用这些方法;
另外,ServletConfig对象调用的getInitParameter()方法获取的是声明在某个servlet中的参数,如:

  <servlet>
  	<servlet-name>song</servlet-name>
  	<servlet-class>web.SongServlet</servlet-class> 	
  	<init-param>
  		<param-name>song</param-name>
   		<param-value>song</param-value>
  	</init-param>
  </servlet>
  
也可以访问jsp-file中声明的参数,如:
	<servlet-name>a5.jsp</servlet-name>
	<jsp-file>/a5.jsp</jsp-file>
	<init-param>
		<param-name>company</param-name>
		<param-value>tarena</param-value>
	</init-param>
</servlet>

与FilterConfig对象的getInitParameter()访问的方式一样,如:
		<filter>
			<filter-name>filterB</filter-name>
			<filter-class>web.CommentFilterB</filter-class>	
			<init-param>
				<param-name>size</param-name>
				<param-value>10</param-value>
			</init-param>
		</filter>
		
而在获取了ServletConfig对象后也可通过它来获取ServletContext,利用getServletContext()方法,
从而利用ServletContext对象来对全局变量(context-param)进行访问,同样是getInitParameter();
不过,ServletConfig没有get/setAttribute()方法,而ServletContext有生命周期最长,访问范围最大的get/setAttribute()方法;


路径的获取:

URI和URL的定义及关系;
URL是RUI命名机制的一个子集;
URI:由一个通过通用资源标志符(Universal Resource Identifier, 简称"URI")进行定位; URI可以是绝对路径,也可以是相对路径;
URL:URL是Uniform Resource Location的缩写,译为"统一资源定位符"; URL一定是绝对路径,因为它必须独立定位到一个准确的资源;


getContextPath、getServletPath、getRequestURI的区别:

假定web application名称为news,在WebRoot文件夹下的main文件夹中存在list.jsp;
在浏览器中输入请求路径： 
http://localhost:8080/news/main/list.jsp

则执行下面代码后打印出如下结果： 
1,System.out.println(request.getContextPath()); //可返回站点的根路径,也就是项目的名字 ;
打印结果：/news 

2,System.out.println(request.getServletPath()); 
打印结果：/main/list.jsp 

3, System.out.println(request.getRequestURI()); 
打印结果：/news/main/list.jsp 

4, System.out.println(getServletContext().getRealPath("list.jsp")); //注意request.getRealPath()方法已经背标为Deprecated;
打印结果：F:\Tomcat 6.0\webapps\news\list.jsp

	
jsp是如何执行的;

step1,.jsp先转换成.java(即servlet类)
html----> service方法,使用out.write输出;
<% %>---> service方法,照搬;
<%= %>--->service方法,使用out.print输出;
<%! %>--->为servlet类增加新的属性或者方法;

step2,调用servlet;

	
如:
......
	<%!
		int i = 100;
		int sum(int a1,int a2){
			return a1+a2;
		}
	%>
......


JSP的注释;

a,<!--注释内容-->:运行注释的内容是java代码,并且java代码会执行,只是结果不会输出;
b,<%--注释内容---%>:不允许注释的内容为java代码;

如:

a8.jsp:
......
	当前的系统时间:<!--<%=new Date()%>-->  //在页面的源代码中其实这段java代码已经执行,并且html注释中已经变为实际的日期,
										  //但是由于实在注释内容中,所以在浏览器上并不会显示出来;
	当前的系统时间:<%--<%=new Date()%>--%> //在页面的源代码中:<%--<%=new Date()%>--%>这段内容没有出现;
......



JSP标签和el表达式;

(1)jsp标签是什么;

因为直接在jsp文件里面写java代码,不方便代码的维护(比如jsp交给美工区修改);
所以,sun公司制定了jsp标签技术规范,利用类似于html标签来代替jsp文件中的java代码;

jsp标签能对应一个标签类,容器在执行jsp标签时,会调用对应的标签类中的java代码;

(2)el表达式是什么?
是一套简单的计算规则,用于给jsp的标签的属性赋值,也可以直接使用(脱离jsp标签);

(3)如何使用el表达式;

1)访问bean的属性; //bean就是java类;

a,方式一:
${user.name} 
容器会依次从pageContext,request,session,application隐含对象中查找绑定名称为"user"的对象;
一旦找到就不会查找下面的对象,查找: getAttribute("user");
接下来会调用该对象的"getName"方法(会自动将name的n大写成getName方法来查找);
最后输出;

与直接使用java代码来访问bean的属性相比,使用el表达式有两个优点:
1,会将属性值Null转换成空字符串""输出;
2,如果依据绑定名找不到对应的对象,会输出空字符串"",不会报空指针异常;


如:

packet bean;

public class User{
	private String uname;
	private int age;
	private String[] interest;
	get/set...
}


jsp:

......
	<%
		User user= new User();
		user.setUname("Jetty");
		user.setAge(33);
		user.setInterest(new String[]{"snoker","fishing"});
		request.setAttribute("user",user);
		
		User user2= new User();
		user.setUname("Tommy");
		user.setAge(33);
		session.setAttribute("user",user2);
	%>
	uname:<%
			User user1 = (User)request.getAttribute("user");
			out.println(user1.getName());
		  %>
	<br/>
	uname:${user.uname}<br/>
	uname:${sessionScope.user.uname}<br/>
	uname:&{user["uname"]}
......


b,方式二:
${user["uname"]}
优势是:允许[]中出现绑定名称,也允许[]中出现下标(从0开始的整数,用于访问数组中的元素);

如:
<%
	request.setAttribute("propname","uname"); 
%>
uname:${user[propname]}<br/>
interest:${user.interest[0]}


2)读取请求参数值;

${param.uname}等价于:
request.getParameter("uname");

${paramValues.city[0]}等价于:
request.getParameterValues("city"); //返回一个字符串数组;


3)进行一些简单的计算,结果用于给jsp标签的属性赋值,也可直接输出;

a,算术运算; + - * / % ,其中+只能用于求和,不能连接字符串;

如:
1+1=${1+1}
${"a"+"b"}  //报错;
${"111"+"111"}

b,关系运算; >,>=,<,<=,==,!=; 

如:
1>2:${1>2}
<%
	request.setAttribute("str1","abc");
	request.setAttribute("str2","abc");
%>
${str1==str2}


c,逻辑运算; &&,||,!;

如:
${1<2&&2<3}


d,empty运算; 判断一个字符串是否是空字符串,或者一个集合是否内容为空;

一下四种情况,结果为true;
空字符串,空的集合,null,找不到的对象;

如:
空字符串:${empty ""}



(4)jstl标签;
1)jstl(java standard taglib java标准标签库);由apache开发的jsp标签,捐献给了sun；


2)怎样使用jstl标签;

step1,将jstl标签对应的jar文件拷贝到WEB-INF\lib下;
jar文件在类似:C:\MyEclipse\plugins\com.genuitec.eclipse.j2eedt.core_x.x.x\data\libraryset\JSTL1.1\lib中;
standard.jar jstl.jar

step2,使用taglib指令引入jsp标签;

<%@taglib uri="" prefix=""%>
uri属性:指定一个命名空间(区分同名的元素);
prefix属性:命名空间的前缀;

uri查找方法,在工程名下的J2EE XX Libraries中的standard.jar里的c.tld,在描述文件中
找到类似<uri>http://java.sun.com/jsp/jstl/core</uri>,然后将内容放入uri=""中;

tomcat在加载一个web工程的时候会去WEB-INF文件夹中加载web.xml和文件夹下的所有的.tld文件,当程序执行到引入标签的语句,就会去这些.tld文件
中查找相应的uri地址,找到后实例化这些标签类,然后在调用了标签语句的地方在标签类中调用相应的方法来转换为java代码并执行;

也可以在web.xml中声明这些tag:

<taglib> 
    <taglib-uri> 
		http://jakarta.apache.org/tomcat/examples-taglib 
    </taglib-uri> 
    <taglib-location> 
       /WEB-INF/jsp/example-taglib.tld 
    </taglib-location> 
</taglib> 


3)jstl的核心标签;

a,<c:if test="" var="" scope=""></c:if>
作用:当test属性为true,容器会执行标签体的内容,标签体可以是html,也可以是java代码;
test属性可以使用el表达式来赋值;

var属性:指定一个绑定名;
scope属性:指定绑定的范围,可以是"page","request","session","application";

如:<%@taglib uri="http://java.sun.com/jsp/hstl/core" prefix="c" %>
......
	<c:if test="${1>0}">
		今天运气很不错
	</c:if>
	<*
		Employee e = new Employee();
		e.setGender("m");
	*>
	request.setAttribute("e",e);
	性别:<c:if test="${e.gender=='m'}" var="rs" scope="request">男</c:if>
	<c:if test="${!rs}">女</c:if>
......

4)jsp标签是如何运行的?
容器根据命名空间找到标签的描述文件(.tld文件),然后一句标签的名称找到标签的类名,然后创建标签类的实例,
然后执行标签对象相应的方法;


b,<c:choose>
	<c:when test="">
		标签体
	</c:when>
	...
	<c:otherwise>
	</c:otherwise>
</c:choose>

when可以出现一次或者多次,当test属性为true,执行标签体的内容;
test属性可以使用el表达式;

otherwise可以出现0次或者1次,表示例外;

例子:

a1.jsp;

<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" import="bean.*" %>
<%@taglib uri="java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
	<head></head>
	<body style="font-size:30px;font-style:italic;">
		<%
			Employee e = new Employee();
			e.setGender("m");
			request.setAttribute("e",e);
		%>
		性别:<c:choose>
			<c:when test="${e.gender=='m'}">男</c:when>
			<c:when test="${e.gender=='f'}">女</c:when>
			<c:otherwise>未知</c:otherwise>
		</c:choose>
	</body>
</html>


c,<c:forEach var="" items="" varStatus="">
	
</c:forEach>

该标签用来遍历一个集合,其中items属性用来指定要遍历的集合,一般使用el表达式来设置items属性的值;

var属性用来指定一个绑定名,绑定的范围是pageContext;
绑定值是集合中的一个对象;

forEach标签每次从集合中取出一个对象,然后绑定到pageContext;

varStatus属性用来指定一个绑定名,绑定范围仍然是pageContext,绑定值是一个由容器创建的对象,
该对象封装了一个当前迭代的状态:
比如,可以通过该对象提供的getIndex方法获得当前迭代对象的下标,或者getCount方法获得当前是第几次迭代;;


例子:jsp文件中不出现java代码,而是用jsp标签来实现显示员工列表;

servlet:

......
	List<Employee> employees = new ArrayList<Employee>();
	......
	request.setAttribute("employees",employees);
	request.getRequestDispatcher("a2.jsp").firward(request,response);
......



a2.jsp:
......
<head>
	<style>
		row1{
			background-color:#fff8dc;
		}
		row2{
			background-color:yellow;
		}
	<style>
</head>
<table cellpadding="0" cellspacing="0" width="60%" border="1">
	<caption>员工列表<caption>
	<thead>
		<tr>
			<td>序号(index)</td>
			<td>姓名</td>
			<td>薪水</td>
			<td>第几次迭代(count)</td>
		</tr>
	</thead>
	<tbody>
		<c:forEach var="e" items="${requestScope.employees}" varStatus="s">
			<tr class="row${s.index%2+1}">
				<rd>${s.index}</td>
				<td>${e.name}</td>
				<td>${e.salary}</td>
				<td>${s.count}</td>
			</tr>
		</c:forEach>
	</tbody>
</table>



自定义JSP标签;

step1,写一个java类,继承SimpleTagSupport类;
step2,在doTag方法里编写处理逻辑;
step3,在.tld中描述标签;

<body-content>empty</body-content> //jsp标签没有标签体;只能:<c1:hello info="" qty""/>这样使用;
<body-content>scriptless</body-content> //jsp标签可以有标签体但是标签体的内容不能出现java代码; 也就是不能出现:<%%><%= %><%! %>这三种形式的代码;
<body-content>JSP</body-content> //有标签体,标签体内容可以是java代码;只有复杂标签技术才支持这个值;(SimpleTagSupport属于简单标签技术,而复杂标签技术已经被其取代)

例子:用标签来代替java代码;

a3.jsp;

<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>
<%@taglib uri="http://www.tarena.com.cn/mytag" prefix="c1" %>
<html>
	<head></head>
	<body style="font-size:30px;font-style:italic;">
		<%
			for(int i=0;i<100;i++){
				out.println("hello world <br/>");
			}
		%>
		
		<c1:hello info="hello world" qty="100"/>
		
	</body>
</html>


package tag;

import javax.servlet.jsp.tagext.SimpleTagSupport;
import javax.servlet.jsp.JspException;

public class HelloTag extends SimpleTagSupport{
//这里可以鼠标右键点击SimpleTagSupport选择source->override->doTag();
//此方法覆盖了SimpleTagSupport中的doTag方法,将处理逻辑放在这里;
 
@Override

public void doTag() throws JspException,IOException{

//如果JSP标签有属性,则属性名称必须与标签类的属性名称一致,而且类型要匹配;
//且标签类的属性必须有对应的set()方法;
	private String info;
	private int qty;
	
	set()方法...
	
//使用SimpleTagSupport类提供的getJspContext方法获得PageContext,PageContext提供了方法
//来找到其他八个隐含对象;

	PageContext ctx = (PageContext)getJspContext();
	JspWriter out = ctx.getOut();
	for(int i=0;i<qty;i++){
		out.println(info+"<br/>");
	}
}
}


mytag.tld文件;

在工程下的WEB-INF文件夹中创建一个mytag.tld文件;然后参考标准标签库中c.tld的格式;

<?xml version="1.0" encoding="UTF-8"?>

<taglib...>

<description>JSTL 1.1 core library</description> //描述可省略;
<display-name>...</display-name> //描述可省略;
<tlib-version>1.1</tlib-version> //版本,由公司的规范决定;
<short-name>c1</short-name>
<uri>http://www.tarena.com.cn/mytag</uri>

<tag>
	<descreption>
		......描述,可省略;
	</descreption>
	<name>hello</name>
	<tag-class>tag.HelloTag</tag-class>
	<body-content>empty</body-content>
	<attribute>
		<name>info</name>
		<required>true</required> //必须出现并被赋值的属性;
		<rtexprvalue>false</rtexprvalue> //不能使用el表达式来赋值;
	</attribute>
	<attribute>
		<name>qty</name>
		<required>true</required> //必须出现并被赋值的属性;
		<rtexprvalue>true</rtexprvalue> //能使用el表达式来赋值;
	</attribute>
	
</tag>

</taglib>


练习:开发一个date标签,按照指定的格式输出当前的系统日期;

<%
	Date date = new Date();
	SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
%>
<%=sdf.format(date)%>

<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>
<%@taglib uri="http://www.tarena.com.cn/mytag" prefix="c2" %>
......
<c2:date pattern="yyyy-MM-dd"/>
......


package tag;

import javax.servlet.jsp.tagext.SimpleTagSupport;
import javax.servlet.jsp.JspException;

public class DateTag extends SimpleTagSupport{

@Override

public void doTag() throws JspException,IOException{

	private String pattern;
	
	set()方法...
	
	PageContext ctx = (PageContext)getJspContext();
	JspWriter out = ctx.getOut();
	Date date = new Date();
	SimpleDateFormat sdf = new SimpleDateFormat(pattern);
	out.println(sdf.format(date));
}
}


mytag.tld文件;

<?xml version="1.0" encoding="UTF-8"?>

<taglib...>

<description>JSTL 1.1 core library</description> //描述可省略;
<display-name>...</display-name> //描述可省略;
<tlib-version>1.1</tlib-version> //版本,由公司的规范决定;
<short-name>c2</short-name>
<uri>http://www.tarena.com.cn/mytag</uri>

<tag>
	<descreption>
		......描述,可省略;
	</descreption>
	<name>date</name>
	<tag-class>tag.DateTag</tag-class>
	<body-content>empty</body-content>
	<attribute>
		<name>pattern</name>
		<required>true</required> //必须出现并被赋值的属性;
		<rtexprvalue>false</rtexprvalue> //不能使用el表达式来赋值;
	</attribute>
	
</tag>

</taglib>


分页;
(1)需求原因;

a,不分页的话,在页面上同时显示很多记录时,用户使用起来不方便(需要来回滚动);

b,如果数据库中记录数很大(比如几万条,甚至几百万条记录),不分页的话,一次性查询
(select * from t_user),查询的记录转化成List<User>会占用web服务器大量的内存
空间,甚至会影响web应用的运行,所以不能一次性查询所有记录,而是要依据查询条件(每页多少
条记录,第几页)查找少量的记录返回给web服务器;


(2)如何分页;

1)什么是真分页;
依据查询条件(每页多少条记录,第几页)查找少量的记录返回给web服务器;

2)sql语句;

mysql:
	select * from t_emp limit ?,?
	第一个问号:是记录的下标(从0开始);
	rowsPerPage*(pages-1);
	第二个问号:是每页多少条记录;
	rowsPerPage;
	
每页的记录数:rowsPerPage;
第几页:pages;

如:
select * from t_emp limit 0,5;
select * from t_emp limit 5,5;


例子:分页显示;

在EmployeeDAO中添加方法;

public List<Employee> findByPages(int rowsPerPage,int pages){
	List<Employee> employees = new ArrayList<Employee>();
	PreparedStatement prep = null;
	ResultSet rst = nul;
	
	try{
		Connection conn = DBUtil.getConnection();
		prep=conn.prepareStatement("select * from t_emp limit ?,?");
		prep.setInt(1,rows.PerPage*(pages-1));
		prep.setInt(2,rowsPerPage);
		rst = prep.executeQuery();
		while(rst.next()){
			Employee e = new Employee();
			e.setId(rst.getInt("id"));
			e.setEname(rst.getString("ename"));
			......
			employees.add(e);
		}
	}catch(SQLException e){
		e.printStackTrace();
		
	}finally{
		if(rst!=null){
			rst.close();
		}
		if(prep!=null){
			prep.close();
		}
		DBUtil.close();
	}
}

//获得总页数;
public int getTotalPages(int rowsPerPages){
	int totalPages = 0;
	PreparedStatement prep = null;
	ResultSet rst = nul;
	
	try{
		Connection conn = DBUtil.getConnection();
		prep=conn.prepareStatement("select count(*) from t_emp");
		rst = prep.executeQuery();
		int totalRows = 0;
		if(rst.next()){
			//总的记录数;
			totalRows = rst.getInt(1);
		}
		if(totalRows%rowsPerPages==0){
			totalPages = totalRows/rowsPerPage;
		}else{
			totalPages = totalRows/rowsPerPage+1;
		}
}catch(SQLException e){
		e.printStackTrace();
	}finally{
		if(rst!=null){
			rst.close();
		}
		if(prep!=null){
			prep.close();
		}
		DBUtil.close();
	}
	return totalPages;
}


在显示员工列表的页面emplist.jsp添加:

......
</table>

<h2>
	<c:choose>
		<c:when test="${pages>1}">
			<a href="list.do?pages=${pages-1}">上一页</a>
		</c:when>
		<c:otherwise>
			已经是第一页了
		</c:otherwise>
	</c:choose>

	第${pages}页
	<c:choose>
		<c:when test="${pages<totalPages}">
			<a href="list.do?pages=${pages-1}">上一页</a>
		</c:when>
		<c:otherwise>
			已经是最后一页了
		</c:otherwise>
	</c:choose>
	总共${totalPages}页
</h2>


在action.java中修改;

if(action.equals("/list")){
	EmployeeDAO dao = new EmployeeDAO();
	try{
		//读取用户要求获取第几页的数据;
		String pages=request.getParameter("pages");
		if(pages==null||pages.equals("")){
			//如果是第一页,则读不到任何参数值;
			pages="1";
		}
		List<Employee> employess=dao.findByPages(5,Integer.parseInt(pages));
		//取得总页数;
		int totalPages=dao.getTotalPages(5);
		request.setAttribute("totalPages",totalPages);
		request.setAttribute("emplist",employess);
		request.setAttribute("pages",pages);
		RequestDispather rd = request.getRequestDispatcher("emplist.jsp");
		rd.forward(request,response);
		System.out.println("转发完成");
	}
}

如果使用oracle数据库:

oracle:

1-5条记录:

select*from
(select a.*,rownum rn
from
	(select*from t_emp)a
where rownum<6)
where rn>=1;

select*from
(select a.*,rownum rn
from
	(select*from t_emp)a
where rownum<?)
where rn>=?;

第一个问号(end):start+rowsPerPage;
第二个问号(start):rowsPerPage*(pages-1)+1;


假分页:
一次性将表中的所有记录全部查询出,放到内存里面(List<Employee>),然后依据用户
参数(第几页,每页多少条记录),查询集合即可;假分页适用表中记录比较少的情况,优点
是效率高,只需访问一次数据库,将查询到的内容放在内存中,以供之后的查询,缺点是不适合记录多
的情况;


javaee5如何使用el表达式和jstl?

javaee5对应的servlet规范是servlet2.5,tomcat6.0实现了该规范;
j2ee1.4(javaee5之前的称呼)对应的servelt规范是servelt2.4,tomcat5.5实现了该规范;

如果我们开发的时候,使用的是javaee5.0建议使用符合servlet2.5的容器,比如使用tomcat6.0;

如果使用jstl,不再需要将standard.jar和jstl.jar拷贝到WEB.INF\lib中了,因为javaee5.0已经将jstl
对应的jar文件合并到其中了;




SERVLET/JSP中编码问题总结:

1、<%@ page pageEncoding="UTF-8" %>的作用是设置JSP编译成Servlet时使用的编码;
它会根据pageEncoding的设定读取jsp,结果是由指定的编码方案翻译成统一的UTF-8 JAVA源码（即.java）,如果pageEncoding设定错了,或没有设定,出来的就是中文乱码;
 
2、<%@ page contentType="text/html;charset=utf-8" %>,
response.setContentType("text/html;charset=UTF-8")
是指服务器发送给客户端时的内容编码,同时指定了浏览器显示的编码;
<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">也有类似功能;

3、request.setCharacterEncoding("UTF-8")是设置从request中取得的值的编码;
指定后可以通过getParameter()则直接获得正确的字符串,如果不指定,则默认使用iso8859-1编码;值得注意的是在执行setCharacterEncoding()之前,不能执行任何getParameter();而且,该指定只对POST方法有效,因为通过POST请求接收到的数据包将跳过servlet容器(tomcat)直接由服务器来接收,所以对GET方法无效,因为GET请求在向servlet容器(tomcat)发送请求时已经决定了其包含在地址中参数的解码方式,是由服务器自身的配置决定的;

4、response.setCharacterEncoding("UTF-8")设置HTTP 响应的编码,如果之前使用response.setContentType设置了编码格式,则使用response.setCharacterEncoding指定的编码格式覆盖之前的设置,因为对于发送数据,服务器按照response.setCharacterEncoding—contentType—pageEncoding的优先顺序,对要发送的数据进行编码;



关于编码（参考SongUnicodeAndAll.java) ;




*/





