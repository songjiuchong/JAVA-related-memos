CrackClient&CodeConfirmation


package testdemo;

import java.util.Scanner;

public class CrackClient {                           //用户密码验证客户端
	public static void main(String[] args) {
		int number;
		Scanner console = new Scanner(System.in);
		while(true){
			System.out.println("please imput 6-digit code to enter your account, type in exit to quit the system!");
			
/////////////////////////////////////////////////////////
			do{
			number=(int)(Math.random()*1000000);     //利用随机数循环尝试替代用户输入破解密码
			}
			while(number<100000);
			String temp=new Integer(number).toString();
		    System.out.println(temp);
//////////////////////////////////////////////////////
			
          //原前台程序：			
//          String temp= console.nextLine();
//			if("exit".equals(temp)){
//				System.out.println("exit!");
//				break;
//			}
//			if(temp.length()!=6){
//				System.out.print("the way u enter ur code is wrong,please try again!");
//				continue;
//			}
//			try{                                   //利用捕获异常来验证用户输入是否全是数字类型的String
//				number=Integer.parseInt(temp);
//			}
//			catch(NumberFormatException e){
//				System.out.println("the way u enter ur code is wrong,please try again!!");
//				continue;
//			}
		    
			CodeConfirmation tempclient=new CodeConfirmation();
			if(tempclient.confirmation(number)){
				System.out.println("welcome!");
				tempclient.access();            //正式进入后台操作程序;
				break;
			}else{
				System.out.println("password is wrong!");
			}
		}
	}
}

//////////////////////////////////////////////////////////////////////////
/* hack:
			do{
			number=(int)(Math.random()*1000000);     //利用随机数循环尝试替代用户输入破解密码
			}
			while(number<100000);
			String temp=new Integer(number).toString();
		    System.out.println(temp);
	     历时2-3分钟,最快时15s;
      586843
      welcome!
      u are access to your personal account !
  
 */









package testdemo;

public class CodeConfirmation {  //CrackClient 的后台验证密码程序
private int accessCode=586843;
private boolean legit=false;

public boolean confirmation(int number){
	if(number==accessCode){
		legit=true;
		return legit;
}else{
	legit=false;
	return legit;
}
}
public void access(){
	if(legit){
		System.out.println("u are access to your personal account !");
		//进入...
	}
}

}

