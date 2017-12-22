ChatRoom Demo

package prototype;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.net.SocketException;
import java.util.Hashtable;
import java.util.Set;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.LinkedBlockingQueue;


//聊天室服务器端：

public class ChatRoomServer {

	public static void main(String[] args) {
		Server server=new Server();
		server.start();
	}

}

class Server{
	//java.net.ServerSocket;
	private ServerSocket serverSocket;

	//线程池;
	private ExecutorService threadPool;
	
	//创建一个线程安全的集合,用于保存所有客户端的ID/输出流;
	private Hashtable<String,Socket> allOut= new Hashtable<String,Socket>();
	
	//双缓冲队列,用于保存所有客户端发送到服务器的信息;
	private BlockingQueue<String> msgQueue;
	
	
	public Server(){
	try{  
		//打开服务器端Socket时要捕获异常,最常见的问题就是申请的端口号已经被其他程序占用了,错误信息中会出现:Address already in use: JVM_Bind信息;
		serverSocket = new ServerSocket(8898);
		
		//创建线程池;
		threadPool=Executors.newCachedThreadPool();
		
		//初始化双缓冲队列;
		msgQueue=new LinkedBlockingQueue<String>();
		
		System.out.println("server has been initiated ");
		
	}catch(IOException e){
		e.printStackTrace();
		}
	}
	
	//start the server: 
	public void start(){
		
		//启动用于转发信息的线程;
		SendInfoToClientHandler SITChandler=new SendInfoToClientHandler();
		
		Thread thread = new Thread(SITChandler);
		
		thread.start();
		
		System.out.println("server has been launched ");
		
		while(true){
		//这里将try/catch语句放入循环体内是为了发生异常后继续执行循环;
		try{
		System.out.println("waiting for client to connect...");
		//accept()方法用于在8088端口进行等待,等待客户端的连接,当一个客户端通过IP/端口号连接上,会返回这个客户端的Socket
		//的Socket与其开始通信;属于阻塞方法,客户端不连接代码就不会继续执行;
		Socket socket = serverSocket.accept();
		//获取IP;为了区分同ID用户,不会在使用allOut.remove()方法是将错误用户socket移除,这里需要用id+ip来做key放入集合;
		String ip = socket.getInetAddress().toString();
		//一旦成功连接了一个客户端就把这个客户端的ID/Socket对象放入共享集合;
		String clientId = new BufferedReader(new InputStreamReader(socket.getInputStream())).readLine();
		//反馈给客户端,连接成功;
		PrintWriter pw = new PrintWriter(socket.getOutputStream());
		pw.println("已与服务器连接");
		pw.flush();
		//将客户端id+ip和其socket放入集合;
		allOut.put(clientId+ip,socket);
		System.out.println(clientId+" (IP:"+ip+") has connected!");
		
		//启动一个新的客户端读取线程;
		Handler handler = new Handler(clientId,socket,ip);
		
		//把任务交给线程池;
		threadPool.execute(handler);
		
		}catch(SocketException e){
			System.out.println("one client failed to connect to server");
			//e.printStackTrace();
			continue;
		}catch(IOException e){
			System.out.println("IO exception occourred ");
			//e.printStackTrace();
			continue;
		}
		}
	}
	
	//内部类定义一个线程体,该线程体用来并发操作,每一个线程用来与一个客户端交流,
	//获取客户端发送过来的信息;
	private class Handler implements Runnable{
		//当前线程要处理的客户端Socket:
		private Socket client;
		private String clientId;
		private String ip;
		public Handler(String clientId,Socket client,String ip){
			this.client=client;
			this.clientId=clientId;
			this.ip=ip;
		}
		
