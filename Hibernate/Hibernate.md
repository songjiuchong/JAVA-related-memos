Hibernate 


1,Hibernate基本概念;

1)框架作用和优点;
Hibernate主要用于对数据库进行访问操作,实现增删改查;
原有JDBC+SQL访问数据库,有以下的缺点:
需要编写大量负载的SQL语句;
需要编写大量代码,实现实体类对象和数据表记录之间的转化;
当数据库移植时,需要修改部分SQL语句;例如分页查询,to_char(),SYSDATE等包含
数据库函数,特有的参数的SQL;

Hibernate可以结局上述问题;
自动生成SQL,简化查询语句;
可以自动将实体类和表记录映射转化;
可以支持目前主流的数据库,便于移植;

2)框架设计思想(设计原理);
Hibernate框架采用了ORM设计思想;

ORM: object relation Mapping;
被称为对象关系映射;
自动完成实体对象和关系数据表中记录的映射;

Hibernate框架是非常流行的ORM的一种实现;除此之外还有MyBatis,JPA等;

ORM工具好处:
采用了ORM工具,工具负责完成Java对象和记录之间映射,程序员在查询时,工具
可以自动返回Java对象;在插入和更新时,工具可以自动将Java对象数据同步到数据库的相应
的表中;中间的SQL操作细节不需要被参与;因此可以提升数据库访问编码的效率;

3)Hibernate体系结构;
a)POJO实体类(n个);
b)hibernate.cfg.xml(1个);
hibernate主配置文件,负责定义数据库连接参数和框架参数;
c)实体类名.hbm.xml(n个);
hibernate映射描述文件,负责定义表和实体类之间的对应信息;定义表名和类型名,字段名和
属性名之间的对应关系;
d)Hibernate的API;
--Configuration;
负责加载hibernate的xml配置文件;
--SessionFactory;
负责创建Session对象;(该Session代表的是与数据库的一次会话连接)
--Session
负责执行增删改查操作方法;
--Query
负责执行hibernate查询;
--Transaction
负责执行事务管理;Hibernate在默认情况下关闭了自动提交功能,在执行增删改操作时,
需要显式的执行commit操作;


2,Hibernate基本应用;
1)使用步骤:
--引入hibernate开发包,数据库驱动包;
--src下添加hibernate.cfg.xml主配置;
--根据数据表编写POJO;
--定义POJO和表的映射文件*.hbm.xml;
(在hibernate.cfg.xml中用<mapping>元素定义)
--采用hibernate API操作;
//按主键做条件查询;
session.load(查询类型,主键值);
session.get(查询类型,主键值);
//添加,根据hbm.xml定义的自动生成主键值;
session.save(obj); 
//更新,按照主键当条件将每一条对象属性更新到数据库;
session.update(obj); 
//删除,按主键当条件删除;
session.delete(obj); 

3.Hibernate映射类型;
在hbm.xml中指定的type属性值;
Java属性值<--映射类型-->表字段值
映射类型负责属性值和字段值之间相互转化;
type可以指定两种格式:
1)Java类型;
例如:java.lang.String;
2)Hibernate类型;
字符串:string
数值:byte,short,integer,long,float,double
布尔类型:yes_no,true_false;
yes_no:将true/false结果转成Y/N;
true_false:true/false结果转成T/F;
日期时间:date(年月日/年月日小时分钟秒),time(年月日小时分钟秒),timestamp(...毫秒);
Blob(blob 图片) Clob(clob 文档)

例子:
public class TestCost{
	public void testFindById(){
		//加载hibernate.cfg.xml;
		Configuration conf = new Configuration();
		conf.configure("hibernate.cfg.xml");
		//获取SessionFactory;
		SessionFactory sf = conf.buildSessionFactory();
		//获取Session连接;
		Session session = sf.openSession();
		//查询(要查询类型,ID主键值);
		Cost cost = (Cost)session.load(Cost.class,1); 
		System.out.println(cost.getId());
		//释放Sesssion连接;
		session.close(); 
	}
}

测试增加:
public void testAdd(){
	Cost cost = new Cost();
	cost.setName("1");
	cost.setBaseDuration(100);
	......
	//采用hibernate添加;
	Session session = HibernateUtil.getSession();
	//事务开启;
	Transaction tx = session.beginTransaction(); 
	session.save(cost);
	//提交事务;
	tx.commit(); 
	session.close();
}

//修改资费名称;
public void testUpdate(Integer id){
	Session session = HibernateUtil.getSession();
	Transaction tx = session.beginTransaction();
	//注意,这里需要先查询再更新,如果这里建立新的对象,并且仅仅对主键id和新的name属性进行赋值,
	那在执行文更新之后,其他未赋值的属性将被更新为Null; 
	Cost cost = session.load(Cost.class,id);
	cost.setName("newName");
	session.update(cost);
	tx.commit();
	session.close();
 
 }
 
//删除某条资费信息;
public void testDelete(Integer id){
	Cost cost = new Cost();
	cost.setId(id);
	Session session = HibernateUtil.getSession();
	Transaction ts = session.beginTransaction();
 	session.delete();
	ts.commit();
 	session.close();
 }

例子:
create table person(
id number(4) primary key,
name varchar(20),
salary number(7,2),
birth_date date,
marry char(1),
login_time date
)

