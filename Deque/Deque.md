Deque

package prototype;

	import java.util.Deque;
	import java.util.LinkedList;


	public class SongDeque {
	
	 /*
	  
	Deque是Queue的子接口,定义了所谓"双端队列"即从队列的两端分别可以入队(offer)和
	出队(poll);LinkedList实现了该接口;
	
	如果将Deque限制为只能从一端入队和出队,则可实现栈的数据结构,对于栈而言入栈(push),
	出栈为(pop);
	
	栈结构：存取数据遵循先进后出原则;
	
	*/
	
	public static void main(String[] args){
		//java.util.Deque
		Deque<String> deque=new LinkedList<String>();
		deque.push("A"); //在栈中压入新元素;
		deque.push("B");
		deque.push("C");
		deque.push("D");
		deque.push("E");
		System.out.println(deque); //[E,D,C,B,A]; 调用toString()方法显示队列时,就是按照其取出顺序来实现的,所以这里和下面while语句里
								   //里元素的输出顺序是一致的;
		
		while(deque.peek()!=null){ //此处不能使用deque.pop()!=null是由于与队列不同,当栈中已无元素无法再调用pop弹出内容时,会报错;
			System.out.println(deque.pop());//E D C B A;
		}
		
	//注意:
	//当使用Deque这个双端队列时,需要注意的是它的模型;把它想象成一个直筒,前段是用来输出其中元素的,后端是用来加入元素的,那么当把Deque当成
	//Queue来使用,也就是完全利用offer和poll方法时,它的运行机制仍旧是从队尾加入元素,从队首取出元素,遵循先进先出的原则,所以对于Queue来说
	//它的两端不可以同时担任入队和出队的工作;而Deque,当完全使用push/pop方法来存放/取出元素时,它的运作机制是从队首放入元素,取出元素也是
	//从队首取出,实现了先进后出的原则,下面是一个例子:
	
		Deque<String> deque2=new LinkedList<String>();
		deque2.offer("A"); //在栈中压入新元素;
		deque2.offer("B");
		deque2.push("C");
		deque2.push("D");
		deque2.offer("E");
		System.out.println(deque2); //[D, C, A, B, E];
//		while(deque2.peek()!=null){ 
//			System.out.println(deque2.poll());//D  C  A  B  E;
//		}
		String x = null;
		while((x=deque2.poll())!=null){ 
			System.out.println(x);//D  C  A  B  E;
		}
//		while((x=deque2.pop())!=null){ 
//			System.out.println(x); //java.util.NoSuchElementException;
//		}
		
	//输出原因分析:
	//deque2.offer("A"); 这这条语句将"A"从队尾放入;接下来同理,"B"被从队尾放入;而deque2.push("C");这条语句将"C"从队首放入,同理
	//"D"被从队首放入;最后一条语句将"E"从队尾放入;而输出时无论用pop还是poll都是将元素一个一个的从队首拿出,所以结果是:D C A B E;
		
	}
}

	
