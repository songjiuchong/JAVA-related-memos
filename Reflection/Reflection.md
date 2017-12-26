Reflection

package prototype;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class SongReflection {

	
	public static void main(String[] args) throws Exception {
		
		//此处传出的类型参数不同,就可以获得不同的对象类型,这就是动态编程;
		Class c1=Class.forName("java.lang.String");  
		
		System.out.println(c1.getClass()); //打印对象类型: class java.lang.Class;  由于getClass()方法是Object方法,返回一个对象的类型,而c1本身是一个类型对象,那它的类型就是Class;
	
		System.out.println(c1);            //打印对象类型: class java.lang.String; 由于Class类重写过toString()方法,这里c1就是String类型对象(不是String的一个实例对象,是String类型的对象),所以打印String类;
		
		Class c2=Personx.class; 
		Object o = c2.newInstance();  //这里返回一个Object对象是由于c2在编译时与Class类绑定(编译时还未发生等号左右两边的赋值与传递,只会检查等号两边的类型是否符合),所以这里获得一个类型的对象实例就必须用Object对象来接收,
		                              //如果用Personx来接收会报编译错误,因为编译器此时并不知道c2与Personx类的联系;
		
		Personx p=(Personx)c2.newInstance();    //利用类型的强转告知编译器c2属于Personx类,所以这里可以用Personx来接收而不会报编译错误;
		
		Personx p2=Personx.class.newInstance(); //创建此 Class 对象所表示的类的一个新实例; 与之前c2调用newInstance()方法不同的是,这里没有用已和Class类绑定的变量来调用newInstance()方法,而是直接用Personx的类型来调用,
												//于是等号右边声明了一个Personx类的实例,所以等号左边用Personx类对象来接收就会被编译器通过;
		
		Method mx = Personx.class.getMethod("setName",String.class);
		System.out.println(p2); //Person: name = null,age = 0;
		
		Class c3 = p.getClass();
		System.out.println(c3); //class prototype.Personx;
		
		System.out.println(c3==c2);  //true; 由于同一个类只会在程序运行时加载一次,所以同一个类型对象拥有同一个地址;
		
		/////////////////////////////////////////////////////////////////
		
		Class c=Personx.class;
		Field f=c.getDeclaredField("name");  //取得属性;//private java.lang.String prototype.Personx.name;
		System.out.println(f); 
		
		Constructor con = c.getConstructor();  //取得无参构造;//public prototype.Personx();
		System.out.println(con); 
		
		Method m = c.getMethod("setAge", int.class); //取得setAge()方法;public void prototype.Personx.setAge(int);
		System.out.println(m); 
		
		/////////////////////////////////////////////////////////////
		
		Class c4=Class.forName("prototype.Personx");
		Object obj=c4.newInstance(); //调用了Personx的无参构造;
		
		Field f1=c4.getDeclaredField("age");
		System.out.println(f1.getType());  //打印属性类型;//int;
		
		Method m2=c4.getMethod("equals", Object.class);
		Object flag=m2.invoke(obj, "str"); //触发equals()方法,相当于用一个Personx对象.equals("str");
		System.out.println(flag); //返回值为：false;
		
		Constructor con2=c4.getConstructor(int.class,String.class);  
		Object p3=con2.newInstance(25,"song");  //调用Personx的有参构造;
		System.out.println(p3); //Person: name = song,age = 25;
		
	}

}

class Personx{
	private int age;
	private String name;
	
