Switch 

package prototype;

public class SongSwitch {
	public static void main(String args []){
		char a = 'A';
		switch(a){ //switch括号中的表达式类型只能是byte,short,char ,int, Enum ! 
		case 'A':
			System.out.println("优秀");
			break;
		case 'B':
			System.out.println("良好");
			break;
		case 'C':
			System.out.println("一般");
			break;
		default:
			System.out.println("及格");
		}
/////////////////////////////////////还有一些技巧需要注意：///////////////////////////////////////////////
//例1：
int i = 0;
switch (i) {
  default:
    System.out.println("default");
  case 0:
    System.out.println("0");
  case 1:
    System.out.println("1");
  case 2:
    System.out.println("2");
    break;
  case 3:
    System.out.println("3");
  case 4:
    System.out.println("4");
    break;
    //这段语句输出的结果是“1,2”说明无论何时default总是放在最后验证，代表其他条件都不满足了再来用它，当找到满足条件的项，会一直运行
    //到break或代码段结束为止
}
//例2：
int x = 8;
switch (x) {
  default:
    System.out.println("default");
  case 0:
    System.out.println("0");
  case 1:
    System.out.println("1");
  case 2:
    System.out.println("2");
    break;
  case 3:
    System.out.println("3");
  case 4:
    System.out.println("4");
    break;
    //运行结果是：default,0,1,2 说明在未找到x参数对应的case时，才到default，然后从default运行到break为止
}
}
}
