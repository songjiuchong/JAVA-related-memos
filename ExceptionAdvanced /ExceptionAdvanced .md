ExceptionAdvanced 


package prototype;

import java.io.FileInputStream;
import java.io.FileNotFoundException;

public class SongExceptionAdvanced {
	static String aaaaaaaa;
	public static void main(String[] args)throws Exception{  //如果在主方法处声明throws关键字,就算没有在div()的方法调用处声明try..catch...语句块,编译时也不会报错,因为主方法中的错误交给JVM去处理了;

		String aa; //通过编译,之后的语句对aa赋值,不然无法调用,报错;
		//System.out.println(aa);  报错;
		
		String aaaa=null;
		System.out.println(aaaa);
		System.out.println(aaaaaaaa); //成员变量为声明内容,在类加载时会给它们初始化一个初值,所以可在被直接调用;而局部变量必须自己初始化一个值,不然无法被调用;
		
		System.out.println(System.getProperty ("syspro1")+System.getProperty ("syspro2")); //注意此处调用System.getProperty ()方法中的参数必须是字符串类型的;System.getProperty ()方法在未设置相应VM arguments时输出null;
		int array[]={1,2,3};
		
		try{assert array.length==0:"错误出现,array不为0！";
		}catch(Exception e){System.out.println("捕获了assert语句产生的异常！？");}  //无法利用try/catch语句捕获异常;
		
		//断言的格式为：assert boolean表达式 ：“报错信息”;（此处也可不加，则JVM会自动生成）
		//在不加-ea参数运行时,不管断言中的boolean表达式结果是否为真,都会跳过此语句继续执行,想要实现或者说激活这条assert语句,则想要在运行时加上-enableassertions（-ea） 参数,如果boolean 为真则不报错,否则报错;
		//注意：在加了此处的-ea参数显然不是加在Program arguments中,因为Program arguments是通过主方法中的args参数传给程序的;
		//这里的是系统参数所以要加在VM arguments中,也就是Virtual Machine arguments中,此处调用了-ea参数后,程序正常执行,如果assert语句报错,那程序也就不会继续执行下去,assert的报错不同于一般的异常抛出,不能用try/catch捕获,所以assert用于debug/测试;
		
		//////////////////////////////////////////////////////////////////////////////////////////////////////////////////
/*关于VM arguments:
 假设在VM arguments中加入语句: -Dsyspro1=sp1 -Dsyspro2=sp2
 那么在运行程序时就能用System.getProperty ("...")方法在JVM中调用事先声明的syspro1/syspro2变量;
 
	public static void main(String[] args){
       System.out.println("Program arguments");
       for ( String str:args ){
           System.out.println( str );
       }
      
       System.out.println("VM arguments");
       String syspro1 = "syspro1" ;
       System.out.println( System.getProperty(syspro1));
       String syspro2 = "syspro2" ;
       System.out.println(System.getProperty(syspro2));
    }
		
		
		
*/
		
		Math1 a = new Math1();
		try{
		System.out.println(a.div(10, 0));  //由于方法声明处抛出的是一个Exception,会被认为是检测异常,其实方法内出现的是非检测异常,所以如果不加上try..catch..语句块,这里编译会报错说未处理抛出的Exception异常;
		System.out.println("try");  //一旦在try中出现异常,语句块之后内容不会被运行 ;
		
	    }
		catch(Exception e){	               //由于之前在方法中抛出的RuntimeException已经向上转型为了Exception,所以这里抛出的一定是Exception,也只能用Exception来接收;不过如果抛出的是ArithmeticException,
			System.out.println("异常！");  //由于Exception范围更大,是其父类,所以这里用Exception也可以处理其抛出异常;
		}
		
		try{
		throw new Exception("自己抛着玩的异常！");  //利用throw关键字来主动抛出一个异常的对象,并把异常描述放出异常构造器中,但是,一旦利用throw关键字抛出一个异常,就必须用try..catch...处理;
		}
		catch(Exception e){
			System.out.println("处理异常："+e); //在终端输出黑色字体:"处理异常：java.lang.Exception: 自己抛着玩的异常！";
			e.printStackTrace(); //在终端抛出红色字体:"自己抛着玩的异常！";
		}
		finally{
			System.out.println("finally!");
		}
		
		///////////////////////////////////////////////////////////////////////////////////////////////////////////
		String s = "123";
		int s1=Integer.parseInt(s); //和在方法调用处抛出RuntimeException一样,这里参数中的s就算改为"a"这样一定会报NumberFormatException这种RuntimeException的编码在编译时也不会被检查,只有在运行时才会
		//public static int parseInt(String s)  根据API中对parseInt()方法的描述,它会抛出一个数字初始化异常,但是这个异常不需要try..catch...语句块来捕捉处理编译也可以通过,其原因就是：
        //throws NumberFormatException          NumberFormatException是RuntimeException的一个子类,而RuntimeException又是Exception的子类,当方法抛出的异常为RuntimeException的子类
        //时,它可以不用try..catch...语句块来处理,而是把异常交给JVM直接处理,因为这类错误无法在编译期确定,只能等到运行了才知道是否会出现错误,但是为了程序的正常执行,还是推荐使用try..catch...处理;
		
		///////////////////////////////////////////////////////////////////////////////////////////
	    
		try{
			throw new MyException("自定义异常！");
		}
		catch(Exception e){
			System.out.println(e); //黑色字体;
			e.printStackTrace();  //红色字体; 与上一条语句内容相同;
		}
		
		
		/////////////////////////////////////////////////////////////////////////////////
		
		Math2 b = new Math2();
		try{ 
		b.demoe();                     //处理了RuntimeException异常,其实这里不用捕获也会通过编译;
		}
		catch(MyRuntimeException e){   //此处用MyRuntimeException可以接收到异常对象,因为方法中抛出的是一个经过向上转型的RuntimeException对象,对象满足instanceof MyRuntimeException,也就是对象堆内存中有指向MyRuntimeException类信息的地址,
			System.out.println("可以捕获到自定义的RuntimeException!!");  //就相当于一个向下转型的过程,所以就可以接收;
		}
		System.out.println("程序继续执行！");   //至此程序可以继续执行;
		ExceptionDemo.demo1();    //抛出一个检测异常,当主方法 throws Exception时,这里不会再编译时报错,但是运行时会报错;
		
		//值得注意的是,当程序在运行时报了一个错后,因为它之后的语句不会继续执行,下面的错误不会被报出;
		
		b.div2(10,0);  //未处理的RuntimeException异常,由JVM来发布报错信息,在方法中和调用方发处都会报错;
		                                
		
	}
}