public class Person implements Serializable{
	private Integer id;
	private String name;
	private Double salary;
	private Date birthDate;  //java.sql和java.util包中都有Date类,区别是sql其实是继承于util的,但是只包括年月日,不会为时分秒赋值;
	private Boolean marry;
	private Timestamp loginTime; //Timestamp包括了年月日时分秒和毫秒;
	getter/setter...
	
}

person.hbm.xml:
......
<id name="id" type="integer">
.....
<property name="marry" type="yes_no">
	<column name="MARRY">
</property>

......
Person p = new Person();
p.setName("zd");
p.setBirthDate(new Date(System.currentTimeMillis()));
p.setMarry(false);
p.setSalary(9000.5);
p.setLoginTime(new Timestamp(System.currentTimeMillis()));
执行保持;
Session session = HibernateUtil.getSession();
Transaction tx = session.beginTransaction();
session.save(p);
tx.commit();
session.close();


主键的生成:
Hibernate框架提供了一些内置的主键值生成方式;使用时通过hbm.xml文件<id>元素的
<generator>指定;
1)sequence;
采用指定序列生成主键值;适用Oracle数据库;
<generator class="sequence">
	<param name="sequence">序列名</param>
</generator>
2)identity;
采用数据库自动增长机制生成;
适用于MySQL,SQLServer等;
<generator class="identity">
</generator>
(数据表主键定义时添加自增长设置)
如:
id int primary key auto_increment,
......
3)native;
根据hibernate.cfg.xml中dialect方言参数自动切换生成方法;
如果是Oracle方言,采用sequence;
如果是MySQL方言,采用identity;
如:
<generator class="native">
	<param name="sequence">序列名</param>
</generator>
4)increment
hibernate先查询主键最大值,之后加1,再执行insert语句;
相当于,如:
select max(ID) from cost
(此方法可能出现并发现象,出现相同的ID值,所以不推荐)
5)assigned
hibernate忽略主键值生成;
使用该方式意味着程序员需要在程序中设置主键值,setId();
6)hillo/uuid
hillo采用高地位算法生成ID值;
uuid采用UUID算法生成ID值;(生成不重复的varchar类型的主键值)


Hibernate基本特性;

1)一级缓存机制(默认开启);
当创建一个Session对象时,Hibernate就会为该Session对象分配一个缓存区,被称为一级缓存;
一级缓存是受Session支配和管理,生命期与Session一致;
优点:
当使用一个Session查询某个对象时,会将查出的对象自动放入一级缓存;
再次查询时,会从缓存获取,不再对数据库进行SQL查询;
(用一个Session访问同一个实体对象多次,只在第一次查询数据库,后续都从缓存取出)
测试:
public void test1(Integer id){
	Session session = HibernateUtil.getSession();
	//第一次查询;
	Cost cost = (Cost)session.load(Cost,class,id);
	System.out.println(cost.getName());
	//第二次查询;
	Cost cost = (Cost)session.load(Cost,class,id);
	System.out.println(cost.getName());
	//释放session和缓存;
	 session.close();
}
由于在xml中设置了显示执行的sql语句,这里控制台中只输出了一次sql语句,第二次直接打印了内容;
但是前提是不能多次去执行:
Session session = HibernateUtil.getSession();session.close();
因为一个session对应一个缓存,所以创建新的session之前的缓存已近销毁了,需要创建新的缓存;

缺点(一般使用不需要考虑,大量使用数据时需要考虑清除缓存):
一级缓存管理的方法如下:
//将obj从缓存清除;
session.evict(obj);
//清除缓存中所有对象;
session.clear();

for(循环多次){
	//每次取出的cost不同;
	Cost cost = (Cost)session.load(Cost.class,i);
	//使用cost处理;这样会造成数据在缓存中的溢出;
	//清除该对象;
	session.evict(cost);
}

2)对象持久化机制(默认开启);
在Hibernate使用过程中,数据对象有以下3种存在状态;
1)临时状态;(对象所对应的记录在数据库中不存在,一级缓存中也不存在,只是对象本身在JVM内存中存在);
采用new方式新创建的数据对象,处于临时状态;
delete()方法使得持久状态对象变回临时状态;
2)持久状态;(对象所对应的记录在数据库中存在,一级缓存中也存在,对象本身在JVM内存中存在);
与session对象关联,例如执行save(),update(),load(),get()方法;后数据对象变成持久对象;
持久状态的对象有以下特点:
a.持久对象不能被垃圾回收器回收;
b.持久对象的数据状态可以与数据库自动同步更新;
c.持久对象一定是处于一级缓存中,受某个session对象管理;
d.持久对象在执行tx.commit()或者执行session.flush()方法时,才进行数据库同步:
flush():同步操作;
commit():先调用flush()同步,再提交;

