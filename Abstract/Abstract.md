Abstract

package prototype;

public abstract class SongAbstract {          //abstract可以修饰带有main方法的public类;
	public static void main(String[] args) {
		B b = new B();
		b.print();
	}
}
abstract class A{                             //定义一个抽象类,包含抽象方法的类必须为抽象方法;抽象方法必须存在子类;
	                                          //抽象类不能直接被实例化,但是可以有构造方法,因为它的非抽象子类实例化时同样要
	                                          //递归调用父类构造方法;
	public A(){
		System.out.println("父类的构造方法");
	}
	static final int ABC = 1;
	private String name="法师1";
	public void setName(String name){
		this.name=name;
	}
	public String getName(){
		return this.name;
	}
	public abstract void print();                 //抽象方法没有实体,所以没有大括号内容;
	
}
class B extends A{                               //抽象类必须有子类,而且其非抽象子类必须覆写父类的所有抽象方法;
	public B(){
		super();
		System.out.println("子类的构造方法");
	}
 
	public void print(){
		System.out.println("子类覆写父类抽象方法、"+getName());
	}
}

abstract class D1 extends A{                     //抽象类可以继承另一个抽象类;

	public abstract void print();          		 //继承一个为抽象类的父类可以选择不去实现其抽象方法或是直接照搬过来;事实上一个普通的父类子类中,子类也可以照搬一个父类的普通方法,虽然这没有必要,因为已经继承了此方法;
	public abstract void print(int a);
}

class D2 extends D1{							 //继承了D1类的D2类必须实现A,D1类的2个抽象方法;

	@Override
	public void print() {
		
	}

	@Override
	public void print(int a) {
		
	}
	
}

//interface和abstract class关系概括：
//
//interface :
//不存在构造方法，不可以用new直接实例化，原因是：它的所有属性都是一个抽象宽泛的概念,在被实现前无法直接派生出具体的实例;并且它不能被继承只会被实现，既然不会有任何实例将被声明，初始化，构造方法就没有存在的意义了;
//
//abstract class :
//存在构造方法，但是不可以用new直接实例化,原因是：抽象类包含了抽象方法,如果直接实例化的话无法使抽象方法作为它创建的实例的具体功能；但是与interface不同，它可以被继承，所以其子类就会继承它的处抽象方法之外的属性,
//并且覆写了父类的抽象方法，很彻底地把父类的属性全都继承并实现了下来，所以它的实例理所当然的成为了其父类“生命的延续”；而要使得子类的实例能够被正确的初始化，构造方法是必须的工具，父类看似“形同虚设”的构造方法在此处起到了让子类继承其非抽象方法之外所有的属性（初始化实例），
//这就是抽象类本身不能直接实例化但是却存在构造方法的原因;
