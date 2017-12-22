BigDecimal BigInteger

package prototype;

import java.math.BigDecimal;
import java.math.BigInteger;

public class BigDecimalBigInteger {
	//java.math.BigDecimal;
	//Java浮点数据类型(float/double)在运算是会有舍入误差;如果希望得到精确的计算结果,可以使用java.math.BigDecimal类;

	
	public static void main(String [] args){
		//BigInteger i1 = new BigInteger(100);  //编译报错,BigInteger的构造方法不存在以直接数作为函数的方法;
		BigInteger i2 = new BigInteger("100");
		BigDecimal d1=new BigDecimal("3.0");
		BigDecimal d2=new BigDecimal("2.9");
		BigDecimal d3=d1.subtract(d2);
		System.out.println(d3); //0.1;
		
		System.out.println(d1.compareTo(d2)); // 1; 表示d1>d2 ; 0表示相等; -1表示d1<d2;
		
		//使用静态方法valueOf可以把普通的数值转换为大数值;
		BigDecimal a=BigDecimal.valueOf(100.1); //100.1; //使用BigDecimal的静态方法valueOf来构建对象,参数为Double类型;
		System.out.println(a);
		BigDecimal b=new BigDecimal(100.1);     //100.099999999999994315658113919198513031005859375;
		System.out.println(b);
		BigDecimal c=new BigDecimal("100.1");   //100.1;
		System.out.println(c);
		
		
		//大数值运算不可以使用运算符: "+　-　*　/ %"
		//它们之间的运算需要通过方法来实现：
		
		//add(); 大整数相加
		
		//subtract(); 相减

		//multiply(); 相乘

		//divide();    相除取整

		//remainder(); 取余

		//abs(); 绝对值

		//对于BigDecimal的divide方法,通常需要制定精度和舍入模式,否则当遇到无限小数时,除法会一直进行下去直到抛出异常;
		
		BigDecimal d4 = d1.divide(d2,8,BigDecimal.ROUND_HALF_UP); //BigDecimal.ROUND_HALF_UP为四舍五入模式;
		System.out.println(d4); //1.03448276;
		
		///////////////////////////////////////////////////////////////////////////////////////////////
		
		//java.math.BigInteger;
		//Java提供的整数类型(int/long)的储值范围有限,当需要进行很大的整数运算时可以使用java.math.BigInteger类;
		//理论上讲BigInteger的储值范围只受内存容量的限制;
		
		
		
		//使用静态方法valueOf可以把普通的数值转换为大数值;
		BigInteger sum=BigInteger.valueOf(1); //使用BigInteger的静态方法valueOf来构建对象,参数为Long类型;
		for(int i=2;i<=200;i++){     //200的阶乘,结果长达375位;
			sum=sum.multiply(BigInteger.valueOf(i));
		}
		System.out.println(sum.toString().length());
		System.out.println(sum);
		
		
	}
	
}


/*
 
 注意：
 
1.BigInteger与BigDecimal都是不可变的（immutable）的,在进行每一步运算时,都会产生一个新的对象,由于创建对象会引起开销,
因此它们不适合于大量的数学运算,应尽量使用long/float/double等基本类型做科学计算或者工程计算;
设计BigInteger与BigDecimal的目的是用来精确地表示大整数和小数,常用于商业计算中;

2.BigDecimal够造方法的参数类型有4种,其中的两个用BigInteger构造,另一个是用double构造,还有一个使用String构造;
应该避免使用double构造BigDecimal,因为：有些数字用double根本无法精确表示,传给BigDecimal构造方法时就已经不精确了;
比如，new BigDecimal(0.1)得到的值是0.1000000000000000055511151231257827021181583404541015625;
使用new BigDecimal("0.1")得到的值是0.1;因此,如果需要精确计算,用String构造BigDecimal,避免用double构造,尽管它看起来更简单！
 
3.当利用BigDecimal的参数为String类型的构造方法初始化的BigDecimal对象调用equals()方法时(调用方法的对象或是方法的参数对象只要有一个用了此构造都会出现这样的情况)
会认为"0.10"和"0.1"是不等的,结果返回false;0.1和"0.1"也是不等的,只有当两个参数数值和类型完全相同:"0.1"和"0.1"或者0.1和0.1才会返回相等;
compareTo()方法则认为任何形式的0.1与0.1相等,0.10与0.1也相等;所以在从数值上比较两个BigDecimal值时,应该使用compareTo()而不是 equals();
其实究其原因,显而易见,equals方法比较的就是两个对象的hashcode,如果一个类覆写了hashcode方法使在一定条件下的两个对象hashcode相同,那调用equal方法当然会返回true,
vice versa;而Integer等包装类和BigInteger等大数据类型很显然都实现了Comparable接口并覆写了其compareTo方法,所以都可以在任何情况下正确的比较两个对象的大小;

4.另外还有一些情形,大数据类小数运算仍不能表示精确结果;例如,1除以9会产生无限循环的小数 .111111...;会引发异常:Non-terminating decimal expansion;
出于这个原因,在进行除法运算时,BigDecimal可以让用户显式地控制舍入;而浮点类型变量遇到结果为无限循环小数,由于它能计算出的结果本身就是有限位数的,
所以不会发生异常;

5.需要注意的是Integer,Double等包装类包括int/double这样直接定义整型的变量不能调用divide/add这样的数学函数,只有BigInteger/BigDecimal
这样的大数据类对象才能使用数学函数,相应地,大数据类不能使用+/-/*这样的直接运算符号;

6.BigInteger类在调用divide方法时其作用和/,也就是整除相同,结果为整数,而BigDecimal类调用divide方法将得到为带小数的结果,这点和直接定义的
整数/浮点类型在利用/做整除时的机制相同;


*/



