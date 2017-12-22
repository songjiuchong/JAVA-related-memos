Instanceof

package prototype;

public class SongInstanceof {
	public static void fun(AA a){                //为了方便向上转型操作设置的方法
		if(a instanceof AA1){                    //利用instanceof这个判断语句，返回boolean值，来执行if语句，并利
			                                     //用向下转型从而达到调用子类各自扩展方法的目的。
		AA1 a1 = (AA1)a;
		a1.funAA1();
		}else if(a instanceof AA2){
			AA2 a2 = (AA2)a;
			a2.funAA2();
		}
		
	}
	public static void main(String[] args) {
		fun(new AA1());
		fun(new AA2());
	}
} 
class AA{                                 //定义一个父类
	public void funAA(){
		System.out.println("AA的方法");
	}
}
class AA1 extends AA{                      //定义第一个子类
	public void funA(){
		System.out.println("AA1覆写AA的方法");
	}
	public void funAA1(){
		System.out.println("AA1扩展的方法");
	}
}
class AA2 extends AA{                      //定义第二个子类
	public void funAA(){
	System.out.println("AA2覆写AA的方法");
	}
	public void funAA2(){
	System.out.println("AA2扩展的方法");
	}
}
