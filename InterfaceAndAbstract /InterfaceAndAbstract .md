InterfaceAndAbstract

package prototype;

public class SongInterfaceAndAbstract {      //需要注意的是，在实际开发中，子类尽量不要继承一个完善（实现好的）的类，
	                                         //而是要去继承抽象类或实现接口;
	public static void main(String[] args) {
		Abstract1 a1 = new Normal1();         //向上转型，实例化抽象类的方法
		a1.print();          
		
		Interface1 i1 = new Normal2();        //向上转型，实例化接口的方法
		i1.print();
	}
}
////////////////////////////////////////////////////////////////////////////////////////////////
abstract class Abstract1{                    //定义抽象类
	public abstract void print();            //定义抽象方法           
}
////////////////////////////////////////////////////////////////////////////////////////////////
interface Interface1{
	void print();
}
///////////////////////////////////////////////////////////////////////////////////////////////
class Normal1 extends Abstract1{              //子类继承抽象类，为实例化抽象类做准备
	public void print(){
		System.out.println("Normal1 覆写 Abstract1方法");
	}
}
class Normal2 implements Interface1{          //子类实现接口，为实例化抽象类做准备
	public void print(){
		System.out.println("Normal2 覆写 Interface1方法");
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

