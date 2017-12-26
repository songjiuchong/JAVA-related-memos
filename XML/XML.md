XML

package prototype;

public class XML {

	public static void main(String[] args) {

	}
}


/*

XML;
是一种基于文本的通用的数据保存格式;

a.通用;
  使用xml格式保存的数据,可以被所有的语言支持,与平台无关;
 比如,我们可以使用java语言将一个对象(Point)保存到一个xml文件里,另外一个用
 c语言写的程序,可以很方便地读取这个xml文件

b.数据保存格式;
 xml使用标记加内容的方式来保存数据;
   比如:
   <point>
     <x>10</x>
     <y>20</y>
   </point>
 
 
xml的发展历史:
   先有html(超文本标记语言);
   根据html进一步改造产生了xml(可扩展的标记语言),使用范围远大于html;
   所以xml使用了html的基本语法,但是更严格,并且可扩展;
   
xhtml:对html按照xml语法来加以约束; 目前html语法都按照xhtml的规则来进行;


元素;
(1)两个标记之间的内容统称为元素;
(2)一个xml文档,只有一个根元素;
(3)元素可以嵌套;
(4)元素必须有开始标记和结束标记,如果是空元素,可以简写为<标记/>;
        比如: <a><a/>是一个空元素,可以简化为<a/>;
(5)元素可以有属性;
        比如:<point type = "normal">
        </point>;
        
属性;
(1)属性用来表示元素的特性;
(2)属性必须用引号括起来;
(3)属性必须写在开始标记中;

实体;
   <question>
     1+2<3?       //这里的<会被解析器视为一个没有结束标记的开始标记,会解析出错;
   </question>
   
正确的写法:
   <question>
    1+2 &lt; 3?     lt表示less than,gt表示greater than,也就是"<,>"的实体;
   </question>

如:
<?xml version="1.0" encoding="utf-8"?> //首行为xml规范;
<question>
   1+2&lt;3?
</question>

有一些特殊的字符,如:<,>,&,'," xml解析器有特殊的含义需要使用相应的字符来代替,那么这些用来代替这些字符的字符
被称为实体;

常见的实体：
<   &lt;
>   &gt;
&   &amp;
'   &apos;
"   &quot;


CDATA段;
当语句中包含过多的需要解析的字符;
<![CDATA[不需要解析器解析的内容]]>

如:

<questions>
   <question>
      1+2&lt;3?
   </question>
   <question2>
      <![CDATA[1+2<3&&2+4>5]]>
   <question2>
</questions>


xml大小写敏感,建议标记用小写;如果有一个标记有多个单词,建议用-连接;
如:
<cust-name>;


xml解析
主要的解析方式:
a.sax;  //simple access for xml; sun公司提供的底层的解析XML的API,多用于开发其他解析工具;有点是占用系统资源较少但是,代码过于复杂;
b.dom;  //dom解析需要将整个xml文档读入内存,对系统资源的占用比较大;代码也比较繁琐;
c.一些开源的工具;  //DOM4J,Digest,pull等等; 相对更加方便;

开源网站;
www.apache.org   
www.github.com
www.sourceforge.org

(1)如何将一个java对象转换成一个xml文档; 
   step1:将dom4j的jar文件加到Build path中;
   step2:先设计好xml文档的结构;
   一般来说,一个java对应一个元素,简单的类型,比如string等,既可以作为属性,也可以作为子元素来设计,复杂类型只能作为子元素来设计;
 例:<Student>是一个元素,他的java属性id可以写在标题中作为其属性,因为id是基本类型的变量,
 而如果要将一个符合类型的属性写在标题中是无法实现的,因为符合类型的属性很可能本身还含有子元素;
   注意:一个XML文档只能对应一个(根)元素;
   step3:将xml文档的内容转换成内存中的一棵树;
   step4:输出;
       比如,可以将xml文档的内容输出到文件或者通过网络发送出去;
   
实例:
   import org.dom4j.Document;
   import org.dom4j.DocumentHelper;
   
   public static void main(String [] args)throws FileNotFoundException
   ,UnsupportedEncodingException
   ,IOException{
      Document doc = DocumentHelper.createDocument(); //在内存中创建一颗空的树;
      Element root = doc.addElement("point");  //添加一个根节点"point";
      root.addAttribute("type","abc");   //为元素添加属性,参数为属性名称和属性的值;
      Element x = root.addElement("x");  //为point添加一个子节点;
      Element y = root.addElement("y");  //为point添加一个子节点;
      x.setText("10");
      y.setText("20");
      OutputStream ops = new FileOutputString("c:/xmltest/point.xml");
      OutputFormat format1 = OutputFormat.createCompactFormat();  //制定紧凑型格式(默认格式),也就是以一行的形式显示元素;
      OutputFormat format2 = OutputFormat.createPrettyPrint();   //制定竖型结构格式,更加便于阅读;
      XMLWriter writer = new XMLWriter(ops,format);  //指定了输出格式的情况下这里就需要用第二个参数了;
      writer.write(doc);  //在指定位置创建了一个xml文件;
   }
   

例:将一个JAVA端的订单对象通过XML文件传输给另一个端;
public class Order{
   private String custType;
   private String custName;
   private String flight;
   private String time;
   private String dest;
   public String getCustTyper(){
      return custYype;
   }
   ......
}

XML design:

<order custType="">
   <cust-name></cust-name>
   <flight></flight>
   <time></time>
   <dest></dest>
</order>

public class OrderToXML throws Exception{
   
   public static void orderToXml(Order order,OutputStream ops){
      Document doc = DocumentHelper.createDocument();
      Element root = doc.addElement("order");
      root.addAttribute("custType",order.getCustType());
      root.addElement("cust-name").setText(order.getCustName());
      root.addElement("flight").setText(order.getFlight());
      root.addElement("time").setText(order.getTime());
      root.addElement("dest").setText(order.getDest());
      OutputFormat format = OutputFormat.createPrettyPrint();
      XMLWriter writer = new XMLWriter(ops,format);
      writer.write(doc);
   }
   
   public static void main(String [] args){
      OrderToXML o = new OrderToXML();
      Order order = new Order();
      OutputStream ops = new FileOutputStream("c:/xmltest/orderproject");
      o.orderToXml(order,ops);
   }
}
   

例:将多个订单对象转换为一个xml文档,输出;
public void OrdersToXml(List<Order> list,OutputStream ops){
   Document doc = DocumentHelper.createDocument();
   Element orders = doc.addElement("orders");
   for(Order temp:list){
      int no = 1;
      Element order = orders.addElement("order"+no++);
      order.addAttribute("custType",temp.getCustType());
      order.addElement("cust-name").setText(temp.getCustName());
      order.addElement("flight").setText(temp.getFlight());
      order.addElement("time").setText(temp.getTime());
      order.addElement("dest").setText(temp.getDest());
   }
   OutputFormat format = OutputFormat.createPrettyPrint();
   XMLWriter writer = new XMLWriter(ops,format);
   writer.write(doc);
}


(2)如何将一个xml文档转换成一个java对象;
   step1:将dom4j的jar文件加到Build path中;
   step2:读取xml文档的内容,转换成内存中的一棵树;
   step3:读取树中的节点数据,然后创建相应的java对象;

实例:

public class XMLToOrder throws Exception{
   public Order xmlToOrder(InputStream ips){
      Order order = new Order();
      SAXReader reader = new SAXReader();
      //read方法,读取xml文档,将其转换成内存中的一棵树;
      Document doc = reader.read(ips);
      //找到根节点;
      Element root = doc.getRootElement();
      order.setCustType(root.attribute("custType").getText());
      //order.setCustType(root.attributeValue("custType"));这条语句和上一条语句等价;
      order.setCustName(root.element("cust-name").getText());
	  //order.setCustName(root.elementText("cust-name"));这条语句等价于上一条语句;
	  order.setFlight(root.elementText("flight"));
	  order.setTime(root.elementText("time"));
	  order.setDest(root.elementText("dest"));
      return order;
   }
   
}

例子:写一个方法,可以将一个xml文件转化为一个orders的List集合;

public class XMLToOrder throws Exception{
   public List<Order> xmlToOrders(InputStream ips){
      List<Order> orders= new ArrayList<Order>();
	  Order order = new Order();
	  SAXReader reader = new SAXReader();
	  Document doc = reader.read(ips);
      list<Element> temp = doc.selectNodes("/orders/order");
      //int noe = temp.size();
      for(Element temporder:temp){
         order.setCustType(temporder.attributeValue("custType"));
         order.setCustName(temporder.elementText("cust-name"));
		 order.setFlight(root.elementText("flight"));
		 order.setTime(root.elementText("time"));
		 order.setDest(root.elementText("dest"));
		 orders.add(order);
      }
	  return orders;
   }
}



dtd(document type definition);
是一种文档,规定了xml文件中可以出现哪些元素,元素可以有哪些属性,属性值可以是哪些内容此外还定义了元素出现的顺序,次数等等;
也就是说,dtd定义了xml文档的结构;

总结其作用是:
a.定义xml文档的结构;
b.解析器可以利用dtd对xml的正确性进行验证;
c.利用dtd可以形成行业规范;

DTD分别用ELEMENT, ATTLIST, ENTITY, #PCDATA, #CDATA等来描述;

关于实体
<!ENTITY 实体名称 "实体的值">, 如:
<!ENTITY writer "Bill Gates">
<!ENTITY copyright "Copyright W3School.com.cn">
则在xml中可以这样引用它们:
<author>&writer;&copyright;</author>

值:
#REQUIRED	属性值是必需的;
#IMPLIED	属性不是必需的;
#FIXED value	属性值是固定的;


实例:

<?xml version="1.0" encoding = "utf-8"?>

<!DOCTYPE orders[  //<!代表了dtd声明的开始;orders参数表示根元素必须是orders;
   <!ELEMENT orders (order+)>  //[开始描述元素; <!代表对一个元素描述的开始; 参数中order+表示orders元素中可以出现1个或多个order子元素(此处+*等的使用与正则表达式相似);
   <!ELEMENT order (ust-name,flight,time,dest)>  //对order元素描述,参数代表必须出现括号中的4种子元素;
   <!ELEMENT cust-name (#PCDATA)>  //表示此元素中只能存在文本作为其内容,不能再包含其它子元素;
   <!ELEMENT flight (#PCDATA)>
   <!ELEMENT time (#PCDATA)>
   <!ELEMENT dest (#PCDATA)>
   <!ATTLIST order custType CDATA #REQUIRED>  //定义属性,表示为order元素的属性,type为属性名,CDATA(不同于描述元素文本#PCDATA)表示属性值为文本,#REQUIRED表示这个值是必须出现的;
]>

<orders>
   <order custType = "VVIP">
      <cust-name>tom</cust-name>
      <flight>CA1888</flight>
	  <time>6:00</time>
	  <dest>beijing</dest>
   </order>
</orders>


xpath;
可以把xml看成是数据库,那么xpath就相当于sql语句;
所以xpath就是面向xml的sql;

使用步骤:
step1:将xml转换为内存中的一棵树,SAXReader reader = new SAXReader();
      Document doc = reader.read(ips);
step2:调用Document对象的selectNodes方法,doc.selectNodes(String path);
      利用三种查询方法(如下)来找到目标元素;

实例:

public static void main(String [] args)throws Exception{
   SAXReader reader = new SAXReader();
   Domument doc = reader.read(new FileInputStream("c:/xmltest/orders.xml"));
   //查询方法一:
   List<Element> elements = doc.selectNodes("/orders/order/cust-name");  //查询内存中树的节点(元素),返回所有符合条件的元素放入一个List集合;
   for(Element ele:elements){
      System.out.println(ele.getText());
   }
   //查询方法二:
   List<Element> elements = doc.selectNodes("/orders/order[1]/cust-name");  //取得第一个order元素中的cust-name元素;
   for(Element ele:elements){
      System.out.println(ele.getText());
   }
   //查询方法三:
   //根据判断类型找到目标元素;	
   List<Element> elements = doc.selectNodes("/orders/order[@type='VIP']/time");
   for(Element ele:elements){
      System.out.println(ele.getText());
   }
}


补充:

一:乱码问题;
1.加入xml文件以<?xml version="1.0" encoding="utf-8" ?> 格式的;如果对xml文件进行修改了,其中包含中文字符的内容,另存为其他格式化时(比如unicod，ANSI)等等格式,
则新保存的配置文件,程序读取时候将会出现乱码,不能正常的读取;所以文件开头的信息标签中的encoding内容就是告诉读取此xml文件的程序使用怎样的解码方式来进行解码,
也就是说encoding属性并不一定和xml的保存时的编码格式相符,这就是产生乱码的根源,所以最简单的解决方式就是在另存为xml文件时保证选择的编码方式和标签中的encoding参数一致;
其次,在中文windows系统下许多的软件,工具默认的编码/解码方式是GBK(包括myeclipse);

2.写入xml文档时的乱码问题;
     将内存中的document对象重新写入xml文档特别要注意乱码问题,当然通过以下方法设置编码方式后需要保证与xml开头的版本号信息标签中encoding相符;

     方法1：使用OutputStreamWriter设置写入文档时所使用的编码表;
  OutputStreamWriter out = new OutputStreamWriter(new FileOutputStream(file),"utf-8");
  document.write(out);
  out.close();
  
     方法2：
  OutputFormat format = OutputFormat.createPrettyPrint();
  format.setEncoding("gb2312");
  XMLWriter writer = new XMLWriter(new FileWriter(file),format);
  writer.write(document);
  writer.close();
  
      方法3：
  OutputFormat format = OutputFormat.createPrettyPrint();
  XMLWriter writer = new XMLWriter(new OutputStreamWriter(new FileOutputStream(file),"utf-8"),format);
  writer.write(document);
  writer.close();


//编码具体参考prototype中SongUnicodeAndAll;

 
 二:XML文档(类型)定义有几种形式?它们之间有何本质区别?
两种形式 dtd,schema;

DTD:
1 DTD可以嵌入在XML文档中,如下面的例子:

<?xml version="1.0"?>  
<!DOCTYPE note [  
  <!ELEMENT note (to,from,heading,body)>  
  <!ELEMENT to      (#PCDATA)>  
  <!ELEMENT from    (#PCDATA)>  
  <!ELEMENT heading (#PCDATA)>  
  <!ELEMENT body    (#PCDATA)>  
]>  
<note>  
  <to>George</to>  
  <from>John</from>  
  <heading>Reminder</heading>  
  <body>Don't forget the meeting!</body>  
</note>  

2 DTD也可以独立的放在一个文件中,如servlet2.3的部署描述文件xml, dtd文件引用了一个网络资源文件:

<?xml version="1.0" encoding="UTF-8" ?>  
  <!DOCTYPE web-app PUBLIC  
    "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"  
    "http://java.sun.com/dtd/web-app_2_3.dtd" >  
<web-app>  
...  
</web-app>  

3 当然DTD文件也可以放在本地,需要注意的是两者DOCTYPE的声明的不同:

<?xml version="1.0"?>  
<!DOCTYPE note SYSTEM "note.dtd">  
<!-- 注意note.dtd要和本xml放在同目录下面 -->  
<note>  
<to>George</to>  
<from>John</from>  
<heading>Reminder</heading>  
<body>Don't forget the meeting!</body>  
</note>   
 
Schema:
是DTD的替代者,它比DTD可以做更多的事情,但是代价就是Schema比DTD更复杂,他本身就是一个xml类型的文本;

如:

XML Schema的引用:

<?xml version="1.0"?>  
<note  
xmlns="http://www.w3school.com.cn"  
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
xsi:schemaLocation="http://www.w3school.com.cn/note.xsd">  
  
<to>George</to>  
<from>John</from>  
<heading>Reminder</heading>  
<body>Don't forget the meeting!</body>  
</note>  

note.xsd:

<?xml version="1.0"?>  
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"  
targetNamespace="http://www.w3school.com.cn"  
xmlns="http://www.w3school.com.cn"  
elementFormDefault="qualified">  
  
<xs:element name="note">  
    <xs:complexType>  
      <xs:sequence>  
    <xs:element name="to" type="xs:string"/>  
    <xs:element name="from" type="xs:string"/>  
    <xs:element name="heading" type="xs:string"/>  
    <xs:element name="body" type="xs:string"/>  
      </xs:sequence>  
    </xs:complexType>  
</xs:element>  
  
</xs:schema>  


三:xml有哪些解析技术?区别是什么? 
有DOM,SAX,STAX等;

DOM:
Dom方式解析XML先要在内存中创建Dom树,然后通过遍历或修改树节点,来对XML文档中的内容进行处理,如果要将Dom树保存成XML文档需要借助Transformer,具体内容请参考我的上篇文档;
Dom方式的优点在于,能够将整个文档读入到内存中,可以在任意时刻修改Dom树的任意节点的内容,并且可以使用XPath接口辅助查询和处理,适合对XML的随机访问;

SAX(Simple API for XML):
事件驱动型的XML解析方式;它顺序读取XML文件,不需要一次全部装载整个文件;当遇到像文件开头,文档结束,或者标签开头与标签结束时,它会触发一个事件,用户通过在其回调事件中写入处理代码来处理XML文件,
适合对XML的顺序访问;但是不允许对XML文件随机存取,如：当前解析到第3个Element,此时程序无法得到第5个Element的信息,因为还没有解析到第5个Element;同样也无法得到第1个 Element的信息,因为已经丢失了;
根据XML原始文档,我们可以设计一个继承于DefaultHandler类的一个控制器,DefaultHandler类将会提供不同种类的回调函数;

STAX(Stream API for XML):
和SAX相同的是,STAX采用一种面向流方式去解析XML,不支持随机访问xml,STAX解析器不同于DOM解析器,和SAX也存在一些不同之处;
STAX事件解析器不同于SAX的数据推送,它只需要把需要的数据从XML文档中加载出来;
在StAX中,程序的切入点是表示XML文档中一个位置的光标,应用程序在需要时向前移动光标,从解析器拉出信息,也就是说SAX采用了一种观察者模式,由解析器去逐行解析来触发事件,而STAX采用pull模式需要开发者
调用解析器的next方法来命令其继续解析xml文本,触发事件后再处理事件;


*/



