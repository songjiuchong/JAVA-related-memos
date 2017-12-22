Main 2.0

package prototype;

public class SongMain {
	public static void main(String []args ){
	/*
	主方法的参数在编译操作：javac 类名称.java结束后，运行java程序：java 类名称的时候输入， 输入格式为：一个参数空一格,如：1 2 3或 "hello world" "hello world2" ，参数的数量不限;
	这里的static表示此方法会直接通过类调用 ，比如执行程序时：java 类名称;
	void表明这个方法是整个程序的开始和结束,不需返回任何东西; 
	*/
		if(args.length!=3){
			System.out.println("参数必须为三个，程序错误！退出！");
			System.exit(1);                            //exit()中参数为非0，则属于异常退出执行的程序
		}
			for(int i = 0; i<args.length;i++){
				System.out.println("第"+(i+1)+"个参数为："+args[i]);            
		}
		
		
		
		
		
        SongMain2 test= new SongMain2();               /*fun();无法被调用，因为主方法是静态方法，“fun();”这个语句默认为类来直接调用, 而fun为非静态方法；而且他属于另一个类，必须通过实例化那个类的对象调用*/
		test.fun();                                    //所以这里只能通过对象来调用其他类中的非静态方法
		
		fun1();                                        //本类中的静态方法,直接调用;
		}
	  public static void fun1(){
		  System.out.println("静态方法输出");    
	  }
	}
  
class SongMain2{
	public void fun(){
		System.out.println("非静态方法输出");
		
	}
}