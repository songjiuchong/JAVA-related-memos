IOStream


package prototype;

import java.io.*;
public class SongIOStream {
	public static void main(String []args){
		
		//FileInputStream:
		
		String s="";
		
		try{
			
			FileInputStream file = new FileInputStream("C:/IOtest.txt"); //文件读取对象,参数为需要被读写的文件及其地址/文件对象;
			
			byte[] b = new byte[10];
			
			for(;;){
				int num=file.read(b);   
				//read()方法参数：存储读取数据的缓冲区大小; 如果无参则是一个字节;如果是字节数组则存储读取数据的大小为数组的大小;
				//返回：读入缓冲区的字节总数，如果因为已经到达文件末尾而没有更多的数据，则返回 -1;
				
				System.out.println(num);//10 10 3 -1  : 第一次读取十个字节,第二次读取十个自己,第三次只剩下3个字节的内容,最后返回-1;
				
				if(num==-1) break;
				s=s+new String(b,0,num); //将此次读取至字节数组缓冲区中的数据取出,取出范围是读入缓冲区的字节数;
			}
			file.close();
			
			System.out.println(s);    //打印所有取出的内容;
			
			
			
		//FileOutputStream:
			
			FileOutputStream fos=new FileOutputStream("C:/JavaOutput.txt");
			fos.write("abcd".getBytes()); //利用.getBytes()方法得到一个Byte数组; 在C:/创建了一个内容为:abcd的txt文件:JavaOutput;
			fos.close();
			
		}catch(Exception e){
			e.printStackTrace();
		}
		
		
		//Copy:
		//用输入流读取目标文件再用输出流将其输出到指定地址;
		
		try{
			FileInputStream fis = new FileInputStream("C:/JavaCopyTest.zip");
			FileOutputStream fos= new FileOutputStream("C:/JavaCopyTest/JavaCopyTestCopy.zip");  //复制JavaCopyTest.zip文件到JavaCopyTest文件夹下以JavaCopyTestCopy.zip名称保存;
			
			byte[] temp=new byte[2048]; //2k个元素的byte数组;
			for(;;){
				int num = fis.read(temp);
				if(num==-1) break;
				fos.write(temp,0,num);
			}
			fis.close();
			fos.close();
		}catch(Exception e){
			e.printStackTrace(); 
		}
		
		//上述的copy方法过于简单存在一定问题;
		//标准的文件复制操作以及IO捕获异常：
		  
		  FileInputStream fis1=null;
		  FileOutputStream fos1=null;
		  
		  try{
		  
		  fis1=new FileInputStream("c:/pack.rar");
		  fos1=new FileOutputStream("c:/pack_copy.rar");
		  
		  byte[] buf=new byte[1024*10];
		  
		  int sum=0;
		  
		  while((sum=fis1.read(buf))>0){
		  fos1.write(buf,0,sum);
		  }
		  
		  System.out.println("复制完毕");
		  
		  }catch(FileNotFoundException e){
		  
		  System.out.println("需要被复制的文件未找到！");
		  e.printStackTrace();
		  
		  }catch(IOException e){
		  
		  System.out.println("读写失败！");
		  e.printStackTrace();
		  
		  }finally{       //由于如果出现异常,try中程序会中断并直接跳入catch语句块,所以必须将close语句写在finally中;
		  
		  try{
		  
		  if(fis1!=null){   //这里检验fis是否为空的原因时如果fis的初始化对象的发生文件不存在异常会导致直接跳入catch语句而并没有初始化fis/fos,
		                    //这样在调用close方法时会发生空指针异常;空指针异常属于RuntimeException,不能用IOException接收;
		  fis1.close();
		  }
		  
		  if(fos1!=null){
		  fos1.close();
		  }
		  
		  }catch(IOException e){
		  e.printStackTrace();
		  
		  }
		  }
		 
		 
		  //用缓冲流包装低级流(复制的速度极大的获得了提升):
		   
		   BufferedInputStream bis=null;
		   BufferedOutputStream bos=null;
		   
		   try{
			   
		   FileInputStream fis5=new FileInputStream("c:/pack.rar");
		   
		   FileOutputStream fos5=new FileOutputStream("c:/pack_copy.rar");
			   
		   bis=new BufferedInputStream(fis5);
		   
		   bos=new BufferedOutputStream(fos5);
		   
		   int d=-1;
		   while((d=bis.read())!=-1){
		   bos.write(d);
		   }
		   }catch(FileNotFoundException e){
			   e.printStackTrace();
		   }catch(IOException e){
			   e.printStackTrace();
		   }finally{
		   try{
		   bis.close();
		   bos.flush();  //通常情况下具有缓冲功能的输出流在关闭前要清空缓存区(将缓存区的内容一次性写出)
		                 //防止用户写入的数据未装满缓冲输出流缓冲区的容量导致数据并没有被真正写入硬盘;
		   bos.close();
		   
		   }catch(Exception e){
			   e.printStackTrace();
		   }
		   }
		   
	
	
		//DataInputStream:
		
		try{
			FileOutputStream fos=new FileOutputStream("C:/JavaCopyTest/JavaDataStreamTest.txt");
			DataOutputStream dos=new DataOutputStream(fos);
			dos.writeInt(12345); //写入的时候是按4个byte的二进制写入,所以文件中与实际参数内容不同,是" 09";
			
			FileInputStream fis= new FileInputStream("C:/JavaCopyTest/JavaDataStreamTest.txt");
			DataInputStream dis=new DataInputStream(fis);
			int i=dis.readInt();
			System.out.println(i);
			
			dos.close();  //关闭外层处理流就默认已经一起关闭了内层的文件流;
			
		}catch(Exception e){
			e.printStackTrace();
		}
		
		
		//FileWriter:
		
		try{
			FileWriter fw=new FileWriter("C:/Javafw.txt",true); //此处在这里加true参数是追加写入方式,abcde的输入会随着程序运行的次数而增加;
			String s1="abcde";
			fw.write(s1);
			fw.close();
			
			FileReader fr=new FileReader("C:/Javafw.txt");
			File f=new File("C:/Javafw.txt");
			long size=f.length();
			char[] ch=new char[(int)size];
			fr.read(ch);
			fr.close();
			
			for(char c:ch){
				System.out.print(c);
			}
			
			String st = new String(ch); //String构造方法可以以char数组为参数;
			System.out.print(st);
			
		}catch(Exception e){
			e.printStackTrace();
		}
		
		
		//Project:
		
		//利用字符输出流使用不同的字符集写入文件：
		
		try{
			String s2="abcd中";
			
			FileOutputStream fos=new FileOutputStream("C:/GBK.txt");  //6字节;
			OutputStreamWriter osw=new OutputStreamWriter(fos,"GBK");
			osw.write(s2);
			osw.close();
			
			FileOutputStream fos2=new FileOutputStream("C:/UTF-8.txt");  //7字节;
			OutputStreamWriter osw2=new OutputStreamWriter(fos2,"UTF-8");
			osw2.write(s2);
			osw2.close();
			
			FileOutputStream fos3=new FileOutputStream("C:/UTF-16BE.txt");  //10字节;(由于编码转换问题文件中出现乱码);
			OutputStreamWriter osw3=new OutputStreamWriter(fos3,"UTF-16BE");
			osw3.write(s2);
			osw3.close();
			
			
		}catch(Exception e){
			e.printStackTrace();
		}
		
		
		//TestKey(将键盘接受到的字节流信息转换为字符流然后利用PrintStream打印在指定的文本中):
		InputStreamReader isr=new InputStreamReader(System.in);
		BufferedReader br=new BufferedReader(isr);
		
		try{
			FileOutputStream fos=new FileOutputStream("C:/JavaTestKey.txt");
			PrintStream ps=new PrintStream(fos);
			for(;;){
				String ss=br.readLine();               //等待用户输入文本信息,以回车结束;
				if("bye".equalsIgnoreCase(ss)) break;  // equalsIgnoreCase();方法是忽略大小写的字符串比较;
				ps.println(ss);
			}
			br.close();
			ps.close();
		}catch(Exception e){
			e.printStackTrace();
		}
		
		//将字符流对象转换为字符流对象再用BufferedReader逐行读取打印到屏幕上：
		
		try{
			
			BufferedReader br2 = new BufferedReader(new InputStreamReader(new FileInputStream("C:/JavaTestKey.txt")));
			for(;;){
				String sss=br2.readLine();
				if(sss==null) break;
					System.out.println(sss);
			}
			br2.close();
		}catch(Exception e){
			e.printStackTrace();
		}
		
		//用缓冲字符流复制文本;
		BufferedReader brtxt=null;
		BufferedWriter bwtxt=null;
		try{
			brtxt=new BufferedReader(new InputStreamReader(new FileInputStream("c:/copytxt.txt"),"GBK")); 
			//由于txt文件编辑默认使用GBK编码,想要真确读取文本信息需要用GBK编码读取;
			
			bwtxt=new BufferedWriter (new OutputStreamWriter(new FileOutputStream("c:/copiedtxt.txt"))); 
			//得到正确的字符信息后,输出的字符集就比较自由,只要输出的目标文件能够显示这种字符集的字符即可;
		  
		   String temp=null;
		   while((temp=brtxt.readLine())!=null){
			   bwtxt.write(temp);
			   bwtxt.newLine();
		   }
		}catch(Exception e){
			e.printStackTrace();
		}finally{
			try{
		   brtxt.close();
		   bwtxt.flush();
		   bwtxt.close();
			}catch(Exception e){
				e.printStackTrace();
			}
		}
		
		
		//用PrintStream的System.out.print向文件中输入数据;
		try{
		PrintStream out = System.out;  //保存向控制台输出的系统输出流;
		PrintStream ps=new PrintStream("c:/systemout.txt");
		System.setOut(ps); //为System.out设置新的输出流;
		
		System.out.println("invisible?");  //此处已经不显示在控制台上,而是写入文件;
		System.out.println("invisible?");

		System.out.flush();
		System.out.close();
		System.setOut(out); //还原向控制台输出的系统输出流;
		
		}catch(Exception e){
			e.printStackTrace();
		}
		System.out.println("visible!");  //重新显示在控制台上;

		
	}

}



