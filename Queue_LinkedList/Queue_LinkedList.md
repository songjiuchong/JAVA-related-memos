Queue_LinkedList

package prototype;

import java.util.LinkedList;
import java.util.Queue;

public class SongQueue_LinkedList {


	/*
	 Queue队列中可以存放任意数据,但是存取数据要严格遵守先进先出原则,存入的内容在队尾,取出的内容在队首;
	 
	 LinkedList因为更适合增删元素,根据这个特点,它更适合队列操作的要求;

	所以LinkedList即实现了List接口,也实现了Queue接口,我们可以把它当做一

	个队列去使用;
	 */
		
	public static void main(String[] args){
		//java.util.Queue
		Queue<String> queue=new LinkedList<String>();
		queue.offer("A");
		queue.offer("B");
		queue.offer("C");
		System.out.println(queue); //[A,B,C];
		System.out.println("队首元素："+queue.peek()); //peek方法绘只是观察队首元素,并不会取出删除;
		//取出队列中的每个元素：
		String str;
		while((str=queue.poll())!=null){
			System.out.println(str);
		}
	  }
	}
