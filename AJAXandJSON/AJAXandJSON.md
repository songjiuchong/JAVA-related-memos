AJAXandJSON


package prototype;

public class AJAXandJSON {

	public static void main(String[] args) {
	}
}


/*

ajax(asynchronous javascript and xml);
异步的javascript和xml;

(1)ajax是什么;
是一种用来改善用户体验的技术(不通过它也能实现,但是用它更人性化);
本质上讲,是利用浏览器内置的一个特殊的BOM(browser object model)对象(XMLHTTPRequest)异步地(当这个对象
向服务器发请求的时候,浏览器不会销毁当前页面,用户仍然可以对当前页面做其他的操作)向服务器发请求,
服务器送回的也不是一个完整的页面,而是部分的数据,利用这些数据,可以对当前页面做部分的更新,整个过程
页面无刷新,不会打断用户操作;

传统的web应用:
以注册为例,用户填写完表单信息之后,需要点击提交,浏览器会将表单中的数据发送给服务器来处理,
此时,浏览器会销毁当前页面,需要等待服务器返回一个新的页面回来,在此期间,用户什么都不能做,
服务器检查用户名后如果有错误,返回一个完整的页面再让用户重新填写;

BOM:browser object model,如window,location等浏览器对象;
DOM:document object model,如html中的div,table等对象;

(2)ajax对象;
1)如何获得ajax对象,XMLHTTPRequest没有标准化,所以要区分浏览器来获得这个对象;
function getXhr(){
	var xhr = null;
	if(window.XMLHttpRequest){
	//非IE浏览器;
	xhr = new XMLHttpRequest();
	}else{
	xhr = new ActiveXObject('Microsoft.XMLHttp');
	}
	return xhr;
}

测试:
<head>
	<script type="text/javascript">
		function test1(){
			if(window.XMLHttpRequest)
			var test = new XMLHttpRequest();
			alert(typeof test);
		}
	</script>
</head>
<body>
	<form action="" method="" onsubmit="test1();">
		<fieldset>
			<legend>LTGB</legend>
			<input type="submit" value="s"/>
		<fieldset>
	</form>
</body>


例子:建立一个test.jsp来获取一个ajax对象;
<html>
	<head>
		<script type="text/javascript">
		function getXhr(){
			var xhr = null;
			if(window.XMLHttpRequest){
			//非IE浏览器;
			xhr = new XMLHttpRequest();
			}else{
			xhr = new ActiveXObject('Microsoft.XMLHttp');
			}
			return xhr;
		}
		</script>
	</head>
	<body>
		<a href="javascript:" onclick="alert(getXhr());">点击查看ajax对象</a>
	</body>
</html>


封装这个获得ajax对象的函数;
在WebRoot下建立js文件夹,创建一个my.js;右键选择属性选择编码UTF-8(否则页面无法保存中文);

		function getXhr(){
			var xhr = null;
			if(window.XMLHttpRequest){
			//非IE浏览器;
			xhr = new XMLHttpRequest();
			}else{
			//IE浏览器;
			xhr = new ActiveXObject('Microsoft.XMLHttp');
			}
			return xhr;
		}


2)重要属性;

a,onreadystatechange:绑定一个事件处理函数,该函数用来处理ajax对象产生的readystatechange事件;
当ajax对象的readyState属性值发生改变(比如从1变为2),就会产生readystatechange事件;

b,responseText;获得服务器返回的文本;

c,responseXML;获得服务器返回的xml文档;

d,status;获得服务器返回的状态码;

e,readyState; 该属性值有5个,分别是:0,1,2,3,4;表示ajax对象与服务器之间通讯的状态;
如:4表示ajax对象已经获得了服务器返回的所有的数据;


(3)编程步骤;

step1,获得ajax对象;
见上例;

step2,发送请求;

第一种方式:get请求;
	xhr.open('get','check_username.do?username='+$F('username'),true);
	xhr.onreadystatechange=f1; //当readystatechange事件被捕获则调用f1函数;
	xhr.send(null);
	注意:true表示发送异步请求,false是同步请求;
	
同步和异步请求;
	
	其实XMLHttpRequest里异步和同步的含义是指ajax执行时候是否阻塞后续javascript代码的执行如果我们选择同步执行方式,那么ajax执行http请求后,
	其他javascript代码必须等待http执行完毕即请求得到了响应并且响应结果被处理完毕后,其他的javascript代码才能执行,那么由于浏览器的线程停留在
	了JS函数的执行阶段,所以页面会出现"假死"状态,无法被用户继续操作;
	如果我们选择异步执行方式,那么ajax执行http请求后不会阻塞其他javascript代码执行即ajax执行http请求后浏览器会单独起一个线程等到这个http的响应,当前线程马上就可以执行其他javascript代码,
	等独立线程获得了http响应再去插队UI线程执行ajax的回调函数;这也是我前面文章讲到使用ajax方式解决阻塞脚本问题的原理;
	
	例子:
	
	//异步;
	function getJSON2(){  
            var tmp;  
            var xmlHttpRequest=createAjaxObject();  
            xmlHttpRequest.open('GET',url,true);//GET即采用get方法,url为请求的地址,true设置为异步,默认就是异步,所以也可以不用写 ;
            xmlHttpRequest.send(null);  
            xmlHttpRequest.onreadystatechange = function(){  //注册回调函数;  
                //readyState有四种可能值：
                0——请求未初始化(在调用 open() 之前),1——请求已提出(调用 send() 之前),2——请求已发送(这里通常可以从响应得到内容头部),3——请求处理中,4——请求已完成;  
                if(xmlHttpRequest.readyState==4){  
                    if(xmlHttpRequest.status == 200){  
                        alert(xmlHttpRequest.responseText);  
                        //var json = eval('('+xmlHttpRequest.responseText+')');  
                        //alert(json.Stops[0].StopName);  
                       //tmp = json.Stops[0].StopName;  
                       //alert(tmp);//有值;  
                     }  
                }  
            }  
            //alert(tmp); //没有值;  
            xmlHttpRequest=null;  
        }  
	
	//同步;
	function getJSON2(){  
            var tmp;  
            var xmlHttpRequest=createAjaxObject();  
            xmlHttpRequest.open('GET',url,false);
            xmlHttpRequest.send(null);  
            var result = xmlHttpRequest2.status;  
                    if(result == 200){  
                        alert(xmlHttpRequest.responseText);  
                        //var json = eval('('+xmlHttpRequest.responseText+')');  
                        //alert(json.Stops[0].StopName);  
                       //tmp = json.Stops[0].StopName;  
                       //alert(tmp);//有值  
            }  
            //alert(tmp); //有值,成功,但是破坏了ajax异步的初衷;
            xmlHttpRequest=null;  
        }
        
	
还需注意的是:
同步异步使用xmlhttp池时都要注意：取得xmlhttp时只能新建xmlhttp,不能从池中取出已用过的xmlhttp,(这种情况主要发生在需要循环的时候)因为被使用过的xmlhttp的readyState为4,
所以同步异步都会send但不执行onreadystatechange;
	
第二种方式:post请求;
	xhr.open('post','check_username.do',true);
	//添加一个content-type消息头;
	xhr.setRequestHeader('content-type','application/x-www-form-urlencoded');
	xhr.onreadystatechange=f1;
	xhr.send('username='+$F('username'));
	
	
step3,服务器处理请求;一般不再需要返回一个完整的页面,只需要返回部分的数据,比如一个
简单的字符串;

step4,事件处理函数;
事件处理函数获取服务器返回的数据,然后更新当前页面;

function f1(){
	//只有readyState等于4,ajax对象才获得了服务器返回的完整的数据;
	if(xhr.readyState == 4){
	var txt = xhr.responseText;
	//dom操作
	}
}


例子:检查用户名能否使用;
将prototype复制到js文件夹下;

regist.jsp;

	<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>

	<html>
		<head>
			<style>
				.s1{
					color:red;
				}
			</style>
			<script type="text/javascript" src="<%request.getContextPath()%>/js/my.js"></script>
			<script type="text/javascript" src="js/prototype-1.6.0.3.js"></script>
			<script type="text/javascript">
				function check_username(){
					//step1,获得ajax对象;
					var xhr = getXhr();
					//step2,发送请求;
					var url = 'check_username.do?username='+$F('username');
					xhr.open('get',encodeURI(url),true);
					
					//这行代码只是绑定了一个函数给事件处理监听对象,然后就执行之后的send方法和服务器交互了,
					//只有当服务器返回了相应的内容才会触发这里的事件处理函数来接收信息;
					xhr.onreadystatechange=function(){ 
					if(xhr.readyState == 4){
						if(xhr.status==200){
						//服务器正确地返回了处理结果,为发生异常;
							var txt=xhr.responseText;
							$('username_msg').innerHTML=txt;
						}else{
							$('username_msg').innerHTML='检查出错...';
						}
					}
					};
					$('username_msg').innerHTML='正在检查...'; //在服务器还未返回信息时先提示正在检查;
					xhr.send(null);
				}
				
			</script>
		</head>
		<body style="font-size:30px;font-style:italic;">
			<form action="regist.do" method="post">
				<fieldset>
					<legend>注册</legend>
					用户名:<input id="username" name="username" onblur="check_userbname();"/>
					<span class="s1" id="username_msg"><br/>
					真实姓名:<input name="realname"/><br/>
					<input type="submit" value="提交"/>
				</fieldset>
			</form>
		</body>
	</html>


action.java;

		public class ActionServlet extends HttpServlet{
			public void service(HttpServletRequest request,HttpServletResponse response)throws ServletException,IOException{
				String uri = request.getRequestURI();
				String action = uri.substring(uri.lastIndexOf("/"),uri.lastIndexOf("."));
				if(action.equals("/check_username")){
					String username = request.getParameter("username");
					
					//模拟服务器正在进行一个比较耗时的操作;
					try{
						Thread.sleep(6000);
					}catch(InterruptedException e){
						e.printStackTrace();
					}
					//模拟一个系统异常;这个异常抛给容器后,如果没有配置错误处理页面,这个异常将被抛给ajax对象,相当于把错误信息返回给span中的文本内容;
					if(1==2){
						throw new ServletException("系统异常");
					}
					
					if("张三".equals(username)){
						out.println("用户名已经存在");
					}else{
						out.println("可以使用");
					}
				}
				out.close()
			}
		}



ajax编程中的编码问题;

(1)如果发送get请求;

a,发生乱码问题的原因:
IE浏览器内置的ajax对象在发送get请求时会对中文使用gbk进行编码,而其他浏览器会使用utf-8进行编码;
	web服务器默认情况下,对于get请求会使用iso-8859-1去解码;

b,解决方式;

step1,让web服务器使用utf-8去解码;
比如,对于tomcat,可以修改conf/server.xml添加URIEncoding="utf-8",然后重启tomcat;

step2,对请求参数使用encodeURI函数去统一编码;encodeURI函数会使用utf-8去编码;
因为浏览器在发送数据包前会先检查数据中是否存在中文,如果存在会使用其默认编码格式去对
其进行编码后再发送,而这个浏览器内置的函数encodeURI可以使其事先对指定中文使用utf-8去编码,
然后浏览器检查数据包时就不存在中文了,已经是编码后的格式,然后发送给服务器;
	
(2)如果发送post请求;

a,发送乱码的原因:
当浏览器内置的ajax对象发送post请求时会使用utf-8对中文进行编码;
web服务器默认情况下对于post会使用iso-8859-1去解码;

b,解决方式;
request.setCharacterEncoding("utf-8");
	
	
	
json;

Stock对象;
code:600015;
name:山东高速;
price:20.05;

两种方案来向浏览器端发送一个对象;
(1)xml;
xml:
	<stock>
		<code>600015</code>
		<name>山东高速</name>
		<price>20.05</price>
	</stock>
	
通过DOM4J来解析;这种技术目前用的较少,因为xml传输和解析速度较慢;
	
(2)json;
{'code':'600015','name':'山东高速','price':20.05}


json(javascript object notation:js对象声明符号语言)?
什么是json?
一种轻量级的数据交换标准;

a,轻量级;
相对于xml(extensible markup language),json解析的速度更快,并且文档的大小要小一些;

b,数据交换;将数据转换成一种通用的与平台无关的数据格式解析发送;

json基本语法;
a,表示一个对象;
{属性名称:属性值,属性名称:属性值}
注意:
a1:属性名称必须使用引号括起;
a2:属性值可以使string number boolean null object;
a3,属性值如果是字符串,必须用引号括起;
a4,json技术借鉴了js中对象声明的语法,但是,区别是js中属性名可以不加引号,但是json中属性名必须用引号;

如:
{'name':'jetty','age':20}


理解json存在的价值:之所以要用ajax需要json的支持是因为,与传统返回整张页面内容来刷新信息不同的是,ajax技术每次只接收少量的文本信息或者xml文件(xml在此处有其弊端),
如果需要给js返回较多的信息就只能通过多次请求,或者返回一条含有所有需要的信息的长字符串然后在js中自行对字符串分割处理,这显然会出现麻烦;又由于js本身就提供了
对象这个概念,所以json借鉴了js中对对象处理的方式,并在java端开发了对java对象的转换,所以这项技术可以在服务器端将一个java对象转换成json对象然后将这个json对象转换
成一个字符串,每次返回给js这个字符串让Js在客户端将这个代表一个json对象的字符串转换为js对象,这样就可以利用js对对象操作的支持一次性从中取出大量的信息,方便了
异步更新客户端页面的信息;要注意的是当服务器将<script type="text/javascript" src="<../js/jQuery-1.4.3.js"></script>这个内容发送给浏览器时,浏览器会再次对服务器
发送获取src地址处的jQuery.js文件,所以在一般开发中回引入一个压缩过的jQuery.js文件,虽然不便于查阅但是增加了浏览器与服务器通讯的效率;



例子:
	<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>

	<html>
		<head>
		<script type="text/javascript" src="js/prototype-1.6.0.3.js"></script>
		<script type="text/javascript">
		function test1(){
			var obj = {'name':'jetty','age':20};
			alert(typeof obj); //显示object;
			alert(obj.name);
		</script>
		
		function test2(){
			var obj = {'name':'witney','address':
				{'city':'beijing','street':'长安街','room':1494}
			};
			alert(obj.address.room);
		}
		function test3(){
			var arr= [{'name':'jetty','age':20},{'name':'tom','age':30}];
			alert(arr[1].name);
		}
		
		//将json字符串转化成js对象;
		function test4(){
			var str = "{'name':'Tom,'age':20}";
			var obj = str.evalJSON();
			alert(obj.name);
		}
		
		function test5(){
			var str = "[{'name':'jetty','age':20},{'name':'tom','age':30}]";
			var arr = str.evalJSON();
			alert(arr[1].name);
		}
		</script>
	</head>
	<body>
		<a href="javascript:;" onclick="test1();">ClickMe</a>
	</body>
</html>


b,表示一个对象组成的数组;
[{},{},{}...]
比如:[{'name':'jetty','age':20},{'name':'tom','age':30}]
	
	
例子:

见上文function test3();
	


json相关编程;

1)如何将一个java对象转化成json字符串;
利用json官方提供的工具来转换;
(www.json.org 下载);

例子:
将json官方的.jar文件放入lib文件夹;

package bean;

public class Stock{
	private String code;
	private String name;
	private double price;
	setter/getter...;
}

public class Test{
	public static void test1(){
		Stock s = new Stock();
		s.setCode("600015");
		s.setName("山东高速");
		s.setPrice(22);
		JSONObject jsonObj = JSONObject.fromObject(s);
		String jsonStr = jsonObj.toString();
		System.out.println(jsonStr);
	}
	
	public static void test2(){
		List<Stock> stocks = new ArrayList<Stock>();
		Random r =new Random();
		DecimalFormat df = new DecimalFormat("#.##"); //来自java.text.DecimalFormat;包;
		for(int i=0;i<8;i++){
		Stock s = new Stock();
		s.setCode("60001"+r.nextInt(10));
		s.setName("深发展"+r.nextInt(10));
		String price = df.format(r.nextDouble()*100);
		s.setPrice(Double.parseDouble(price));
		stocks.add(s);
		}
		JSONArray jsonArr = JSONArray.fromObject(stocks);
		String jsonStr = jsonArr.toString();
	    System.out.println(jsonStr);
	}
	public static void main(String[] args){
	
	}
}


2)如何将json字符串转化成javascript对象;	
prototype提供的evalJSON函数;

见上文function test4();


例子:利用ajax/json在网页上定时更新DOM;

action.java;

......
else if(action.equals("/quoto")){
//模拟生成几只股票的信息;
Test.test2();

	  
.jsp;
<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>
<html>
	<head>
		<style>
			#d1{
				width:500px;
				height:310px;
				background-color:black;
				border:1px solid red;
				margin-left:350px;
				margin-top:80px;
			}
			#d2{
				height:40px;
				color:yellow;
				background-color:red;
			}
			table{
				font-size:24px;
				font-style:italic;
				color:yellow;
			}
		</style>
		
		<script type="text/javascript" src="<js/my.js"></script>
		<script type="text/javascript" src="js/prototype-1.6.0.3.js"></script>
		<script type="text/javascript">
		
			function f1(){
				//每隔一段时间调用quoto函数;
				setInterval(quoto,3000);
			}
			
			//quoto异步向服务器发请求,并将服务器返回的json字符串转化成相应的js对象,然后根据js对象生存的信息更新表格;
			function quoto(){
				var xhr = getXhr();
				xhr.open('get','quoto.do',true);
				xhr.onreadystatechange=function(){
					if(ehr.readyState ==4 ){
						var txt = xhr.responseText;
						var arr = txt.evalJSON();
						var html='';
						for(i=0;i<arr.length;i++){
							//stock:一个js对象,描述一支股票的信息;
							var stock = arr[i];
							html+='<tr><td>'+stock.code+'</td><td>'+stock.name'+</td><td>+'stock.price'+</td></tr>';
						}
						$('tb1').innerHTML=html;  //此处在使用IE浏览器时会发生异常,原因是IE浏览器只允许对<td>中内容进行更新,其他table相关的标签都无法使用innerHTML;
					}							  //框架已经解决了这样的DOM不兼容问题;
				};
				xhr.send(null);
			}
			
		</script>
	</head>
	<body style="font-size:30px;font_style:italic;"
	onload = "f1();">
		<div id="d1">
			<div id="d2">股票实时行情</div>
			<div id="d3">
				<table cellpadding="0" cellspacing="0" width"100%">
					<thead>
						<tr>
							<td>代码</td>
							<td>名称</td>
							<td>价格</td>
						</tr>
					</thead>
					<tbody id="tb1">
					</tbody>
				</table>
			</div>
		</div>
	</body>
</html>



缓存问题:
(1)什么是缓存问题?
如果使用ie浏览器内置的ajax对象发送get请求时,会查看是否访问过该地址,如果访问过,则不再发送请求;

(2)解决方式;

a,方式一:

给请求地址添加一个随机数,比如:
xhr.open('get','check.do?'+Math.random(),true);

b,方式二:

用post方式发请求;




	
*/


