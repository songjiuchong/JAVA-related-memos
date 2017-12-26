SaftyGamer（Map 运用）

package testdemo;
import java.util.*;
public class SaftyGamer {                             //创建新的id，自动绑定一个随机的6位流水号，并且增加了通过id查询流水号功能
	public static void main(String[] args) {
		SaftyProcess p1 =new SaftyProcess("eijfiee"); //在引用SaftyProcess构造实例化对象的同时，一个随机流水号已经和id绑定放入映射
		SaftyProcess p2 =new SaftyProcess("asdsdww");
		SaftyProcess p3 =new SaftyProcess("eisajkde");
		SaftyProcess p4 =new SaftyProcess("gakgjs");
		SaftyProcess p5 =new SaftyProcess("roerf");
		SaftyProcess p6 =new SaftyProcess("asdsdww"); //与p1重复的id初始化对象;
		SaftyProcess.innerIndex("roerf");          //通过id来显示此id建立时分配的随机流水号
		System.out.println(p5);                    //为了证明innerIndex方法确实返回的是随机流水号，这里输出id所对应的对象检查
		SaftyProcess.innerIndex("asdsdww");        //此处查询的是HashMap中的此id的value,会按照这个id中最后输入的流水号来保存;
		System.out.println(p2);					   //p2在保存id和流水号时是此id的第一个流水号;
		System.out.println(p6);                    //p6在保存id和流水号时是此id生成的第二个流水号;是最后输入的流水号所以也是查询的结果;
	}
}

class SaftyProcess{
	private String id;
	private int code;
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public int getCode() {
		return code;
	}
	public void setCode(int code) {
		this.code = code;
	}
	public Map getIdbank() {
		return idbank;
	}
	public void setIdbank(Map idbank) {
		this.idbank = idbank;
	}
	/////////////////////////////////////////////
	public boolean equals(Object obj){              //重写了equals方法，使得相同只会存在一个,且它的最新流水号会覆盖之前保存的流水号;
		if(obj==null){
			return false;
		}else if(obj instanceof SaftyProcess){
			SaftyProcess temp=(SaftyProcess) obj;
			return this.id.equals(temp.id);
		}else 
				return false;
	}

	//////////////////////////////////////////////////
	public int hashCode(){                         //重写了hashCode方法
		int temp=this.getClass().hashCode();
		return temp*43+this.id.hashCode();
	}
	//////////////////////////////////////////////
	public String toString(){                      //重写了toString方法
		return this.id+":"+this.code;
	}
	/////////////////////////////////////////////
	private static Map<String,Integer> idbank =new HashMap<String,Integer>();
	public SaftyProcess(String id){                //通过构造方法来建立新的id并绑定一个随机流水号到映射中
		this.setId(id);
		while(code<=100000){                       //取得6位伪随机整数
		code=(int)(Math.random()*1000000);}
		idbank.put(this.id, this.code);
	}
	
	
	public static void innerIndex(String id){      //利用map的get(key)方法返回value值，系统自动解包，传到temp后输出
		int temp=idbank.get(id);
		System.out.println(temp);
	}
}
