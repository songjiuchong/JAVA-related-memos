HTML/CSS


package prototype;

public class HTMLCSS {

	public static void main(String[] args) {

	}
}

/*

WEB基础;

软件; 
可执行的桌面应用程序; exe;
web 类型的应用程序; B/S;

结构:
UI层---显示和交互； 
业务层---JDBC;
数据存储层---Oracle,IO;

Web类型的应用程序:网站,OA,CRM等;
所需要的技术:
(1)客户端技术:浏览器发出请求,显示页面;
(2)服务器端技术:侦听,解析,返回页面(响应);

html(排布,结构)+CSS(外观美化)+JavaScript(脚本,实现功能);


HTML:
一:html概述和基础语法;
HTML(HyperText Markup Language)：超文本标记语言;
创建的文件必须是.html或者是.htm的;
html是一种编写网页的解释性的标记语言;解释性:是一种翻译而非编译的过程(不存在报错提醒),所以如果代码有错误,它还是会去
尽可能的试图将代码转换为页面上的内容(就比如一个只有开始标记而没有结束标记的语句也能正确显示其标记的效果),
但是可能导致效果与事先要求的不同;
结合CSS和JS可以实现功能复杂的页面;

标记:用一对<>括起的,显示时会有特定的效果;
封闭类型的标记:成对出现,也称为双标记;比如<h1></h1>;
非封闭类型的标记:只有一个标记,也被称为单标记;由于版本在更新,会出现几种都行得通的写法,但是要使用最合理的,<br/>(建议)或者<br>;

HTML文档结构:
html页面；

-版本信息：<!DOCTYPE html.....>  //为了指定当前代码的风格和要求(能被哪个版本所兼容);
-Stricg DTD //严格型; 对最新的版本兼容,重新开发一个项目;
-Transitional DTD  //传统(过渡)型; 兼容型比较好,可以适应各个版本的要求,适合维护一个旧的项目;
-Frameset DTD  //框架型;

-文件头:<head></head> 页面头元素,包含了页面整体信息相关的内容; 
(1)title:为页面定义标题;

(2)meta：元数据; 用于定义网页的基本信息;
为空标记,其常用属性有:content,http-equiv;

//属性：属性也通常出现在开始标记里,用空格隔开,如:<h1 align="center">sss</h1> ;
       如果有多个属性可以用空格隔开,如:<h1 align="center" ......>sss</h1>;
      
如:
<head>
  <meta http-equiv="refresh" content="10"/>  //每10秒刷新一次页面;
  <meta http-equiv="content-type" content="text/html;charset=utf-8"/>  //设置浏览器的字符编码;
  //注意:如果在html文件中设置的是如上的utf-8编码,但是在保存文件时(比如选择另存为)可以在定义文件名下面定义文件存储的物理字符编码,如果
             这两个设置矛盾时,将以物理字符编码为准,所以要尽量在保存时将这个选项保持和文本中的设置一致;
</head>                                        

例子:写一个标准格式的HTML;

<!DOCTYPE html.....>
<html>
   <head>
      <title>第一个页面</title>
      <meta http-equiv="refresh" content="3"/>
      <meta http-equiv="content-type" content="text/html;charset=utf-8"/>
   </head>
   <body>
      ......
   </body>
</html>

(3)style：样式;
(4)script:脚本代码;


-文档主体部分:<body></body> 

(1)文本标记;

a.空格折叠:多个空格或制表符压缩成单个空格,即只显示一个空格;

b.特殊字符(如空格,><),可以用转义字符,也称为字符实体;
空格   &nbsp;
<  &lt;
>  &gt;

c.标题
h1：一号标题; h1--h6(由大到小,标题独占一行);
如:aaa<h1>ssss</h1>bbb
显示为三行显示,因为标题必须独占一行;

d.段落;如果想要让一段文字在必要处换行,那么直接在html文本中用回车是无效的,只能显示一个空格,
那么如果使用<br/>则确实会换行,但是换行处的行间距和标准行间距相同;但是就标准段落的形式显示而言
,段与段之间的行间距会大于标准行与行之间的间距;

<p align="center">段落文本内容</p>

e.换行<br/>

f.分组元素:
<div></div> //独占一行的分组;
<span></span> //不会影响原有的布局;

div属于块级(block)元素,一定会独占一行的元素,比如h1/p;
span属于行级(inline)元素,可以和其它元素在同一行中显示,比如a;

如:
aaa<div align="right">bbb</div>ccc
这样将bbb分成一组,但是对bbb附加了一个换行效果;

如:
ggg<span>kkk</span>jjj

实例:

<!版本信息>
<html>
    <head>
        <title>test</title>
        <meta http-equiv="content-type" content="text/html;charset=utf-8"/>
    </head>
    <body>
        <h1>Java 语言基础
        <span style="color:red;">&lt;Day03&gt;</span><h1>
        <h2>1 XXXXX</h2>
        <h3>1.1 XX</h3>
        <p>XXXXXXXXXXXXXXXXXXXXXXX</P>
        <h3>1.2 XX</h3>
        <p>XXXXXXXXXXXXXXXXXXXXXXX</P>
        <h3>1.3 XX</h3>
        <p>XXXXXXXXXXXXXXXXXXXXXXX</P>
    </body>
</html>


(2)图像和连接;

a.图像标记 ; 
<img src="c:/a.jpg"/>  //需要本地文件的支持,不适合在html中使用;
<img src="a.jpg"/>     //相对路径;当前html盘符中的a.jpg文件;
<img src="http://aaaa/a.jpg"/>  //绝对路径;

如果不定义图片的大小,则会按照图片的原图大小显示;为了按照原有比例自动缩放,建议只指定一边的距离;
如:
<img src="images/cat.jpg" width="100"/>

b.超级链接;
<a href="...">click me</a>
<a href="users/login.html"></a> //相对路径;
<a href="http://www.xxx...."></a> //绝对路径;
注意:默认为在原有页面打开新的页面,可以用a的另一个属性;
如:
<a href="demo.html" target="_blank">  //在新页面中打开;
<a href="demo.html" target="_self">  //在原有页面中打开;

c.不同页面之间的跳转; 如上;

d.同一个页面的不同部分之间的跳转;
<a href="#">回到页面的顶端</a>  //回到页面的顶端;

e.锚点的定义和使用:
实现选择性的在页面的不同位置之间跳转;
<a name="xxx"></a> 设置锚点位置;
<a href="#xxx">To xxx</a> 跳转到锚点;


(3)列表;
有序列表(ordered list)<ol>
无序列表(unordered list)<ul>
列表项    (listed item)<li>; 用于指示具体的列表内容;

如:
<ul>
    <li>aaa</li>
    <li>bbb</li>
    <li>ccc</li>
<ul>

注意:经常会使用嵌套的列表实现导航菜单;

如:

<ul>
    <li>
        <a name="">电器</a>
        <ul>
            <li><a>小家电</a></li>
            <li><a>大家电</a></li>
        </ul>
    </li>
    <li>
                           图书
        <ol>
            <li>考试用书</li>
            <li>儿童读物</li>
        </ol>
    </li>
</ul>


(4)表格;
显示网格数据,布局; 
a.表格的基本结构; (自上而下,从左到右);
<table>  //默认情况下不显示边框; 且表格的宽度为自适应,也就是根据其中的内容而变化;
<table border="" width="" height="">  //设置其边框的粗细,以及规定其宽度和高度;
注意:而在规定了其整体的长度以及高度之后其中自单元格的比例分配还是按照其中内容来自适应;如果要解决这样的
问题只能在td中再次对其单元格进行大小规定;同一行和同一列是共享所设置的高和宽,所以只需设置第一个td即可;

<tr>---table row
<td>---单元格(table defination)

如:
<table border="1" width="200" height="100" cellspacing="10" cellpadding="5">  
//设置边框为1; 单元格间距为10; 为0时表格边框变为单实线; 设置单元格中文字与单元格边框间距为5;
	<tr style="color:red;text-align:center"> //注意:text-align是CSS中样式;
		<td width="60" height="30">a</td>
		<td>b</td>
	</tr>
	<tr>
		<td align="right" valign="top">c</td>  //valign(vertical align)是规定垂直方向的对齐可以设置为top/bottom;
		<td>d</td>
	</tr>
</table>

b.常用的属性;
table: border/width/height/cellspacing/cellpadding ;
td: width/height/align/valign ;

e.为表格添加标题;
<table>
<caption>表-1</caption> (默认在整个表的宽度范围内居中,但是处于表格之外;)
......
</table>

f.行分组;
thead/tbody/tfoot;
复杂分组时可以有多个tbody;
为了让大表格(table)在下载的时候可以分段的显示,就是说在浏览器解析HTML时,table是作为一个整体解释的,使用tbody可以优化显示;如果表格很长,用tbody分段,可以一部分一部分地显示,不用等整个表格都下载完成;
下载一块显示一块,表格巨大时有比较好的效果;tbody包含行的内容下载完优先显示,不必等待表格结束;另外,还需要注意一个地方;表格行本来是从上向下显示的;但是,应用了thead/tbody/tfoot以后,就"从头到脚"显示,不管你的行代码顺序如何;
也就是说如果thead写在了tbody的后面,html显示时,还是以先thead后tbody显示;

thead和tfoot只能出现一个,如果定义多个除了第一个,其他的将不会按照head->body->foot的顺序排列,而是直接按照文本中的顺序;

th
<th>和<td>一样,也是需要嵌套在<tr>当中的,<tr>嵌套在<table>当中,<th>中显示的文字不光是粗体,还是居中的;

<table>
	<thead>
		<tr>
			<th width="60" height="30">a</th>
			<td>b</td>
		</tr>
	</thead>
	<tfoot>
		<tr>
			<td width="60" height="30">a</td>
			<td>b</td>
		</tr>
	</tfoot>
	<tbody style="color:red;">
		<tr>
			<td width="60" height="30">a</td>
			<td>b</td>
		</tr>
		<tr>
			<td width="60" height="30">a</td>
			<td>b</td>
		</tr>
	</tbody>
</table>

g.不规则的表格;
需要设置td的跨行或者跨列;
colspan = "3" ---横跨;
rowspan = "3" ---竖跨;

如:
<table border = "1" width="200">
	<tr>
		<td colspan="2">a</td>
	</tr>
	<tr>
		<td width="60">b</td>
		<td>c</td>
	</tr>
</table>

<table border = "1" width="200">
	<tr>
		<td rowspan="2">a</td>
		<td>b</td>
	</tr>
	<tr>
		<td>c</td>
	</tr>
</table>


(6)表格的嵌套; table写在td里;

实例:

<p>XXX按要求显示一个表,名为表-1;</p>
<table border="1" width="400" height="150" align="center">
	<caption>表-1</caption>
	<tr style="background-color:silver;">
		<td width="200">额度</td>
		<td width="100">税率</td>
		<td>扣除数</td>
	</tr>
	<tr>
		<td>11</td>
		<td>22</td>
		<td>33</td>
	</tr>
	<tr>
		<td>44</td>
		<td>55</td>
		<td>66</td>
	</tr>
	<tr>
		<td colspan="3">aa</td>
	</tr>
	<tr>
		<td colspan="3">
			<table>
				<tr>
					<td width="100">计算公式为： </td>
					<td>XXXX </td>
				</tr>
			</table>
		</td>
	</tr>


</table>


(6)注释;
<!--注释-->


(7)表单;
用来承载表单上的元素(如文本框,按钮等),数据的录入与交互,提交到服务器端;
<form action="login.jsp" method="post"></form>

表单的两个基本部分:
-实现数据交互的可见页面元素,比如文本框或按钮;
-提交后的表单处理;

界面元素:
-使用<form>元素创建表单;
-在<form>元素中添加其它表单可以包含的控件元素;

表单上的元素:
-input:输入框;
<input type="text"/>
<input type="password"/>
<input type="radio"/>     //互斥的单选效果;
<input type="checkbox"/>  //多选;
<input type="submit"/>    //提交; 有一个刷新页面的动作;并以名值对的方式提交表单中数据到服务器端;
<input type="reset"/>     //重置; 不会刷新页面,但是会让表单中的元素回到初始值;
<input type="button"/>    //自定义按钮;
<input type="file"/>      //选择本地的文件,用于上传文件;
<input type="hidden"/>    //隐藏域;常用来记载不希望用户看到的关键数据,但是这些数据需要被提交给服务器;
                          //但是不推荐放敏感的私密数据;

如:
<body>
 	<form action="" method="">
		用户名： <input type="text" value="游客"/>
		<br/>
		密码：     <input type="password"/>
		<br/>
		性别： <input type="radio" name="sex" value="f"/> 女生 
		    <input type="radio" name="sex" value="m"/> 男生
		<br/>
		//name属性相同则代表处于同一个radio实现互斥;
		城市： <input type="checkbox" name="city" value="beijin"/> 北京
		    <input type="checkbox" name="city" value="shanghai"/> 上海
		    <input type="checkbox" name="city" value="nanjin"/> 南京
		<br/>
		爱好： <input type="checkbox" name="hobby" value="swimming"/> 游泳
	        <input type="checkbox" name="hobby" value="ball"/> 打球
		    <input type="checkbox" name="hobby" value="XX"/> XX
		<br/>
		<input type="submit" value="s"/>
	</form>
</body>


input元素的属性:
-name: 提交数据; (当在表单中使用了submit元素,只有拥有name属性的元素中的value会被提交);
-value:文本框中的初始值(用于修改原有信息时使用);
                       单选或多选时所提交的内容; 相当于将所选项的name/value用键值对的形式提交;


标签元素;
<label>文本</label>
for属性:表示与该元素相联系的控件的ID值;
作用是将文本与控件联系在一起后,单击文本,效果就同单击控件一样;

如:
<input type="checkbox" name="chkHid" id="chkHid"/> //不规定checkbox的vallue值,则如果选中将以name/on的形式传输;
<label for="chkHid">不要公开我的信息</label>


例子:

<body>
	<h1>增加管理员</h1>
	<form action="" method="">
		<table >
			<tr>
				<td>姓名: </td> 
				<td><input type="text" name="name" value="4565"/> </td> 
				<td>10个字符以内 </td> 
			</tr>
			<tr>
				<td>密码: </td> 
				<td><input type="password" name="pwd"/> </td> 
				<td>10个字符以内 </td> 
			</tr>
			<tr>
				<td>性别：  </td> 
				<td>
					<input type="radio" name="sex" value="f"/> 女士
				    <input type="radio" name="sex" value="m"/> 男士
				</td> 
				<td></td> 
			</tr>
			<tr>
				<td>角色：  </td>
				<td>
					<input type="checkbox" name="role" value="sa" id="role1"/> 
					<label for="role1">超级管理员</label>
				    <br/>
				    <input type="checkbox" name="role" value="na" id="role2"/> 
				    <label for="role2">普通管理员</label>
				</td> 
				<td>至少选择一个</td> 
			</tr>
			<tr>
				<td>头像: </td>
				<td>
					<input type="file" name="userimg"/> 
				</td>
				<td></td>
			</tr>
			<tr>
				<td></td>
				<td>
				<input type="submit" value="保存"/>
				<input type="reset" value="重填"/>
				</td>
				<td></td>
			</tr>
		</table>		
			xxx<input type="hidden" name="id" value="111"/>xxx
	</form>

</body>


选择元素;
<select name="number">  //下拉式,默认size为1;不要忘记在select标签中给出统一的name,在option中给出name无效;
	<option value="1">a</option>
	<option value="2">b</option>
	<option value="3">c</option>
	<option value="4">d</option>
</select>

<select name="number" size="3"> //size属性为显示选项的个数,size!=1时为列表式(无法下拉但是可以滚动查看选择);
	<option value="1">a</option>
	<option value="2">b</option>
	<option value="3">c</option>
	<option value="4">d</option>
</select>


多行文本域;
<textarea rows="5" cols="20">
</textarea>


表单中控件的分组;
<fieldset>字段集合;
<legend>标题</legend> //整个fieldset的内容从这里起将被定义在legend边框中;
</fieldset>


(8)浮动框架;
<iframe src="其他页面的URL"></iframe>
在当前页面上嵌入其他页面,常用于广告页面;



CSS (Cascading Style Sheets); (级联/层叠 样式表);
作用是定义网页的外观;可以实现内容和表现形式的分离;
html的样式属性对于每一个元素来说虽然功能相同但是声明方法可能会有区别,而CSS是统一化的;

<textarea cols="" rows=""></testarea>
利用html的样式属性修饰一个元素;

<textarea style="width:200px;height:100px;"></testarea>
利用CSS来修饰一个元素;


CSS的基础语法:
样式的属性名称1:值1;样式的属性名称2:值2;


CSS的定义方式:
一:内联方式,样式定义在html元素的style的属性里,重用性和可维护度不好;

如:
...
<p style="color:red;background-color:silver;">段落文本</p>
<h2>2号标题</h2>
...

二:内部样式表;将CSS样式定义在head里的style元素中;
语法注意:元素名称加大括号中定义的样式,称为样式选择器;
...
<head>
	<title>标题</title>
	<style type="text/css">  //纯文本的CSS类型;
	p
	{
		color:red;background-color:silver;
	}
	h2
	{
		color:green;background-color:silver;
	}
	</style>
</head>
<body>
	<p>段落文本1</p>
	<p>段落文本2</p>
	<h2>标题</h2>
</body>

三:外部样式表; 将CSS样式定义在单独的.css文件中,在html页面中用link引入;

如:
建立一个新的文件demo.css;

h3
{
height:300px;
border:1px solid red;
}

利用link元素在需要用到此样式的html的head中引用.css文件;
<link href="demo.css" type="text/css" rel="stylesheet"/>

注意:css的注释方式为:/*xxx*/
/*所以在html中用内部样式表定义样式时的注释也是这种方式;


css特征;
级联/层叠 样式表的含义;
就近优先级机制决定了相同的样式取优先级高的来设置,优先级:内联>内部/外部(内部和外部的优先级由他们在html定义/引用的先后位置决定);
而不同的样式取并集;
还需要注意的是css样式表的覆盖机制,以最后一次定义的为准,覆盖之前的重复部分,所以在一个外部css样式表的最后一行添加一种样式,则相当于
重新定义了一遍此样式;

如:

<style type="text/css">
	div{color:green;border:1px solid red;}
</style>
<body>
	<div style="color:red;font-size:10pt;">text</div> 
	<!--pt：磅(绝对单位),px:像素(相对单位)-->
</body>



css选择器的定义;

(1)元素选择器,如h1/div/p--以html中标记的名称为一类元素定义样式;
但是这种选择器不便于在同一种元素下细分不同的样式;
其次,它也无法实现多种不同元素以相同的样式共同声明;

(2)样式类;
css: .name{}
html: <元素 class="name">

如:

<style type="text/css">
.s1
{
	font-size:20pt;
}
</style>

<p class="s1">p text</p>
<span class="s1">span text</p>


(3)分类;

CSS: 元素名.name{} //限制了只能是目标元素类型才能使用声明的css样式类;
html: 

如:

<style type="text/css">
input.txt
{
	width:150px;
}
</style>

<input class="txt">


(4)ID选择器;
对页面上某个元素的唯一定义;

css: #idvalue{}
html: <元素 id="idvalue">

如:

#pageTitle
{
	font-size:30pt;
	color:orange;
}

<span id="pageTitle">我的购物车</span>


(5)派生选择器;
利用html元素的层次关系,选中某种结构下的元素;
css: 
html:

如:

ul li a
{
	color:red;
}

<ul>
	<li><a href="#">aaa</a></li>
	<li><a href="#">bbb</a></li>
</ul>


(6)选择器分组;
为多个选择器定义相同的样式部分;

如:
input,a.link,#title,.s1 {XXX}


(7)伪类;
同一个元素在不同情况下有不同的状态;
:link--未访问过的;
:visited--访问过;
:hover--悬浮,悬停;
:active--激活(按下);

css: 某种选择器:link/hover...

如:

a.link1:hover
{
	font-size:20pt;
}
a.link1:active
{
	color:green;
}



css各种样式属性;

css单位;

-尺寸:
%--百分比;
in:英寸;
cm:厘米;
mm:毫米;
pt:磅(1pt等于1/72英寸)
px:像素(计算机屏幕上的一个点)
em:1em等于当前的字体尺寸,2em等于其两倍;

-颜色:
rgb(x,x,x): RGB值,如rgb(255,0,0);
rgb(x%,x%,x%): RGB百分比,如rgb(100%,0%,0%);
#rrggbb:十六进制,如#ff0000; 注:每两位代表一个单色的色值;
#rgb:简写的十六进制数,如#f00; 注:如果上一种情况下单表单色的两位相同就可以用简写;

尺寸属性;
width/height;

overflow:当内容溢出元素框是如何处理;
visible; 默认值,溢出也能显示;
hidden;  隐藏溢出内容; 
scroll;  溢出/非溢出情况都需要通过滚动条查看;
auto;    只有溢出发生才会需要通过滚动条来查看;

如:

<p style="width:px;height:50px;border:1px solid red;overflow:scroll;">
testesttesttesttesttesttesttesttest
</p>


边框属性;

简写方式:
border: width/style/color;

单边定义:
border-left/right/top/bottom:width/style/color;

如:
border-left:2px dotted blue;
border-left-width:2px; //也可以单独定义一个属性的一个样式;

注意:
table中的border属性不包括其内边框的定义,需要在td中自行定义(tr中无效),但是类似border-collapse这样的属性在table中定义后整个table中的所有边框都生效;


框模型(Box Model);
定义了元素框处理元素内容,内边距(padding),边框和外边距(margin)的方式;
注:内边距和外边距是相对的,根据所选的边框不同而改变;
width和height指的是内容区域的宽度和高度,增加内边距,边框和外边距不会影响内容区域的尺寸，,
但是可能会增加元素框的总尺寸;

margin:20px; 四个方向;
margin-left:10px; 单方向;
margin-top:20px;
margin:10px 20px 30px 40px; 单方向简写,顺序为:top/right/bottom/left(瞬时针);
注:上面的情况如果上下边距相同/左右边距相同,可以用margin:10px 20px;(值复制);

margin:10px auto;--居中显示; 相当于设置上下为10px,左右为auto;系统将自动取居中的尺寸;

padding:与margin同理;
注:实际显示中总是先满足内部元素与外部元素上部的尺寸要求和左侧的尺寸要求(在范围内则照要求显示,超出范围则外部元素被拉伸),
其次是右侧和底部(在范围内则保持现有内部元素和外部元素的位置关系,外部元素不会因为尺寸要求而缩小,在范围外则外部元素被拉伸)
所以定义边距尺寸不当可能会将外边框顶开(撑大);

如:

<div style="width:200px;height:200px;border:1px solid black;">
	<div style="width:100px;height:100px;margin:20px;padding:5px 15px;border:1px solid black;">
		<div style="width:50px;height:50px;border:1px solid black;"></div>
	</div>
</div>


实例: (NetCTOSS)
...
<head>
<title>标题</title>
<link href="admin.css" type="text/css" rel="stylesheet"/>
</head>
<body>     //注意:body默认有一定的内边距;
	<!--header部分-->
	<div id="header"></div>
	<!--navi部分-->
	<div id="navi"></div>
	<!--主要数据部分-->
	<div id="main">
		<!--操作按钮部分-->
		<div id="operater"></div>
		<!--数据表格部分-->
		<div id="dataList"></div>
		<!--分页页码部分-->
		<div id="pages"></div>
	</div>
	<!--页脚部分-->
	<div id="footer"></div>
</body>



admin.css文件:

//定义公共样式;
body
{
	padding:0px;
	margin:0px;
}
div
{
	border:1px solid black;
	margin:0px auto;
}

//分别定义;
#header,#footer{width:960px;}
#operater,#dataList,#pages{width:910px;}

#header
{
	height:61px;
}
#navi
{
	width:100%;
	height:91px;
}
#main
{
	width:950px;
	height:410px;
	border:5px solid #8ac1db;
}
#footer
{
	height:50px;
}
#operater
{
	height:30px;
}
#dataList
{
	height:340px;
}
#pages
{
	height:28px;
}


继承:
子元素可以使用父元素的某些样式--一般与字体相关和背景相关的可以被继承;

如:
<body style="color:red;">
	111
	<p>text</p>
</body>



背景:

背景色: background-color:颜色;
背景图片: background-image:url(地址); //如果图片比引用图片元素的范围小,则默认为平铺;
				      //如果图片大于引用图片元素的范围,则图片将不会显示完全;
对背景图片设置其显示方式:
	  background-repeat:repeat(默认)/no-repeat/repeat-x(仅水平方向平铺)/repeat-y(仅垂直方向平铺)
背景图片的位置:
 	  background-position:x y;
背景图片的附着方式(元素内容与背景同步移动,或者背景保持静止):
	  background-attachment:scroll(默认)/fixed;

如:
<html>
<head>
<title>标题</title>
<link href="demo.css" type="text/css" rel="stylesheet"/>
</head>
<body>     
	<div style="height:200px; border:1px solid black;background-image:url(1.jpg);background-repeat:no-repeat;background-position:30px 50%;"></div>
</body>
</html>



实例: (NetCTOSS)

在css中相关定义处添加:

body
{
	background-image:url(images/body_bg.png);
	background-repeat:repeat-x;
}
#header
{
	background-image:url(images/top_bg.png);
	background-repeat:no-repeat;
}
#navi
{
	background-image:url(images/navigation.png);
	background-repeat:repeat-x;
}
#main
{
	background-color:#e8f3f8
}



文本:

字型/字体:font-family value1,value2,value3; //优先用第一个,如果找不到则用第二个..以此类推;
如:
font-family:new times,times;
字体颜色:color:value;
字体加粗:font-weight:normal/bold;
字体大小:fontsize:value;
文本排列:text-align:left/right/center;
文本修饰(是否有下划线):text-decoration:none/underline;
文本的首行缩进:text-indent:value;
文本行高:line-height:value; //行高不会改变字体的大小;

注意:
1.vertical-align:top/middle/bottom;只能在td元素中使用;
2.td标签中使用的align/valign的属性与CSS中的text-align/vertical-align相同;
3.在tr中使用以上属性会直接影响tr中的所有td;
4.在table标签中定义align/valign属性改变的是整个table的显示位置;




实例: (NetCTOSS)

在html相关处添加:

<div id="header">
	<a href="#">[退出]</a>
</div>

<div id="footer">
	<p>页脚的内容p1页脚的内容p1页脚的内容p1</p>
	<p>页脚的内容p2页脚的内容p2</p>
</div>

在css相关处添加:

body
{
	color:white;
	font-size:10pt;
	font-family:times;
}
#header a
{
	color:white;
	text-decoration:none;
	line-height:61px;
	font-size:10pt;
	margin-right:40px;
}
#header a:hover
{
	font-weight:bold;
	text-decoration:underline;
}
#header
{
	text-align:right;
}
#footer p
{
	margin:0px;
	padding:0px;
	text-align:center;
	line-height:25px;
}



表格特有样式;

垂直对齐:
vertical-align:top/middle/bottom;
边框合并(外边框与内边框合并):
border-collapse:separate(默认)/collapse;
边框之间的边距:
border-spacing:value; //border-spacing属性设置相邻单元格的边框间的距离(仅用于"边框分离"模式);某些版本的IE浏览器不支持此属性;

如:
<table style="border-collapse:collapse;">
	<tr>
		<td>a</td>
		<td>b</td>
	</tr>
	<tr>
		<td>c</td>
		<td>d</td>
	</tr>
</table>



实例: (NetCTOSS)

在html相关处添加:

<div id="dataList">
	<table id="dataTable">
		<tr class="tableHeader">
			<td>
				<input type="checkbox"/>
				全选
			</td>
			<td>
				管理员ID
			</td>
			<td>
				姓名
			</td>
			<td>
				拥有角色
			</td>
			<td></td>
		</tr>
		<tr>
			<td>
				<input type="checkbox"/>
			</td>
			<td>
				1
			</td>
			<td>
				张三
			</td>
			<td>
				超级管理员
			</td>
			<td>
				<input type="button" value="修改"/>
				<input type="button" value="删除"/>
			</td>
		</tr>
	</table>
</div>


在css相关处添加:

//数据表格部分;
#dataTable
{
	width:910px;
	background-color:white;
	color:black;
    text-align:center;
    border:1px solid #ccc;
    border-collapse:collapse;
}
#dataTable td
{
	border:1px solid #ccc;
	height:32px;
}
tr.tableHeader
{
	background-color:#fbedce;
	font-weight:bold;
	height:40px;
}
#dataTable tr:hover
{
	background-color:#f7f9fd;
}
#dataTable tr.tableHeader:hover
{
	background-color:#fbedce;
}


选择器的优先级:
范围越小,优先级越高;
id>分类>元素;

如:
<html>
<head>
<style type="text/css">
#p1 { color:blue;}
.green { color:green;}
p{ color:red;}
p{ color:black;}
</style>
</head>
<body >
<p id="p1" class="green">
这是一个段落,它的颜色为蓝色
</p>
</body>
</html>

先考虑选择器的优先级,优先级相同最后设置的之前设置的优先级高;


cursor:光标;
该属性定义了鼠标指针放在一个元素边界范围内时所用的光标形状;
default/pointer/help/wait/crosshair;;


浮动;
页面默认情况下使用流布局模式;

当设置了元素浮动,此元素将脱离原有的布局,不再保留原有的位置,后续元素会补上它的位置;
浮动定位是指:
将元素排除在普通流之外;
将浮动元素放置在包含框的左边或者右边;
浮动元素依旧位于包含框之内;

浮动框可以向左或者向有移动,直到它的外边缘碰到包含框或者另一个浮动框的边框为止;


float:none/left/right;

清除浮动元素带来的影响(使其后的元素不会补上它原有的位置);
clear:left/right/both;

如:利用float将三个框在同一行中显示;
<style>
	div
	{
		border:1px solid red;
	}
</style>
<div style="float:left;">1</div>
<div style="float:left;">2</div>
<div style="float:left;">3</div>
<p style="clear:left;">321</p>

关于浮动的设置于清除具体参考:经验分享：CSS浮动(float,clear)通俗讲解这篇文章;


显示---元素的显示方式;
每个html元素都有其默认的显示方式(块级,行内),
display:
none--不显示,经常结合js代码实现动态显示效果;
如:
aaa<span style="display:none;">bbb</span>ccc

block
注意:行内元素的高度和宽度定义无效,所以需要定义为块级元素;
如:
<span style="display=block;width:50px;height:50px;border:1px solid red;
background-color:silver;">

inline
如:
<div style="display:inline;">aa</div>
<div style="display:inline;">bb</div>


列表的样式;
list-style-type:none/disc（实心圈）/circle（空心圈）/...;
list-style-image:url(); //需要将上一个样式先设置为none;


定位;
(1)默认情况下是流模式;
(2)使用float:修改布局和定位;
(3)使用position:static(默认)/relative(相对定位)/absolute(绝对定位);
 相对定位的偏移属性: top/left/right/bottom;
(4)堆叠顺序;
z-index;  //z轴的位置决定了元素处于原有层的上面还是下面; 原有层z-index:0;

相对定位:
(1)元素仍保持其之前的形状;
(2)原本所占用的空间仍保留;
(3)根据自身原本的位置发生相对移动,与父元素位置无关(top就是从原本位置向下偏移,left就是向右偏移);

如:
<body style="margin:0;padding:0;">
	<div style="height:50;width:50;background-color:red;"></div>
	<div style="height:100;width:120;background-color:blue;position:relative;top:10px;left:20px;z-index:-1;"></div> //偏移量可以为负数,被定为的元素会覆盖在原有层元素之上;
	</div>
	<div style="height:50;width:50;background-color:red;"></div>
</body>

绝对定位:
(1)将元素的内容从普通层中完全移除(不会保留原有的位置);
(2)并使用偏移属性来固定该元素的位置;
-相对于最近的已定位的父元素;
-如果元素没有已定位的父元素(使用position:relative/absolute),那么它的位置相对于最初的包含块,比如body元素;

如:
<body style="margin:0px;">
	<div style="position:relative;top:10px;left:20px;background-color:red;height:100;width:100;">
	<div style="position:absolute;top:10px;left:20px;background-color:blue;height:100;width:100;"></div> 
	</div>
</body>




二:html文档的创建;

......



*/


