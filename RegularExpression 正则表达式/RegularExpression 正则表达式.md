RegularExpression 正则表达式

package prototype;

import java.util.Arrays;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

//正则表达式(REGULAR EXPRESSION or REGEX / RE):
//使用单个字符串来描述,匹配一系列符合某个句法规则的字符串;
public class SongRegularExpression {
/*
 正则表达式语法：
 []：描述一个字符,方括号中可以规定出现的字符是什么,任选其一;
  例如：
  [abc]:a或b或c其中的一个字符;
  [^abc]:非abc三个中的一个字符;
  [a-z]:表示可以出现任意一个英文小写字母;
  [a-z0-9A-Z]:字母数字间本身就默认是或的关系,所以可以直接连写;表示任意数字字母;
  [a-z&&[^c-f]]:表示可以使任意一个非cdef的小写字母;
  
  ".":代表任意一个字符(包括符号);
  \d:任意一个数字,相当于[0-9];
  \D:一个非数字,相当于[^0-9];
  \s:任意一个空白字符(空格/制表等); 等价于[\f\n\r\t\v]; 
  \S：一个非空白字符;
  \t:一个制表符(tab);
  \w:任意一个单词字符[0-9a-zA-Z_];
  \W:一个非单词字符;
  
  量词：
  ?:修饰其左边的内容出现0-1次;    [a-z]?:最多出现一个小写字母;
  *:修饰其左边的内容出现0-正无穷次; [a-z]*:任意个小写字母;
  +:修饰其左边的内容出现1-正无穷次; [a-z]+:至少一个小写字母;
  
  {n}:n是一个数字,修饰其左边内容必须要出现n次; [a-z]{3}:必须有三个小写字母;
  {n,m}:n和m是一个数字,且n要小于m,修饰其左边的内容需要出现n-m次,双闭区间;
  {n,}:n-正无穷次;
  
  |:代表选择;
  例如：
  gray|grey
  
  ():将括号内的内容看做一个整体,定义操作符的范围和优先度;
    例如：
    gr(a|e)y 等价于 gray|grey
   (\+86)?
   (\+86|0086)? :"+86"或"0086"两者其中一个可以出现一次或者不出现;
   
   ^:匹配输入字符串的开始位置;
   $:匹配输入字符串的结束位置;
   
   //实例：
    
邮政编码：^[0-9]{6}$
         ^\d{6}$
用户名：^[a-z0-9_-]{3-16}$  注意"-"中划线只要它后面没有内容就不代表从..到..,而是单纯表示中划线,所以中划线一般都放在表达式最后;
密码：  ^[a-z0-9_-]{6,18}$
电话号码： ^(\+86)?\s?\d{11}$
身份证： ^\d{15}(\d{2}[0-9xX])?$
       
       
 */
	
  //String中的方法就有可以支持正则表达式验证的：
  //boolean matches(String rex); 使用rex这个正则表达式匹配当前字符串,若格式符合则返回true；
  
  //String[] split(String rex); 根据正则表达式拆分当前字符串; 
	
	
	
