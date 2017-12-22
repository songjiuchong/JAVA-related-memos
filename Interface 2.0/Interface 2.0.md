Interface 2.0

package prototype;

public class SongInterface {
	public static void main(String[] args) {
		X x = new X();
		System.out.println(x.NAME);
		System.out.println(A1.NAME);
		System.out.println(X.NAME);   //以上三种方法都正常输出了"圣骑士";
		x.print();
		x.getInfo();
		x.choose();
		x.rollNeed();
		
		B1 b1 = new B1(){
			public void choose(){
					System.out.println("实现了一个接口的匿名内部类的临时实例,而不是这个接口的实例");
				}
			};
		b1.choose();
		//////////////////////////////////////////
		B1.b2.choose(); //有趣的是:interface B1是接口不能直接实例化对象,但是通过在其内部创建一个匿名内部类实现它的抽象方法后,相当于把一个临时,匿名且实现了其抽象方法的类的对象保存为这个接口的一个属性;在主方法中也成功的通过此接口调用了其方法;
        //B1.b2=null;   这里报错说：b2已经被定义为final了,不能改变;     
	}

}
interface A1 {                                    //定义接口A1（接口是一个特殊的类），其中的元素必须只包含静态（全局）常量和抽象方法
	public static final String NAME = "圣骑士";  //静态（全局）常量:如果接口A中有一个public访问权限的静态变量a(首先排除了非静态的可能，
	                                            //因为接口不能实例化对象也不能被继承);按照java的语义， 我们可以不通过实现接口的对象来访问变量a，
	                                            //通过A.a = xxx;就可以改变接口中的变量a的值了。正如抽象类中是可以这样做的，那么实现接口A的所有对象也都会自动拥有这一改变后的a的值了，
	                                            //也就是说一个地方改变了a，所有这些对象中a的值也都跟着变了。这和抽象类有什么区别呢，怎么体现接口更高的抽象级别呢，怎么体现接口提供的统一的协议呢，
	                                            //那还要接口这种抽象来做什么呢？所以接口中不能出现变量，如果有变量，就和接口提供的统一的抽象这种思想是抵触的。所以接口中的属性必然是常量，只能读不能改，
	                                            //这样才能为实现接口的对象提供一个统一的属性;另外需要注意的是：实现了此接口的类实际上已经继承了此接口的静态常量属性，这一点在此程序的开头，主方法中已经证明了;
	                                             
	
	public abstract void print();                 //抽象方法

	String getInfo();                             //简化成默认定义，因为接口的元素的类型单一，所以其定义具有唯一性，即：
	String LVL="16";                              //常量一定是public static final;而方法也一定是 public abstract所以可以省略
	
}
interface B1{
	void choose();
	B1 b2 = new B1(){  //有趣的是:interface B1是接口不能直接实例化对象,但是通过在其内部创建一个匿名内部类实现它的抽象方法后,相当于把一个临时,匿名且实现了其抽象方法的类的对象保存为这个接口的一个属性;在主方法中也成功的通过此接口直接调用了其方法;等于是为编程者提供了一个临时快捷的调用其方法的途径,从一定程度上破坏了面向对象的特性;
		               //还需注意的是：这里的接口属性b2其实已经被声明为public static final了,这也就是为什么主方法中可以直接通过接口名调用的原因; 另外,抽象类也可以通过此方法来声明一个类似的属性,不过注意抽象类不会默认为其属性定义public static final;
		public void choose(){
				System.out.println("实现了一个接口的匿名内部类的临时实例,而不是这个接口的实例");
			}
		};
}
abstract class C{                                   //定义抽象类C
	public abstract void rollNeed();
	public void print(){                           //这里父类C“帮助”子类覆写了一个接口的抽象方法
		System.out.println("子类的print方法");     
	}
}

interface E extends A1,B1{                      //接口也可以继承其他接口(而且是多继承),但是不能继承其他类
	public void print3();
	
}

class Y implements E{                          //同样，其非抽象子类必须覆写所有抽象方法
	public void print3(){
		System.out.println("子类的print3方法"); 
}
	public void print(){
		System.out.println("子类的print方法");
	}
	public String getInfo(){
		System.out.println("子类的getInfo方法");
		return NAME;
	}
	public void choose(){
		System.out.println("子类的choose方法");
	}
	public void rollNeed(){
		System.out.println("子类的rollNeed方法");
	}
}

class X extends C implements A1,B1{       //接口也和抽象类一样必须由子类去实现其功能，而不同的是，一个接口的子类可以实现多个接口，
	                                      //同时还可以继承其他的父类，但是其非抽象子类必须覆写父类们的所有抽象方法(或者由其父类承担)
	                                      //一部分的覆写操作，但是其覆写的覆盖面之和必须为全部！
//	public void print(){
//		System.out.println("子类的print方法"); 如果子类继承了父类的某个方法,而此方法满足子类所实现的某个抽象方法的覆写条件，那此时子类就不用在类中去实现这个抽象方法了，继承下来的父类方法已经帮助子类实现了;
//		
//	}
	public String getInfo(){
		System.out.println("子类的getInfo方法");
		return NAME;
	}
	public void choose(){                 //覆写此方法时不能忘加public,虽然在接口中省略了此声明;
		System.out.println("子类的choose方法");
	}
	public void rollNeed(){
		System.out.println("子类的rollNeed方法");
	}
}

//interface和abstract class关系概括：
//
//interface :
//不存在构造方法，不可以用new直接实例化，原因是：它的所有属性都是一个抽象宽泛的概念,在被实现前无法直接派生出具体的实例;并且它不能被继承只会被实现，既然不会有任何实例将被声明，初始化，构造方法就没有存在的意义了;
//
//abstract class :
//存在构造方法，但是不可以用new直接实例化，原因是：抽象类包含了抽象方法，如果直接实例化的话无法使抽象方法作为它创建的实例的具体功能；但是与interface不同，它可以被继承，所以其子类就会继承它的处抽象方法之外的属性,
//并且覆写了父类的抽象方法，很彻底地把父类的属性全都继承并实现了下来，所以它的实例理所当然的成为了其父类“生命的延续”；而要使得子类的实例能够被正确的初始化，构造方法是必须的工具，父类看似“形同虚设”的构造方法在此处起到了让子类继承其非抽象方法之外所有的属性（初始化实例），
//这就是抽象类本身不能直接实例化但是却存在构造方法的原因;
