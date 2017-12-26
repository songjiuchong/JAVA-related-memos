Template  

package prototype;

public class SongTemplate {

	/*
	 一.单例模式（单实例模式）：
	 该模式限制了一个类全局只有一个实例; 
	 1.所以构造方法不能暴露给外部;
	 2.提供可以获取唯一实例对象的静态方法;
	 3.保存唯一对象的变量也必须是静态的;
	  
	 二.模板模式：
	 定义一个典型模板式的操作流程逻辑,将细节上的差别延迟到子类去实现;
	 在父类中定义逻辑的骨架,把可变细节的选择权交给子类,通常模板类是抽象的;
	 
	 三.装饰模式：
	 在不修改源程序的情况下,额外的为其添加一些功能/逻辑;
	 
	 
	 */
		
		public static void main(String[] args){
			
			//Singleton Test
			
			//Singleton obj1=new Singleton(); 已无法实例化新的对象;
			
			Singleton obj1=Singleton.getInstance();
			Singleton obj2=Singleton.getInstance();
			
			System.out.println(obj1==obj2); //true;
			
			//////////////////////////////////////////////
			
			//Template Test
			
			template t1=new template1();
			template t2=new template2();
			
			t1.say();
			t2.say();
			
			/////////////////////////////////////////////
			
			//Decorator Test
			
			// 正常模式：
			People person = new Teacher();
			person.say();

			// 装饰模式：
			People person2 = new Decorator(new Teacher());
			person2.say();

			// 装饰模式2(将装饰模式1装入)：
			People person3 = new Decorator2(person2);
			person3.say();
			
		}
	}

//////////////////////////////////////////////////////////////

	class Singleton{
		
		//保存一个私有静态属性,当前类型实例,仅创建一次;
		private static Singleton obj= new Singleton();
		
		//私有化构造方法,禁止外界通过new关键字创建实例;
		private Singleton(){
			
		}
		
		//对外提供一个静态获取当前类型实例的方法;
		public static Singleton getInstance(){
			return obj;
		}
	}
	////////////////////////////////////////////////////
		
	abstract class template{
		
		//模板方法,该方法定义具有部分相同的内容(可以参考 The LastSong Tetris中的Layer抽象类的应用);
		public void say(){
			System.out.println("To beginning with,");
			System.out.println("this mode is "+getName());
			System.out.println("it is ");
			System.out.println(getInfo());
			System.out.println("Thank u,have a nice day!");
		}
		
		//抽象方法,模板方法中细节不同的内容;
		public abstract String getName();
		
		public abstract String getInfo();
		
	}
		
	class template1 extends template{

		@Override
		public String getName() {
			return "template1";
		}
		
		@Override
		public String getInfo() {
			return "very helpful!";
		}
	}

	class template2 extends template{

			@Override
			public String getName() {
				return "template2";
			}
			
			@Override
			public String getInfo() {
				return "very ...sophisticated!";
			}
	}
	
	/////////////////////////////////////////////////////
	
	//装饰类1：
	class Decorator implements People {

		// 需要被装饰的对象：
		private People person;

		// 通过构造方法,将需要装饰的对象传入进来;
		public Decorator(People person) {
			this.person = person;
		}

		@Override
		public void say() {
			System.out.println("befor aaa,bbb");
			person.say();
			System.out.println("after aaa,ccc");
		}
	}

	//装饰类2：
	class Decorator2 implements People {

		private People person;

		public Decorator2(People person) {
			this.person = person;
		}

		@Override
		public void say() {
			System.out.println("xxx");
			person.say();
			System.out.println("xxxxx");
		}
	}

	
	interface People {
		void say();
	}

	//被装饰的类
	class Teacher implements People {

		@Override
		public void say() {
			System.out.println("aaa");
		}
	}

	
	
