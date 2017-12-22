BreakContinueRetuen

package prototype;

public class SongBreakContinueRetuen {
	public static void main(String[] args) {
		int a = 10;
		//int b = 15;
		c:
		for(;a<20;a++){
			//c:
			for(int b =15;b<25;){
			System.out.println("" + a + b++);
			//if(b==16){
			//continue;
			//}
			//System.out.print("b=16时，这条语句因为continue不会被执行");//continue需要有前提条件，直接放在循环语句中无意义
			continue c; //直接跳至标签循环出继续进行
			//}
			//break c;  // 直接结束标签代表的循环并继续执行此循环外的其他部分
			//return;  //直接结束整个方法
		}
		}
		System.out.print("这个语句在执行RETURN的情况下不会运行！");
	}
}
