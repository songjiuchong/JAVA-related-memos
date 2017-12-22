Clone

package prototype;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;
import java.util.ArrayList;
import java.util.HashSet;

public class SongClone {

	public static void main(String[] args) throws IOException, ClassNotFoundException {
		
		 //clone:
		
		 ArrayList<Person3> list=new ArrayList<Person3>();  //这里不用List<> list = new ArrayList<>();是因为List是接口,
		                                                    //它并没有实现Cloneable接口,也不可能去覆写clone()方法,而ArrayList已经满足了这两点;
		  list.add(new Person3("一",1));
		  list.add(new Person3("二",2));
		  list.add(new Person3("三",3));
		
		 ArrayList<Person3> clone = (ArrayList<Person3>)list.clone();
		 
		 System.out.println(list.equals(clone));  //true;集合中元素内容一致;元素中对象的地址也相同,所以元素中保存的是同一个对象;
		 System.out.println(list==clone);         //false; //克隆后的对象拥有和原对象不同的地址,属于不同对象;
		 
		 /////////////////////////////////////////////////////////////////////////////////////////
		 
		 //集合的复制构造器：
		 
		 ArrayList<Person3> clone2 = new ArrayList<Person3>(list); //将一个集合对象作为参数传入,可以返回一个效果和使用clone方法完全一样的集合;
		 
		 
		 HashSet<Person3> set=new HashSet<Person3>(clone2); 
		 //注意：将List集合传入Set集合中后,其顺序是不定的,Set是无序的,且List中相同的元素会被Set排除在外;
		 
		 
		 /////////////////////////////////////////////////////////////////////////////////////////
		 
		//高效深层复制:
			
			ArrayList<Person3> list2 = new ArrayList<Person3>();
			list2.add(new Person3("5",5));
			list2.add(new Person3("6",6));
			
			ByteArrayOutputStream bos=new ByteArrayOutputStream();
			
			ObjectOutputStream oos=new ObjectOutputStream(bos);
			
			oos.writeObject(list2);
			
			byte[] data=bos.toByteArray();  //将其内部的字节数组取出放入一个新的数组;
			
			ByteArrayInputStream bis=new ByteArrayInputStream(data);
			
			ObjectInputStream ois=new ObjectInputStream(bis);
			
			ArrayList<Person3> array = (ArrayList<Person3>) ois.readObject();
			
		
			oos.close();
			ois.close();
			
			System.out.println(list2==array); //false;
			System.out.println(list2.get(0)==array.get(0)); //false;
			System.out.println(list2.get(0).age==array.get(0).age); //true;
			System.out.println(list2.get(0).name.equals(array.get(0).name)); //true;
			
	}

}


class Person3 implements Serializable{
	public Person3(String name,int age){
		this.name=name;
		this.age=age;
	}
	
	String name;
	int age;
}


/*
  Object类中有clone方法(浅层复制)：
  
      浅表复制：只复制外观,内部不复制;
      
   Object clone();
  
  
   注意：clone方法生成的克隆对象中保存的引用对象拥有相同的地址,无论谁改动了此引用对象,所有克隆对象或是本体对象中的引用对象都已经被改变了;
   
   
  ////////////////////////////////////////////////////////
   
  
    集合的复制构造器：
    在对集合进行浅表复制的时候,可以使用集合的复制构造器将一个集合对象作为参数传入,可以返回一个效果和使用clone方法完全一样的集合;
    所有集合均有集合构造器;
  
    如：ArrayList clone = new ArrayList(Collection c);
    
    
  
    通过集合构造器还可以实现将一个List集合转化为一个Set集合等转换;
  
  HashSet<Person> set=new HashSet<Person>(clone);
  
  
  ///////////////////////////////////////////////////////////
    
  
   深层复制： 
  
   序列化与反序列化一个对象属于深层复制,不旦被序列化的对象被复制(内容相同但拥有新的地址),其中被保存的对象也被复制
   了(同样内容相同且地址不同);这与使用clone方法获得的浅层复制对象不同;
  
  
   高效深层复制:
   
   创建一个把流写入内存而非写入硬盘(文件)再读取的方法(高效深层复制)：

  ByteArrayInputStream/ByteArrayOutputStream:字节数组输入/输出流(低级流);

   该输出流内部维护着一个字节数组,使用该输出流写入的字节,都保存在了自身维护的数组上(内存中);
   给定该输出流一个字节数组,该输入流就会从数组中读取所有字节;

   简单地说,使用这对流,就是把对字节数组的操作包装为字节流的读写操作而已;



 注意:
 1.所有想要使用clone()方法的对象必须先实现java.lang.Cloneable接口,这个接口内虽然没有clone()方法,但是只有继承它才能指示 Object.clone()方法可以合法地对该类实例进行按字段复制; 
  如果在没有实现 Cloneable接口的实例上调用 Object的 clone 方法,则会导致抛出 CloneNotSupportedException异常;第二个比较特殊的地方在于这个方法是protected修饰的,覆写clone()方法的时候需要写成public,才能让类外部的代码调用;
  注意虽然这里覆写clone方法可以也定义为protected,但是这样就只能在本包内和子类内部调用了,在外部就无法调用了;最后还需注意的一点是,如果不覆写此方法,那么在继承了Cloneable接口后,任何类都可以在其内部调用clone方法,因为所有类都
  是Object类的子类,但是如果要用类对象在外部调用clone方法,就必须用public覆写;

 2.对于深层复制使用的ByteArrayOutputStream/ByteArrayIntputStream方法,它们继承了java.io.OutputStream/java.io.InputStream抽象类;但是由于深层复制需要序列化与反序列化来实现,所以一般都将它们放在ObjectOutputStream/ObjectIntputStream
  这样的高级对象流中使用;又因为字节数组输入输出流是读写内部维护内存的流,而不读写硬盘,所以它们的构造方法也与其他流不同;
 
  输出流;
 ByteArrayOutputStream();
          创建一个新的 byte数组输出流,默认将数据写入其内部维护的数组内存,缓冲区的容量最初是 32字节;
 ByteArrayOutputStream(int size);
          创建一个新的 byte 数组输出流，它具有指定大小的缓冲区容量(以字节为单位);

 byte[] toByteArray(); 创建一个新的 byte数组,其大小是此输出流缓存区中数组的当前大小,并且缓冲区的有效内容已复制到该数组中;
  
  注意:这个方法相当于将写入的内容放入了一个新的字节数组;但是由于深层复制需要的反序列化,是将字节重新组合为对象,所以还是必须用ObjectInputStream包装ByteArrayInputStream来实现,
  但是这个方法的存在的意义是,ByteArrayInputStream构造方法需要指定一个数组最为读取流的缓冲区数组,这就是要指定一个读取的目标数组最为缓冲数组,再从这个缓冲数组中读取内容,所以在
  使用了输出流之后,将输出流的默认缓冲数组取出放入新的中间数组,这样之后输入流就有指定的数组来读取了;
          
   输入流:
 ByteArrayInputStream(byte[] buf);
          创建一个 ByteArrayInputStream,使用 buf 作为其缓冲区数组,字节输出流就从这个指定的缓冲区数组来读写内容;
 ByteArrayInputStream(byte[] buf, int offset, int length);

 3.这两个流虽然有close方法,但是是无效的;
 
 
 
*/


