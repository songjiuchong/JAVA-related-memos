Extends

package prototype;

public class SongExtends extends Republic{
	int credits;
	public SongExtends(){
	    super();                                //编译器在编译的时候会默认加上这条语句，编程者可以省略
	    System.out.println("调用了SongExtends()构造器");
	}
	public SongExtends(String name,int lvl,String profession,int credits){
		super(name,lvl,profession);             //用super()来调用父类的构造器，达到初始化的目的，但是这个构造语句必须放在第一行
		this.credits=credits;
		System.out.println("调用了SongExtends(name,lvl,profession,credits)构造器");
	}
	public int showStat(){                                          //重写/覆盖父类的方法，满足条件：1.名字相同，不然就是新方法；
		                                                            //2.参数列表相同，不然也是新的方法，属于方法的重载，而不是覆盖
		                                                            //3.返回值类型相同（ JDK5.0以后可以返回值类型不同） 
		System.out.println(getName()+getLvl()+getProfession()+this.credits);
		return 0;
	}
	public static void main(String[] args) {
		SongExtends guardian = new SongExtends();
		guardian.toString();                                                      //调用”父类的父类“--object类的方法
		guardian.setName("therapists");
		System.out.println(guardian.getName());                                   //成功调用父类方法
		SongExtends guardian2 = new SongExtends("exorcist",45,"guardian",10000);  //调用具有混合参数的构造方法  
		/*以下是运行结果：
		 调用了Republic()构造器                                                                          （先递归调用了父类的无参构造器）
                      调用了SongExtends()构造器                                                                  （再调用自己的无参构造器）
         therapists                                       （成功调用父类方法）
                      调用了Republic(name,lvl,profession)构造器                        （同上）
                      调用了SongExtends(name,lvl,profession,credits)构造器
		 */
		guardian2.showStat();                             //成功运行重写了的父类的方法
	}
}
class Guardian extends Republic{        //代表Guardian 类是Republic类的子类，逻辑上满足条件“Guardian is a Republic”
	                                    //且JAVA中类和类是单继承关系,继承的是类里的属性和方法（构造不能继承，私有方法不能继承）
	                                    //子类会默认调用父类无参构造，创建实例时会递归分配父类空间
	private int force=100;
	public void setForce(int force){
		this.force=force;
	}
	public int getForce(){
		return force;
	}
}
