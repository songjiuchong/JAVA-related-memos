CollectionFramework2：(hash set map)


package prototype;
import java.util.*;
import java.util.Map.Entry;

public class SongCollectionFramework2 {          //(hash set map) and refer to SongCollectionFramework.java (list)
	public static void main(String[] args) {
		Set<Players> set = new HashSet<Players>();
		Players p1= new Players(100,"song1");
		Players p2= new Players(100,"song?");
		Players p3= new Players(101,"song2");
		Players p4= new Players(102,"song3");
		set.add(p1);set.add(p2);set.add(p3);set.add(p4);
		for(Object obj:set){
			System.out.println(obj);
		}
			////////////////////////////////////////////////
			Map<Players,String> map = new HashMap<Players,String>();
			for(int i =0;i<7;i++){
				Players pp =new Players(i+1,"player"+(i+1));
				map.put(pp, ""+(i+1));
			}
			Players pp =new Players(1,"player?");  //由于之前已经覆写了.equals()和hashCode()方法;所以在调用了put()方法
			//后得到了与之前pp对象相同的hashcode,从而放入了同一个散列数组元素(散列桶接着通过检查Entry中的key值是否重复时,会根据"lvl"值判断,所以输出size()为7不变;

			map.put(pp, "?");               //所以这里的put()方法相当于替换了原来pp(1,player1)中的value;
			System.out.println(map.size());
			/////////////////////////////////////////
			Set<Players> set2= map.keySet();                //利用keySet();把key放入一个Set中，再遍历输出key与其value;
			for(Object key:set2){
				Object value=map.get(key);
				System.out.println(key+"="+value);
			}
		//关于HashMap和HashSet,对于它们的操作都是先调用hashCode();计算出散列的位置后,再放入,删除或取出，如有相同key的hashCode,
		//在放入时调用.equals();检查key是否相同，对于HashMap如果相同则替换,不同则添加;对于HashSet如果相同则不会放入覆盖;
			
		//关于泛型和集合： jdk5.0之后出现了泛型
			List<String> list = new ArrayList<String>();
			list.add("1");
			list.add("2");
			list.add("3");
			//list.add(new Integer(2));不符合泛型会报错！
			for(String s :list){
				System.out.println(s);
			}
			
			Map<String,Integer> map2=new HashMap<String,Integer>();  //虽然是泛型,但是其类型必须是引用类型的;
			map2.put("play6",3);    //这里的3自动装箱了;
			map2.put("play7",2);    
			map2.put("play8",5);   
			Set<String> set3 = map2.keySet(); //由于之前map2已经被定义了泛型,编译器在这里会检查泛型中的声明是否和之前一致,不一致报错;
			Set<Entry<String, Integer>> set4 = map2.entrySet(); //如果定义entry类的泛型,就必须按照之前定义map2时的泛型定义,不然会编译报错;
			for(String key:set3){
				Integer value=map2.get(key);
				System.out.println(key+"="+value);
			}
			
			//为了使意义更明确,HashMap将原本HashTable中存在的caontains方法换成了containsKey()和containsValue();
			System.err.println(map2.containsKey("play6"));
			System.err.println(map2.containsValue(3));

//java中除了有有序的集合接口：List,还可以有无序的集合接口：Set,Set中的元素没有存放的次序,其中元素的判断比较由.equals()判断,并且不允许有重复的元素;
//HashSet的底层是一个HashMap

//Set的主要方法：
//增加：add();
//删除：remove(Object obj),clear();
//查找：contains(),迭代器  Set中不存在get()方法,因为他的元素不存在顺序,只能通过迭代器（for each）循环输出;
//个数：size();

///////////////////////////////////////////////////////////////////////////////////
			//测试LinkedList和ArrayList随机访问的速度：
			Integer a = 1;
			LinkedList<Integer> listx = new LinkedList<Integer>();
			for(int i =0; i<1000000;i++){
				listx.add(a);
			}
			System.out.println(listx.size());
			long start=System.nanoTime(); //返回最准确的可用系统计时器的当前值,以纳米秒为单位(10^-9);
			listx.get(500000);
			long end=System.nanoTime();
			System.out.println(start);
			System.out.println(end);
			System.out.println("LinkedList 随机访问： "+ (end-start)); //大约0.0047s;
////////////////////////////////////////////
			List<Integer> listxx = new ArrayList<Integer>();
			for(int i =0; i<1000000;i++){
				listxx.add(a);
			}
			System.out.println(listxx.size());
			long start2=System.nanoTime(); 
			listxx.get(500000);
			long end2=System.nanoTime();
			System.out.println(start2);
			System.out.println(end2);
			System.out.println("ArrayList 随机访问： "+ (end2-start2)); //大约0.00002s;
			
//////////////////////////////////////////////////////////////////////////
long start3=System.nanoTime(); 
for(Integer tempx:listx){
	//System.out.println(tempx);
}
long end3=System.nanoTime(); 
System.out.println("LinkedList 遍历： "+ (end3-start3)); //0.000015;


long start4=System.nanoTime(); 
for(Integer tempx:listxx){
	//System.out.println(tempx);
}
long end4=System.nanoTime(); 
System.out.println("ArrayList 遍历： "+ (end4-start4)); //0.000033;


	}
}
/////////////////////////////////////////////////////////////
//比较总结：1.LinkedList不支持高效的随机元素访问,但是它的遍历能力比较强;