		@Override
		public void run() {
			try{
				InputStream in = client.getInputStream();
				BufferedReader read = new BufferedReader(new InputStreamReader(in));
			
				String info=null;
				while((info = read.readLine())!=null){
				//当读取到客户端发送过来的信息后,就将这条信息放入双缓冲队列,等待被转发,并在信息中将clientId加入;
					msgQueue.offer(clientId+" says: "+info);
				}
			}catch(SocketException e){
				//断开连接的Client的Socket对象将在allOut列表中被删除;
				allOut.remove(clientId+ip);
				System.out.println(clientId+" has Disconnected.");
			}catch(Exception e){
				e.printStackTrace();
			}
			
		}
		
	}
	
	//内部类,定义一个线程体,用于循环读取双缓冲队列中的信息,并转发给所有客户端;
	private class SendInfoToClientHandler implements Runnable{

		@Override
		public void run() {
			try{
				//循环读取双缓冲队列
				while(true){
					int size=msgQueue.size();
					for(int i=0;i<size;i++){
						//获取信息;
						String info =msgQueue.poll();
						//转发给所有客户端;
						Set<String> set = allOut.keySet();
						for(String clientId:set){
							
							PrintWriter writer =new PrintWriter(allOut.get(clientId).getOutputStream());
							writer.println(info);
							writer.flush();
						}
					}
					//每次将所有信息都转发后都停顿一下,防止while语句做无意义的循环次数过多;
					Thread.sleep(100);
				}
			}catch(Exception e){
				e.printStackTrace();
			}
			
		}
		
	}
	
	
}



/////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////




package prototype;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.PrintWriter;
import java.net.Socket;
import java.net.SocketException;


//聊天室客户端：

public class ChatRoomClient {
	private String ClientId=null;
	private static BufferedReader uniqueReader;
	
	public static void main(String[] args) {
		ChatRoomClient client = new ChatRoomClient();
		client.start();
	}
	
	
private Socket socket;
	
	public ChatRoomClient(){
			//为了避免客户端因为等待客户端的ID输入流而发生线程的阻塞,导致无法接收其他客户端的请求,这里先要求输入用户名后再去请求连接;
			//如果是断线重连则不需要再次输入ID;
			if(ClientId==null){
			System.out.println("please type in your chat id : ");
			InputStream clientId = System.in;
			try {
				ClientId=new BufferedReader(new InputStreamReader(clientId)).readLine();
			} catch (IOException e) {
				e.printStackTrace();
				System.exit(1);
			}
		}
	}
	
	public void start(){
		while(true){
		try{
			//Socket构造方法参数1: 服务器IP地址;
			//Socket构造方法参数2：服务器程序申请的端口号;
			socket=new Socket("127.0.0.1",8898);  //IP地址也可使用"localhost";
			
			//向服务器传递此客户端的ID/在客户端保存此ID;
			PrintWriter sendId = new PrintWriter(socket.getOutputStream());
			sendId.println(ClientId);
			sendId.flush();
			BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
			String success = br.readLine();
			System.out.println(success);
			}catch(Exception e){
			System.out.println("服务器繁忙,正在尝试重新连接...");
			//e.printStackTrace();
			try {
				//给服务器重新启动的时间;
				Thread.sleep(10000);
			} catch (InterruptedException e1) {
				e1.printStackTrace();
			}
			continue;
		}
		try{
			//启动线程,用于读取服务器发送过来的信息并输出到控制台上：
			GetServerInfoHandler handler= new GetServerInfoHandler(socket);
			Thread thread = new Thread(handler);
			thread.start();
			
			OutputStream out = socket.getOutputStream();
			
			PrintWriter writer = new PrintWriter(out); //一般情况写字符信息PrintWriter要优于BuffredWriter,同样是缓冲高级流,它可以直接封装一个字节输出流;

			//创建一个输入流,用于读取键盘写入的内容;
			uniqueReader = new BufferedReader(new InputStreamReader(System.in));
			
			//循环读取键盘输入的信息,并通过输出流写到服务器去;
			String clientinfo = null;
			while((clientinfo=uniqueReader.readLine())!=null){
				writer.println(clientinfo);
				writer.flush();
			}
			}catch(Exception e){
				//e.printStackTrace();
				continue;
			}
		}
	}
	
