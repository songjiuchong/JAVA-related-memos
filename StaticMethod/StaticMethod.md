StaticMethod

package prototype;

public class SongStaticMethod{
    public static void main(String [] args){
    	new SongDemo2().fun();
    }
}
class SongDemo2{
	public static void fun(){
		new SongDemo2().fun1();             //这里如果直接调用fun1();会报错，因为静态方法无法调用非静态方法,只能通过对象去调用;
		new SongDemo3().fun3();             //调用其他类中的方法,虽然fun3()是静态方法，但是由于它在别的类中，也只能通过对象去调;
		new SongDemo3().fun2();             //两种情况都不满足，只能通过对象调用;
		System.out.println();
	}
	public void fun1(){
		
	}
}
class SongDemo3{
	public void fun2(){
		this.fun3();
		System.out.println();
	}
    public  static void fun3(){
    	System.out.println();
	}
} 