//可以这样说,当操作是在一系列数据的最后改动元素而不是开头或者中间,并且需要随机访问其中元素时,ArrayList会提供较高的性能;
//而当你是在一系列数据的开头或者中间改动元素,并且按照顺序访问其中元素时,就应该使用LinkedList;

class Players{
	public Players(int lvl, String name) {
		super();
		this.setLvl(lvl);
		this.setName(name);
	}
	
	public Players() {
		super();
	}

	private int lvl;
	private String name;
	public int getLvl() {
		return lvl;
	}
	public void setLvl(int lvl) {
		this.lvl = lvl;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String toString(){
		return lvl+","+name;
	}
	public boolean equals(Object obj){
		if(obj==null)
			return false;
		else if(obj instanceof Players){
			Players s = (Players)obj;
			return this.lvl==s.lvl;
		}
		return false;
	}
	public int hashCode(){
		int type=this.getClass().hashCode();
		return type*43+lvl;
	}
}

//总结：
//CollectionFramework在JAVA API中的大致树形结构：
/*                                            
                                                  Collection                          <interface>
                       Map                            |
                        |                  |                       |         
                        |                 Set                     List                <interface>
                   |         |             |                       |
                   |         |         |       |              |         |      
                HashMap   TreeMap   HashSet  TreeSet      ArrayList  LinkedList       <class>
              (HashTable)              |                  (Vector)
                                 LinkedHashSet                                        <class>
  
 ///////////////////////////////////////////////////////////////////////////////////////////////////////////
 About  Collection Framework:
 1.集合框架（Collection Framework)泛指java.util包的若干类和接口;
 2.Collection 和 Collections的区别是：Collection是集合类的上级接口;Collections是针对集合类的一个帮助类,它提供一系列静态方法实现对各类集合的搜索、排序、线程安全化等操作;
 3.ArrayList 和 Vector 它们的主要区别为：Vector是线程安全,也就是说它的方法之间线程同步,而ArrayList是线程不安全的,但是效率要高一些;
 4.HashMap 和 HashTable 的区别为：(1)HashTable是线程安全的,HashMap不是线程安全的;(2)HashMap允许null作为一个entry的key或者value,而HashTable不允许;
 5.LinkedList也不是线性安全的;它在内存中是离散而非连续的,每一个元素都有下一个元素的引用,所以每一个元素占用的空间相对比较大;
        它使用双向链表实现存储,按序号索引数据需要进行向前或向后的遍历,但是插入数据时只需要记录本项的前后项引用即可,所以插入速度快;LinkedList中的提供了使它能被当做堆栈来使用;
         注：参见主方法最后对比ArrayList和LinkedList效率的代码;
 */


//集合框架大致分为三种类型：
//(1)列表(List):以一定的顺序容纳对象,能够对容纳的对象进行随机存取;
//(2)数学概念中的集合(Set):已无序的方式容纳对象,并且任意两个对象均不相同;
//(3)映射(Map):将对象以一个键(Key)值存放,根据键值可以访问对应的对象;

//list中可以存在多个null元素;set中只能存在一个null元素;map与set一样只能存在一个key为null的元素;

//Collection接口：Collection是最基本的集合接口，一个Collection代表一组Object,即Collection的元素;一些Collection允许存在相同的元素,一些能够排序,
//而另一些不行;JAVA SDK
//(SDK 是 Software Development Kit 的缩写，中文意思是“软件开发工具包”;JDK 是 Java Development Kit 的缩写，中文意思是“JAVA开发工具包”;
//所以，不难看出，SDK是一个总称，JDK是SDK中具体的一种软件开发包;)
//不提供直接继承自Collection的类,而提供继承自Collection的子接口如：Set,List;

/* Hash: 一般翻译做“散列”,也有直接音译为“哈希”的，就是把任意长度的输入（又叫做预映射, pre-image）,通过散列算法,
  变换成固定长度的输出,该输出就是散列值;这种转换是一种压缩映射,也就是,散列值的空间通常远小于输入的空间,
  不同的输入可能会散列成相同的输出,而不可能从散列值来唯一的确定输入值;
  
 Hash 基本概念：在数学范畴中若结构中存在和关键字K相等的记录，则必定在f(K)的存储位置上;由此,不需比较便可直接取得所查记录;
 称这个对应关系f为散列函数(Hash function),按这个思想建立的表为散列表;
 
 进一步的说：对不同的关键字可能得到同一散列地址,即key1≠key2,而f(key1)=f(key2),这种现象称碰撞;
 具有相同函数值的关键字对该散列函数来说称做同义词。综上所述，根据散列函数H(key)和处理冲突的方法将一组关键字映象到一个有限的连续的地址集（区间）上,
 这种表便称为散列表;典型的散列函数f(K)都有无限定义域,和有限的值域;
 
在JAVA中使用的HASH函数为：直接取余法;

 */

//Map：映射,一个key对应一个value
//概念：映射Map,又称为字典(Dictionary),是由关键字(Key)及其对应的元素值(Value)所组成的元素单元 (Element)的表单式集合; 通常,对于Map而言,使用给定的Key,可以迅速地从单元集合中检索到相应的元素;因此,在需要对大量数据进行查找操作而查找的性能又占据重要地位的场合,Map无疑是一种较理想的容器;

//属性： size(),isEmpty();
//添加元素： put(key,value); key不可以重复,放入是无序的,value可以相同出现;
//删除元素： remove(key),clear();
//更新元素： put(key,value); 查找到已有的key,并更新value;
//查询元素： get(key),containsKey(key),containsValue(Value);

/*
遍历Map:
对于Map接口而言,JDK源码中将其分为三种视图(集合视图是查看集合中部分或全部数据的窗口),起始就是三种以某种
集合接口存储Map中键值的表现形式分别为：

(1)获取所有的key：方法返回一个Set<K>视图,其元素是map中所有的key; 
  keySet();
 
 
(2)获取所有的value: 返回一个Collection<V>视图,不是Set<K>视图;
  values();
 
(3)Entry类：Entry(java.util.Map.Entry)是map中定义的内部类,专属于描述map中的键值对
map中每一个Entry实例代表一组键值对;Entry实例保存在每一个map元素里的链接列表linkedlist的一个元素里;
然后可以通过此类中的getKey()和getValue()方法来获取这组键值对的键和值;Entry也支持泛型,其泛型必须与保存它的Map的泛型一致;

  set<Entry<K,V>> entries=map.entrySet();
  
  Map<String,Integer> a = new HashMap<String,Integer>();
		a.put("a",1);
		a.put("b",2);
		Set set = a.entrySet();
		for(Object obj:set){
			System.out.println(obj); //Entry向上转型为Object,由于覆写过toString()方法,这里输出:b=2;a=1;
		}
		
		Set<Entry<String,Integer>> set2 = a.entrySet();  //由于a已被编译器标记为<String,Integer>泛型,所以这里Set如果用泛型就只能为:Entry<String,Integer>,不然就会报错:ype mismatch: cannot convert from Set<Map.Entry<String,Integer>> to Set<...>;
		for(Map.Entry obj2:set2){  //如果没有import Map的内部类:java.util.Map.Entry;可以直接引用"Map.Entry";
			System.out.println(obj2); //输出:b=2;a=1;
			System.out.println(obj2.getKey()); //输出:b;a;
			System.out.println(obj2.getValue()); //输出:2;1;



//(哈希)散类表的概念：它的数据结构是成对存储key:value,key不能重复,value可以,无序存储;由一个线性数组来存储若干个链表,每一个唯一的散列值对应着这个数组的一个下标,数组的每一个元素都是一个链表,链表中的每一项就是按照key的散列值存入的Entry:key和value;
//容量：散列表中散列数组的大小;
//散列运算：key->散列值(散列数组下标),key的散列值通过hashCode方法获得,然后通过计算得出所在散列数组的下标,也就是此数组的位置;

//散列表存储原理：就是把Key通过一个固定的算法函数既所谓的哈希函数转换成一个整型数字,然后就将该数字对数组长度进行取余,
//取余结果就当作数组的下标,将value存储在以该数字为下标的数组空间里，而当使用哈希表进行查询的时候,
//就是再次使用哈希函数将key转换为对应的数组下标,并定位到该空间获取value,如此一来，就可以充分利用到数组的定位性能进行数据定位; 

//散列桶：散列数组中每一个空间存储的其实就是一个散列桶(一个链表),它是散列值相同的元素的线性集合;
//加载因子：就是散列数组的加载率,一般在50%-100%,75%最佳,其实就是个利用率,负载率,在75%时就选择去扩容;
//散列查找：
//1.key计算散列值 hashcode
//2.根据散列值确定散列数组下标,找到散列桶(链表);
//3.按顺序比较key,如果一样,返回其value;
//
//
 
 
关于TreeSet和TreeMap;

与HashSet完全类似,TreeSet里面绝大部分方法都市直接调用TreeMap方法来实现的;

相同点：
TreeMap和TreeSet都是有序的集合,也就是说他们存储的值都是排好序的(默认从小打到);
TreeMap和TreeSet都是非同步集合,因此他们不能在多线程之间共享,不过可以使用方法Collections.synchroinzedMap()来实现同步;
运行速度都要比Hash集合慢,他们内部对元素的操作时间复杂度为O(logN),而HashMap/HashSet则为O(1);

不同点：
最主要的区别就是TreeSet和TreeMap实现Set和Map接口;
TreeSet只存储一个对象,而TreeMap存储两个对象Key和Value(仅仅key对象有序);

缺点：

       对于 TreeMap而言,由于它底层采用一棵"红黑树"来保存集合中的 Entry,这意味这 TreeMap添加元素,取出元素的性能都比 HashMap低;
当 TreeMap添加元素时,需要通过循环找到新增 Entry的插入位置,因此比较耗性能;
当从 TreeMap中取出元素时,需要通过循环才能找到合适的 Entry,也比较耗性能;

HashMap的存储位置是按照key这个对象的hashCode来存放的,而TreeMap则是按照实现的Comparable接口的compareTo这个方法来存储的,只要compareTo的返回结果为0就表示两个对象相等,
那么就存不进去两个对象,并且排序是按照compareTo方法定义的排序规则;
甚至我们都不用重写equals和hashCode方法,而只需要实现Comparable接口来重写comparareTo方法就行了;

优点：
   TreeMap中的所有 Entry总是按 key根据指定排序规则保持有序状态,TreeSet中所有元素总是根据指定排序规则保持有序状态;
 
 
关于"红黑树";

 红黑树又称红-黑二叉树,它首先是一颗二叉树,它具体二叉树所有的特性;同时红黑树更是一颗自平衡的排序二叉树;
 平衡二叉树必须具备如下特性：
 它是一棵空树或它的左右两个子树的高度差的绝对值不超过1,并且左右两个子树都是一棵平衡二叉树;
 也就是说该二叉树的任何一个子节点,其左右子树的高度都相近;它是通过 :左旋rotateLeft(),右旋rotateRight(),着色setColor()来保持这一特性的;
 

 
*/


