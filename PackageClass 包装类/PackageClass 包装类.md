PackageClass 包装类


package prototype;

public class SongPackageClass {
	public static void main(String[] args) {
		int x = 10; //基本数据类型
		Integer x1 = new Integer(x);    //把基本数据类型转换为包装类的形式,称为装箱;
		int temp1 = x1.intValue();      //把包装类型转换为基本数据类型,称为拆箱;
		String x3 = "1";
		Integer x4 = new Integer(x3);   //以String的数字形式作为参数;
		int temp3 = x4.intValue(); 
		System.out.println(temp3);      //1;
		//在JDK 1.5版本之后，提供了自动装箱和拆箱的功能，类似于String类的操作;
		Integer x2 = 20;             //自动装箱;
		int temp2 = x2;              //自动拆箱;
		
		Integer in3=2;
		Integer in4=2;
		System.out.println(in3==in4);  //true
		Integer in5=211;
		Integer in6=211;
		System.out.println(in5==in6);  //false
		//系统预先帮我们完成了-128~~127的自动装箱,在这个范围内的自动装箱都是系统提供的,就类似String类提供字符串常量池一样,
		//如果值相同,则只会开辟一块内存，然后不同的引用指向同一块内存,
		//因此内存地址相同,而超出范围的是后来再自动装箱的，所以会重新申请内存;
		
		
		System.out.println("////////////////////////////////////");
		////////////////////////////////////////////////////////////////////////////////////////////
		
		Integer in1 = new Integer(2);
		Integer in2 = new Integer(2);
		System.out.println(in1==in2); //显然不同;
		System.out.println(in1.equals(in2)); //因为Integer类重写了equals方法，所以这里相同;
		
		//int和String转换(非常常用):
		
		String s = "123";
		int re = Integer.parseInt(s);
		System.out.println(re);
		
		int in =100;
		String st = Integer.toString(in);
		String st2=in+"!"; //这种情况下属于字符串的链接，它会默认将非字符串的变量转换成字符串类型的;
		System.out.println(st);
		System.out.println(st2);
		
		String s2 = "30.5";                 //其他的包装类与Integer类的使用方法异曲同工;
		float temp4 = Float.parseFloat(s2);
		
	}
}
//JAVA 尊崇一切皆对象,但是8中基本数据类型不算是对象,所以引用了包装类,把8种基本数据类型包装起来,变为类的形式,这就是包装类


/*以Integer类为例,它实际上就是把一个基本类型的变量作为其属性,以便于引用各种方法来实现把int转换成其他类型变量等的目的(包装类的方法大多是static的,大多数情况直接类名.来调用)：
  
  其构造方法的参数必须是int或只包含数字的String类型,没有无参构造:
 
 Integer(int/string)
 
 注意1: 以下语句的正确与否;
 String a1 = new String (new char[6]);  正确;
 
 //String a2= new char[6]; 编译错误;
		
 Integer b1= new Integer("123");  正确;
 
 //Integer b2= "123";  编译错误;
  
 //int b3 ="123";  编译错误;
 
 注意2:
 
 封装类在某些地方与Sring类型非常类似;
 就以Integer为例,无论是int a = 的范围属于-128~127;还是Integer a = 的范围属于-128~127,为了节省内存空间和减少运行时CPU的负担,JVM都会将它们指向同一块内存,同样的数就如同同样的
 字符串一样只开辟一次空间,然后在常量池中做解析,将拘留地址赋给对应的常量符号;而由于用new Integert()/new String()创建的是一个对象,在编译时无法确定其内容,所以在运行时它才会为此
 对象的参数(也就是Integer/String对象中保存的整数/字符串属性)开辟新的空间,所以无法使用拘留地址空间;
 
 另外,Integer覆写了equals方法,但是要保证调用此方法的是Integer对象,而不是单纯的int变量,如：
 int a = 128;
 int b = 128;
 System.out.println(a.equals(b));  //这里会编译报错;
 
 所以需要Integer a = 128;
        int b/Integer b = 128;
 System.out.println(a.equals(b));  //true;
 
 还需注意的是:
 Integer a =new Integer(129);
 int b =129;
 System.out.println(b==a);  //true,这里由于相比较的不是对象和对象,而是对象和变量,这里JVM做了一个自动拆箱,将a对象中的int整数属性值取出来和b做比较,所以相等,如果是:
 
 Integer a =new Integer(127);
 Integer b =127;
 System.out.println(b==a);  //false; 这里比较的是地址,所以不同;
 
  同样: 
 Integer a =128;
 Integer b =128;
 System.out.println(b==a);  //false; 比较的同样是对象的地址; 
 
  但是:
 Integer a =-128;
 Integer b =-128;
 System.out.println(b==a);  //true; 整数类型的拘留池作用; 
 
 
 补充:
 
Integer.valueOf(String s)是将一个实际值为数字的变量先转成string型再将它转成Integer型的包装类对象,完的对象就具有方法和属性了; 
Integer.valueOf(int s)还能将一个基本类型的变量转化为其包装类型的对象;

Integer.parseInt(String s)只是将是数字的字符串转成数字,注意他返回的是int型变量不具备方法和属性;

其它的包装类如:Float,Double,Long 等都有此类静态方法;

 
 */



