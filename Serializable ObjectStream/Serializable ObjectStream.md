Serializable ObjectStream

package prototype;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;

public class SongObjectSerializableIO {
	
//对象序列化接口Serializable:
//Java是一门面向对象的语言,很多的信息和数据都是以对象为单位进行存储和传输的,
//那么我们就需要直接用流去读写对象,Java把对象的读写规范写成了一个接口,就是java.io.Serializable接口;
//虽然Serializable接口中没有定义任何方法和字段(属性),仅用于标识可序列化的语义;但是如果想正确的读写对象,就必须实现这个接口,因为它会告知
//JVM此类中的对象可以进行对象数据的分解与合并;
	
//ArrayList已近实现了序列化接口;(JDK中类似Set/Map/List/String保存数据的类都已经实现了序列化接口);
	
//将基本类型数据转换为字节的形式称为：序列化;
//从字节转换为其对应的基本类型数据称为：反序列化;
//将数据保存到磁盘中称为：持久化;
	
//ObjectInputStream/ObjectOutputStream (处理流):
//允许对任意对象进行直接的读写操作,但是要求必须是实现对象序列化接口的类;
//在对象序列化时,包名,类名和属性可以被序列化;构造/方法不能被序列化;
//static的属性不属于对象,属于类,所以static的属性不参与序列化;
	
//ObjectInputStream(InputStream in);  //创建从指定 InputStream 读取的 ObjectInputStream;
//readObject();  //从 ObjectInputStream 读取对象; (注意这里读取对象返回的是Object对象);
    
//ObjectOutputStream(OutputStream out);  //创建写入指定 OutputStream 的 ObjectOutputStream;
//writeObject(Object obj);  //将指定的对象写入 ObjectOutputStream;
	
//transient 关键字：
//被此关键字修饰的属性不会被序列化;
//注意：被transient声明的属性本身会被序列化并写入文件,而其内容不会被序列化写入,所以输出时是一个初始值：0/null;

//序列化版本号 serialVersionUID;
//需要注意的是：如果在写入对象成功之后对此对象类中进行修改,比如重写了toString()方法,那在未重新写入之前就去读取此对象的内容会发生java.io.InvalidClassException报错,
//错误原因,serialVersionUID,类的序列化编号不一致;JVM在一个类创建后会保存一个相应的序列化版本号：UID,在序列化对象时会把此UID一起保存在序列化后的对象数据中;
//反序列化一个对象数据时会检查此对象数据中保存的UID编码与其本地类的UID编码是否保持一致,如果不一致就说明其本地类有过改动,不能执行反序列化操作;
//但是如果用户自定义声明了一个序列化版本号,如：private static final long serialVersionUID = 1L;那就会覆盖系统默认给定的UID,也就是说如果用户在修改了类的内容后必须
//更新版本号,不然JVM在读取对象流时检查版本号通过不会报错,这时你就接收到了一个和现在类中内容不符的类对象;
//所以,每当我们对该类进行修改,都应该改变版本号,这样在反序列化时就会检查被反序列化的对象和当前类型版本号是否一致,只有一致才能进行反序列化;
//还需要注意的是,一旦定义了某个属性为transient,那改变类中和此属性相关的内容是不会造成JVM对版本号的修改的;
	
/////////////////////////////////////////////////
//WritePerson:
	public static void main(String[] args){
		Person p =new Person("song",20);
		try{
			//将对象流写入文件;
			FileOutputStream fos=new FileOutputStream("C:/JavaPerson.tmp");
			ObjectOutputStream oos=new ObjectOutputStream(fos);
			oos.writeObject(p);
			
			oos.close();
			
			//从文件读取对象流;
			FileInputStream fis=new FileInputStream("C:/JavaPerson.tmp");
			ObjectInputStream ois=new ObjectInputStream(fis);
			Object obj=ois.readObject();
			
			ois.close();
			if(obj instanceof Person){
				Person p1=(Person) obj;
				System.out.println(p1.getName()+p1.getAge());  //song0; 由于属性age被定义为transient,所以这里输出的是0;
			}
			
		}catch(Exception e){
			e.printStackTrace();
		}
		
		
	}
	
}

//////////////////////////////////////////////////////
class Person implements java.io.Serializable{
	
	private static final long serialVersionUID = 1L; //serialVersionUID是serialVersionUID唯一可以识别的一个变量名,它被认为是此类的序列化ID;它应该是Serializable接口中定义的一个常量;
	
	private String name;
	private transient int age;  //用transient声明age属性;
	
	public Person(String name, int age) {
		super();
		this.setName(name);
		this.setAge(age);
	}
	public Person() {
		super();
	}
	
	public void setName(String name) {
		this.name = name;
	}
	public String getName() {
		return name;
	}
	public void setAge(int age) {
		if(age>0&&age<150){
		this.age = age;
		}else{
			System.out.println("age error");
		}
	}
	public int getAge() {
		return age;
	}
}
	