例子:
public void test1(){
	Session session = HibernateUtil.getSession();
	Transaction tx = session.beginTransaction();
	Cost cost = session.load(Cost.class,id);
	System.out.println(cost.getName());
	cost.setName("5.9");
	tx.commit(); //触发同步+提交事务;,
	//session.flush(); //触发同步:
	session.close();
	//cost处于游离状态;
	System.out.println(cost.getDesrc());
}
public void test2(){
	Cost cost = new Cost();
	cost.setName("包月1");
	cost.setBaseCost(100);
	cost.setDsectr("100元");
	cost.setCreateTime(new Date());
	//保存:
	Session session = HibernateUtil.getSession)();
	Transaction tx = session.beginTracsaction();
	session.save(cost); 
	session.evict(cost); 
	cost.setName("包月");
	tx.commit();
	session.close();
}
3)游离状态;(对象所对应的记录在数据库中存在,一级缓存中不存在,对象本身在JVM内存中存在);
原来处于持久状态,后来脱离了session管理;例如执行了session.evict(obj),clear(),close();原来
持久对象转变成游离状态;再次使用save(),update(),load(),get()或者是lock()方法使得游离状态对象
变回持久状态;

Hibernate是一个持久层框架;持久层-->持久对象(具有持久性);

3)延迟加载机制(默认开启);
a.当使用Hibernate中一个具有延迟加载机制的API查询方法时,方法返回的对象没有数据库的数据,而是
在之后使用属性的get方法时,才会触发SQL查询数据库,加载对象数据;
b.具有延迟加载机制的方法;
session.load();
query.iterator();
关联映射中关联属性的get方法;
c.延迟加载操作注意事项;
此类异常(LazyInitializationException)的解决方案:
1)采用立即加载的API,
如用get()方法代替load()方法;
--query.list()可以替代query.iterator();
--将session关闭延迟到数据对象真正加载之后;
在JavaWeb程序中,可以采用openSessionInView模式控制session关闭;
实现方法如下:
--采用struts2框架拦截器实现;
--采用Filter过滤器实现;

如:
public void doFilter(ServletRequest arg0,ServletRequest arg1,FilterChain chain )throws IOException{
	//前部分逻辑;
	chain.doFilter(arg0,arg1);
	//后部分逻辑;//关闭session;
}

如果在struts2中dao中使用hibernate的load()方法,dao中创建了session,需要在拦截器中result返回之后来关闭session;
如:
......
public String intercept(ActionInvocation in){
	//前半部部分处理;
	in.invoke(); //执行action,result(JSP);
	//后半部分处理;
	//关闭session;
}

例子:
public class Testlazy{
	public void test1(){
		Session session = HibernateUtil.getSesion();
		Cost cost = (Cost)session.load(Cost.class,id); //并未触发sql查询语句,也就是说此时cost对象中并无数据;
		//访问数据;
		System.out.println(cost.getName()); //触发了查询语句,将cost对象的所有数据都从数据库取出;
		System.out.println(cost.getDesrc()); //第一次触发了查询语句后,之后同一个对象就不会再去查询数据库,直接从对象中取;
		session.close(); //注意:由于延迟加载机制,此处的close()方法不能再cost.getName()方法之前执行,否则会报异常:
									//LazyInitializationException:...(延迟加载实例化发生找不到session的异常);
	}
}

d.延迟加载的好处;
--可以降低系统并发几率和数量;
--可以减少对象占用缓存的时间;



采用Struts2+Hibernate重构资费管理;



延迟加载实现原理;
session.load();
session.get();
延迟加载方法返回的是一个动态代理类;
该类是当前POJO实体类的一个子类,该类采用动态代理机制,是利用
CGLIB技术生成(在jar包中可以找到这个包);
临时在内存中创建一个类,类似:
public class Cost$$EnhancerByCGLIB... extends Cost{
	//覆写getXXX方法;
	public String getName(){
		//判断是否已加载对象数据,如果未加载,发送SQL查询,返回name属性值;
		return super.getName();
	}
}

public class test{
	public void test1(){
		Session session = HibernateUtil.getSession();
		Cost cost= (Cost)session.load(Cost.class,2);
		System.out.println(cost.getClass().getName());
		System.out.println("++++++++++");
		System.out.println(cost.getName());
		session.close();
	}
}


Hibernate关联映射和操作;
账务账号Account;
用户想使用UNIX服务器,需要通过身份证首先开通一个账务账号;
业务账号Service;
一个业务账号代表一种UNIX服务;
隶属于某个账务账号;

业务关系如下:
一个账务账号可以有多个业务账号,一个业务账号隶属于某一个账务账号;
ACCOUNT.ID=SERVICE.ACCOUNT_ID;
功能需求:
1.访问某个账务账号信息(1010),以及下属的业务账号信息;
Account采用一对多关系加载相关Service对象;
a.第一步在Account里追加关联属性;
b.第二步在Account.hbm.xml中加关联属性映射描述;
        <set name="services">
        	<!-- 指定关联条件的外键字段,默认与当前Account的主键关联 -->
        	<key column="ACCOUNT_ID"></key>
        	<!-- 指定了关联的类class路径 -->
        	<one-to-many class="com.netctoss.pojo.Service"/>
        </set>
c.代码中通过关联属性get方法getServices();获取关系对象数据;

public class Test{
	public void testFind(){
		Session session = HibernateUtil.getSession();
		Account account = (Account)session.load(Account.class,1010);
		System.out.println(account.getRealName()); //此时在查询id为1010的记录并赋值给account对象时并不会将services一起查询出;
		System.out.println("下属业务账号");
		Set<Service> services = account.getServices();
		for(Service s:services){
			System.out.println(s.getId());
		}
		session.close();
	}
}