class Math1{
	public int div(int a,int b) throws Exception {  //用throws关键字声明表示如果在方法中出现异常,则交给调用方法处处理,此处就不会出现检测报错了,但是一旦方法调用处未处理异常,发生报错,此处也会被包括在报错信息中;
		int c;   //注意如果直接把int c 定义在try内,那么他的生命周期在try之后就结束了,无法让return触及到;
		System.out.println("计算开始！");
		try{
	    c=a/b;     //如果不使用throws关键字,发生异常时这一行会报错;使用后不管是否有异常,如果方法调用处没有try...catch...则报方法调用处的未处理抛出异常错误,如果声明了try..catch...则直接处理;
		}
		catch(Exception e){  //这里接收的e其实是一个RuntimeException对象,用Exception接收后其实就是完成了一个向上转型的过程,之后在方法声明处只能用throws Exception来抛出异常,而在方法调用处也将接受到一个Exception;
			throw e;
		}
		finally{
			System.out.println("计算结束！");
		}
		System.out.println("计算结束！！");  //这条语句无法被执行,一旦抛出异常,之后的语句将不被执行,除非在finally中;
		return c;      //如果在方法中就用try..catch...处理了潜在异常,则方法调用处就安全了,不然需要throws语句来转移异常处理任务;还要注意的是try语句块中抛出了异常之后的语句不会继续被执行,但是catch语句块之后的所有语句都会继续正常执行(包括finally语句块后的);
	}
}

class MyException extends Exception{   //只要继承Exception类或其异常子类就可以自定义异常类;
	public MyException(String s){
		super(s);                      //调用父类中只有一个参数的构造方法输入异常信息;
	}
}