  public static void main(String[] args){
	  
  //描述一个邮箱的正则表达式：sssss_123@qq.com
	  
  //正则：[a-zA-Z0-9_]+@[0-9a-zA-Z]+\.com
  /*注意：由于.在正则表达式代表任意一个字符,那如果需要格式为.com就需要转移字符\.;但是在
    JAVA中.本身没有特殊含义,在字符串中使用\.会报错;所以就需要在字符串中使用\\.
    (由于\本身就是用来把其后有特殊含义的符号转换为没有特殊还以的符号,如\")
        来告诉JVM把第二个\转换为没有特殊含义的单纯\符号显示,那\.这样的组合就不会报错了;同样的
        情况还包括\\+等的运用;
  */  
  
  //matches:
  //boolean matches(String regex); //告知此字符串是否匹配给定的正则表达式;
       
  String regex="^[a-zA-Z0-9_]+@[0-9a-zA-Z]+\\.com$";
  
  String mail="sssss_123@qq.com";
  System.out.println("是否为一个有效的邮箱地址： "+mail.matches(regex)); //true;
	 
  
  //描述一个手机号的正则表达式： +86 1381111111 /0086 13811111111
  //(\+86|0086)?\s\d{11}
  //
  String phoneNumberRegExString="(\\+86|0086)?\\s\\d{11}";
  String phoneNumber="+86 13811111111";
  System.out.println("是否为一个有效的手机号码： "+phoneNumber.matches(phoneNumberRegExString)); //true;
  
  //split:
  String info="123;345;567;789";
  String[] array=info.split(";");
  for(String temp:array){
  System.out.print(temp+";");
  }
  System.out.println(Arrays.toString(array));
  
  String info2=";;;123;;;345;;;";
  String[] array2=info2.split(";");
  System.out.println(Arrays.toString(array2));
  //[, , , 123, , , 345] ;;之间没有内容但是输出不是null是空字符串;
  //注意1:如果;;在最后则会忽略;
  
  
  //split的应用：获取图片后缀名;
  String imgName="1234.jpg";
  //1.将图片名根据"."拆分; 2.用当前系统时间的毫秒值/序号来为新图片命名; 3.用新图片名加上后缀;
  
  //1
  String[] name =imgName.split("\\.");
  //2
  String newImgName=System.currentTimeMillis()+"";
  //3
  newImgName=newImgName+"."+name[1];
  System.out.println(newImgName);
  //注意2:split是根据正则表达式来拆分的,它根据所给的正则表达式逐个对字符串检查,
  //如果不是参数所给的正则表达式则成为数组的一个元素,直到检查到所给的正则表达式,
  //删除它,然后准备将接下去的内容放入数组的下一个元素;所以如果上式中split参数为("."),
  //对于正则来说"."意味着任意字符那也就是imgName中每一个字符都被删除了所以数组的元素个数为0;
  //而另一种情况,如果是类似"a$b$c$d",然后用split("$");最后的结果是照原样输出：a$b$c$d;原因是：
  //split()参数中的是正则表达式,$是有含义的,而用来调用split方法的是String对象,其中的$是没有特殊
  //含义的,所以在进行对比时,用有特殊含义的$去比较单纯字符$肯定是不同的,所以最后没有找到匹配分割符,
  //全部输出,所以如果想要用$分割,就要split(\\$);
  
  //ReplaceAll
  //参数：需要被替换字符串的正则表达式,替换成的内容;
  
  String str="12345abcd5678iuys9678kjkhg";
  str=str.replaceAll("[0-9]+", "#number#");
  System.out.println(str);
  
  //java.util.regex包中有两个核心的类：
  //Pattern/Matcher,其中Pattern用来定义一个正则表达式,而Matcher对象来实施针对特定字符串的匹配,查找,
  //替换等操作;
  
  Pattern p=Pattern.compile("[abc]123[xyz]");
  Matcher m1=p.matcher("abc123xyz");
  Matcher m2=p.matcher("b123x");
  
  System.out.println(m1.matches());
  System.out.println(m2.matches());
  //比起String直接使用正则相关方法,这两个类比较繁琐;
  
  }
}

/*
 * 补充:
 * java中的转义符由\担当;
 * 
 * 在用''和""显示字符或字符串内容时,遇到到显示\这样的字符时需要转义\\,达到显示\的目的;
 * 
 * 还需要注意的是,''单引号用于显示单个字符,可以是空格但是不能是空字符;
 * 
 * 另外,在""双引号中无法直接显示"字符,需要转义,但是可以直接显示',相同的,在''中无法直接显示',需要转义,但是"可以直接显示;
 * 
 * 
 * 例:
 * 
 * System.out.println("\\+''-\"");  //显示结果:\+''-" ;
 * 
 * System.out.println(""+'"'+'\"'+'\'');  //显示结果:""' ;
 * 
 * System.out.println(""");  //编译报错;
 *  
 * System.out.println('''); //编译报错;
 * 
 * System.out.println('');  //编译报错;
 * 
 * System.out.println('12');  //编译报错;
 * 
 * 
 * 注意:
 * 
 * System.out.println('a'+'a'+"123");
 * 
 * 类似这样的语句会输出什么呢?
 * 
 * 结果: 194123 ;
 * 
 * ''单引号中的内容相当于char类型的字符,在没有用""连接之前输出的内容一定是它所对应的字符编码数;
 * 
 * 
 */