2.访问某个业务账号信息以及所属的账务账号信息;
Service对象采用多对一关系加载相关的Account信息;
a.在Service类中添加属性,用于存储相关的Account;
b.在Service.hbm.xml中,描述关系属性;
        <many-to-one name="account" 
        	column="ACCOUNT_ID" 
        	class="com.netctoss.pojo.Account">
        </many-to-one>
c.将外键字段的<property>映射和对应属性删除;
d.使用service.getAccount()方法;

注意:
在session.load(Service.class,id);
service.getAccount().getId();
时hibernate运行的SQL语句将所有的service对象的属性赋值完成,
其中最后将account对象的id属性也赋值了,所以在运行service.getAccount().getId();
时获得了正确的account_id,由于在POJO中此时没有account_id对应的属性和getter/setter方法,
hibernate通过多对一关系会将account.id看成这个account_id属性在查询service对象是将其account_id
属性值赋到account.id中;
在这之后,当运行类似service.getAccount().getRealName()这种不属于
多对一关系中配置的属性时再通过SQL语句根据account.id来查询多对一关系中对应的account对象,并把此对象
中其他的account属性值赋值到account对象中;


3.基于关系映射的一些特殊使用;
1)关联属性值的查询;
默认情况下,关联属性采用的是延迟加载机制查询相关表记录;
当调用关联属性get方法时,会再发送一条SQL语句加载数据;
如果需要减少与数据库交互次数,可以使用join fetch进行查询;
实现用一条查询语句将主对象和关联属性一起取出,从而减少与数据库的交互;

如:
public voi test{
	Session session = HibernateUtil.getSession();
	String hql = "from Service s where join fetch s.account where s.id = ?";
	Query query = session.createQuery(hql);
	query.setInteger(0,2005);
	Service service = (Service)query.uniqueResult();
}	

一对多;
public test{
	......
	String hql = "from Account a join fetch a.services where a.id=?"
	......
}

注意:上述功能也可以通过修改Hbm.xml中的映射描述实现;
在关联属性描述部分使用:
 <set name="services" lazy="false" fetch="join">
        	<!-- 指定关联条件的外键字段,默认与当前Account的主键关联 -->
        	<key column="ACCOUNT_ID"></key>
        	<!-- 指定了关联的类class路径 -->
        	<one-to-many class="com.netctoss.pojo.Service"/>
</set>

lazy="false":关闭延迟加载,立刻加载,这样的话还是会发送两次SQL语句,在查询
出account对象后马上再次用SQL语句查询关联属性;
fetch="join":指定关联属性抓取的方法,采取表链接查询;
默认为"select":采用单独发送SQL来加载关联属性;


级联添加/删除;

级联添加:当我们对主对象添加时,可以将关联属性中的数据也进行添加操作;
使用步骤:
将Account对象和Service建立关系映射;
将Service对象给Accout对象关联属性赋值;
在Account.hbm.xml文件关联属性描述部分添加cascade属性,用于开启级联操作;
其属性值如下:
delete:级联删除;
save-update:级联添加和更新;
all:级联添加,删除,更新;
none:默认,不支持级联;
在程序中,对Account对象操作,可以将关联属性的数据也执行相应操作;

a.inverse属性作用执行级联操作时,hibernate需要追加关系维护操作,即update关系字段值
如:将service对象的account_id自动更新为所属的account中的id字段,防止在利用
setServices时添加的service对象中的account_id值与所在实体类的account对象
的id不符而造成关系矛盾;为"true"时代表当前一方不去维护此操作,就不会出现update语句,默认为
"false";
推荐:在一对多的一这一方添加inverse=true,不去让hibernate执行字段的更新;

例子:
//TestCascade;
public void testAdd(){
		//指定一个account对象;
		Account account = new Account();
		account.set......
		//定义两个service对象;
		Service s1 = new Service();
		s1.setAccount(account);
		s1.set......
		
		Service s2 = new Service();
		s2.setAccount(account);
		s2.set......
		
		//执行添加;
		Session session = HibernateUtil.getSession();
		Transaction tx = session.beginTransaction();
		//将s1,s2给关联属性services赋值;
		account.getServices().add(s1);
		account.getServices().add(s2);
		//对主对象account添加,发现services中有s1,s2对象,也会执行添加;
		session.save(account);
		//session.save(s1);
		//session.save(s2);
		tx.commit();
		session.close();
}

级联删除:
删除主对象时,可以将关联属性对象数据一起删除;
使用注意事项:
删除的对象需要用查询获得,不能new一个临时对象;
在关联属性映射描述部分,添加cascade属性;
由于级联删除,清楚关联属性数据时,采用先一条一条删除一对多中的多对应的对象,
再删除一这个对象的方式,当关联属性数据标较多时,性能会较低,建议采用下面方法:
采用HQL语句批量删除;
delete from Service where account.id=?

public void testDelete(){
		Account account = (Account)session.load(Account.class,1559);
		Session session = HibernateUtil.getSession();
		Transaction tx = session.beginTransaction();
		session.delete(account);
		tx.commit();
		session.close();
}

//将与Service表关联的account与cost对象同时取出;
String hql = "from Service s join fetch s.account join fetch s.cost where s.id=?"