class MyRuntimeException extends RuntimeException{   //只要继承Exception类就可以自定义异常类;
	public MyRuntimeException(String s){
		super(s);                      //调用父类中只有一个参数的构造方法输入异常信息;
	}
}

class Math2{
	public int div2(int a,int b) throws RuntimeException{  //以Exception抛出异常,其实也是一个将RuntimeException向上转型为Exception的过程,所以在方法调用处会抛出一个Exception的异常;
		                                              
		int c ;
		c=a/b;
		return c;
	}
	public void demoe()throws Exception{
		throw new MyRuntimeException("自定义MyRuntimeException！");
	}
}
class ExceptionDemo{
	public static void demo1() throws FileNotFoundException{  //一般throws用于抛出检测异常,从而通过编译;
	FileInputStream fis = new FileInputStream("abc.txt");    //找不到文件的检测异常;
	}
}

///////////////////////////////////////////////////////////////////////////////
/* RuntimeException又称为Unchecked Exception（非检测异常）:此类异常属于程序员本身粗心导致的错误,可以通过代码的完善而避免,所以
 * 此类错误编译器不会去检查，而是在程序执行时才会发现错误;
 * 
 * 其余的Exception被称为检测异常：由于客观或者外部的环境导致的,比如:访问的文件不存在;网络为正常连接等;这类异常由于无法通过程序员
 * 简单的修改代码来避免,所以编译器在编译时会事先报错通知程序员这些可能出现的问题,强制要求程序员去事先处理这些可能发生的异常,
 * 或抛给上层程序处理,以免程序在不明原因的情况下无法正常运行;
 * 
 * 常见的非检测异常：
 * java.lang.ArithmeticException
 * java.lang.NullPointerException
 * java.lang.ArrayIndexoutofBoundsException
 * java.lang.NumberFormatException
 * java.lang.ClassCastException (类型转换异常);
 * 
       关于ClassCast异常需要注意的是:
       假设父类A,子类B;
   A a = new A();
   B b =new B();
   //b=a;  //这里会产生编译报错,但是是由于A类的对象地址中根本没有其子类B类的信息,所以无法传递对象,而不是检测异常;
   b=(B)a; //这里编译通过,但是会发生非检测异常,也就是java.lang.ClassCastException;A类的对象a不能被强制转换为B类的对象;
           //其实这也说明了JVM在发生强制转换时不会去分析被强转的对象和转入的对象的关系,只要强转后的等式两边匹配就编译通过,运行时才会发现无法强转这样的异常;
           
 * 检测异常：
 * java.io.IOException
 * java.sql.SQLException
 * java.io.FileNotFoundException
  
  
/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// 
 
Exception/finally 终极演示;


情况1：try块中没有抛出异常try和finally块中都有return语句

public static int NoException(){  
 int i=10;  
 try{  
  System.out.println("i in try block is"+i);  
  return --i;  
 }catch(Exception e){  
  --i;  
  System.out.println("i in catch - form try block is"+i);  
  return --i;  
 }finally{  
    
  System.out.println("i in finally - from try or catch block is"+i);  
  return --i;  
 }  
}  

执行结果：
i in try block is10
i in finally - from try or catch block is9
the method value is8
执行顺序：执行try块，执行到return语句时，先执行return的语句，--i，但是不返回到main 方法，执行finally块，遇到finally块中的return语句，执行--i,并将值返回到main方法，这里就不会再回去返回try块中计算得到的值
 
情况2：try块中没有抛出异常，仅try中有return语句
代码：

public static int NoException(){  
    int i=10;  
    try{  
        System.out.println("i in try block is--"+i);  
        return --i;  
    }catch(Exception e){  
        --i;  
        System.out.println("i in catch - form try block is--"+i);  
        return --i;  
    }finally{  
          
        System.out.println("i in finally - from try or catch block is--"+i);  
        --i;  
        System.out.println("i in finally block is--"+i);  
        //return --i;  
    }  
}  

执行结果：
i in try block is--10
i in finally - from try or catch block is--9
i in finally block is--8
the method value is--9
顺序：try中执行完return的语句后，不返回，执行finally块，finally块执行结束后，返回到try块中，返回i在try块中最后的值
 
情况3：try块中抛出异常try,catch,finally中都有return语句
代码：

public static int WithException(){  
    int i=10;  
    try{  
        System.out.println("i in try block is--"+i);  
        i = i/0;  
        return --i;  
    }catch(Exception e){  
        System.out.println("i in catch - form try block is--"+i);  
        --i;  
        System.out.println("i in catch block is--"+i);  
        return --i;  
    }finally{  
          
        System.out.println("i in finally - from try or catch block is--"+i);  
        --i;  
        System.out.println("i in finally block is--"+i);  
        return --i;  
    }  
}  

执行结果：
i in try block is--10
i in catch - form try block is--10
i in catch block is--9
i in finally - from try or catch block is--8
i in finally block is--7
the method value is--6
顺序，抛出异常后，执行catch块，在catch块的return的--i执行完后，并不直接返回而是执行finally，因finally中有return语句，所以，执行，返回结果6
 
情况4，catch中有return,finally中没有，同上，执行完finally语句后，依旧返回catch中的执行return语句后的值，而不是finally中修改的值
情况5：try和catch中都有异常，finally中无return语句

public static int CatchException(){  
    int i=10;  
    try{  
        System.out.println("i in try block is--"+i);  
        i=i/0;  
        return --i;  
    }catch(Exception e){  
        System.out.println("i in catch - form try block is--"+i);  
        int j = i/0;  
        return --i;  
    }finally{  
          
        System.out.println("i in finally - from try or catch block is--"+i);  
        --i;  
        System.out.println("i in finally block is--"+i);  
        //return --i;  
    }  
}  

结果：
i in try block is--10
i in catch - form try block is--10
i in finally - from try or catch block is--10
i in finally block is--9
Exception in thread "main" java.lang.ArithmeticException: / by zero
 at exception.ExceptionTest0123.CatchException(ExceptionTest0123.java:29)
 at exception.ExceptionTest0123.main(ExceptionTest0123.java:17)
执行顺序：在try块中出现异常，到catch中，执行到异常，到finally中执行，finally执行结束后判断发现异常，抛出
 
情况6：try,catch中都出现异常，在finally中有返回

public static int CatchException(){  
    int i=10;  
    try{  
        System.out.println("i in try block is--"+i);  
        i=i/0;  
        return --i;  
    }catch(Exception e){  
        System.out.println("i in catch - form try block is--"+i);  
        int j = i/0;  
        return --i;  
    }finally{  
          
        System.out.println("i in finally - from try or catch block is--"+i);  
        --i;  
        System.out.println("i in finally block is--"+i);  
        return --i;  
    }  
}  

运行结果：
i in try block is--10
i in catch - form try block is--10
i in finally - from try or catch block is--10
i in finally block is--9
the method value is--8
执行顺序：try块中出现异常到catch，catch中出现异常到finally，finally中执行到return语句返回，不检查异常
 
没有catch，只有try和finally时，执行顺序和上面的几种情况差不多，只是少了catch块的执行


情况X：

public static int CatchException(){  
    int i=10;  
    try{  
        System.out.println("i in try block is--"+i);  
        i=i/0;  
        return --i;  
    }catch(Exception e){  
        System.out.println("i in catch - form try block is--"+i);  
        int j = i/0;  
        return --i;  
    }finally{  
          
        System.out.println("i in finally - from try or catch block is--"+i);  
        --i;  
        System.out.println("i in finally block is--"+i);  
        return --i;  
    }  
    return i;
}  

注意:此处的finally之后的return会产生编译报错:Unreachable code;原因是将所有代码看出一个整体,暂时将finally作为特殊代码块不去考虑,那么只要在最后一个return i语句
之前发生了未捕获的异常或是已经return了就无法在运行到最后一个return了,而finally一定会在catch()语句之后执行(如果没有catch语句则一定在try之后执行),所以无论是否异常被
捕获了(如果报出异常未捕获则在执行完finally代码块后抛出,如果finally中有return语句则返回语句执行后异常不会被抛出),都会去执行finally中语句,所以之前的return语句产生的
返回值会被"更新",那么也就是说只要在最后一个return语句之前发生了return或是有未捕获异常被抛出都将导致此return无法被执行到;
  
*/





