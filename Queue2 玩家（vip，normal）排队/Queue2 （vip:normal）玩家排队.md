Queue2 （vip/normal）玩家排队

package testdemo;

import java.util.*;


public class Queue2 {    //利用list集合的有序性和可扩展性,设计了一个排队系统,(同一个ID被视为同一名玩家,无法排队多次)VIP玩家排在队伍最前列的VIP玩家之后,
	                     //ID相同的玩家不会被排第二次,最后显示整个队列;
	public static void main(String[] args) {
		aboutQueue1.initializeQueue1(new Playerxx(145521,"goska",false));
		aboutQueue1.initializeQueue1(new Playerxx(142351,"oejnk",false));
		aboutQueue1.initializeQueue1(new Playerxx(141331,"qqqpee",false));
		aboutQueue1.initializeQueue1(new Playerxx(142521,"uaskja",false));
		aboutQueue1.initializeQueue1(new Playerxx(142521,"uaskja",false));
		aboutQueue1.initializeQueue1(new Playerxx(142691,"gakja",true));
		aboutQueue1.initializeQueue1(new Playerxx(173331,"kjlakja",false));
		aboutQueue1.initializeQueue1(new Playerxx(173335,"343523",true));
		aboutQueue1.initializeQueue1(new Playerxx(142691,"gakja",true));
		aboutQueue1.initializeQueue1(new Playerxx(111111,"song",true));
		aboutQueue1.printQueue1();
	}
}

///////////////////////////////////////////////////////////////////////////////////
class Playerxx{
	private int id;
	private String name;
	private boolean Vip;
	private int no;  //位于第几个;
	static int count; //记录排队总人数;
	
	public boolean isVip() {
		return Vip;
	}
	public void setVip(boolean vip) {
		Vip = vip;
	}

	public Playerxx(int id, String name,boolean vip) {
		//super();
		this.setId(id);
		this.setName(name) ;
		this.setVip(vip);
	}
	public int getNo() {
		return no;
	}
	public void setNo(int no) {
		this.no = no;
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	/////////////////////////////////////////////////////////////////////////////////////////
	public boolean equals(Object obj){//不要忘了这里是list,加入新元素并不会自动利用.equals()方法来判断元素是否相同,
		                              //因为list支持相同元素的存在;
		if(obj==null){
			return false;
		}
		else if(obj instanceof Playerxx){
			Playerxx p =(Playerxx)obj;
			if(this.id==p.id){
				System.out.println(this.id+":"+this.name+", you are already in the queue1");
				return true;
			}else{
				return false;
			}
	}else{
		return false;
	}	
	}
	////////////////////////////////////////////////////////////////////////////////////
//		public int hashCode(){        //由于此程序仅利用Playerxx类对象的ID进行比较,而不会比较对象的hashcode,所以无需覆写hashCode方法;
//			int type=this.getClass().hashCode();
//			return type*43+this.id;
//	}
	////////////////////////////////////////////////////////
		public String toString(){                                   //覆写toString;
			String vip;
			if(this.Vip==true){
			vip="（VIP玩家）";}else{
			vip="（普通玩家）";
			}
			return "id:"+this.id+"的玩家： "+this.name+vip+ "位于第"+this.no+"位,总人数为: "+this.count;
		}
	////////////////////////////////////////////////////////
	
	}
class aboutQueue1{
	
	static List<Playerxx> queue1=new ArrayList<Playerxx>();
	
	public static boolean checkSame(Playerxx p){         
		//为了检查将要新加入的元素是否和list中已存在的元素拥有相同的id,这里利用了for each遍历和equals()的组合实现检查,返回boolean;
		//这里需要特别注意的是,之所以队列中在无元素的情况下利用for each和eqauls语句不会出现空指向异常的原因是:当实例化一个集合类型的对象,
		//在未指定任何元素放入其中之前,这个集合中并不存在任何的内容;而不像如:整数类型/引用类型的数组,它们在实例化了对象之后会分配相应的内存
		//空间并初始化其中的元素,整数类型初始化为0,引用类型初始化为null;而集合类型的对象由于无法在放入元素之前确定元素的个数,也就无法确定需要
		//开辟的空间大小,所以不存在这样的初始化工作;所以说在实际运行时,当运行到了for(Object player:aboutQueue1.queue1)这条语句时,如果集合中
		//根本就不存在元素,那么这条语句就在for条件判断时就被认为执行完毕了,根本不会再去执行语句块中的内容,会跳过并继续下面的语句;
		//假设这里遍历的不是一个空集合,而是一个空的数组,那么必定会发生空指向异常;
		
		boolean c=false;
		for(Object player:aboutQueue1.queue1){
			if(player.equals(p)){
				c=true;
				break;
			}else{
			}
		}
		return c;
	}

	public static void initializeQueue1(Playerxx p){      //客户端添加元素所使用的方法,包含了检查是否VIP,是否重复元素,
		                                                  //把元素加入list等操作,属于一个枢纽;
		int vipnumber=0;                                  //记录队列中已有的VIP玩家数量;
		if(checkSame(p)){                                 //检查ID相同,也就是重排的现象;
			
		}else if(p.isVip()==true){
			for(Playerxx pp:queue1){    //这里不用进行instance of 检查是因为在声明ArrayList的时候已经用泛型限制了对象的加入条件;
				if(pp.isVip()){        //历遍检查队伍中VIP的数量;
					vipnumber++;
				}
			}
			queue1.add(vipnumber,p);
			p.setNo(vipnumber+1);
			p.count+=1;
			//由于vip玩家的"插队",使得所有普通玩家的队位后移一位;
			for(Playerxx pp:queue1){
				if(!pp.isVip()){
					pp.setNo(pp.getNo()+1);
				}
			}
		}else{
			queue1.add(p);
			p.setNo(++p.count);
		}
}
	public static void printQueue1(){  //调用for each来打印list集合内容,这里会根据Playerxx类中覆写的toString方法显示,
		for(Object obj:queue1){		   //因为这里相当于向上转型后的动态链接;
			System.out.println(obj);   //注意:这里不用担心如果System.out.println()中的参数是Playerxx的对象,但是这个对象如果是null的情况,
			 						   //会不会导致调用Playerxx类中toString()方法是产生空指向异常;因为当System.out.println()参数为非空对象时
									   //JVM才会自动调用这个对象的toString方法,为null的对象就显示null;   
		}
	}
}