多对多关系映射:
数据库设计:由3张数据表表示;
2张实体信息表,1张关系表;
ADMIN_INFO(实体表) -- ADMIN_ROLE(关系表) -- ROLE(实体表);
需求:需要根据admin查找role,建立Admin-->Role一方的多对多映射;
a.在admin中添加roles集合属性,用于存储相关的Role信息;
b.在Admin.hbm.xml中添加映射描述信息;
        <set name="roles" table="ADMIN_ROLE">
        	<!-- 指定关联条件的外键字段,默认与当前Admin的主键关联 -->
        	<key column="ADMIN_ID"></key>
        	<!-- 指定了关联的类class路径 -->
        	<many-to-many class="com.netctoss.pojo.Role"
        	column="ROLE_ID">
        	</many-to-many>
        </set>
c.通过关联属性的get方法,可以获取相关的对象信息;


多对多关系表操作:
public void test(){
		Session session = HibernateUtil.getSession();
		Transaction tx = session.beginTransaction();
		//新增一个角色;
		Admin admin = (admin)session.load(Admin.class,190);
		Role role = (Role)session.load(Role.class,309);
		//将role给admin/从admin的关联属性中删除;
		admin.getRoles().add(role)/remove(role);
		//session.update(admin);
		tx.commit();
		session.close();	
}
中间的关系表不需要映射,在以上操作结束后,admin_role表中的190对应的
admin_id中增加/删除了一条role_id为309的信息;
而且这样的操作不需要设置级联;
如果设置了级联,则不仅会影响到中间表,还会影响到实体关系表,由于多对多的关系<
这样的操作会造成除了目标元素,其他关联元素也一并被删除的情况;

//添加一个新的管理员,并为其添加角色信息;
public void test(){
	Admin admin = new Admin();
	admin.set......
	//获得指定的角色信息;
	Session session = HibernateUtil.getSession();
	Transaction tx = session.beginTransaction();
	Role role1 = (Role)session.load(Role.class,309);
	Role role2 = (Role)session.load(Role.class,301);
	Role role3 = (Role)session.load(Role.class,302);
	//将角色信息给admin;
	admin.getRoles().add(role1);
	admin.getRoles().add(role2);
	admin.getRoles().add(role3);
	session.save(admin);//执行添加;
	tx.commit();
	session.close();
}
运行完毕后,不仅在admin表中插入了新的admin对象信息,在admin_role表中也将对应的
admin_id和role_id添加了进去;


2.继承关系映射;
Product(父类)
--ID
--NAME
--PRICE
--PRODUCT_PIC

BOOK(子类) extends Product
--AUTHOR
--PUBLISHING
--WORD_NUMBER
--TOTAL_PAGE

CAR(子类) extends Product
--BRAND
--TYPE
--COLOR
--DISPLACEMENT

数据库设计不同,可以有不同的映射方法;

第一种:数据库中只有一张数据表;
PRODUCT表;
ID NAME   PRICE  PIC       PRODUCT_TYPE
1   JAVA    30         1.JPG   BOOK
2   qq          50k       2.jpg     car
AUTHOR   PUBLISHING  WORD_NUMBER TOTAL_PAGE
张三                大众                       4                              3
null              null                   null                           null
BRAND TYPE COLOR DISPLACEMENT
null         null      null         null
奇瑞          轿车       银色          0.5

映射描述;
<class name="Product" table="PRODUCT">
	//父表主键配置;
	<id></id>
	//指定PRODUCT_TYPE为区分类型的标示;
	<discriminator column="PRODUCT_TYPE"/>
	//采用<property>定义name,price,pic属性的映射;
	<subclass name="Book" discriminator-value="BOOK">
		//采用<property>定义author,publishing等属性的映射;
	</subclass>
		<subclass name="Car" discriminator-value="car">
		//采用<rpoperty>定义color,brand等属性的映射;
	</subclass>
</class>

第二种:数据库为每个具体类设计一张表(本例需要3张);
Product父类-->PRODUCT父类表;
Book子类-->BOOK子类表;
Car子类-->CAR子类表;

采用<joined-subclass>描述;

public class Product implements Serializable{
	private Integer id;
	private String name;
	private Double price;
	private String pic;
	setter/getter...;
}

public class Book extends Product{
	private String author;
	private String publishing;
	private String wordNumber;
	private String totalPage;
	getter/setter...
}

Product.hbm.xml;
......
<class name="org.tarena.pojo.Product" table="PRODUCT">
	<id name="id" type="integer">
		<column name="ID"/>
		<generator class="sequence">
			<param name="sequence">PRODUCT_SEQ</param>
		</generator>
	</id>
	<property name="name" type="string">
		<column name="NAME"></column>
	</property>
.....

	<!--Book继承关系映射-->
	<joined-subclass table="BOOK" name="org.tarena.pojo.Book">
		<!--指定BOOK中那个字段与PRODUCT关联-->
		<key column="ID"></key>
		<property name="author" type="string">
			<column name="AUTHOR"/>
		</property>
		......
	</joined-subclass>
	
</class>

public class test{
	public void test(){
		Session session = HibernateUtil.getSession();
		Transaction tx = session.beginTransaction();
		Book book = new Book();
		//设置product信息;
		book.setName("123");
		book.setPrice(30.0);
		book.setPic("a.jpg");
		//设置book信息;
		book.setAuthor("张三");
		book.setPublishing("大众");
		book.setWordNumber("123")'
		book.setTotalPage("3");
		//添加;
		session.save(book); //PRODUCT表中的ID和BOOK表中的ID已经自动被关联了;
		tx.commit();
		session.close();
	}
}

