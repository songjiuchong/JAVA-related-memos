Knights

package testdemo;

public class KnightsDemo {       //简单工厂模式，实现客户端输入5个以内设定好的3中职业并显示详细信息,输入格式错误或超过数量,报错;
	public static void main(String[] args) {
		Gm.setKnight("Warrior", "xiao1");
		Gm.setKnight("Warrior", "xiao2");
		Gm.setKnight("Mage", "xiao3");
		Gm.setKnight("Mage", "xiao4");
		Gm.setKnight("Ro", "xiao5");
		Gm.setKnight("Ro1", "xiao5");
		Gm.setKnight("Rogue", "xiao5");
		Gm.setKnight("Rogue", "xiao6");
		System.out.println();
		Gm.showTeam();
		
	}
}

////////////////////////////////////////////////////////////////////////////

interface Knights{
	int getHp();
	int getMp();
	int getAtk();
	String getOccupation();
	String getName();
}

/////////////////////////////////////////////////////////////////////////////

class Mage implements Knights{
	private String occupation="Mage   ";  //为了输出职业后,名字的显示能够左对齐,这里补空格;
	public String getOccupation() {
		return this.occupation;
	}
	private int hp;
	private int mp;
	private int atk;
	private String name;
	public int getHp() {
		return hp;
	}
	public int getMp() {
		return mp;
	}
	public int getAtk() {
		return atk;
	}
	public String getName() {
		return name;
	}
	public Mage(String name){
		this.name=name;
		this.hp=10;
		this.mp=10;
		this.atk=1;
		
	}
}
///////////////////////////////////////////////////////////////////////////////////

class Warrior implements Knights{
	private String occupation="Warrior";
	public String getOccupation() {
		return this.occupation;
	}
	private int hp;
	private int mp;
	private int atk;
	private String name;
	public int getHp() {
		return hp;
	}
	public int getMp() {
		return mp;
	}
	public int getAtk() {
		return atk;
	}
	public String getName() {
		return name;
	}
	public Warrior(String name){
		this.name=name;
		this.hp=20;
		this.mp=5;
		this.atk=3;
		
	}
}
//////////////////////////////////////////////////////////////////////////

class Rogue implements Knights{
	private String occupation="Rogue  ";  //为了输出职业后,名字的显示能够左对齐,这里补空格;
	public String getOccupation() {
		return this.occupation;
	}
	private int hp;
	private int mp;
	private int atk;
	private String name;
	public int getHp() {
		return hp;
	}
	public int getMp() {
		return mp;
	}
	public int getAtk() {
		return atk;
	}
	public String getName() {
		return name;
	}
	public Rogue(String name){
		this.name=name;
		this.hp=15;
		this.mp=6;
		this.atk=5;
		
	}
}
/////////////////////////////////////////////////////////////////

class Gm{
	static int c;  //记录正确语句的次数;
	static int r;  //记录错误语句出现的次数;
	static Knights[]team=new Knights[5];
	public static void setKnight(String occupation,String name){
		if(occupation.equals("Mage")&&c<=4){
			team[c]=new Mage(name);
			c++;
		}else if(occupation.equals("Warrior")&&c<=4){
			team[c]=new Warrior(name);
			c++;
		}else if(occupation.equals("Rogue")&&c<=4){
			team[c]=new Rogue(name);
			c++;
		}else if((c<=4)){
			r++;
			System.out.println("Warning! "+ '"' +occupation+ '"' + " can not be recognized as a given occupation, no."+(r+c) + " command has failed to be inserted into the list !");
		}else{
			r++;
			System.out.println("Warning! The team has full,only 5 ppl can be in the team,no."+(r+c)+" command has failed to be inserted into the list !");
		}
	}
	
	public static void showTeam(){
		for(int i =0;i<Gm.team.length;i++){
			if(team[i]!=null){
			System.out.println("Member"+(i+1)+ ": "+team[i].getOccupation()+'\t'+team[i].getName()+ " stats: "+ "HP: "+
					team[i].getHp()+" MP:"+team[i].getMp()+" ATTACK: "+team[i].getAtk());
			}else{
			System.out.println(" member"+(i+1)+ ": null!");	
			}
		}
	}
}

