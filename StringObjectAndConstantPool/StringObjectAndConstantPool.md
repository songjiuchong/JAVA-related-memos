StringObjectAndConstantPool 


package prototype;


public class SongStringObjectAndConstantPool {
	//int a =1; 在类中不能对一个对象或参数赋值,只能初始化,赋值操作在方法中(也就是栈中)进行;
	//a=2; 报错;
	
	/*
	字符串常量池概念：
	 
	   java中的常量池技术，是为了方便快捷地创建某些对象而出现的，当需要一个对象时,就可以从池中取一个出来（如果池中没有则创建一个）,
	  则在需要重复创建相等变量时节省了很多时间。常量池其实也就是一个内存空间，常量池存在于方法区中;
	  
	     在Java源代码中的每一个字面值字符串，都会在编译成class文件阶段，形成标志号为8(CONSTANT_String_info)的常量表 ;
	当JVM加载 class文件的时候，会为对应的常量池建立一个内存数据结构，并存放在方法区中;
	同时JVM会自动为CONSTANT_String_info常量表中的字符串常量的字面值 在堆中创建新的String对象(intern字符串对象 ,
	又叫拘留字符串对象)。然后把CONSTANT_String_info常量表的入口地址转变成这个堆中String对象的直接地址(常量池解析);
	  
	     JVM的一个实例中所有相同字面值的字符串常量只可能建立唯一 一个拘留字符串对象;
	实际上JVM是通过一个记录了拘留字符串引用的内部数据结构来维持这一特性的;
	在Java程序中，可以调用String的intern()方法来使得一个常规字符串对象成为拘留字符串对象;
	
	对于任何两个字符串 s 和 t，当且仅当 s.equals(t) 为 true 时，s.intern() == t.intern() 才为 true;
/////////////////////////////////////////////////////////////////////////////////////////////////////
 	
	 java中基本类型的包装类的大部分都实现了常量池技术，这些类是Byte,Short,Integer,Long,Character,Boolean,
另外两种浮点数类型的包装类则没有实现。另外Byte,Short,Integer,Long,Character这5种整型的包装类也只是在对应值小于等于127时才可使用常量池,
也即对象不负责创建和管理小于127的这些类的对象;测试代码：

public class Test{ public static void main(String[] args){

//5种整形的包装类Byte,Short,Integer,Long,Character的对象，
//在值小于127时可以使用常量池
 
Integer i1=127;
Integer i2=127;
System.out.println(i1==i2); //输出true

//值大于127时，不会从常量池中取对象
 * 
Integer i3=128;
Integer i4=128;

System.out.println(i3==i4); //输出false


//Boolean类也实现了常量池技术
 * 
Boolean bool1=true;
Boolean bool2=true;
System.out.println(bool1==bool2); //输出true


//浮点类型的包装类没有实现常量池技术
Double d1=1.0;
Double d2=1.0;
System.out.println(d1==d2); //输出false
}
}
	
	*/

	public static void main(String[] args) {
        String name = "song"; //其实“song”相当于是一个String类的匿名对象,它的引用是常量池中“song”符号;内存地址在intern pool中开辟;
        String name2 = "ling"; 
        String name3 = "song";
        
        System.out.println(name==name2);//输出结果为false因为用==时判断的是大小是否相等，所以这里比较的是它们指向的堆内存地址;
        System.out.println(name==name3);//true 它们在同一个拘留字符串的堆地址;
        System.out.println(name.equals(name3));//输出结果为true因为.equals()方法比较的是字符串内容是否相同
       
        String str1 = "a";
        str1 = "b";    //根据String类的特点，此时str1并没有丢失堆内存中“a”的内容，只是重新指向了一个新的堆内存，其内容为“b”
      
        String str2 = new String("c");//由于"c"已经开辟了其特定的拘留池堆内存, 重新声明对象会将对象str2指向一个新的堆内存地址，造成内存的浪费;
                    
        ////////////////////////////////////////////////////
        String s1 = new String("Hello world"); //事实上，在运行这段指令之前，JVM就已经为"Hello world"在堆中创建了一个拘留字符串,
        //(值得注意的是：如果源程序中还有一个"Hello world"字符串常量,那么他们都对应了同一个堆中的拘留字符串);
        //然后用这个拘留字符串的值来初始化堆中用new指令创建出来的新的String对象，局部变量s实际上存储的是new出来的堆对象地址;
		   System.out.println(s1=="Hello world"); //false;  s1是实例化对象堆内存的引用,"s"直接指向了intern pool的地址;
		   System.out.println(s1.intern()=="Hello world"); //true;  调用String的intern()方法来使得一个常规字符串对象成为拘留字符串对象;
		   
	    /////////////////////////
		 B4.test();//true; 参见下面对A4,B4类中代码的详解;
		 
	}

}