第三种:数据库为每个子类定义数据表;(本例需要2个)
Book表;
ID NAME PRICE PIC AUTHOR PUBLISHING...
CAR表;
ID NAME PRICE PIC BRAND COLOR...

采用<union-subclass>元素描述;
<class>
	//父类中主键和非主键的字段映射;
	<union-subclass name="子类" table="子表">
		//子类中的字段映射;
	</union-subclass>
</class>

第二种方案显然效率最高,并且没有冗余;


1.Hibernate查询;
session.load和get方法只能按照主键做条件查询,其他查询需要使用一下方法;
1)HQL语句(推荐)
HQL全称:Hibernate Query Language;

a.HQL和SQL的相似点:
--都支持select,from,where,order by,group by,having;
--支持分组分组统计函数(count,max,min,avg,sum);
--支持>,>=,<,<=,in,not in,like,between...and...等过滤条件;
--支持+,-,*,/等运算符和表达式;
--支持inner join..,left/right outer join..;

b.HQL和SQL不同点:
--HQL是面向对象的查询语言,SQL是面向结构的;
HQL是针对于实体类型和属性元素查询;SQL是针对于表和字段;
--HQL不支持select *写法;支持select count(*)写法;
--HQL不能使用join .. on ..中的on子句;
--HQL不能使用数据库所特有的函数或者关键字,例如:SYSDATE,to_char(),to_date();
因为Hibernate会根据方言来调用相应的函数,使用特定数据库的函数会使得Hibernate无法解析;
--HQL语句是大小写敏感;类名和属性名需要严格区分大小写;

例子:
public class test{
	//按非主键做条件查询;
	//查询所有套餐类型的资费记录;
	//sql:select * from cost where cost_type=? and name like ?";
	public void test1(){
		String hql = "from Cost where costType=? and name like ? ";
		//利用查询参数而不是index来为query中参数赋值;
		//String hql = "from Cost where costType=:type and name like :name"; //冒号和查询参数之间不能有空格;
		Session session = HibernateUtil.getSeesion();
		Query query  = session.createQuery(hql);
		query.setString(0,"2");
		//query.setString("type","2");
		query.setString(1,"%套餐%");
		//query.setString("name","%套餐%");
		List<Cost> list = query.list();
		for(Cost c:list){
			System.out.println(c.getId()+c.getName());
		}
		session.close();
	}
}


//查询指定列的数据;
//select ID,NAME from COST ...
public void test(){
	String hql = "select c.id,c.name from Cost c where c.costType=? and c.name like ?"
	Session session = HibernateUtil.getSeesion();
	Query query  = session.createQuery(hql);
	query.setString(0,"2");
	query.setString(1,"%套餐%");
	//指定列查询,默认采用object[]封装一行记录,其元素存放顺序和查询语句中的查询顺序相同;
	List<Object[]> list = query.list();
	for(Object[] o:list){
		System.out.println(c[0]+c[1]);
	}
	session.close();
}

public void test(){
	//仍旧是查询指定列,但是让hibernate返回一个实体类对象来封装这些数据;(必须保证实体类中存在相应参数的构造方法);
	String hql = "select new Cost(c.id,c.name) from Cost c where c.costType=? and c.name like ?"
	Session session = HibernateUtil.getSeesion();
	Query query  = session.createQuery(hql);
	query.setString(0,"2");
	query.setString(1,"%套餐%");
	//指定列查询,默认采用object[]封装一行记录,其元素存放顺序和查询语句中的查询顺序相同;
	List<Cost> list = query.list();
	for(Cost c:list){
		System.out.println(c.getId()+c.getName()); //这里如果取其它非查询属性则返回空值;
	}
	session.close();
}


将hql语句提取到hbm.xml配置文件中;
如:
    <!-- 定义hql查询 -->
    <query name="find1">
    	<!-- 设置一个文本区,其中如果出现一些类似'>','<'等敏感字符,不会去解析 -->
    	<![CDATA[
    	select new Cost(c.id,c.name) 
    	from Cost c where c.costType=?
    	]]>
    </query>

然后查询语句块变为:

public void test(){
	Session session = HibernateUtil.getSeesion();
	Query query  = session.getNamedQuery("find1");
	query.setString(0,"2");
	List<Cost> list = query.list();
	for(Cost c:list){
		System.out.println(c.getId()+c.getName()); //这里如果取其它非查询属性则返回空值;
	}
	session.close();
}


//统计记录数量;
//统计套餐资费类型的个数;
//sql:select count(*) from COST where cost_type=? ;
public void test(){
	String hql = "select count(*) Cost where costType=?"
	Session session = HibernateUtil.getSeesion();
	Query query = session.createQuery(hql);
	query.setString(0,"2");
	//不能使用Integer,会出现ClassCastException;
	Long size = (Long)query.uniqueResult();
	System.out.println("资费个数":size);
	session.close();
}

//分页查询;
public void test(){
	String hql = "from Cost order by id";
	Session session = HibernateUtil.getSeesion();
	Query query = session.createQuery(hql);	
	//设置分页查询参数;(0-4,一共5条记录);
	query.setFirstResult(0); //设置抓取起点n-1;
	query.setMaxResults(5); //设置抓取记录的最大数量<=n;
	List<Cost> list = query.list();
	for(Cost c:list){
		System.out.println(c.getId()+c.getName());
	}
}


