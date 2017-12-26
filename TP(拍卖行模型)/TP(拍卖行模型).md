TP(拍卖行模型)

package prototype;


//由宠物店模型引申而来的拍卖行demo！实现添加物品和搜索物品2种主要功能！


//客户端;
public class SongTP {       
	
	private static TP database;
	static{
		database = new TP(50);
	}
	
	public static void main(String[] args) {
		database.addItems(new Sword("日出",100,1000));
		database.addItems(new Sword("暮光",100,1000));
		database.addItems(new Sword("冰龙剑",90,900));
		database.addItems(new Sword("Bolt",50,600));
		database.addItems(new Sword("霜之哀伤",100,1000));
		database.addItems(new Hammer("雷神之锤",100,1000));
		database.addItems(new Potion("圣水",10,100));
		database.addItems(new Potion("venom",10,100));
		database.addItems(new Bag("格鲁尔之袋",30,90));
		
		database.printAll();
		
		database.searchItem("o");
		
	}
}

///////////////////////////////////////////////////////

//物品总接口;
interface Item{
	String getName();
	int getPrice();
}
/////////////////////////////////////////////////////

//武器子接口;
interface Weapon extends Item{
	float getAtk();
}

//消耗品子接口;
interface Consumable extends Item{
	int getLvl();
}
//////////////////////////////////////////////////////

//具体物品类实现接口;
class Sword implements Weapon{
	private String name;
	private int price;
	private float atk;
	
	public Sword(String name,int price,float atk){
		this.setName(name);
		this.setPrice(price);
		this.setAtk(atk);
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getPrice() {
		return price;
	}
	public void setPrice(int price) {
		this.price = price;
	}
	public float getAtk() {
		return atk;
	}
	public void setAtk(float atk) {
		this.atk = atk;
	}
}

class Hammer implements Weapon{
	private String name;
	private int price;
	private float atk;
	
	public Hammer(String name,int price,float atk){
		this.setName(name);
		this.setPrice(price);
		this.setAtk(atk);
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getPrice() {
		return price;
	}
	public void setPrice(int price) {
		this.price = price;
	}
	public float getAtk() {
		return atk;
	}
	public void setAtk(float atk) {
		this.atk = atk;
	}
}

class Potion implements Consumable{
	private String name;
	private int price;
	private int lvl;
	
	public Potion(String name,int price,int lvl){
		this.setName(name);
		this.setPrice(price);
		this.setLvl(lvl);
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getPrice() {
		return price;
	}
	public void setPrice(int price) {
		this.price = price;
	}
	public int getLvl() {
		return lvl;
	}
	public void setLvl(int lvl) {
		this.lvl = lvl;
	}
}

class Bag implements Consumable{
	private String name;
	private int price;
	private int lvl;
	
	public Bag(String name,int price,int lvl){
		this.setName(name);
		this.setPrice(price);
		this.setLvl(lvl);
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getPrice() {
		return price;
	}
	public void setPrice(int price) {
		this.price = price;
	}
	public int getLvl() {
		return lvl;
	}
	public void setLvl(int lvl) {
		this.lvl = lvl;
	}
}

//////////////////////////////////////////////////////

//工厂类,实现各种功能;
class TP{
	private Item[] items; //已放入TP的物品数组：
	int foot=0; //待放置的物品角标;
	
	//TP构造方法限定了可放入物品数量;
	public TP(int len){
		if(len>0){
			this.items=new Item[len];
		}else{
			System.out.println("TP is closed for now, check later !");
			System.exit(-1);
		}
	}
	////////////////////////////////////////////////////////////
	public Item[] getItems(){
		return this.items;
	}
	////////////////////////////////////////////////////
	
	//添加物品功能;
	public void addItems(Item item){
		if(foot<this.items.length){
			this.items[foot]=item;
			foot++;
			System.out.println("A new item is added to TP!");
			}else{
				System.out.println("TP is currently full,try later!");
			}
			
		}
	
	////////////////////////////////////////////////////////
	
	//根据关键字搜索TP中的所有符合条件的物品并显示;
	public void searchItem(String keyWord){
		Item[] temp =null;
		int sum = 0;
		for(int i=0;i<this.items.length;i++){
			if(this.items[i]!=null&&this.items[i].getName().indexOf(keyWord)!=-1){
				sum++;
			}
		}
		temp=new Item[sum];
		int f= 0 ;
		for(int i=0;i<this.items.length;i++){
			if(this.items[i]!=null&&this.items[i].getName().indexOf(keyWord)!=-1){
				temp[f]=this.items[i];
				f++;
			}
		  }
		for(Item i : temp){
			System.out.println("Item found: "+ i.getName()+ " cost: "+i.getPrice());
		}
		}
	
	
	//显示TP中所有物品的详细信息;
	public void printAll(){
		int spot=items.length;
		for(int i=0;i<items.length;i++){
			if(items[i]==null){
				continue;
			}
			if(items[i] instanceof Weapon){
			Weapon w = (Weapon)items[i];
			System.out.println(items[i].getName()+'\t'+items[i].getPrice()+'\t'+w.getAtk());
			spot-=1;
			continue;
			}
			if(items[i] instanceof Consumable){
			Consumable c= (Consumable)items[i];
			System.out.println(items[i].getName()+'\t'+items[i].getPrice()+'\t'+c.getLvl());
			spot-=1;
			}
			}
		System.out.println(spot+" slots available now!");
		
		}
	}


