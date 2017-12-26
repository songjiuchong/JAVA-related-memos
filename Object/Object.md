Object


package prototype;

public class SongObject {
	public static void main(String[] args) {
		int s = 3;
		Normal n = new Normal();
		System.out.println(n);                       //在覆写toString()方法之前，打印结为果：prototype.Normal@150bd4d
		System.out.println(n.toString());            //在覆写toString()方法之前，打印结为果同上，说明打印时会默认调用toString()
		Normal n2 = new Normal("spider man",26);      
		System.out.println(""+n2+s);                 //在覆写toString()方法之后，打印结果为“年龄26姓名spider man”，return的内容
		                                             //说明在打印对象名时，会自动调用.toString()方法
		///////////////////////////////////////////////////////////////////////////////////////////////////////////////
		String n5 = "x-man";
		Normal n3 = new Normal("x-man",30);    
		Normal n4 = new Normal("spider man",26);  
		System.out.println(n2.equals(n3)?"相同":"不同！"); //利用三目运算输出比较结果：n对象是否和n2对象相同
		System.out.println(n2.equals(n4)?"相同":"不同！"); 
		System.out.println(n3.name.equals(n5)?"相同":"不同！");  //相同！
		/////////////////////////////////////////////////////////////////////////////////////////////////////////////
		Normal33 n6 = new Normal3();                  //利用向上转型实例化接口Normal33
		Normal333 n9=n6;
		Object obj = n6;                              //继续向上转型，接口向Object类转
		Normal33 nx = (Normal33) n6;                  //向下转型
		Normal3 n7 = (Normal3) n6;                    //向下转型
		//obj.print();                                //对于Object类来说,它之中并没有print()方法,所以这里的print()相当于Normal3子类的扩张类无法被调用;
		n7.print();
		nx.print();
		n9.print();
	}                                              
}
class Normal extends Object{                         //所有的类都是Object类的子类，相当于每一个类都extends Object
	                                                 //Object类可以接受一切引用类型的参数，包括接口和数组！
	String name;              //为了调用方便，使用default声明;
	private int age;
	public Normal(){
	}
	public Normal(String name,int age){
		this.name= name;
		this.age=age;
	}
	public String toString(){                            //为了试验toString()的调用机制，覆写了Object类的toString()方法
		return "年龄"+this.age+"姓名"+this.name;
	}
	public boolean equals(Object obj){                   //覆写了.equals()方法，这个方法其实就是比较两个对象的地址是否相同
		                                                 //也包括String的匿名对象,如："匿名1".equals("匿名1");结果返回true,
		                                                 //虽然String的匿名对象内容放在栈中，其比较的是栈中地址。
		if(this==obj){
			return true;
		}
		if(!(obj instanceof Normal)){
			return false;
		}
		Normal temp = (Normal) obj;
		if(this.name.equals(temp.name)&&this.age==temp.age){  //实际调用.equals()方法，不会实现比较不同地址中对象的属性是否
			                                                  //完全相同,但是如果String的匿名对象内容和new String()中内容
			                                                  //相同，也会return true;
			return true;
		}else{
			return false;
		}
	}
	}
interface Normal33 extends Normal333{                                         //这里编写一个接口和一个实现接口的类是为了测试接口是否也可以
                                                           //向上转型为：Object类,虽然它无法向上转型为其他任何的非接口类，
                                                           //因为接口不能继承任何非接口类
//void print();
}
interface Normal333{
void print();
}

class Normal3 implements Normal33{
		public void print(){
			System.out.println("覆写接口的方法！");
		}
	}
