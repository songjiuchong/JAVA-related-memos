Inner （内部类，局部内部类）

package prototype;


public class SongInner {
	
	
	public static void main(String[] args) {
		new Outer().fun();         //调用外部类的fun()
		new Outer.Inner().print(); //当Inner类还是非static时，虽然内部类编译后产生了Outer$Inner.class文件，相当于承认
		                           //Outer.Inner结构，但是无法直接用这种方式调用内部类
		Outer.Inner.a=2;           //static的内部类可以直接被外部类调用
//		Outer out= new Outer();          当内部类为非静态时，这3行语句能正常执行，因为想要实例化内部类，必须先实例化外部类
//		Outer.Inner in =out.new Inner();
//		in.print();
		//new Outer().new InnerMethod().print(1);   外部类的方法中的内部类，不能这样调用
		Outer innerm =new Outer();
		//InnerMethod im = new InnerMethod();    //局部内部类对外部是不可见的，只能通过外部类对象调用其外部方法来访问，类似于被封装了的类
		innerm.fun();                            //局部内部类只能通过其外部方法来调用;
////////////////////////////////////////////////////////////////////////////////////////
		OutterS.ss=OutterS.getSS(); //通过方法和其局部内部类得到一个临时实现了某个接口功能的类的对象;
		OutterS.ss.printS();  //成功调用了局部内部类中的方法并且调用了外部方法中的变量,说明此时之前创建的局部内部类对象还存在并且所访问的外部方法的变量确实创建了一个副本保存在类中,也就证明了对象的生命周期比局部内部类的外部方法要长,因为方法已经在上一行调用结束了;
		                      //也进一步的证明了只要有引用指向局部内部类的对象,它就不会被回收;
	}
}

class Outer{
	private static String s ="outside";     //定义外部类的私有属性
	int c = 1;
	static class Inner{                     //定义内部类，只有内部类可以用static来修饰，因为这样外部类可以直接以类.来调用内部类
		public void print(){
			System.out.println(s);  //内部类的作用和最大优点就是：可直接访问外部类的私有属性；当他被定义为static时，所访问的
			                      //外部变量也必须为static，其原因是static的内部类是可以被外部类直接调用的，那么通过外部类类来调用
			                      //内部类后再调用其引用来的方法或变量（访问外部类所得的）时，如果不是static的，就需要外部类的
			                    //对象去对应，所以，static内部类不能使用外部类的非static的成员变量和非static的成员方法
			                    //但是static的内部类可以声明自己的非静态方法和参数，当然这需要内部类的对象去访问;
		}
		static int a =1;
		int x =2;                    //static的内部类可以声明非静态参数
	}
	
	//内部类存在的意义是在某些情况下内部类的存在会减少代码的复杂性，比如当2个不相干的类要在各自的方法中多次调用对方的方法或（私有）变量：
	//A类中的方法调用了B类的方法，而B类的这个方法又调用了A类的私有属性...这种情况下，会需要在B类中建立A类的对象作为B类的属性以保存调用
	//了A类方法的A类对象，以便之后B类方法中可以通过此对象调用A类中的get()方法取得私有变量....具体参见JAVA Tips中的内部类：图3;
	
	public void fun(){               //定义外部类方法
		final int temp=1;
		class InnerMethod{         //定义在方法中的类称之为局部内部类,作用域相当于局部变量,public,protected,private声明自然就不能使用了;
			int c =2;
			public void print(){
				System.out.println(temp+this.c+Outer.this.c);    //内部类想要访问外部方法中的参数，必须设置参数为final
			}                                                    //想要访问外部类中的变量，则用类名.this.参数调用
		}
		
//关于局部内部类：在内部类中访问局部变量，编译器实际上会为该内部类创建一个成员变量，以及带有参数的构造方法，
//然后将该变量传入构造方法，也就是说外面的变量和类里面的变量就是名字相同而已，此时你无论修改哪一个都对另外一个不产生影响，
//这样就出现矛盾了，防止这种现象就规定只准用final;内部类在使用局部变量的时候为什么要创建一个该局部变量的拷贝呢？
//原因就是：局部变量在方法结束后生命周期就结束了，但是内部类的对象却不是，所以内部类中使用局部变量的话，就需要该变量的一份拷贝。
//但是既然是拷贝,就会出现两边值(局部变量和局部变量的拷贝)不一致的情况,所以要确保同步最直接的方法就是不对该变量做修改,这里也体现了程序设计语言的设计是受到实现技术的限制的;
//而"全局变量"的生命周期比局部变量相对要长,不会出现该问题,也就不需要拷贝,也就不需要强制为final来确保值同步了;
		
		InnerMethod m=new InnerMethod(); //通过实例化局部内部类对象调用其方法
		m.print();
		
		Inner n = new Inner();       //通过实例化内部类对象调用其方法
		n.print();                   //
	}
}

//下面是一个说明局部内部类对象的生命周期大于等于它所在方法的生命周期,同时也表现出了局部内部类存在的价值:
class OutterS{
	static InterS ss;
	public static InterS getSS(){
		final int xx=1;
		class InnerS implements InterS{
			public void printS(){
				System.out.println("i am still alive! and here is xx ! "+ xx);
			}
		}
		return new InnerS();
	}
}
interface InterS{
	void printS();
}

