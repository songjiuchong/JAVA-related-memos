CodeBlock

package prototype;


class SongCodeDemo{
	static{
		System.out.println("静态代码块");      //用static修饰的静态代码块，它将优先于构造块与构造方法，
		                                      //并且实例化多个对象时，它只被运行一次，用于初始化静态属性
	}
	public SongCodeDemo(){
		System.out.println("构造方法");        //构造方法
	}
	{
		System.out.println("构造块");          //在类中的代码块称为构造块
	}
}

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
//ultimate test:

class fatherx{
	int a;
	static int x =1; //由于x静态属性定义在静态方法块之前,所以在类初始化时将先被赋值,然后在静态块中又一次被赋值,所以最后x为2,反之亦然;
	static{
		//a=1;  //与静态方法相同的是,静态代码块其中也只能访问类的静态属性;
		x=2;
		System.out.println("parent static code block");
	}

	public fatherx(){  //这里需要注意的是无论是构造方法在类中的位置在前还是构造块,都会先执行构造块中的内容;
		System.out.println("parent structive method");
		this.print();		
	}
	{
		//代码块相当于定义一块匿名方法的代码区,所以它的作用域仅限于代码块内,
		//如果想要初始化属性就必须现在类中定义好,在代码块中定义的变量生命周期就在块内;
		a=1;  
		System.out.println("parent structive code block");
	}
	
	public void print(){
		{System.out.println("parent normal code block");}
		System.out.println(this.a);
	}
}

class sonx extends fatherx{
	int b;
	static{
		System.out.println("child static code block");
	}
	{	
		b=3;
		System.out.println("child structive code block");
	}	
	public sonx(){
		//super();
		System.out.println("child structive method");
	}
	public void print(){
		{System.out.println("child normal code block");}
		System.out.println(this.b);
	}
}

public class SongCodeBlock {
	static{
		System.out.println("主方法所在类中的静态代码块");   //在主方法所在类中定义静态代码块 ,它将优先于主方法运行，它
		//System.exit(0);                                 //甚至可以抛开主方法独立运行，不过为了不报错，需要exit来退出程序
		                                                  //System.exit(0);参数0为正常退出，非0为程序异常退出，结果相同，只是
		                                                  //为了方便编程者分类讨论
	}
	
	public SongCodeBlock(){
		System.out.println("主方法所在类的构造方法"); 
	}
	
	{
		System.out.println("主方法所在的类中的构造块");   
	}
	
	
	public static void main(String[] args) {
		{
			System.out.println("普通代码块");   //直接定义在方法中的代码块称为普通代码块;
		}
		new SongCodeDemo();                    //实例化对象,运行结果为:构造块和构造方法都输出了“3遍”,并且构造块优先于构造方法;
		new SongCodeDemo();
		new SongCodeDemo();
		new SongCodeBlock();
		//////////////////////////////////////////////////////////////////////////////////////////////////////
		//ultimate test:
		sonx s = new sonx();  //这里输出的值为0,原因之前在SongThis.java中也曾提到过,这里不再重复;
	}
}

/*

终极理解: 
  
假设子类和父类都不存在主方法(不然在程序刚运行时就已经被加载了),子类第一次创建对象且其父类未被加载过;

首先当运行到子类创建对象的方法时,JVM会检查并将其父类加载,加载的过程中父类的所有静态属性被初始化(开辟空间,赋初始值),
然后按照静态属性与静态代码块在类中的出现顺序来执行赋值和运行的工作;<parent static code block>

然后子类经历了同样的过程;<child static code block>

接下去来到了子类的创建对象环节,首先JVM会为这个新的对象开辟堆内存空间,将父类和子类的初始化工作完成(为变量赋初值放入堆内存,
将方法区类信息地址放入堆内存等);之后是运行子类构造方法中的内容,super();一旦出现super(),子类构造方法会将自己最后的初始化工作(执行构造块中内容,为属性赋值)搁置,
先去找到父类的构造方法,然后进行父类最后的初始化工作(执行构造块中内容,为属性赋值);需要注意的是这里的初始化工作的顺序是由属性和构造块在类中出现的顺序一致,这点和JVM在
初始化类中静态属性和静态块的机制是一样的;<parent structive code block>

接着运行父类构造方法中的所有内容;<parent structive method> <child normal code block> <0>

然后真正来到子类构造方法的执行阶段,子类进行最后的初始化工作(执行构造块中内容,为属性赋值);<child structive code block>

最后,运行子类构造方法中的所有内容;<child structive method>



*/