	public Personx(){
		super();
		System.out.println("Person()");
	}
	public Personx(int age,String name){
		super();
		this.setAge(age);
		this.setName(name);
		System.out.println("Person(int age,String name)");
	}
	public void setAge(int age) {
		System.out.println("setAge is running");
		this.age = age;
	}
	public int getAge() {
		return age;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getName() {
		return name;
	}
	public String toString(){
		return "Person: name = "+name+",age = "+age;
	}
	
}


/*

Java反射的作用：

主要用于动态编程,可以动态的生成一个类的对象或者是动态的调用一些方法,不同于传统的先确定类才能创建对象,先明确需要调用方法的对象才能去选择调用哪个方法;

反射的思想不同用于主流的面向对象思想,在创建对象时并不知道会创建谁的对象,调用方法时也不确定会调用哪个方法;

JDK1.4开始改造了反射机制,提升了反射的效率;

现在主流的一些框架的底层都大量采用了反射;比如：Struts/Hibernate;


反射的机制：Class类

Java的变量都需要先声明类型,而类型也是对象,代表类型的对象是Class对象;

java.lang.Class对应java的类型,java中所有的类型(包括void)都有对应的Class对象;

Class对象是在类加载时生成的,每种类型都加载自己对应的Class对象,因此每种类型的Class
对象在内存中只创建一次,也只有唯一的一个地址;


java.lang.Class是反射的入口类,所有对象都有对应的Class,获得方式：

1.调用静态的Class.forName(String name); //返回一个对应的Class对象; 参数为：包名.类名;  注意：此方法不适用于基本类型;

2.在已有对象的前提下,调用对象的getClass()可以获得一个类对象;

3.简单类的封装类都有TYPE属性,比如：Integert.TYPE代表int的类对象;

4.所有的类型利用：类型.class可以得到该类型的Class对象,如：int.class;


Class中的重要方法：

由于Class对象在类加载时创建,所有没有构造方法;

Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes); //返回一个 Constructor对象,该对象反映此 Class 对象所表示的类或接口的指定构造方法;参数为类型名称和指定构造方法中的参数类型;

Constructor<T> getConstructor(Class<?>... parameterTypes);
        
          
Method getDeclaredMethod(String name, Class<?>... parameterTypes); //返回一个 Method 对象,该对象反映此 Class对象所表示的类或接口的指定已声明方法 ;参数为方法名和指定方法的参数类型;
    
Method getMethod(String name, Class<?>... parameterTypes);


Field getDeclaredField(String name); //返回一个 Field 对象,该对象反映此 Class 对象所表示的类或接口的指定已声明字段(属性); 
             
Field getField(String name); 


newInstance();  //调用类对象的newInstance(),可以获得一个类对象的新实例;
          

Class有一系列的get方法可以得到对应类型的属性/方法/构造,有Declared的方法支持私有内容,不支持继承内容;没有Declared的方法支持继承但只能取得public声明的内容;

一般在调用类型属性的时候用declared的方法,调用方法和构造是用不加declared的方法;


反射机制的一些相关类：
java.lang.reflect.*;

Method类;

invoke(Object obj, Object... args);  //对带有指定参数的指定对象调用由此 Method对象表示的底层方法,返回一个Object对象,是方法的返回值;
          
          
Field类;

Object get(Object obj);  //返回指定对象上此 Field 表示的字段的值 ;

          
Constructor类;  //注意：Class类的newInstance()方法只能通过无参构造声明一个新的实例;

newInstance(Object... initargs); //Constructor类的newInstance支持有参构造,需要把参数传入,和Class类的newInstance()方法一样,
如果利用向上转型后的Class对象调用getConstructor()方法,此处的newInstance()得到的是一个目标类的有参初始化实例,但是返回的仍旧是一个Object对象,体现了反射的动态编程;


Array类 (java.lang.reflect.Array);

boolean bArray = obj.getClass().isArray();  //判断传入的对象是不是数组;

int length = java.lang.reflect.Array.getLength(obj);  //拿到数组的长度; 如果拿到的obj对象不是数组对象会报错,java.lang.IllegalArgumentException: Argument is not an array;

Array.get(obj, i);  //返回一个指定数组对象中的指定位置元素; 同样要保证obj为数组对象;

Class elementType = obj.getClass().getComponentType();  //判断数组元素的类型;

Object array = Array.newInstance(componentType, length);  //如果是运行时才知道数组类型,用此方法可以实例化数组一个指定数组对象;

Array.set(array, index, value);  //设置数组元素;




反射动态编程的体现(类似于多态的体现)：

Class c=Class.forName("java.lang.String");  //特别注意这里在等号右边声明的并不是String类的对象,而是String类型本身,所以等号左边只能用Class类接收,只有Class类可以代表一个类型;

Object o = c.newInstance();  //此处其实已经体现出了动态编程,不需要一个特定的类对象来接收这个实例,而是统一用Object来接收,增强代码的通用性,维护性;

Constructor con=c.getConstructor(int.class,String.class);  

Object o2=con.newInstance(25,"song");   //仍旧返回一个装着目标类对象的Object对象;

c.get...;  //方便之后对此目标类对象的各种操作;  //注意：此处不能用o2去调用get...方法,因为这些方法是属于Class类的,Object类中没有这些方法;

Method m = c.getMethod("setAge", int.class); //包括这里可以通过传入参数获得不同的方法也是动态编程的体现;

......


*/



