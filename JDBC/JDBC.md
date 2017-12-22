JDBC


package prototype;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBC {
	
	 private static String dbdriver = "oracle.jdbc.driver.OracleDriver";
	 private static String url = "jdbc:oracle:thin:@localhost:1521:song";
	 private static String uid = "system";
	 private static String pwd = "songsong";
	    
	 private static Connection conn = null;
	 private static Statement stat = null;
	 private static ResultSet rst = null;
	    
	 private static String SQLStatment = "select id,username,age,pwd from t_user ";

	public static void main(String[] args) {
			
				try{
				    Class.forName(dbdriver);
			
					conn = DriverManager.getConnection(url,uid,pwd);
					
					System.out.println("connected!");
					
					stat = conn.createStatement();
					
					rst = stat.executeQuery(SQLStatment);
					
					while(rst.next()){
						int id = rst.getInt("id");
						String username = rst.getString("username");
						int age = rst.getInt("age");
						String pwd = rst.getString("pwd");
						System.out.print("id:"+id+" username:"+username+" age:"+age+" pwd:"+pwd+"\n");
					}
				}catch(ClassNotFoundException e){
					e.printStackTrace();
					System.out.println("驱动类未找到！");
				}catch (SQLException e) {
					e.printStackTrace();
					System.out.println("访问数据库出错！");
				}finally{
					try{
					   if(rst!=null){
						   rst.close();
					   }
					   if(stat!=null){
						   stat.close();
						}
					   if(conn!=null){
						   conn.close();
					   }
					}catch(Exception e){}
					}
				}
	}


