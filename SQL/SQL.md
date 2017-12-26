SQL


package prototype;

import java.sql.*;

public class SQL {
	public static final String DBDRIVER= "oracle.jdbc.OracleDriver";  //设置驱动类;
	
	public static final String DBURL= "jdbc:oracle:thin:@127.0.0.1:1521:song";  //定义链接地址;
	
	public static final String DBID= "system";  //数据库连接用户名;
	
	public static final String DBPW= "song";  //数据库连接密码;
	
	private static Connection conn=null;
	
	public static void main(String[] args) {
		try{
			Class.forName(DBDRIVER);  //加载驱动类;
		}catch(ClassNotFoundException e){
			e.printStackTrace();
		}
		
		try{
			conn=DriverManager.getConnection(DBURL,DBID,DBPW);
			
			conn.close();  //关闭数据库连接;
			
		}catch(SQLException e){
			e.printStackTrace();
		}
		
		System.out.println(conn);  //正常打印代表连接成功;
		
		
		
	}

}


/*

JDBC(Java Database Connectivity,java 数据库连接),提供了一种与平台无关的用于执行SQL(Structured Query Language,结构化查询语言)
的标准Java API,可以方便的实现多种关系型数据库的统一操作,它由一组用Java语言编写的类和接口组成;
在实际开发中可以直接使用JDBC进行各个数据库的连接与操作,而且可以方便的向数据库中发送各种SQL命令;在JDBC中提供的是一套标准的接口,
这样,各个支持JAVA的数据库生产商只要按照此接口提供相应的实现,则就都可以使用JDBC进行操作,极大的体现了JAVA可移植性的设计思想;

java.sql包中保存了大量的类和接口;

JDBC本身提供的是一套数据库操作标准,而这些标准又需要各个数据库厂商的实现,所以针对每一个数据库厂商都会提供一个JDBC的驱动程序,目前
比较常见的驱动类型有：

1.JDBC-ODBC桥驱动;  //是SUN提供的一个标准的JDBC操作,直接利用微软的开发数据库连接规范ODBC进行数据库的连接操作,但是这种操作性能较低,
					     所以不推荐; JDBC->ODBC->DB;
2.JDBC本地驱动;		//直接使用各个数据库生产商提供的JDBC驱动程序,但是因为其只能应用在特定的数据库上,会丧失掉程序的可移植性,但是这样
					     操作的性能较高;不过这些驱动程序需要单独配置,往往是以一组jar包(zip包)的形式出现的,如果要使用的话则肯定要配置
					  classpath,在开发中大部分情况都是基于一种数据库开发的,所以使用此种模式较多;
3.本地协议纯JDBC驱动;

4.JDBC网络驱动;


一般在开发中会将以上的四类简单划分成3类：

1.JDBC-ODBC;
2.纯JDBC连接;
3.网络的JDBC连接;



数据库的操作过程：

1.配置对应数据库的驱动程序;方法如下：
(1)我的电脑->属性->高级->环境变量->找到CLASSPATH(如果没有就新建：.;address)在最后添加驱动程序地址; 如：.\C:\app\song\product\11.2.0\dbhome_2\jdbc\lib\ojdbc6.jar; 
  (驱动版本需要根据JDK版本选择,JDK版本的查看方法：cmd->java -version 查看;) 
(2)通过选中项目->右键->build path->Add External Archives->选中对应的驱动jar文件;


2.加载驱动程序：
  通过反射机制的Class.forName()语句可以加载一个驱动程序;
  首先Class的实例化需要一个完整的"包.类"名称,也就是这个驱动程序的名称;
  需要通过打开方式为压缩程序来打开对应的jar包,并找到其中的driver.class文件,将路径设置为驱动类; 如：public static final String DBDRIVER= "oracle.jdbc.OracleDriver";


3.连接数据库,连接的时候一般都要输入用户名/密码;

DriverManager;  //是一个最常用的类,使用此类可以取得一个数据库的连接;

static Connection getConnection(String url); //建立与数据库的连接,参数为地址;

static Connection getConnection(String url, String user, String password);  //建立与数据库的连接,参数为地址/用户名/密码;
 
Connection;  //每一个Connection的实例化对象都表示一个数据库连接;

在DriverManager中,提供的主要操作就是得到一个数据库的连接,getConnection()方法就是用来取得连接对象,此方法的返回类型是：Connection对象,
不管使用哪种连接方式,都必须提供一个数据库的连接地址,如果在连接数据库的时候需要用户名/密码,则需给出;

Oracle数据库的链接地址格式:
jdbc:oracle:thin:@MyDbComputerNameOrIP:1521:ORCL", sUsr, sPwd ;
(jdbc是协议,jdbc URL中的协议总是jdbc; oracle属于子协议,驱动程序名/数据库连接机制名; 接下去的内容:主机名:端口:数据库名; )

补充：
(thin是一种瘦客户端的连接方式,即采用这种连接方式不需要安装oracle客户端,只要求classpath中包含jdbc驱动的jar包就行;thin就是纯粹用Java写的ORACLE数据库访问接口;
oci是一种胖客户端的连接方式,即采用这种连接方式需要安装oracle客户端;oci是Oracle Call Interface的首字母缩写,是ORACLE公司提供了访问接口,就是使用Java来调用本机的Oracle客户端,
然后再访问数据库,优点是速度 快,但是需要安装和配置数据库;)

通过DriverManager取得Connection对象之后,实际上就表示数据库连接上了,之后就可以进行数据库的更新及查询操作,最后需关闭数据库连接;


4.操作数据库,创建表,查询表,更新记录;

用户的sql developer进程和Oracle server众多的服务进程(Server process)中相对应的oracleSID进行交互完成对数据的操作,当用户端执行语句时,把信息传递给对应的Oracle服务进程,
由它来对语句进行编译和生成执行计划并执行语句,然后返回给用户的sql developer进程并显示在程序的终端上;


5.关闭;



数据库的操作： Statement/PreparedStatement;

数据库的查询: ResultSet;

调用存储过程: CallableStatement;



SQL概要:

数据库标准语言：SQL 结构化查询语言(Structured Query Language);

两种数据存储(数据持久化)形式：1.文件; 2.数据库(database);


SQL三种主要功能:

-DDL(data definition language): 数据定义语言,用于定义数据的结构,如创建/修改/删除数据库对象;

-DML(data manipulation language): 数据操作语言,用于检索/修改数据;

-DCL(data control language): 数据控制语言,用于定义数据库用户的权限;


-DQL(data query language):数据查询语言; 用于检索数据,也可包含在DML中;

-TCL(Transaction control language):事务/交易控制语言; 主要负责提交(commit,相当于保存)/回滚(rollback,相当于撤销);  



DBMS(database management system): 数据库管理系统; 
RDBMS(Relationship ...): R代表这种数据库存储数据的基本结构;关系型数据库管理系统;这里的关系可以直接理解为 table;
关系型/ 层次型/ 网状型;

-ORACLE 10g (由ORACLE 提供);
-DB2 (IBM提供);
-SQL SERVER/assess(MicroSoft提供);
-MySQL (瑞典的AB公司);

小型数据库：access/foxbase;		中型数据库：mysql/sql server/informix;		大型数据库：sybase/oracle/db2;

选用什么类型的数据库：
1.项目的规模; (a.负载量多大,用户量多大;b.成本;c.安全性;)  举例：留言板,负载量100人内,成本千元内,安全性要求不高,选择小型数据库;



数据库创建流程：

(dba的职责) 																					(程序开发人员的职责)								 
安装ORACLE-> 创建数据库(创建了:data file/log file/control file ) -> 创建用户(创建授权的用户) -> 登录数据库 -> 用SQL操作表 ;



sql developer 发起对数据库的连接需要提供:

1.database server的进程在哪台计算机,需要给出计算机的IP; 
2.具体的进程端口 sql developer默认port:1521;
3.具体的数据库名字(SID); 


c/s结构:
client:sql developer;
server:oracle db server;


client 与数据库建立连接的方式：
1.client:sql developer(linux)->tcp/ip->  database(solaris)
  client需要提供：ip/port/dbname/db_username/passwd
2.远程登录安装了数据库的机器(telnet ip,需要提供os账号,口令)，用服务器上的sqlplus登录数据库(提供db_username/passwd)

sql developer就是用java写的,用到的接口就是JDBC相关接口;


cmd:
telnet 远程登录的方式连接到了数据库所在的机器,执行telnet命令的机器变成了数据库机器的终端;
也称为伪终端;前提是知道目标机器的os账号;

$ORACLE_SID;
连接数据库的名称;


SQL 常用语句:

(快捷键:shift+home/end 选中一行;)

conn 用户身份/密码;  //连接数据库; 例：conn system/song;

disc;  //断开与当前数据库的连接; (sqlplus语句;)

exit;  //断开当前数据库连接并退出sqlplus; (sqlplus语句;)

start/@;  //运行sql脚本(脚本中有命令); 例：@ d:\a.sql; (sqlplus语句;)

edit;  //打开指定的脚本文件进行编辑; (sqlplus语句;)

spool+spool off; 将之间的内容输入到指定的脚本文件中去(如果没有此文件会新建); (sqlplus语句;)

passw;  //修改密码;


create datebase (dba建库);

grant;  //授权;

revoke;  //收回;

commit;  //提交; 相当于保存;

rollback;  //回滚; 相当于撤销;

clear scr;  //清空控制台输出的所有内容;


1.create table test ; //创建一个表格,名为test;

例:
create table test(
c1 NUMBER,
c2 NUMBER(6),
c3 NUMBER(4,3),
c4 NUMBER(3,-3),
c5 NUMBER(2,4) );

在表格中创建5列,
第一列名为c1,为数字类型;  (超过有效数字部分按四舍五入进位);
第二列c2含有6位有效数字,小数点后无内容; 如果填入999999.5则会自动进位为1000000然后报溢出错误;
第三列c3含有4位有效数字,小数点后有三位,可表示的最大数字为：9.999;
第四列c4含有3位有效数字,小数点前将有3位被忽略不计(不计入有效位数),可表示的最大数字为：999000; 如果填入1234则转换为1000,如果填入1520则转换为2000;
第五列c5含有2为有效数字,小数点后有4位,可表示的最大数字为：0.0099; 


2.drop table test ; //删除一个名为test的表格;

  drop table test purge ;  //drop后的表被放在回收站(user_recyclebin)里,而不是直接删除掉;这样,回收站里的表信息就可以被恢复,或彻底清除;
     					       通过查询回收站show recyclebin;获取被删除的表信息;
     					       若要恢复表:flashback table <table_name> to before drop [<new_table_name>];    
     					       若要彻底删除表,则使用语句：drop table <table_name> purge;
						       清除回收站里的指定表：purge table <table_name>;
						       清除当前用户的回收站：purge recyclebin;
						       清除所有用户的回收站：purge dba_recyclebin;

3.show user ; //显示当前用户名;


4.desc test ; //显示名为test的表的结构;


5.insert into test values (1,2,3,4,5); //将表中每一列的内容输入; 可以填入null表示空值;


6.insert into test(c2) values(1); //直接给c2赋值;


7.alter table test modify(c2 char(10));  //修改表中c2列的类型; 注意:在改变表中的某个列的类型时,必须先保证此列中的元素为null,不然更新后的数据会有和类型不符的风险,所以会报错;
 
  alter table test add(c6 varchar2(10));  //给表增加一个新列c6;
  
  alter table test drop(c6);  //删除表中的c6列;


8.select length(c1),length(c2) from test ; //检索c1列的长度;

varchar2必须定义长度,最大长度4000字节,char可以不定义长度,默认为1,最大长度2000字节;
varchar2按字符串的实际长度存,char按定义长度存;
如果列的取值是定长,定义成char类型;如果不固定,定义为varchar2;


9.PRIMARY KEY; 主键能唯一的标识表中的每一行,就是说这一列非空且值不重复,可以指定为主键;作用是用来强制约束表中的每一行数据的唯一性;
               (外键是b表中的某一列引用的值来源于a表中的主键列;也是约束b表中的外键列的值必须对应a表中的主键列值,修改a表主键的值会连带影响b表中的相应内容;可以形成a表b表的联系,保持数据的约束和关联性;)


10.SEQUENCE;  //序列;
   Oracle中不像MYSQL和SQLServer中那样可以指定一个列为自动增长,不过在Oracle中可以通过SEQUENCE序列来实现自动增长字段;在Oracle中SEQUENCE被称为序列,可以控制其自动增加,一般用在需要按序列号排序的地方;
在使用SEQUENCE前需要首先定义一个SEQUENCE,定义SEQUENCE的语法如下:

CREATE SEQUENCE sequence_name; //sequence_name为序列名;
  
INCREMENT BY step;  //step为步长;
  
START WITH startvalue;  //startvalue为序列的起始数字;

一旦定义了SEQUENCE,就可以用CURRVAL来取得SEQUENCE的当前值,也可以通过NEXTVAL来增加SEQUENCE,然后返回 新的SEQUENCE值;比如:

sequence_name.CURRVAL;
  
sequence_name.NEXTVAL; 

如果SEQUENCE不需要的话就可以将其删除:

DROP SEQUENCE sequence_name;


下面举一个使用SEQUENCE序列实现自动增长的例子;创建一个名称为seq_PersonId 的SEQUENCE:

CREATE SEQUENCE seq_PersonId
  
MINVALUE 0
  
INCREMENT BY 1
  
START WITH 0; 
  
注1: 如果没加这句(MINVALUE 0),可能会出现这个错误(ORA-04006: START WITH不能小于 MINVALUE);解决方法就是指定最小值;

之后在将数据插入键值列中时插入: seq_PersonId.NEXTVAL;

注2:在选取seq_PersonId.CURRVAL之前必须先选取一次seq_PersonId.NEXTVAL,相当于seq_PersonId.NEXTVAL代表序列的开始,不然会报错,比如: 
select seq_PersonId.NEXTVAL from dual;
select seq_PersonId.CURRVAL from dual;


11.upate;  //更新行中的指定内容;
例:
update users 
set sex='男' 
where name = 'song';  //若此处不指定条件,则会更新表中的所有行的指定内容;


12.delete;  //删除内容;
例:
delete from users  
where name = 'song';  //若此处不指定条件,则会删除表中的所有内容;

13.Date:

ORACLE用7个字节来储存日期和时间信息;
世纪/年/月/日/时/分/秒;
默认日期格式为DD-MON-YY;
SYSDATE是一个系统函数,返回当前系统日期和时间;

create table test(
c1 date);

select sysdate from dual;  //获得系统当前日期;

insert into test values ('01-JAN-09');  //系统默认给出当前世纪的00:00:00;


14.select语句：

select查询的表成为源表 (from+源表)；

select的查询结果被称为结果集；

select功能：

投影(projection)操作：结果集是源表中的目标列;

选择(selection)操作：结果集是源表的目标行;

连接操作：结果集是多张源表的行组合;


select 语句 ： select 子句,from子句 (子句相当于语句中的关键字);


数学表达式:

声明为number的列可以使用数学表达式连接：
例：select name, (250-base_duration)*unit_cost + base_cost cost_250 ;

列别名：给列起别名,能够改变一个列/表达式的标识;
-适合计算字段；
-在原名和别名之间可以用as关键字,也可以直接用空格隔开;
-别名中包含空格/特殊字符/希望大小写敏感用双引号将其括起;


单引号：表达字符/字符串(即字符值);
双引号：表达标识(表名、列名/标识名);


oracle server对sql语句先编译,生成执行计划,再执行;
对于上例：select name, (250-base_duration)*unit_cost + base_cost cost_250 ;

对于数学表达式的执行计划为：select base_duration,unit_cost ,base_cost;
然后对所选出的所有行内容进行遍历执行数学表达式,把最后的结果作为cost_250也就是
数学表达式列的结果显示在select语句执行结果中,如果在表达式中出现null,则最后的结果也为null;


15.函数：
fun1(p1,p2) --> 返回值;

oracle提供的函数：
nvl(p1,p2);  
//if p1 is null then return p2 
else 
return p1
end if;

为了避免出现null,上例可以变更为: select name, (250-nvl(base_duration,0))*nvl(unit_cost,0) + nvl(base_cost,0) cost_250 ;


16.拼接运算符：
用||表示，字符表达式意为字符串的拼接,||可以将目标列与字符串拼接在一起;

例：select real_name||idcard_no from account;
执行后的返回结果为一列,real_name和idcard_no中每行内容被无缝拼接在一起,列名称就是表达式real_name||idcard_no；

select real_name||' '||idcard_no from account costumer;
用字符值(空格)来优化拼接显示；

表达以单引号作为字符值的字符值：'''' 第一个和第四个单引号是定界符,第二三个单引号表达一个单引号;
例：real_name||'''s IDCARD NO is'||idcard_no||'.' costumer; 


17.模糊查询：
where 列名 like '%关键字%' ;

例：
select * from users where name like '%s%' or password like '%5%'  //筛选出名字中含有s的项或密码中带有5的项;
select * from users where name like '%s%' and password like '%5%'  //筛选出名字中含有s的项且密码中带有5的项;


18.选择指定的行数：

        使用rownum表示需要被显示的行数;
        
        例: select * from tablename where rownum <= 30;  //取前30行的内容;
        
        注:在mysql中使用limit关键字达到同样的效果,如：select * from tablename limit 0,5 ;  //从第一行开始取,取5行内容;
   
   
19.去重复操作：

   select distinct column_name from table_name ;  //取出目标表中目标列中去重复后的行信息;

   select distinct column_name1,column_name2 from table_name;  //取得对组合后的列去重复后的行信息,distinct关键字只能放在最前,放在中间会导致不同列的筛选逻辑不一致,无法统一的显示,出现编译错误;
 
 
20.条件语句：
   
where子句跟在from子句后,之后跟上条件表达式：
     列名/常量+比较运算符+对应的数值;
     例：每月的固定费用是5.9,计算一年的固定费用,一年的包在线时长;
  select 12*base_cost ann_cost,12*base_duration ann_duration from cost where base_cost=5.9;

注: 1.where后不能跟列别名;因为语句的执行顺序为: from子句->where子句->select子句;

    2.字符必须用单引号括起,大小写敏感,如果不加引号或者加双引号则被视为标识(列名等);
    
    3.如果在选取字符时事先不知道其大小写,可以使用:
      upper(os_words);  大写;
      lower(os_words);  小写;
      initcap(os_words); 首字母大写;
      
    4.varchar2对空格敏感,在末尾多一个空格也会造成查找不到信息;char字符在查询时如果不足声明时的字符数会先补足再比较,所以如果空格出现在最后不会影响查找;
    
    5.and/or ;  //连接条件语句;
  	例: select base_cost,base_duration
    from cost
    where base_cost >=5
    and base_cost <=10 ;


21.where X between A and B 语句,相当于 where X >= A and X <=B ;
    
    
22.where x in/=any (a,b,c ...) ; //相当于用或链接元素;

例题:哪些资费的月固定费用是5.9/8.5/10.5元; (有三种方法实现:)

(1)select name,base_cost from cost
where base_cost = 5.9 
or base_cost = 8.5
or base_cost = 10.5 ;

(2)select name,base_cost in (5.9,8.5,10.5) ;

(3)select name,base_cost =any (5.9,8.5,10.5) ;

    
23.like ;  //可以利用like和通配符来进行模糊查询;
通配符：% 表示零或多个字符;
	    _ 表示任意单个字符;

例:哪些Unix服务器上的os账号名是以h_开头的;

select unix_host,os_username
from service
where os_username like 'h\_%' escape '\' ;  //此处的escape '\'代表\后的符号被转义了;
    
    
24. is null; //判断是否为空(未知);

例：哪种资费标准时包月的,即没有单位费用;

select unit_cost,name
from cost
where unit_cost = null ;  //没有显示结果,因为null=null不成立;null带有未知的含义,未知和未知不相等,同样<>null也不会有返回结果,但是不会报错;
where unit_coet is null ; //想表达某个参数未知时用is null表达; 
注意:事实上,不管条件为什么=null/!=null,返回都无显示结果,但不会报错,所以要注意,应该使用is/is not;
    
    
25.否定形式;

(1)= -> <>/!=/^= ;

(2)is null -> is not null
例:哪些资费信息不是包月的,即有单位费用;
select name,unit_cost
from cost
where unit_cost is not null ;
where unit_cost <> null ;  //没有显示内容,由于unit_cost不能说就一定不等于null这个未知的数,如果这个未知的数就等于此时的unit_cost呢,所以和=null一样逻辑上错误;

(3)between a = b -> not between a and b ; 

(4)in/=any -> not in/<>all ; (如果集合中有null时,not in一定没有返回值;)
例: 哪些资费信息的月固定费用不是 5.9,8.5,10.5 ;
select name,unit_cost from cost
where unit_cost not in (5.9,8.5,10.5) ;
or base_cost is null ;  //这条语句不能遗漏,由于计时收费的base_cost为null,而null与5.9,8.5,10.5选项无法比较,所以不会被选出,就必须加上一条限制语句来筛选出计时收费的内容;


26.order by ;  //用order by子句对查询出来的结果进行排序,即对select子句的计算结果排序;

asc : 升序(默认);

desc : 降序;

order by 是select语句中最后的子句;

例:按开通的时间先后显示业务账号的信息;

select unix_host,os_username,create_date
from service
order by create_date desc ;

例:按月固定费用从大到小的顺序显示资费信息;

select name,base_cost 
from cost
order by base_cost desc ;
注:null被显示在了第一行;

例:按年固定费用从大到小显示资费信息;

select name,base_cost,12*base_cost ann_cost
from cost
order by ann_cost desc ;
注:order by 的执行顺序是在select之后,所以可以用列别名;

例:按unix服务器ip地址升序,开通时间降序显示业务账号信息;

select unix_host,os_username,create_date
from service
order by unix_host,create_date desc ;
order by 1,3 desc ;  //这条语句和上一条相同,1/3代表select后的选项的编号;


27.select后可以跟常量,显示数量为此表的列数;

例: 计算1+1;
select 1+1 from dual ; (dual表只有一列,所以显示比较清晰;)

例: 显示guojing的大写形式;
select upper('guojing') from dual ;



/////////////SQL函数(单行函数,转换函数):  //(函数在不同的数据库中基本都会有所差别)////////////////////


28.字符类型,数值类型函数:

     字符类型  //length/upper/lower/initcap

     数值类型  //round/trunc

round用法: 
round(45.923,2); //45.92; 参数表示取45.923的两位小数(如果第二个参数为0或者不写代表只取整数部分)结果为四舍五入后的结果;
round(45.923);   //46;
round(45.923,-1);  //50; 当第二个参数为-1时表示精确到整数的十位数;

trunc用法:
trunc(45.923,2);  //45.92; 参数表示取45.923的两位小数(如果第二个参数为0或者不写代表只取整数部分)结果为直接舍去不需要位数后的结果;
round(45.923,0);  //45;
round(45.923,-1); //40;

例:月固定费用四舍五入/截取到角,元;
select name,round/trunc(base_cost,1) r1,
            round/trunc(base_cost) r2
from cost;


29.日期类型
默认显示格式: '01-AUG-13' 'DD-MON-RR' ;
当运行insert into table_name values('01-AUG-13') ; 时系统隐式地调用了:insert into table_name values(to_date('01-AUG-13','DD-MON-RR'));
修改系统日期的显示格式: alter session set nls_date_format = 'yyyy mm dd hh24:mi:ss' ;
修改日期格式后的显示: '2013 08 01 17:51:20' ;

session(会话)和connection同时建立,当客户端进程与服务器进程进行连接交互时,session被建立,成为了SQL的运行环境;

to_date函数:将字符串转换成一个日期值; insert into table_name(column_name) values to_date('10 september 2009', dd Month yyyy); 此函数的第二个参数只能暂时改变字符串日期的显示格式,但没有改变SQL运行环境中的显示格式,如要改变参照上文中的语句;

to_char函数:当处理一个日期值时可以用此函数将目标日期显示为用户希望格式的字符串;
注:
(1)第一个参数为要处理的日期,第二个参数为格式,如果不写第二个参数则为session中的默认格式;
(2)格式必须用单引号扩起,并且大小写敏感;
(3)必须是有效的日期格式;
(4)fm可以去掉两端的空格以及前导零;


日期格式:

yyyy:用数字表达的四位年;
mm:用数字表达的二位月;
d:用数字表达的一周内的第几日(周日为1);
dd:用数字表达的一月中的第几日;
ddd:用数字表达的一年中的第几日;
hh24:用数字表达的24小时制的小时;
hh12:用数字表达的12小时制的小时;
mi:用数字表达的分钟;
ss:用数字表达的小时;

day:用全拼表达的星期几;(sunday)
month:用全拼表达的月;(april)
mon:用简拼表达的月;(mar)

例:显示今天星期几;
select to_char(sysdate,'DAY') from dual ;


对含有日期类型的项进行去重操作时,就算显示格式是'10-FEB-12'这样的格式,但是
系统在读取date类型的信息时是包含'时分秒'信息的,所以在去重时会保留两个看似相同的
日期元素,其实它们的不同之处没有被显示出来,因此在对日期类型去重时用类似:
to_char(date_name)这样的形式来让日期类型变为字符类型后进行去重就能保证所显示的
日期项没有重复的内容了;还需要注意的是to_char(date,date_formate)这个函数的返回值类型为varchar2;

同样,如:
select distinct unix_host||' '||date_name
from service ；

也能使日期类型转换为字符类型;

例:创建一张表,包含date类型的列,插入2008年8月8日8点8分8秒并显示;

insert into test values
(to_date('2008 08 08 08:08:08','yyyy mm dd hh24:mi:ss')) ;



30.to_number;
例:哪些os账号是在3月份创建的;

select os_username,create_date
from service
where to_char(create_date,'mm')='03' ; 

where to_char(create_date,'mm')=3 ;
这里的'03'可以用3代替的原因是在oracle中:'03'=3; '03'<>'3';
oracle有隐式的类型转换,当字符类型和数字类型比较时,oracle会把字符类型通过to_number函数转换为数字类型类比较;
此式就相当于:where to_number(to_char(create_date,'mm'))=3 ;


31.fm;ltrim/rtrim;
where to_char(create_date,'fmmm')='3' ;
在to_char函数中,fm起到了去前后空格与去前导0的作用;

ltrim/rtrim的作用是将前导/后导空格/指定字符集去除,如:
select*from test
where rtrim(c1)='abc';

注意1:
ltrim/rtrim函数的返回值是varchar2,假设c1类的类型是char(10),其中有一项为'10',那么,
where ltrim(c1,'1')='0';  此处的返回值为false;
where ltrim(c1,'1')='0        ';  0+8个空格,此处的返回值为true;


注意2:
对于ltrim/rtrim的用法oracle与其他数据库可能有些不同,不同点主要体现在第二个参数上;
先看几个实例：

SQL> select ltrim('109224323','109') from dual;

LTRIM('109224323','109')
------------------------
224323

这个的功能应该都知道的噢~~  再来看一个：

SQL> select ltrim('10900094323','109') from dual;

LTRIM('10900094323','109')
---------------------------
4323

是不是有点迷糊了？按道理说应该是00094323的结果嘛~~  再来看两个对比的：

SQL> select ltrim('10900111000991110224323','109') from dual;

LTRIM('10900111000991110224323
------------------------------
224323

SQL> select ltrim('109200111000991110224323','109') from dual;

LTRIM('10920011100099111022432
------------------------------
200111000991110224323

是不是有这样的疑问：为什么第二个查询语句多了一个2就没被截了呢？

再来看一个：

SQL> select ltrim('902100111000991110224323','109') from dual;

LTRIM('90210011100099111022432
------------------------------
2100111000991110224323

按道理说是截109的值，为什么90也被截了？

总结：ltrim(x,y) 函数是按照y中的字符一个一个截掉x中的字符，并且是从左边开始执行的，只要遇到y中有的字符, x中的字符都会被截掉, 直到在x的字符中遇到y中没有的字符为止函数命令才结束 ;


32.where create_date like '%MAR%' ;
由于使用了like语句,orcale将会把create_date转换为字符类型后再与like的字符内容比较,
所以这条语句相当于:where to_char(create_date) like '%MAR%' ; 它的局限性也很明显,
就是要保证create_date在转换为字符类型后月份的显示格式与模糊查找的内容相匹配;


33.字符类型用运算符号链接;
select '1'+'1' from dual ;
结果为:2 ; 
注意:和java不同的是oracle的字符拼接用的是||号,所以这里相当于数学运算;

select 'a'+'b' from dual ; 
结果:报错,invalid number;

select to_number('a','x')+to_number('b','x')
from dual ;
结果:21; to_number利用参数将字符类型转换为16进制的数来进行运算;


34.rpad/lpad;  右/左填充;

select rpad('MARCH',9,' ')||'d' from dual ;  //第二个参数9代表填充后一共的位数;
结果: MARCH    d;

这就类似于char类型变量的填充机制;

注意:char/varchar2与任意指定的'abc'字符比较的区别;
当char与一个直接量'xxx'比较时,系统自动将'xxx'转换为长度和所比较变量一致的char类型,也就是说在比较前会自动补足空格,所以被认为是空格不敏感;
而当varchar2与一个直接量'xxx'比较时,系统自动将'xxx'转换为varchar2类型,不会补空格,所以是空格敏感的,相差一个空格也会视为不同字符;


35.to_char函数的格式参数;
select to_char(base_cost,'L99.99')
from cost;
结果:(L代表货币符号:dollar;)
 $5.90
 $6.90
 $8.50
$10.50

$20.50

select to_char(base_cost,'L00.00')
from cost;
结果:
$05.90
$06.90
$08.50
$10.50

$20.50


36..sql运行环境的国家/语言转换;
alter session set nls_territory = 'CHINA' ; //改变sql运行环境的地区参数为china；

select to_char(base_cost,'L00.00')
from cost;
结果:
￥05.90
￥06.90
￥08.50
￥10.50

￥20.50

alter session set nls_language =  'SIMPLIFIED CHINESE' ;

select sysdate from dual;
结果:
SYSDATE
02-8月 -13

select to_char(sysdate,'MONTH') from dual;
结果:
SYSDATE
8月;

alter session set nls_territory = 'AMERICA' ;
alter session set nls_language =  'AMERICAN' ;
将地区和语言改为美国;


例:显示月固定费用,单位费用,单位费用为null则显示no unit cost;

select base_cost,
nvl(to_char(unit_cost,'0.0000'),'No unit cost')  
from cost;
注意1:这里to_char(unit_cost,'0.0000')的作用是使显示整数位为0的小数时不会为'0.xxxx',如果不设置,在使用了to_char(0.xxx)后系统默认的显示为:'.xxxx';
注意2:在使用了to_char(对number进行格式设置)后应该用数字类型去接收这个函数的返回值;


37.日期类型的运算;
-对日期加减一个数字,返回值为一个日期;
alter session set nls_date_format=
'yyyy mm dd hh24:mi:ss' ;

select sysdate-1,sysdate,sysdate+1
from dual ;
结果:2013 08 01 10:53:15	 2013 08 02 10:53:15  2013 08 03 10:53:15
日期中的'天'被改变;

例:计算10分钟之后的时间;
select sysdate,sysdate+10/60/24
from dual;
结果:2013 08 02 10:53:15  2013 08 02 11:03:15


-两个日期相减表示两个日期相差的天数;
select sysdate-create_date
from account;
结果:两个日期的相差天数;

例:按照开通天数的长短显示业务账号的信息;
select unix_host,os_username,round(system-create_date) days
from service
order by days desc ;


38.日期函数;

months_between('date1','date2'); //两个日期之间相差多少个月;

add_months;  //一个日期加减一个月;

next_day;  //根据参数(必须是具体的描述'天'的日期,如to_date(sysdate,'day')/'FRIDAY')下一个指定的'天'到来的日期;

last_day;  //同一个月的最后一天的日期;


例:显示一个月之后的日期;
select add_months(sysdate,1)
from dual;
结果:2013 09 02 11:09:53  (一个月之后的日期;)

例:显示下一个周五的日期;
select next_day(sysdate,'FRIDAY') 
from dual;
结果:2013 08 09 11:14:40 


39.SQL语句中的分支;

-分支表达式 case when ;

例:当月包在线时长为20小时,单位费用涨5分,为40小时,单位费用涨3分,其他不变;

select base_duration,
	   case when base_duration = 20
	        then unit_cost + 0.05 
	        when base_duration = 40
			then unit_cost + 0.03
	   else               (如果没有else,其他不符合case的项将都为null处理;)
	        unit_cost
	   end  new_unit_cost (列别名)
from cost ;


-分支函数 decode ;
例:同上;
select base_duration,unit_cost,
	   decode(base_duration,20,unit_cost+0.05,
	                        40,unit_cost+0.03,
	                        unit_cost) new unit_cost
from cost;


40.substr; //取子字符;
select substr(real_name,1,2),real_name
from account;
注意:这里的参数1代表real_name的第一位,2代表一共取2位;如果第二个参数为负数,代表从右往左(倒数)的第几位开始取;


41.concat;  //拼接字符串;
select concat(real_name,idcard_no)
from account;
相当于||符号,拼接字符串;





//////////////////////////SQL函数(多行函数/组函数)///////////////////////


1.组函数(多行函数);
注1:组函数一般情况下只处理非空值;
注2:组函数中的参数可以是未经过分组的列名;
如:select count(id),count(unit_cost),count(startime);
from cost;
结果: 6  5  0 ;
所以当组函数要处理的所有元素值都为null时,count函数返回0,其它函数返回null;

常有于报表统计:
经常需要对表中的数据进行分析;统计它们的平均值,最大值,最小值等;Oracle提供了这样的函数;

组函数操作在一组行(记录)上,每组返回一个结果;
AVG(DISTINCT|ALL|n);  //平均值(number类型);
SUM(DISTINCT|ALL|n);  //求和(number类型);
COUNT(DISTINCT|ALL|expr|*);  //计数(number/char/date);
count(*);  //返回记录数(将null元素也计入计数);
MAX(DISTINCT|expr);  //最大值(number/char/date);
MIN(DISTINCT|expr);  //最小值(number/char/date);


例:单位费用的总和,平均值/最大值/最小值/个数;

select avg(unit_cost) avgcost,  //这里avg中的参数all被省略了;
	   sum(unit_cost) sumcost,
	   count(unit_cost) countcost,
	   max(unit_cost) maxcost,
	   min(unit_cost) mincost,
from cost;
结果:
AVGCOST SUMCOST COUNTCOST MAXCOST MINCOST
   xxxx    xxxx      xxxx    xxxx    xxxx

注1:当使用avg函数时,如果其中有元素为null,那么在算平均数时是不会把这个空值加入基数的,也就是说,这个null不会被当成0来看待,
如果操作者需要将为Null的项看作0,则可以用:
select avg(nvl(unit_cost,0)) from cost ;这样的函数达到目的,结果将会相比之前变小;

注2:与单行函数不同的是,像select round(unit_cost) from cost这样的单行函数会对unit_cost列中的
所有行进行调用处理,所以最后显示的结果也是unit_cost中元素的行数;而多行函数会同时对一个列中所有
的函数做处理,所以最后的返回值也只有一条(行);


例:有多少台unix服务器对外提供远程登录业务;

select count(distinct unix_host) from service ;
去重复后再做统计;


2.group by;

例:每台unix服务器上开通的os帐号数即开户数;

select unix_host,count(os_username)
from  service
group by unix_host;

运行步骤:
(1)from service
(2)group by unix_host; 相当于对unix_host做重复分配处理,把unix_host相同的项分成一组成为这一类相同元素的"标识",它们将在结果中作为unix_host的项显示;
(3)select unix_host; 将分组后的unix_host"标识"显示;
(4)count(os_username); 与之前未分组的情况不同的是,这里的多行函数将不会只运行一次记录所有os_username的元素数,而是以分组后的unix_host为条件,记录每一组unix_host对应的os_username中的非空元素数;

结果:
UNIX_HOST		COUNT(OS_USERNAME)
192.168.0.20		            4
192.168.0.23                    1
192.168.0.26                    3


例：tarena26(192.168.0.26)上开通的os帐号数,即开户数;

(1)select count(*) 
from service
where unix_host = '192.168.0.26'; //这里不报错的原因是select语句后跟的只是一个单独的组函数,这个函数将把where语句选出的行作为参数执行后显示,没有问题;
结果:3;

(2)select unix_host,count(*)
from service 
where unix_host = '192.168.0.26';
结果:报错！unix_host和count(*)无法匹配显示,这里的Unix_host只是单独的项;

(3)select unix_host,count(*)
from service 
where unix_host = '192.168.0.26';
group by unix_host;
结果:3;

(4)select max(unix_host),count(os_username)
from service
where unix_host in ('192.168.0.26','192.168.0.23')
结果:  192.168.0.26		4;

注意:若存在group by子句,select后面只能跟组名/组函数;


例:根据Unix服务器ip地址,开通时间统计开通的os帐号数,即开户数;

select unix_host,to_char(creat_date,'yyyymmdd'),count(os_username)
from service
group by unix_host,to_char(creat_date,'yyyymmdd');

注:这里select也要用to_char(creat_date,'yyyymmdd')的原因是这里select后必须跟组标识,而此处的组标识为to_char(creat_date,'yyyymmdd'),而不是creat_date;


3.having; //针对组的条件筛选;


例:显示开户数多于2个的Unix服务器上的最早开通日期;

select unix_host,min(create_date)
from service
group by unix_host
having count(os_username)>2;


例:哪些Unix服务器开通的os帐号数即开户数多于2个;

select unix_host,count(*)
from service
group by unix_host
having count(*)>2;


例:哪些unix服务器在哪几天的的开户数多于1个;

select unix_host,to_char(create_date'yyyymmdd'),count(*)
from service
group by unix_host,to_char(create_date,'yyyymmdd')
having count(*)>1;

注:having后只能跟group by后的组标识/组函数(条件和select相似);


4.所有语句的执行顺序:
from
where
group by
having
select 
order by




//////////////////////////子查询//////////////////////////////

1.子查询:
子查询就是在一条SQL语句中嵌入select语句;
子查询的执行过程:先执行子查询,子查询的返回结果作为主查询的条件,再执行主查询;
子查询只执行一遍,若子查询的返回结果为多个值,ORACLE会去掉重复值之后再将结果返回给主查询;


例:哪些os账号的开通时间是最早的;

select unix_host,os_username,create_date
form service
where create_date= (select min(create_date) 
				    form service)
//注1:这里如果使用 where create_date = min(create_date)会报错:group function is not allowed here;因为create_date是列名,而非选出的一个类组,组的特征是:
                由具有相同特性的,将相同项合并之后的多行组成;而单纯的一列并没有满足将相同项合并为一项的条件,所以无法和min()这样的组函数连接成等式;这样的函数是要将结果
                赋给一个组/与组进行比较,连接显示的内容的,而不是仅仅与一行相作用的;
//注2:这里的子查询语句中查询的内容可以是另一张表的;
 
 
例:哪些os账号的开通时间比unix服务器192.168.0.26上的huangr晚;

select unix_host,os_username,create_date
from service
where create_date > (select create_date 
					 from service
					 where unix_host = '192.168.0.26'
					 and os_username = 'huangr')
					 

例:哪些os账号的开通时间比huangr晚;(多台unix服务器上都有各为huangr的os账号)

select unix_host,os_username,create_date from service
where create_date > 
				(select max(create_date)
				 from service
				 where os_username = 'huangr')
				 
				 
2.多值运算符:
  =any 等价于 in
  <>all 等价于 not in 
  >all 等价于 >(select max(..)...);
  >any 等价于 >(select min(..)...);
  
  
例:哪些os账号的开通时间比huangr晚;(多台unix服务器上都有各为huangr的os账号)
(比所有的满足条件的'huangr'都晚):
				 
select unix_host,os_username,create_date from service
where create_date > 
				(select max(create_date)
				 from service
				 where os_username = 'huangr')


select unix_host,os_username,create_date from service
where create_date >all 
				(select create_date
				 from service
				 where os_username = 'huangr')

上一个例题(比任意个满足条件的'huangr'都晚):

select unix_host,os_username,create_date from service
where create_date > 
				(select min(create_date)
				 from service
				 where os_username = 'huangr')

select unix_host,os_username,create_date from service
where create_date >any 
				(select create_date
				 from service
				 where os_username = 'huangr')

注:若子查询的返回结果仅为一个值,可以用单值运算符如'=';
        若子查询的返回结果可能为多值,必须用多值运算符如'in';


例:哪些用户有推荐人;
select id,real_name
from account
where recommender_id is not null;

例:哪些用户没有有推荐人;
select id,real_name
from account
where recommender_id is null;

select distinct recommender_id from account;
注:用distinct去重时如果有多个空值结果会返回一个空值作为这一类结果;


例：哪些客户是/不是推荐人:

select real_name
from service
where id in
		 (select recommender_id from account)

select real_name
from service
where id not in
		 (select recommender_id from account
		  where recommender_id is not null)
		  
select real_name
from service
where id not in
		 (select nvl(recommender_id,0) 
		  from account)
		  
注:这里不能用not in (select recommender_id from account)
的原因是not in()中的条件是与的关系,必须每个比较下来都不相等才能被选出,但是一旦比较到了null就会
就会发生无法比较的情况,因为任何值包括null本身和null都无法保证一定是不相等的,所以又由于recommender_id中
存在null,所以如果直接用not in (select recommender_id from account)返回的一定是空;


例:哪些os账号的开通时间是所在unix服务器上最早的;

select unix_host,os_username,create_date
from service
where (unix_host,create_date) in
        (select unix_host,min(create_date)
        from service
        group by unix_host)
        

例:哪些os账号的开通时间比所在unix服务器上的最早开通时间晚九天;
select unix_host,os_username,create_date
from service
where unix_host,to_char(create_date,'yyyymmdd') in(select unix_host,to_char(min(create_date)+9,'yyyymmdd')
                                                  from service
                                                  group by unix_host)
                                                  
                                                  
例:哪些os账号的开通天数等于同一台unix服务器上的平均开通天数;

select unix_host,os_username,round(sysdate-create_date)
from service
where unix_host,round(sysdate-create_date) in (select unix_host,round(avg(sysdate-create_date))
                                               from service
                                               group by unix_host)


3.关联子查询;

非关联子查询:子查询不会引用主表中的列作为条件;

关联子查询:子查询一定会引用主表中的列作为条件;


子查询的执行顺序/执行次数比较:

非关联子查询先执行子查询,执行次数为1次;

关联子查询先执行主查询,每一条主表的查询记录都要求执行一次子查询并根据条件检查其结果,执
行次数由主查询记录数所决定;

注意:
关联子查询在某些情况下使用的频率会较高,这些情况如:要求主表中某列在必须满足某条件的情况下,其他列还需满足不同的条件,此时,
如果用非关联子查询的where语句局限性就比较大,只能通过where A列,B列 ...条件语句...子查询内容; 最主要的原因是:非关联子查询
的条件语句只会运行一次,就像下例中同一台unix服务器,那很显然用一次条件语句查询是不能得出结果的,如果一定要用非关联子查询来
写这样的案列,那只能是类似: where unix_host in ... and round(sysdate-create_date)> (select round(avg(sysdate-create_date))
                                                                                  from service i
                                                                                  where i.unix_host=o.unix_host)
但是由于非关联子查询只会运行一次条件语句,那unix_host要么只限定为一个唯一的值如:where unix_host = xxxxxxxx and round(sysdate-create_date)> (select round(avg(sysdate-create_date))
                                                                                                                    from service i
                                                                                                                    where i.unix_host=o.unix_host)
不然像where unix_host in ...这样的语句相当于没有作用,因为它只执行一次,不出意外肯定满足条件;
这种情况下,需要多次利用条件语句对某列判断的就要用关联子查询,它的运行机制如下:

以下例,我们来研究一下关联子查询的执行机制;
例:哪些os账号的开通天数比同一台unix服务器上的开通平均天数长;

select unix_host,os_username,round(sysdate-create_date)
from service o
where round(sysdate-create_date)>(select round(avg(sysdate-create_date))
                                  from service i
                                  where i.unix_host=o.unix_host)

执行机制:
首先当主表运行到where条件语句时,将把service o表中的第一条内容拿出检查,将它的round(sysdate-create_date)算出;
然后,运行子查询的内容,还是在service表 i中,但是规定了查询条件为此表(i表)中的列unix_host必须和正在接受条件语句判断
的主表o中unix_host相同,所以只有和o表第一条内容的unix_host相同的行才会在子查询中被选出,选出的内容组成了一张新的表;
之后,通过avg和round函数从此表中得到一个确切的数(平均开通天数);
最后,将刚开始从o表中拿出的第一条内容通过筛选条件得出的确切round(sysdate-create_date)和子查询中得出的确切
round(avg(sysdate-create_date)进行比较,如果满足条件(前者>后者)则此行内容将被选出,不然就不被选出,接着继续把service o表
中的第二条信息取出做相同的判断,知直到o表中所有内容都被判断过了,那选出的所有行信息就是最终满足where语句的所有内容;
这就是为什么说关联子查询一定会引用主表中的列作为条件,并且每一条主表的查询记录都将执行一次子查询并根据条件检查其结果;


例:哪些客户是推荐人;

select real_name
from account o
where exists 
      (select 1 
       from account i
       where o.id = i.recommender_id)
       
注:exists函数返回boolean类型的结果(为true则此条件满足,为false则算不满足条件),只要其参数有返回结果就算true,如果没有返回值则false;


例:哪些客户申请了远程登录业务;

非关联子查询:
select real_name
from account
where id in(select account_id
			from service)

关联子查询:
select real_name
from account o
where exists(select 1
             from service i
             where o.id=i.account_id)


例:哪些客户不是推荐人:

select real_name
from account o
where not exists(select 1
             from service i
             where o.id=i.account_id)

注:这里的not exists语句要求其中参数没有返回值,也就是都不满足才会返回true;


例:哪些客户没有申请远程登录业务;

select real_name
from account o
where not exists(select 1
                 from service i
                 where o.id=i.account_id)

select real_name
from account
where id not in(select account_id
                from service)


4.in/exists比较:

exists是用循环(loop)的方式,由outer表的记录数决定循环的次数,对于
也就是说外表的每一天记录都会运行一次exists函数,
所以其记录数较少时使用exists效率更高;

in先执行子查询,子查询的返回结果去重后,再执行主查询,所以子查询的返
回结果越少效率越高;


5.多表查询:

多表查询的作用:
结果集中的记录保存在多张表中;

多表连接的种类,根据结果集生成的规则不同,连接可以分为:


(1)交叉连接(cross join);
select a.real_name,s.os_username
from account a cross join service s

结果:
REAL_NAME		OS_USERNAME
aa              a1
aa              a2
aa              a3
aa              a4
aa              a5
bb              a1              
bb              a2
bb              a3
bb              a4
bb              a5

注:account表中的每一条记录将会和service表中的所有记录连接一次;
这样的结果集被称为笛卡尔积;

但是要取得有意义的记录通常需要添加条件;
select a.real_name,s.os_username
from account a cross join service s
where a.id = s.account_id;



(2)内连接(inner join);  //inner可以省略; 必须添加on语句来限定条件;


例:哪些客户在unix服务器上申请了远程登录业务,是哪些业务;

select real_name,a.id,s.account_id,os_username
from account a join service s
on a.id = s.account_id;

nested loop; 嵌套循环;

内连接执行机制:
驱动表,匹配表：驱动表的每一项根据on条件分别去匹配表遍历寻找符合条件的所有项并显示;
除了可以连接两张不同的表中的内容外,其他匹配机制和关联子查询非常相似,以下例来研究其运行机制:

例:客户huangrong在哪些unix服务器上申请了远程登录业务;

select real_name,a.id,s.account_id,os_username
from account a join service s
on a.real_name = 'huangrong'
and a.id = s.account_id

运行机制:
首先来到from语句,驱动表为account a表,匹配表为service s表;
然后根据on条件的内容来从account a表中分别取出每一行来遍历匹配service s表中符合条件的项;
如:先取出account a 表中的第一行内容,如果此行中的real_name为'huangrong'则继续执行,如果不是则不选出,不会执行接下去的条件检查,而是取出a表
中的第二行内容继续;
如果选出的a表中此行的real_name内容符合,则再去遍历比较s表中所有的account_id,满足的项会被和a表中此行的id连接成了若干新的行并被选出
(这点和关联子查询的子查询代码块中的运行机制类似);接着再从a表中选出下一行内容继续进行on条件的筛选过程;
最终当a表所有行都被检查过了以后,所有满足条件并连接后的行组成了一个新的表格,这就是最后选出的内容;

注:过滤条件尽量放在连接条件之后,执行效率会更高;


例:有哪些客户在192.168.0.26上开通了远程登录服务;

select real_name,a.id,s.account_id,os_username
from account a join service s
on s.unix_host = '192.168.0.26'
and s.account_id = a.id  


例:列出客户姓名以及开通远程登录业务的数量;

select max(a.real_name),count(*)
from account a join service s
on a.id = s.account_id
group by a.id

注1:这里不能按real_name分组因为不同id的客户可能拥有相同的姓名;
注2:这里的select后不能直接跟real_name,因为已经用过group by分过组了;


例:列出客户姓名以及开通远程登录业务的数量;

select a.real_name,c
from account a join
               (select account_id,count(*)
                from service
                group by account_id) c
on a.id = c.account_id

//在from语句后可以以子查询作为一张表来与原表连接;
 注意此时一定要在子查询后加别名,不然无法引用;


例:哪些os帐号的开通时间比同一台机器上os帐号的平均开通时间长;

关联子查询:
select unix_host,os_username,unix_host,round(sysdate-create_date)
from service o
where round(sysdate-create_date) >
          (select round(avg(sysdate-create_date))
           from service i
           where o.unix_host = i.unix_host
           )
           

多表连接:
select s.unix_host,s.os_username,round(sysdate-s.create_date) ,u.x
from service s join
              (select unix_host,round(avg(sysdate-create_date)) x
               from service
               group by unix_host) u
on s.unix_host = u.unix_host
and round(sysdate-s.create_date)>u.x    
           
           

(3)自连接(同一张表中的不同列要产生关系);


例:列出客户姓名以及他的推荐人姓名,如果没有推荐人请在推荐人一列写:No Recommende;

select b.real_name CUSTOMER,decode(a.real_name ,b.real_name , 'No Recommender', a.real_name) RECOMMENDER 
from account a join  account b
on a.id = nvl(b.recommender_id,b.id)

注意:
由于此表的结构大致为:
 id   real_name   recommender_id
 所以要解决此问题就必须利用别名同时对同样的两张表做不同的操作;
当多表链接完成后,组成了一张新的表,此表中的两列id此时是相同的,
最后选出的b.real的含义是从新表中属于b表区域的recommender_id处
取出起原表中对应得real_name项,其含义就是被推荐人;
而a.real的含义是从新表中属于a表区域的id处取出原表中对应的real_name
项,其含义就是推荐人;


例:那些客户是推荐人;
select distinct a.real_name
from account a join account b 
on b.recommander_id = a.id


(4)外连接(outter join)

内连接
from t1 join t2  
on t1.id = t2.id

外连接
from t1 left (outer) join t2   left代表t1一定是驱动表,t2为匹配表;
on on t1.id = t2.id

from t1 right (outer) join t2
on on t1.id = t2.id

from t1 full (outer) join t2
on on t1.id = t2.id

外连接的运行机制:
当匹配条件时驱动表在匹配表中能找到相匹配的内容时,它对这条记录的运行机制和内连接完全相同,
但是如果找不到能够匹配的记录时,和内连接不一样的是,内连接不会将这条记录选出,但是外连接会在
驱动表中取出这条记录,然后在本该由匹配表和它连接的部分用一个空值与其连接;


例:列出客户姓名以及他的推荐人姓名;
select b.real_name CUSTOMER ,nvl(a.real_name,'No recommender') RECOMMDANDER
from account a RIGHT outer join account b
on a.id = b.recommander_id

注意:这里用right join的原因为,在最后显示需要b.real_name来显示所有(包括有/没有推荐人)的客户姓名,那么如果
使用left join的话a表中所有id会被取出,但是那些没有当过推荐人的客户的记录,也就是a.id在b.recommander_id中
找不到能够匹配的记录的项将被直接以空值的形式与a表中没有当过推荐人的客户的id连接,这也直接导致了最后在
select b.real_name语句时,这些客户的信息根本无法找到,因为连接后的新表中b表中存在的可查询记录只剩下那些当
过推荐人的客户,而这些记录所对应的real_name就是有推荐人的客户姓名,所以就无法在此处显示所有(包括有/没有推荐人)客户信息;
而使用right join则b表中所有的recommander_id记录都被选出(就算为null的记录也被选出),能和a表中id匹配的客户
记录当然照常被取出并和a表相应客户id记录连接,那些匹配不了的也就是b表中recommander_id为null的项,将和一个空值连接;
这样的话,在运行到语句:b.real_name时无论此条recommander_id记录中是否为Null,都会取出原表中相应的客户real_name，
相应地,和recommander_id为空的记录连接的那些a表处的空值也被处理为'No recommender';


例:列出客户姓名以及所开通的远程登录业务的信息(没有申请远程登录业务的客户也要出现在结果集中);

select a.real_name , nvl(b.unix_host,'No service'), nvl(b.os_username,'No service')
from account a left outer join service b 
on a.id = b.account_id


例:列出客户姓名以及所开通的远程登录业务的数量(没有申请远程登录业务的客户也要出现在结果集中);

select max(a.real_name) ,count(s.account)
from account a left outer join service b 
on a.id = b.account_id
group by a.id

注意:select语句中如果写count(*)/count(a.id)都是根据实际的记录数来计算,也就是不会出现0;


上例用先统计后连接的方式:

select a.real_name,nvl(s.cnt,0)
from account a left join
                    (select account_id,count(*) cnt
                    from service
                    group by account_id) s
on a.id = s.account_id


例:哪些客户不是推荐人(用外连接解决);

select a.real_name,b.real_name
from account a left join account b
on a.id = b.recommender_id
where b.id is null

注:类似这种否定问题使用外连接,这里where语句中筛选用b.id是由于id在account表中本是非空列,
而这里b.id为空的唯一原因就是外连接过程中b.recommender_id没有被a.id匹配上从而使得外连接结果集
中的这一行的b表所在处被赋予了空,那此时b表处的b.id为空的行就一定是结果集中无匹配的
行,而假设用b.recommender_id来筛选外连接结果集中的行内容,则可能会出现b.recommender_id
的空是被匹配上的情况而不一定是未匹配上的(如果a.id也存在null的情况),就不能实现
筛选非匹配项的作用;


外连接的应用场景:
1.驱动表的记录需要在结果集中被全部显示,不管是否符合条件;
2.否定问题(不匹配问题),需要配合where语句和匹配表的非空列is null来实现;


host表内容: 此表中代表所有的机器及其名称,但是它们不一定都被使用了,只有在service表中有的机器才被注册使用了;
ID       机器的地址;
NAME     机器名;
LOCATION 机器的使用地址;


例:哪些UNIX服务器上没有开通os帐号;

select a.id , a.name
from host a left join service b
on a.id = b.unix_host
where b.id is null


例:哪些UNIX服务器上没有os帐号weixib;

select a.id,a.name
from host a left join 
(select unix_host from service
where os_username='weixb') b
on a.id=b.unix_host
where b.id is null

select a.id,a.name
from host a left join 
service b
on a.id=b.unix_host
and b.os_username = 'weixb'
where b.id is null

执行顺序:from (host service) -> on (先执行and后关于匹配表的筛选内容,b.os_username = 'weixb')
-> left join (a.id=b.unix_host,过滤后选出结果集)-> where 通过结果集中匹配表的列对外连接结果集进行第二次过滤)
-> select 显示选出结果集;



外连接注意点:
1.在外连接之前用匹配表的列过滤(对匹配表本身过滤)用on中的and语句来实现;
     在外连接之后用匹配表的列过滤(对外连接的结果集过滤)用where语句来实现;
     
2.对驱动表的列过滤无论是连接前还是连接后都必须用where语句实现;
(对于驱动表来说连接前/后过滤的效果是相同的,但是连接前过滤效率更高)

3.on和where后都可以跟多个条件表达式,表达式之间用and连接;


age segment表,其结构类似：
id  name        lowage    hiage
0   少年                      10        19 
1   青春期                 20        29
2   中年                      30        49
3   老年                      50        ***


例:显示客户的年龄段:

select a.real_name,round((sysdate-birthdate)/365) age,s.name
from account a join age_segment s
on round((sysdate-birthdate)/365) between s.lowage and s.hiage


例:huangrong是属于哪个年龄段的;

select a.real_name,round((sysdate-birthdate)/365) age,s.name
from account a join age_segment s
on round((sysdate-birthdate)/365) between s.lowage and s.hiage
and a.real_name = 'huangrong' //这里用and对a表列进行筛选,注意这里是内连接,所以a表也不是所谓的驱动表,不需要用where来实现;


例:那些客户是属于中年期的;

select a.real_name,round((sysdate-birthdate)/365) age,s.name
from account a join age_segment s
on round((sysdate-birthdate)/365) between s.lowage and s.hiage
and s.name = '中年'


例:显示各个年龄段的客户数(没有客户的年龄段不用显示);

select max(s.name), count(*)
from account a join age_segment s 
on round((sysdate-birthdate)/365) between s.lowage and s.hiage
group by s.id


例:显示各个年龄段的客户数(没有客户的年龄段的客户数为0);

select max(s.name),count(a.id) cnt
from account a right join age_segment s 
on round((sysdate-birthdate)/365) between s.lowage and s.hiage 
group by s.name


例:哪些年龄段没有客户;

方法一:
select max(s.name)
from account a right join age_segment s 
on round((sysdate-birthdate)/365) between s.lowage and s.hiage 
group by s.name
having count(a.id) = 0

方法二:
select s.name 
from account a right join age_segment s 
on round((sysdate-birthdate)/365) between s.lowage and s.hiage 
where a.id is null



6.集合:set

1.若将两张表看成集合,匹配问题就是集合运算中的交集,不匹配问题就是集合运算中的差,匹配+不匹配问题就是集合运算中的并集;

2.集合运算要求两个select语句是同构的,即列的个数和数据类型必须一致;

3.union/union all; 并集,前者会去重,后者保留重复;

4.intersect ; 交集：

5.minus ; 差;



例:当月包在线时长为20小时,单位费用涨5分,为40小时涨3分,其他不变(用union all实现);

select base_duration,unit_cost+0.05
from cost
where base_duration = 20
union all 
select base_duration,unit_cost+0.03
from cost
where base_duration = 40
union all
select base_duration,unit_cost
from cost
where base_duration not in(20,40)
or base_duration is null


例:列出客户姓名以及他的推荐人(包括所有的客户);

select b.real_name customer,a.real_name recommender
from account a join account b
on a.id=b.recommender_id
union all
select real_name,'No recommender'
from account
where recommender_id is null


例:sun280和sun-server上的远程登录业务使用了哪些相同的资费标准;

select cost_id
from host h join service s
on h.id = s.unix_host
and h.name = 'sun280'
intersect
select cost_id
from host h join service s
on h.id = s.unix_host
and h.name = 'sun-server'


上例需要选出资费的名称:

select name 
from cost

where id in(
select cost_id
from host h join service s
on h.id = s.unix_host
and h.name = 'sun280'
intersect
select cost_id
from host h join service s
on h.id = s.unix_host
and h.name = 'sun-server'
)


例:哪台UNIX服务器上没有开通远程登录业务;
select name, location
from host
where id in(
select id 
from host
minus
select unix_host 
from service 
)


7.ROWNUM (分页/排序)

rownum是一个伪列,方便查询返回的行编号即行号,由1开始递增;


例:
select rownum,real_name
from account
where rownum = 1/where rownum<=2
类似 where rownum=2/ where rownum between 2 and 5这样的语句是无法执行的,
因为oracle内部机制决定了如果要选出伪列的一行或多行必须以从第一行起连续的取出行作为条件来选取;
但是可以利用minus来解决选择离散行的目的,如:选出rownum为4,5,6,的三行内容:

select rownum,real_name
from account
where rownum<=6
minus
select rownum,real_name
from account
where rownum<=3

也可以先选出包含rownum与目标列的结果集作为一张表,然后再去筛选需要的行：

select rn,real_name
from (select rownum rn,real_name
from account 
where rownum<=6)
where rn between 4 and 6


例:最后开通NetCTOSS系统的三个客户;

select rownum, real_name,create_date
from
(select real_name
from account
order by create_date desc)
where rownum<=3

注意:由于order by的语法/执行顺序都是在最后执行的所以不允许出现类似:

order by create_date desc
where rownum<=3

这样的语句;


例:倒数4到6位开通NetCTOSS系统的客户;

select rn,real_name,create_date,
from
(select rownum rn,real_name,crete_date
from
(select real_name,create_date
from account
order by create_date desc)
where rownum<=6)
where rn between 4 and 6



8.sqo脚本;

用文本文件保存一段sql代码,如:
test.sql
保存在d盘;

这样的文件被称为sqo脚本,每一条sql语句后需要";"来标识;

然后在Oracle SQL Developer中输入:
@d:\test.sql  //oracle会逐行去执行文件中的代码;



9.constraints; 表的约束;

适用环境:
DDL: create table
列名 数据类型(number char varchar2 date) 长度
insert into
select 


需解决问题:
(1)某列必须非空且唯一;
(2)某列的取值受到另一列取值的限制;

目标:
限制无效的数据无法进入到表中;


约束类型:
(1)primary key; 主键约束保证了唯一且非空;
(2)unique key; 唯一键约束;
(3)not null; 非空约束;
(4)references  foreign key; 外键约束;
(5)check; 检查约束;


op:
create table test(
c1 number,
c2 number);

insert into test values(1,1);
insert into test values(null,null);

test表出现了相同的元素和Null元素;


op:
create table test(
c1 number constraint test_c1_pk primary key,  //c1被定义为主键列,保证了此列中不能出现空值/无法出现相同的值;
c2 number );

insert into test values(null,null); //报错！无法将空值插入test表c1列;

insert into test values(1,1);
insert into test values(1,2);  //报错！ 这条语句违反了唯一性约束：test_c1_pk;


//两种约束格式:
 
(1)列级约束：

create table parent
(c1 number(2) constraint parent_c1_pk primary key ,
c2 number
);

(2)表级约束(可以实现联合主键约束):

create table parent
(c1 number,
 c2 number,
 constraint parent_c1_pk primary key(c1,c2),
 c3 number);

//联合主键:此处的primary key(c1,c2)表示的是联合主键,也就是说c1,c2列联合起来不能出现重复值,他们作为列单独出现重复是允许的;
但是必须满足两个列都不能出现空值;

insert into test values(1,1,1);
insert into test values(1,2,1);  //正确;
insert into test values(1,null,1); //错误！主键列必须为非空值;


//对存在的表增加主键约束,通过表级约束的形式实现;
  
alter table parent
add constraint parent_c1_pk
primary key(c1); 

注意:在已存在的表增加主键约束时需要满足目标列必须已经满足primary key的条件,不然无法添加约束条件;


op：
create table test(
c1 number primary key);

注意:此处省略了constraint 修饰符和约束名;那么在报错时系统就会给出一个系统默认的错误代码,
如:unique constraint (HILOO.SYS_C00222975);所以最好还是给每一个约束起名,这样在修正错误时,
便于查找原因;


op:
alter table test add(c2 number);
alter table test add
constraint test_c2_pk primary key(c2);

注意:这里会报错:table can have only one primary key; 一张表只能存在一个主键约束;



//not null; 非空约束(一张表可以有多个not null约束);
  
   不允许该列的值为空,只有列级约束形式;
   
  
  
//unique; 唯一键约束(一张表可以有多个unique约束);
  
   不允许该列的值重复(可以存在多个空值);
 
op(列级):
create table test
(c1 number(3) constraint test_c1_pk primary key,
 c2 number(2) constraint test_c2_uk unique,
 c3 number(4) constraint test_c3_uk unique);
  
  
 op(表级)：
 create table test3
(c1 number(3) test_c1_pk primary key,
 c2 number(2),
 c3 number(4),
 constraint test3_c2c3_uk unique(c2,c3));
 
 注意：这里属于一个联合unique约束,其机制和联合primary key相同;
 
 
 op:
 alter table test2
 modify (c2 not null); //这里需要用modify关键字类添加已存在表中某列的非空约束;
 
 
 check; 检查约束;
 
 op(列级):
 create table test(
 c1 number(3) constraint test_c1_pk primary key,
 c2 number(3) constraint test_c2_ck check(c2>100));
  
 op(表级):
 create table test(
 c1 number(3) constraint test_c1_pk primary key,
 c2 number(3) ,
 c3 bumber(2) ,
 constraint test_c2c3_ck check(c2+c3>100)/
 constraint test_c2_ck check(c2 is not null)/
 constraint test_c2_ck check(c2 in (1,3,5))
 );
 
 注意:也存在联合约束,check(c2+c3>100);
 
 
 
 //需求分析->E-R图:entity-relationship实体关系图;
 
 entity: 客户;  服务;  资费;
 
 relationship: 实体之间关系;
 
 如:
 1:n(客户:服务 ; 资费:服务)
 m:n(客户:资费; 学生:课程)
 1:1(夫妻)
 
 
//references;
  foreign key;  外键约束;
  
op(列级):
create table child
(c1 number(2) constraint child_c1_Pk primary key,
 c2 number(3) constraint child_c2_fk
              references parent(c1)
 );
 
 注意:报错:parent表不存在; 必须定义parent表才能创建外键约束;
 
 
 op:
 create table parent(
 c1 number(3)
 );
 
 create table child
(c1 number(2) constraint child_c1_Pk primary key,
 c2 number(3) constraint child_c2_fk
              references parent(c1)
 );
 
 注意:这里会报错:在parent的c1列未定义为unique/pk,references约束的引用列必须是受唯一约束限制的;


 op:
 alter table parent
 add constraint parent_c1_pk primary key(c1);
 
 create table child
(c1 number(2) constraint child_c1_Pk primary key,
 c2 number(3) constraint child_c2_fk
              references parent(c1)
 );

 Table created.  //成功建表;
 
 
 op:
 insert into child values(1,1);
 
 注意:这里发生报错:integrity constraint;  这个关键字说明违反了外键约束;
 原因是parent表的c1列中不存在1这个元素;


op:
delete from parent where c1 = 1;

注意:在删除父表的c1列元素时发生报错:integrity constraint; 因为子表中已经对父表
的c1列声明了外键约束,且parent的列c1中的1已经在child表中创建了,删除parent表中的
此元素会直接导致child表中所声明的外键约束被违反,所以报错;
如果要删除parent表中c1列中的1元素,需要:

delete from child where c2=1;

1 row deleted.

delete from parent where c1 = 1;

1 row deleted.


op:
drop table parent purge;

注意:这里会报错:parent表中的unique/pk正在被child表的外键引用,所以无法删除;
需要先删出子表中的外键相关内容或者外键约束才能再删除父表;


//cascade constraints; 约束级联化删除;
 
op:
drop table parent cascade constraints purge;

相当于做了这样的操作:

alter table child 
drop constraint child_c2_fk;

drop table parent purge;

cascade constraints; 关键字使oracle先断开了所有引用了待删除父表中uk/fk约束列的子表的外键约束,
然后再删除父表;


op(表级):

create table parent(
c1 number(3) constraint parent_c1_pk primary key
);

create table child(
c1 number(2) constraint child_c1_pk primary key
c2 number(3),
constraint child_c2_fk foreign key(c2)
           references parent(c1)
);


//on delete cascade; 级联删除;
 
create table child1(
c1 number constraint child1_c1_pk primary key,
c2 number(3) constraint child1_c2_fk
             references parent(c1)
             on delete cascade);
             
注意:这里的on delete cascade; 关键字会让父表删除被子表外键约束引用的相关内容时先将
子表中的相关内容删除再删除父表中的待删除内容,但是子表中相关列的外键约束仍然存在未被删除;


op:
insert into parent values(1);

insert into child1 values(1,1);

delete from parent where c1=1;  //成功删除;

相当于:

delete from child1 where c2 = 1;
delete from parent where c1 = 1;


//on delete set null; 置空级联删除;

op:
create table child2(
c1 number constraint child2_c1_pk primary key
c2 number(3) constraint child2_c2_fk
             references parent(c1)
             on delete set null);

insert into parent values(1);
insert into child2 values(1,1);

delete from parent
where c1 =1;  //删除成功;

注意：
在子表中对声明了外键约束的列添加on delete set null关键字后,父表在删除其中
正在被此外键约束引用的元素时会先将子表中的相关置空,与on delete cascade
关键字不同的是,被删除的子表中内容会变为null,而不是不存在了,同样,子表中相关列的外键约束仍然存在;

相当于:

update child2
set c2 = null
where c2 = 1;

delete from parent 
where c1 = 1;



//一对多关系:
 如:  客户pk               fk  服务
     account      service(account_id)
 
 
//多对多关系:
 如: 客户pk                fk  服务  fk        pk资费
    account                 service            cost
                    (account_id   cost_id)    

注意:多对多关系都需要一个中间表来实现;


//一对一关系:
 
1:1 表; 通过合表来实现;

如果1:n表通过合表来实现会在某些列出现重复信息,占用更多空间,造成数据冗余;


//按照范式设计表结构:
 
第一范式:有主键,不会记录重复的项;每一列的属性单一(仅描述一类事物,不可再分);


第二范式:每个非主属性必须完全依赖于单一的主属性;这点是针对联合主键而言的,如果一些非主属性仅仅满足了
联合主键中的其中一个,而另一些非主属性满足了联合主键中的另一个,那么整张表就会显得冗余,因为,联合
主键的任何一个列都是可以重复的;
比如:
(多对多关系合表)
student_id(pk)   class_id(pk)   student_name   student_score
1                1              s              90
2                1              d              90
3                1              f              90
1                2              s              90
1                3              s              90
student_id和class_id为联合主键,联合主键的任何一个列都是可以重复的;
所以表的前三行很显然是以依赖student_id(pk)来组建的;
而表的后3行是以class_id(pk)来组建的,那就难免出现了冗余的信息;


第三范式:非主属性之间不能有依赖关系,也就是只有主键能决定其它非主属性的内容;
如果某一非主键列由另一非主键列决定,那由于只有主属性规定了其中元素不能重复,那这一列
由非主键列决定的列就会产生重复内容而不被限制;
比如:
(一对多关系合表)
service_id(pk)    account_id    real_name
1                 a             ss 
2                 a             ss
3                 b             zz

service_id为主键,那么account_id由service_id决定,虽然出现了重复项,但是其意义是由不可
重复的主键service_id决定的,所以其意义是不同的;而real_name是可以直接由account_id决定的,
那account_id是可以重复的,那由account_id决定的real_name不仅形式上会发生重复,而且其意义
也会发生重复,这样就造成了信息的冗余;


1:1表;

A id(pk) fk --> B id(pk);  //id相同,就能实现了1对1关系的表;



10.transaction;  //事务;

事务是由一组DML语句和commit/rollback组成,是改变数据库数据的最小逻辑单元,如果是commit,
表示数据入库;如果是rollback,表示取消所有的DML操作;


事务的结束:
-commit/rollback;
-DDL语句会自动提交(commit);


事务的开始:
-一个事务的结束就是下一个事务的开始;


ATM: Automatic Teller Machine;  //自动柜员机;

这里从数据库层面可以被理解为: Automatic Transaction Machine;  

假设从A银行卡转账2000到B银行卡; 

account(card_id,balance);

update account set balance = balance - 2000
where card_id = 'A'
update account set balance = balance + 2000
where card_id = 'B';

注意:以上语句属于分步操作,从理论上讲如果硬件条件等发生异常很可能导致程序只执行了一部分而导致
用户数据的不安全;所以需要把这样的多条语句转化为一个事务,其实就是若干DML的结合,另分步操作整合
为整个程序的一个原子操作;

正常的系统数据库会出现并发session,从而会出现并发事务;


事务的特性:

原子性(atomictiy);
-一个事务或其中语句完整执行,或一条也不执行;

一致性(consistency);
-事务把数据库从一个一致状态转变为另一个一致状态,也就是说数据库中数据的变化与事务的
指令是一致的,不会只改变事务要求改变的部分数据,而是同步改变全部数据,和原子性有一些相关;

隔离性(isolation);
-在事务提交之前,其他事务觉察不到事务的影响,也就是说一个事务去访问/修改另一个数据能够
看到的一定是那个事务提交前/提交后的状态;

持久性(durability);
-一旦事务提交,它是永久的;


数据库开发的关键挑战:
在开发多用户,数据库驱动的应用程序中,关键性的挑战之一是要使并行的访问量达到最大,同时还要
保证每一个用户(会话)可以以一致的方式读取并修改数据;


锁(lock)机制;

-用来管理对一个共享资源的并行访问;

如:如果一个session 用DML修改一条记录,但是未commit,那这个事务就并未结束,那另一个session就无法
update这条记录,会进入等待;一旦第一个操作此条记录的事务提交了操作,另一个session马上就获得了
更新;
另外,如果在一个事务处于正在修改数据库状态,也就是上锁状态时,DDL的删表操作会直接产生报错;

Oracle是以数据行为单位上锁的,其他的数据库是以数据块的单位上锁的,一个数据块中包含若干数据行信息;
所以锁的级别越低并发实现越好;


DML操作引发的锁:

在行记录上添加排他锁(X锁); (也就是DML操作引发的X锁属于事务锁即行级锁);
-排他锁:如果一个对象上加了X锁,在这个锁被采用后,知道commit/rollback释放它之前,该对象上不能施加任何其他类型的锁;


在表上添加共享锁(S锁); (也就是DML操作引发的S锁属于意向锁即表级锁);
-共享锁:如果一个对象被加上了S锁,该对象上可以加其他类型的S锁,但是,在该所释放之前,该对象不能被加任何其他类型的X锁;

如:
三个session,s1,s2,s3同时与数据库建立了连接;
s1修改了table的c1列第一行,成功修改,并未提交; s1获得了c1列第一行的X锁; 同时获得了table的S锁;
s2修改了table的c1列第一行,修改语句无法执行,处于等待状态; 这条记录由于s1的X锁未释放,所以无法被修改; 但是s2获得了table的S锁,对于同一个table而言可以加上不同类型的S锁;
s3修改了table的c1列第二行,成功修改,并未提交; s3获得了c1列第二行的X锁; 同时获得了table的S锁;


DDL操作引发的锁:

在表上添加排他锁(X锁);
这样的锁防止其他会话在DDL期间修改表,但是可以查询表;

所以,如果s1的修改并未提交,那么对table的DDL操作将会报错,因为table表此时已经被加上了S锁,无法再添加如何的
X锁;


Oracle的另一个比较优秀的设计是,在一个表或者行记录被锁住时,虽然无法在此时对其内容进行随意的修改,但是用
类似select查询其中内容是可以的,查询到的内容是还未被修改的内容;


ORACLE在用delete删除数据时会把被修改的内容放入回滚段资源,方便之后的rollback操作;
在修改内容不被提交的状态下,其资源将一直被占用,回滚段资源空间是有限的,会导致无法删除;
所以在删除大表时不要使用delete from删除语言,用truncate table; 
truncate; 不会将表的结构删除,只会将其中的数据删除,属于DDL语句,自动提交,不会保存到回滚段空间;


结合以上来看,事务必须及时提交!


savepoint; 保留点;

用savepoint在当前事务里创建一个保留点;
用rollback to savepoint命令将事务回滚到标记点;

如:
insert;
update;
SAVEPOINT update_done;
(Savepoint created)
insert;
rollback to update_done;
(Rollback complete)


定义缺省值:Default;
给一列定义缺省值以后,这一列就算没有被赋值,它的内容不会为空,而是其定义的缺省值;

例:
drop table test purge;
create table test
(c1 number default 1,
 c2 number );

insert into test(c2) values(2);  //此时表中的c1中的值为1;
insert into test values(default,3);  //也可以插入default关键字来代表插入缺省值;

insert into test values(4,4);
update test set c1 = default;
where c1 = 4;



/////////////////////////////////////////////////////
 
项目:电信收费系统;
NetCTOSS业务;

背景:
1.为客户提供远程登录服务(业务)(telnet);
2.一个客户可以在多台Unix服务器上申请远程登录服务(业务);
3.每一个远程登录服务申请一种资费标准;
4.一个远程登录服务在一月中可以产生多条详单,一次登入登出视为一条详单;
5.对远程登陆服务采用按月计费的方式,根据每月的登录时长和资费标准计算费用;


脚本(参考经典案例):
由于需要反复修改运行脚本,而反复的添加表需要先把表删除再添加;
所以脚本中的语句应该是先drop再create表,而drop一张表需要先取消
所有引用了这张表内容的外键,所以drop语句需要添加cascade constraints purge;
所以第一次运行脚本由于所有表未创建,所以drop操作会报错;
如:
drop table account cascade constraints purge;
create table account;
insert...
......
commit;
drop table cost cascade constraints purge;
create table cost;
......
......
service
service_detail


在创建表时可能会遇到加入需要利用外键引用别的表/列中的主键信息但是主键信息尚未创建
的情况,如在同一张表中,account_recommender外键指向了此表中的account主键列,那在插入
行信息的时候就会发生account_recommender外键无法建立;

所以同一张表有主外键关系时,插入数据的顺序是先插主键记录数据(父记录),再插外键制约的记录数据(子记录);

还可以在创建了表和其中列的外键约束后,暂时disable外键约束,再批量地插入记录;
批量处理数据后再enable约束可以提高插入效率;

alter table account disable/enable constraint 
account_recommender_id_fk


暂时结束主键约束需要注意的是此主键不能被外键约束引用,所以可以使用级联disable,
同时将引用此主键的外键约束disable;

alter table account disable primary key cascade; 


联合外键约束;

create table child(
c1 number,
c2 number,
constraint child_c1_c2_pk primary key(c1,c2),
c3 number,
c4 number,
constraint child_c3_c4_fk
foreign key(c3,c4) references parent(c1,c2));

注意:联合外键约束必须引用联合主键的内容,其列的个数是对应的;


例:所有开通了服务的客户的开通服务详情;

account,service,cost的合表:

select a.real_name,s.unix_host,s.os_user_name,c.name
from account a join service s   //一对多合表;
on a.id = s.account_id
join cost c                     //结果集与cost一对一合表,对于整个结果集来说就是多对多合表的结果;
on s.cost_id = c.id


//带子查询的create table语句:
 
create table tablename [(column1 , column2,...)]
AS subquery; //subquery就是之后的select语句;

如:
create table service_19
as
select * from service
where unix_host = '192.168.0.19';

注意:用子查询来建表,子查询的返回结果集中的记录也将被添加到
被新创建的表中; 并且原表中的非空约束也会被带到新创建的表中,
但是所有其它的约束将无法被带入新表;
带有子查询的create table语句中可以定义列名(会覆盖将复制进新表的
子查询结果集中的列名),缺省值,约束;


例:创建一张表account_90,表结构与account一致,没有数据;

create table account_90
as
select * from account
where 1=2 ;  //利用永非语句使得子查询结果集一定为空,这样account_90就获得了
             //account的所有结构(由于select * from),但是却不会获得其中记录
             //(只有子查询的结果集中的记录能够复制到新的表中);
也可以利用where 1=1;这样的语句将所有自查询表的记录复制到新表中;
其实如果只有select * from account,也会将所有account表中的记录复制到新表中去;


注意:
create table t10
as
select c1,count(*) cnt
from t1
group by c1

这样的语句的问题是,select语句后的函数表达式一定要给其起别名,不然这一列不能正常
的显示名称;


//带子查询的insert语句;

 如:
insert into new_table
(column1,column2,column3,...)
select column1,column2,column3,...
from original_table
where condition;
 
注意:
insert指定的列的数量要与select语句指定的列数量一致;
一次可以插入多条记录,不能和values子句联合使用;

 
例:account_90表中包含所有的90后客户;

create table account_90
as 
select * from account
where 1= 2;

insert into account_90
select * from account
where to_char(birthdate,'yyyy') between '1990' and '1999'; 



数据库除了table 外的其它一些类型(对象):

(1)synonym; //同义词;
如:
connect tarena/tarena;
grant select on account to jsd1405; //授权帐号jsd1405可以获得tarena帐号上account表的select(查看)权限;

connect jsd1405/jsd1405;
create synonym account for tarena.account;  //设置同义词使得jsd1405能够直接用account关键词访问到tarena帐号上的account表;
select * from account; 相当于select * from tarena.account;



(2)view;  //视图;
如:
create or replace view test_v1  //如果test_v1存在就替换,如果不存在就创建;
                                //起始view相当于一个快捷方式,存储的就是一条
                                //select语句,也就是select * from test; where c1 = 1;
as
select * from test
where c1 = 1;

View created.

desc test_v1;

和表结构显示相同;

select * from test_v1;

和表记录显示相同;

insert into test_v1 values(1,2);

1 row created.

select * from test_v1;

select * from test;

注意:
此时test表中也能看到在view中插入的内容;

insert into test values(1,3)

select * from test_v1;

注意：
此时test_v1中也能看到在表中插入的内容;

insert into test values(2,4)

select * from test_v1;

注意:此时在view test_v1中是无法看到c1 = 2, c2 = 4这条语句的,因为test_v1
所代表的就只是 select * from test; where c1 = 1; 这条语句;


如果此时将test表删除,在去查看test_v1表,那就会报错;

oracle内部存在系统表和用户表;
系统表(存储数据库信息): user_tables user_views user_objects...
用户表 ：account service cost (DDL); //用户子集创建的表;

注意:当用户自己创建的一张表时,系统表发生了insert操作,将此表的信息加入到了系统表中;
而当用户在创建了一张表的视图之后将原表删除,此视图的状态信息在系统表中将发生变化,导致
用户在查询次视图时会出现报错信息;因为drop一张表系统将自动把所有依赖该对象的对象的状态
更新为invalid;

如:
select object_name, object_type , status
from user_objects
where object_name = 'test_v1';

查询结果:
OBJECT_NAME   OBJECT_TYPE   STATUS
TEST_V1       VIEW          INVALID


当系统执行select * from test_v1; 
会先编译此对象更新对象的状态:
alter view test_v1 compile;  //编译失败;报错;

当重新创建原表后,view对象的状态也不会自动变为vaild;
再次执行select * from test_v1;语句时系统会再次编译
test_v1,之后次test_v1的状态更新为valid;

view的总结:
1.视图在数据库中不会存储数据值,不占用存储空间;
2.视图起始就是一条事先写好方便查询的select语句;把select语句以数据库对象的形式保存起来;


视图的应用场景:
1.简化操作,屏蔽了复杂的SQL语句,直接对视图操作;
2.控制权限,只允许查询表中的部分数据,普通授权将把整张表的内容暴露给访问者,而对视图的授权
将很好的控制访问者可以查询的内容;
3.通过视图将多张表union all成一张逻辑大表,作为单独的一个数据库对象,实现表的超集;

如:
create or replace view beijing
as
select * from haidian
union all
select * from xicheng
union all
select * from fengtai

select count(*) from beijing;


partition table; 分区表; 用来应对大表,将一张大表分成不同的物理区;


例:每个客户选择了哪些资费标准;

create or replace view view_s1
as 
select distinct a.real_name,c.name
from account a join service s
on a.id = s. account_id
join cost c
on s.cost_id = c.id


视图的分类:

1.简单视图:基于单张表并且不包含函数和表达式的视图,在该
视图上可以执行DML语句(既对原表执行增,删,该);

2.复杂视图:包含函数,表达式或者分组数据的视图,在该视图上
执行DML语句时必须要符合特定条件,不能随意改变统计数据;
需要注意为函数或者表达式定义别名;

3.连接视图:基于多个表建立的视图,一般来说不会在该视图上
进行inser,update,delete操作;


create or replace view test_v1
as
select * from test
where c1 = 1 with check option

insert into test_v1 values(2,3);  //报错;

注意:
"with check option"关键字相当于在view上添加了约束,
当执行insert into test_v1 values(2,3);时会报错,和之前插入
次数据时不同的是,之前可以插入但是由于where限制在view
中不能查看,但是添加了这个约束后插入不符合where条件记录的操作
将无法进行并报错;


create or replace view test_v1
as
select * from test
with read only

注意:
"with read only"关键字为view添加了约束:只读;



(3)index; //索引;

create index service_account_id_idx
on service(account_id);


全表扫描FTS(FULL TABLE SCAN);

高水位线HWM(HIGH WATER MARK):
曾经插入数据的最远块;

全部逐个扫描就是扫描高水位下以下的所有数据块(datablock);
delete 不释放表占用的空间(HWM不会改变);
truncate 释放占用的空间(HWM也将降低);

(关于HWM见本文最后的补充内容;)


//rowid; //伪列;
 
rownum主要解决分页和排序问题;
rowid是一条记录的物理位置;类似一个引用类型对象的hashcode;
在ORACLE看来,没有重复记录,每条记录都有不同的rowid;


rowid包含如下信息:

1.该记录属于哪张表(数据库对象); data_object_id;

2.该记录在哪个数据文件里; file_id;

3.该记录在数据文件的第几个数据块里:block_id;

4.该记录在数据块里是的几条记录; row_id;


去重操作;
delete from test o
where rowid < 
(select max(rowid) from test i
where o.c1=i.c1
and   o.c2=i.c2)


create index test_c1
on test(c1);

select c1,rowid from test

结果:
c1		rowid
1       AABKgNAAEAACMLnAAC
2       AABKgNAAEAACMLnAAD

index entry; 索引项; 
包含了以上例子结果中的两类值; (列的取值:key值 , rowid:value值);

oracle利用树状结构对index entry进行排序;系统在查找index entry时,先从
root块(有对列信息的取值范围信息)出发,在通过几步的分支块(将范围细分),最后
找到叶子块,叶子块是以链表的形式保存index entry,能够迅速在叶子块中找到
目标index entry,得到value值;

之后再运行类似语句:where c1 = 2;时,系统会先做判断,如果c1列上没有索引,则会使用
全表扫描的方式查找;如果c1列上有索引,oracle就会进入index entry中寻找key值为
c1=2的项,获取了value值(rowid)后,where c1 = 2; 这条语句就被转化为了where rowid = AABKgNAAEAACMLnAAD,
这样就直接定位到了c1=2的所在物理地址,相比之前的低水位查找要效率高很多;因为这样就降低了需要读取数据块的数量;

注意:
1.索引不能像table或者view一样用命令直接去查看,只能通过表中的rowid去观察行的物理地址;
2.索引的使用和更新时自动的,使用指的是当用户查询一个带有索引的列时,系统会自动使用索引
去查找记录;更新指的是当用于对添加了索引的列使用dml语句做了修改之后,系统将自动对其索引
也做相应的更新;

在表经过长时间的操作之后,索引需要维护来避免其中空数据占用空间不被释放而造成查找记录时
要遍历更多的数据块;那么在遇到需要删除一个索引时,可以使用语句:

drop index;
create index;

alter index test_c1 rebuild;  //重建索引; 并没有把老索引删除,而是用老索引的数据来定义相应的新
索引的内容,在新索引建立完毕后,老索引的空间被释放; 所以相对drop+create index 速度会更快,因为不需要
重新排序,但是会占更多的空间;

系统权限(dba);
grant create view to jsd1405;

对象权限(对象的owner,属主)
grant select on account to jsd1405


哪些列适合建索引:

1.经常出现在where子句;
2.表连接的列; 驱动表做的一定是全表扫描,但是匹配表会利用索引来快速定位数据;
3.该列是高基数数列(此列中的元素种类很多); 如果是低基数数列它的元素较为单一,
那么在查询此列信息时被查找出的记录很可能是此表的大部分记录,那么全表扫描会
更加的合适;
4.该列包含了许多null值; 因为索引不计空值; 所以where c1 is null;这样的语句一定是全表扫描;

5.主键列(PK),唯一键列(UK); 系统会自动为添加了这两个约束的列添加唯一索引,vice versa;
如:
create unique index test_index
on test(c1)
insert into test(c1) values (1);

报错:unique constraint ....;

6.外键列(FK), 外键列很可能就是表连接列;

7.经常需要排序(order by)和分组(group by)的列;  但是必须声明此列为非空列;


联合索引;

create index test_c1_c2 
on test(c1,c2);

联合索引中将保存c1,c2列的联合值作为key,并且拥有一个物理地址作为value;


一些会导致索引无法使用的因素:

1.函数导致索引无法使用;
where upper(colname)= 'whitney';

2.表达式导致索引无法使用;
where colname*12=12000;

3.部分隐式数据类型转化导致索引无法使用;
where colname = 2 ;  //这条语句就相当于where to_number(colname)=2;

4.like和substr
where colname LIKE 'CA%';  //可以使用索引;
where substr(colname,1,2) = 'CA'; //无法使用索引;

5.NULL;
where colname is null;

6.否定形式;
not in/<>;  //not exists 可以用索引,因为not exists其实就是表连接;


建立函数索引;

create index test_c1
on test(round(c1));

注意:这样的索引中直接存的就是函数转化之后的值;

一个索引如果没用了,需要及时drop掉;



(4)sequence; //序列号;

create sequence s1;
[incrementby 1/integer]
[start with integer]
[maxvalue integer/nomaxvalue]
[minvalue integer/nominvalue]
[cycle/nocycle]  //超过最大/小值时是否循环,默认条件是nocycle;
[cache 20/integer/no cache] //默认cache 20,当用户去一个值时,系统会取20个值放入内存,
而当把sequence设置为cycle时,必须保证一个cycle的数量必须不大于cache的设置值;

如:
create sequence s2
start with 1
increment by 1
maxvalue 5
cycle
cache 4;

select s1.nextval from dual;


例:制作一张表的一列按照jsd1306001开始递增;

create table test(c1 char(10));
create sequence test_c1
start with 1306001;
insert into test values('jsd'||test_c1.nextval);

create sequence test_c1;
insert into test values('jsd1306'||to_char(test_c1.nextval,'000'));

create sequence test_c1;
insert into test values('jsd1306'||lpad(test_c1.nextval,3,'0'));


补充:
标量子查询;
跟在select语句后的子查询;

例:列举客户和他们的推荐人,如果没有推荐人就显示No recommender;
select real_name,
                 nvl((select real_name from account i
                      where o.recommender_id = i.id),
                      'No recommender')
from account o;

注意:标量子查询的子查询返回记录必须是单行单列的;
执行顺序是:从account o 出发,先将结果集的第一列确定,real_name列;
然后去子查询按照account o 的每一行去筛选子查询中的条件判断是否满足,
如果满足则选出(类似关联子查询),最后将结果显示在结果集的第二列,相当于
将两列连接为最后结果集,这也就是为什么标量子查询的子查询返回记录必须是单行
单列的原因;

至此SQL告一段落;


补充:

在Oracle数据的存储中,可以把存储空间想象为一个水库,数据想象为水库中的水;水库中的水的位置有一条线叫做水位线,在Oracle中,这条线被称为高水位线（High-warter mark, HWM）;
在数据库表刚建立的时候,由于没有任何数据,所以这个时候水位线是空的,也就是说HWM为最低值;当插入了数据以后,高水位线就会上涨,但是这里也有一个特性,就是如果你采用delete语句删除数据的话,
数据虽然被删除了,但是高水位线却没有降低,还是你刚才删除数据以前那么高的水位;也就是说,这条高水位线在日常的增删操作中只会上涨,不会下跌;
HWM通常增长的幅度为一次5个数据块;也就是说如果高水位线下的空余数据块不够用了就会一次性加入5个数据块,所以总是会有还未被使用的数据块存在于高水位线下(手动模式下这些数据块都被初始化了,自动模式下它们只有在被存入数据使用时才会被初始化);
 
Select语句会对表中的数据进行一次扫描,但是究竟扫描多少数据存储块呢,这个并不是说数据库中有多少数据,Oracle就扫描这么大的数据块,而是Oracle会扫描高水位线以下的数据块;现在来想象一下,
如果刚才是一张刚刚建立的空表,你进行了一次Select操作,那么由于高水位线HWM在最低的0位置上,所以没有数据块需要被扫描,扫描时间会极短;而如果这个时候你首先插入了一千万条数据,然后再用delete语句删除这一千万条数据;由于插入了一千万条数据,
所以这个时候的高水位线就在一千万条数据这里;后来删除这一千万条数据的时候,由于delete语句不影响高水位线,所以高水位线依然在一千万条数据这里;这个时候再一次用select语句进行扫描,虽然这个时候表中没有数据,但是由于扫描是按照高水位线来的,
所以需要把一千万条数据的存储空间都要扫描一次,也就是说这次扫描所需要的时间和扫描一千万条数据所需要的时间是一样多的;所以有时候有人总是经常说,怎么我的表中没有几条数据,但是还是这么慢呢,
这个时候其实奥秘就是这里的高水位线了;

那有没有办法让高水位线下降呢,其实有一种比较简单的方法,那就是采用TRUNCATE语句进行删除数据;采用TRUNCATE语句删除一个表的数据的时候,类似于重新建立了表,不仅把数据都删除了,还把HWM给清空恢复为0;所以如果需要把表清空,
在有可能利用TRUNCATE语句来删除数据的时候就利用TRUNCATE语句来删除表,特别是那种数据量有可能很大的临时存储表;
 
在手动段空间管理（Manual Segment Space Management）中,段中只有一个HWM,但是在Oracle 9i Release1才添加的自动段空间管理（Automatic Segment Space Management）中,又有了一个低HWM的概念出来;为什么有了HWM还又有一个低HWM呢,这个是因为自动段空间管理的特性造成的;
在手段段空间管理中,当数据插入以后,如果是插入到新的数据块中,数据块就会被自动格式化等待数据访问;而在自动段空间管理中,数据插入到新的数据块以后,数据块并没有被格式化,而是在第一次访问这个数据块的时候才格式化这个块;所以我们又需要一条水位线,用来标示已经被格式化的块;这条水位线就叫做低HWM;
一般来说,低HWM肯定是低于等于HWM的;需要注意的是,低水位线下的一定都是已近被格式化过的数据块,但是由于数据存入数据块中并不是按照由低到高的顺序存入的,所以在低水位线和高水位线中可能会零散的出现一些被初始化了并使用了的数据块,所以在全表扫描时,数据库会先将低水位线下的所以数据扫描,因为它们
一定是都被格式化并使用过的,然后在低水位线和高水位线中查找已经被格式化过的数据块;

所以说索引要比全包扫描快,不过如果对索引所引用的数据表中经过了较多的删除添加记录操作,会造成索引中存在碎片的情况,所以有时需要重构索引来提高索引查找数据的效率;



(具体可参考页面: Oracle 高水位(HWM: High Water Mark)说明);


*/











