javascript


package prototype;

public class Javascript {

	public static void main(String[] args) {

	}
}


/*

一:javascript概述;
JavaScript是一种网页编程技术,用来向HTML页面添加交互行为;

JavaScript是一种基于对象和事件驱动的解释性脚本语言,具有与java和C语言类似的语法;
-直接嵌入HTML页面;
-由浏览器解释执行代码,不进行预编译;
注意:脚本的意思是不能独立生效,需要依附在其它事物之上的;

JavaScript JScript/VBScript----由于版本较多,其实质相同,后W3C将其作为标准,可简称为JS;

JavaScript的特点;
可以使用任何文本编辑工具编写,只要浏览器就可以执行程序;
解释执行:事先不编译,逐行执行;
基于对象:内置大量现成对象;
适宜:
-客户端数据计算;
-客户端表单合法性验证;
-浏览器事件的触发;
-网页特殊显示效果制作;

js的书写方式:
-直接写在onxxx事件中;
-写在<head>的<script>块中,在onxxx事件中调用---适用于当前页面的重用;
-js文件中,页面上先使用<script scr="">引用,onxxx事件中调用;

如:

html:
<html>
	<head>
		<title>标题</title>
		<script type="text/javascript" language="javascript">
		function firstMethod()
		{
			alert("hello");
		}
		</script>
		<script src="jstest.js" type="text/javascript"></script>
	</head>
	<body>
		<form>
			<input type="button" value="第一个按钮" onclick="firstMethod();"/>
			<input type="button" value="第二个按钮" onclick="firstMethod();"/>
			<input type="button" value="调用单独的js中的方法" onclick="secondMethod

();">
			</form>
	</body>
</html>

js:
function secondMethod()
{
	alert("function in file");
}


二:基础语法;
1.变量;
-声明变量: var 名称 = 值;
-变量的数据类型以赋值为准;
标识符:
-由不以数字开头的字母,数字,下划线,美元符的符号组成;
-不能和系统的保留关键字重名;

2.数据类型;
-基本类型; number/string/boolean;
如:
var s = "m\'a\"ry\nhello";
var s1 = "\u4e02";  //正则表达式 [\u4e00-\u9fa5]{3}

-特殊类型; null/undefined; 
没有定义过的变量为undefined;
html中没有的元素通过查找方法的返回结果为null;

如:
var s; 
alert(s);

会报出undefined提示;

-复杂类型; Array/Date/Math;


3.基本数据类型之间的转换;
隐式;
var s="a";
var n=1;
var b1=true;
var b2=false;
alert(s+n);   //a1
alert(s+b1);  //atrue
alert(n+b1);  //2
alert(b1+b2); //1

显式;
使用转换方法;
parseInt/parseFloat/toString..;
-强制转换成浮点数;
-如果不能转换,则返回NaN(not a number);
-例如:parseFloat("6.12")=6.12;
isNaN("???")---boolean; //注意:空格或连续空格不会被视为NaN,数字前导/后导空格将被忽略;

如:
<html>
<head>
<script type="text/javascript" language="javascript">
function getSquare()
{	
	var s1 = document.getElementById("txtNumber").value;  //由于isNaN无法识别空格,所以需要利用trim类型的方法来去除空格,但是JS本身没有此类方法,需要自行编写;
	s1 = s1.replace(/(^\s+)|(\s+$)/,"");
	if(isNaN(s1)||s1==""){
	alert("it has to be a number");
	}else{
	var data=parseFloat(s1);
	alert(data*data);
	}
}
</script> 
</head>
<body>
<input type="text" id="txtNumber"/>
<input type="button" value="计算" onclick="getSquare();"/>
</form> 
</body>
</html>


4.运算符与表达式;
严格相等:===;
-类型相同;
-数值相同;
如:
var a=10;
var b="10";
if(a==b)
alert("equal");
if(a===b)
alert("same");


非严格相等:==;
比较数值;

三元运算符;

例子:猜大小,答案为10;
html:
<input type="text" id="txtGuess" onblur="getNumber();"/>
js:
function getNumber()
{
	var s = document.getElementById("txtGuess").value;
	var data = parseInt(s);
	var result=data>10? "大了":"小了";
	result= data==10? "猜对了":result;
	result= isNaN(data)? "请输入数字":result; 
	alert(result);
}



5.流程控制;
默认情况下,顺序执行;--改变其执行顺序;
条件:if/else switch/case;
循环:for/while;

var result = 0;
for(var i=0;i<=100;i++)
{
	result+=i;
}
alert(result);



三:内置对象的使用;
-JavaScript是一种基于对象的语言;
-对象是JavaScript中最重要的元素;
-JavaScript包含多种对象:
内置对象;
自定义对象;
浏览器对象;
HTML DOM对象;
ActiveX对象;

简单数据对象;
-String/Number/Boolean;
组合对象;
-Array/Math/Date;
高级对象;
-Function/RegExp;

RegExp内置对象;
var regexp = new RegExp(pattern, attributes); 
//参数 pattern是一个字符串,指定了正则表达式的模式或其他正则表达式对象;表达正则表达式的字符串不需要用"//"来声明,直接放在引号中;
//参数 attributes是一个可选的字符串,包含属性 "g","i"和 "m",如果 pattern是正则表达式对象,而不是字符串,则必须省略该参数;

String对象;
创建字符串对象;
创建:
var str1="hello world";
var str2=new String("hello world");
属性:length;
alert(str1.length);
方法:
var s1 = s.substring(1,3); //前闭后开;
toLowerCase/toUpperCase;  //方法的大小写敏感,错误将导致无法执行;
split;
substring;
charAt;
indexOf/lastIndexOf; //lastIndexOf相当于从右向左查找的indexOf,机制相同;
replace/search/match;--结合正则表达式使用; //search无法使用/g来返回索引的数组,replace和match可以用/g来匹配全部符合要求的内容,match会返回一个数组;
exec; --返回的是一个长度始终为1的数组,其元素就是最近一次匹配的结果,如果正则表达式利用了g模式的匹配,那么在此调用exec方法返回数组中的元素就是上一个匹配内容index之后再匹配的内容;

例子:
function test(){
  var s = "AaBbCcDdEeFfGgHhIiJjKkLlMmGNnGOoggPp"
  var r = new RegExp("g","ig");
  var a = r.exec(s);
  alert(a[0]); //G;
  a = r.exec(s);
  alert(a[0]); //g;
}


正则表达式在js中放置在//之间,结合匹配标识用;
/xxx/g---g(global);
/xxx/i---i(ignored);
/xxx/m---m(multiline);
 

例子:过滤敏感字符(录入完毕,将所有的gcd替换为*);
html:
<input type="text" id="txtReplace" onblur="replaceString();"/>
js:
function replaceString()
{
	var str=document.getElementById("txtReplace").value;
	str=str.replace(/gcd/gi,"***");
	document.getElementById("txtReplace").value=str;
}

方法使用:

-replace:返回替换后的结果;
-match:返回所有匹配字符串组成的数组;
-search返回匹配字符串的首字符位置索引; //只返回第一个匹配项的下标,不支持全局匹配的正则表达式(g),找不到返回-1;

注:
$1-$9存放着正则表达式中最近的9个正则表达式的匹配结果,这些结果按照子匹配的出现顺序依次排列,以()为子匹配的标志;
这些属性是静态的,除了replace中的第二个参数可以省略RegExp之外,其他地方使用都要用RegExp.来调用;

例子:
function ReplaceDemo()
{
  var r, re;
  var s = "The quick brown fox jumped over the lazy yellow dog.";
  re =/(\S+)(\s+)(\S+)/g;
  r = s.replace(re,"$3$2$1");	//交换每一对单词;
  alert(r);
  alert(RegExp.$3);
}


补充1:docunment.write();
浏览器会在write之前先docunment.open();一个文档,再把write的内容输出到原文档里面;write结束后,默认是不会有docunment.close()的,
否则相当于打开了一个新的输入流,之后的document.write()会覆盖之前的内容;
当将document.write()直接写在<script></script>中,也就是在页面加载时就会执行,那么就会使用加载页面的流,而不会开一个新的流,当页面全都加载完了之后会默认调用close方法,
关闭了输入流,之后再调用方法来执行write方法就会开一个新的流而覆盖之前页面中的内容;

例子:
js:
	document.write('hello world!!!');
	function test(){
		document.write('hello world!');
	}
	
html:
<input type="button" value="test" onclick="test();"/>

补充2:rgexp.compile(regexp,pattern);
compile()方法用于在脚本执行过程中编译正则表达式,也可用于改变和重新编译正则表达式;
同样,第一个参数可以是正则表达式对象,这种情况下再给出第二个参数会报错;
第一个参数也可以是正则表达式内容的字符串(不需要"//"包含),第二个参数是模式参数;



4.数组;
创建:
var a1 = new Array(); //不用规定长度,即使规定后,也可以扩容;
a1[0]=12;
a1[1]="mary";
a1[2]=true;

var s2=[12,"mary",true]; //简写;

二维数组:
var s3=new Array();
a3[0]=[10,20];
a3[1]=[30,40];

数组的属性:
length;

方法:
toString();--使用,连接成一个字符串,常用于输出数组的内容显示;
join("");--使用双引号中的自定义符号来将数组元素练成一个字符串; //将参数插入到元素与元素之间后连接字符串;
concat(value,...);--连接数组; //参数可以是数组也可以是字符串或者数字等;
slice(start,end); --数组的截取;
注意:start参数为开始索引位置,end-1为结束索引位置,如果省略end参数则默认为从start位置直到数组的结尾;

reverse();
sort();--默认情况下,按照字符比较,如:12 2,先比较第一个元素12的第一个字符1和第二个元素2,2大,就不会再把12中的第二个字符2拿来继续比较了;
如果出现类似12 1这样第一个元素的第一个字符与第二个元素相同那么由于第一个元素存在第二个字符,那么就默认第一个元素大于第二个元素;


如:

var a1=new Array(100,200,300);
alert(a1.toString());
alert(a1.join("|"));

var a =[1,2,3];
var b=a.concat(4,5); 
alert(b.toString()); //1,2,3,4,5;

var arr1=['a','b','c','d'];
var arr2=arr1.slice(2,4);
alert(arr2.toString()); //c,d;
var arr3=arr1.silce(1);
alert(arr3.toString()); //b,c,d;

例子:将输入框中用,隔开的数字实现逆序和从小到大排序;

html:
<input id="txtArray" type="text" value=""/>
<input type="button" value="翻转" onclick="operateArray(1);"/>
<input type="button" value="排序(按照字符)" onclick="operateArray(2);"/>
<input type="button" value="排序(按照数值)" onclick="operateArray(3);"/>

js:
function operateArray(t)
{
	var str = document.getElementById("txtArray").value;
	var array  = str.split(",");
	switch(t)
	{
		case 1:
			array.reverse();
			break;
		case 2:
			array.sort();
			break;
		case 3:
			array.sort(sortFunc);  //指定一个用于排序的方法;
			break;
	}
	alert(array.join("|"));
}
function sortFunc(a,b)
{
	return a-b;
}


5.Math对象;

属性/方法: Math.xxx();

Math.abs(x); //取绝对值;
Math.max(x,y,...); //取最大值;
Math.min(x,y,...); //取最小值;
Math.random(); //取随机数(0-1)前闭后开;
Math.round(x); //四舍五入;
Math.floor(x); //去小数取小值;
Math.ceil(x);  //去小数取大值;

例子:在min/max中取随机数;
html:
<input type="button" value="random" onclick="getRandom(1,12)"/>;

js:
function getRandom(min max)
{
	var r = Math.random();  //0-1之间的数(1取不到);
	var result = Math.floor(r*(max+1-min)+min);  //这是几率分配最公平的算法,考虑到max是取不到的;
	alert(result);
}


6.Number对象;

Number对象是原始数值的包装对象;

方法:
toString();

toFixed();  //数值转换为字符串,并保留小数点后一定位数,最后一位被保留的小数会发生四舍五入,也可能被0补足;

如:
var data=23.56789;
alert(data.toFixed(2)); //23.57;

data=23.5;
alert(data.toFixed(2)); //23.50;


7.正则表达式对象;
一:结合string对象的方法:replace/search/match;
二:直接使用正则表达式对象的方法;
var r=/\d{3}/; --r属于一个正则表达式对象;
r.test(string); --判断stinrg是否符合此表达式,返回boolean;

例子: 验证3-5位小写字母;
html:
录入数据: <input type="text" id="txtS" onblur="validData();">;

js:
function validData()
{
	var str=document.getElementById("txtS").value;
	var reg=/^[a-z]{3,5}$/;  
	//^&代表从开始到结束必须完全满足正则表达式内容;如果没有标识,则只要在字符串中能找到正则表达式中要求的内容,
	就算通过,而不是只能出现正则表达式中要求的内容;
	
	if(!reg.test(str))
		alert("请重新录入");
}


8.Date 对象;
创建:   
var r=new Date(); --当前时间;
var r=new Date(dateVal); --如果是数字值,dateVal 表示指定日期与 1970年 1 月 1 日午夜间全球标准时间 的毫秒数;
如果是字符串;则 dateVal按照 parse 方法中的规则进行解析;其中 dateVal是一个包含以诸如 "Jan 5, 1996 08:47:00"的格式表示的日期的字符串;

Date.parse(dateVal); //parse方法返回一个整数值,这个整数表示 dateVal中所包含的日期与 1970 年 1 月 1 日午夜之间相间隔的毫秒数;

方法:

(1)getXXX();
   getDate()//getMonth();
   
(2)setXXX();//修改日期;
   
(3)toXXX()--得到某种格式的字符串;

如:
html:
<input type="button" value="testDate" onclick="testDate();">

js:
function testDate()
{
	var r= new Date();
	alert(r.getDate());    //日;
	alert(r.getMonth()+1); //月;
	alert(r.toString());   //全信息;
	alert(r.toLocaleTimeString());   //(上午/下午)xx：xx：xx;
	r.setDate(r.getDate()+5); //显示5天后的日期;
	alert(r.toLocaleDateString());   //xx年xx月xx日;
}


9.方法;
a.定义:
function Name(x,y,z)
{
	return xxx;
}

b.重载;
传统意义上的重载无法在js中实现,只要方法重名,以最后定义的一个方法为准;
方法中可以使用arguments得到传入的参数的数组,可以模拟方法的重载;

如:
<input type="button" value="方法的重载" onlick="f(12,30)";>

function f()
{
	if(arguments.length == 1)
	{
		var data = arguments[0];
		alert(data*data);
	}else if(arguments.length == 2)
	{
		alert(arguments[0]+arguments[1]);
	}
}

c.匿名内部方法Function(可以指定在方法外部);

如:
function testInner()
{
	var array = [23,4,321,76]
	
	var f = new Function("alert('mary');");
	f();
	var f2 = new Function("x","y","alert(x+y);");
	f1(10,20);
	
	var f3 = new Function("x","y","return x-y;");
	
	array.sort(f3);
	
	alert(array.toString());
}

d.匿名内部方法function(可以指定在方法外部);

如:
function testInner()
{
	var array = [23,4,321,76]
	
	var f = function(a,b)
	{
		return a-b;
	};
	
	array.sort(f);
	
	alert(array.toString());
}


补充:js方法的运用实例;

(1)
var foo01 = function()  //or fun01 = function()  
 {  
     var temp = 100;  
     this.temp = 200;  
     return temp + this.temp;  
 }  
 alert(typeof(foo01));  //function;
 alert(foo01()); //300;
//最普通的function使用方式,定一个JavaScript函数;两种写法表现出来的运行效果完全相同,唯一的却别是后一种写法有较高的初始化优先级;
在大扩号内的变量作用域中,this指代foo01的所有者,即window对象;

(2)
var y="global";  
function constructFunction()  
{  
    var y="local";  
    var fun = new Function("alert(y);"); //global;  
    fun();  
}  
constructFunction();
//Function定义的方法体中引用的变了是全局的;

(3)
var yx01 = new function() {return "圆心"}; 
alert(yx01); //[object object] ;

此时该代码等价于:
function a(){ 
    return "圆心"; 
} 
var yx01 = new a(); 
alert(yx01);

var yx01 = new function() {return new String("圆心")}; 
alert(yx01); //圆心;
//只要 new表达式之后的 constructor返回(return)一个引用对象(数组,对象,函数等),都将覆盖new创建的匿名对象,如果返回一个原始类型(无return 时其实为 return 原始类型 undefined),
那么就返回 new 创建的对象;由于new String会构造一个对象,而不是一个 string直接量,且new String(x)如果带参数,那么alert它的时候就会返回 x;所以 yx01将返回 new String(”圆心”)这个对象,而 alert yx01则显示 "圆心";

var yx02 = function() {return "圆心"}(); 
alert(yx02); //圆心

代码等价于： 
var 匿名函数 = function() {return "圆心"}; 
yx02 = 匿名函数(); 
alert(yx02);

need test:
var foo02 = new function()  
 {  
     var temp = 100;  
     this.temp = 200;  
     return temp + this.temp;  
 }  

 alert(typeof(foo02));  //object;
 alert(foo02.constructor());  //300;
//这是一个比较puzzle的function的使用方式,好像是定一个函数;但是实际上这是定一个JavaScript中的用户自定义对象,不过这里是个匿名类;这个用法和函数本身的使用基本没有任何关系,在大扩号中会构建一个变量作用域,this指代这个作用域本身;


例子:统计文本框中录入的各字符的个数;
html:
<input id="countString" type="text" value="aacccbii555" onblur="count();"/>

js:
function count()
{
	var array = new Array();
	var char = new Array();
	var quat = new Array();
	var sum = 1;
	var show="";
	var str = document.getElementById("countString").value;
	var len = str.length;
	for(var i=0;i<len;i++)
	{
		array[i]=str.charAt(i);
	}
	
	for(var i=0;i<len;i++)
	{
		for(var j=i+1;j<len;j++)
		{
			if(array[i]==array[j])
			{
				//var temp = array[j];
				array[j]=array[len-1];
				//array[len-1]=temp;
				alert(array.toString());
				len--;
				sum++;	
				j--;
			}
						
		}
		char[i]=array[i];
		quat[i]=sum;
		sum=1;
	}
	for(var i=0;i<char.length;i++)
	{
		show = show+char[i]+'='+quat[i]+' ';
	}
	alert(show);
}



例子:彩票双色球生成器;(前6个号 1-33,不能重复;最后一个号码1-16);
html:
<input type="button" value="双色球" onclick="randomball();">

js:
function randomball()
{
	var redball = new Array(6);
	var blueball;
	var test = new Array(33);
	for(var i=0;i<33;i++)
	{
		test[i]=true;
	}
	var random = function(min,max)
	{
		var r = Math.random();
		var result = Math.floor(r*(max+1-min)+min);
		return result;
	};
        for(var i=0;i<6;i++)
	{
		var r = random(1,33);
		if(test[r-1])
		{
			test[r-1]=false;
			redball[i]=r;
		}else
		{
		i--;
		}
	}

	blueball=random(1,16);

	alert("redball:"+redball.toString()+" | "+"blueball:"+blueball);
}


10.全局函数;

全局函数可用于所有内建的javascript对象,常用的全局函数有:
parseInt/isNaN/eval/decodeURI/encodeURI 等;

encodeURI():把字符串作为URI进行编码;

decodeURI():对encodeURI()函数编码过的URI进行解码;

如:
html
<input type="button" value="test encode" onclick="testURI();">

js:
function testURI()
{
		var str="http://tts6.tarena.com.cn/index.html?name=mary";
		var r1 =encodeURI(str);
		alert(r1);  //和原str没有变化;
		var str2="http://tts6.tarena.com.cn/index.html?name=张三";
		var r2 =encodeURI(str2);
		alert(r2); //tts6.tarena.com.cn/index.html?name=%E8%B4%A5...
		var r3=decodeURI(r2);
		alert(r3); //回到原始的str2;
}


eval;
提取并执行被传入的字符串中合法的语句;
只接受原始字符串作为参数,如果参数中没有合法的表达式则抛出异常;

var str="2+3";
alert(str);  //2+3;

var r=eval(str);
alert(r);    //5;


例子:简单的页面计算器;

html:
<input type="button" value="1" onclick="cal(1);"/>
<input type="button" value="2" onclick="cal(2);"/>
<input type="button" value="3" onclick="cal(3);"/>
<input type="button" value="+" onclick="cal('+');"/>
<input type="button" value="-" onclick="cal('-');"/>
<input type="button" value="=" onclick="cal('=');"/>
<br/>
<input type="text" id="txtCal" />

js:
var checkRefresh = false;
function cal(str)
{	

	if(str=="=")
	{
		var ex=document.getElementById("txtCal").value;
		var result=eval(ex);
		document.getElementById("txtCal").value=result;
		checkRefresh = true;
	}
	else
	{	
		if(checkRefresh&&str!="+"&&str!="-"&&str!="="){
		document.getElementById("txtCal").value="";
		document.getElementById("txtCal").value+=str;
		checkRefresh =false;
		}else{
		document.getElementById("txtCal").value+=str;
		checkRefresh =false;
		}
	}
}


四:DHTML应用;
dynamic HTML;
动态网页效果;

DHTML对象模型;
Window;    //顶层对象,整个浏览器中打开的页面窗口;
History;   //历史访问记录;
Navigator; //浏览器相关信息;
Location;  //地址栏;
Screen;    //分辨率,色彩等配置;
event;     //事件;
document;  //当前页面内容;


1.window对象;

方法;

对话框:
(1)(window.)alert(str);

(2)(window.)confirm(str);--确认对话框,显示str字符串内容,按确定按钮返回true,其他按钮(取消/X号)返回false；

如:
html:
<input type="submit" value="删除" onclick="return delData();"/>

js:
function delData()
{
	var r = window.confirm("是否删除？");
 	return r;
}


事件:

onxxx="return true;"---默认值,事件触发;
onxxx="return false;"---事件被取消;

如:
html:
<input type="submit" value="删除" onclick="return false;"/>


(3)(window.)prompt(str,value);--输入框,第一个参数为提示信息,第二个参数为弹出框中输入框的初始内容,最终返回输入框中内容;


打开新窗口:
window.open("url","name",config);--如果没有第二个标识参数,则会不断打开新的页面,如果有第二个标识参数则只会在第一次打开一个新窗口后就不会重复
打开;而第三个参数则是配置新打开窗口的大小,位置等参值;返回打开窗口的对象;
window.close(); //默认关闭当前窗口,也可以用窗口对象.close()来关闭指定窗口;

如:
var config="toolbar=yes,location=no,width=500,height=300"; //有工具栏,无地址栏,高..宽..;
var openurl="http://www.google.com";
var newWin= window.open(openurl,"popwin",config);
newWin.focus();


定时器相关;
周期性定时器:
window.setInterval(function,time); //第一个参数为需要执行的操作,第二个参数为操作相隔的时间;返回定时器对象;
window.clearInterval(需要清除定时的timer对象);

如:
html:
<input type="text" id="txtTime"/>
<input type="button" value="启动时钟" onclick="startClock();"/>
<input type="button" value="停止时钟" onclick="stopClock();"/>

js:
function showTime()
{
	var r= new Date();
	document.getElementById("txtTime").value = r.toLocaleTimeString();
}

var timer;

function startClock()
{
	window.clearInterval(timer); //这条语句的作用是,如果用户连续触发startClock()方法会导致之前开启的timer任务没有结束,又开始了新的任务,并且将timer对象刷新了,也就无法指向性关闭之前的任务了;
	timer = window.setInterval(showTime,1000);

}
function stopClock()
{
	window.clearInterval(timer);
}


一次性定时器:
window.setTimeout(function,time); //在time时间结束后执行function;
window.clearTimeout(需要清除的一次性定时器对象);

如:
html:
<input type="button" value="5s后将跳转到之前的页面" onclick="openWindow();"/>
停留在当前页面,请点击<a href="javascript:stayWindow();">这里</a>

js:
var timer1;
function openFunc()
{
	window.open("http://tts6.tarena.com.cn","previouspage");
}
function openWindow()
{
	window.clearTimeout(timer1);
	timer1 = window.setTimeout(openFunc,5000);
}
function stayWindow()
{
	window.clearTimeout(timer1);
}


document对象与DOM; (js中原始的选择器)
DOM--document object model;--对document直接做的操作被统称为DOM操作;

每个载入浏览器的HTML文档都会成为document对象;
通过使用document对象可以从脚本中对HTML页面中的所有元素进行访问;

HTML页面上的每个内容作为一个节点存在--整个文档是一颗节点树;
document代表整棵树,树根;

a.查询;--找到文档中的某个节点对象;
方法一:document.getElementById("")--通过id找到对象或者null;

b.读取信息或者修改信息;
将HTML标签对象化;
<input obj.value/type
<a     obj.href
<img   obj.src

元素中间的文本内容--innerHTML
<a>text</a>   obj.innerHTML

样式
<p style="color:Red;">p text</p>
obj.style.color="red";
obj.style.fontSize="18pt"; font-size在此处被写为fontSize,由于标示符只包含字母数字下划线和美元符,所以-这里不能被使用;
obj.style.backgroundColor="silver";

如:
html:
<input type="button" value="test DOM" onclick="testDom();"/>
<img src="" id="img1"/>
<p id="p1">p text</p>

js:
function testDOM()
{
	var obj=document.getElementById("img1");
	obj.src="account_out.png";
	var pobj=document.getElementById("p1");
	pobj.innerHtml="aaabbb";
	pobj.style.color="red";
	pobj.style.backgroundColor="silver";
}


通过引入class,动态改变复杂的样式;

如:
html:
<h1 id="h1" >h1 text</h1>

js:
function testDOM()
{
	var hobj=document.getElementById("h1");
	hobj.className = "s1"; 由于在JS中class如同var一样是关键字,所以这里用className代替;
}


例子:用户名验证框;

html:
<style>
	div.success,div.fail
	{
		width:150px;
		height:30px;
	}
	div.success
	{
		color:green;
		border:1px solid green;
	}
	div.fail
	{
		color:red;
		border:1px solid red;
	}
</style>

<table>
	<tr>
		<td>用户名: </td>
		<td>
			<input type="text" id="txtName" onblur="validName();"/>
		</td>
		<td>
			<div id="nameInfo"></div>
		</td>
	</tr>
</table>

js:
function validName()
{
	var str = document.getElementById("txtName").value;
	var reg= /^[a-z]{3,5}$/;
	var obj= document.getElementById("nameInfo");
	if(reg.test(str))
	{
		obj.className="success";
		obj.innerHTML="用户名正确";
		return true;
	}
	else
	{
		obj.className="fail";
		obj.innerHTML="用户名错误";
		return false;
	}
}


例子:根据上例验证年龄,并设计提交按钮验证用户名和年龄后提交;
......
html:
<table>
	<tr>
		<td>年龄: </td>
		<td>
			<input type="text" id="txtAge" onblur="validAge();"/>					
		</td>
		<td>
			<div id="ageInfo"></div>
		</td>
	</tr>
</table>
<br/>
<input type="submit" value="保存" onclick="return validData();"/>;

js:
function validAge()
{
	var str = document.getElementById("txtAge").value;
	var reg = /^[0-9]{2}$/;
	var obj = document.getElementById("ageInfo");
	if(reg.test(str)&&parseInt(str)<70)
	{
		obj.className="success";
		obj.innerHTML="年龄正确";
		return true;
	}
	else
	{
		obj.className="fail";
		obj.innerHTML="年龄错误";
		return false;
	}
}
function validData()
{
	var r1 = validName();
	var r2 = validAge();
	return r1&&r2;
}


查询;
方法二:根据层次关系来查询;
obj.parentNode--返回单个节点;
obj.childNodes--返回多个节点的数组;
obj.firstChild/lastChild;

如:
html:
<select id="sel1">
	<option>aaa</option>
	<option>bbb</option>
	<option>ccc</option>
</select>

js:
var selobj = document.getElementById("sel1");  //selobj此时为一个节点对象;
//alert(selobj.childNodes.length);  //7,.childNodes方法返回一个节点数组,由于空白处也被算为子节点,所以返回7个;
//alert(selobj.childNodes[0].nodeName);  //#text;
//alert(selobj.childNodes[1].nodeName);  //OPTION;
var count = 0;
for(var i=o;i<selobj.childNodes.length;i++)
{
		if(selobj.childNodes[i].nodeName=="OPTION")
		{
			count++;
		}
		alert(count);
}


c.未知节点对象类型时读取数据;
obj.nodeName---返回值全大写;

方法三:
根据标签名查找;
document.getElementsByTagName("input")---返回节点数组;

如:
obj.getElementsByTagName("input")[1];


例子:购物车;
html:
<h2>购物车</h2>
			<table border="1" id="table1">
				<tr>
					<td>名称</td>
					<td>价格</td>
					<td>数量</td>
					<td>小计</td>
				</tr>
				<tr>
					<td>a</td>
					<td>10</td>
					<td>
						<input type="button" value="-" onclick="decrease

(this);"/>
						<input type="text" value="1"/>
						<input type="button" value="+" onclick="increase

(this);"/>
					</td>
					<td>10.00</td>
				</tr>
				<tr>
					<td>b</td>
					<td>20</td>
					<td>
						<input type="button" value="-" onclick="decrease

(this);"/>
						<input type="text" value="1"/>
						<input type="button" value="+" onclick="increase

(this);"/>
					</td>
					<td>20.00</td>
				</tr>
			</table>
			总计:<span id="totalPrice">30.00</span>
			
js:
function increase(btnObj)
{
	var childs = btnObj.parentNode.childNodes;	
	for(var i=0;i<childs.length;i++)
	{
		var node = childs[i];
		if(node.nodeName == "INPUT" && node.type =="text")
		{
			var count = parseInt(node.value);
			count++;
			node.value = count;
		}
	}
	calTotal();
}
function decrease(btnObj)
{
	var childs = btnObj.parentNode.childNodes;	
	for(var i=0;i<childs.length;i++)
	{
		var node = childs[i];
		if(node.nodeName == "INPUT" && node.type =="text")
		{
			var count = parseInt(node.value);
			if(count>0)
			{
				count--;
			}
			node.value = count;
		}
	}
	calTotal();
}
function calTotal()
{
	var t = document.getElementById("table1");
	var rows = t.getElementsByTagName("tr");
	var total = 0;
	for(var i=1;i<rows.length;i++)
	{
		var cells = rows[i].getElementsByTagName("td");
		var price = cells[1].innerHTML;
		var quantity = cells[2].getElementsByTagName("input")[1].value;
		var sum = parseFloat(price)*parseFloat(quantity);
		cells[3].innerHTML=sum.toFixed(2);
		total+=sum;
	}
	document.getElementById("totalPrice").innerHTML=total.toFixed(2);
}


DOM(文档对象模型):document;
BOM(浏览器对象模型):window,history,location,event...等;


d.增加新节点;
第一步:创建新的节点对象;
document.createElement("a/input/p");

第二步:设置新对象的各信息;

第三步:xxx.appendChild(newObj);--追加newObj新节点到其父节点xxx的最后;
	   xxx.insertBefore(newObj,refNode);--追加newObj新节点放到refNode之前,必须满足参数的两个节点都是xxx的子节点;
	   
如:
<head>
	<script type="text/javascript">

var fff = Function("alert('hello');");

function addNewNode()
{
	var txtObj = document.createElement("input");
	txtObj.type = "text";
	txtObj.value = "newButton";
	var f = document.getElementById("f1");
	f.appendChild(txtObj);
	
	var aObj = document.createElement("a");
	aObj.href="http://tts6.tarena.com.cn";
	aObj.target = "_blank";
	aObj.innerHTML = "TTS6.0";
	
	var btnObj = document.createElement("input");
	btnObj.type = "button";
	btnObj.value = "click me";
	btnObj.onclick = fff;
	btnObj.onclick = function(){ //效果同上一行相同;注意此处无法通过类似xxx.onclick="alert('123');";这样的方法为onclick事件注册回调函数;
	alert("hello");
	};
	f.insertBefore(aObj,f.lastChild);
	f.insertBefore(btnObj,f.lastChild);
	
}
	</script>
</head>
<body>
<form id="f1">
<input type="button" value="添加新的节点" onclick="addNewNode();">
</form>
</body>


e.节点的删除;
xxx.removeChild(obj); //需要保证obj为xxx的子节点;

如:
obj.parentNode.removeChild(obj);


例子:下拉框联动;
html:
<select id="sel1" onchange="changeOptions();">
	<option>--请选择--</option>
	<option>--JAVA--</option>
	<option>--PHP--</option>
	<option>--.NET--</option>
</select>
<select id="sel2">
	<option>--请选择--</option>
</select>

js:
function changeOptions()
{
	var array = [
			["--请选择--"],
			["SE","JDBC","JSP"],
			["HTML","CSS","PHP"],
			["C#",".NET"]
			];
			
	var index = document.getElementById("sel1").selectedIndex;
	var data = array[index];
	var sel = document.getElementById("sel2");
	while(sel.childNodes.length>0)
	{
		sel.removeChild(sel.firstChild);
	}
	for(var i=0;i<data.length;i++)
	{
		var obj = document.createElement("option");
		obj.innerHTML = data[i];
		sel.appendChild(obj);
	}
}

HTML DOM:标准的DOM操作,进行封装,以实现代码的简化;
var obj = new Option("JDBC","1");
sel.options[1] = obj;

主要介绍两种封装好的元素;
select+option;
table+tr+td;

select对象;
常用属性:
options;  //所有选项的数组;
selectedIndex;
size;

常用方法:
add(option);
remove(index);

option对象;
创建对象:
var o =new Option(text,value);

常用属性:
index,text,value,selected;


例子:将联动方法用html dom重新编写一次;

function changeOptions()
{
	var array = [
			["--请选择--"],
			["SE","JDBC","JSP"],
			["HTML","CSS","PHP"],
			["C#",".NET"]
			];
			
	var index = document.getElementById("sel1").selectedIndex;
	var data = array[index];
	var sel = document.getElementById("sel2");
	sel.options.length = 0;  //简化;
	for(var i=0;i<data.length;i++)
	{
		var obj = new Option(data[i]);  //简化;
		sel.add(obj);  //简化;
	}
}


table+tr+td;

属性:
table.rows;
row.cells;
cell.cellIndex/innerHTML/colSpan/rowSpan;

方法:
table.insertRow(index); //插入行; 
row.insertCell(index);  //插入列;
table.deleteRow(index);
row.deleteCell(index);


如:

html:
Name: <input type="text" id="txtName1" /><br/>
Price: <input type="text" id="txtPrice"/><br/>
<input type="button" value="Add" onclick="addRow();"/>
<table id="t1" border="1">
<tr>
	<td>名称</td>
	<td>价格</td>
</tr>
<tr>
	<td>a</td>
	<td>10</td>
</tr>

js:
function addRow()
{
	var t = document.getElementById("t1");
	var row = t.insertRow(t.rows.length);
	var cell1 = row.insertCell(0);
	cell1.innerHTML=document.getElementById("txtName1").value;
	var cell2 = row.insertCell(1);
	cell2.innerHTML=document.getElementById("txtPrice").value;
}


window对象的其它子对象(BOM);

6.history对象:当前浏览器窗口的历史访问记录;
history.back(); //必须保证存在可以后退的页面(历史记录);
history.forward();
history.length;

如:
html:
<input type="button" value="BOMtest" onclick="testBOM();">

js:
function testBOM()
{
	history.back();
}


7.screen对象:代表当前屏幕信息;

常用属性:
-width/height;
如:1024X768;

-availWidth/availHeight; //有效高度/有效宽度(去除了例如开始栏以及其他固有存在内容的距离);

如:
window.width=screen.availWidth;
window.height=screen.availHeight;

屏幕信息不能被修改只能被读取;
screen.width = 123; //错误！


8.location对象:代表浏览器URL地址栏;

location.href="url"; //起到了跳转的效果;并且创建了历史访问记录;

location.replace("url"); //起到了跳转的效果;不会产生新的历史访问记录;

如:
js:
function testBOM()
{
	location.href="http://tts6.tarena.com.cn";
	location.replace("http://tts6.tarena.com.cn");
}


页面间跳转总结:

<a></a>--静态(可设置是否在新窗口中打开);
window.open---一定会打开新窗口;
history.xxx()---在原窗口打开,受限于历史记录;
location.href---在原窗口打开,保留历史记录;
location.replace()---在原窗口打开,不保留历史记录;


9.navigator对象;

注意:
在js中以下两种语法的效果相同;
screen.width;
screen["width"];

通过js遍历查询某个对象的所有属性;
如:
js:
function testBOM()
{
	var str = "";
	for(var p in navigator)
	{
		str+=p+":"+navigator[p]+"\n";
	}
}


10.event事件;

a.事件分类;

鼠标事件; onclick/onmouseover(鼠标移上)/onmouseout(鼠标移开)/ondblclick(双击事件);
键盘事件; onkeyup/onkeydown;
状态事件; onblur/onfocus/onsubmit/onchange/onload/onunload(关闭退出时);

b.如何定义事件;

<元素标签 onxxx = "funciton();"> ---静态;
btnObj.onxxx = Function; ---动态;

c.事件可以被取消;(常用于页面的提交;)
onxxx = "return true;" ---默认值,事件触发;
onxxx = "return false;" ---事件被取消;

d.事件有冒泡机制; 
---当多层元素之间定义了相同的事件时,事件会从最里层开始层层向上触发到最外级的事件;
或者当最里层定义了一个事件,外层元素只要满足触发事件的条件就会触发里层事件;


event对象;
-任何事件触发后将会产生一个event对象,记录了事件发生时鼠标的位置,键盘按键状态和触发的对象等信息;
也就是说event对象封装了和当前事件相关的所有信息;

常用属性;
clientX/clientY; //事件发生的坐标点;
srcElement;  //引发当前事件的元素;

例子:使用事件的冒泡机制来简化之前的简单计算器;
html:
			改进版的简单计算器(通过冒泡机制):
			<div style="border:1px solid black;" onclick="cal2();">
				<input type="button" value="1"/>
				<input type="button" value="2"/>
				<input type="button" value="3"/>
				<input type="button" value="+"/>
				<input type="button" value="-"/>
				<input type="button" value="="/>
			<br/>
			<input type="text" id="txtCal2"/>
			</div>
			
js:
var checkRefresh2 = false;
function cal2()
{
	var obj = event.srcElement;
	if(obj.nodeName=="INPUT"&&obj.type=="button"){
		if(obj.value=="="){
			var str = document.getElementById("txtCal2").value;
			var data = eval(str);
			document.getElementById("txtCal2").value=data;
			checkRefresh2 = true;
		}else{
			if(checkRefresh2){
			document.getElementById("txtCal2").value="";
			document.getElementById("txtCal2").value+=obj.value;
			checkRefresh2 = false;
			}else{
			document.getElementById("txtCal2").value+=obj.value;
			}
		}
	}
}


使用event对象时,可以直接在html页面或者js中使用event关键字,但是火狐浏览器
只识别html中的event关键字;并且不识别event对象的方法srcElement,而识别target方法,虽然其作用是相同的;
那么鉴于IE浏览器不识别target关键字,火狐不识别srcElement,而Chrome都能识别,那怎么才能让代码兼容所有
的浏览器呢?

解决方法:
在html中的捕获事件的相关方法中增加一个事件对象参数(eObj),也就是在html中将事件对象event当作参数传入js中,之后js中也就不需要
使用event关键字了;然后利用语句: var obj = eObj.target||eObj.srcElement;

如:
html:
<div style="border:1px solid black;" onclick="cal2(event);">

js:
function cal2(eObj)
{
	...
}


浏览器兼容问题:
1.首先要保证所写的代码是符合标准规范的,这样能够达到最大的适用性;
2.那么对于一些特殊情况,那就需要特殊处理,具体怎么特殊处理那只能用个案实例来解释了;


js中的面向对象基础;

封装:对象相关的数据和行为组织起来;
数据:属性;
行为:方法;

1.使用Object(通用对象)封装--使用简单,重用性差,适用于简单数据的封装;

如:
html:
<input type="button" value="使用object来封装数据" onclick="testObject();"/>

js:
function testObject()
{
//封装;
var obj = new Object();
obj.name = "mary";
obj.age = 18;
obj.sing = new Function("alert('lala')");
//测试;
alert(obj.name);
alert(obj.age);
obj.sing();
}

2.类似于创建一个类,其实在js中为自定义对象,利用关键字this的使用来令js辨识其为一个类;
重用性较好,适用于大多数的封装;

如:
html:
<input type="button" value="使用自定义类来封装数据" onclick="testClass();">

js:
function Student(n,a)
{
	this.name=n;
	this.age=a;
	this.introduceSelf=function()
	{
		var str = "i am " + this.name + "," + this.age + " years old.";
		alert(str);
	};
}
function testClass()
{
	var p1 = new Student("mary",18);
	var p2 = new Student("john",20);
	alert(p1.name);
	p1.introduceSelf(); //i am mary,18 years old.
	alert(p2.age);
	p2.introduceSelf(); //i am john,20 years old.
}

附加举例:
<head>
	<script type="text/javascript">
function Dog(name,age) {
    this.Name = name;
    this.Age = age;
}
function testClass()
{
Dog.prototype.Category1 = "狗类";
var dog01 = new Dog("土狗",2);
var dog02 = new Dog("黄狗",5);
alert(dog01.Name+" "+dog01.Category1);
alert(dog02.Name+" "+dog02.Category1);
Dog.prototype.Category2 = "犬类";
alert(dog01.Name+" "+dog01.Category2);
alert(dog02.Name+" "+dog02.Category2);
}
	</script>
</head>
<body>
<input type="button" value="面向对象" onclick="testClass();"/>
</body>
注意:利用类名.prototype相当于定义一个static的属性;


3.传递到服务器端;--服务器端只能识别此种封装数据;
JSON(JavaScript Object Notation)--轻量级的数据交换格式;

如:
html:
<input type="button" value="使用JSON来封装数据" onclick="testJSON();"/>

var obj={
	"firstName":"Brett","lastName":"Lavigne";
};

js:
function testJSON()
{
	var obj = {
	"name":"mary","age":"18"
	};
	alert(obj.name);
	alert(obj.age);	
}


//HTML/CSS/JS告一段落;

*/


