GuessNumber

package testdemo;

import java.util.Arrays;
import java.util.Random;
import java.util.Scanner;

public class GuessNumber {       //猜数字游戏:猜4位数字,4A0B则完全才对;A代表数字和位置都对,B代表仅是其中的一个数字,位置不对;
	static int[] randomx=new int[4];  //int数组保存生成的4位随机数字;
	static int[] getx = new int[4];   //保存用户输入的4位数字;
	static Random random = new Random();
	static Scanner console= new Scanner(System.in);
	static int times =0 ;
	
	//////////////////////////////////////////////////
	public static void generateAnswer(){
		int index = 0; 
		boolean[] used= new boolean[10];
		while(index<4){
			int i =random.nextInt(10);
			if(!used[i]){
				randomx[index]=i;
				index++;
				used[i]=true;
			}
		}
	}
	//////////////////////////////////////////////////////
	public static void getGuessNumber(){
		    loop1:
			while(true){
			System.out.println("please enter a 4-digit number without repetition to guess the answer,enter "+'\"'+"exit"+'\"'+" to quit!");
			String temps= console.nextLine().trim().toLowerCase();
			if(temps.equals("exit")){                     //退出选择;
				System.exit(1);
			}
			if(!(temps.length()==getx.length)){    //检测用户输入长度是否与游戏要求输入长度一致;
			System.out.println("error occoured! only 4-digit number is valid ! try again !");
			continue loop1;
			}
			
			try{                                  //检测是否用户输入都为数字;
				int temps2=Integer.parseInt(temps);
				}
			catch(NumberFormatException e){
				System.out.println("error occoured! only 4-digit number is valid ! try again !");
				continue loop1;
			}
			
			char[]temps3=new char[getx.length];
			for(int i=0;i<getx.length;i++){
				temps3[i]= temps.charAt(i);
			}
			for(int i=0;i<temps3.length;i++){
				for(int j=i;j<temps3.length-1;j++){
					if(temps3[i]==temps3[j+1]){
						System.out.println("error occoured! only 4-digit number WITHOUT repetition is valid ! try again !");
						continue loop1;
					}
				}
				getx[i]=temps3[i]-'0'; //通过减去'0'的操作来把char数组中的数字char元素转换为int变量;
			}
			times++;
			System.out.println("the number u have guessed is: "+Arrays.toString(getx));
			checkAnswer();            
			}
	}
//			times++;                    这段程序由于过于繁琐而且兼容性不强,如果游戏中猜数字的长度改变,将不便于维护和修改;
//			for(int i =0;i<temps.length();i++){
//			char ch = temps.charAt(i);
//			if(ch<='9'&&ch>='0'){
//			}else{
//				times--;                    //输入格式错误不算在计次之内;
//				continue  loop1;
//			}
//			}
//			char ch0 = temps.charAt(0);     //利用char'0'-'9'从小到大顺序排列的规律和向基本类型转换，成功把char数字转换为int数字;
//			int n1=ch0-'0';
//			char ch1 = temps.charAt(1);
//			int n2=ch1-'0';
//			char ch2 = temps.charAt(2);
//			int n3=ch2-'0';
//			char ch3 = temps.charAt(3);
//			int n4=ch3-'0';
			/////////////////////////////////////////////////
//			int temp =Integer.parseInt(temps);           //利用数学运算把4位数拆成4个一位整数;
//			int n1=temp/1000;
//			int n2=(temp-1000*n1)/100;
//			int n3=(temp-1000*n1-100*n2)/10;
//			int n4= temp%10;
			////////////////////////////////////////////////
//			if(!(temps.length()!=4||n1==n2||n1==n3||n1==n4||n2==n3||n2==n4||n3==n4)){
//				getx[0]=n1;
//				getx[1]=n2;
//				getx[2]=n3;
//				getx[3]=n4;
//			System.out.println("the number u have guessed is: "+n1+n2+n3+n4);
//			checkAnswer();                               //每一次输入格式正确就能进入checkAnswer();中接受检查;
//	        continue loop1;
//			}
//			times--;                                     //输入格式错误不算在计次之内;
//			System.out.println("error occurred!");
//			break;
//		}
//	}
/////////////////////////////////////////////////////////////////////////////////////////////
public static void checkAnswer(){
	int a=0;
	int b=0;
	for(int i=0;i<getx.length;i++){                 //比较仅数字对的个数;
		for(int y=0;y<randomx.length;y++){
			if(getx[i]==randomx[y]){
				b++;
				if(i==y){                               //再次比较是否位置也正确;
					b--;
					a++;
				}
				continue;
			}
		}
	}
	if(a==4){                                      //全对
	System.out.println("哇！传说！仅用了"+times+"次！");
	System.exit(0);
	}else{                                         //继续猜，递归调用getGuessNumber()
	System.out.println("结果： "+a+"A"+b+"B");
	}
}
////////////////////////////////////////////////////////////////////////////////////////////
	public static void main(String[] args) {
		generateAnswer();
		for(int i =0;i<getx.length;i++){
		System.out.println(randomx[i]);
	}
		getGuessNumber();
	}
}
