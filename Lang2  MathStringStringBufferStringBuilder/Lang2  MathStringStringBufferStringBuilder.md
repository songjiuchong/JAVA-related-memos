Lang2 : Math/String/StringBuffer/StringBuilder


package prototype;

import testdemo.SongDemo1;

public class SongJAVASElang2 {      // Lang2 : Math;String/StringBuffer/StringBuilder;
	public static void main(String[] args) {
		
		System.out.println(Math.abs(-5));     //去负号操作; 参数为整数时返回int,为浮点数时返回double: 5/5.0;
		System.out.println(Math.floor(4.5));  //去小数操作; 返回double值: 4.0; 
		System.out.println(Math.round(4.5));  //四舍五入操作; 参数为整数时返回int,为浮点数时返回long: 5;
		System.out.println(Math.sqrt(4.5));   //开平方数操作; 返回double值;
		///////////////////////////////////////////////////////
		System.out.println(Math.random());  //去随机数操作,返回一个double值: [0-100);
		
		//随机数的生成，比如生成0-100随机数:
		int temp= (int)(Math.random()*101); 
		System.out.println(temp);
		
		//////////////////////////////////////////////////////
		
		String st = "marvel";
		
		System.out.println(st.length());  //返回字符串的长度(int);
		
		System.out.println(st.substring(2,5));  //返回"rve";
		
		System.out.println(st.indexOf("ar"));  //1; 返回"ar"在"marvel"中的索引位置;
		
		System.out.println(st.toUpperCase());  //"MARVEL"; 返回"marvel"的全大写字符串;
		
		System.out.println(st.startsWith("m"));  //true; 返回表明"marvel"是否以"s"为开头的boolean值(大小写敏感);
		
		String str = "marvel,hero";
		String[]arr = str.split(","); //返回一个string数组,此中元素按照","来分割;
		for(int i=0; i<arr.length;i++){
			System.out.println(arr[i]);
		}
		
		//////////////////////////////////////////////////////////////////////////////
		StringBuilder str1 = new StringBuilder("abcd");
		StringBuilder str2 = new StringBuilder("abcd");
		//StringBuilder str3 = "123";    这里会报错,只有String类可以直接声明字符串对象;
		System.out.println(str1==str2);         //false;
		System.out.println(str1.equals(str2));  //运行结果是false,因为在StringBuilder或Buffer中没有重写equals()方法,直接继承自object类,也就相当于==比较引用变量的地址;
		str2=str1.append("e");                  //不会创造新的对象,还是在原来的内存追加“e”,所以被称为可变字符串类;
		System.out.println(str1==str2);			//true;
		System.out.println(str1);				//abcde;
	}
}

/*java中有关数学方面运算的代码由java.lang.Math类提供(java.lang包是不需要input的),主要包括：
 * 
1.abs(double a) 返回double值的绝对值;

2.floor(double a) 返回一个无限接近所给参数的double数,该数小于参数,并等于某个整数,多用于舍去小数部分;

3.static long round(double a) 返回一个最接近参数的long (也就是四舍五入);

4.sqrt(double) 返回开平方根;

5.random() 返回一个正的double值,其值满足大于等于0.0且小于1.0;

*/

////////////////////////////////////////////////////////////////////////////////////////////////////

