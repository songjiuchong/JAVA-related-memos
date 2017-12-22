BreakContinueWithNote


package prototype;

public class SongBreakContinueWithNote {          //演示带标注的break和continue的用法:
	                                              //Java 中有 goto 关键字，但这个关键字没有任何作用，换句话说，
	                                              //我们不能使用 goto 来进行跳转到某行。Java 的标签只能定义在三种循环:
	                                              //for(){},do{}while(),while(){})的开始位置，否则编译器会报告说找不到标签。
	public static void main(String[] args) {
		loop1:
			while(true){
			System.out.println("while!!");
			for(int i=0;i<100;i++){
				System.out.println(i);
				if(i==10){
					break loop1;                  //这里的break loop1;起到的作用是一旦i=10，则跳出while循环
				}
			}
				System.out.println("after for!!");
				break;
			}
///////////////////////////////////////////////////////////////////////////////
		while(true){
		System.out.println("while!!");
		for(int i=0;i<100;i++){
			System.out.println(i);
			if(i==10){
				break;                          //这里的break;起到的作用是一旦i=10，则跳出当前循环，也就是for循环
			}
		}
			System.out.println("after for!!");
			break;
		}
//////////////////////////////////////////////////////////////////////////////
	loop1:
		for(int a =0;a<2;a++){
		System.out.println("a= "+a);
		for(int b=0;b<10;b++){
			if(b==5){
				continue loop1;                  //这里的continue loop1;起到的作用是一旦i=5，则结束当前的内部和外部for循环，接着
				                                 //从外部的下一次for循环继续
			}
			System.out.println("b= "+b);
		}
	}
/////////////////////////////////////////////////////////////////////////////
		for(int a =0;a<2;a++){
		System.out.println("a= "+a);
		for(int b=0;b<10;b++){
			if(b==5){
				continue;                  //这里的continue;起到的作用是一旦i=5，则结束当前的内部for循环，接着
				                           //从内部的下一次for循环继续
			}
			System.out.println("b= "+b);
		}
		}
////////////////////////////////////////////////////////////////////////////////
		int a = 1;
		c:
			do{
				a++;
				if(a==5){
					continue c;
				}
				System.out.println(a);
			}
			while(a<10);
	
	}
}