表连接查询,SQL:

//查询SERVICE的ID,OS_USERNAME,UNIX_HOST和ACCOUNT中的REAL_NAME,IDCARD_NO;
select s.id,s.os_username,s.unex_host a.real_name a.idcard_no 
from SERVICE s 
join ACCOUNT a 
on(s.account_id=a.id)

第一种写法:

HQL:
public void test(){
	//由于HQL语句不支持表连接中的on语句,所以这里需要用关联属性来代替引用另一个实体类;
	String hql = "select s.id,s.osUsername,s.unitHost,a.realName,a.idcardNo 
	from Service s 
	inner join s.account a"; //多对一关系;
	Session session = HibernateUtil.getSeesion();
	Query query = session.createQuery(hql);	
	List<Object[]>query.list();
	for(Object[] obj:list){
		System.out.println(obj[0]+obj[1]+obj[2]+obj[3]+obj[4]);
	}
	session.close();
}

//一对多关系;
......
from Account a inner join a.services s
  
第二种写法:
HQL:
多对一:
......
	String hql = "select 
	s.id,
	s.osUsername,
	s.unitHost,
	a.account.realName,
	a.account.idcardNo 
	from Service s "; 

一对多:
无法使用类似:
select a.services.id...
来查询;

第三种:
两个实体类不存在关联映射;
使用较为原始的,与SQL基本语法一致的(hibernate采取的表连接)方式:
......
from Service s,Accout a
where s.accountId=a.id


2)Criteria (QBC)
条件查询;
将SQL查询封装成了API的方法;
Criteria不支持统计函数;

//查询所有Cost记录;
public void test(){
	Session session = HibernateUtil.getSession();
	//指定查询的数据源,等价于HQL中的:from Cost;
	Criteria c = session.createCriteria(Cost.class);
	//指定查询条件COST_TYPE='2';
	c.add(
	Restrictions.and(
	Restrictions.eq("costType","2"),
	Restrictions.like("name","%套餐%")
	)
	);
	//指定排序规则 order by id;
	c.addOrder(Order.asc("id"));
	List<Cost> list = c.list();
	for(Cost c:list){
		System.out.println(c.getId()+c.getName());
	}
	session.close();
}

//分页;
public void test(){
	Session session = HibernateUtil.getSession();	
	Criteria c = session.createCriteria(Cost.class);
	c.setFirstResult(1); //从第二条开始取;
	c.setMaxResult(3);
	List list = c.list();
	for(Cost c:list){
		System.out.println(c.getId()+c.getName());
	}
	session.close();
}


3)Native SQL
可以将原始的SQL语句交给Hibernate执行;
//查询所有记录;
public void test(){
	String sql = "select * from COST";
	Session session = HibernateUtil.getSession();	
	//默认采用Object[]封装一行记录;
	SQLQuery query = session.createSQLQuery(sql);
	//设置分页参数;
	query.setFirstResult(2); //从第三个开始取;
	query.setMaxResults(5); //取5个数据;
	//指定封装一行记录的类型;
	query.addEntity(Cost.class);
	//采用Cost封装一行记录;
	List<Cost> list = query.list();
	for(Cost c:list){
		System.out.println(c.getId()+c.getName());
	}
	session.close();
}

//按照资费类型和名称查询;
public void test(){
	String sql = "select * from COST 
	where COST_TYPE=? and NAME like ?";
	Session session = HibernateUtil.getSession();	
	SQLQuery query = session.createSQLQuery(sql);	
	query.setString(0,"2"); 
	query.setString(1,"%套餐%"); 
	List<Object[]> list = query.list();
	for(object[] objs:list){
		System.out.println(objs[0]+objs[1]);
	}
	session.close();
}


1.Hibernate高级特性;
1)二级缓存;(默认关闭)
当创建SessionFactory对象时,Hibernate为其分配一个缓冲区,该缓冲区被称为二级缓存,
它受SessionFactory管理;
a.二级缓存特点;
可以跨Session共享访问缓存中数据;
b.二级缓存的生命期和SessionFactory保持一致,使用时需要手工管理缓存空间;
sessionFactory.evict(Cost.class); //可以移除某一类的对象或者根据其他条件来移除,具体参考API;
一般适合存放共享的数据对象;而且更新频率较低的对象;
例如系统菜单目录等数据;
c.应用方法;
--引入ehcache.jar二级缓存包;
--在src下添加ehcache.cml缓存配置文件;
--修改hibernate.cfg.xml,添加开启二级缓存和指定二级缓存驱动;
--修改POJO.hbm.xml,添加<cache/>元素,指定该类型的对象启用二级缓存存储;


