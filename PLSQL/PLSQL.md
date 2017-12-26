PLSQL 

package prototype;

public class SongPLSQL {

	public static void main(String[] args) {

	}

}


/*

SQL复习;

当前台工具SQL Developer 与 oracle server 连接之后,server端启动进程server process,是一个执行SQL语句
的进程,分析语义并生成执行计划,最终算出结果集后再传给SQL Developer,server process在SQL的执行环境,
也就是session(会话)中执行sql;

sql运行环境session
alter session set 
nls_date_format    (yyyy mm dd hh24:mi:ss)
nls_language       MARCH/三月
nls_territory      $/￥


数据库的负载:

从os(终端)层面上看数据库的连接数:计算此数据库用户的进程数;

从db层面上看数据库的连接数:计算session数(一个session对应一个进程);


session上运行的最小逻辑单元:事务; (DMLs + TCL<commit/rollback>);

DML锁(表级共享锁,行级排他锁);
DDL锁(DDL排他锁);

所占回滚段不释放(公共资源);

数据库缺省事务隔离级别: read committed;  其他session看不见已修改的内容;


同一个用户的不同session:并发问题;

connect jsd1405/jsd1405
update test set c1 =1 where c2 =1;

建立一个新的session;
update test set c1 = 1 where c2 =1;  //无法操作;


同一个数据库中的不同用户的session之间:权限问题;

connect jsd1405/jsd1405
select count(*) from jsd0999.test; //无法查看;

connect jsd0999/jsd0999
grant select on test to jsd1405;

connect jsd1405/jsd1405
select count(*) from jsd0999.test; //成功查看;





PL/SQL; 
过程化SQL语言; (Procedural Language/SQL)是ORACLE在标准SQL的基础上增加了过程化处理,
把DML和SELECT语句组织在PL/SQL代码的过程性单元中,通过逻辑判断,循环等操作实现
复杂的功能或者计算的程序语言;


扩展:
变量和类型;
控制结构;
过程与函数;


PL/SQL块;
声明部分:DECLARE;
执行部分:BEGIN;
异常处理:EXCEPTION;
END;

系统包; 保存了若干的过程,函数; dbms_output(包); dbms_output.put_line()(.后的内容是一个过程,其中只有一个字符类型的参数);
包名.过程名(参数);

例:打印"hello world";

begin 
   dbms_output.put_line('hello world');
end;  
 
set serveroutput on

打印成功:hello world;


注意:server process 包括了:SQL语句执行器; PL/SQL语句执行器;
这里如果没有set serveroutput on语句就无法正常打印的原因是:
server端需要指定一个输出端来显示输出的内容,不同于之前直接在
SQL Developer终端上显示的查询内容,那些内容是将服务器中的数据
反馈给客户端查看的,而这里的输出是属于过程语言的输出内容,并非
数据库中的内容,所以需要指定一个终端显示;

select 'hello world' from dual;  同样是显示了hello world,属于
SQL语句所以直接显示内容;


PL/SQL的注释方法和JAVA完全相同;



//变量与数据类型;
 

标量类型;
- 数字型,字符型,日期型,布尔型;

1.数字类型: NUMBER;  //内部存储方式:1234->1.234X10^3;
           NUMBER的子类型:DEC(38)/FLOAT(38)/REAL(18); 
           BINARY INTEGER; (只能用于PL/SQL)
           //按照二进制来存储;如果一个变量会参与运算并且是整数类型可以定义为这种类型;

2.字符型:
VARCHAR2,VARCHAR;
STRING;  //只能用于PL/SQL;
CHAR;
LONG;

3.DATE;  //与SQL相同;

4.BOOLEAN; //只有PL/SQL才有的类型;

取值:TRUE,FALSE,NULL; //当定义了boolean类型但是未赋值则会使得其值为null;这点与JAVA不同;


复合类型;(需要用户自己定义)
TODO


变量声明;

语法:

DECLARE
v_RealName VARCHAR2(20); //未给定初始值,其值为Null;
v_BaseCost NUMBER(7,2): = 5.9; //":=" 是PL/SQL中的赋值符号;
v_Count BINARY_INTEGER: =0;

DECLARE v_RealName account.real_name%TYPE; //与SQL结合;

DECLARE v_TempVar Number(7,3) NOT NULL :=12.3; //NOT NULL限定了其必须赋初值;
        v_AnotherVar v_TempVar%TYPE :=12/3;

DECLARE v_b1 boolean := true;


例:定义日期类型的变量,值为sysdate,并且打印;

declare
  v_c1 varchar2(30);
  v_d1 date := sysdate; //调用了系统函数;函数本身是又返回值的,所以可以直接调用;

begin

  dbms_output.put_line(to_char(v_d1,'yyyy mm dd hh24:mi:ss')); 
  
  v_c1 := to_char(v_d1,'yyyy mm dd hh24:mi:ss');
  dbms_output.put_line(v_c1);  //put_line过程的参数会被程序默认用to_char来输出;
  
end;


IF语句;

IF boolean_expression1 THEN
...;
ELSIF boolean_expression2 THEN
...;
ELSE
...;
END IF;

ELSIF/ELSE语句都可以省略;


例:打印一个boolean类型变量的结果;

declare
   v_b1 boolean := true;
begin
   dbms_output.put_line(v_b1);  //报错,无法直接输出boolean类型的变量;
end;


declare
   v_b1 boolean := null;
begin
   IF v_b1 THEN
   dbms_output.put_line('true');
   ELSIF v_b1 is null THEN
   dbms_output.put_line('null');
   ELSE
   dbms_output.put_line('false');
   END IF;
end;


PL/SQL中的DML和TCL;

静态SQL(DML/TCL)可以直接在PL/SQL中使用的标准SQL;

1.DML语句;
2.事务空值语句;

静态SQL：//在编译过程块时就已经对此DML语句进行编译了也就是说如果DML语句写错会出现编译报错;
begin
   insert into host(id) values('10.0.0.11');
   commit;
end;


如何在PL/SQL中实现DDL操作;

本地动态SQL; //在编译程序块时只检查"execute immediate"语句的书写格式是否正确,而字符串中的内容不会检查;只有到执行过程块的时候才会去编译字符串中的内容;
begin
   execute immediate 'drop table test purge';
end;


注意:在PL/SQL中可以用本地动态SQL的方式(也就是字符串的方式),再利用execute immediate语句来实现;


编译与执行;
PL/SQL程序分为编译阶段和执行阶段;
SQL语句分为编译(分析)和执行;
SQL语句的编译发生在PL/SQL程序的编译阶段;
SQL语句的编译发生在PL/SQL程序的执行阶段;

declare
begin 
   execute immediate 'create table test(c1 number)';
   insert into test values(1);
commit;
end;

注意:由于第一条语句在PL/SQL编译时并未将DDL语句一起编译,而第二条语句将会被编译,所以一定会发生test不存在的报错;
而下面这种格式使得DDL/DML语句都在PL/SQL过程块执行时被编译,所以编译结束后能正常运行;


declare
begin 
   execute immediate 'create table test(c1 number)';
   execute immediate 'insert into test values(1)';
commit;
end;



//匿名块编译完成后直接执行,是在一起完成的;

   非匿名块(过程,函数,包)编译和执行是分开的;


//LOOP循环语句;

语法(1): 相当于do while;

LOOP      
 statement;
 statement;
 EXIT WHEN <condition>
END LOOP;


语法(2): 相当于while;

WHILE<boolean expression> LOOP
    statement;
    statement;
END LOOP;


语法(3): 相当于for;

FOR 循环计数器 IN 下限..上限 LOOP
   statement;
   statement;
END LOOP;



例:用循环向test表中插入1000条记录;(1到1000);

静态;
create table test(c1 number);
begin
for i in 1...1000 loop
  insert into test values(i);
  end loop;
end;


动态;
begin
  execute immediate 'create table test(c1 number);';

for i in 1...1000 loop

  dbms_output.put_line('i='||i);
  
  execute immediate 'insert into test values('||i||')';
  
  end loop;
  
  commit;
end;


注意:静态SQL比动态SQL快;所以尽量用静态SQL;



PL/SQL中的SELECT语句的实现;

(1)当且仅当返回一条记录;
-用select...into...语句;

如果查询结果是单行单列,INTO子句后用标量类型,
与select后的目标数据类型一致即可;

如果查询结果是单行多列,INTO子句后的变量个数,
顺序,每个变量的数据类型应该与select子句后的目标
数据相匹配,也可以用记录类型的变量;


例:把id为1005的客户的信息以:名字's age is 年龄的格式输出;

declare v_realname account.real_name%type;
      v_age number(3);
begin

select real_name,round((sysdate-birthdate)/365) into v_realname,v_age
from account
where id = 1005;  //如果select语句没有返回值则会报异常;

dbms_output.put_line(v_realname||'''s age is '||v_age||'.');

end;


例:同上,用复合变量类型来解决;

declare

 type t_account_record is record(   //用户自己定义的复合变量类型,其实就是声明了一个新的数据类型,这个数据类型的类型为record;
   realname account.real_name%type
   age number(3));
   
 v_account t_account_record;  //声明一个变量它的类型是用户自定义的数据类型:t_account_record;
   
begin

select real_name,round((sysdate-birthdate)/365) into v_account
from account
where id = 1005;  //如果select语句没有返回值则会报异常;

dbms_output.put_line(v_account.realname||'''s age is '||v_account.age||'.');

end;

注意:记录类型的变量不能直接输出,dbms_output.put_line(v_account),系统无法识别;
但是两个声明完全相同的记录类型变量可以用等式连接v_account:=v_account_1;


%ROWTYPE;  定义记录类型变量;
用表结构或视图结构定义变量;
当使用%rowtype定义记录变量时,record成员的名称和类型与表或视图的列名称和类型完全相同;

如:
declare
v_cost cost%ROWTYPE;

begin
v_cost.base_cost:=5.9;
v_cost.base_duration:=20;
v_cost.unit_cost:=0.4;

v_cost_1 = v_cost;
select base_cost,base_duration,unit_cost
       into v_cost_1
       from cost
       where id =2;


在insert和update语句中可以直接使用记录类型变量;

如:用pl/sql操作表cost_t1

begin 
  insert into cost_t1 values v_cost;  //添加一条记录;
  update cost_t1 set row = v_cost_1;  //直接改变一条记录;
  commit;
end;


例:提供客户ID,打印客户名称和身份证号,若该用户不存在,打印该客户不存在;

declare
   type t_account is record(
   account.real_name%type,idcard_no%type);
   
   v_account t_account;
   
   v_id number(10):=  这里是所提供的客户id;
   
   v_check number(10);
   
begin
   select count(*) into v_check 
   from account
   where id = v_id

if v_check = 0 then
dbms_output.putline('该用户不存在');
else
    select real_name,idcard_no into v_account
    from account
    where id = v_id;
dbms_output.putline(v_id||'用户的名字是: '||v_account.real_name||' 身份证号为: '||v_account.idcard_no);

end if;

end;



(2)返回0条或多条记录;
-用显示cursor实现;
有些地方将cursor成为游标/光标;

server process使用的内存空间:PGA(private global area); 计算过程(比如排序),结果集等
将放在这个空间中暂存;

cursor：Oracle使用专有SQL工作区(private SQL workarea)来执行SQL语句,存储处理信息;
Oracle所执行的每一条SQL语句都有唯一的cursor与其对应,而这个cursor空间就在PGA中;
也就是说server process每执行一条SQL语句就要从PGA中申请一块内存空间去执行语句;

open_cursors; 可以同时打开的cursors,可以同时执行的SQL语句的数量,其实就是限定了可以同时
执行SQL语句的数量;如果不释放cursors占用资源,会导致无法继续执行SQL语句;


显示cursor的属性;用来获取有关显示cursor的状态信息;

BOOLEAN %ISOPEN;  判断cursor是否为open;

BOOLEAN %NOTFOUND 如果前一个fetch语句没有返回一行记录,返回TRUE;如果没有fetch这个属性的值会是null;

BOOLEAN %FOUND 与上一条命令相反;

数值 %ROWCOUNT 到目前为止,CURSOR已提取的总行数;


显示cursor;

例:将所有用户的姓名打印出来;

select real_name from account;

PL/SQL来实现;

declare 

   cursor c_account is
     select real_name from account;
     
     v_realname varchar2(20);
     
begin
   open c_account;
//用Loop;
 
   loop 
      fetch c_account into v_realname;
      exit when c_account%notfound;
      dbms_output.put_line(v_realname);
   end loop;
   close c_account;  
end;

//用while;
 
   fetch c_account into v_realname;
   
   while c_account%found loop
      dbms_output.put_line(v_realname);
      fetch c_account into v_realname;
   end loop;

//用for;(内部已经将cursor操作整合了,所以不需要open/close cursor,fetch);
  
 for i in a_account loop   //这里i其实是记录类型的变量,相当于声明i  c_account%rowtype;
    dbms_output.put_line(i.real_name);
 end loop;
  
  
 
set serveroutput on;


步骤:
1.declare,声明cursor(游标),创建和命名一个SQL工作区;
2.open,打开游标;执行SQL语句产生结果集;
3.fetch,提取游标,在结果集中提取一条符合条件的记录;
fetch cursor_name into variable;
4.判断结果集中是否还有未提取的记录;
5.关闭cursor;  //释放了cursor中资源;


隐式cursor;

隐式cursor的属性;用来获取有关隐式cursor的状态信息; 这里的相关命令都是针对离命令行最近的那条DML语句而言的;

BOOLEAN SQL%ISOPEN  DML执行中为true,结束后为false;由于是隐式的cursor,所以执行中的状态无法获取,这条语句返回的DML语句状态一定是已经执行完毕的;

BOOLEAN SQL%FOUND  值为true表示DML操作成功;

BOOLEAN SQL%NOTFOUND  与上条命令相反;

数值 SQL%ROWCOUNT


declare
begin 
   insert into test values(1);
   dbms_output.put_line(sql%rowcount);  //这里sql%rowcount返回1;
   insert into test values(1);
   insert into test values(1);
   commit;
   delete from test where c1 = 1;
   dbms_output.put_line(sql%rowcount);  //这里sql%rowcount返回3;
   commit;


cursor的声明补充:
在cursor的声明中可以再查询语句中使用order by子句,这样就能指定处理的次序;
也可以再查询中引用变量,但必须在cursor语句之前声明这些变量;

decalre
cursor c_service(temp_id number) is
select id from service
where cost_id = temp_id;
begin
   open c_saervice(5);
......


close curosr;
cursor 一旦关闭,所有和该cursor相关的资源都会被释放,不可再从关闭的cursor中提取数据(fetch);
任何对关闭的cursor的操作都会引发invalid_cursor错误;
每个session能打开的cursor的数量,其实就是每个server process可以同时处理的sql的数量,是由open_cursor决定的;


例:打印每个客户的名字和身份证号码;

declare

cursor c_account(temp_v number) is
select real_name ,idcard_no from account
where 1=temp_v;

begin

open c_account(1);  
for i in c_account loop
  dbms_output.put_line(i.real_name||'''s idcard no is '||i.idcard_no);
end loop;
close c_account;

end;


collection; 集合;

Collection是按某种顺序排列的一组元素,所哟逇元素有相同的数据类型,每个元素
有唯一一个下标标识其在这组元素中的位置;

分类:
-associative array(关联数组,索引表)又称为index-by table,使用键值访问;
-nested table嵌套表;(暂不讲解);
-varry数组,变长数组;(暂不讲解);


associative array; 关联数组;

declare 
   type t_indtab is table of varchar2(10) index by binary_integer;

   v_indtab t_indtab;

begin
   v_indtab(5):='5';
   v_indtab(1):='1';
   v_indtab(2):='1';
   
    dbms_output.put_line( v_indtab(1));
    dbms_output.put_line( v_indtab(3));  //出现异常:no data found,此元素不存在;
end;


关联数组提供的方法;

关联数组名.first //元素的第一个下标;
关联数组名.last  //元素的最后一个下标;

关联数组名.exists(i); 判断第i个元素是否存在;

prior(n)/next(n); 返回第n个元素的前/后一个元素的下标,如果不存在返回Null；

count;  返回联合数组的元素个数,不包括被删除的元素,对于空的联合数组,返回0;

declare 
   type t_indtab is table of varchar2(10) index by binary_integer;

   v_indtab t_indtab;
   v_index binary_integer;
   
begin
   v_indtab(5000):='5000';
   v_indtab(1):='10';
   v_indtab(2):='20';
   v_index :=v_indtab.first;
   
   for i in v_indtab.first..v_indtab.last loop
   if v_indtab.exists(i) then
   dbms_output.put_line( v_indtab(i));
   end if;
   end loop;
   //用for来遍历,由于第三个元素的下标与前一个元素差的过大,如果用这样的遍历方式会浪费资源,所以采取以下方式;
   
   while v_index<= v_indtab.last loop
   
    dbms_output.put_line( v_indtab(v_index));
    v_index:=v_indtab.next(v_index);  //当v_index为5000时,此处v_index为null;而由于null和任何数比较结果都是false,所以之后将跳出循环;
    
  end loop;

end;


关联数组其实就是键值对的集合,其中键是唯一的,用于确定数组中对应的值,键可以使整数或者字符串,第一次
使用键来指派一个对应的值就是添加元素,而后续这样的操作就是更新元素;
关联数组能帮我们存放任意大小的数据集合,快速查找数组中的元素,它像一个简单的存放在内存中的临时SQL表,可以按主键来检索
数据;


例:将real_name这一列的数据存放在一个集合中,然后利用遍历集合来打印结果;

declare 

cursor c_account is 
select id,real_name from account;

type t_account is table of varchar2(20)
index by binary_integer;

v_index binary_integer;

v_account t_account;

begin 
   for i in c_account loop
   v_account(i.id) :=i.real_name;
   end loop;

v_index = v_account.first;

//method 1:
while v_index<=v_account.last loop
dbms_output.put_line( v_account(v_index));
v_index=v_account.next(v_index);
end loop;

//method 2:
loop

dbms_output.put_line( v_account(v_index));
exit when v_account.next(v_index) is null;
v_index=v_account.next(v_index);

end loop;

end;


例:打印每个客户的名字,年龄,以及累计年龄;

declare
   cursor c_account is
      select id,real_name,round((sysdate-birthdate)/365) age
      from account;
      
   type t_rec is record(
   realname account.real_name%type,age number(3));
   
   type t_account is table of t_rec
   index by binary_integer;
   
   v_account t_account;
   
   v_numage number := 0;
   
   v_index binary_integer;
   
begin

   for i in c_account loop
    v_account(i.id).realname:=i.real_name;
    v_account(i.id).age:=i.age;
    end loop;

v_index:=v_account.first;

while v_index<=v_account.last loop

v_numage = v_account(v_index).age+v_numage;

dbms_output.put_line(v_account(v_index).realname||'''s '||v_account(v_index).age||' years old, and the lump age number is '||v_numage); 
 
v_index = v_account.next(v_index);

end loop;

end;



bulk collect; 批量绑定; 
在select语句中将多条记录直接用into放入一个集合类型的变量,下标按照+1递增;
在使用fetch语句时也可以用bulk collect关键字将cursor中内容一并取出利用into放入一个集合类型的变量;
还可以在使用了bulk collect的fetch语句最后用limit关键字来限制依次fetch所取的最大记录数;


隐式;
declare 
   type t_account is table of varchar2(2)
   index by binary_integer;
   v_account t_account;
begin
   select real_name bulk collect into v_account
   from account;
   for i in v_account.first..v_account.alst
   loop
      dbms_output.put_line(v_account(i));
   end loop;
end;


显式;
declare 
   type t_account is table of varchar2(2)
   index by binary_integer;
   v_account t_account;
   cursor c_account is
   select real_name from account;
   
begin
   open c_account;
   fetch c_account bulk collect into v_account;
   close c_account;
   
   for i in v_account.first..v_account.alst
   loop
      dbms_output.put_line(v_account(i));
   end loop;
end;


利用limit,分部提取数据; 
//由于每一次利用bulk collect关键字从cursor中取出数据放入集合中都是从集合的第一个元素开始放,所以这里必须分部打印;

declare 
   type t_account is table of varchar2(2)
   index by binary_integer;
   v_account t_account;
   cursor c_account is
   select real_name from account;
   
begin
   open c_account;
   fetch c_account bulk collect into v_account limit 3;  
   while v_account.count <> 0 loop              //这里不能使用 while c_account%found loop语句,原因是当最后一次fetch bulk collect limit 3,
                                                                                                                                相当于fetch了3次,只有第一次有返回值,那就是说最后一次fetch是没有返回值的,所以最后一次不会再进入循环,就会少打一个值 ;
      for i in v_account.first..v_account.alst
      loop
         dbms_output.put_line(v_account(i));
      end loop;
      fetch c_account bulk collect into v_account limit 3;
   end loop;
   close c_account;
end;



梳理:
previous in PL/SQL;
declare
   cursor is select..from..where..
   type(is record,is table of .. index by..)
   variable type
begin
   execute immediate 'DDL';
   insert into
   execute immediate 'DML';
   TCL(commit/rollback)
   select..into...(结果集:单行/多行);
        多行:cursor/select bulk collect into 集合变量(关联数组);
   cursor(loop/while/for/fetch into/fetch bulk collect into/...limit n);
   
   
server process 分为两个引擎: (sql engine; pl/sql engine) 运行他们时都需要使用内存空间(PGA),
那么在处理这两种不同类型的语句时所开辟的空间是分开的,那么在当这两种语句互相作用时,就需要这两
块空间的数据互相传递,这时使用批量处理(bulk collect)就可以增加一次内存空间传递数据的数量,从而
减少内存空间交换数据的次数,提高效率;

关联数组的处理;
方法:first,last.next,exists,count;

...

exception

end;



EXCEPTION; 异常;

PL/SQL错误;
-编译时错误;
-运行时错误;

在程序运行期间的错误对应一个异常;编译期间的异常,会直接报出,不会被捕获;

一个错误对应一个异常,当错误产生时抛出相应的异常,并被异常处理器捕获,程序控制权
传递给异常处理器,由异常处理器来处理运行时错误;


隐式触发;
-oracle预定义异常;
-非oracle预定义异常;

显示触发;
-用户自定义异常;


如：

declare
   v_realname varchar2(20);
begin
   select real_name into v_realname
   from account
   where id = 1;
   dbms_output.put_line(v_realname);
   where 1 = 1;
   dbms_output.put_line(v_realname);
   
exception
   when no_data_found then
     dbms_output.put_line('account not exists');
     
   when too_many_rows then
     dbms_output.put_line('too many records ');
end;

输出:account not exists;


常见的预定义错误:

NO_DATA_FOUND    (ORA-1403)  //如:select中where id = 1;但是此表中没有id为1的记录;
TOO_MANY_ROWS    (ORA-1422)  //select..into..语句返回多行记录;
INVALID_CURSOR   (ORA-1001)  //操作一个未open的cursor;
ZERO_DIVIDE      (ORA-1476)  //将0作为除数;
DUP_VAL_ON_INDEX (ORA-001)   //违反了唯一性约束;
VALUE_ERROR      (ORA-6502)  //如: for i in v_first..2 loop (v_first为null);


由于oracle并没有预定义类似外键约束错误,所以属于非oracle预定义异常;

declare
   e_no_parent_key exception;  //声明一个异常;
   pragma exception_init(e_no_parent_key,-2291);  //预编译异常;将异常与ORACLE的一个错误编号相关联;
begin 
   insert into child values(1,1);
   commit
exception
   when e_no_parent_key then
      dbms_output.out_line('parent key not exists');
end;

注:-2292错误是在外键约束还未消除的情况下修改父表中相关内容;


用户自定义异常;

declare
  e_bigger10 exception;
  v_n1 binary_integer:=11;
begin
  if v_n1 > 10 then
     raise e_bigger10;
  end if;
exception
   when e_bigger10 then
      dbms_output.put_line('>10');
end;


others; 关键字用来捕获未在exception中声明需要捕获的错误;

declare
v_2 number;

begin
v_2:='a'+1;

exception
   when others then
      dbms_output.put_line('error is '||sqlcode);  //sqlcode就是错误代码;
end;


输出: error is -6502;
   

sqlcode/sqlerrm函数;

sqlcode;将返回一个错误代码; 

用户自定义错误返回值:1; 
ORA-1403:NO DATA FOUND错误返回值：100;
其他ORACLE内部错误返回相应的错误号;

sqlerrm;将返回当前错误的消息文本;
如果是用户定义错误,返回:'User—defined Exception';
如果是ORACLE内部错误,返回系统内部的错误描述;

但是需要注意的是,如果在exception捕获语句块中用类似：
insert into log_table values(1,sqlcode,sqlerrm); 此处oracle处于某种原因不允许直接使用sqlerrm的文本返回值,
而是需要先将其赋给一个变量,再利用变量将内容输出; v_sqlerrm:=sqlerrm;


异常的传播:
如果不处理异常,异常将传播到外层语句块,如:pl/sql语句块中没有exception块来捕获异常,则异常会向上传播到调用环境,也就是SQL DEVELOPER中显示;



非匿名子程序;

匿名子程序;
-匿名块无法存在数据库中;
-每次使用都会重写进行编译;
-不能再其他块中相互调用;

命名的PL/SQL块,其源码和编译后的代码都会被存储在数据库中,可以在任何需要调用的地方调用;

子程序的组成部分:
-子程序头;
-声明部分;
-可执行部分;
-异常处理部分;


有名子程序的分类;

PROCEDURE; 过程; (put_line);
FUNCTION; 函数;  (单行函数/多行函数);
PACKAGE; 包;     (dbms_output);
TRIGGER; 触发器;


PROCEDURE; 过程;

create or replace procedure proc_name [参数]
is/as 声明部分;
begin
exception
end;

如:
create or replace procedure account_number
is
  v_cnt binary_integer;
begin
   select count(*) into v_cnt from account;
   dbms_output.put_line('account number is '||v_cnt);
end;



当一个procedure被创建成功,也就说明这个过程编译成功,并且源码和编译后的代码都被数据库保存了,并且此过程的状态被更新为VALID;
如果一个过程中存在编译错误,系统会报错:procedure created with compilation errors; 说明此过程虽然被创建,但只有源码会被保存,状态被更新为INVALID;
当调用一个无效的过程时会报错;

注意:如果出现procedure created with compilation errors; 这样的报错,想要知道错误原因,用show error;代码来查看错误日志;


查看过程的源码:

select text from user_source
where name = 'account_number';

结果:此过程的源码会被打印;


查看过程的状态:
select object_type,status
from user_objects
where objuct_name='account_number';

结果:
OBJECT_TYPE   STATUS
PROCEDURE     VALID


在匿名块中调用一个过程:
begin
   account_number;
end;


在sql工作表中直接调用过程:
exec account_number;


定义待参数的过程;
create or replace procedure proc1
(p_in in varchar2     //形参不能定义长度; 
 p_out out varchar2
 p_inout in out varchar2
)
is
   v_c1 varchar2(10);
begin
   --p_in:=p_in || 'd';  
   v_c1:=p_in|| 'd';
   p_out:= p_out|| 'd';
   p_inout:=p_inout|| 'd';
   dbms_output.put_line(v_c1);
end;


形参的种类;

in关键词:参数的缺省模式;
-在调用过程的时候,实际参数的值被传递给形参,在过程内部,形参是只读的,不能被重新赋值,其实参可以是变量,也可以是常量;


out关键词;
-在调用过程时,其参数必须是一个变量而不能是常量,其原因是在此参数将会在过程中被赋值,
并且最后的值将被重新传回给这个形参,也就是说这个作为参数的变量是来自过程外部的,
将由过程处理后将结果传递给外部的变量,那么用匿名块调用带有这种参数的过程就可以达到修改
匿名块中声明的变量的目的;还需要注意的是,out关键词限定的参数就算有初值也不会被传递到过程内部,
对其做修改就相当于对一个未被赋值的,仅被初始化的变量做修改;


in out关键词;
-是in与out的组合,与out关键词相同之处是,在调用过程时,其参数必须是一个变量而不能是常量,同样这个歌
作为参数的变量是来自过程外部的,将由过程处理后将结果传递给外部的变量;不过同时这个关键词又继承了
in的作用,也就是作为参数的变量的初值可以被过程接收(读取),并且在其初值的基础上做修改,最后将值重新
传递给参数中的变量,这点和out关键词完全不同;


如:
declare
   v_out varchar2(10):='abc';
   v_intout varchar2(10);:='abc';
begin
   proc1('abc',v_out,v_inout);
   dbms_output.put_line('========');
   dbms_output.put_line(v_out);
   dbms_output.put_line(v_inout);
end;

结果:
abcd
========
d
abcd


在调用一个过程时,首先要清楚其参数的类型等;

desc proc1;

显示:
Argument Name      Type          In/Out Defalut?
P_IN               VARCHAR2      IN
P_OUT              VARCHAR2      OUT
P_INOUT            VARCHAR2      IN/OUT


带参数的过程调用时参数的格式:

位置表示法; //按照形参的顺序依次输入实参;

名字表示法; //调用时给出形参名字,并给出实参;

这两个表示法是可以混合使用的;

如:
declare
   v_out varchar2(10);
   v_inout varchar2(10):='abc';
begin
   proc1('abc',
         p_out=>v_out,
         p_inout=>v_inout)
end;


给参数设置默认调用值;

create or replace procedure proc1
(p_in1 number,
 p_in2 number,
 p_in3 number default 1,
 p_in4 number default 2)
is
 v_n1 number;
begin 
 v_n1 := p_in1+p_in2+p_in3+p_in4;
 dbms_output.put_line(v_n1);
end;

exec proc1(1,2);


例:建立一个功能为创建一张表的存储过程;

create or replace procedure p_create_table
is
begin
   execute immediate'create table table_1 (c1 number)';
end;


注意:如果想要在一个存储过程中创建一个对象到数据库,必须由DBA直接授予操作者这个权限;


角色:

create user jsd1405;  //创建一个用户,无任何权限;
直接赋予权限 ：grant create table to xxx; grant create index to xxx;

角色: create role role1; 可以给一个role赋予多个权限,相当于将权限设置打包;
grant create session to role1;
grant create table to role1;
grant create view to role1;

grant role1 to jsd1405; //之间将角色权限赋给一个用户;

系统有缺省role: connect/resource;  //包含一些最基本的权限(如,create table);

grant connect,resource to jsd1405;

所以,此时,jsd1405账号并没有直接被授予create table权限;
而是被赋予了一个role(connect);

而这个通过role赋予的权限在建立过程时无效;原因是存储过程的编译和执行是分开的,先执行预编译,
而到真正执行的时候其权限不能确定是否还是除于生效状态;所以为了避免这种可能发生的前后不一致,
系统规定所有在存储过程中的role将会失效(disable),所以想要在存储过程中执行DDL操作必须直接授予相关权限;
而如果采用直接授予权限建立存储过程,有在之后回收相关权限,则这个存储过程会自动失效;

使用role的另一个好处是可以迅速让一个角色中包含的权限失效(disable),并且随时enable;
而不用频繁地回收和授予某个权限;


owner授予对象权限:grant execute on proc1 to user_1; (proc1的属主为owner)
DBA授予系统权限:grant create table to user_1;


//提供客户ID,打印客户名称和身份证号,若该客户不存在,打印不存在,用存储过程解决;

create or replace procedure p_account
(p_id in account.id%type)
is 
type t_info is record(account.real_name%type,account.idcard_no%type);
v_check number:=0;
v_info t_info;

begin

select count(*) into v_check
from account
where id = p_id;

if v_check = 0 then
dbms_output.put_line('This client is not exists.');
else
select real_name,idcard_no into v_info
from account
where id = p.id;
dbms_output.put_line('This client's name is '||v_info.real_name||' his/her idcard no is '||v_info.idcard_no);
end if;
end;



function;  函数;

如:
create or replace function fun1(p_in number,p_out out number)
return number
is 
begin
   p_out:=2;
   return p_in;
end;


调用:
declare
   v_out number ;
begin
   dbms_output.put_line(fun1(1,v_out));
   dbms_output.put_line(v_out);
end;


函数的调用环境;(允许写值的位置就可以调用函数);
-用匿名子程序调用;
-用有名子程序调用;
-在DML语句和select中调用;

删除函数;
drop function;


例:提供客户ID,返回客户名字和年龄,若该客户不存在,返回null;
(用函数实现,out型参数年龄和函数返回值为客户名称);


create or replace function f_test(
p_id in account.id%type,
p_age out account.age%type)

return account.real_name%type

is
type t_info is record(
account.real_name%type,account.age%type);

v_check number:=0;

begin

select count(*) into v_check
from account
where id = p_id;
if v_check =0 then
return null;
else
select real_name,age into v_info
from account
where id = p_id;
end if;

p_age:=v-info.age
return v_info.real_name;

end;


在匿名块中直接声明临时的function/procedure;

declare
   v_n1 number:=1;
   
   function fun1(p_in number)
   return number
   is
   begin
      return p_in;
   end;
   
   procedure proc1
   is
   begin
      dbms_output.put_line(fun1(v_n1));
   end;
   
begin
      proc1;
end;
   
 

变量;

局部变量; 在匿名块或者存储过程中定义的变量为局部变量,既作用域在整个匿名块
或存储过程中,上述程序运行结束该变量也消亡了;


绑定变量; 直接在SQL中定义;

variable var1 number;
exec :var1 :=1;

print var1;

declare
begin
  :var1 :=2;
end;

print var1;

注意:在SQL中直接定义的变量(绑定变量/宿主变量)在重新连接一个账号,也就是重新建立一个session,也就是
在service端重新建立了一个server process,这时在之前session中定义的绑定变量仍然存在,说明这个变量
是被存在了SQL PLUS端的公用内存中,只有通过exit退出sql plus,这时server process(oracleCID)进程和
sql plus进程退出,释放了sql plus占用的公用内存,才会清除这个变量;


比较:


静态SQL;
create or replace procedure proc1
is
begin
   for I in 1..1000 loop
      insert into test values(I);  //在预编译时,这里的参数中的变量就已经被编译为(:i)这样绑定变量的形式了;
   end loop;
   commit;
end;
编译存储过程时编译sql语句;

begin 
proc1
end;
1次软分析,1次硬分析,1000次绑定变量,1000次执行;

动态SQL;
create or replace procedure proc2
is 
begin
   for i in 1..1000 loop
      execute immediate'insert into test values('||i||')'; //生拼的写法;
   end loop;
   commit
end;
编译存储过程时不编译sql语句;

begin
proc2
end;
1000次软分析,1000次硬分析,1000次执行;

动态SQL;(绑定变量);

variable i number;

create or replace procedure proc3
is 
begin
   for i in 1..1000 loop
      execute immediate'insert into test values(:i)' //这里的:i对应的是variable i number;声明的绑定变量i;
      using i;
   end loop;
   commit
end;
编译存储过程时不编译sql语句;

begin
proc3
end;
1000次软分析,1次硬分析,1000次绑定变量,1000次执行;


注意:
1.对于静态SQL,server process会将执行完的SQL cache起来,也就是说cursor并没有关闭,游标没有改变,当继续执行此语句时就不需要软分析,绑定变量赋值后,直接执行;
2.静态SQL优于动态SQL语句,如果必须写动态SQL语句,则用其中带绑定变量的是最优的写法;


SQL语句的处理过程;

1.分析语句(编译):

(soft parse):
-搜索是否有相同语句;      

(hard parse):
-检查语法(syntax check); 
  分析此sql语句的拼写是否符合语法;
  
-语义检查(semantic check);
 检查语句中访问对象是否存在/该用户是否具有相应的权限;
 
-在分析过程中给对象加锁; 

-对sql语句进行解析(parse);
 利用内部算法对sql进行解析,生成解析树,执行计划;

-生成执行计划;          

2.绑定变量:给变量赋值;

3.执行语句;

4.获取数据:将数据返回给用户进程;


SGA:system global area; 共享内存段; (数据库端的一块较大的区域包括了内存+ORACLE后台进程); 
oracle为了避免重复从数据库硬盘中读取相同记录这样的操作,这个内存空间有很大一部分是为了缓存
信息的(buffer data),第一次读取一项数据之后需要读取此相同数据的操作将会直接从此块内存中
读取,从而减少了硬盘的读取次数;

shared pool; 共享池;(属于SGA;并且包含了library cache);
用来cache sql语句和pl/sql程序; 将硬分析(语法语义分析和生成执行计划)后的信息存储在内存中;

硬分析(hard parse); 消耗大量资源(cpu 内存);

软分析(soft parse); 寻找目标语句是否已经被分析过(已经被分析过的语句拥有独立的hash value,需要注意的是,语句中的大小写和空格会导致hash value的不同);
Oracle利用内部的hash算法来取得该sql的hash值,然后在library cache中查找是否存在该hash值,如果存在相同hash值则再比较其内容,如果相同则利用已有的解析树
与执行计划,而省略了许多相关工作,这就是软解析的过程;
如果上两个环节其中有一个不成立,那么此条语句将进行创建解析树,生成执行计划,这个过程就叫硬分析;
因此,从这种角度来说sql语句是大小写敏感的;(功能上不敏感,性能上敏感);

每个server process进程都存在自己的PGA;不同的用户不能查看其它用户的PGA;
但是所有的server process都可以访问SGA;PGA的cursor指向了SGA,结果集保存在PGA;



package; 包;

Package是一个可以将相关对象存储在一起的PL/SQL结构;package包含了两个分离的组成部分:
specification(package的声明)和body(声明中的程序实现,包体);每个部分都单独被存储在
数据字典中;包声明是一个操作接口,对应用来说是可见的;
包体是黑盒,对应用来说隐藏了实现细节;

package优点:

1.这种分离了声明和组成部分使得在包体还未完成的时候,后续操作(引用了包中的内容的语句)可以通过编译,待之后
其他人员完成包体之后就能直接执行语句;使工作分布,提高了效率;

2.还有一个好处是降低了包和引用包的语句之间的耦合,当包体中的内容被修改,之前已经编译好的语句不会受到影响;

3.方便对存储过程和函数的安全性管理;
-整个包的访问权限只需一次性授权,其中的语句就能够被调用;
-限制了一些语句的使用方式,只能按照包中设定的过程或函数来执行,因为并没有直接授予用户类似insert,delete等权限;

4.改善性能;在包被首次调用时作为一个整体全部调入内存;减少了磁盘的I/O次数;

注意:package的声明部分会被保存在SGA的shared pool中,但是变量将被保存在每一个调用者的session中;


specification：
create or replace package pkg1
is
   type t_rec is record
   (m1 number,
    m2 varchar2(10));
    v_rec t_rec;
    procedure proc1;
    function fun(p_in number) return number;
end;

注意:包的声明部分相当于一个JAVA接口,包含了各种声明,但是都并未实现;


body：

create or replace package body pkg1
is
   procedure proc1
   is
   begin
      dbms_output.put_line(v_rec.m1);  //同一个包内的变量可以直接引用,不需要包名.来调用;
   end;

   function fun1(p_in number) return number
   is
   begin
      return p_in;
   end;
 
 begin
    v_rec.m1:=100;
 end;
 

注意:
1.包体中的实现体的类型和名称必须和包(声明)中的完全一致;
2. begin
    v_rec.m1:=100;
   end;
    当用户首次调用包中的过程或函数时,这段代码在过程和函数执行之前被执行一次,且仅这一次,也就是说当用户第二次调用包中的函数或者过程,这段代码不会再被执行;
    这段代码被称为初始化代码,它没有被独立调用的功能;

exec pkg1.fun1(10); //函数不能这样被调用,这种形式是用来调用过程的;


调用包中函数:

declare
   v_n1 number;
begin
   v_n1:=pkg1.fun1(10);
   dbms_output.put_line(v_n1);
end;


exec dbms_output.put_line(pkg1.fun1(10));


select pkg1.fun1(10) from dual;


调用包中过程:

begin
   pkg1.v_rec.m1:=10;
end;

exec pkg1.proc1;  //10;

注意:为包中变量赋值,其生命周期为session;也就是说包中声明的变量为session全局变量;当对此包执行再次编译后,其变量恢复初值;


例:定义包,包含函数f_account和过程p_account;函数f_account(p_id number),p_id为账务账号ID,返回值为客户名称和年龄;
过程p_account,打印函数值;

create or replace package test
is
  type t_info is record(
  name account.real_name%type, age number);
  
  v_info t_info;
  
  function f_account(p_id account.id%type) return t_info;
  
  procedure p_account(p_id account.id%type);

end;
  
  
  
 create or replace package body test
 is
 
  function f_account(p_id number) return t_info
  is
  begin
     select real_name,round((systemdate-birthdate)/360) into v_info
     from account
     where id = p_id;
     return v_info;
  end;
  
  procedure p_account(p_id in account.id%type)
  is
  v_temp_name account.real_name%type;
  v_temp_age number;
  v_temp_check number;
  begin
     
     select count(*) into v_temp_check
     from account
     where id=p_id;
     
     if v_temp_check = 0 then
     dbms_output.put_lint('no such client.');
     else
     v_temp_name:=f_account(p_id).name;
     v_temp_age:=f_account(p_id).age;
     dbms_output.put_line('the client's name is  '||v_temp_name||' ane his/her is '||v_temp_age 'years old.');
     end if;
     
  end;

end;


exec test.p_account(1005);


注意:以上语句逻辑上正确,但是还有需要改进的地方,比如用将异常处理加入到function中,这样避免了其他过程调用此function时的报错可能,
另外,题目中表示包中需要包含p_account,并没有说明这个过程是否可以声明参数,那么假设不能声明参数,一下语句就是对之前的优化处理:


create or replace package test
is
  type t_info is record(
  name account.real_name%type, age number);
  
  v_info t_info;
  
  v_id account.id%type;
  
  function f_account(p_id account.id%type) return t_info;
  
  procedure p_account;

end;
  
  
  
 create or replace package body test
 is
 
  function f_account(p_id number) return t_info
  is
  begin
     select real_name,round((systemdate-birthdate)/360) into v_info
     from account
     where id = v_id;
     return v_info;
     exception
     when no_data found then
     v_info.name:='no such client.';
     v_age:=0;
     return v_info;
  end;
  
  procedure p_account
  is
  v_temp_name account.real_name%type;
  v_temp_age number;
  begin
     
     v_temp_name:=f_account(v_id).name;
     v_temp_age:=f_account(v_id).age;
     
     if v_temp_age=0 then
     dbms_output.put_line(v_temp_name);
     else
     dbms_output.put_line('the client's name is  '||v_temp_name||' ane his/her is '||v_temp_age 'years old.');
     end if;
     
  end;

end;


exec test.v_id:=1005;  //给全局变量赋值,session有效;
exec test.p_account();



trigger; 触发器;


DML触发器由四部分组成:

触发时间: 触发事件的时间次序;     before,after;

触发事件: DML语句是触发事件;      insert,update,delete;

触发器类型: 触发器体被执行的次数;  statement,row;

触发器体: 该触发器将要执行的动作;  完整的PL/SQL块;


create or replace trigger trigger_name

  1 before update on table_name  //语句级before;(只触发一次)
  2 before update on table_name for each row //行级before;(每一条受影响记录都会触发一次)
  3 after update on table_name for each row  //行级after;
  4 after update on table_name  //语句级after;

注意:1.假设update语句修改了table_name表中的若干条语句,那么上述4个触发器的发生顺序是:
     before statement: 在(所有)SQL语句(update)执行之前执行一次;
     before row: SQL语句影响的每一条记录被update/delete/insert之前执行一次;
     update: update语句执行;
     after row: SQL语句影响的每一条记录被update/delete/insert之后执行一次;
     after statement: 在(所有)SQL语句(update)执行之后执行一次;
     
     2.trigger中不能写TCL语句(commit,rollback),除非用"自治事物"来实现;
     3.如果触发器内调用其它函数或者过程,当他们被删除或修改后,触发器的状态被标识为无效;
     4.当之后DML语句激活一个无效触发器时,ORACLE将重新编译触发器代码,如果编译错误,这条DML语句将执行失败;
     5.trigger可以被disable/enable; 
              改变一个触发器: alter trigger trig_anme disable/enable; 
              改变与指定表相关的所有触发器: alter table tab_name enable/disable all triggers; 

:old/:new ; 在执行DML语句时oracle系统内部定义的记录类型的绑定变量; 只能在行级触发器中使用;

在行级触发器中,在列名前加上:old标示符表示该列变化前的值,加上:new标示符表示变化后的值;
若要修改:old/:new变量,只能在before的行级触发器中才能修改;

        :old                       :new 
insert: 所有字段都是null;           insert语句中要插入的值;
update: 在update之前该列的原始值;   update语句中要更新的新值;
delete: 在DELETE行之前的原始值;     所有字段都是null;


例:在做DML操作时,不需要提供主键值,系统自动生成;

crate or replace trigger gen_pk
   before insert on test
   for each row
   declare
   begin
   select s1.nextval into :new.c1 
   from dual;
end;

create table test(c1 number primary key,c2 number);

create sequence s1;

insert into test(c2) values(9);  //成功运行;在没有声明触发器之前这条语句会报错,因为这里的赋值跳过了主键列,使得主键列中的值为Null;


insert into test values(1);
insert into test values(1);
insert into test values(10,1);

结果: c1   c2
      1    1
      2    1
      3    1     (新值10被覆盖了)
      
      
      
利用before行级trigger实现级联更新;
      
注意:为了实现级联更新,也就是在修改父表中被子表外键约束引用的内容时需要同时将子表中的相应内容作修改,无论使用before还是after的trigger形式
都不会出现约束问题,原因是约束的检查是在DML语句完全执行完毕后才进行的;
      
      
PL/SQL特点总结:
1.结构化模块编程; 
2.良好的可移植性; (操作系统平台上的转换)
3.良好的可维护性; (如果直接写在JDBC中,那修改内容就会另所有用JDBC代码的执行用户需要修改)
4.提升系统的性能; (package,procedure,function预编译,静态SQL的实现;JDBC现编译)
5.不便于向异构数据库移植应用程序; (DB2全面支持ORACLE)
      
      
      
*/





