Enum Annotation

package prototype;

import java.lang.annotation.*;
import java.util.ArrayList;


public class EnumAnnotation { //refer to public enum Color;
	
	@SuppressWarnings("unchecked") //忽略警告的标注;
	public static void main(String [] args){
		
		//test enum
		
		Color c1 =Color.RED; //不能用new Color;
		System.out.println(c1);
		Color[] colors=Color.values();
		for(Color c:colors){
			System.out.println(c);
			
		}
		
	//////////////////////////////////////////
		
	//test Anno
		
		EnumAnnotation anno = new EnumAnnotation();
		System.out.println(anno);
		
		ArrayList a1 = new ArrayList();  //由于在主方法前使用了标注：@SuppressWarnings("unchecked"),此处原本报黄的遗漏泛型警告被取消了;
		a1.add("aaa");
		
	}
	
	@Override  //典型的注释;可以起到检测下一个方法的作用,如果不符合要求编译器会报错;
	public String toString(){
		return "Anno";
	}

	@Anno(name="s",age=25)   //使用自定义标注;
	public void testanno(){
		//可以用反射操作标注;
	}
	
}


/////////////////////////////////////////////////

//自定义一个标注;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)

@interface Anno{
	String name();
	int age();
}



/*

Enum 枚举 (抽象类)：

枚举类型是JDK5.0的新特性,可以说是一种和类/接口平行的新的类型,下面就是一个典型枚举类型的定义：

public enum Color{
	RED,BLUE,BLACK,YELLO,GREEN
}

Color枚举类是特殊的Class,其枚举值(RED,BLUE...)是Color的类对象(类实例)： Color c=Color.RED;
而且这些枚举值都是public static final的,也就是常量,因此枚举类中的枚举值需要全部大写;


enum很像特殊的class,实际上enum声明定义的类型就是一个类,是一种类型的清单;
而这些类都是类库中Enum类的子类(java.lang.Enum<E>);它们继承了这个Enum中的许多有用的方法;

既然枚举类是class,当然有它的构造器/方法/属性;但是,枚举类的构造器有很大的不同：
构造器只是在构造枚举值的时候被调用,且只能为private,绝对不允许有Public构造器;这样可以保证外部代码
无法新构造枚举类的实例,这也是完全符合情理的,因为我们知道枚举值是常量;
枚举类的方法和属性可以允许外部访问;

枚举的方法：

values();  //静态方法,返回一个包含全部枚举值的数组; (在API中可能无法找到此方法);

Color[] colors = Color.values();
 for(Color c:colors){
 System.out.println(c+",");
 }
 
 toString();  //此方法被枚举重写了,返回枚举值的名称;
 
 switch语句支持使用enum;



Annotation 标注/注释：

标注,又叫注释;简单的说,就是代码里的标记,这些标记可以在类加载,运行时或是编译的时候被解释;
通过使用标注,程序开发人员可以在不改变原有逻辑的情况下,在源文件嵌入一些补充的信息;

Java常用的一些标注：

@Override  //方法的覆写

@SuppressWarnings("unchecked")  //取消警告

@Deprecated //废弃的方法;



Java标注的API--java.lang.annotation:

Java中所有的标注相关的API都在java.lang.annotation包中;

Java中所有的标注都继承java.lang.annotation.Annotation接口;

标注的类型用@interface表示;

而上述的3个常用标注是在java.lang包中预先定义了的标注类型;


Java提供的修饰标注的标注：

@Target(value={...});  //用来描述自定义注释所适用的程序元素的种类;其实就是定义了修饰的对象类型;

@Retention(value=...);  //描述注释能够保留到什么时候;

@Inherited;  //表示注释会被自动继承;

@Documented;  //表示某一类型的注释将通过javadoc和类似的默认工具进行文档化;

其中,@Target这个注释的值只能是ElementType枚举值,具体值参考API文档;

	 @Retention的值只能是RetentionPolicy枚举的值,具体参考API文档;
	 
	 
举例： API中对Override的描述：
@Target(value=METHOD)
@Rentention(value=SOURCE)

表示Override标注用来标注方法;
表示此标注是提示编译器用的,编译器会利用此标注产生编译报错提醒用户,但是不会被真正编译进类和内存;



Java支持用户自定义标注类型,用@interface表示;

标注的属性类型可以是8种基本类型/String/Enum/Class/Annotation以及它们的数组;
标注的属性类型需要用()表示;
自定义标注时,需要用java.lang.annotation包中定义的几种标注类型;
自定义标注的解析需要使用反射技术;


*/


//////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////



package prototype;

public enum Color {  //refer to EnumAnnotation;
	
	RED,BLUE,GREEN;
	
	public String toString(){
		return "enum : "+super.toString();
	}

}