二级缓存的应用;
将ehcache jar(还有其它不同的缓存实现jar包)包导入项目,将其xml文件放入src中;
 在hibernate主配置文件hibernate.cfg.xml中设置:
 <!--开启二级缓存-->
 <property name="hibernate.cache.use_sesond_level_cache">
 true
 </property>
 <!--指定驱动类-->
 <propery name="hibernate.cache.provider_class"> 
 net.sf.ehcache.hibernate.EhCacheProvider
 </property>
 ......
 
 在Cost.hbm.xml文件中加入:
 <!--该对象启用二级存储,如果不设置region参数,则默认采用ehcache.xml中的defaultCache参数设置-->
 <cache usage="read-write" region="simpleCache1"/> //usage参数可以使read-write read-only 等;
 
 public void findById(){
 	Session session = HibernateUtil.getSession();
 	Cost cost = (Cost)session.load(Cost.class,1);
 	System.out.println(cost.getName()+cost.getId());
 	session.close();
 }
 
 public void test(){
 	findById();
 	findById();
 }

//执行后只执行了一次查询,第二次查找直接从二级缓存中取得对象; 
  

2)查询缓存(默认关闭);
a.概念;
二级缓存默认(一级缓存也是这样)只能存储单个实体对象,遇到结果集,对象数组等内容时,就需要使用查询缓存;
查询缓存借用二级缓存的机制,可以对SQL查询结果存储;
b.优点
开启查询缓存后,可以将查询语句SQL和结果集缓存下来,当再次发送相同SQL查询时,
从缓存将数据取出,避免对数据库的查询;
c.适合的使用场合;
--共享的数据集合;
--集合数据尽量不发生改变;
d.使用方法;
--对查询对象开启二级缓存;(查询缓存的空间在二级缓存中分配)
--修改Hibernate.cfg.xm,开启查询缓存配置;
--修改查询代码,在执行query.XXX发送查询语句之前调用query.setCacheable(true);
(如果缓存中有记录,从缓存取;没有记录,去数据库查,将结果或结果集和SQL放入缓存区)

 在hibernate主配置文件hibernate.cfg.xml中设置:
 <!--开启查询缓存-->
<property name="hibate.cache.use_query_cache">
true
</property>

public class test{
	public void test(){
		findAll(1);
		findAll(1);
	}
	
	private void findAll(String type){
		String hql = "from Cost where costType=?";
		Session session = HibernateUtil.getSession();
		Query query = session.createQuery(hql);
		query.setString(0,type);
		//指定采用查询缓存机制查找;
		query.setCacheable(true);
		List<Cost> list = query.list();
		for(Cost c:list){
			System.out.println(c.getId()+c.getName());
		}
	}
}


2.Hibernate事务并发处理;
当用户并发请求时,会出现请求处理的并发性,对数据库操作出现交叉处理现象;
Hibernate提供了2种锁机制,即悲观锁,乐观锁;

1)乐观锁机制;
实现效果:当多个人同时对某个对象操作时,只有第一个提交的用户成功,其他用户会抛出异常;

a.使用方法:
--在数据表中添加一个数值类型的字段;
--在POJO中定义一个属性,用于映射到表中追加字段;
--在hbm.xml中采用<version>定义属性和字段的映射;

b.实现机制;
用户在更新POJO数据对象时,首先取得数据库中<version>指定的版本字段对应的数值,在提交后Hibernate会自动将<version>指定的版本字段值+1;
每次用户更新提交,Hibernate首先检测对象的版本属性值是否大于等于数据库记录中的版本字段值;
如果对象属性大于等于数据表字段,允许执行更新操作,并且将版本字段+1处理;
如果对象版本属性小于数据库<version>指定的版本,则会抛出异常,并阻止提交更新;

2)悲观锁;
实现效果:当多个人同时对某个对象操作时,会将他们排列,一个一个地处理;

a.使用方法如下:
--在查询数据时,使用下面的get/load方法;
session.load(类型,主键值,LockMode.UPGRADE);
b.实现机制;
悲观锁在查询对象记录时,将该记录锁住,会阻止其他的线程对该对象执行增删改查操作;
只有当锁住的线程事务结束才能执行;
Hibernate悲观锁机制实际上是借助于数据库锁机制完成的;
在select语句后面追加for update关键字实现的;
如:......where xxx.id=? for update (行级锁) 


在DB Browser中,先建立连接;
在可视化窗口中找到table列表,选中其中一张表,右键->Hibernate Reverse Engineering;
在Java Src Folder中选择路径,但是由于只有用Myeclipse自带的Hibernate组件生成的项目
才可以在这里被选中,所以可以临时用其自带的Hibernate jar包创建一个项目,生成了目标POJO类后
再将POJO类文件放入之前的项目;

创建一个WEB Project,右键工程名->myeclipse->add Hibernate Capability;
next->next->DB Driver中选择先前创建的DB Browser,其中的参数会自动载入;
java package填写一个路径->finish;

回到之前的步骤,Hibernate Reverse Engineering,路径选择临时工程,
选中下面两项:Hibernate mapping file和Java Data Object(POJO),
不要选中create abstract class;->next->Type Mapping:Hibernate types->next
->finish;

总结:
1.Hibernate作用和实现原理;
2.Hibernate框架基本特性和高级特性;
3.Hibernate框架查询方法;
4.设计模式:有单例(sessionFactory),工厂模式(hibernateUtil),动态代理(反射)(实现了延迟加载)等;
5.Hibernate框架引入(jar包引入,配置,HibernateUtil);
6.对单表进行增删改查操作;
7.掌握关联映射的建立方法;
(多对一,一对多,多对多,继承);
8.掌握OpenSessionInView技术(为了支持延迟加载操作);
9.关联操作(join fetch,级联操作);
10.掌握HQL语句用法;




*/


