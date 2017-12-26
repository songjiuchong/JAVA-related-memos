SkillPoints 2.0（工厂）

package testdemo;

import java.util.Scanner;

public class SongSkillPoints {                                    //客户端;
	
	static{                                                       //在运行程序前时就会运行的提醒语句;
		System.out.println("please ready to set your skill points in the order of Guard, Deception and Madness.");
		System.out.println("now input : ");
	}
	
	public static void main(String[] args) {                    //简洁的客户端运行程序，所有操作都交给工厂类实施;
		Allocation.SetSkillPoints();
	}                                                     
}
///////////////////////////////////////////////////////////////////////
interface SkillPoints{                                             
	int getPoints();                                         //三个具体实例必定会调用的关键方法;
	String getName();
	int getLeft();
}

/////////////////////////////////////////////////////////////////////////////////////////////////
class Allocation{                                                  //工厂模式中的工厂类;
	public static int sum;                                         //利用静态变量保存每一次剩下的skill points;
	public static void SetSkillPoints(){
		Allocation.sum=30;
		Scanner sc = new Scanner(System.in);
		System.out.println("Points invest in Guard : ");         //利用固定输入来避免客户的错误输入格式;
		int s1= sc.nextInt();
		Allocation.SkillPointsPrint(new Guard(s1));
		
		System.out.println("Points invest in Deception : ");         //利用固定输入来避免客户的错误输入格式;
		s1= sc.nextInt();
		Allocation.SkillPointsPrint(new Deception(s1));
		
		System.out.println("Points invest in Madness : ");         //利用固定输入来避免客户的错误输入格式;
		s1= sc.nextInt();
		Allocation.SkillPointsPrint(new Madness(s1));
	}
	
	public static void SkillPointsPrint(SkillPoints s){
		System.out.println("you have invested "+s.getPoints()+" points in the build "+s.getName());
		System.out.println("you have "+s.getLeft()+" Pointsleft!");     
	}
}

////////////////////////////////////////////////////////////////////////////
class Deception implements SkillPoints{                                 //设置Deception;
	private int points;
	private int left;
	private String name="Deception";
	public Deception(int points){                   //在构造方法中递归调用set方法来设置本build中的投入点数和当前剩余点数;
		this.setPoints(points);
	}
	
	public void setPoints(int points) {
		if(points<=Allocation.sum&&points>=0){
			Allocation.sum-=points;
			this.points=points;
		}else if(points>Allocation.sum){
			this.points=Allocation.sum;
			Allocation.sum-=this.points;
		}else{
			this.points=0;
			Allocation.sum-=this.points;
		}
		left=Allocation.sum;                               //把当前的剩余点数保存起来;
	}
	
	
	public int getPoints() {
		return this.points;
	}
	public String getName() {
		return this.name;
	}
	public  int getLeft(){
		return left;
	}
}
///////////////////////////////////////////////////////////////////////////////
class Guard implements SkillPoints{                                  //设置Guard;
	private int points;
	private  int left;
	private String name="Guard";
	public Guard(int points){
		this.setPoints(points);
	}
	
	public void setPoints(int points) {
		if(points<=Allocation.sum&&points>=0){
			Allocation.sum-=points;
			this.points=points;
		}else if(points>Allocation.sum){
			this.points=Allocation.sum;
			Allocation.sum-=this.points;
		}else{
			this.points=0;
			Allocation.sum-=this.points;
		}
		left=Allocation.sum;
	}
	
	
	public int getPoints() {
		return this.points;
	}
	public String getName() {
		return this.name;
	}
	public  int getLeft(){
		return left;
	}
}
////////////////////////////////////////////////////////////////////////////////
class Madness implements SkillPoints{                                        //设置Madness;
	private int points;
	private  int left;
	private String name="Madness";
	public Madness(int points){
		this.setPoints(points);
	}
	
	public void setPoints(int points) {
		if(points<=Allocation.sum&&points>=0){
			Allocation.sum-=points;
			this.points=points;
		}else if(points>Allocation.sum){
			this.points=Allocation.sum;
			Allocation.sum-=this.points;
		}else{
			this.points=0;
			Allocation.sum-=this.points;
		}
		left=Allocation.sum;
	}
	
	
	public int getPoints() {
		return this.points;
	}
	public String getName() {
		return this.name;
	}
	public int getLeft(){
		return left;
	}
}

