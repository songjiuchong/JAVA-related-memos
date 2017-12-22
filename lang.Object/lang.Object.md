lang.Object

package prototype;

public class SongJAVASElang {
	public static void main(String[] args) {
		int a =1;
		int b =2;
		A3 a1 = new A3(1);
		A3 a2 = new A3(1);
		System.out.println(a1.equals(a2));              //如果不重写equals方法，结果是false；重写完方法后结果是true.
		System.out.println(a1.hashCode());
		System.out.println(a2.hashCode());              //因为hashCode方法没有跟随equals一起重写，这里的2个输出结果是不同的整数
		                                                //在重写了hashCode方法之后返回的整数是相等的
		System.out.println(a1);                         //这里的输出结果是“prototype.A3@150bd4e”，其实就是系统默认调用toString()
		                                                //的情况1的结果！  (方法已重写！)
		System.out.println("abc123"+a1+"abc123");       //情况2的结果！       (方法已重写！)
		String x= "abc123"+a1+"abc123";                 //情况2所说的字符串连接时！
		//System.out.println(a.equals(b));              //报错
	}
}
/* 		JAVA SE类库：     （利用API来索引：Application Programming Interface）
     	JAVA SE 中比较主要的一些包：（包中有各种的类）
     	1.java.lang包 (核心包)
     	2.java.util包 (工具)
     	3.java.io包(import/output)
     	4.java.sql包(结构化查询语言：用于访问数据库)
     	5.java.net包(网络)
     	
     	Java.lang.Object (超类/根类) ： 所有对象包括数组都实现这个类的方法
     	API 应用程序接口
     	
 */

//下面是Object类中的3个重要方法：
//equals():
//equals()在应用时其实相当于==，但是不能被用来比较基本类型变量，它的优势是可以被重写：
class A3{
	int a = 1;
	public A3(int a){
		this.a=a;
	}
	public boolean equals(Object obj){          //重写equals方法，使其能比较A3类对象的基本类型属性是否相当
		if(obj==null){
			return false;
		}else if(obj instanceof A3){
			A3 a = (A3)obj;
			return this.a==a.a;
		}else{
			return false;
		}
	}
	public int hashCode(){
		int type = this.getClass().hashCode();  //取得对象类型的哈希码，确保是同一类型在比较
		return type + a ;
		//return type + name.hashCode();   如果是引用类型的都用hashCode()来取得其“身份验证”值
	}
	public String toString(){
		return "类名：A3 "+" 变量a=:"+this.a;
	}
}
 
//hashCode():
//如果根据equals方法结果是2个被比较对象相等，则这两个对象调用hasCode()必须得到相等的整数

//toString():
//返回一个对象的文本描述，默认返回：包名.类名@HashCodede1十六进制表示;
//经常会被系统默认调用：
//情况1：System.out.print()系列方法时
//情况2：字符串连接