/*


JDBC; JAVA database connectivity;
是一种sun公司制定的一种通用的数据库访问接口,该接口由不同的数据库厂商来实现,
这样开发人员可以使用相同的方式来访问不同的数据库;
(mysql是开源免费的);
   
3个重要的接口:(这些接口由各数据库厂商来实现,将实现类打包(.jar文件,也就是驱动)提供给JDBC用户来使用)

Connection;  负责建立数据库连接(底层采用socket编程);
     |
Statement;   执行SQL语句的容器,负责将SQL语句发送给数据库; 
     |
ResultSet;   数据库将结果集返回给JVM端;
   

JDBC编程步骤:

1.加载驱动;

(1) 驱动: 由不同的数据库厂商实现的JDBC对应的JAVA类压缩之后生成的.jar文件;所以,访问某个数据库就必须使用对应的驱动;
   
(2) 驱动的放置位置在build path对应的路径里;
   
(3) Class.forName(String classname);
    forName方法: JVM依据classname找到类的字节码文件(.class文件),将字节码文件的内容读到方法区,生成一个class对象;
  
  
2.获得连接;

  Connection conn = DriverManager.getConnection(String url,String sername,String pwd);
     注意:
  (1)getConnection方法会返回一个符合Connection接口要求的对象,称之为连接对象;
  (2)如果连接对象获得了,则表示连接已经建立成功;
  
  
3.创建Statement;

  Statement stat = conn.createStatement();
     注意:
  (1)createStatement()方法会返回一个符合Statement接口要求的对象,该对象可以理解为一个执行sql的容器;
 
  
4.执行sql;

  ResultSet rst = stat.executeQuery(String sql);  //执行查询;
  
  int x = stat.executeUpdate(String sql);  //执行插入,修改,删除;
      注意:
  (1)executeUpdate方法的返回值是一个整数,表示该sql执行成功之后,收到影响的记录的个数;
  (2)executeQuery方法返回的是一个结果集,需要遍历;
   
   
5.如果是查询,需要遍历;

   ResultSet有一个next()方法,每执行一次该方法,会返回一个true/false,如果值为true,表示还有记录可以被取出;
   
   while(rst.next()){
   rst.get***方法(如:getString("name"));  //这里的get***,jdbc驱动会自动做类型的转换;根据数据库字段的类型来选择这里的方法;
   }                                      //参数为列的别名(在select语句中声明的别名)/序号(从1开始);


6.关闭资源; //内存被释放;
  rst.close();
  stat.close();
  conn.close();
     注意:其实只要关闭connection接口的实现类对象,和他相关的Statement和ResultSet实现类对象同样也会被断开,
     但是,由于关闭所有的对象才能释放数据库连接池的所有相关空间;所以最好按照下面的语句顺序关闭;
  
  if(rst!=null){
     try{
     rst.close();
     }catch(SQLException e){  //其实这里是设计失误,因为只要rst不为空,这里不会发生异常;所以catch中不需要任何语句,只求编译通过;
     }
  }
  if(stat!=null){
     try{
     stat.close();
     }catch(SQLExcetion e){
     }
  }
  if(conn!=null){
     try{
     conn.close();
     }catch(SQLExcetion e){
     }
  }
  
     
     
 ***补充:mysql的使用***：
 
(1)登录mysql;    
   mysql -uroot; //以用户名为root登录mysql(此时默认为没有设置密码);
(2)查看当前有哪些数据库;
   show databases;
(3)创建一个新的数据库;
   create database jsd1405db default //创建了一个名叫jsd1405db的数据库;
   character set utf8;               //并且设置缺省的字符集为utf8;
(4)使用某个数据库;
   use jsd1405db;                    //设置目前使用的数据库为jsd1405db;
(5)查看当前数据库有哪些表;
   show tables;
(6)建表;
   create table t _user(
                        id int primary key auto_increment,
                        username varchar(50),
                        pwd varchar(30),
                        age int
                        )type=innodb;  //主键最好自动生成,不要包含业务含义,称为代理键,除了Oracle,其它数据库都有自增长列可以实现主键的自动生成;
      
        注意:
   type=innodb:让该表支持事务(默认情况下不支持事务,效率会提高);
   auto_increment:自增长列,由数据库自动为这个列赋值(当插入纪录时,数据库会生成一个唯一的值);
   
   insert into t_user(username,pwd,age) values('tom','test1',21);
   insert into t_user(username,pwd,age) values('tom2','test2',22);
   insert into t_user(username,pwd,age) values('tom3','test3',23);
        注意:mysql的DML语句不需要commit,都是自动提交的;
   
   (如果时Oracle数据库:
        事先建立一个sequence t_user_seq;
   insert into t_user values(t_user_seq.nextval,'tom','test',22);)
   
   
   select * from  t_user;
   
   

实例一:

1.新建一个工程JDBC01,在其中创建一个lib文件夹,将驱动复制到这个文件夹中(可以在java中执行可视化操作,右键文件夹黏贴,
或者在PC中找到工程路径,手动复制进lib文件夹),然后刷新一下这个工程,驱动变成可见;
然后在工程文件上右键Build Path中选中Configure Build Path,选中Libraries中的Add JARs选项,将配置文件都选入;

2.编写JDBC程序;
  
     在JDBC类中定义参数变量:
     
  Oracle：
  private static String driverName="oracle.jdbc.driver.OracleDriver";
  private static String url="jdbc:oracle:thin:@localhost:1521:song";
  private static String username="song";
  private static String pwd="song";
  
  mysql：
  private static String driverName="com.mysql.jdbc.Driver";
  private static String url="jdbc:mysql://localhost:3306/song";
  private static String username="root";
  private static String pwd="";
  
  
     主方法中执行:
  
  //为了在finally语句块中关闭各个接口
  Connection conn = null;
  Statement stat=null;
  ResultSet rst=null;
  
  try{
  
  Class.forName(driverName);
  
  conn = DriverManager.getConnection(url,username,pwd);
  System.out.println("conn:"+conn.getClass().getName());
  
  stat =  conn.createStatement();
  
  rst = stat.executeQuery(
                       "select id,username uname,pwd,age from t_user"); //这里最好不要用*,因为有可能数据库中可能会增加新的列,
                                                                                                                                     列的顺序有可能会变化, 那么在下面的遍历取出结果集记录时get***()中参数就会发生不匹配的情况,
                                                                                                                                     所以在这里最好将要查询的所有列都写出;
  
  while(rst.next()){
     int id = rst.getInt("id");  
     String username = rst.getString("uname");  //这里用的是别名;
     String pwd = rst.getString("pwd");
     int age = rst.getInt("age");
     System.out.println("id:"+id+"username:"+username+"pwd:"+pwd+"age:"+age);
  }
  
  }catch(ClassNotFoundException e){
   e.printStackTrace();
   System.out.println("加载驱动失败.");
   
  }catch(SQLExcetion e){
   e.printStackTrace();
   System.out.println("访问数据库失败.");
  }finally{
  
  if(rst!=null){
     try{
     rst.close();
     }catch(SQLException e){ 
     }
  }
  if(stat!=null){
     try{
     stat.close();
     }catch(SQLExcetion e){
     }
  }
  if(conn!=null){
     try{
     conn.close();
     }catch(SQLExcetion e){
     }
  }
  
  }


//例一:
         使用jdbc访问oracle数据库上的一张表(t_user表),输出该表中所有的记录;
  
Oracle端：

create sequence seq_user;

create table t_user(id sequence,username varchar2(20),age number, pwd varchar2(30));

insert into t_user values(seq_user.nextval,'tom1',21,'123451');
insert into t_user values(seq_user.nextval,'tom2',22,'123452');
insert into t_user values(seq_user.nextval,'tom3',23,'123453');
insert into t_user values(seq_user.nextval,'tom4',24,'123454');
insert into t_user values(seq_user.nextval,'tom5',25,'123455');

commit;


JAVA端:

见主方法;


//例二:JDBC实现带查询功能的方法;

public static void select(String username,String pwd){
...

rst = stat.executeQuery("select * from "+
                        "t_user where username = '" + username + 
                        "' and pwd='"+pwd+"'");
                        
...
}

主方法中执行: select("tom1","123451");  //正确返回一个用户的记录; 

转换为SQL语句:select * from t_user where username = 'a' and pwd = '123' ; 


//例三:当用户名密码不知道的情况下查询数据库中所有记录; (SQL注入);

主方法中执行: select("xxx","1' or '1'='1");  //t_user表中的所有记录都被取出;

转换为SQL语句:select * from t_user where username = 'xxx' and pwd = '1' or '1'='1';

注意:所谓SQL注入,指的是一些用户可以通过输入一些刻意构造的参数来改变SQL语句的结构,从而达到破坏
系统验证等目的;上例就是非法登录;



PreparedStatement; //预编译的Statement接口;

为了防护注入,SUN对Statement做了改进,提供了一个Statement的自接口:PreparedStatement(预编译的Statement);

如:
public static void select2(String name,String pwd){
   Connection conn = null;
   PreparedStatement stat - null;
   ResultSet rst = null;
   
   try{
            Class.forName(dbdriver);
	
			conn = DriverManager.getConnection(url,uid,pwd);
			
			String sql = "select * from t_user "+
                "where username=? and pwd=?";
                
            stat = conn.prepareStatement(sql);
                                       
            stat.setString(1,name);    //PreparedStatement中占位符处的参数需要赋值,需要用相应的setXXX()方法;
            stat.setString(2,pwd);     //第一个参数1表示为第一个问号(占位符)处赋值;
			
			rst = stat.executeQuery(); //由于之前已经将SQL语句预编译,并且在占位符处赋值,这里就不需要参数了;
            
        while(rst.next()){
				int id = rst.getInt("id");
				String username = rst.getString("username");
				int age = rst.getInt("age");
				String pwd = rst.getString("pwd");
				System.out.print("id:"+id+" username:"+username+" age:"+age+" pwd:"+pwd+"/n");
	    }
	}catch(ClassNotFoundException e){
			e.printStackTrace();
			System.out.println("驱动类未找到！");
	}catch (SQLException e) {
			e.printStackTrace();
			System.out.println("访问数据库出错！");
	}finally{
		try{
			   if(rst!=null){
				   rst.close();
			   }
			   if(stat!=null){
				   stat.close();
				}
			   if(conn!=null){
				   conn.close();
			   }
		}catch(Exception e){}
		}
}

在主方法中执行: test2("abcd","1' or '1'='1");  //没有任何记录,已经防止了注入;

注意:PreparedStatement接口之所以可以防止注入是因为,
在PreparedStatement stat = conn.prepareStatement(sql);中的参数sql会将
"select * from t_user "+"where username=? and pwd=?"语句事先传给数据库并生成了一个执行计划(预编译)
,然后stat.setString(1,name); stat.setString(2,pwd);语句将问号处的参数传给数据库,
数据库将参数放入SQL处后等待ResultSet rst = stat.executeQuery();语句执行后就可以真正执行SQL语句了;

预编译的Statement有两大优点:
1.数据库生成执行计划后,SQL语句的结构不会因为之后所给的参数赋值而改变,从而防止了SQL注入;
2.执行多条结构相同的SQL,效率会高很多,因为每次只需重写传递新的参数给服务器,而不用重复编译SQL语句(硬分析的次数降低);


例:向数据库insert数据的方法;
public static void test3(String name,String pwd,int age){
   Connection conn = null;
   PreparedStatement stat = null;
   
try{

   Class.forName(dbdriver);
   conn = DriverManager.getConnection(url,uid,pwd);
   
   String sql = "insert into t_user(id,username,pwd,age) values(seq.nextval,?,?,?)";
   stat = conn.prepareStatement(sql);
   
   stat.setString(1,name);   
   stat.setString(2,pwd);
   stat.setInt(2,age);
   
   int i = stat.executeUpdate();
   System.out.println(i+" sentences have been changed.");

}catch(ClassNotFoundException e){
			e.printStackTrace();
			System.out.println("驱动类未找到！");
}catch (SQLException e) {
			e.printStackTrace();
			System.out.println("访问数据库出错！");
}finally{
		try{
			   if(stat!=null){
				   stat.close();
				}
			   if(conn!=null){
				   conn.close();
			   }
		}catch(Exception e){}
}



扩展知识:
Access不支持JDBC,需要ODBC-JDBC桥;JDBC直接调用ODBC的API实现;
ODBC:微软的"JDBC";不开放平台,只能在微软的OS中应用,所以未流行;

注册驱动:

用代码来抽象解释,假设oracle提供的驱动类:

//oracle提供的驱动类;
package oracle.jdbc.driver;
public class OracleDriver{
   static{
     //注册驱动;
     DriverManager.setConnection(new ConnectionToOracle());
     }
}

//oracle提供的实现类;
package oracle.jdbc.???
public class ConnectionToOracle implements Connection{
   void f1(){
   System.out.println("Methods for use.");
   }
}

//sun提供的Connection接口：
package java.sql;
public interface Connection{
   void f1();
}

//sun提供的DriverManager类:
package java.sql;
public class DriverManager{
private static Connection con = null;

public static void setConnection(Connection conn){
   con = conn;
}

public static Connection getConnection(){
   return conn;
}
}

之后,在执行Class.forName(oracle.jdbc.driver.OracleDriver);后,OracleDriver类被加载,
他的静态块中的语句被执行,也就是向DriverManager类中添加了一个新的Oracle连接(Connection)的实现类对象;
然后利用DriverManager.getConnection(url,uid,pwd);获取到刚才建立的实现类对象并连接到数据库,
最后就可以用这个方法返回的对象调用数据库厂商实现的方法了;



封装:
(1)DAO(Data access object 数据访问对象),封装了数据访问逻辑的模块;
(2)Properties; 封装类; java.util中Properties类是用来访问.properties类型的配置文件;类似与Map,它其中的key/values都是String类的;

在类所在包中建立一个名为config.properties的文件,文件内容如下:

name=tom  //key=value
#age=22   //用#代表注释,被注释掉的内容读取出null;

//在demo包中创建类;
public class PropertiesTest{
   public static void test1(){
   Properties props = new Properties();
   props.setProperty("name","tom");
   props.getProperty("name");
   String name = props.getProperty("name");
   System.out.println("name:"+name);
   }
   
   public static void test2(){
      Properties props = new Properties();
      InputStream ips = PropertiesTest.class.getClassLoader().getResourceAsStream("demo/config.properties");  
      //获得加载了PropertiesTest类的类加载器,再调用getResourceAsStream()方法(此方法参数路径需要给出src下包名和配置文件名,如果文件直接放在src下则不用包名);
       
      try{
      props.load(ips);  //通过流将配置文件内容读入;
      }catch(IOException e){
      System.out.println("配置文件读写失败");
      }
      System.out.printnln(props.getProperty("name"));  //取得tom;
   }
   
}



封装;

实体类与表对应,是为了方便实现对表中记录进行访问而设计的一个简单的java类;

package entity;
 //与相关表对应;类中的属性与表中的列一致;要求类型要匹配,且这些属性有相应的get/set方法;
 public class User{
 private int id; 
 private String username; 
 private String pwd;
 private int age;
 
 	public void setId(int id) {
		this.id = id;
	}
	public int getId() {
		return id;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getUsername() {
		return username;
	}
	public void setPwd(String pwd) {
		this.pwd = pwd;
	}
	public String getPwd() {
		return pwd;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public int getAge() {
		return age;
		
		
	}
 }


package dao;

public class UserDao{


   public List<User> findAll()throws IOException{
   
      List<User> users = new ArrayList<User>();
      Connection conn = null;
      PreparedStatement prep = null;
      ResultSet rst= null;
      
      try{
      conn = DBUtil.getConnection();
      prep = conn.prepareStatement("select * from t_user");
      rst= prep.executeQuery();
      while(rst.next()){
         int id = rst.getInt("id");
         String username = rst.getString("username");
         String pwd = rst.getString("pwd");
         int age = rst.getInt("age");
         
         User user = new User(); //将记录转换为User对象;
         user.setId(id);
         user.setUsername(username);
         user.setPwd(pwd);
         user.setAge(age);
         
         users.add(user); //将User对象添加到List集合;
      }
      }catch(SQLException e){
      e.printStackTrace();
      throw e;
      }finally{
      DBUtil.close(rst,stat,pwd);
      }
      return users;
   }
}


//在util包中建立一个名为db.properties的配置文件,其中内容为:
  #oracle
  drivername=oracle.jdbc.driver.OracleDriver
  url=......
  username=......
  pwd=......
  
  #mysql
  #drivername=......
  #url=......
  #username=......
  #pwd=......
  
package util;

public class DBUtil{
   private static Properties props = new Properties();
   static{
      InputStream ips = DBUtil.class.getClassLoader().getResourceAsStream("util/db.properties");
      try{
      props.load(ips);
      }catch(IOException e){
      System.out.println("配置文件读取失败");
      }
   }
   
   public static Connection getConneciton()throws SQLException{
   Connection conn = null;
   try{
   Class.forName(props.getProperty("drivername"));  //className由所使用的数据库决定;
   conn = DriverManager.getConnection(
   props.getProperty("url"),
   props.getProperty("username"),
   props.getProperty("pwd")
   );
   }catch(ClassNotFoundException e){ //在实际开发中不可能发生的异常;
   }catch(SQLException e){
      e.printStackTrace();
      throw e;  //如果数据库连接不上,抛出连接异常,因为此异常无法通过JAVA端解决,只能提醒调用者连接异常;
   }
   return conn; //将连接对象返回;
   }
   
   
   public void save(User user)throws SQLException{
      Connection conn = null;
      PreparedStatement prep = null;
      try{
         conn = DBUtil.getConnection();
         prep = conn.prepareStatement("insert into t_user() values(?,?,?)");  //注意,这里的SQL语句结尾都不能加";" ;
         prep.setString(1,user.getUsername());
         prep.setString(2,user.getPwd());
         prep.setInt(3,user.getAge());
         prep.executeUpdate();
      }catch(SQLException e){
      e.printStackTrace();
      throw e;
      }finally{
         DBUtil.close(null,perp,conn);
      }
   }
   
   public void addUsers(List<User> users)throws SQLException{
      Connection conn = null;
      PreparedStatement prep = null;
      try{
      conn = DBUtil.getConnection();
      prep = conn.prepareStatement("insert into t_user() values(?,?,?)");
      for(int i = 0;i<users.size();i++){
      User temp = users.get(i);
      prep.setString(1,temp.getUsername());
      prep.setString(2,temp.getPwd());
      prep.setInt(3,temp.getAge());
      prep.executeUpdate();
      }
      }catch(SQLException e){
      e.printStackTrace();
      throw e;
      }finally{
         DBUtil.close(null,perp,conn);
      }
   }
   
   public static void delete(int id)throws SQLException{
      Connection conn = null;
      PreparedStatement prep = null;
      try{
         conn.DBUtil.getConnection();
         prep = conn.prepareStatement("delet from t_user where id=?");
         prep.setInt(1,id);
         prep.executeUpdate();
      }catch(SQLException e){
      e.printStackTrace();
      throw e;
      }finally{
         DBUtil.close(null,perp,conn);
      }
   }
   
   public static void close(ResultSet rst,Statement stat,Connection conn){
      if(rst!=null){
       try{
       rst.close();
       }catch(Exception e){}  //不会发生的异常;
       }
       if(stat!=null){
       try{
       rst.close();
       }catch(Exception e){}  //不会发生的异常;
       }
       if(conn!=null){
       try{
       rst.close();
       }catch(Exception e){}  //不会发生的异常;
       }
   }          
   
   public static void main(String []args){
     ...... //测试封装类是否正常运行,这样封装的好处是,调用findAll()方法者根本不需要会使用JDBC语言,不用直接去接触ResultSet这样的类也能将数据库记录取出,并且可以跨平台;
 }
}



//单元测试工具; (JUnit); java单元测试工具,主要用于程序员对自己开发的程序进行测试;

package test;

public class A {
   public void f1(){
      System.out.println("A's f1...");
   }
    public void f2(){
      System.out.println("A's f2...");
   }

}

在类的文件上鼠标右键new-> JUnit Test Case->next-> 选中New JUnit 4 test->在窗口在下的Warning处,Clikck here to add JUnit 4 to the build path ...->ok->next->选中方法->finish;

出现一个新的类文件;

import static org.junit.Assert.*;

import org.junit.Test;

public class ATest {

	@Test
	public void testF1() {
		//fail("Not yet implemented"); //删除;
		A a = new A();
		a.f1();
	}

	@Test
	public void testF2() {
	
	}

}

在上方工具栏->Window->Show View->Outline->在视图中选中需要测试的方法右键run as ->JUnit test;
此测试相当于在main方法中测试一个方法的运行情况;方便程序员单独测试各个方法;

用以上方法测试findAll()方法的正确性;
@Test
public void testFindAll()throws SQLException{
   UserDAO dao = new UserDAO();
   List<User> users = dao.findAll();
   System.out.println(users);  //在User类中添加toString()方法,然后再此处查看是否正确;

}

@Test
public void testSave(){
   UserDAO dao= new UserDAO();
   User user = new User();
   user.setUsername("user007");
   user.setPwd("test");
   user.setAge(22);
   dao.save(user);
}

@Test
public void testDelete()throws SQLException{
   UserDAO dao = new UserDAO();
   dao.delete(5);
}



事务;
如果addUsers()方法在添加各个user对象中间出现了异常,导致插入若干个对象后发生异常,之后的程序没有继续运行,后面的用户没有插入数据库,
这样就会出现问题;所以引入了事务的概念;就是将多个SQL语句组成一个原子操作,成功则全部成功,一条失败则都执行失败; 

相关的API:
Connection.getAutoCommit(); //返回当前事务的提交方式,默认为true；
Connection.setAutoCommit(); //设置事务的提交属性(自动提交:true或手动提交:false)；

设为手动提交时,可以使用一下方法;
Connection.commit();  //提交事务;
Connection.rollback();  //回滚事务;
在之前无任何提交操作时,回滚到连接建立完成的状态;如果之前有过提交操作,则回滚到最近一次提交后的状态;也就是说commit之后就无法rollback了; 

所以只有在一个原子操作中的所有SQL语句都成功执行,才能调用提交方法,只要其中任何一条SQL出现问题就必须回滚,将之前执行的所有SQL变为无效;

事务实例:

public class TransactionDemo {
   public static void main(String [] args){
      
      Connection conn = DBUtil.getConnection();
      
      String sql = "insert into t_user(id,username,pwd) values(?,?,?)";
   
      PreparedStatement ps = conn.prepareStatement(sql);
      
      conn.setAutoCommit(false);
    
      try{
         for(int i = 10; i>15; i++){
            ps.setInt(1,i);
            ps.setInt(2,"demo"+i);
            ps.setInt(1,"ps1234"+i);
            ps.executeUpdate();  //此时这条语句不会自动提交SQL;
         }
         conn.commit();  //提交;
      }catch(Exception e){
         conn.rollback(); //只要出现异常就回滚;
      }finally{
         conn.setAutoCommit(true);  //将提交状态改为默认值;
      }
      ps.close();
      conn.close();
}



批量处理;
上面的实例中更新操作ps.executeUpdate();执行了5次,如果要执行500次呢,就必须减少读写次数来提高读写的效率;

相关API;

addBatch(String sql); //Statement类的方法,可以将多条sql语句添加到Statement对象的SQL语句列表中;

addBatch(); //PreparedStatement类的方法,可以将多条预编译的sql语句添加到PreparedStatement对象的SQL语句列表中;
注意:如果PreparedStatement对象中的SQL列表包含过多的待处理SQL语句,可能会产生OutOfMemory错误,所以需要计时处理SQL语句列表,并且每次处理后调用clearBatch()方法; 

executeBatch(); //把Statement对象或PreparedStatement对象语句列表中的所有SQL语句发送给数据库进行处理;
                //并返回一个int数组,其元素数为一批中的sql语句数,每一个元素就代表了对应的sql语句影响的数据库中记录的数量;

clearBatch();  //清空当前SQL语句列表;

需要注意的是一般批处理都是对插入SQL语句来进行处理的,update和delete语句由于可以通过一条SQL语句查询出所有需要被操作的记录,所以也就只会生成一次执行计划,不需要批处理;


如:

......
try{
for(int i =0; i<1000;i++){
    ps.setInt(1,i);
    ps.setInt(2,"demo"+i);
    ps.setInt(1,"ps1234"+i);
    ps.addBatch();   //将三个替换的值添加到一条sql语句中;
} 
    if(i%500==0){    //为了防止溢出,每500条语句提交一次并清空列表;
    int[] arr = ps.executeBatch(); 
    conn.commit();
    ps.clearBatch(); 
    }
    int[] arr = ps.executeBatch(); //如果剩余的SQL语句数不足500条则再这里执行;
    conn.commit();
    ps.clearBatch(); 
}catch(Exception e){
    conn.rollback(); //只要出现异常就回滚;
}finally{
    conn.setAutoCommit(true);  //将提交状态改为默认值;
    DBUtil.close(null,prep,conn);
}


//例:给UserDAO添加一个修改员工的方法:

public void modify(User user)throws SQLRxception(
   Connection conn = null;
   PreparedStatement prep = null;
   try{
      conn.DBUtil.getConnection();
      conn.setAutoStatement(false);
      prep = conn.prepareStatement(
        "update t_user set username =?,pwd=?,age=? where id =?"
      );
      prep.setString(1,user.getUsername());
      prep.setString(2,user.getPwd());
      prep.setString(3,user.getAge());
      prep.setInt(4,user.getId());
      conn.commit();
   }catch(SQLException e){
      e.printStackTrace();
      conn.rollback();
      throw e;
   }finally{
      conn.setAutoCommit(true);
      DBUtil.close(null,prep,conn);
   }

)
  

事务的封装;
 
实例:股票操作;
用户有一个资金账户和一个股票账户;

step1:建表;

   create table t_account(
   id int primary key auto_increment,
   accountNo varchar(16) unique,
   balance int 
   )type=innodb;
   
   create table t_stock(
   id int primary key auto_increment,
   stockCode varchar(6) unique,
   qty int
   )type = innodb;
   
   insert into t_account(accountNo,balance) values('6225881003192000',1000);
   insert into t_stock(stockCode,qty) values('600015',0);
  
 step2:实体类;
        注:引用之前所创建的util包的内容(连接数据库的封装类,配置文件);
 
   Account实体类;
   package entity;
   
   public class Account{
    private int id;
    private String accountNo;
    private int balance;
    public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getAccountNo() {
		return accountNo;
	}

	public void setAccountNo(String accountNo) {
		this.accountNo = accountNo;
	}

	public int getBalance() {
		return balance;
	}

	public void setBalance(int balance) {
		this.balance = balance;
	}
   }
   
   Stock实体类;
   package entity;
   
   public class Stock{
    private int id;
    private String stockCode;
    private int qty;
   
   }
    
    public String getStockCode() {
		return stockCode;
	}

	public void setStockCode(String stockCode) {
		this.stockCode = stockCode;
	}

	public int getQty() {
		return qty;
	}

	public void setQty(int qty) {
		this.qty = qty;
	}
   }
    
    
   package dao;
   
   public class AccountDAO{
   
    //查询账户方法;
    public Account findByAccountNo(String accountNo)throw SQLException{
     Account a =null;
     Connection conn = null;
     PreparedStatement prep = null;
     ResultSet rst = null;
     try{
     conn=DBUtil.getConnection();
     prep = conn.prepareStatement("select * from t_account where accountNo=?");
     prep.setString(1,accountNo);
     rst=prep.executeQuery();
     if(rst.next()){
      a = new Account();
      a.setAccountNo(accountNo);
      a.setBalance(rst.getInt("balance"));
      a.setId(rst.getInt("id"));
     }
     }catch(SQLException e){
     e.printStackTrace();
     throw e;
     }finally{
      DBUtil.close(rst,prep,conn);
     }
     return a;
    }
    
    //修改账户信息方法;
    public void modify(Account a)throws SQLException{
     Connection conn = null;
     PreparedStatement prep = null;
     try{
     conn=DBUtil.getConnection();
     conn.setAutoCommit(false);
     prep = conn.prepareStatement(
      "update t_account set balance=? where accountNo=?"
     );
     prep.setInt(1,a.getBalance());
     prep.executeUpdate();
     conn.commit();
     }catch(SQLException e){
      e.printStackTrace();
      conn.rollback();
      throw e;
     }finally{
      conn.setAutoCommit(true);
      DBUtil.close(null,prep,conn);
     }
    }
    }
    
    
    package dao;
    
    public class StockDAO{
       //股票账户查询;
       public Stock findByStockCode(String stockCode)throws SQLException{
          Stock s =null;
          Connection conn = null;
          PreparedStatement prep = null;
          ResultSet rst = null;
          try{
             conn = DBUtil.getConnection();
             prep = conn.prepareStatement(
             "select*from t_stock where stockCode=?");
             prep.setString(1,stockCode);
             rst = prep.executeQuery();
          if(rst.next()){
             s = new Stock();
             s.setId(rst.getInt("id"));
             s.setStockCode(stockCode);
             s.setQty(rst.getInt("qty"));
             
          }
          }catch(SQLException e){
             e.printStackTrace();
             throw e;
          }finally{
             DBUtil.close(rst,prep,conn);
          }
             return s;
     }
       
       
     public void modify(Stock s)throws SQLException{
        Connection conn - null;
        PreparedStatement prep = null;
        try{
           conn = DBUtil.getConnection();
           conn.setAutoCommit(false);
           prep = conn.prepareStatement("update t_stock set qty =? where stockCode=?");
           prep.setInt(1,s.getQty());
           prep.setString(2.s.getStockCode());
           prep.executeUpdate();
           conn.commit();
        }catch(SQLException e){
           e.printStackTrace();
           conn.rollback();
           throw e;
      }finally{
         conn.setAutoCommit(true);
         DBUtil.close(null,prep,conn);
      }
      
     }
    

  package service
  
  public StockService{
     public void buy(String accountNo,String stockCode,int amount){
        AccoubtDAO dao = new AccountDAO();
        try{
           Account a = dao.findByAccountNo(accountNo);  //再到账户信息;
           a.setBalance(a.getBalance()-amount);  //在账户中扣除购买价格;
           dao.modify(a);  //将最新的用户信息更新到数据库;
           StockDAO dao2 = new StockDAO();
           Stock s = dao2.findByStockCode(stockCode); //找到股票信息;
           s.setQty(s.getQty()+amount/10); //这里讲解的业务逻辑明显有问题,只能当成抽象模块来看;
           dao2.modify(s);  //更新数据库中股票的信息;
        }catch(SQLException e) 
           e.printStackTrace(); 
           System.out.println("系统繁忙,稍候重试");
     }
  }
  
  

注意:虽然客户账户的DAO和股票的DAO都是以事务的形式运行的,但是在将这两个事务被封装在一起时也需要成为一个原子操作,成为一个事务;
所以需要由业务类来控制数据提交或是回滚;也就是事务类应该由业务类来控制;


Threadlocal类;
是JDK1.2之后的一个类,称为线程局部变量;
可以将其视为一个HashMap,它会为每一个使用某个变量的线程维护一个该变量值的副本;
所以当一个线程调用了多个方法,要在多个方法之间共享相同的数据变量,可以考虑使用Threadlocal;

ThreadLocal的get()方法是以执行当前这条代码的线程作为key,返回对应的value;
ThreadLocal的set()方法是以执行当前这条代码的线程作为key保存一个value;

修改DBUtil类:

public class DBUtil{
   private static Properties props = new Properties();
   private static ThreadLocal<Connection> connectionHolders = new ThreadLocal<Connection>();
   ......
   
   //将原来的getConnection()方法改为private的并且重新命名为getConn()方法,其他不变,
   //新的getConnection方法:
   public synchronized static Connection getConnection()throws SQLExceotion{
      Connection conn = connectionHolders.get();   //试图通过线程认证从表中取出一个conn对象;
      if(conn = null){      //若此时表中还未保存conn对象,则通过getConn()方法获取一个;
         conn = getConn();
         connectionHolders.set(conn);  
      }
      return conn;
   }
   ......
}

//修改close方法:
public static void close()throws SQLException{
   Connection conn = connectionHolders.get();
   if(conn!=null){
     conn.close();
     connectionHolders.set(null); //连接关闭后将列表中此线程的value清空;防止线程操作继续但是拿到一个已经关闭的连接无法使用;
   }
}

相应的,在业务类中,将AccountDAO类中的所有方法和StockDAO中所有方法的commit和rollback包括close操作都去除,
因为这些操作将由业务类把AccountDAO/StockDAO操作看成一个整体写在一个try/catch/finally语句中,统一commit/rollback/close;


但是这样的语句仍旧不够好,getConnection()方法存在线程安全问题,很可能造成第二个进入线程用一个新的连接将前一个线程的conn变量覆盖,对之后
获取连接造成问题,所以将getConnection()方法定义为synchronized;

另一个问题是:在新的StockService类中存在conn.setAutoCommit(false);类似这样的属于JDBC的非通用语句,所以解决方法如下:

在Util包中建立一个新的事务管理器类:

public class TransactionManager{

   //开始事务;
   public static void begin()throws SQLException{
      try{
      Connection conn = DBUil.getConnection();
      conn.setAutoCommit(false);
      }catch(SQLException e){
       e.printStackTrace();
       System.out.print("启动事务失败");
       throw e;
      }
   }
   
   //提交事务;
   public static void commit()throws SQLException{
   try{
      Connection conn = DBUil.getConnection();
      conn.commit();
      DBUtil.close();
   }catch(SQLException e){
      e.printStackTrace();
      System.out.println("提交事务失败");
   }
   
   //回滚事务;
   public static void rollback(){
   try{
      DBUtil.getConnection();
      conn.rollback();
      DBUtil.close();
   }catch(SQLException e){
      e.printStackTrace();
      System.out.println("回滚事务失败");
   }
   
}


之后业务类中也做相应的修改;

 public void buy(String accountNo,String stockCode,int amount){
        try{
           TransactionManager.begin();
           
           AccountDAO dao = new AccountDAO();
           Account a = dao.findByAccountNo(accountNo);
           ......
           TransactionManager.commit();
        catch(SQLException e){
           e.printStackTrance();
           TransactionManager.rollback();
           System.out.println("系统繁忙,稍候重试");
        }
  }    
        
        
        
分页;    

根据两个参数:
a.每页多少条记录;
b.第几页;
来查询数据库,返回有限的记录;
  
使用分页的原因,当数据库中的记录很多,那么一次性地查询所有记录会严重影响系统的性能;
        
mysql:
   
   select * from t_user limit ?,?   //第一个问号表示从哪个记录的序号(从0开始)开始取;
                                    //第二个问号表示每页多少条记录;

例:一个分页方法;

mysql:

public class UserDAO(
   public List<User> findAll2(int rowsPerPages,int pages){
   List<User> users = new ArrayList<User>();
   Connection conn = null;
   PreparedStatement prep = null;
   ResultSet rst = null;
   try{
      conn = DBUtil.getConnection();
      prep = conn.prepareStatement("select * from t_user limit ?,?");
      prep.setInt(1,rowsPerPages*(pages-1));
      prep.setInt(2,rowsPerPages);
      rst = prep.executeQuery();
      while(rst.next()){
         int id = rst.getInt("id");
         String username = rst.getString("username");
         String pwd = rst.getString("pwd");
         int age = rst.getInt("age");
         User user = new User();
         user.setId(id);
         user.setUsername(username);
         user.setPwd(pwd);
         user.setAge(age);
         users.add(user);
      }
   }catch(SQLException e){
      e.printStackTrace();
   throw e;
   }finally{
      DBUtil.close();
   }
   return users;
   }
}


oracle:
注:rownum必须在数据先被查询出后才有序号;

select * from
   (select a.*,rownum rn
   from
      (select * from t_user)a
   where rownum<?)
where rn>?

public List<User> findAll2(int rowsPerPages,int pages){
......
prep.setInt(1,rowsPerPages*pages);
prep.setInt(2,rowsPerPages*(pages-1)+1);
prep.executeQuery();
......
}


伪分页;
仍旧是一次查询出所有的数据,放在一个集合里,但是从集合中取出数据时以分页的形式展现给用户,所以
还是需要保证查询的数据不能太多;


补充:
一.注册JDBC驱动的三种方式;

1. Class.forName("com.mysql.jdbc.Driver");
由于驱动类中有类似:

static{  
      try {  
          java.sql.DriverManager.registerDriver(new com.mysql.jdbc.Driver());  
      }catch (SQLException E) {  
          throw new RuntimeException("Can't register driver!");  
      }  
 }  

这样的静态代码块,所以在加载了驱动类后就会自动向DriverManager注册这个驱动;
DiverManager.class里有个属性drivers,它实际上是一个vector(线程安全的ArrayList驱动类列表);可在列表中加入很多驱动,当DriverManager去取连接的时候,
若果drivers里有很多驱动,它会把drivers里面的各个驱动的url和创建连接时传进来的url逐一比较,遇到对应的url,则建立连接,
所以之后就可以直接通过DriverManager.getConnection();来获取连接了;
另外,此方式由于参数为字符串,可以通过读取配置文件的方式动态建立参数,因此方便更换数据库,移植性强;

例:
Class.forName("com.mysql.jdbc.Driver"); //加载数据库驱动  
String url="jdbc:mysql://localhost:3306/databasename"; //数据库连接子协议  
Connection conn=DriverManager.getConnection(url,"username","password");  


2. new com.mysql.jdbc.Driver();

例:
new com.mysql.jdbc.Driver(); //创建driver对象,加载数据库驱动;
String url="jdbc:mysql://localhost:3306/databasename"; //数据库连接子协议;
Connection conn=DriverManager.getConnection(url,"username","password");  
 
这里不需要这样写DriverManager.registerDriver(new com.mysql.jdbc.Driver()),原因是之前所说的,com.mysql.jdbc.Driver类的静态代码快里面已经进行了注册操作; 
但是,由new com.mysql.jdbc.Driver()可以知道,这里需要创建一个类的实例;创建类的实例就需要在java文件中将该类通过import导入,否则就会报错,即采用这种方式,程序在编译的时候不能脱离驱动类包,
为程序切换到其他数据库带来麻烦;


3.System.setProperty("jdbc.drivers","com.mysql.jdbc.Driver");

例:
System.setProperty("jdbc.driver","com.mysql.jdbc.Driver"); //系统属性指定数据库驱动;
String url="jdbc:mysql://localhost:3306/databasename";//数据库连接子协议;
Connection conn=DriverManager.getConnection(url,"username","password");  
 
DriverManager的getConnection()会从jdbc.drivers参数中获取jdbc驱动,然后出则到自己的驱动列表中,并且可以同时导入多个jdbc驱动,中间用冒号"："分开,
比如System.setProperty("jdbc.drivers","XXXDriver:XXXDriver:XXXDriver");
这样就一次注册了三个数据库驱动;


二.PreparedStatement和Statement的用法区别;
作为Statement的子类,PreparedStatement继承了Statement的所有功能;三种方法execute、 executeQuery和 executeUpdate已被更改为不再需要参数;
但是,在JDBC应用中如果你已经是稍有水平开发者,应该始终以PreparedStatement代替Statement也就是说,在任何时候都不要使用Statement;
基于以下的原因:

1.代码的可读性和可维护性;
虽然用PreparedStatement来代替Statement会使代码多出几行,但这样的代码无论从可读性还是可维护性上来说,都比直接用Statement的代码高很多;

2.PreparedStatement性能更好;
PreparedStatemen语句在被DB的编译器编译后的执行代码被缓存下来,那么下次调用时只要是相同的预编译语句就不需要再编译生成执行计划,只要将参数直接传入编译过的语句
中(相当于一个函数)就会得到执行;对于整个DB中,只要预编译的语句语法和缓存中匹配,那么就可以不需要再次编译而可以直接执行(addBatch()和executeBatch()就是最好的例子);
而statement的语句中,即使是相同一操作,而由于每次操作的数据不同所以使整个语句相匹配的机会极小,几乎不太可能匹配;比如:
insert into tb_name (col1,col2) values ('11','22');
insert into tb_name (col1,col2) values ('11','23');
即使是相同操作但因为数据内容不一样,所以整个个语句本身不能匹配,没有缓存语句的意义;事实是没有数据库会对普通语句编译后的执行代码缓存;

3.最重要的一点是极大地提高了安全性;
也就是防止SQL注入;
例如:
String sql = "select * from tb_name where name= '"+varname+"' and passwd='"+varpasswd+"'";
如果我们把[' or '1' = '1]作为varpasswd传入进来,用户名随意,看看会成为什么?

select * from tb_name = '随意' and passwd = '' or '1' = '1';
因为'1'='1'肯定成立,所以可以任何通过验证;
更有甚者,把[';drop table tb_name;]作为varpasswd传入进来,则:
select * from tb_name = '随意' and passwd = '';drop table tb_name;
使用了预编译SQL的数据库是不会让你注入SQL语句成功的,但也有很多数据库就可以使这些语句得到执行;


三:
使用JDBC操作存储过程;

使用JDBC操作存储过程,可以借助于一个借口CallableStatement实现;
此时调用存储过程的sql语句为：{call procedure_name [(arg1),(arg2)]}

而CallableStatement可以通过数据库连接对象的prepareCall()方法获得;

例如：

conn.prepareCall(sql);
如果此时的存储过程有输出参数,可以通过他的registerOutParameter()方法将输出参数注册为JDBC类型;

例如：
registerOutParameter(int parameterIndex,int sqlType);

下面写一个存储过程统计学生表中的人数;

create or replace procedure getStudentCount(v_student out number) is
begin
  select count(*) into v_student from student;
end getStudentCount;

然后测试一下这个存储过程：

declare
  v_count number;
begin
  getstudentCount(v_count);
  dbms_output.put_line(v_count);
end;
输出结果为7;存储过程没有问题;

然后在看如何使用JDBC调用存储过程;

import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
 
public class ProcedureDemo {
    public static void main(String[] args) {
        Connection conn = null;
        int studentCount = 0;
        try {
            Class.forName("oracle.jdbc.driver.OracleDriver");
            conn = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:ORCL", 
                    "myhr", "myhr");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        String sql = "{call getStudentCount(?)}";
        try {
            CallableStatement proc = conn.prepareCall(sql);
            proc.registerOutParameter(1,java.sql.Types.INTEGER);
            proc.execute();
            studentCount = proc.getInt(1);
        } catch (SQLException e) {
            e.printStackTrace();
        }
        System.out.println(studentCount);
    }
}



*/