	//内部类,启动一个线程,用于循环读取服务器发送过来的信息,单独启动一个线程
	//接收服务器发送的信息是为了不影响我们随时可以发送信息给服务器;两件不相干
	//的操作在某些情况会互相干扰,就应该写在两个不同的线程里;
	private class GetServerInfoHandler implements Runnable{
		private Socket socket;
		
		public GetServerInfoHandler(Socket socket){
			this.socket=socket;
		}
		
		@Override
		public void run(){
			try{
				//通过socket获取从服务端发送过来的输入流;
				InputStream in=socket.getInputStream();
				BufferedReader reader= new BufferedReader(new InputStreamReader(in));
				
				String info=null;
				//循环读取服务端发送过来的字符串信息,并输出到控制台;
				while((info=reader.readLine())!=null){
					System.out.println(info);
				}
				
			}catch(SocketException e){
				//e.printStackTrace();
				System.out.println("服务器发生异常,与服务器的连接已断开;");
				System.out.println("请输入任意内容来重新获取连接;");
				//利用捕获主线程中等待用户输入的流对象的空指针异常来捕获并让主线程重新运行来获取服务器连接;
				uniqueReader=null;
			}catch(Exception e){
				e.printStackTrace();
			}
			
		}
		
	}
	
}


/*

流程概要:

ChatRoomServer/Client总体分为server端和client端;

server端(ChatRoomServer类):

1.main method: 
(1)initialize ChatRoomServer类;
(2)调用start()方法;

2.ChatRoomServer类的属性和方法:

属性;
(1)private ServerSocket serverSocket
(2)private ExcutorService threadPool  //线程池,所有接收客户端文本消息输入流的线程都将放入此线程池;
(3)private HashTable<String Socket> allout //用于保存连接到服务器的用户ID及其socket;
(4)private LinkBlockingQueue<String> msgQueue //用于保存所有传入服务器的文本信息;

方法;
(1)Server构造方法;
(a)初始化ServerSocket对象serverSocket端口号8088;
(b)初始化线程池threadPool=Excutors.newCachedThreadPool();
(c)初始化双端缓冲队列msgQueue;

(2)start()方法;（整个start方法处于while(true)循环中等待客户端连入并处理状态,直到Server出现异常则继续循环开启连接等待客户端连接）;
(a)将SendInfoToClientHandler线程体放入一个新的线程并运行;
(b)利用Socket socket=serverSocket.accept();开始等待并接收客户端的连接;
(c)一旦接收到了一个客户端的请求(socket对象)则利用BufferedReader和socket.getInputStream()方法将此客户端处利用socket.getOutputStream()传出的用户ID:clintId(key)和其socket(value)一起放入allout映射中保存;
(d)初始化一个Handler对象,并将接收到的clintId和socket作为参数传入;将此线程体对象放入一个新的线程中,再将此线程交给线程池threadPool来操作其运行;


3.内部类;
(1)SendInfoToClientHandler内部类实现Runable接口,定义线程方法run():
循环读取msgQueue缓冲阻塞队列中所有的文本信息,只要队列中有元素,
就用for循环将每个元素对应的文本内容逐条取出,再将allout映射中的所有value(也就是socket)
取出,利用getOutputStream方法及PrintWriter流的println()方法将文本内容发送给所有的用户;
(2)Handler内部类实现了Runable接口,构造方法要求传入此线程对应的客户ID及其socket;
定义线程方法run():
利用socket参数用BufferedReader流的readLine()方法和socket.getInputStreatment()读取对应客户端的输出流;将接收到的文本信息用参数:客户ID+内容的形式重新包装后offer入msgQueue缓冲阻塞队列中保存;
作为输入流,也就是等待输出流响应的一方,如果输出流端断开连接那么输入流端将立即抛出socketException异常,所以此处需要处理这个异常,catch中告知服务器此用户ID已断开连接,
并且将此用户ID+IP(key)及其socket(value)从allout映射中删除,避免SendInfoToClientHandler线程发送文本信息给一个已经断开的客户端;


client端(ChatRoomClient类):

1.main method: 
(1)initialize ChatRoomClient类;
(2)调用start()方法;

2.ChatRoomClient类的属性和方法:

属性:
(1)String clientId
(2)static BufferedReader uniqueReader

方法:
(1)构造方法:
(a)通过System.in这个字节输入流要求使用此客户端的客户输入一个聊天时用的clientId,用BufferedReader接收;
如果之前用户已经输入过则无需再输入(防止与服务器断开连接后重连时再次输入ID);

(2)start()方法;（整个start方法处于while(true)循环中,一旦出现异常则继续循环重新试图建立与服务器的连接）;
(a)初始化一个socket对象Socket socket = new Socket("localhost",8088)来对服务器发送连接请求;
(b)一旦服务器端接收到了请求,客户端主线程将继续运行;利用socket.getOutputStream()向已连接的客户端将clientId发送过去;
(a)将GetSendInfoToClientHandler线程体放入一个新的线程并运行;
(b)利用System.in输入字节流和BufferedReader的readLine()方法,再利用
while(输入流对象.readLine()！=null)这样的结构不断等待并接收用户键盘传递的字节输入流以换行符号作为分段信号,将文本信息通过socket.getOutputStream和PrintWriter()传递给服务器;

3.内部类;
(1)GetSendInfoToClientHandler内部类实现Runable接口,相当于建立一个独立的线程体用来接收从服务器传递过来的文本信息,与传递文本消息给服务器的线程互不干扰,构造方法要求传入此客户端的socket对象,线程方法run():
利用socket的getInputStream()方法和BufferedReader流的readLine()方法接收从客户端传递过来的文本并显示在客户端的终端上;同样,与服务器端的Handler线程体异曲同工,当GetSendInfoToClientHandler
中发生了SocketException则告知用户服务器端发生了问题,与服务器的连接已断开;


注意:

1.与一般从硬盘读取内容的输入流不同的是,有些读取方法在读取到输入流相关内容的结尾时会返回null,但是如果遇到等待输入流,如System.in字节输入流配合读取方法要求客户在终端输入内容并读取,这种情况下,
线程将会在调用读取方法处等待用户的输入,读取后线程继续;同样socket相关的inputstream的读取方法也会等待另一端的outputstream的内容传递,所以如果利用类似while(BufferedReader.readLine()!=null){}这样的循环方法,
由于读取方法将以换行符作为标志读取一条信息,并且此方法的返回机制是只有当再次读取输入流中信息而其中已无内容时才会返回null,但是在此种循环语句中读取完一条消息后,输入流还存在,会继续要求客户输入,所以这样的循环不会返回null,
将持续等待和读取的动作直到出现异常,最后线程结束;

2.Handler方法体中socket对象的读取方法会等待并接受客户端传递过来的信息,但是如果客户端此时断开了连接,则在服务器端的此线程将会因为调用读取方法的socket连接对象已经消亡而报出异常;又因为此读取方法使用了注意点一中的while循环,
所以对客户端连接对象socket的等待和监测几乎是实时的,那么在此方法体中的异常捕获语句中来提示服务器端某个客户端的连接状态将是最准确和及时的;因此,在捕获到了socket异常之后就可以将此客户的socket和clientId键值对从allout映射中删除,
因为没有必要在SendInfoToClientHandler线程中向一个已经断开了连接的用户发送文本消息;
需要特别注意的是:只有一个等待输入信息的socket读取流所在的线程会因为输出数据的socket端断开而报出异常,而向另一个socket端发送信息的输出流线程不会因为另一端等待并接收信息的socket断开连接而报异常,
这也是为什么此程序会在Handler这个线程体中来监测客户端是否断开了与服务器的连接;




*/
