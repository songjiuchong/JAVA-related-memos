AnonymousInnerClass

package prototype;

public class SongAnonymousInnerClass {
	public static void main(String[] args) {
		final int c =1;           //和局部内部类异曲同工,都是存在于方法中的类;由于内部类访问外部方法中的局部变量是被允许的,但是
		//c=c+1;                  //由于生命周期的不同,方法结束的时候局部变量销毁,但是内部类中的变量还是存在的,此时会出现引用指向
		                          //已被销毁的内存的情况;再加上如果之后在方法中要通过匿名对象.调用变量的话就必须使得变量是存在于类之中的,
		                          //所以java会复制一份局部变量到内部类中,“延长”它的生命周期;那如果方法中局部变量在声明匿名内部类之后被更改了,
	                              //那此时内部类中所保存的变量与方法中的变量其语义效果不一致了,为了使其保存一直,所以
		                          //需要final声明其访问的外部方法的变量或是参数，具体参见java tips中的内部类相关内容;
		
		
		
		
		AAA a = new AAA(){
			public void print(){
				int d = c+1;        //访问了外部方法中的局部变量;
				System.out.println("匿名内部类方法调用成功！");
			}
			public void printx(){   //扩展方法通过编译,但是无法用a对象来调用;
				int d = c+2;       
				System.out.println("匿名内部类扩展方法调用成功！");
			}
		};                   
		
		BBB b = new BBB(){
		    	int d = c+1;        //访问了外部方法中的局部变量;
			public void print(){
				System.out.println("匿名内部类方法调用成功！");
			}
			public void printx(){   //扩展方法通过编译,但是无法用a对象来调用;
				int d = c+2;       
				System.out.println("匿名内部类扩展方法调用成功！");
			}
		};                           //这里的分号是本应该在AAA a = new AAA();这里的;接口的实例化,其实是
		                             //匿名地临时地创造了具有某个接口功能的类的实例，往往是我们需要实现某种功能,
		                             //但这个类的实现,并不具有现实意义,也不便于维护和管理,的一种妥协行为,
		                             //某种意义上破坏了面向对象的特性,但是方便了编程人员;
		                             //你可以看做他其实是某个实现了该接口的类的实例，而不是这个接口的实例;
		                             //这里的;不能忘记加！
		a.print();
		//a.printx();  //这里要注意的是由于通过匿名内部类来实现某个接口或者某个抽象类一定是以向上转型的方式实现的,所以一个实现/继承接口/抽象类的方法体中一定可以有覆写与类中自己扩展的方法(包括重载继承/覆写后的方法),
		               //但是实现接口和继承抽象类的匿名内部类对象无法调用任何扩展方法;
		b.print();
		//b.printx();  //同理,继承抽象父类的匿名内部类对象无法调用其扩展方法; 
		
		//////////////////////////////////////
		 AAAA.a.print();   
		 AAAA.a.print("1");   
		 System.out.println(AAAA.a.aa);  
		 //AAAA.a.printx(); //无法调用其扩展方法;
		 
		 AAAA a2 = new AAAA(){
				public void print(){
					System.out.println("普通类中的匿名内部类覆写了类中的方法");
				}
				void print(int a){
					System.out.println("普通类中的匿名内部类重载了类中的方法"+a);
				}
				int b=aa+1;
			};
			a2.print();     //成功调用;
			//a2.print(9);  //重载继承下来的方法也属于子类的扩展方法,所以这里无法调用;
			//System,out.println(a2.b); //b为匿名内部类的扩展属性,这里无法访问;
	}

}
interface AAA{
	public void print();
}
abstract class BBB{
	public abstract void print();
	
}


//如果在类中声明本类的匿名内部类(虽然这么做没有意义),但是注意其声明的格式：
class AAAA{
	static int aa= 1;
	public void print(){ 
		System.out.println("AAAA 方法");
	};
	public void print(String b){
		System.out.println(b);
	}
	public static AAAA a = new AAAA(){
		public void print(){
			System.out.println("普通类中的匿名内部类覆写了类中的方法");
			aa=2; //此处的aa变量是属于外部类的而不是外部方法,它的生命周期要比内部类中的变量长,所以无需一定为final的;
		}
		public void printx(){
			System.out.println ("扩展方法！");  //由于这里的匿名内部类是一个普通的继承类,相当于临时声明了一个继承这个普通类的子类对象,也是向上转型,所以无法调用内部类中的扩展方法;
		}
	};
}


/*
 特别注意：
 在声明匿名内部类的时候,其实就是创建了一个实现/继承声明类的一个子类的匿名实例;
 
 如果此匿名内部类实现了一个接口:
 
 interface i1=new interface(){
 @Override
 		public void test(){
 		
 		}
 
 }; 
  
  相当于:
 class normal implements interface {
 		@Override
 		public void test(){
 		
 		}
 };
interface i1 = new normal();  //只不过匿名内部类不会对实现接口类命名;

而类似继承接口之后的normal类不通过向上转型的方式初始化对象是无法在匿名内部类中做到的:
normal i1 = new normal();

抽象类同理;

 
 i1就是向上转型后那个匿名实例的对象,代码块中的内容就是实现/继承interface的那个子类的内容,
  所以在代码块中覆写父类方法/重载继承后的方法,声明子类自己的扩展方法都是可以的;但是当利用i1去
 调用方法时,这些没有通过动态绑定的方法仍旧视为是只属于子类本身的,无法享受对象的多态,也就不能
 被调用了,所以最后能被调用的只有覆写过的父类方法/继承下来的方法;
 
 另外还需特别注意的是:
 在接口中是无法定义private/static/final的抽象方法的,抽象类中的抽象方法也必须满足此条件;只有public/abstract/protected(抽象类)修饰符可用;原因是接口的抽象方法是需要被实现才有意义的,如果定义为private/static/final
 那其实现类根本无法覆写(实现)接口/抽象类的方法,因为对于子类来说private/static/final修饰符都会使得被修饰方法对子类invisible,也就是子类中虽然有这些父类方法的地址,但是无法将它们作为子类自己的东西,也就是无法覆写它们,
 只能在调用这些方法的时候在父类的方法区去调用它们,就算子类覆写了private/static的方法也是属于子类自己方法,而final方法 更是无法覆写;
 那么可以见这些abstract方法其实根本没有被真正的实现,在向上转型时根本无法被调用,所以java编译器会禁止这样的违反多态的声明行为;
  补充内容具体可以参考SongExtendsPremium.java;
  
 */









