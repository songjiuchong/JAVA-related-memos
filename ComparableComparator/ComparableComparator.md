Comparable/Comparator


package prototype;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;


public class SongComparable {
	
//Comparable接口：
//用于描述其子类具有可比较性;实现该接口就必须重写比较方法,用于定义该类实例的比较规则;

//int compareTo():抽象方法必须实现;
//如果返回的int为负数：当前对象比参数对象小;
//如果返回的int为0：当前对象等于参数对象;
//如果返回的int为正数：当前对象大于参数对象;
	
//Collections集合工具类：提供了很多方便操作集合的方法;
//Collection和Collections区别：Collection是集合的父接口;Collections是集合的工具类;
	
//Collections.sort(); //允许对给定的List集合元素自然排序,依赖compareTo()方法的返回值;
//需要注意的是此方法的参数只能是List类型的;如果参数中集合里的对象未实现过Comparable接口,
//此方法也会按照列表中对象的"自然排序",也就是hashcode大小来排序;

//由于Comparable的排序时默认从小到大的,所以如果编程者想要从大到小排序,可以利用例如：
//在this.类对象大于参数中的被比较对象时返回一个小于0的值来"欺骗"Comparable的排序方法,
//让它所认定的从小到大变为编程者期望的从大到小;

//Comparator比较器：
//当我们想对集合元素进行排序,又不希望改变元素已经存在了的比较规则,
//我们可以使用Collections的重载sort方法,定义一个额外的规则进行排序;

//String也实现了Comparable接口;
	
	
//同样,Arrays.sort();方法也提供了从小到大排序数组中元素的方法,也具有第二个参数为comparator的重载方法;原理与Collections.sort()相同;


	public static void main(String [] args){
		Point p1=new Point(4,5);
		Point p2=new Point(3,2);
		if(p1.compareTo(p2)>0){
			System.out.println("p1大于p2");
		}else if(p1.compareTo(p2)<0){
			System.out.println("p1小于p2");
		}else{
			System.out.println("p1等于p2");
		}
		
		
		Point p3 = new Point(1,2);
		List<Point> l1=new ArrayList<Point>();
		l1.add(p1);
		l1.add(p2);
		l1.add(p3);
		for(Point temp:l1){
			System.out.println("x= "+temp.x+" y= "+temp.y);
		}
		//使用Point的比较规则进行自然排序;
		Collections.sort(l1);
		System.out.println(l1);
		
		/////////////////////////////////////////////////////////////////////////
		Comparator<Point> comparator=new Comparator<Point>(){
			@Override
			public int compare(Point o1, Point o2) {
				return o1.x-o2.x; //从小到大;
			}
		};
		//使用自定义的比较规则进行自然排序;
		Collections.sort(l1,comparator);
		System.out.println(l1);
		
		///////////////////////////////////////////////////////////////////////////
		//String也可以使用Collections.sort排序：
		List<String> slist=new ArrayList<String>();
		slist.add("aa");
		slist.add("b");
		slist.add("A");
		slist.add("ab");
		slist.add("abva");
		System.out.println(slist);
		Collections.sort(slist); //默认按各字符的序列化编码排序;
		System.out.println(slist);
		
		Comparator<String> comparator2=new Comparator<String>(){

			@Override
			public int compare(String o1, String o2) {
				return o2.length()-o1.length(); //按字符串字符串从多到少排序;
			}
		};
		Collections.sort(slist,comparator2); 
		System.out.println(slist);
		
	}
}


class Point implements Comparable<Point>{ //Comparable接口及其方法支持使用泛型;
	int x;
	int y;
	public Point(int x,int y){
		this.x=x;
		this.y=y;
	}
	
	@Override
	public int compareTo(Point o) { //参数由Object变为泛型内容Point;
		//计算当前调用此方法点在直角坐标系中2点间的距离的平方:
		int d = this.x*this.x+this.y*this.y;
		//计算参数给定的对象在直角坐标系中2点间的距离的平方:
		int d2= o.x*o.x+o.y*this.y;
		return d2-d; //从大到小;
	}
	//覆写toString()方法：
	public String toString(){
		String temp="x= "+this.x+" y= "+this.y;
		return temp;
	}
}
