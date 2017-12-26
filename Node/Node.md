Node

package prototype;

class Node1{                                        
	private String data;                  //保存每一个节点的数据
	private Node1 next;                   //用next来保存指向下一个节点的地址，这种“类.引用类型变量”名称的声明方法可以用来保存地址
	
	public Node1(String data){
		this.data=data;
	}
	public void setNext(Node1 next){
		this.next=next;
	}
	public Node1 getNext(){
		return this.next;
	}
	public String getData(){
		return this.data;
	}
}

public class SongNode {
	public static void main(String[] args) {
		Node1 p1 = new Node1("法师");
		Node1 p2 = new Node1("战士");
		Node1 p3 = new Node1("猎人");
		Node1 p4 = new Node1("死亡骑士");
		p1.setNext(p2);
		p2.setNext(p3);
		p3.setNext(p4);
		dataPrint(p1);                        //给予一个初节点数据
	}
	public static void dataPrint(Node1 node){
		System.out.println(node.getData());   //打印初节点
		if(node.getNext()!=null){             //判断当前节点是否存在下一个需要打印的节点
			dataPrint(node.getNext());        //递归调用方法，指向下一个节点地址
		}
	}
		
	}

