Static


package prototype;

class Player{
	String name="";
	private static int lvl = 1;
	private static int count = 0;   //计算有几个对象调用过count参数;
	public Player(String name,int lvl){
		this.name=name;
		this.setLvl(lvl);
		count++;                    //计算有几个对象调用过count参数(同上);
		System.out.println("这是第"+count+"个用Player类实例化的对象");   //计算一共有几个对象被实例化了！
	}
	public static void setLvl(int lvl){
		Player.lvl=lvl;
		/*这里不能用this.参数因为lvl此时已经属于类而不是一个对象;而且如果声明一个静态方法，则其中参数或方法一定要为静态,反之则不必要*/
	}
	public static int getLvl(){
		return lvl;
	}
	
	
}

public class SongStatic {
	public static void main(String[] args) {
		Player p1 = new Player("玩家1",10);
		System.out.println(p1.name+'\t'+p1.getLvl());
		Player p2 = new Player("玩家2",11);
		System.out.println(p2.name+'\t'+p2.getLvl());
		Player p3 = new Player("玩家3",12);
		System.out.println(p3.name+'\t'+p3.getLvl());
		////////////////////////////////////////////////////////////////
		System.out.println(p1.name+'\t'+p1.getLvl());
		System.out.println(p2.name+'\t'+p2.getLvl());
		System.out.println(p3.name+'\t'+p3.getLvl());   //这里因为p3修改lvl为12，则p1,p2,p3lvl都变为12;
		
		Player.setLvl(0);
		/*因为static不需要先实例化对象再调用方法或属性，他属于一个类而不是个体，对静态参数的修改将会作用于每一个调用过此参数的对象，一劳永逸，不用逐个修改，因为他们指向同一个全局数据区,而不是存储局部变量的堆栈内存中（方法保存在全局代码区中） ;
		其实也就是说静态变量/方法都在类加载时就被初始化/保存在了方法区中,而对象在其堆内存中存有指向方法区中其类信息的地址,从而能够访问/调用->静态变量/方法;*/
		
		System.out.println(p1.name+'\t'+p1.getLvl());
		System.out.println(p2.name+'\t'+p2.getLvl());
		System.out.println(p3.name+'\t'+p3.getLvl());
	}
}


