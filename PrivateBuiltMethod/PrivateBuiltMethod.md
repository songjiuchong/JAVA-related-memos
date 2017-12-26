PrivateBuiltMethod

package prototype;
class PrivateBuiltMethod{
	static int a =1;
	int c=2;
	private static PrivateBuiltMethod ss = new PrivateBuiltMethod(); //在自身类的内部可以正常实例化对象，这里是一个被封装的静态对象;
	public  static PrivateBuiltMethod getSs(){
		return ss;
	}
	private PrivateBuiltMethod(){                                   //建立一个被封装了的构造方法;
		
	}
	public void print(){
		System.out.println("调用方法");
		System.out.println(PrivateBuiltMethod.ss.c);//即使c为非静态变量,但是通过类调用静态对象后再调用它也是可以的,毕竟ss还是属于对象的范畴;
	}
	//int y=PrivateBuiltMethod.c; 这样调用就会报错了,因为直接通过类来调用的必须是静态的;
}

public class SongPrivateBuiltMethod {
	public static void main(String [] args){
		PrivateBuiltMethod s1 = null;    //声明一个对象，还未实例化，所以不会报错;
		PrivateBuiltMethod s2 = null;
		PrivateBuiltMethod s3 = null;
		s1=PrivateBuiltMethod.getSs();   //只能通过调用在其自身类内部实例化的静态对象的“取得方法”（因为这个对象被封装了）实现
	                                	 //建立新的对象;
		s2=PrivateBuiltMethod.getSs();   
		s3=PrivateBuiltMethod.getSs();   //这里建立多个对象他们都指向同一个内存地址，属于单态（单例）设计模式，其意义就是在入口处
		                                 //(构造方法处)限制了实例化对象的条件（不希望产生过多的对象）
		int b =PrivateBuiltMethod.a;     //应用参数时同理，需要静态参数，因为是被类直接引用 
		int d=s1.c;                      //非静态变量可以在静态对象中存在;
		s1.print();                      //成功调用方法;
		s2.print();
		s3.print();
	}
}
