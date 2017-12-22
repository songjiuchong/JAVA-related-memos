ExtendsPaternal

package prototype;

public class SongExtendsPaternal {

}
class Republic{
	private String name="";                        //一般顺序为：类的属性；构造；方法
	private int lvl ;
	private String profession="";
	public Republic(){
		System.out.println("调用了Republic()构造器");
	}
	public Republic(String name,int lvl,String profession){
		this.setName(name);
		this.setLvl(lvl);
		this.setProfession(profession);
		System.out.println("调用了Republic(name,lvl,profession)构造器");
	}

	public void setName(String name){
		this.name=name;
	}
	public String getName(){
		return name;
	}
	public void setLvl(int lvl){
		this.lvl=lvl;
	}
	public int getLvl(){
		return lvl;
	}
	public void setProfession(String profession){
		this.profession=profession;
	}
	public String getProfession(){
		return profession;
	}
	public int showStat(){                                    //父类的方法
		System.out.println(name+lvl+profession);
		return 0;
	}
}