/*

IO流：

Java中关于输入输出的信息交互使用IO流实现,需要根据不同的情况调用不同的IO流;


IO流的分类：

IO流的方向：输入流/输出流;  

输入流：从外部读入程序的就是输入流;  InputStream/Reader;
输出流：程序向外部输出的就是输出流;  OutputStream/Writer;

IO流的操作单位：字节流/字符流;  

字节流：以字节为数据的读写单位;  InputStream/OutputStream;
字符流：以字符为数据的读写单位;  Reader/Writer; (只能处理文本信息);

IO流的处理对象：低级流/高级流;
低级流(节点流)：数据有明确的来源和去向;直接操作设备(文件/网络); 功能比较底层,单一;
高级流(过滤流,处理流)：不能独立存在,必须依托于另一个流,达到简化读写操作的作用;可以对节点流进行包装,也称为包装流; 直接操作节点流; 功能更强大;

注意：

1.所有的流都是需要关闭的;关闭最外层流时会先关闭内部流;有些流在调用close方法时会自动调用flush()方法,比如BufferedWriter;
     在程序结束时,对文件资源的访问会自动释放,所以不关流问题不大,但是如果在服务器上的程序不关流,由于程序不会消亡,所以会造成比较大的问题;

2.IOstream在读取时其实也存在记录位置的指针,只不过IOstream的读/写流是两个独立的流,各自拥有自己的指针,而RandomAccessFile是读写一体的,所以很可能发生写入和读取之间因为没有重置指针发生了空读取的现象;
	还有一个原因是：无论是FileOutputStream还是FileWriter,IOstream的输出流都提供了如：(File file, boolean append),这样参数的构造,允许append也就是从已有内容之后开始写入,而RandomAccessFile必须实现给定指针位置,
	否则就会重头读写,所以更需要注意指针的概念;

3.向文本文档写入内容时,比如使用void write(int b);方法写入,虽然写入的实际是二进制码, 但是文本按照其字符码转换成字符后显示,比如：
 FileOutputStream fos=new FileOutputStream("c:/e.txt"); 
 fos.write(49);//1;
 fos.write(65);//A;

4.String提供了重载的构造方法：String(byte[],String charset);
                            String(byte[],int offset,int len,String charset);

5.FileInputStream 类的 int available(); 返回当前输入流允许读取的总字节量,这里就相当于返回文件的大小;该方法是InputStream中定义的,所以所有的输入流都有该方法,
     但是实现上有些差别,所有不建议使用该方法的返回值作为字节数组的长度;
  
6. 高级流之间可以互相嵌套,如：DataOutputStream中套BufferedOutputStream再装FileOutputStream;

  
一：FileInputStream/FileOutputStream;

FileInputStream:

	FileInputStream(File file); //这里的文件对象必须是对应文件而不能是目录;
	
	FileInputStream(String name); //这里的名称必须是对应文件而不能是目录;

读取时如果文件不存在会出现异常;

	int read();  //从此输入流中读取一个字节数据;返回一个 0 到 255 范围内的 int 字节值;如果已到达文件末尾,则返回 -1;
	
	int read(byte[] b);  //从此输入流中将最多 b.length个字节的数据读入一个 b数组中;返回读入缓冲区的字节总数,如果因为已经到达文件末尾而没有更多的数据,则返回 -1;
	
	int read(byte[] b, int off, int len) ;   //从此输入流中将最多 len个字节的数据读入一个 byte数组中的off起始处;返回读入缓冲区的字节总数,如果因为已经到达文件末尾而没有更多的数据,则返回 -1;
 
	long skip(long n);  //跳过 n 个字节的数据;返回实际跳过的字节数;

 
 FileOutputStream:
 
  FileOutoutStream(File file); //这里的文件对象必须是对应文件而不能是目录;
  FileOutoutStream(File file, boolean append); 如果append为true,则代表写入内容为添加在已有内容之后,false代表覆盖已有内容重新写入;
  
  
  FileOutoutStream(String name); //这里的名称必须是对应文件而不能是目录;
  FileOutoutStream(String name, boolean append );
  
  
	
 写入时如果文件不存在会新建文件;    

  write(byte[] b); // 从b数组写入b.length个字节到文件输出流中;
  
  write(byte[] b, int off, int len);  //将b数组中从off起始,长度为len的字节写入文件输出流中;

  write(int b);   //将指定字节写入文件输出流中;


////////////////////////////////////////////////////////////////////////////////////////////
  
 
二：DataInputStream/DataOutputStream;

   高级流DataOutputStream会将待写入类型的数据转化为相应的字节,然后再通过低级流FileOutputString将这些字节写到文件里;   
   高级流DataInputStreamis会先通过FileInputStream从文件中连续读取多个字节,然后再将它们组合成目标类型的数据值返回;
   
   这对流中的方法可以读写基本类型和String,功能上比FileInputStream/FileOutputString更强大;
  
 DataInputStream(InputStream in);  //像这种构造方法的参数为InputStream/OutputStream对象的类属于处理流类;
 
  int read(); //虽然在一些API中没有提到这个无参方法,但是无论按照逻辑还是测试结果都有这个方法;
  
  int read(byte[] b);
 
  int read(byte[] b, int off, int len);

  readByte/Char/Int/Long/short/Double/Float(...); 读取基本类型;返回相应的基本数据类型;
  
  readUTF(); 读取String;
  
  int skipBytes(int n);  //跳过n个字节数据;返回实际跳过的字节数;

  
  
  DataOutputStream(OutputStream out);
  
  write(int b);
  
  write(byte[] b, int off, int len);

  writeByte/Char/Int/Long/short/Double/Float(...); 写入基本类型; 按字节的形式写：如,Int为4个byte;
  
  writeBytes(String s);
 
  writeChars(String s);
  
  writeUTF(); 写入String;
  
  
     注意: 作为字节流的高级流,DataInputStream的readLine方法被标记为deprecated,不再使用;
  
  ////////////////////////////////////////////////////////////////////////////////////////////
   
  
  三： BufferedInputStream/BufferedOutputStream; 缓冲字节输入/输出流;
  
     缓冲输入输出流中自行维护了一块缓冲区,原理和在输入输出流中用参数的字节数组作为缓冲区一样;
     
  BufferedInputStream读取数据会一次性尽可能多的读取字节,并缓存起来,这样我们通过该流读取数据时,实际上就是从缓冲区读取了,都是在内存中进行的,所以硬盘操作次数较少,读取效率高;
     当缓冲区中的内容都读取完毕后,该流会再次尽可能多的读取字节缓存起来,等待我们再次读取;
     
  BufferedOutputStream写出数据时会先写入到其内部的缓冲区中,当缓冲区满了,会进行一次真正的写操作;也就是说程序员在执行写入操作时,BOS并没用把数据直接写入硬盘,而是放入其缓冲区保存,
     知道缓存区满了才会进行一次把所有缓存区数据写入硬盘的操作;同样减少了硬盘操作的次数,提高了效率;
  
   BufferedInputStream:
  
   int read();
         
   int read(byte[] b, int off, int len);
 

   BufferedOutputStream:

   void flush();  //刷新此缓冲的输出流;
          
   void write(byte[] b, int off, int len);
       
   void write(int b);
 

  
////////////////////////////////////////////////////////////////////////////////////////////
 
 
  四：Reader/Writer; (字符流);
     
 	文本信息在Java中是以字符串格式处理的;字符串就是字符的集合,是char数组;Java字符在内存中是char类型数据,是一个16位无符号整数,
 数值是字符对应的Unicode编码;
	字符流就是以字符为单位读写数据的,读用Reader或者其子类,写用Writer或者其子类;
  
  
  Reader/Writer 抽象父类;
  
  
  Reader:
  
  int read();  //读取单个字符;
  
  int read(char[] c); //将字符读入数组;
  
  int read(char[] c,int off, int len); //将字符读入数组的某一部分;
  
  long skip(long n);  //跳过n个字符;
 
  
  
  Writer:
  
  append(char c); //将指定字符添加到此writer; 由于Writer本身没有提供append功能的构造所有加入了append方法;
  
  write(char[] c); //写入字符数组内容;
  
  write(char[] c, int off, int len);  //写入字符数组的某一部分;
          
  write(int c); //写入单个字符;要写入的字符包含在给定整数值的 16个低位中,16 高位被忽略;
  
  write(String str);  //写入字符串;
      
  write(String str, int off, int len);  //写入字符串的某一部分;
          

  FileReader/FileWriter:
  
     如果想要读写的文件本身就是文本文件,那就不用使用转换流去把FileIn/OutputStream转化为字符流再去读写,
     而可以直接使用FileReader/FileWriter去读写字符数据;需要注意的是这两个流仅限于文本读写;
     但是,由于FileReader/FileWriter没有设置字符集的构造,只能使用默认的字符集,所以局限性比较大;
    
  FileReader:
     必须是字符文件;
  
  FileReader(File file);

  FileReader(String fileName);
  
  FileReader的方法主要继承自Reader;
  
  
  FileWriter:
     必须是字符文件;
  
  FileWriter(File file);  //根据给定的 File 对象构造一个 FileWriter 对象; 
        
  FileWriter(File file, boolean append); //根据给定的 File对象构造一个 FileWriter对象;
        
  FileWriter(String fileName); //根据给定的文件名构造一个 FileWriter 对象; 
          
  FileWriter(String fileName, boolean append);  //根据给定的文件名以及指示是否附加写入数据的 boolean值来构造 FileWriter对象 ;
         
  FileWriter的方法主要继承自Writer;
  
  int read();  //同样返回一个int类型的数据,只有其低16位是有效的;
  
   
///////////////////////////////////////////////////////////////////////////////////////////
 

  五：InputStreamReader/OutputStreamWriter; (转换流);
  
  	很多的信息都是只能得到字节流的,比如键盘信息System.in就是一个InputStream.还是网络的读写只能得到字节流,那么此时如果需要用字符流
  进行包装,就需要进行字节流向字符流的转换;
  	字节流转换成字符流需要使用转换流,包括：InputStreamReader/OutputStreamWriter;
  	
  
  
  InputStreamReader(InputStream in); //创建一个使用默认字符集的 InputStreamReader;
      
  InputStreamReader(InputStream in, Charset cs); //创建使用给定字符集的 InputStreamReader;
  
  
  InputStreamReader方法：
     
  int read();  //根据编码集读取一个字符数据到JVM后转换为字符对应的一个16位的char类型数据;返回的是一个int类型的数据,
                                             只有其低16位是有内容的;返回-1如果读到文件结尾;
  
      

  OutputStreamWriter(OutputStream out); // 创建使用默认字符编码的 OutputStreamWriter;
     
  OutputStreamWriter(OutputStream out, Charset cs); //创建使用给定字符集的 OutputStreamWriter;
  
  
  OutputStreamWriter方法：
  
  write(String str);  //虽然在一些API中没有提到参数仅为String的方法,但是无论按照逻辑还是测试结果都有这个方法;
  
  write(char[] chrs);
  
  write(String str, int off, int len);
 
  write(int chr);  //一次写一个字符(两个字节);只会写入int值的低16位;然后再根据所给的字符集将信息显示在文件中,
                                                       文件的真实大小也会根据字符集的不同而改变;
                                                       
  注意: 由于写入和读取时都将按照所给字符集对字符进行转化,所以Output时的字符编码要与Input时的字符编码保持一致,不然会发生乱码;
                 如果只是读取后写入,就只需注意读取时的字符集要与读取文件中文本的字符集相同,不然读取到的很可能已是错误的数据了;
    
      
  
      连续读取：
  InputStreamReader reader = new InputStreamReader(new FileInputSream("c:/XXX"));
  int char=-1;
  while((chr=reader.read()!=-1)){
  System.out.print((char)chr);
  }
  reader.close;        
  
  
  ////////////////////////////////////////////////////////////////////////////////////////////
   
  
  六: BufferedReader/BufferedWriter; 缓冲字符输入输出流;
	
	BufferedReader/BufferedWriter可以以字符为单位读写信息,一般和转换流结合使用;
	
	如： BufferedReader br=new BufferedReader(new InputStreamReader(new FileInputStream("c:/abc.txt")));
	
	
	增强的方法：String readLine(); //在读取到\n也就是换行符时就认为读取完了一行,但不会把\n读入;当读到文件末尾时将返回null;
	
	write(String str);  //虽然在一些API中没有提到参数仅为String的方法,但是无论按照逻辑还是测试结果都有这个方法;
	
	write(String s, int off, int len);
 
    newLine();  //方法会在调用处添加一个换行符; 
 	
 	
 	注意: 在这里提到的常用输入输出流中只有BufferedWriter有newLine()方法;
 	      
   ////////////////////////////////////////////////////////////////////////////////////////////
 	  
 	 
      七：PrintWriter/PrintStream; 缓冲字符输出流/缓冲字节输出流;
      
   PrintStream/PrintWriter的构造方法和print相关方法基本相同,但是PrintStream是字节流,它的write相关方法只有 void write(byte[] buf, int off, int len); void write(int b);这样写入
        字节的方法;而PrintWriter的write相关方法只有写入字符的方法;
      
    
    PrintStream构造：
    
    PrintStream(File file);
          
	PrintStream(File file, String csn);

	PrintStream(OutputStream out); //  创建新的打印流;
	
	
	
	PrintWriter构造：
	
	PrintWriter(File file);
  
	PrintWriter(File file, String csn);
      
	PrintWriter(OutputStream out);
 
	PrintWriter(OutputStream out,boolean autoFlush); //可以指定是否自动行刷新,若第二个参数为true,则println/printf等方法输出数据后会
                                                                                                                                                   立即flush缓冲区;显然及时性提高的同时会降低效率;
	
	PrintWriter(Writer writer);
  
    PrintWriter(Writer writer,boolean autoFlush);
   
   
  
	void print(String s);     //print方法没有返回值,直接写入;
	
	void println(String str); //输出带换行符的字符串;
 
	void print(char c);
 
    void print(int i);
    
          虽然PrintWriter/PrintStream都可以将字符类型的数据输出,但是在需要写入字符而不是写入字节的情况下，应该尽量使用 PrintWriter类;
   
   
   
    PrintStream out = System.out;
    
    System.out.println(); //其实是System类下的一个静态输出流out(向终端输出)调用了println()方法;
 
	InputStream in = System.in;
		
	System.in //数据源是键盘的输入流;它是一个字节输入流;
	(数据源 DataSource：数据的来源,比如Scanner的数据源是键盘输入的信息;)
	
	
  /////////////////////////////////////////////////////////////////////////////////////////
   
    
      八：字符序列化编码;
   
  1.ASCII: 最早的编码; 包含大小写字母,数字,符号等128个编码,用于解决0/1不能直接表示字符的问题,来为每一个字符编号;(英文占一个字节);
  
  2.ISO8859-1:西欧拉丁语系编码;
  
  3.GB2312：是早期中文编码标准,能表示6000多汉字;
  
  4.GBK: 20000+汉字,日文,朝鲜文; 属于GB2312的超集; 采取1-2变长编码：(英文占一个字节,中文占两个字节);
  
  5.UTF-16BE: 2个字节定长编码,中英文各占2个字节;
  
  6.UTF-8: 1-4变长编码,英文1字节,中文3个字节;
  
  
  /////////////////////////////////////////////////////////////////////////////////////////
  
  
  九: I/O复习指引:
  
  1.主要还是根据API结合上述内容进行比较,分类;
  
  2.RandomAccessFile和DataInputStream/DataOutputStream实现了DataInput/DataOutput这两个接口,包含了一些较为高级的输入输出流,在以字节为
     单位输入输出的基础上,能够将基础单位转换为各种整型,浮点类型,甚至是字符串类型进行操作;
     
  3.InputStream/OutputStream是包含最基本的以字节为单位输入输出方法的抽象类,FileInputStream/FileOutputStream和BufferedInputStream/BufferedOutputStream
     是这个抽象类的子类,都实现了最基本的字节流输入输出方法;
  
  4.Reader/Writer是包含了最基本的以字符为单位输入输出方法的抽象类,InputStreamReader/OutputStreamWriter,BufferedReader/BufferedWriter和FileReader/FileWriter
     都是其子类,其中FileReader/FileWriter其实还是InputStreamReader/OutputStreamWriter这个转换流的子类,所以不能看出JVM从原理上还是在读写字节,只不过通过转换流将字节
     整理后达到能够直接读写字符的效果;

  5.在此处提到的常用输入输出类中,只有FileOutputStream和FileWriter这样的基本节点输出流才拥有第二个参数为boolean append的在目标文件末尾继续输出的构造方法,由于
     它们是节点流,所以高级流在将节点流作为参数来进行输出操作时也会根据它们的append状态决定输出信息是否在目标文件末尾继续; 

  6.在此处提到的常用输入输出类中,只有转换流InputStreamReader/OutputStreamWriter和PrintStream/PrintWriter才拥有第二个参数为String csn(设定输入输出字符集)的构造方法; 

  
*/