//结合main方法中对OutterS类的相关调用来得出结论; 


////////////////////////////////////////////
//下面来看一个实现了局部内部类的实例：
//public class Travel implements OnTheRoad{
//	   private String attractions;
//	   public Travel(String visit) {  
//		   attractions = visit;  
//		   }
//	   public String readAttractions() {
//	          return attractions;
//	    }
//	    static Travel g = new Travel("zoo");
//	    static Destination d = g.dest("Beijing");
//	    public Destination dest(String s) {  
//	        class AsiaDestination implements Destination {  
//	            private String label;  
//	            private AsiaDestination(String whereTo) {  
//	                label = whereTo;  
//	            }  
//	            public String readLabel() {  
//	                return label;  
//	            }  
//	        }  
//	        return new AsiaDestination(s);  
//	    }  
//	    public static void main(String[] args) {  
//	        System.out.println(d.readLabel());
//	        System.out.println(g.readAttractions());
//	    }  
//	}  
//	interface Destination{
//		String readLabel();
//	}
//	interface OnTheRoad{
//	  String readAttractions();
//	}

//具体参见JAVA Tips中的内部类终极总结:
/*
  Java内部类（Inner Class）,实际上类似的概念在C++里也有，那就是嵌套类(Nested Class),乍看上去内部类似乎有些多余,
  它的用处对于初学者来说可能并不是那么显著，但是随着对它的深入了解，你会发现Java的设计者在内部类身上的确是用心良苦;
  学会使用内部类,是掌握Java高级编程的一部分,它可以让你更优雅地设计你的程序结构;
  
  为什么需要内部类？
java内部类有什么好处？为什么需要内部类？

          首先举一个简单的例子，如果你想实现一个接口,但是这个接口中的一个方法和你构想的这个类中的一个方法的名称,
参数相同，你应该怎么办？这时候，你可以建一个内部类实现这个接口。由于内部类对外部类的所有内容都是可访问的,
所以这样做可以完成所有你直接实现这个接口的功能;不过你可能要质疑，更改一下方法的不就行了吗？
的确,以此作为设计内部类的理由,实在没有说服力;
真正的原因是这样的:java中的内部类和接口,抽象类,基本类加在一起，可以的解决常被C++程序员抱怨java中存在的一个问题 没有多继承;实际上，C++的多继承设计起来很复杂，而java通过内部类加上接口，可以很好的实现多继承的效果;
另外,对于需要临时使用某个接口或是类中的方法或变量,但是直接实现这个接口或者去继承一个类又没有必要,或者说逻辑上没有实际的意义,此时可以通过局部内部类或者匿名内部类来解"燃眉之急",有省下了很多继承实现的无意义,繁琐的操作,但是
从某些角度上来说这样做破坏了面向对象的概念;


补充测试:

如果一个类t实现了一个接口t1,又继承了另一个类t2,而这个接口和类中拥有同名的变量a,且t1自身没有通过声明同名变量a来隐藏起超类或超接口的变量,
那么,t1在调用a变量时会发生The field a is ambiguous的编译报错;
那么解决方法有:

(1)利用super.a和t1.a来调用;
由于接口中声明的变量都是默认public final static类型的,所以可以直接用t1.来调用,但是不能用super.来调用,而父类可以用super.来调用,就能区分开来分别调用了;

(2)利用内部类来调用;

interface t1{
	int a = 3;
}

class t2{
	int a = 4;
}

public class SongDemo1 extends t2 {

		class son implements t1{
		private int getA(){
			return a;
		}
	}
	
	public static void main(String []args){
		SongDemo1 outter  =  new SongDemo1();
		SongDemo1.son inner = outter.new son();
		System.err.println(inner.a);      //3;
		System.err.println(inner.getA()); //3;
		System.err.println(outter.a);     //4;
		} 
		
} 

(3)利用局部内部类;

public class SongDemo1 extends t2 {

	public int getA(){
		class son implements t1{
	}
		son inner= new son();
		return inner.a;
	}
	
	public static void main(String []args){
		SongDemo1 outter  =  new SongDemo1();
		System.err.println(outter.getA());  //3;
		System.err.println(outter.a);   //4;
		} 
		
} 


补充:class.this的用法;
假设有一个类T中有个一个局部内部类son,T类中有名为b的变量,getB方法中有局部变量b,局部内部类中有变量b,怎么分别访问这些变量;

public class T {
	int b = 2;
	public void getB(){
		final int b = 3;
		class son{
		int b = 5;
		private void getB(int b){
			System.err.println(b);
		}
	}
		son son = new son();
		son.getB(b); //局部内部类访问外部方法中的变量b;
		System.err.println(son.b); //局部内部类自身的变量b;
		System.err.println(T.this.b); //在局部内部类中访问外部类的变量b;
		//注意:outterClass.this.不仅可以用在局部内部类中,其在内部类中的效果是一样的;
	}
	public static void main(String []args){
		T t = new T();
		System.err.println(t.b); //外部类自身的变量b;
		t.getB();
		} 
} 


 */

