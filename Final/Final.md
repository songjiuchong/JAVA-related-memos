Final


package prototype;

public class SongFinal {
	
	
	public static void main(String[] args) {
		S3 p1 = new S3();
		//S3.MARVAL="";                        //改变常量,报错！
		System.out.println(""+p1.a+p1.b);      
		//p1.a=1; p1.b=2;                      //作为对象的一个属性,对象中final参数不能在被这个对象改变;(报错！)
		S3 p2 = new S3(3,4);                   
		System.out.println(""+p2.a+p2.b);      //因为对象不同,输出的final a ,final b 是不一样的;
		//p1.a=3; p1.b=4; 
		
		////////////////////////////////////////////////////////////
		
	}

}
final class S{                         //定义一个final类,它不能有子类！
}
//class S2 extends S{                  // 报错！
//}
 class S3{
	 
	 final int a;                      //用final声明的变量不是常量，虽然不能被改变,但是不同的对象可以拥属于自己的有不同的
	                                   //final 变量,由此可知，可以在声明final参数时不给初值,但是随后在每一个构造方法里必须
	                                   //给final参数赋值！如果在类中就为final变量赋予了初值,那么之后声明的每一个对象都会拥有
	                                   //这个相同的值,且不能被改变;
	 final int b;
	 
	 int c=1;
	 
	 public S3(){
		 this.a = 1;
		 this.b = 2;
	 }
	 
	 public S3(int a, int b){
		 this.a=a;
		 this.b=b;
	 }
	 
	 public static final String MARVAL=""; //这样的static和final联合指定,称为全局常量指定(常量);不能指定空的final变量,
	                                       //所以必须给个初值,也就是最终值;（声明常量的名称必须全用大写！！）
	 
	 public final void print(){
		 System.out.println("final方法不能被子类覆写");
	 }
	 ///////////////////////////////////////////////////////////////////////////
	 //关于final类型参数的限制：
	 public void test1(final int a){ //当整型变量参数被设置为final时,它无法在方法体内被再次修改;
		 //a=1;   //这里会报错,因为参数a被定为final之后传入test1方法(一定是已经被初始化的,不然传入时报错),已无法再被修改;
	 }
	 
	 public void test2(final S3 s ){ //当引用类型变量参数被设置为final时,它无法指向一块新的堆内存;但是可以改变它其中的成员变量;
		 //s=new S3();   //这里会报错,因为对象参数s被传入test2方法后无法重新指向另一块内存;
 		 s.c++;     
	 }
 }
 final class S4 extends S3{                       //但是final类可以有父类;
	 //public final void print(){                 //final方法不能被子类覆写,报错！
		 //System.out.println("不能覆写");
	 //}
 }
 
 //注意:数据成员既可以是final,也可以不是,取决于我们具体选择,final类的实例化对象不是final的,其引用可以被改变;
 //将类定义成final后,结果只是禁止进行继承————没有更多的限制;然而,由于它禁止了继承,所以一个final类中的所有方法都默认为final;
 //因为此时再也无法覆盖它们;还需要注意的是:final方法可以被子类继承,但是不能覆写(无论是覆写其final形式还是无final限制的形式);
 //final方法可以被重载(无论是重载其final形式还是无final限制的形式都可以),子类继承了一个final方法后虽然不能覆写它,但是可以重载
 //它,因为重载相当于定义了一个新的方法,而不是改写原来的方法;
 
 
 