class A4{

	static String a="1"; 
	}

class B4{
	public static void test(){

	String test1=A4.a;  //把A4类中的String对象赋给本类的String对象,当程序运行到此条语句时,A4类被加载,由于A4类在其编译时已对字符对象与字面量做了标记,加载后其常量池在方法区被创建,接着JVM会执行创建/链接拘留池(intern pool)内存的任务(常量池解析);也就是说,此时"1"已经以拘留字符对象的形式存入了堆内存中,并且test1所对应的内存地址就是"1"的拘留字符对象地址;

	String test2="1"; 
	/* 而其实早在B4类编译时就已经为这条语句做了标记,之后在其加载时这条语句已经使得JVM在方法区B4类的字符串常量池中加入了"1"的符号,接着JVM去intern pool中寻找是否有相同的字符串对象(利用.equals()方法),如果有那就直接把这个intern pool的地址链接这个符号(常量池解析);由于之前的语句已经让A4类得以加载,并且创建了"1"的拘留字符对象,又由于一个JVM实例对应一个拘留池,
	所以，B4类在常量池解析时就把之前A4类加载时的"1"拘留对象地址连接了B4类字符常量池中的"1";
	需要注意的是：理论上来说intern pool也是属于方法区的,又由于方法区是堆内存分配的,所以可以认为intern pool是存在堆内存中的; 
	*/

	System.out.println(test1==test2); //运行结果为true;由上两条语句的分析可以得出test2对应的地址就是test1对应的地址——"1"的拘留字符地址;

	}
	}


//注：关于字符串的链接和StringBuilder类请参考"SongJAVASElang2.java";



/*pool补充说明:

字符串字面量,包括用+连接的字符串字面量在编译阶段已近被标记为连接后的字符串符号,以便于之后在类被加载时字符串常量池的建立;而字符串常量池的建立,字符
串拘留池的建立,包括他们的连接(常量池解析),都是在类被加载的过程中(载入class文件到内存->连接<声明类在方法区的内存区域,常量池解析等>->初始化类的
静态属性等)的连接这一步完成的;而在之后程序真正开始运行时,只有调用intern()方法才可能声明一块新的字符串拘留内存并更新常量池后完成解析;比如:两个含
有字符串类型的变量的连接操作,String a = b + c;a.intern();或者String d = new StringBuilder(a).toString(); d.intern();不然只会存在用拘留池地址对
字符串对象赋值的操作;

需要注意的是整个方法区只存在一个字符串常量池,也就是一个JVM对象只对应一个字符串常量池,这和被加载到方法区的类的常量池不同;

每当一个类被加载到方法区都会有一个与之对应的常量池产生;一个在方法区存放的类，它的main()方法，包括其它方法中的元素将按照它们存在于方法中的顺序,将它们的符号引用放入常量池;方法在执行时会按常量池中此方法中各符号引用的
顺序来将它们放入栈中等待执行。一旦需要调用一个类则会先检查它是否已经加载,如果未加载则先加载，然后常量池中对于这个类引用的符号引用会解析为指针的
直接指向引用，指向方法区的这个类信息的存放地址，之后就可以直接访问此类了;而在栈方面：假设顺序依照常量池中main()方法里的符号引用，执行到某个静态
方法：A.f（）；这样的语句，那JVM首先检查类A的class类实例是否存在，如果不存在则先加载，动态链接（解析常量池），然后按照常量池中对A类的指针引用找到
方法区A类的信息，然后找到其中f()方法的实际内容替换栈帧中此方法的符号引用,这种加载必要的类来连接未解析的符号引用的动作称为：动态链接（常量池解析),常量池解析的一个非常重要的作用是可以方便类来实例化对象;所以如果刚才
类A中main()执行的语句为A a=new A(); a.f();则其实现的原理是一样的;

*/

