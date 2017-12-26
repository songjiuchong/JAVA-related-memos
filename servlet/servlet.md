servlet


package prototype;

import java.io.PrintWriter;
import java.net.URLDecoder;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;


public class Servlet {

	public static void main(String[] args) {

	}
}

/*
Servlet

B/S
-browser/web server;

browser中的页面由开发者编写,其通信层是已经编写好的;
HTTP超文本传输协议,由w3c提供;
由于浏览器和web服务器的开发商是不同的,所以规定浏览器/web服务器中的通讯层的数据包的发送和接收都将遵守HTTP协议;

web server中的业务模块将由servlet来编写;

(1)三层的client/server

   a.基本结构:
   
             数据库:负责数据的存储与检索;
             应用服务器:处理所有的业务逻辑;(一般使用java语言来开发)
             客户端:提供操作的界面,并且显示处理的结果;
             
   b.优缺点:
   
             优点:移植性好,比如换数据库,因为业务逻辑都写在应用服务器上,所以影响不大;
             换应用服务器所在的平台,不管是换操作系统还是硬件,只要 有JVM,应用服务器都能运行;

	   缺点:客户端需要单独安装和维护;另外,开发相对复杂,因为需要自定义协议,开发相应的通讯层;
	   
(2)brower/server;
   a.基本结构:
   
             数据库:负责数据的存储与检索;
	 web服务器:处理所有的业务逻辑;
	  浏览器:提供操作的界面,并且显示处理的结果;
	  
   b.优缺点:
   
             优点:客户端不再需要单独安装了,开发相对简单一些,因为不再需要开发自定义协议,通信层已经开发好了;
	 web服务器和浏览器都已经提供了相应的通信层;


servlet;
sun公司制定的一种用来扩展web服务器功能的组件规范;
(1)用来扩展web服务器功能;

CGI: common gateway interface; 公共网关接口规范;
早期的web服务器(比如,apache)不能处理动态资源(根据请求的参数进行计算然后生成页面)的请求;
要让web服务器能够处理动态资源的请求就必须扩展其功能;早期使用的是CGI程序来扩展的;但是其
开发相对复杂,可移植性不好,性能也不佳,所以应用不广泛;


(2)组件规范;

sun发起了一个名为JCP的组织,其成员包括IBM,HP,Oracle等大型公司,其目的是来审核与制度一些新的java规范,
被通过的JSR,(java specification request)就会成为新的规范;

a.组件;
符合一定规范,实现部分功能并且可以部署到相应的容器里运行的软件模块;
servlet就是一个组件,必须部署到servlet容器中才能运行;

Tomcat是目前最优的servlet容器;
servlet容器为servlet提供一些基础服务(网络调用,也就是说容器中已经包含了符合协议的通信层);

b.容器;
符合某个规范,为组件提供一些基础服务的程序;比如,servlet容器会为servlet提供网络相关的基础服务,
开发人员在使用servlet时,不需要编写网络相关的代码;



关于tomcat:
之所以称tomcat为容器,是因为tomcat是以servlet组件为引擎来运行的,通俗地讲,一切编码的执行还是由jvm来执行的,tomcat将servlet/jsp等
文件放入了它的内部,根据通信层所接收到接收到的请求来选择性地让JVM来调用这些文件并获得返回内容生成html页面后来实现与浏览器的信息交互;
可以把tomcat想象为一个myeclipse的特殊的workspace,jvm会从这个workspace中来寻找需要被执行的字节码文件,不同的是浏览器的请求直接决定了
这个特殊的workspace什么时候该执行哪些文件,什么时候将运行结果反馈给浏览器;相当于一个与myeclipse相兼容,嵌入式的携带了能与浏览器自由交
互的通信层的jvm临时指挥者;(原本是由myeclipse可视化界面来向jvm发送字节码文件的执行请求,一旦开启了tomcat服务器,就相当于让tomcat来充当
向jvm直接发送命令的角色);


tomcat内部运行原理:

1,Service是这样一个集合：它由一个或者多个Connector组成，以及一个Engine，负责处理所有Connector所获得的客户请求;

2,一个Connector将在某个指定端口上侦听客户请求,并将获得的请求交给Engine来处理,从Engine处获得回应并返回客户端;

3.tomcat有两个典型的Connector:
一个直接侦听来自browser的http请求,一个侦听来自其它WebServer的请求;
Coyote Http/1.1 Connector在端口8080处侦听来自客户browser的http请求;
Coyote JK2 Connector 在端口8009处侦听来自其它WebServer(Apache)的servlet/jsp代理请求;

4,Engine下可以配置多个虚拟主机Virtual Host,每个虚拟主机都有一个域名;
当Engine获得一个请求时，它把该请求匹配到某个Host上,然后把该请求交给该Host来处理;
Engine有一个默认虚拟主机,当请求无法匹配到任何一个Host上的时候,将交给该默认Host来处理;

5,Virtual Host,虚拟主机,每个虚拟主机和某个网络域名Domain Name相匹配;
每个虚拟主机下都可以部署(deploy)一个或者多个WebApp,每个WebApp对应于一个Context,有一个Context path(就是这个Web项目的根目录);
当Host获得一个请求时,将把该请求匹配到某个Context上,然后把该请求交给该Context来处理;

6,一个Context对应于一个Web Application,一个Web Application由一个或者多个Servlet组成;
Context在创建的时候将根据配置文件$CATALINA_HOME/conf/web.xml和$WEBAPP_HOME/WEB-INF/web.xml载入Servlet类;
当Context获得请求时,将在自己的映射表(mapping table)中寻找相匹配的Servlet类;
如果找到,则执行该类,获得请求的回应,并返回;

7,配置文件$CATALINA_HOME/conf/server.xml,该文件描述了如何启动Tomcat Server;

8,当一个Web App被初始化的时候,它将用自己的ClassLoader对象加载配置文件web.xml中定义的每个servlet类;

9,web.xml文件有两部分:
servlet类定义和servlet映射定义;
每个被载入的servlet类都有一个名字,且被填入该Context的映射表(mapping table)中,和某种URL PATTERN对应;
当该Context获得请求时,将查询mapping table,找到被请求的servlet,并执行以获得请求回应;


tomcat Server处理一个http请求的过程:

假设来自客户的请求为: "http://localhost:8080/wsota/wsota_index.jsp"

1)请求被发送到本机端口8080,被在那里侦听的Coyote HTTP/1.1 Connector获得
2)Connector把该请求交给它所在的Service的Engine来处理,并等待来自Engine的回应;
3)Engine获得请求localhost/wsota/wsota_index.jsp,匹配它所拥有的所有虚拟主机Host;
4)Engine匹配到名为localhost的Host;
5)localhost Host获得请求/wsota/wsota_index.jsp,匹配它所拥有的所有Context;
6)Host匹配到路径为/wsota的Context;
7)path="/wsota"的Context获得请求/wsota_index.jsp,在它的mapping table中寻找对应的servlet;
8)Context匹配到URL PATTERN为*.jsp的servlet,对应于JspServlet类;
9)构造HttpServletRequest对象和HttpServletResponse对象,作为参数调用JspServlet的doGet或doPost方法;
10)Context把执行完了之后的HttpServletResponse对象返回给Host;
11)Host把HttpServletResponse对象返回给Engine;
12)Engine把HttpServletResponse对象返回给Connector;
13)Connector把HttpServletResponse对象返回给客户端browser;



servlet的开发;

step1:写一个java类,实现servlet接口,或者继承HttpServlet抽象类;

step2:编译(生成字节码文件);

step3:打包成一个组件;
-建立一个文件夹:
-appname(自定义名称)
   -WEB-INF
     -classes(字节码文件)
     -lib(可选,jar文件)
     -web.xml(配置,部署描述文件)

注意:appname可以自定义名称,其它必须按照结构来写;

step4:部署;
将step3生成的整个文件夹拷贝到servlet容器指定的某个文件下面(比如,tomcat一般拷贝到webapps文件夹下);
或者,也可以将step3生成的整个文件夹使用jar命令压缩,生成.war文件,然后再拷贝;

step5:启动servlet容器,访问servlet;
http://ip:port/appname/servlet-url;

ip:servlet容器所在机器的ip地址;

servlet-url: web.xml文件当中的配置;


安装tomcat(是一个开源的servlet容器);

step1,将tomcat解压到/home/soft01下;

step2,配置环境变量; 
-JAVA_HOME:jdk的安装路径;
-PATH:加上tomcat的路径/bin;
-CATALINA_HOME:(tomcat的安装路径)

step3,启动tomcat;

cd/home/soft01/apache-tomcat/bin  (不一定是这个路径,只是个例子)
sh startup.sh(或者sh catalina.sh run)

然后打开浏览器,地址栏输入:
http://localhost:8080;

关闭tomcat,使用sh shutdown.sh;

控制台:

cd/...进入存放tomcat组件的路径;
C:\test\work\javac -cp servlet-api.jar -d. HelloServlet.java //-cp(-classpath)表示如果在编译时有找不到的api则在-cp后指定的api中寻找;
-d 表示将编译后的class文件放在-d后的指定路径中,此处为.也就是当前路径,work文件夹中;

文件夹的构造和其中web.xml的内容等参考underline review中的1224中的work文件夹;
在创建好了servlet组件文件夹后将其放入tomcat文件夹中的webapps文件夹;

以上是不用开发工具的原始实现方法;


/////////////////////////////////////////////////////////////////////////////////////////////////////////////

使用myeclipse开发一个servlet;

step1,让myeclipse管理tomcat;
run/stop/restart MyEclipse Servers按钮,中configure server;下拉框中都是servlet容器,然后选tomcat,以及其对应的版本;
来到右边的目录输入,home directory : tomcat的主目录;

在Tomcat选项下选中JDK,这个应该已经是配置好的;launch中选run mode,然后apply;

配置好后的run/stop/restart MyEclipse Servers按钮启动中有相应的tomcat选项然后run;

在浏览器中打开页面:http://localhost:8080/ 就是tomcat的默认页面;

step2,创建一个web工程;

File-new-Web Project-create new class...;


.java文件:

package web;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class song extends HttpServlet{
	public void service(HttpServletRequest request,HttpServletResponse response)
	throws ServletException,IOException{
		//设置一个消息头,"content-type",告诉浏览器返回的数据类型;
		response.setContentType("text/html");
		//通过响应对象(response)获得输出流;
		PrintWriter out = response.getWriter();
		//输出;
		out.println("<div style='color:red;"+"font-size:60px;font-style:italic;'>HelloWorld</div>");
		out.close();
	}
}


编写好java文件后,需要编辑xml文件;


.xml文件:

<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" 
	xmlns="http://java.sun.com/xml/ns/javaee" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
  <servlet>
  	<servlet-name>song</servlet-name>
  	<servlet-class>web.song</servlet-class> 	
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>song</servlet-name>
  	<url-pattern>/h</url-pattern>
  </servlet-mapping>
</web-app>


然后在工具栏找到deploy按钮选择工程名;

add一个tomcat服务器;deploy type如果是development mode则是不压缩的开发模式,production mode生成的是.war格式的压缩文件,生产模式; 

此时在tomcat文件夹中的webapps文件夹里就有了songweb文件夹,其中内容与之前提到的标准文件部署格式相同;

相应地,在浏览器中的地址:http://localhost:8080/song/hello


servlet是如何运行的;

比如,在浏览器地址栏输入: 
http://ip:port/web01/hello

step1,浏览器依据ip:port建立与web服务器之间的连接(注意:tomcat容器同时也是一个简单的web服务器)

step2,浏览器将需要发送给web服务器的数据打包(按照http协议打包),然后发送;

step3,web服务器解析请求数据包(即拆包),将解析之后得到的数据封装到request对象上,
同时,还会创建response对象;

step4,web服务器依据请求资源路径("/songweb/hello")找到servlet的配置,然后创建servlet对象;

step5,web服务器调用servlet的service方法,在调用该方法时,会将事先创建好的request,response对象作为参数传递给service方法;

step6,在service方法里,可以通过request对象获得请求参数,并进行相应处理,然后将处理结果写到response对象里;

step7,web服务器会从response对象中取出处理结果,打包(即响应数据包),然后发送给服务器;

step8,浏览器拆包(即解析相应数据包)取出数据,生成相应的效果;


应用名的选取;
在创建web project时,默认情况下工程名就会成为tomcat中webapps文件夹中的应用名,想要重新定义应用名可以在
创建web project的窗口中的 Context root URL中在/后将名字修改即可;


常见错误及解决方式;
(1)404; 
404是一个状态码(是一个三位数字,由w3c定义的,表示web服务器的一种状态,即web服务器在处理
客户端的请求时是否正常),404表示根据请求地址,找不到对应的请求资源;
解决方式:
a1,请求地址写错,仔细检查请求地址;
(http://ip:port/appname/servlet-url);
a2,web.xml文件当中,检查servlet-name是否写错;

(2)500;
a1,程序运行时发生了异常,仔细检查java源代码;
a2,web.xml文件中的servlet-class写错,需要按照实际的包名和类名来写;
a3,类没有继承HttpServlet;

(3)405;
a1,类中servlet的service方法覆写格式不正确,要按照public void service(...)标准来写;


http协议;
(1)http协议;
超文本传输协议,由w3c制定的一种应用层协议,规定了浏览器和web服务器之间如何通讯,以及通讯时所
使用的数据格式;

a,通讯原理;
step1,浏览器建立于web服务器之间的连接;
step2,浏览器将请求数据打包,发送请求数据包给web服务器;
step3,web服务器将处理结果打包,发送响应数据包给浏览器;
step4,web服务器关闭连接;

b,特点;
一次请求,一次连接;

c,优点;
效率高,即web服务器可以利用有限的连接为尽可能多的客户端(浏览器)服务;


(2)请求数据包和响应数据包;

get请求与post请求;
1)哪一些情况下,浏览器会发送get请求;
a,浏览器地址栏直接输入某个地址;
b,点击链接;
c,表单默认提交方式; <form>

2)那些情况下,浏览器会发送post请求;
a,设置表单的method属性等于"post"时; <form method="post">

3)get请求的特点:
a,会将请求参数条件到请求资源路径的?后面;因为请求行能够添加的数据很少,所以get方法只
适合少量的数据;
b,get方式会将请求参数暴露在浏览器地址栏,不安全(路由器会记录url);

4)post请求的特点:
a,会将请求参数加到实体内容中;能够提交的数据很多;
b,post方式不会讲请求参数显示在浏览器地址栏,相对安全;

如果有敏感数据需要提交给服务器,一般使用https协议(该协议会对请求数据加密);

例:
在web工程的WEB-INF文件夹下建立html文件:
<html>
	<head>
		<title>song</title>
		<!--模拟一个消息头(content-type),告诉浏览器数据类型和编码-->
		<meta http-equiv="content-type" content="text/html; charset=UTF-8">
	</head>
	<body style="font-zise:30px;font-style:italic;">
		<form action="hello" method="post">
			username:<input name="username"/>
			<input type="submit" value="Confirm"/>
		</form>
	</body>
</html>

此时在webapps文件夹中多了这个html文件;

此时的访问地址:http://ip:port/appname/servlet-url_htmlname.html/


GET /web02/hello?username=tom HTTP/1.1
POST /web02/hello HTTP/1.1

1)请求数据包;
a,请求行: 请求方式,请求资源路径,协议类型和版本;
b,若干消息头:消息头是一些key,value键值对,一般有w3c来制定,浏览器与web服务器之间可以通过发送一些
消息头来传递特定的信息,比如,浏览器可以通过"user-agent"消息头,告诉web服务器浏览器的类型和版本;
c,实体内容:只有当发送post请求时,浏览器才会将请求参数添加到试题内容中,如果是get请求,浏览器会将
请求参数添加到请求资源路径的?后面;

HTTP/1.1 200 OK

2)响应数据包;
a,状态行: 协议类型/版本号 "200"状态码 状态码的简单描述;
状态码:是一个三位数字,由w3c定义,由web服务器发送给浏览器,告诉浏览器web服务器是否正确的处理了请求;
200:正确;
404:依据请求地址找不到对应的资源;
500:程序运行出错;
...

b,若干消息头: web服务器也可以发送一些消息头给浏览器,比如"content-type",告诉浏览器,服务器
返回的数据类型;

c,实体内容:程序处理的结果;如:<div style='color:red;font-size:60px;font-style:italic;'>Hello tom</div>


http://ip:port/web02/hello?username=tom

public class song extends HttpServlet{
	public void service(HttpServletRequest request,HttpServletResponse response)
	throws ServletException,IOException{
		//依据参数名获得参数值;
		String username = request.getParameter("username");
		//设置一个消息头,"content-type",告诉浏览器返回的数据类型;
		response.setContentType("text/html");
		//通过响应对象(response)获得输出流;
		PrintWriter out = response.getWriter();
		//输出;
		out.println("<div style='color:red;font-size:60px;font-style:italic;'>Hello"+username+"</div>");
		out.close();
	}
}


如何获得请求参数值;
1)String request.getParameter(String 参数名);
注意:
1,参数名写错,获得Null;
2,如果参数名对应的值没有赋值,获得"";
比如 ?username=;

2)String[] request.getParameterValues(String 参数名);
注意:
1,当有多个参数名相同的时候,用这个方法;
比如:hello?city=bj&city=sh&city=wh

<html>
	<head>
		<title>song</title>
		<!--模拟一个消息头(content-type),告诉浏览器数据类型和编码-->
		<meta http-equiv="content-type" content="text/html; charset=UTF-8">
	</head>
	<body style="font-zise:30px;font-style:italic;">
		<form action="hello" method="post">
			username:<input name="username"/>
			city:beijing
			<input type="checkbox" name="city" value="bj" checked="checked";/>
			shanghai
			<input type="checkbox" name="city" value="sh"/>
			wuhan
			<input type="checkbox" name="city" value="wh"/>
			<br/>
			<input type="submit" value="Confierm"/>
		</form>
	</body>
</html>

public class song extends HttpServlet{
	public void service(HttpServletRequest request,HttpServletResponse response)
	throws ServletException,IOException{
		//依据参数名获得参数值;
		String username = request.getParameter("username");
		String[] city = request.getParameterValues("city");
		//设置一个消息头,"content-type",告诉浏览器返回的数据类型以及编码格式,如果不设置则会按照
		  ISO-8859-1的方式来编码;
		      通过response获得的流(out)会使用指定的编码格式进行编码;
		response.setContentType("text/html;charset=utf-8");
		//通过响应对象(response)获得输出流;
		PrintWriter out = response.getWriter();
		//输出;
		out.println("<div style='color:red;font-size:60px;font-style:italic;'>Hello"+username+"</div>");
		if(city == null){
			out.println("<h1>no city selected</h1>")
		}
		for(int i=0;i<city.length;i++){
			out.println("<h1>"+city[i]+"</h1>")
		}
		out.close();
	}
}


如何处理表单中的中文参数值;
1)乱码问题产生的原因;
 a.表单提交时,浏览器会对表单中的参数值按照打开该表单所在的页面的编码格式来编码;
 比如:表单所在的页面打开时按照"utf-8"编码格式,则表单提交时,对表单中的参数值按照
 "utf-8"去编码;
 b.web服务器在默认的情况下,会使用"iso-8859-1"去解码;
   由于编码和解码使用的编码格式不一致,所以会产生乱码;

java在内存中采用unicode来保存字符,无论是中文还是外文都是用2个字节来保存一个字符;
编码:将unicode编码格式(内存中)对应的字节数组转换为某种本地编码格式(gbk,utf-8)对应的字节数组;(如:从内存到硬盘);
解码:将某种本地编码格式保存的字节数组转换成unicode编码格式对应的字节数组;

java:
String str = URLEncoder.encode("小月","utf-8");
//str:%E5%B0%8F%E6%9C%88
System.out.println(str);
String str2 = URLDecoder.decode(str,"utf-8");
System.out.println(str2);

encode方法:对字符串使用指定的编码格式进行编码,并将其转换成一个字符串;

decode方法:解码;


如何解决乱码问题;
a.方式一:
  step1,保证表单所在的页面按照指定的编码格式打开;
                     比如,对于html文件,添加<meta http-equiv="content-type" content="text/html;charset=utf-8">
  step2,让web服务器使用指定的编码格式去解码;
        request.setCharacterEncoding("utf-8");
        注意:1.此方法必须放在request.getParameter();之前; 
        2.而且此方法只对method=post请求有效;
        
b.方式二(对method=post/get请求都有效):
  step1,同上;
  
  step2,
  String username = request.getParameter("username");
  username = new String(username.getBytes("iso-8859-1"),"utf-8");


servlet如何输出中文;(从web服务器端servlet输出时避免乱码的出现)
response.setContentType("text/html;charset=utf-8");
该方法的作用:
1)设置一个消息头,"content-type",告诉浏览器返回的数据类型以及编码格式,如果不设置则会按照
ISO-8859-1的方式来解码;

2)通过response获得的流(out)会使用指定的编码格式进行编码;
response.setContentType("text/html;charset=utf-8");


添加TCP/IP monitor;
在myeclipse中的window->show view->other->MyEclipse Common->TCP/IP Monitor;
在TCP/IP Monitor中空白处右键->properties->
Add 本地监听端口:8888或者1234等;
监听对象的设置: 
hostname:localhose/127.0.0.1
port:8080
type:HTTP
Timeout:0

之后再访问需要被监听的链接时就用localhost:8888/1234监听器已经映射到localhost:8080端口了,实现了HTTP协议的监听;

补充:TPC/IP协议是传输层协议,主要解决数据如何在网络中传输,而HTTP是应用层协议,主要解决如何包装数据;关于TCP/IP和HTTP协议的关系,
有一段比较容易理解的介绍:我们在传输数据时,可以只使用(传输层)TCP/IP协议,但是那样的话,如果没有应用层,便无法识别数据内容,如果想要使传输的数据有意义,
则必须使用到应用层协议,应用层协议有很多,比如,HTTP、FTP、TELNET等,也可以自己定义应用层协议;
WEB使用HTTP协议作应用层协议,以封装HTTP文本信息,然后使用TCP/IP做传输层协议将它发到网络上;



例子:添加员工表单,姓名,薪水,年龄都有文本输入框,最后有一个提交按钮,用数据库来管理数据;

html:
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>addEmp</title>
	
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    
    <!--<link rel="stylesheet" type="text/css" href="./styles.css">-->

  </head>
  <body style="font-zise:30px;font-style:italic;">
  	<form action="wt01" method="post">
  		<fieldset>
  		<legend>添加员工</legend>
  		姓名:
  		<input name="name"/>
  		<br/><br/>
  		薪水:
  		<input name="wage"/>
  		<br/><br/>
  		年龄:
  		<input name="age"/>
  		<br/><br/>
  		<input type="submit" value="提交"/>
  		</fieldset>
  	</form>
  </body>
</html>

servlet:

public class AddEmp extends HttpServlet{
	public void service(HttpServletRequest request,HttpServletResponse response)
	throws ServletException,IOException{
		request.setCharacterEncoding("utf-8");
		String name = request.getParameter("name");
		String wage = request.getParameter("wage");
		String age = request.getParameter("age");
		//需要对请求参数值做合法性检查(服务器端验证);浏览器对表单参数的检查时客户端检查(javascript);
		//这里暂时不做验证;
		System.out.println(name);
		System.out.println(wage);
		System.out.println(age);
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
//		out.println("<div style='color:red;font-size:60px;font-style:italic;'>姓名:"+name+" 薪水:"+wage+" 年龄:"+age+"</div>");
//		out.close();
		//使用jdbc访问数据库;
		Connection conn = null;
		try {
			Class.forName("");
				conn = DriverManager.getConnection("","","");
				PreparedStatement prep = conn.prepareStatement("insert into t_emp(ename,salary,age) values(?,?,?)");
				prep.setString(1, name);
				prep.setDouble(2, Double.parseDouble(wage));
				prep.setInt(3, Integer.parseInt(age));
				prep.executeUpdate();
				out.println("<h1>插入成功</h1>");
				out.close();
		}catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			//异常发生后,第一步,记录日志;一般记录在文件上,比如使用一些日志工具(log4j);
			e.printStackTrace(); //也是一种记录日志的方式,不利于保存;
			//判断异常是否能恢复,如果不能通过代码恢复(系统异常,程序运行时所依赖的资源不可用,比如数据库不可用或网络中断等),采用以下措施提醒用户;
			out.println("<h1>系统繁忙,稍后重试</h1>");
			//如果能够恢复(即应用异常)要采取相应措施及时恢复;
		}finally{
			if(conn!=null){
				try {
					//连接对象(conn)关闭,其关联的ResultSet,Statement都会自动关闭,
					//所以,一般情况下,只需要关闭conn就可以;但是,如果使用了数据库连接池,
					//调用了conn的close方法时,连接并没有真正的关闭,此时,需要调用ResultSet,Statement
					//的close方法;
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
		}
	}


mysql:

create table t_emp(
	id int primary key auto_increment,
	ename varchar(50),
	salary double,
	age int
)type=innodb;



Servlet访问数据库;

step1,将jdbc驱动(.jar文件)拷贝到WEB-INF\lib下;
      tomcat提供了自己的classloader类加载器会查找WEB-INF\lib下的驱动文件,所以这里和之前配置JDBC不一样,不需要再配置buildpath;

step2,注意异常的处理;



例子:写一个ListEmpServlet,该servlet查询t_emp表,将所有员工信息查询到之后,以表格的方式输出,并且增加前往添加员工html页面的跳转按钮和添加,删除和编辑员工的功能;

servlet:

public class ListEmpServlet extends HttpServlet{
	public void service(HttpServletRequest request,HttpServletResponse response)throws
	ServletException,IOException{
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		Connection conn = null;
		try {
			Class.forName("");
			conn = DriverManager.getConnection("","","");
			PreparedStatement prep = conn.prepareStatement("select*from t_emp");
			ResultSet rst = prep.executeQuery();
			out.println("<table border='1' width='60%' cellpadding='0' cellspacing='0'>");
			out.println("<caption>员工列表</caption>");
			out.println("<tr><td>ID</td><td>姓名</td><td>薪水</td><td>年龄</td></tr>");
			while(rst.next()){
				int id = rst.getInt("id");
				String ename = rst.getString("ename");
				double salary = rst.getDouble("salary");
				int age = rst.getInt("age");
				out.println("<tr><td>"+id+"</td><td>"+ename+"</td><td>"+salary+"</td><td>"+age+"</td>
				<td><a href='del?id="+id+"'>删除</a>&nbsp;<a href='load?"+id+"'>修改</a></td></tr>");
			}
			out.println("</table>");
			out.println("<a href='addEmp.html'>添加员工</a>");
		}catch(Exception e){
			e.printStackTrace();
			out.println("<h1>系统繁忙,稍后重试</h1>");
		}finally{
			if(conn!=null){
				try {
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
		}
	}
}

......AddEmp......
		//out.println("<h1>插入成功</h1>");
		//out.println("<a href='list'>返回员工列表</a>");
		response.sendRedirect("list"); //重定向; 
......


package web;

import java.io.PrintWriter;
import java.sql.DriverManager;
import java.sql.PreparedStatement;

public class DelEmpServlet extends HttpServlet{
	public void service(HttpServletRequest request,HttpServletResponse response)throws
	ServletException,IOException{
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		int id = Integer.parseInt(request.getParameter("id"));
		Connection conn = null;
		try {
			Class.forName("");
			conn = DriverManager.getConnection("","","");
			PreparedStatement prep = conn.prepareStatement("delete from t_emp where id =?");
			prep.setInt(1,id);
			prep.executeUpdate();
			response.sendRedirect("list");
		}catch(Exception e){
			e.printStackTrace();
			out.println("系统繁忙,稍后再试！");
		}finally{
			if(conn!=null){
				conn.close();
			}
		}
	}
}


public class LoadEmpServlet extends HttpServlet{
	public void service(HttpServletRequest request,HttpServletResponse response)throws
	ServletException,IOException{
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		int id = Integer.parseInt(request.getParameter("id"));
		Connection conn = null;
		try {
			Class.forName("");
			conn = DriverManager.getConnection("","","");
			PreparedStatement prep = conn.prepareStatement("select * from t_emp where id=?");
			prep.setInt(1,id);
			ResultSet rst = prep.executeQuery();
		    if(rst.next()){
				String ename = rst.getString("ename");
				double salary = rst.getDouble("salary");
				int age = rst.getInt("age");
				out.println("<form action='modify' method='post'>");
				out.println("id:"+id+"</br>");
				out.println("<input type='hidden' name='id' value='"+id+"'>");
				out.println("姓名：<input name='ename' value='"+ename+"'>");
				out.println("薪水：<input name='salary' value='"+salary+"'>");
				out.println("年龄：<input name='age' value='"+age+"'>");
				out.println("<input type='submit' value='提交'>");
				out.println("</form>");
		    }
		}catch(Exception e){
			e.printStackTrace();
			out.println("系统繁忙,稍后再试！");
		}finally{
			if(conn!=null){
				conn.close();
			}
		}
	}
}


public class ModifyEmpServlet extends HttpServlet{
	public void service(HttpServletRequest request,HttpServletResponse response)throws
	ServletException,IOException{
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		int id = Integer.parseInt(request.getParament("id"));
		String ename = request.getParameter("ename");
		double salary = Double.parseDouble(request.getParameter("salary"));
		int age = Integer.parseInt(request.getParameter("age"));
		Connection conn = null;
		try {
			Class.forName("");
			conn = DriverManager.getConnection("","","");
			PreparedStatement prep =  conn.prepareStatement("..?.?.?.. where id = ?");
			prep.setInt(4,id);
			prep.setString(1,ename);
			prep.setDouble(2,salary);
			prep.setInt(3,age);
			prep.executeUpdate();
			response.sendRedirect("list");
		}catch(Exception e){
			e.printStackTrace();
			out.println("系统繁忙,稍后再试！");
		}finally{
			if(conn!=null){
				conn.close();
			}
		}
	}
}
			
			
			
重定向:
(1)什么是重定向;
服务器通知浏览器立即向一个新的地址发请求;
即服务器可以发送一个302状态码和一个Location消息头(值为一个地址,成为重定向地址);
浏览器收到之后,会立即向重定向地址发请求;

(2)编程;
response.sendRedirect(String url); //url:重定向地址;

(3)注意两个问题;
a,重定向之前,不能调用out.close(); //一旦调用了out.close()(会先调用out.flush())或者service方法运行到最后,都会将out流中缓存的内容发送给客户端,
与之后的重定向发生冲突,所以在发送了out中的内容后就会报错;
b,重定向之前,servlet容器会先清空response对象上缓存的数据;

(4)特点;
a,重定向的地址是任意的;
b,重定向之后,浏览器地址栏的地址会变成重定向地址;


将中文插入到数据库;
a,数据库要支持中文
  create database jsd1306db default
  character set utf8;
b,要保证jdbc驱动程序支持中文
  jdbc驱动程序在访问数据库时,需要做正确地编码(插入数据)和解码(查询数据),
  有部分jdbc驱动程序不能正确地进行编码和解码(默认使用"iso-8859-1");

解决方式:
换驱动程序;


DAO;

public class ListEmpServlet extends HttpServlet{
	public void service(HttpServletRequest request,HttpServletResponse response)throws
	ServletException,IOException{
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		try {
			EmployeeDAO dao = new EmployeeDAO();
			List<Employee> employees = dao.findAll();
			out.println("<table border='1' width='60%' cellpadding='0' cellspacing='0'>");
			out.println("<caption>员工列表</caption>");
			out.println("<tr><td>ID</td><td>姓名</td><td>薪水</td><td>年龄</td></tr>");
			for(int i=0;i<employees.size();i++){
			Employee e = employees.get(i);
			
			int id = e.getId();
			String ename = e.getEname();
			double salary = e.getSalary();
			int age = e.getAge();
			out.println("<tr><td>"+id+"</td><td>"+ename+"</td><td>"+salary+"</td><td>"+age+"</td><td><a href='del?id="+id+"'>删除</a>&nbsp;<a href='load?"+id+"'>修改</a></td></tr>");
			}
			out.println("</table>");
			out.println("<a href='addEmp.html'>添加员工</a>");
			out.close();
		}catch(Exception e){
			e.printStackTrace();
			out.println("<h1>系统繁忙,稍后重试</h1>");
		}
	}
}

JDBC转化为DAO形式;

package entity;

public class Employee {
	private int id;
	private String ename;
	private double salary;
	private int age;


	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getEname() {
		return ename;
	}

	public void setEname(String ename) {
		this.ename = ename;
	}

	public double getSalary() {
		return salary;
	}

	public void setSalary(double salary) {
		this.salary = salary;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}
	
	public static void main(String[] args) {

	}
}


import entity.Employee;

public class EmployeeDAO {
	public List<Employee> findAll() throws SQLException{
		List<Employee> employees= new ArrayList<Employee>();
		//加入DBUtil包;
	try{	
		Connection conn=DBUtil.getConnection();
		PreparedStatement prep = conn.prepareStatement("select * from t_emp");
		ResultSet rst = prep.executeQuery();
		while(rst.next()){
			Employee e = new Employee();
			e.setId(rst.getInt("id"));
			e.setEname(rst.getString("ename"));
			e.setSalary(rst.getDouble("salary"));
			e.setAge(rst.getInt("age"));
			employees.add(e);
		}
		return employees;
	}catch(SQLException e){
		e.printStackTrace();
		throw e;
	}finally{
		DBUtil.close();
	}
	}
}


servlet容器如何处理请求资源路径;
比如,在浏览器地址栏输入
http://ip:port/web04_2/abc.html
浏览器会将"/web04_2/abc.html"作为请求资源路径发送给容器;
容器首先依据请求资源路径找到应用所在的文件夹(web04_2),然后,
容器会先假设访问的是一个servlet,所以去web.xml文件中和url-pattern匹配:
a,精确匹配:要求url-pattern与路径完全一致,比如,对于上面的请求,要求<url-pattern>/abc.html</url-pattern>；
所以.html结尾的地址未必就是转向一个html页面,也可能是一个url-pattern为.html结尾的一个servlet;

b,通配符匹配:
即使用"*"来匹配0个或多个字符;
比如:
 <url-pattern>/*</url-pattern>
 <url-pattern>/abc/*</url-pattern>
 
c,后缀匹配:
  使用"*."开头,后接任意的多个字符(一个或多个);
比如:
 <url-pattern>*.do</url-pattern>
以上会匹配所有以.do结尾的请求;

d,如果以上都不匹配,容器认为这是一个资源文件(html文件),然后查找该文件,如果找到则返回,
找不到就返回404代码;


//获得请求资源路径;
 String url = request.getRequestURI();
 System.out.println("url:"+url);
 String action = url.substring(url.lastIndexOf("/"),url.lastIndexOf("."));
 System.out.println("action:"+action);
 
 if(action.equals("/list)){
 //开始处理员工列表的请求;
 }else if(action.equals("/add")){
 //插入员工;
 }


如何让一个servlet处理多个请求?

step1,使用后缀匹配模式,比如:
 <url-pattern>*.do</url-pattern>
step2,在servlet的service方法里,获得请求资源路径,通过分析路径然后调用不同的分支来处理;
利用: String uri = request.getRequestURI();


servlet的生命周期;

(1)什么是servlet的生命周期;
servlet容器如何创建servlet对象,如何初始化,如何调用servlet对象的方法来处理请求
以及如何销毁servlet对象的整个过程;

(2)生命周期的四个阶段;
1)实例化
 a,实例化的含义:
 容器调用servlet的构造器,创建一个servlet对象;
 b,什么时候实例化;
        情况一:
             容器收到请求后才创建;
        情况二:
             容器启动的时候,事先创建好;   
   load-on-startup参数;
 在web.xml中添加:
 <servlet-name></servlet-name>
 <servlet-class></servlet-class>
 <!--让容器启动时,就创建该servlet实例,值是一个大于等于0的整数,越小优先级越高-->
 <load-on-startup>1</load-on-startup>
   
   
   如:
   public class test extends HttpServlet{
   	public test(){
   	System.out.println("new constructor...");
   	}
	public void service(HttpServletRequest request,HttpServletResponse response)throws
	ServletException,IOException{
	......
	}
   }

 c,servlet容器在默认情况下,只会为某个servlet创建一个实例,不管有多少个请求;
 
2)初始化

  a,初始化的含义:
     容器在创建好servlet对象之后,会立即调用servlet对象的init方法;
     
  b,init(ServletConfig config)方法;
  GenericServlet已经实现了该方法,一般情况下,我们不需要再提供该方法的实现;
  GenericServlet的init方法是这样实现的:
     将容器传递进来的config对象保存下来,并提供了一个getServletConfig()方法;
     如果GenericServlet中带参数的init方法不能满足我们的要求,可以利用其中调用了一个无参的init重载方法这样的
     人性化的机制,在继承了HttpServlet抽象类的类中覆盖此方法,以达到扩展功能的目的;
  
  c,init方法只会执行一次;
  
  d,ServletConfig对象提供了一个方法:
  String getInitParameter(String paramname);
     用于访问servlet的初始化参数;
     如:web.xml中添加:
    <servlet-class>web.SomeServlet</servlet-class>
    <!--配置初始化参数-->
    <init-param>
   		<param-name>company</param-name>
   		<param-value>tarena</param-value>
    </init-param>
    <init-param>
   		<param-name>address</param-name>
   		<param-value>china</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  ......
    然后在service方法中:
    String company = config.getInitParameter("company");
    System.out.println("company:"+company);
    
3)就绪 ;
  a,就绪的含义:
     容器收到请求之后,会调用servlet对象的service方法来处理请求;
    
  b,HttpServlet已经实现了service方法,该方法依据请求方式(get/post)分别去调用
  doGet/doPost方法来处理;
     我们在编写servlet时,可以override HttpServlet的service方法,或者也可以override HttpSerlet的doGet/doPost方法;
  
4)销毁;
  a,销毁的含义:
     容器依据自身的算法,在销毁servlet对象之前调用次对象的destroy方法;
     
  b,destroy方法只会执行一次;
  
  c,GenericServlet已经实现了此方法;

     覆写此方法,如:
     
  @Override
  public void destroy(){
  System.out.println("SomeServlet's been destroyed...");
  }
  
  然后在MyEclipse中的deploy选项中将此web应用remove,相应的servlet对象就会调用此方法;

(3)servlet生命周期相关的几个类与接口;

  1)servlet接口;
    a,init(ServletConfig config) :用于初始化;
    b,service(ServletRequest request,ServletResponse response) :用于处理请求;
    c.destroy() :用于销毁servlet对象;
    
  2)GenericServlet抽象类;
    实现了Servlet接口,主要实现了init,destroy方法;  
    
  3)HttpServlet 抽象类;
    继承了GenericServlet,主要实现了service方法;
  
    在GenericServlet抽象类中被实现的init方法的方法体:
    public void init(ServletConfig config) throws ServletException{
    //将容器创建的config对象的引用保存下来;
    this.config = config;
    //init()方法的作用:用于子类去override;
    this.init();
    }
 
   需要注意的是:在GenericServlet抽象类中有一个不带参数的init()方法:
  public void init() throws ServletException{
  
  }
  
   又由于HttpServlet本身继承了GenericServlet抽象类,在继承了HttpServlet抽象类的类中可以覆盖此方法,以达到扩展功能的目的:
  @Override
  public void init() throws ServletException{
     System.out.println("SomeServlet's init...");
  }
   
   
   此外,在GenericServlet抽象类中还有一个方法:
  
  public ServletConfig getServletConfig()(
  return config;
  }
  
  所以在service方法中就可以使用这个方法,如:
  ServletConfig config = getServletConfig();
  
  4)ServletConfig 接口主要提供了String getInitParameter(String paramname);
     用来获取servlet的初始化参数;
     
  5)ServletRequest 接口是HttpServletRequest的父接口;
  
  6)ServletResponse 接口是HttpServletResponse的父接口;
 
  
 "UML"统一建模语言;
  
  序列图(动态);
  
  类图(静态);
  
  
 练习:
 对一个产品计价; 出售价格=原始价格X(1+城市汇率);

HTML:

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>saleinfol.html</title>
	
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    
    <!--<link rel="stylesheet" type="text/css" href="./styles.css">-->

  </head>
  
  <body style="font-zise:30px;font-style:italic;">
  	<form action="sale" method="post">
  		<h1>产品计价</h1>
  		原始价格:<input name="baseprice"/>
  		</br></br>
  		销售地区:<select name="city"/ style="width
  		;120px;">
  			<option value="bj">北京</option>
  			<option value="sh">上海</option>
  			<option value="wh">武汉</option>
  		</select>
  		</br></br>
  		<input type="submit" value="提交"/>
  	</form>
  </body>
</html>
 

快速开发servlet:
在工程中的src右键选择new->Servlet,然后设置;
  
servlet:

public class Sale extends HttpServlet{
	//保存城市与税率的对应关系;
	private HashMap<String,Double> taxes = new HashMap<String,Double>();
	public void init() throws ServletException{
		ServletConfig config = getServletConfig();
		//taxInfo: bj,0.08;sh,0.09;wh,0.04
		String taxInfo = config.getInitParameter("taxRate");
		String[] tax1 = taxInfo.split(";");
		for(int i = 0;i<tax1.length;i++){
			String tax2 = tax1[i];
			String[] tax3 = tax2.split(",");
			taxes.put(tax3[0],Double.parseDouble(tax3[1]));
		}
	}
	public void service(HttpServletRequest request,HttpServletResponse response)throws
	ServletException,IOException{
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		String city = request.getParameter("city");
		double basePrice = Double.parseDouble(request.getParameter("baseprice"));
		//根据city找到税率;
		double taxRate=taxes.get(city);
		double price = basePrice*(1+taxRate);
		out.println("<h1>出售价格:"+price+"</h1>");
		out.close();
	}
}

XML:
......
	<servlet>
		<servlet-name>Sale</servlet-name>
		<servlet-class>web.Sale</servlet-class>
		<init-param>
		<param-name>taxRate</param-name>
		<param-value>bj,0.08;sh,0.09;wh,0.04</param-value>
		</init-param>
	</servlet>

	<servlet-mapping>
		<servlet-name>Sale</servlet-name>
		<url-pattern>/sale</url-pattern>
	</servlet-mapping>
</web-app>
  
  
 
*/







