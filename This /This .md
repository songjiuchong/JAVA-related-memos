This

package prototype;

public class SongThis {
	String name = "";
	int age ;
	public SongThis(){
		System.out.println("用this()调用构造方法");
		
	}
	public SongThis(String name){
		this();  //用this调用构造方法必须放在首行，且要满足至少有一个构造方法中没有用this()这调语句，不然会出现递归调用，无限循环;
		this.name=name;  //this.name这里表示去找到类中的属性name,也可以说是调用这个构造方法的那个对象的属性;
	}
	public SongThis(String name, int age){
		this(name);
	//	name=name;  就近原则使得参数name只赋值给方法中的变量name，而不会涉及到类中的属性name;
	//	age=age;    age同理;
    	this.age=age;  
	}
    public boolean compare(SongThis per){   /*调用此方法是存在2个对象，一个是调用此方法的，也就是this,为per1还有一个是待比较对象，也就是参数中传入的那个对象per2;*/
    	SongThis p1 = this;
    	SongThis p2 = per;
    	if(p1==p2){
    		System.out.println("这2个对象完全相同");   //因为==比较的是的对象的地址，所以地址相同则内存为同一个;
    		return true;
    	}else if(p1.name.equals(p2.name)&&p1.age==p2.age){
    		System.out.println("这2个对象相同");
    		return true;
    	}else{
    		System.out.println("这2个对象不相同");
    		return false;
    	}
    }
	///////////////////////////////////////////////
	public static void main(String[] args) {
		SongThis p1=new SongThis("玩家1",26);
		SongThis p2=new SongThis("玩家1",26);
		p1.compare(p2);                                //用上面创建的方法比较对象;
	////////////////////////////////////////////
		new Son2();  //这里的输出是b=0; 原因分析：由于在执行到new Son2();语句时，JVM首先会Son2与其父类是否已经加载，然后根据方法区的类信息为这个临时对象分配堆空间并且初始化了其变量”a=0;b=0;“;接下去是执行Son2的构造方法来初始化对象，Son2的构造调用了父类的无参构造(如果父类定义了一个构造而没有再去定义无参构造，则子类根本无法通过编译，因为它将不能调用父类的无参构造，无法初始化),所以先会执行父类的无参构造：首先初始化令a=1;然后执行this.f();注意：这里的this.f();指的是调用这个构造方法的对象.f();由于是子类对象调用的构造方法，所以这里的f();会调用子类的f();方法输出b的值，由于此时子类还未执行自己的对象初始化工作，所以输出b=0;之后父类的构造方法执行完毕，再初始化子类对象，b=2;对象的初始化工作就此结束;
	////////////////////////////////////////////
		Demo1 d=new Demo1();
		d.f(2); //3;
	}
}
class Father2{
	int a=1;
	public Father2(){
		/* 初始化: a=1;*/
		this.f();
	}
	public void f()
{
		System.out.println("a= "+this.a);
		}
}
class Son2 extends Father2{
	int b=2;
	public Son2(){
		super();
		/* 初始化: b=2;*/
	}
	public void f()
{       
		System.out.println("b= "+this.b);
}
}
///////////////////////////////////////////////////
class Demo1{
	int a=1;
	public void f(/*Demo1 this,*/int a){ //”this“是方法的第一个隐含参数，在方法”被对象调用期间“方法隐含传递给此方法的this参数(也就是调用方法的对象),所以在方法运行期间调用方法的对象与this参数绑定！;
		System.out.println(this.a+a);
		//this=new Demo1();  //编译报错:The left-hand side of an assignment must be a variable;虽然this是相当于一个引用类型的参数被传递到方法中,但是它并没有被视为一个引用变量,而是那个调用方法的对象本身,所以不能被放在等号左边赋值,这也保护了调用方法对象的引用地址,维护了面向对象原则;
	}
}

