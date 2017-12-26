MultipleStatusCaution2.0


package prototype;

public class SongMultipleStatusCaution{
	public static void main(String args[]){
		BB b = new BB();            //在向下转型时必须注意的是如果之前没有执行过向上转型,那么BB和BB2中未建立关系！
		if(b instanceof BB2){		 /*相当于在判断b的内存中是否包含BB2类的地址;这里显然不包含,因为子类对象才会包含父类的地址,父类对象不会保存其子类的地址;
		注意:如果想要把某个类的对象b的实例化内存地址赋给另一个类的对象a,先要满足b是a所在类的一个实例(b中存有a所在类的地址),不然b的实例内存中根本没有指向a所在类的地址,赋值之后的a指向的是b的对象内存,可其中无任何关于a所在类的内容,就自然无法进行任何属于a所在类有关的操作;
		*/
			
			BB2 a = (BB2)b;        //编译时a与类BB2绑定,b的引用地址给a,语法上没问题,通过编译,但是显然会运行时报错;因为此时的a对象要接收一个可以说是对其类根本没有任何信息交互的对象;
			
		                           //注意：这条语句并没有在b和a中建立关系;如果是BB2 b = new BB2(); BB a = b;这样才在a与b中建立了关系,由于b所在子类
								   //本身就有其父类的地址,所以将它的对象内存赋给a,之后a虽然指向一个非a所在类的一个其他类对象地址,但是还是能够调用a所在
								   //类中的内容;
			
		a.print1();                
		a.print2();                  
		/*编译时BB2类的方法表中能找到print1与print2方法,所以编译通过,但是if语句判断时,b中无BB2类的地址,所以if语句块中内容不会执行！*/
		}
		
		
		BB b2=new BB2();           //正确的向上/向下转型;
		b2.print1();
		System.out.println(b2.a); //这里输出的是2,因为子类继承了父类的变量a,由于向上转型,b2编译时与父类绑定，而变量不存在覆写,
		                          //子类中即存在继承下来的父类变量a,又存有自己声明的变量a; 由于绑定,b2无法访问子类的非继承,非覆写内容;
		if(b2 instanceof BB2){
			BB2 a2 = (BB2)b2;
			a2.print1();
			a2.print2();
			System.out.println(a2.a);
		}
	}
}
class BB{
	int a =2;                   //父类的变量a;
	public void print1(){
		System.out.println("父类的方法");
	}
	
}
class  BB2 extends BB{
	int a=1;                   //子类自己的变量a，同时还继承了父类的变量a;
	public void print1(){
		System.out.println("子类覆写父类的方法");
	}
	public void print2(){
		System.out.println("子类的方法");
	}
}