////////////////////////////////////////////////////////////////////////////////////////////////////////
 

/*


关于编码（参考SongUnicodeAndAll.java) ;

SERVLET/JSP中编码问题总结:

1、<%@ page pageEncoding="UTF-8" %>的作用是设置JSP编译成Servlet时使用的编码;
它会根据pageEncoding的设定读取jsp,结果是由指定的编码方案翻译成统一的UTF-8 JAVA源码（即.java）,如果pageEncoding设定错了,或没有设定,出来的就是中文乱码;
 
2、<%@ page contentType="text/html;charset=utf-8" %>,
response.setContentType("text/html;charset=UTF-8")
是指服务器发送给客户端时的内容编码,同时指定了浏览器显示的编码;
<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">也有类似功能;

3、request.setCharacterEncoding("UTF-8")是设置从request中取得的值的编码;
指定后可以通过getParameter()则直接获得正确的字符串,如果不指定,则默认使用iso8859-1编码;值得注意的是在执行setCharacterEncoding()之前,不能执行任何getParameter();
而且,该指定只对POST方法有效,因为通过POST请求的路径中没有包含请求参数,而servlet容器(tomcat)会对所有的请求资源路径采取相当于URLDecoder.decode(uri,"iso-8859-1");的方法解码;但是POST请求的参数在
HTTP实体包中,tomcat在通过请求资源路径定位了相应的servlet组件后会将请求包中的实体(包含了post请求参数)提取出来提供给servlet组件调用,而这个过程中并没有之前的解码过程,相当于直接把原始的请求参数提供给
相应的servlet组件,由它自己去处理;而GET请求在向servlet容器(tomcat)发送请求时已经决定了其包含在地址中参数的解码方式,是由容器自身的配置决定的;

4、response.setCharacterEncoding("UTF-8")设置HTTP 响应的编码,如果之前使用response.setContentType设置了编码格式,则使用response.setCharacterEncoding指定的编码格式覆盖之前的设置,
因为对于发送数据,服务器按照response.setCharacterEncoding—>contentType—>pageEncoding的优先顺序,对要发送的数据进行编码;



关于获取路径的方法:

假定你的web application名称为news,你在浏览器中输入请求路径： 
http://localhost:8080/news/main/list.jsp 

则执行下面向行代码后打印出如下结果： 
1. System.out.println(request.getContextPath()); //可返回站点的根路径,也就是项目的名字;
打印结果：/news 

2.System.out.println(request.getServletPath()); 
打印结果：/main/list.jsp 

3.System.out.println(request.getRequestURI()); 
打印结果：/news/main/list.jsp 

4.System.out.println(request.getRequestURL()); 
打印结果：http://localhost:8080/news/main/list.jsp

5.System.out.println(request.getRealPath("/")); 
打印结果：F:\Tomcat 6.0\webapps\news\test


补充:

URIs, URLs, URNs;
首先,URI,是uniform resource identifier,统一资源标识符,用来唯一的标识一个资源;
而URL是uniform resource locator,统一资源定位器,它是一种具体的URI,即URL可以用来标识一个资源,而且还指明了如何locate这个资源;
而URN,uniform resource name,统一资源命名,是通过名字来标识资源,比如mailto:java-net@java.sun.com;
也就是说,URI是以一种抽象的,高层次概念定义统一资源标识,而URL和URN则是具体的资源标识的方式;URL和URN都是一种URI;

在Java的URI中,一个URI实例可以代表绝对的,也可以是相对的,只要它符合URI的语法规则;而URL类则不仅符合语义,还包含了定位该资源的信息,因此它不能是相对的;

从HttpServletRequest的javadoc中可以看出,getRequestURI返回一个String:“the part of this request’s URL from the protocol name up to the query string in the first line of the HTTP request”,比如“POST /some/path.html?a=b HTTP/1.1”,则返回的值为”/some/path.html”;
现在可以明白为什么是getRequestURI而不是getRequestURL了,因为此处返回的是相对的路径;
而getRequestURL返回一个StringBuffer:“The returned URL contains a protocol, server name, port number, and server path, but it does not include query string parameters.”,完整的请求资源路径,不包括querystring;


*/


