DateCalender2.0

package prototype;
import java.text.SimpleDateFormat;
import java.util.Calendar;                      //非lang包内容，所以必须先import
import java.util.Date;
public class SongDateCalender {
	public static void main(String [] args)throws Exception{
		Date d = new Date();              //得到表示一个计算机当前时间的Date对象;
		Date dd = new Date(1000);         //由于参数时毫秒数,1000就是一秒,所以输出结果是Thu Jan 01 08:00:01 CST 1970;
		System.out.println(d);            //利用Date来声明一个实例得到的对象在调用了Date类重写的toString()后是这样的输出：Thu Jun 19 22:37:45 CST 2014;(CST代表China Standard Time(中国标准时间，也就是北京时间))
		System.out.println(dd); 
		System.out.println(dd.getTime()); //这里输出为long型的整数1000;
		
//可是Date类的一些方法本身设计有问题，遭到众多批评，而这些遭受批评的功能都已移植到另外一个类里面，Calendar里面;

		
//下面罗列了几个Date类中还未被Deprecated的方法：
//1.Date();构造方法也就是上面输出的d对象的内容;
//2.Date(long date); 构造方法,分配 Date 对象并初始化此对象，以表示自从标准基准时间（称为“历元（epoch）”，即 1970 年 1 月 1 日 00:00:00 GMT）以来的指定毫秒数;GMT:就是格林威治标准时间的英文缩写(Greenwich Mean Time 格林尼治标准时间);
//可以参见上面的dd对象的输出结果;    
//3.getTime();返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数(long型);
//4.after(Date when);/before(Date when);测试此日期是否在指定日期之后/之前;
		System.out.println(d.after(dd));//返回结果为true;
//5.compareTo(Date anotherDate);如果参数 Date 等于此 Date，则返回值 0；如果此 Date 在 Date 参数之前，则返回小于 0 的值；如果此 Date 在 Date 参数之后，则返回大于 0 的值;
		System.out.println(d.compareTo(dd)); //1;
		
		
//一下是一些相对重要的Calendar方法:
		Calendar cal = Calendar.getInstance();//Calender是abstract类，不能new，但是可以利用其static方法调用,得到一个计算机当前时间的Calendar类信息;
		cal.set(2014,8,8,20,8,8);             //设置Calendar类信息,根据API 这里的参数月份是基于0的，所以8代表9月;
		System.out.println(cal);  //由于Calendar类toString实现的没有Date类那么直观，所以直接输出Calendar类的对象意义不大;
		
		Date d2 = cal.getTime(); //利用getTime();返回一个包含此Calendar信息的Date类对象,一遍输出时更加直观;
		System.out.println(d2);
		//set方法中参数最全的是：set(int year, int month, int date, int hourOfDay, int minute, int second) ;
		//也可以利用setTime();来以Date对象作为参数得到一个Calendar信息对象;
		Calendar cal2 = Calendar.getInstance();
		cal2.setTime(d2);
		
		
		/*
		 如果只设定某个字段，例如日期的值，则可以使用如下set方法：

         public void set(int field,int value)

在该方法中，参数field代表要设置的字段的类型，常见类型如下：

         Calendar.YEAR——年份

         Calendar.MONTH——月份

         Calendar.DATE——日期

         Calendar.DAY_OF_MONTH——日期，和上面的字段完全相同

         Calendar.HOUR——12小时制的小时数

         Calendar.HOUR_OF_DAY——24小时制的小时数

         Calendar.MINUTE——分钟

         Calendar.SECOND——秒

         Calendar.DAY_OF_WEEK——星期几

后续的参数value代表，设置成的值。例如：
*/
        cal.set(Calendar.DATE,10);
		Date dd2 = cal.getTime(); 
		System.out.println(dd2);

//该代码的作用是将cal对象代表的时间中日期设置为10号，其它所有的数值会被重新计算，例如星期几以及对应的相对时间数值等;
		
/*
 使用Calendar类中的get方法可以获得Calendar对象中对应的信息，get方法的声明如下：

         public int get(int field)

其中参数field代表需要获得的字段的值，字段说明和上面的set方法保持一致。需要说明的是，获得的月份为实际的月份值减1，获得的星期的值和Date类不一样。在Calendar类中，周日是1，周一是2，周二是3，依次类推。
下面是实例演示：
 */
                   Calendar c2 = Calendar.getInstance();

                   //年份

                   int year = c2.get(Calendar.YEAR);

                   //月份

                   int month = c2.get(Calendar.MONTH) + 1;

                   //日期

                   int date = c2.get(Calendar.DATE);

                   //小时

                   int hour = c2.get(Calendar.HOUR_OF_DAY);

                   //分钟

                   int minute = c2.get(Calendar.MINUTE);

                   //秒

                   int second = c2.get(Calendar.SECOND);

                   //星期几

                   int day = c2.get(Calendar.DAY_OF_WEEK);

                   System.out.println("年份：" + year);

                   System.out.println("月份：" + month);

                   System.out.println("日期：" + date);

                   System.out.println("小时：" + hour);

                   System.out.println("分钟：" + minute);

                   System.out.println("秒：" + second);

                   System.out.println("星期：" + day);
//////////////////////////////////////////////////////////////
   
   //  Calendar对象和相对时间之间的互转:

                            Calendar c8 = Calendar.getInstance();

                            //将Calendar对象转换为相对时间

                            long t1 = c8.getTimeInMillis();
                            System.out.println(t1);
                            
                            Date ddd=new Date();
                            System.out.println(ddd.getTime());//与上式输出结果相同;

                            //将相对时间转换为Calendar对象

                            Calendar c9 = Calendar.getInstance();

                            c9.setTimeInMillis(t1);
/////////////////////////////////////////////////////////////////////////////
//下面为一个范例：
/*
 计算两个日期之间相差的天数

例如计算2010年4月1号和2009年3月11号之间相差的天数，则可以使用时间和日期处理进行计算。

该程序实现的原理为：首先代表两个特定的时间点，这里使用Calendar的对象进行代表，然后将两个时间点转换为对应的相对时间，求两个时间点相对时间的差值，然后除以1天的毫秒数(24小时X60分钟X60秒X1000毫秒)即可获得对应的天数。实现该示例的完整代码如下：

计算两个日期之间相差的天数
 */

                   //设置两个日期

                   //日期：2009年3月11号

                   Calendar cc = Calendar.getInstance();

                   cc.set(2009, 3 - 1, 11);

                   //日期：2010年4月1号

                   Calendar cc2 = Calendar.getInstance();

                   cc2.set(2010, 4 - 1, 1);

                   //转换为相对时间

                   long tt = cc.getTimeInMillis();

                   long tt2 = cc2.getTimeInMillis();

                   //计算天数

                   long days = (tt2 - tt)/(24 * 60 * 60 * 1000);

                   System.out.println(days+" days");
//////////////////////////////////////////////////////////////////////////

		SimpleDateFormat a = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"); //利用SimpleDateFormat类配合Date()类来控制日期输出的格式;
		SimpleDateFormat a2 = new SimpleDateFormat("yyyyMMdd,HH:mm:ss"); //利用SimpleDateFormat类配合Date()类来控制日期输出的格式;
		SimpleDateFormat a3 = new SimpleDateFormat("yyyyMMdd"); //利用SimpleDateFormat类配合Date()类来控制日期输出的格式;
		//从以上可以看出，只要字段输入正确，其格式和分隔符相对比较自由;字段内容具体参见API;
		Date d3 = new Date();
		String s1 = a.format(d3); //以给定格式输出d3对象所表示日期的字符串对象;
		String s2 = a2.format(d3);
		String s3 = a3.format(d3);
		System.out.println(s1);
		System.out.println(s2);
		System.out.println(s3);
		
		String s4 = "2008-08-08 20:08:08";
		Date d4 = a.parse(s4);   //通过格式解析字符串对象返回一个该日期的Date对象;
		System.out.println(d4);
	}

}


