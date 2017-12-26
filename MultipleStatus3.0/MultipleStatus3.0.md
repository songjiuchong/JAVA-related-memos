MultipleStatus3.0

package prototype;

class Pet{
	int age;
	String name;
	public Pet(){};
	public Pet(int age,String name){
		this.age=age;
		this.name=name;
	}
	public void print(){
		System.out.println("父类方法");
	}
	public static void prints(){
		System.out.println("父类静态方法");
	}
	
}
public class SongMultipleStatus                   //一下内容为了方便理解，请把“SongMultipleStatus”看做“Dog”
extends Pet{
	int no;
	public SongMultipleStatus(int no,int age,String name){
		super(age,name);
		this.no=no;
		System.out.println("no"+this.no);         //输出“no”为了证明在实例化对象p时，分配了no这个扩张属性的内存，因为实例化是在
		                                          //程序运行的时候执行的，他读取的是Dog的构造方法。
	}
	public void print(){
		System.out.println("子类方法");           //重写父类方法
	}
	public void print(int a){
		System.out.println("子类方法");           //重载继承下来的父类方法相当于是子类自己的扩展方法
	}
	public static int print(int a ,int b){        //重载一个方法，方法的返回类型、修饰符可以相同，也可不同
		System.out.println("子类方法"); 
		return a;
	}
	public static void prints(){                 //重写父类的静态方法，虽然编译通过，但是其实是子类自己的新方法，因为静态方法的
		                                         //重写无法展现方法的多态性，只有通过实例对象才能表现多态性。
		System.out.println("子类覆写父类静态方法");
	}
	public static void main(String[] args) {
	SongMultipleStatus ss = new SongMultipleStatus(1,10,"lucky");
	Pet p = new SongMultipleStatus(1,10,"lucky"); //向上转型！此时，Pet（父类）的引用变量指向了Dog(子类)的对象，这个对象虽然属于Dog类，
    //这其实属于引用类型的类型转换，                                          //但是他的实际功能已经完全变为了Pet类的对象，所以不能访问Dog类扩展的属性及
	//小类型转换到大类型                                                                        //方法，只能调用父类的。 
	//而大类型转小类型的强制引用类型转换实际上就是重新定义一个子类的引用变量来指向子类的实例，从而达到调用子类扩展方法等的目的。
	                                              //这就是对象的多态，他触发的前提必须是有继承的产生，而之前方法的重载与重写
	                                              //属于方法的多态。
	
	                                              //编译器会将引用变量p和之后的对象都认为是父类的，所以只会编译所有父类的内容，
	                                              //而在虚拟机开始运行程序的时候，会调用右方的构造准备实例化对象，这是就会按照
	                                              //Dog类，子类来进行实例化，这就是“动态绑定”。
	
	//p.no=2;                                     //这里的报错是因为，编译无法通过，p在之前的编译时被看做Pet类变量，而Pet类里无
	                                              //no这个属性
	
	ss.print(1,2);                                //子类调用重写后又重载后的父类方法，只能子类自己调用
    //p.print(1);                                 //试图调用子类重载过的父类方法，结果报错，所以重载过的方法属于子类自己的扩展方法
	p.print();                                    //这里输出的是子类方法，因为在执行程序时，实例化子类对象，绑定了子类的
                                                  //代码区中的重写过的方法，但是如果用静态方法重写，那么因为对象无法调用静态
                                                  //方法，所以此时会调用父类方法
    p.prints();                                   //这里调用的仍旧是父类的静态方法
    
    if(p instanceof SongMultipleStatus){          //使用intanceof代码来判断引用变量是否指向了对象所属的类，防止强制转换出错。
    	
    SongMultipleStatus s= (SongMultipleStatus)p;  //向下转型！，强制转换引用类型，生成一个新的子类引用参数，指向同一块内存
    s.prints();                                   //向下转型后，子类调用自己的静态方法
    }else{
    	System.out.println("error");
    }
    
	}
	
}
class TestPet{
	public void print(Pet p){
		p.print();
	}
	{
		TestPet t =new TestPet();
		SongMultipleStatus s=new SongMultipleStatus(1,10,"lucky");
		t.print(s);                              //这里就属于传递参数时的多态
	}
}
