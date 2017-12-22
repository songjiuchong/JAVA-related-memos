IO：File

package prototype;

import java.io.File;
import java.io.FileFilter;
import java.io.RandomAccessFile;

import java.io.FilenameFilter;

public class SongIOFile {
	
	// 利用File类的boolean delete();递归删除含有子项的目录;

	public static void delete(File file) {

		// 先判断是否为空,因为如果不判断的话后一个.exists()有报空指针的风险,
		// 又由于用了"||",一旦第一个判断为空就不会判断第二个条件了;
		if (file == null || !file.exists()) {
			throw new RuntimeException("文件或目录不存在！");
		}

		// 判断给定的File对象是文件还是目录,如果是文件直接删除;
		// 如果是目录,获取所有子项后再对每一个子项递归调用delete方法进行检查和删除;
		if (file.isDirectory()) {
			for (File sub : file.listFiles()) {
				delete(sub); // 递归调用delete方法删除子项中的文件与目录;
			}
		}
		file.delete(); // 删除文件或已不包含子项的文件;
	}


	public static void main(String[] args) {
		
		//执行delete方法：
		
		File fx1 = new File("c:/songjava/a/b/c/");
		fx1.mkdirs();
		File fx2 = new File("c:/songjava");
		delete(fx2);
		
		//File:
		
		File f1= new File("c:/123.txt");  //File文件对象可以是文件也可以是目录;创建时可以不存在，不会发生异常;
		try{
		if(!f1.exists()){
			boolean temp=f1.createNewFile();   //这里会有一个编译异常IOException,由于没有处理潜在的异常：创建的文件路径不存在；没有访问权限等;
			System.out.println(temp);  //如果没有if语句限制的情况下：当第一次创建一个新文件时返回值为true,文件已存在或以创建好第二次运行此程序时返回false;
		}
	}catch(Exception e){
		e.printStackTrace();
	}
	

	File f2 = new File("c:/");
	String[] st = f2.list();
	for(String name : st){
		System.out.println(name);
	}
	
	
	File f3 = new File("c:/javademo.jpg");
	long leng= f3.length();
	System.out.println("文件大小： "+leng);

	//////////////////////////////////////////////////////

	
	File f4=new File("./");    
	System.out.println(f4.getName());  //当前的path name就是.,所以这里输出.;
	System.out.println(f4.getAbsolutePath());
	
	////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
	
	
	File f5= new File("../../../../../javademo.jpg");
	System.out.println(f5.exists());  //这里返回true，说明文件存在，注意的是：如果这里输出f5.getName();可以正确输出文件名，但是不能说明此文件在此路径下存在，因为创建File对象不需要文件存在;
  
	////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
	

	File f6=new File("C:/Users/song/Workspaces/MyEclipse 8.5/JavaTest/");  //这里最后的文件夹名之后可以跟/或者/.也可以不跟，都代表目前处于这个文件夹之中
    System.out.println(f6.getName());
    String[] st2 = f6.list();
    for(String name : st2){
		System.out.println(name);
	}
   
    System.out.println("//////////////////////////////////////////////////////////////////////////////////////////////");
   
    //FilenameFilter:
    
    //第一种使用方法更简洁,推荐;
    
    File f7= new File("C:/");   //windows盘符不区分大小写;Unix区分
    String[] st3 =f7.list(new FilenameFilter(){
    	public boolean accept(File file,String name){  //这里的file参数就是f7,name就是遍历时每一个file路径中的文件名/目录名; 
    		File tempfile = new File(file,name);
    		return name.endsWith("txt")||tempfile.isDirectory();  //判断此名称的文件后缀是否为.txt; 利用String类的方法  public boolean endsWith(String suffix);
    		                                                     //或者是目录也为真;
    	}
    });
    for(String name : st3){
    	System.out.println(name);
    }
    
    System.out.println("////////////////////////////////////////////");
    
    //第二种使用方法较繁琐,不推荐;
    
    FilenameFilter ff1=new FilenameFilter(){

		@Override
		public boolean accept(File file,String name) {
			File tempfile = new File(file,name);
			return name.endsWith("txt")||tempfile.isDirectory();
		}
    };
    
    String[] st4=f7.list(ff1);
    for(String name : st4){
    	System.out.println(name);
    }
   
    System.out.println("////////////////////////////////////////////");
    
    //FileFilter:
    
    File[] fx= f7.listFiles(new FileFilter(){

		@Override
		public boolean accept(File pathname) {
			
			return pathname.getName().endsWith("txt")||pathname.isDirectory();
		}
    });
    
    for(File temp: fx){
    	System.out.println(temp.getName());
    }
    
    
    //RandomAccessFile:
    
    File f = new File("C:/JavaRandom/Random.txt");
    try {
		RandomAccessFile rdf=new RandomAccessFile(f,"rw");
		String name=null;
		int age=0;
		name="superman";  //长度为8;
		age=30  ;  //长度为4;
		rdf.writeBytes(name);
		rdf.writeInt(age);
		
		name="batman  ";  //长度为8;
		age=32  ;  //长度为4;
		rdf.writeBytes(name);
		rdf.writeInt(age);
		
		rdf.close();
		
		
		RandomAccessFile rdf2=new RandomAccessFile(f,"rw");
		byte[] b=new byte[8];
		rdf2.skipBytes(12); //直接读取batman信息,空出superman信息;
		for(int i=0;i<b.length;i++){
			b[i]=rdf2.readByte();
		}
		String infoname=new String(b);
		int infoage=rdf2.readInt(); 
		System.out.println(infoname+infoage);
		
		rdf2.seek(0); //指针回到开始地址;
		for(int i=0;i<b.length;i++){
			b[i]=rdf2.readByte();
		}
		infoname=new String(b);
		infoage=rdf2.readInt();  //为了输出内容以写入时一致,这里需要与之前的写入时保持一致; 
		System.out.println(infoname+infoage);
		
		rdf.close();
		
		
	} catch (Exception e) {
		e.printStackTrace();
	}
    
	System.out.println("//////////////////////////////////////////////////////////////////////////////////////////////");
	
	File file=new File("C:/JavaIOP.txt");
	
	try{
		RandomAccessFile raf=new RandomAccessFile(file,"rw");
		
		raf.writeInt(-1);  //占4个字节;
		System.out.println("偏移量："+raf.getFilePointer());  //偏移量：4;
		raf.write(-1);
		System.out.println("偏移量："+raf.getFilePointer());  //偏移量：5;
		
		raf.seek(0);  //此处的指针还原非常重要,如果没有这条语句,将从最后开始读取,又由于使用readInt()方法读取不会返回末尾检测警告值-1,而是直接报错:EOFException;
		int a=raf.readInt(); //由于readInt();方法不会检测是否到达文件的末尾并返回-1,所以此处返回的-1一定是读取出的实际整数-1;
		System.out.println("偏移量："+raf.getFilePointer());  //偏移量：4;
		System.out.println(a);
		
		int b=raf.read();   //以整数形式返回此字节，范围在 0 到 255 (0x00-0x0ff);所以这里不会输出-1,而是255;
		System.out.println("偏移量："+raf.getFilePointer());   //偏移量：5;
		System.out.println(b);
		
		raf.close();
	}catch(Exception e){
		e.printStackTrace();
	}
	
    
	}
}


	/*

	1.关于输出的类库基本都定义在java.io和java.nio系列包中, nio包是1.4开始加入的;
	java.io包主要包括两个组成部分：
	
	一.文件、目录的非读写操作：File类 ( File即代表了文件，也代表了目录);
	二.各种输入输出的信息交互：IO流;
	
	这里主要研究File类的应用;
	
	文件和目录的描述方式：
	1.相对路径：从当前所在目录出发描述路径的方式(在软件开发中，相对路径使用较多，因为java需要跨平台开发，绝对路径无法保证跨平台后地址仍然有效！)
	2.绝对路径：从根目录出发，描述路径的方式
	
	目录的描述方式：
	.代表当前目录
	..代表上一层目录
	
	路径分隔符：
	Windows和Unix的路径分隔符是不同的：
	Windows  c:\ java中需要c:\\转义
	Unix     /root/
	在java类库中,可以使用/做windows和Unix统一的路径分隔符(可能是由于\是转义字符利用它的非转义功能时需要写两遍\\);
	File.separator也是通用的分隔符;直接+即可,相当于/;
	

	////////////////////////////////////////////////////////////////////////
	 
	文件和目录File类的API：
	
     	构造：File(String pathname) 如果此文件或目录(相对路径/绝对路径)不存在也可，因为这里是先在内存中创建了一个对象;
     	
     	方法：
     	
     	   boolean createNewFile(); 文件不存在可以用此方法创建,但是要求文件的上级文件目录必须已经存在;
	    
	       boolean mkdir();   只能创建一层目录;如果创建的目录的上级目录不存在就不能使用;
	       
	       boolean mkdirs();  目录不存在可以用此方法创建(而且可以创建多层目录);
	       
	       boolean delete(); 删除目录时需要保证目录下没有任何子项才可成功执行;当且仅当成功删除文件或目录时，返回 true；否则返回 false;
	       

	       static File createTempFile(String prefix,String suffix, File directory);  创建一个零时文件(前缀,后缀,位置);
	     
	       boolean delete(); 删除
	       
	       boolean equals(Object obj);   已经被重写,比较的是对象不是引用地址;
	       
	       String getName(); 取得文件或目录的名称
	       
	       getAbsolutePath();取得文件或目录的绝对路径
	       
	       boolean isFile(); 判断是文件还是目录
	       
	       long length();    文件的大小  这里用long返回文件大小是由于int的话范围在：-2^31--2^31-1 (2^10是1k,2^20是k*k=1M,2^30=1G,那2^31-1相当于2G大小，显然不够大);
	     
	       String[] list()  /  String[] list(FilenameFilter filter); 显示子目录和子文件的名称;
	     
	       File[] listFiles(); 返回一个对象数组;
	     
	       boolean exists();  判断此路径或文件是否存在;
	       
       
	//////////////////////////////////////////////////////////////////////
	
	文件名过滤接口：FileFilter/FilenameFilter 
	使用其匿名内部类实现接口中的accept();再利用返回值确定时候过滤文件名;
	
	FilenameFilter:
	 boolean accept(File dir,String name);  //参数为：调用此方法的File对象中的文件/目录的地址(如果是文件的话不会报错,但是无任何意义,因为遍历后返回的String[]中将最多存在一个元素,也就是这个文件的名称),目录中遍历到的文件/目录名称;如：("c:/","234.txt");
	
	使用方法：String[] list(FilenameFilter filter);   //返回一个字符串数组，这些字符串指定此抽象路径名表示的目录中满足指定过滤器的文件和目录;
 
	 
	FileFilter:
	 boolean accept(File pathname);        //参数为：调用此方法的File对象中遍历得到的每一个文件/目录地址对象(同样,如果是文件的话无意义),如: new File("c:/234.txt");
	 
	 使用方法：File[] listFiles(FileFilter filter);    //返回File对象数组，这些对象表示此File对象目录中满足指定过滤器的文件和目录 ;
          
	 
	这两个方法无本质区别，选择一个便可;
	 
	 另外：File的构造方法也包括： File filex = new File("c:/","234.txt");
	 

	////////////////////////////////////////////////////////////////////////
	  
	RandomAccessFile:
	  
	RandomAccessFile类的主要功能是完成随机读取功能(Random access 可以直接访问任一个存储单元,只要知道该单元所在的地址即可;Sequential access 属于循序存取,需要从头开始达到目标对象,但是避免了繁琐的指针);
	  
	由于在文件中所有的内容都是按字节存放的, 都有固定的保存位置; 但是这个类操作起来限制较多而且缺少辅助的类与方法,所以在IOStream中将会提供更加倾向于开发目的的IO类;
	
	RandomAccessFile是基于指针读写的，写入后读取数据时注意指针的位置是否需要还原;
	
	注意：IOstream在读取时其实也存在记录位置的指针,只不过IOstream的读/写流是两个独立的流,各自拥有自己的指针,而RandomAccessFile是读写一体的,所以很可能发生写入和读取之间因为没有重置指针发生了空读取的现象;
	还有一个原因是：无论是FileOutputStream还是FileWriter,IOstream的输出流都提供了如：(File file, boolean append),这样参数的构造,允许append也就是从已有内容之后开始写入,而RandomAccessFile必须实现给定指针位置,
	否则就会重头读写,所以更需要注意指针的概念;
	
	构造:
	RandomAccessFile(File file,String mode);
	RandomAccessFile(String name, String mode) 

	mode: r:只读;
	      rw:读写,如果文件不存在,会自动创建;
	      
	write(int v); 向文件中写入1个字节，这个字节是int的低8位,其他位数忽略;
	
	writeInt(int v);  // 按4个字节将 int 写入该文件，先写高字节;
	  
	writeBytes(String s);  //该字符串中的每个字符均按顺序写出,由于char为16位无符号,所以会丢弃其高八位;

	writeByte(int v);  //按单字节值将 byte 写入该文件;
	
	write(byte[] b);  //将 b.length 个字节从指定 byte 数组写入到此文件，并从当前文件指针开始;
	
	writeUTF(); //将字符串的字节数写入文件(以便读取时分辨哪些是字符串内容),再写入字符串内容;
	
	

	readUTF();  //将此记录字符串字节数的占位码以及字符串读出;
          
	int read(); //读取一个字节,返回一个int值,该值只有低8位有效;返回值为-1时,已经读取到了文件的末尾;
	
	int read(byte[] b); //读入缓冲区的总字节数，如果由于已到达此文件的末尾而不再有数据，则返回 -1;

    int readInt();  //从此文件读取一个有符号的 32位整数;返回此文件的下四个字节，解释为一个 int;
    
    byte readByte();  //从此文件读取一个有符号的八位值 ;返回有符号的八位 byte;
          
	String readLine(); //从此文件读取文本的下一行此文件文本的下一行,如果连一个字节也没有读取就已到达文件的末尾,则返回 null;



 	long length();  //返回此文件的长度 ;
          
	int skipBytes(int n); //  尝试跳过输入的 n 个字节;返回跳过的实际字节数;
        
	seek(long pos);  //设置到此文件开头测量到的文件指针偏移量，在该位置发生下一个读取或写入操作;
	
	long getFilePointer(); //获取到此文件开头的偏移量（以字节为单位），在该位置发生下一个读取或写入操作;
          
          
*/


