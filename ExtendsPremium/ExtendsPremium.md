ExtendsPremium


package prototype;



public class SongExtendsPremium {
		public static void main(String args[]){
			Son s = new Son();
			s.son();                     //间接访问子类与父类私有方法同名的私有方法;
			System.out.println(s.getA());//当用子类的实例化对象去调用父类的方法时，私有属性a对于子类是不可见的,所以这里return 0;
			System.out.println(s.getB());//子类能够继承父类的final属性;
			///////////////////////////////////////////////////////////////////////////
			Father f1=new Son();
			f1.print3();             //这里输出的是父类的print3();方法结果：父类static方法;因为print3()方法和类绑定了,
			                         //按照多态向上转型的原则，这里用f1调用子类覆写过的方法应该输出子类覆写后的内容,所以证明了：
			                         //子类无法从真正意义上覆写父类的静态方法！
			System.out.println(s instanceof Father); //这里输出为true;所以子类对象可以继承并调用与父类绑定的静态方法;
			s.print3();              //在子类未覆写父类静态方法之前,这里的输入为：父类static方法;证明了子类继承了父类的静态方法;
			                         //而在覆写了父类的静态方法之后,这里的输出为：重写父类static方法?功能上确实是,但是无法实现动态绑定！
			                         //证明子类实际上只是声明了一个新的属于自己的静态同名方法;
		}
	}
	class Father{
		public final int b=2;   //父类的final属性;
		private int a=1;        //父类的私有属性;
		public static int c=3;
		public int getA(){
			return this.a;     
		}
		public int getB(){
			return this.b;     
		}
		public int getC(){
			return this.c;     
		}
	    private void print(){
			System.out.println("父类私有方法");
		}
	    final void print2(){
			System.out.println("父类final方法");
		}
	    public static void print3(){
			System.out.println("父类static方法");
		}
	}
	class Son extends Father{
//		int b =3;   //这里把继承来的b变量覆盖了,如果在这条语句被正常编译的情况下getB();方法想要返回父类的b变量需改为：return super.b;
		public int getA(){
			System.out.println("重写父类方法");
			//return this.a;  //这里会报错：the field Father.a is not visible,由于Son中没有自己的a属性,所以会调用父类的,
		                      //而父类的私有属性对子类不可见,其实在实例化子类时,已经先实例化了父类,那父类的私有属性存入了
		    //子类对象的内存中,但是标记为父类的private;(不然在访问父类的私有属性时也不会报不可见错误,而是报未声明变量内存错误)
			//return super.a;  //同样无法调用;
			return 0;
		}
		 private void print(){    //编译通过;此方法属于子类自己的方法,根本就是对子类不可见又何来覆写一说?所以向上转型时调用此方法时,静态绑定了父类中的private的同名方法,所以调用的是父类自己的private方法,子类自己调用此方法时调用的是扩展print方法;
				System.out.println("声明与父类私有方法同名的私有方法");
			}
		 
//		 void print(){
//			 System.out.println("声明与父类私有方法同名的私有方法,并改变扩大其定义域"); //同上;
//		 }
		 
		public int getB(){
			     
			return this.b;     
		}
		public int getC(){
			return this.c;     
		}

//		public final void print2(){    //这里会报错：不能覆写父类的final方法;
//			System.out.println("子类重写父类final方法？");
//		}
//		public void print2(){    //就算把final定义去掉,这里仍会报错：不能覆写父类的final方法;
//			System.out.println("子类重写父类final方法？不行！");
//		}
//		void print3(){      //不能把父类静态方法覆写为非静态的,反之亦然,在方法的固有属性方面(static)必须两者保持一致,报错!
//		System.out.println("重写父类static方法？");
	//}
//		public static void print3(){             
//		/*虽然这里看似成功覆写了父类的static方法,但是它违背了面向对象的多态性,因为只有对象存在多态,
//		  static声明变量和方法都是属于类的,它是和声明它的类在编译时就绑定的,所以一个类不可能也具有多态性去调用另一个类的静态方法,
//		     所以从格式上确实是覆写了父类的静态方法,其实从意义上就是子类自己重新声明的一个属于自己类的静态方法而已;
//		*/ 
//		System.out.println("重写父类static方法?功能上确实是,但是无法实现动态绑定！");
	//}
		
		public void son(){
		    print();  //在子类重写父类私有方法之前,会报错：the method from the type Father is not visible 父类的私有方法对子类不可见;
			print2(); //子类继承了父类的final方法
			print3(); //子类继承了父类的static方法
			
		}
		
	}


	/*总结：(结合SongHideOverloadOverride.java中的补充内容);
	
	1.private继承(否):判断是否继承其实就是子类是否能够以子类自己的形式来调用父类中的这个方法;子类在调用父类的private方法或熟悉时编译器在父类信息中找到这个方法发现时private标签的,首先就无法通过编译,所以没有继承私有属性和方法;
	 
	  private覆写(否)：子类可以重新声明父类的私有方法,不过要注意的是,在编译时向上转型通过编译,用标记为父类的对象调用了一个父类的私有方法,那么编译器将在父类中查找这个方法,找到了,通过编译,并且标识了这个方法的名称及其关键定义修饰符
	  (此处为private),这就是所谓的静态绑定;那之后在程序实际运行时,此对象调用了这个方法,那JVM就将在其初始化时(也就是new语句)绑定的子类空间中寻找其父类信息的地址并到方法区直接找到同名方法,所以向上转型后调用的private方法一定
	    还是父类的方法;那就说明这个子类重新声明的private方法只能被子类自己调用了;另外,如果将private修饰符去掉再覆写父类的private方法,也是通过编译的,但此方法还是属于子类自己的,原因是这个调用方法的语句在编译时已经实现了静态绑定,
	    根本不会去方法区的子类信息中查找同名方法,而此去掉private修饰的重新声明父类的私有同名方法仍旧是只属于子类自己的;以上的情况进一步的说明了父类的private方法不会被子类继承,子类怎么"覆写"都是在写属于自己的扩展方法;
	      
	2.final继承(是)：子类继承了父类的final方法;
	  
	  final覆写(否)：子类无法覆写父类的final方法,就是把final修饰去掉也无法覆写,final本身就表明无法被改变;值得一提的是:子类可以"覆写"父类的final变量,其实是声明了一个属于自己的同名变量,想要调用父类的final变量时,用super.来调用;
	    
	3.static继承(是)：(子类对象 instanceof 父类)输出结果是true,表明了子类对象中存在父类的地址,那么子类对象可以自由调用父类的static方法;也就是说,子类确实继承了父类的static方法;
	 
	  static覆写(是,无法实现动态绑定)：参照之前private总结中提到的编译时对调用方法的静态绑定,static关键字仍旧会让编译器直接去父类的方法区查找同名方法并且绑定之后运行时直接去调用这个绑定了的父类方法;所以这种"覆写"还是在写子类自己的扩展静态方法;
	     另外,static声明的方法是唯一一个具有"隐藏"机制的,也就是说子类和父类都能够以他们的自身形式调用这个方法,但是一旦子类也声明了同名的一个static方法,对于子类来说,之前父类的static方法就被隐藏起来了,只能通过父类自己来调用了,子类的
	     这个同名静态方法就变为其自己的扩展静态方法了,类似属性的隐藏机制;需要注意的是private方法虽然在子类"覆写"后也能达到这种效果,但是由于之前子类本身就不能去调用它,所以根本谈不上隐藏了,因为隐藏的前提是之前是可见的(可以被调用的);
	    
	     最后要注意的是:
	  static是方法的一种属性,并非访问权限,这种方法的属性只能被子类继承而不能被改变,所以子类既无法以非静态形式覆写父类的静态方法,又无法以静态形式覆写父类的非静态方法,如果这样编写编译阶段就会报错,说明java禁止了这种前后不一致的行为;
	     而对父类/子类中同名的static属性的声明不存在这样的限制;
	  
	  
/////////////////////////
 
	特别注意一：
	以下程序暴露了一个比较隐晦的注意点,很容易被忽视：
	
	public class SongDemo1{
	public void print(){
		System.out.println("111");
	}
	   public static void main(String[] args) {  
		   SongDemo1 test=new demo2();  //向上转型;
		   test.print();
	}
}
class demo2 extends SongDemo1{
	int a=3;                 //子类自己的变量;
	public void print2(){    //子类自己的扩展方法;
		System.out.println("extends method");
	}
	public void print(){    //覆写父类的方法;
		System.out.println(this.a);
		this.print2();
	}
}
//输出为3,extends method;
     结论：在利用对象向上转型操作时,之所以此对象虽然在编译时绑定为父类型对象,
     能够调用子类对象中覆写过了的方法是因为动态绑定使得此对象能够在子类中找到符合修饰符标识条件并且同名的方法的情况下直接调用
     此子类中的方法(也就是子类覆写过的父类方法);而编译器只标识了某个向上转型的对象为其父类,而不会将子类的覆写方法中的this视为父类的,
     一个方法中的this就代表了方法所在类的一个对象,这里就代表了子类的一个对象,因为能调用这个方法的一定是这个类的一个对象,或者是这个类本身,也就是说这里的this被标识为属于子类的对象;
     那么在之后运行时,向上转型对象成功的调用了这个方法,方法中的this就会在编译时绑定的类(子类)中查找相关的属性和方法(a/print2()),这就是向上转型
     的对象能够通过覆写的方法直接调用其子类中的扩展内容的原因,这个方法实际上帮助这个对象实现了向下转型;


/////////////////////////////////////////////////////////////////////////////////
 
 特别注意二：
  以下程序揭露了一个比较容易忽视的问题：
 
 public class SongDemo1{

	public static void main (String arg []) {
	father s = new son();
	s.print();  
	//当子类没有覆写父类的print方法时,此处的输出a=1,结合上一个注意点就能发现,向上转型后的对象调用一个没有被子类覆写过的方法,JVM会默认认为
	是父类的对象在调用方法,那方法中所访问/修改的变量为父类的变量,也就是子类继承下来的在堆内存中(被标记为不可见)的变量,而不是子类覆写的同名
	变量(子类的扩展变量);这就解释了当子类覆写了父类的print方法后在方法中它所访问和改变的变量是其自己的(在堆内存中可见的)扩展同名变量;
	}
}
class father{
	int a;
	public void print(){
		this.a++;
		System.out.println("!!!!"+a);
	}
}
class son extends father {
	int a =3;
	public void print(){    //覆写父类的方法;
		this.a++;           //访问/改变同名变量;
		System.out.println("!!!!"+a);
	}
}
  
  
  //总结:其实结合上一个例子就不难发现,由于父类的方法中的this被绑定为父类的对象,那在这个方法被调用时this语句得到的一定是父类中的相应内容,
          同理,向上转型后调用了子类覆写的方法后this语句得到的一定是子类中的相应内容,子类覆写方法中的this被绑定为子类的一个对象了,所以上例中未覆写
         父类的方法时,向上转型后的对象调用的是父类的方法,this.a得到的是父类中的a属性的内容,覆写以后调用了子类的方法,this.a获得的就是子类中a属性对应
         的内容;
  
  ////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
   
   
       进一步理解:
       之前提到过每个方法都存在一个隐藏参数Object obj来记录调用此方法的对象,以便方法中的this关键字调用,此参数的类型默认此方法所在的类;那么,
       如果子类调用继承下来的父类方法,显然无法用this来调用子类的扩展属性(包括“覆写”的属性)和方法,但是由于子类对象初始化时会递归调用父类的构造方法来初始化属性
  (也就是在子类对象的堆内存中生成父类的属性和父类在方法区的地址以供子类对象调用),所以如果子类正常声明对象,然后就算修改对象中的非扩展属性
  (其实就是修改子类堆内存中声明好的父类信息区域中的属性)，那么在之后调用继承下来的父类方法时可以正常利用this来调用其子类的属性(其实就是继承下的父类的属性);
       如果是向上转型呢,在编译时标记为父类的对象如果调用了子类覆写过的方法,显然调用了隐藏参数类型为子类类型的方法,那么此时这个对象被传入方法参数是相当于被向下转型了,
       那么这个方法中的this对象显然可以调用子类自己的扩展内容,而不能调用只属于父类的内容(比如private参数和方法,被“覆写”了的父类参数);
   
       补充理解:
   
       假设son类extends father类;
       
   father s = new son();
   
        有一个方法:
        public void test(son s){
        	......
        }
        
        这时如果通过.test(s);调用方法,则一定会编译报错,因为JVM检测到了s不符合方法参数中定义的son s,如果向上转型是可以通过编译的;
        但是如果不是通过这种编程者的直接赋值,而是由JVM来完成赋值的情况呢,比如在进一步理解中提到的向上转型后对象调用一个子类覆写的父类方法,
        这时,就会用一个类似向下转型的动作:.test((son)s);编程者也可以通过这种方式来通过编译;
        
   /////////////////////////////////////////////////////////////////////////////////////////////////////////////////
   
        结合动态绑定之后的终极理解: (一次向上转型,一次强转(father)x,两次动态绑定);
        
   public class xxxxx  {
	public static void main(String []args){
		father x = new son();
		x.print(); //3;
	}
}

class father{

	public void print(){
		this.print2();
	}
	public void print2(){
		
		System.out.println(2);
	}
}

class son extends father{
	static int a =2;
	public void print2(){
		
		System.out.println(3);
	}
}     
        

补充测试:

public class xxxxx  {
	public static void main(String []args){
		son x = new son();
		x.print(); //12,11;
		x.print2();//12;
		x.print(); //12,12;
	}
}


class father{
	int a =12;
	static int b =11;
	public void print(){	
		System.out.println(this.a);	
		System.out.println(this.b);
	}
}

class son extends father{
	static int a =2;
	
	public void print2(){	
		this.b++;
		System.out.println(super.b);
	}
}    

*/

	
	
