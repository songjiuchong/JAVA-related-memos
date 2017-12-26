Scanner类的next and nextLine混合运用

package prototype;

import java.util.Scanner;

public class SongnextAndnextLine {
	public static void main(String[] args) {
		Scanner console = new Scanner(System.in);
		Scanner console2 = new Scanner(System.in);
		for(int i =0;i<2;i++){   //这里循环一次来测试输出结果;
		System.out.println("imput!"); //循环开始的标志,提示输入;
		
		String c1=console.next();
		System.out.println(c1);    //这里的2行都输出了console中的第一个输入内容,相当于next();方法是把下一个输入内容赋值给c1;输入内容以空格或tab隔开;
		System.out.println(c1);    //结果同上;
		
		c1=console.next();         //由于之前console中已经输出了第一个输入的对象,现在其内部指针停在了第二个输入内容前,next();方法以此空格为标识读取下一个内容;如果还有第二个输入内容则会在此处输出,如果没有那将会再次要求用户输入;
		System.out.println(c1);
		
		c1=console.nextLine();     //运行到这里为止,console中已经输出了2个输入的内容了,指针目前停留在第三个输入内容前(如果没有第3个输入内容,此处将会输出一个空格),由于此处调用了nextLine();方法，程序会将指针处开始的之后一整行内容都输出(内容前同样有一个空格),至此console中将再无剩余的输入内容;
		System.out.println(c1);
		System.out.println(c1);    //同样这里的c1被赋与了指针后一整行的内容;
		
		//由于出现nextLine();方法,程序在此处一定会再次要求用户输出;
		c1=console.nextLine();     //由于console中已无输入内容,这里用户将再次被要求输入内容;如果不输入内容直接回车,将输出一个空格;
		System.out.println(c1);    //输入的所有内容将会在此处被输出,sonsole中继续保持空内容;
		
		//由于出现nextLine();方法,程序在此处一定会再次要求用户输出;
		c1=console.next();         //由于console中已无输入内容,这里用户将再次被要求输入内容;
		System.out.println(c1);    //此处若是输入大于1个内容,进入下一个循环后会直接输入此内容,因为下一个Scanner对象接收的方法依旧是next();
		}
	}
}

//注意点：在出现next();和nextLine();连用的情况下,如果前一个next();用完了所有输入内容,那下一个Scanner对象若是调用next();方法则会再次要求客户输入;要是后一个Scanner对象调用的是nextLine();那这里会输出一个空格(如果此时Scanner对象中还存在输入内容则先输出一个空格再将剩下的整行内容一并输出)并结束此轮输入/输出;继续接下去的程序;
//而如果前一个为Scanner对象调用nextLine();则后一个无论是next();还是nextLine();都会被要求重新输入,因为一旦出现nextLine();就代表了一轮输入/输出已经结束;

