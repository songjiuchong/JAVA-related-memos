Exception

package prototype;

public class SongException {
	public static void main(String[] args) {
		System.out.println("/////////计算之前///////////");
		int i =10;
		int j =0;
		try{                      //try{}用来捕获异常，如果语句块中发生异常则不会继续运行语句块中剩下的内容，而是去catch{}匹配
		String a = args[0];       //输入运行参数来测试异常
		String b = args[1];
		i = Integer.parseInt(a);
		j = Integer.parseInt(b);
		int temp=i/j;             //由于除数不能为零，程序运行到这里时会结束程序，把异常内容告知用户，之后的语句不会继续执行。
		System.out.println("计算结果为： "+temp);
		}
		catch(ArithmeticException e){  //catch(){}用来处理异常，如果catch()中内容符合捕获的异常，则运行语句块中内容，然后继续程序
		//System.out.println("数学计算异常： "+ e);
		
		e.printStackTrace();           //是Throwable类的方法，效果就是打印异常内容，和上一条语句相符;
		
		System.out.println(e.getMessage()); // e.getMessage(); 返回一个异常内容的字符串,仅包含错误类型信息,没有具体定位信息; 注意这里的输出非红色字体,容易忽略;
		
		}
		catch(ArrayIndexOutOfBoundsException e){  //catch(){}用来处理异常，如果catch()中内容符合捕获的异常，则运行语句块中内容，然后继续程序
			System.out.println("数组索引绑定异常： "+ e);
			}
		catch(NumberFormatException e){  //catch(){}用来处理异常，如果catch()中内容符合捕获的异常，则运行语句块中内容，然后继续程序
			System.out.println("数字格式化异常： "+ e);
			}
		catch(Exception e){                      //通过向上转型来处理所有可以捕获到的异常，在所有异常用统一处理方式时直接用Exception父类接收会更加便利，
			System.out.println("其他的异常： "+ e); //但是细分化的异常处理在实际开发中更能被接受;要注意的是，像Exception这种更大范围的异常处理语句块必须
			                                       //放在范围更小的处理语句块之后，也就是在它子类的异常处理语句块之后，不然编译时会出现“异常已被捕获的报错”
		}
		finally{                     //作为所有异常的出口，不管之前有没有捕获到异常，都会指向语句块中的内容
		System.out.println("异常的统一出口，不管有否异常都会执行 ");//如果没有catch(){}处理异常语句，则会在报错后执行finally里的语句
		}
		System.out.println("/////////计算之后///////////");    
		
		//以上语句由参数来决定计算结果，所以有3个出现异常的风险：
		//1.ArrayIndexOutOfBoundsException  如果输入的参数不足，会导致数组中未初始化内存要求被赋值，会报错
		//2.NumberFormatException           如果输入了非数字参数，则Integer.parseInt();无法正常转换为int类型的数据，报错
		//3.ArithmeticException             除数为0时的报错
		
		//在java的异常结构中有2个最常见的异常类：Exception(可以通过try和catch处理的异常)和Error(JVM错误，无法处理的程序异常);
		//它们是Throwable的子类;而之前捕获的3种Exception都是Exception的子类;
		
		/*异常的处理机制：如果出现异常，则JVM首先会实例化一个此异常类的对象,在try语句中对发生的异常进行捕获;然后把产生的异常对象与
		 * catch()中的异常类进行匹配，如果属于某一个异常类则把此对象赋给catch()中的引用变量,并执行catch语句块中的代码;
		
		那么在某些时候用Exception(){}直接处理确实会更便捷，那么用Throwable()会更好吗？答案是否定的，因为try{}中能捕获的只有Exception
		异常，而Throwable()中还包括了Error类的异常，所以在概念上把try和throwable搭配使用是错误的
		
		
		
		*/
		//计算机发展史上的2大杀手：1.断电  2.除数为零,会导致结果无穷大,占满内存;
	}
}

