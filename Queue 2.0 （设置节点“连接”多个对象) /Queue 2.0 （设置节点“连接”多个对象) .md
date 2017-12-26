Queue 2.0 （设置节点“连接”多个对象) 

package prototype;

import testdemo.SongDemo1;

public class SongQueue{
	public static void main(String args[]){
		Node p1 = new Node("法师",10);
		Node p2 = new Node("法师2",11);
		Node p3 = new Node("法师3",12);
		p1.next=p2;
		p2.next=p3;
		p1.print();
		p1.printAlternative(); //与print();输出相同;
		}
	}
class Node{
	public Node next;
	String name;
	int a;
	int count1=0;
	static int count=0;                         
	public Node(String name,int a ){
		count++;                                //利用静态参数count保存每一次的增大;
		this.name=name;
		this.a=a;
		this.count1=count;                      //利用静态参数count每一次的变化把一次增大的值赋给每一个count1;
	}
	public void print(){                        //利用递归调用来实现循环输出;
	System.out.println(this.name+'\t'+this.a+"你排在第"+this.count1+"位");
	if(this.next!=null){
		this.next.print();
	}else{
		System.out.println("排队总人数为："+count);
	}
}
	public void printAlternative(){	          //利用do while实现同样的循环输出效果;
		Node temp=this;
		do{
			System.out.println(temp.name+'\t'+temp.a+"你排在第"+temp.count1+"位");
		if(temp.next!=null){
			temp=temp.next;
		}else{
			temp=null;
			System.out.println("排队总人数为："+count);
		}
		}
		while(temp!=null);
	}
	
}