/*  String:
 *  String(参考详细资料:"SongStringObjectAndConstantPool.java"/"SongArrayAndClass");
 *  关于字符串的链接：
 *  String类的其拘留字符串是一个char[];字符串的链接在底层用到了Arrays.copyOf()和StringBuilder append()方法;
 *  
 *  例一:String a ="123"+"456"; 字面量直接链接：
 *      在类装载的时候"123","456"已经建立了自己的拘留字符串内存(其实就是char[]),
 *  需要注意的是由于是字面量直接用运算符号链接,此时"123456"的拘留字符串内存也通过Arrays.copyOf()创建了;
 *  其过程是："123456"=Arrays.copyOf("123","123".length+"456".length);
 *  这里会出现一个长度为6的char[]的副本:[1][2][3][0][0][0](此副本被当做"123456"的拘留字符串保存),
 *  然后运用System.arrayscopy("456",0,"123456","123".length,"456".length);
 *  将"456"拘留字符串内存中的char[]:[4][5][6]赋值给"123456"拘留字符串内存中的char[]值为空的后几位,至此字符串链接结束;
           同时,之后的字符串常量池中的"123456"直接指向了这个拘留地址（常量池解析）;

          例二：String b ="789"; String c= a+ b ; 含有变量的链接：
                      当执行a+b时,JVM首先会在堆中创建一个StringBuilder类,同时用a指向的拘留字符串对象完成对StringBuilder的初始化,
                      然后调用append方法完成对b所指向的拘留字符串的合并操作，接着调用StringBuilder的toString()方法在堆中创建一个String对象,
                      最后将生成的String对象的堆地址存放在局部变量c中;这里要注意了,此时堆中实际上有四个相关的字符串对象：
                      两个拘留字符串对象、一个String对象和一个StringBuilder对象;

 *  String(JDK1.0时代)          不可变字符序列
    StringBuffer(JDK1.0时代)    线程安全的可变字符序列
    StringBuilder(JDK1.5时代)   非线程安全的可变字符序列 
    
    StringBuffer和StringBuilder算法和API几乎一样！ 前者是java早期提供的,速度稍慢,线程安全的类,后者是java5以后提供的,速度更快,非线程安全;
          后来sun公司试图摘取前者的线程安全,但是考虑到之前用这个代码的所有使用者会面临安全问题,所以重新定义了一个类StringBuilder;
 /////////////////////////////////////////////////////   
    StringBuffer与String的可变性问题;
          这两个类的部分源代码：

Java代码  //String   
public final class String  
{  
        private final char value[];  
  
         public String(String original) {  
              // 把原字符串original切分成字符数组并赋给value[];  
         }  
}  
  
//StringBuffer   
public final class StringBuffer extends AbstractStringBuilder  
{  
         char value[]; //继承了父类AbstractStringBuilder中的value[]  
         public StringBuffer(String str) {  
                 super(str.length() + 16); //继承父类的构造器,并创建一个大小为str.length()+16的value[]数组;
                 append(str); //将str切分成字符序列并加入到value[]中  ;
        }  
}  

      很显然，String和StringBuffer中的value[]都用于存储字符序列;但是,
  (1) String中的是常量(final)数组，只能被赋值一次;
      比如：new String("abc")使得value[]={'a','b','c'}(查看jdk String 就是这么实现的)，之后这个String对象中的value[]再也不能改变了;这也正是大家常说的,String是不可变的原因 ;
      注意：这个对初学者来说有个误区,有人说String str1=new String("abc"); str1=new String("cba");不是改变了字符串str1吗？那么你有必要先搞懂对象引用和对象本身的区别;这里我简单的说明一下,对象本身指的是存放在堆空间中的该对象的实例数据(非静态非常量字段),而对象引用指的是堆中对象本身所存放的地址,一般方法区和Java栈中存储的都是对象引用,而非对象本身的数据;


  (2) StringBuffer中的value[]就是一个很普通的数组,而且可以通过append()方法将新字符串加入value[]末尾;这样也就改变了value[]的内容和大小了;

      比如：new StringBuffer("abc")使得value[]={'a','b','c','',''...}(注意构造的长度是str.length()+16);如果再将这个对象append("abc")，那么这个对象中的value[]={'a','b','c','a','b','c',''....};这也就是为什么大家说 StringBuffer是可变字符串 的涵义了;从这一点也可以看出,StringBuffer中的value[]完全可以作为字符串的缓冲区功能;其累加性能是很不错的,在后面我们会进行比较;

     总结:讨论String和StringBuffer可不可变;本质上是指对象中的value[]字符数组可不可变,而不是对象引用可不可变;
////////////////////////////////////////////////////////////////////////////
 
  
String常量与String变量的"+"操作比较 :

        ▲测试①代码：     (测试代码位置1)  String str="";
                      (测试代码位置2)  str="Heart"+"Raid";
            [耗时：  0ms]
             
       ▲测试②代码        (测试代码位置1)  String s1="Heart";
                                     String s2="Raid";
                                     String str="";
                    (测试代码位置2)  str=s1+s2;
            [耗时：  15—16ms]
            
      结论：String常量的“+连接”  稍优于  String变量的“+连接”; 
      
      原因：测试①的"Heart"+"Raid"在编译阶段就已经连接起来,形成了一个字符串常量"HeartRaid",并在类加载时指向堆中的拘留字符串对象;运行时只需要将"HeartRaid"指向的拘留字符串对象地址取出1次,存放在局部变量str中;这确实不需要什么时间;
                     测试②中局部变量s1和s2存放的是两个不同的拘留字符串对象的地址;然后会通过下面三个步骤完成“+连接”：
                                1、StringBuilder temp=new StringBuilder(s1)，
                                2、temp.append(s2);
                                3、str=temp.toString();
               我们发现,虽然在中间的时候也用到了append()方法,但是在开始和结束的时候分别创建了StringBuilder和String对象;可想而知：调用1W次,是不是就创建了1W次这两种对象呢？不划算;

     但是,String变量的"+连接"操作比String常量的"+连接"操作使用的更加广泛; 这一点是不言而喻的;
    

String对象的"累+"连接操作与StringBuffer对象的append()累和连接操作比较: 
          ▲测试①代码：     (代码位置1)  String s1="Heart";
                                     String s="";
                        (代码位置2)  s=s+s1;
             [耗时：  4200—4500ms]
             
          ▲测试②代码        (代码位置1)  String s1="Heart";
                                    StringBuffer sb=new StringBuffer();
                       (代码位置2) sb.append(s1);
            [耗时：  0ms(当循环100000次的时候,耗时大概16—31ms)]
            
         结论：大量字符串累加时,StringBuffer的append()效率远好于String对象的"累+"连接 ;
         原因：测试① 中的s=s+s1,JVM会利用首先创建一个StringBuilder，并利用append方法完成s和s1所指向的字符串对象值的合并操作,接着调用StringBuilder的 toString()方法在堆中创建一个新的String对象,其值为刚才字符串的合并结果;而局部变量s指向了新创建的String对象;

                        因为String对象中的value[]是不能改变的,每一次合并后字符串值都需要创建一个新的String对象来存放;循环1W次自然需要创建1W个String对象和1W个StringBuilder对象,效率低就可想而知了;


                      测试②中sb.append(s1);只需要将自己的value[]数组不停的扩大来存放s1即可;循环过程中无需在堆中创建任何新的对象;效率高就不足为奇了;


 /////////////////////////////////////////////////////////////////////  
 *  String类常用方法：
 *  
 *  1.public int length(); 返回此字符串的长度;
 *  
 *  2.public char charAt(int index); 返回指定索引处的char值,范围是0~~length()-1;
 *  
 *  3.public int indexOf(String str); 返回第一次出现的指定子字符串在此字符串中的索引;
 *    public int indexOf(String str, int fromIndex)从指定索引处开始查找;如果index不存在 返回-1;
 *    
 *  4.public String toUpperCase(),toLowerCase(); 把所有的字母转换成大/小写;
 *  
 *  5.public String substring(int beginIndex),substring(int beginIndex,int endIndex); 根据给予的索引返回一个新的字符串,
 *  要注意的是endIndex是一个开区间,beginIndex是一个闭区间;
 *  
 *  6.public boolean startWith(String prefix)/endWith(String suffix); 判断一个字符串是否已指定的前缀开始或后缀结束;
 *  
 *  7.public String trim();返回字符串的副本,忽略其前导空白和尾部空白;空白包括：\t \n \r \f space;
 *  
 *  
 *  \t 制表符
    \r 回车符    回到行首 ; Unix系统里,每行结尾只有“\n”;Windows系统里面,每行结尾是“\n\r”;Mac系统里,每行结尾是“\r”;
	\n 换行符
	\f 换页符
    
    8.需要特别注意的是：String类的构造方法中提供了String(char[] value) 这样的重载方法,可以一个字符数组作为参数传入String对象,使其表示字符数组参数中当前包含的字符序列;
    //类似于字符数组调用了toString()方法;

	9. public String replaceAll(String regex, String replacement); 使用给定的 replacement替换此字符串所有匹配给定的正则表达式的子字符串;
	
	10.public String replace(CharSequence target,CharSequence replacement); 使用指定的字面值替换序列替换此字符串所有匹配字面值目标序列的子字符串;
	
	11.public String[] split(String regex); 根据匹配给定的正则表达式来拆分此字符串;
	
	
	
  	实例:
  	金额转换,阿拉伯数字的金额转换成中国传统的形式如:(￥ 1011)->(一千零一拾一元)输出;
  	
  	public class SongDemo1 {
	private static final char[] data = new char[]{ 
		'零','壹','贰','叁','肆','伍','陆','柒','捌','玖' 
	}; 
	private static final char[] units = new char[]{ 
		'元','拾','佰','仟','万','拾','佰','仟','亿' 
	}; 

	public static String convert(int money) 
	{ 
		StringBuffer sbf = new StringBuffer(); 
		int unit = 0; 
		while(money!=0) 
		{ 
			sbf.insert(0,units[unit++]); 
			int number = money%10; 
			sbf.insert(0, data[number]); 
			money/= 10; 
		} 
		return sbf.toString().replaceAll("零[拾佰仟]","零").replaceAll("零+万","万").replaceAll("零+元","元 ").replaceAll("零+","零");  
	} 
	
		public static void main(String []args){
		System.err.println(SongDemo1.convert(109090913)); //壹亿零玖佰零玖万零玖佰壹拾叁元;
	}	
	
} 


补充:

StringBuilder reverse(); 反置此StringBuilder中字符串的字符顺序;
	......
	StringBuilder sb = new StringBuilder(SongDemo1.convert(109090913));
	System.err.println(sb.reverse());  //元叁拾壹佰玖零万玖零佰玖零亿壹;
  	
  	
 */


