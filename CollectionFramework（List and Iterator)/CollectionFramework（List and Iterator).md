CollectionFramework：(list and iterator)

package prototype;

import java.util.*;

public class SongCollectionFramework {    //java集合框架：集合是一种数据结构，它实现了快速而高效的内存利用，能自动扩容，方便遍历。
	public static void main(String[] args) {  //(list) and refer to SongCollectionFramework1.java (hash set map)
		List<String> list = new ArrayList<String>();
		list.add("one");
		list.add("two");
		list.add("three");
		list.add("four");
		list.add("one");
		list.add("one");
		System.out.println(list.size());
		//list.remove("one");  删除第一个“one”元素
		for(int i =0;i<list.size();i++){      //试图通过逐个筛选删除所有“one”元素，但是注意，元素下标在每一次删除元素后都会上移
			                                  //所以，当两个元素相连时，删除完前一个元素，后一个元素的下标会变为前一个元素之前的
			                                  //下标，导致i++后，直接跳过了对后一个连续元素的检查
			if("one".equals(list.get(i))) {   //常量.equals(变量)这样的写法可以有效的规避空指针异常，因为null.equals会报错
				list.remove(i);
				i--;            //加上这个语句之后，能保证删除后，再原地点检查一遍元素，也就是检查被删除元素的下一个元素，避免遗漏
			}
		}		
		for(int i =0;i<list.size();i++){
			Object obj = list.get(i);
			System.out.println(obj);
		}
		list.add(1,"x");     //利用add(index,element);增加元素会把下标位置已存在的元素往后"挤";
		//list.add(5,"xx");  这里会报错：IndexOutOfBoundsException,由于list此时一共只有4个元素,这里参数中的5代表放在加在第6个元素的位置上,相当于跳过了一个不存在的元素下标,所以报错;
		//list.set(4,"xx");  这里会报错：IndexOutOfBoundsException,因为set();实际上就是替换元素的方法,不能替换本来就不存在元素的位置;
		System.out.println(list.size()); //整体的list大小增加1;
		Object[] obj2=list.toArray();      //把数组集合转换为元素数组，使其能按照数组的方式输出内容
		for(int i =0;i<obj2.length;i++){
			Object obj = obj2[i];
			System.out.println(obj);
	}
		System.out.println("*****************************************");//////////////////////////
	/*
	集合属性：   注：集合中的所有元素必须是引用类型的，如果放入基本类型，会自动装箱
	isEmpty() 集合中是否有元素，返回true或false
	size()    集合中元素的个数，可以代替第一个属性
	
	集合元素操作：              注：           index从0开始
	添加： add(element),add(index,element);
	删除： clear(),remove(index),remove(element);删除查询到的第一个元素
	修改： set(index,element);
	查询： contains(element),get(index),indexOf(elements),lastIndexOf(element);
	
	*/
		

//所有集合包括数组都提供了Iterator接口的实现（映射--Map）除外。它可以遍历有序或无序的集合。
//在迭代期间不能使用更新方法：add remove set等，否则会引发异常，但可以利用迭代器的删除方法：iterator(这里的iterator代表你设置的迭代器对象,如同下式中的it).remove();
//在java 5.0后迭代器被称为for each

	List list2 = new ArrayList();
	list2.add("two");
	list2.add("one");
	list2.add("three");
	list2.add("four");
	
	Iterator it=list2.iterator();    //调用方法返回一个list2的迭代器
	while(it.hasNext()){             //hasNext();方法的指针位置不会改变，只是侦测下一个元素是否存在，返回boolean值
		Object obj3 = it.next();     //it.next();方法会改变指针位置并把其位置上的元素返回
		if("one".equals(obj3)){
		//list2.remove("one");         这里运行会报错
		it.remove();                 //删除当前指向的元素,他的动作是这样的：删除当前指向的元素，在删除的位置上不会留下空的元素空间，也不会直接指向下一个存在的元素，但是其实下一个元素的下标已经自动往前移了一位，这样就保持了数组下标的统一性
		//it.remove();               //注意：如果执行.remove();两次或者在执行前未用.next();输出一个iterator，就会报错 java.lang.IllegalStateException;原因是.remove()是删除迭代器返回的最后一个元素，如果没有返回元素则报错
		for(int i =0;i<list2.size();i++){
			Object obj = list2.get(i);
			System.out.println("输出删除元素后list内容： "+obj);
		}
		}else{
		System.out.println(obj3); //利用it.hasNext+it.next输出list2中所有元素，相当于List的get()方法，但是显然这里的通用范围更大,可以在list长度未知的情况下输出全部元素;可以避免在用get()+remove()方法在历遍期间删除元素导致下标变化带来的历遍错误(前面已经解释过了);
	}
	}
	//System.out.println(it.next()); 这里会报错：NoSuchElementException;因为没有下一个元素了;
	
	System.out.println("*****************************************");
	while(it.hasNext()){             //这里的语句没有执行，因为迭代器的指针只能前进不能后退，上一个循环中指针已经在“four”上了。
		                             //只能用一个新的迭代器
		Object obj3 = it.next();
		System.out.println(obj3); 
	}
	System.out.println("*****************************************");
	//while(list2.iterator().hasNext()){      这里输出的是死循环语句，因为每一次循环都会生成一个新的迭代器，条件永远成立
		//System.out.println(list2.iterator().next());   
	//}
	System.out.println("*****************************************");
	Iterator it2 = list2.iterator();
	//while(it2.hasNext()){  
	//System.out.println(list2.iterator().next());   
//}                这里输出的仍旧无限输出语句，因为hasNext()并不会移动指针，而输出语句里的.next()移动的不是it2迭代器
	
	
	//从5.0之后的版本，支持for each写法（它的底层仍然是迭代器）：
	for(Object obj:list2){         
		System.out.println(obj);
	}
	
	
	String[]str=new String[2];     //for each 也支持数组！
	str[0]="one";
	str[1]="two";
	for(String s:str){           
		System.out.println(s);
		
	}
	
/////////////////////////////////////////////////////////////////////////////////
	

//subList()的用法：
	
//List的subList方法用于获取子List;
//需要注意的是,subList获取的List与原List占有相同的存储空间(地址),属于引用传递,所以对子List的操作会影响原List;
	
	List<Integer> list3=new ArrayList<Integer>();
	list3.add(1);
	list3.add(2);
	list3.add(3);
	list3.add(4);
	list3.add(5);
	
	List<Integer> subList=list3.subList(1, 4); //这里的参数为截取母List相应序号下的元素(前闭后开);
	for(Integer a:subList){
		System.out.print(a); //234;
	}
	
	
	System.out.println();
	
//由于其子List的引用传递性,可以利用list.subList(i,j).clear();来清除list中的某一段数据;
	list3.subList(2, 3).clear();
	
	for(Integer a:list3){
		System.out.print(a); //1245;
	}
	
	System.out.println();
	
	
	
///////////////////////////////////////////////////
	
	
//Collection/Map 对toString()方法的覆写效果:
	
	List l1 = new ArrayList();
	l1.add(1);
	l1.add("1");
	System.out.println(l1);  //[1, 1];
	
	Set s1 = new HashSet();
	s1.add(1);
	s1.add("1");
	System.out.println(s1);  //[1, 1];
	
	Map m1 = new HashMap();
	m1.put(1,"1");
	m1.put(2,"2");
	System.out.println(m1);  //{1=1, 2=2};
	
	Integer[] i1 = {1,2,3};
	System.out.println(i1);  //[Ljava.lang.Integer;@f62373 ;
	System.out.println(Arrays.toString(i1));  //[1, 2, 3];
	
	//显然Collection下的集合类对toString方法提供了覆写操作使得其对象能够更加清晰的打印其中的内容;
	//数组并没有直接覆写toString方法,但是在Arrays类中提供了继承自Object类并重载了的toString()方法,
	//需要注意的是Object类中提供的toString方法不是静态的,所以这里Arrays类不能直接覆写Object的此方法,
	//只能通过重载的方式在toString方法中添加了相应的参数使得此方法成为静态的,并且在Arrays类中起到其效果;
	
	
//////////////////////////////////////////////////
	
	
//关于泛型的不可传递性：
	
	List<String> list4=new ArrayList<String>();
	list4.add("a");
	list4.add("b");
	
	convey(list4);
	for(Object a:list4){     //由于此时list4中即包含了Integer类型又包含了String类型的对象,所以只能用Object输出;但是如果用String来接收,编译不会报错,因为编译器已经给list4标上了String的泛型标签;
		System.out.print(a); //ab1; 
	}
	
	
	//泛型传递的局限性:其只是作用于代码编译阶段,在编译过程中,对于正确检验泛型结果后,会将泛型的相关信息擦出,也就是说,成功编译过后的class文件中是不包含任何泛型信息的;泛型信息不会进入到运行时阶段; 
	
	//可见当list4被当参数传递到convey方法时,JVM并没有把它当成<String>类型的列表,而是默认的Object类型的,因为此处使用List来接收,当把list4传入convey方法时JVM检查list4确实属于List类型,通过编译,
	//也就在此认定此参数为List类型的;所以在方法块中listx.add(1)也就自然通过了编译,因为标上泛型Stirng标签的list4的地址赋给了List类型的listx参数变量来接收,而此变量则被编译器认定为Object型的List对象,
	//就解释了listx可以用add方法添加非String类型对象的原因了;但是如果在convey方法的参数中限制(List<Integer> listx),那list4就无法被传入了,报错：
	//The method convey(List<Integer>) in the type SongCollectionFramework is not applicable for the arguments (List<String>);根据上面编译器给对象标识泛型的机制,这也不难理解报错原因了;
	
	//举个最简单的传递例子就能进一步理解上面的内容:
	
	List<String> a = new ArrayList<String>();
	//List<Integer> b = a;  //由于a已经被编译器标上了String泛型的标签,所以这里会报错:Type mismatch: cannot convert from List<String> to List<Integer>;
	List b = a;  //通过了编译,因为a确实属于List类型;而b不会被赋予a的泛型标签,编译器仅仅认为b是Object类型的List对象;
	for(Object obj:b){  //如果这里用String obj:b来接收,会报编译错误:Type mismatch: cannot convert from element type Object to String; 很好的证明了泛型的传递限制;
		System.out.println(obj);
	}
	
	
	//////////////////////////////////////////
	
	System.out.println();
	//关于迭代器的泛型:
	List<String> aa = new ArrayList<String>();
	aa.add("iterator ");
	Iterator x = aa.iterator();
	//String y = x.next();  //未给出泛型的迭代器默认返回Object类型的元素;报错:Type mismatch: cannot convert from Object to String;
	Iterator<String> x2 = aa.iterator();
	String y2 = x2.next();
	System.out.println(y2);
	
}
	
	
	public static void convey(List listx){ 
		listx.add(1);                       
	}
}



