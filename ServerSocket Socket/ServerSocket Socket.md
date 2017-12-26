ServerSocket/Socket （Service/Client）

package prototype;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;



//Socket/ServerSocket;


public class SongWebServer {
	
	public static void main(String [] args) throws IOException{
		
		//服务器端：
		
		ServerSocket ss= new ServerSocket(2222);
		System.out.println("server start...");
		Socket so = ss.accept();  //阻塞,等待客户端的连接;
		System.out.println("connected. ");
		
		DataInputStream dis = new DataInputStream(so.getInputStream());
		DataOutputStream dos=new DataOutputStream(so.getOutputStream());
		String word =dis.readUTF();  //接受来自客户端的字符串信息;
		dos.writeUTF("from server:"+word);  //返回客户端的字符串信息;
		
		
		dis.close();
		dos.close();
		so.close();
		ss.close();
		
	}

}


/*

网络：

          把分布在不同地理区域的计算机与专门的外部设备用通信线路互联成一个规模宏大,
功能强大的网络系统,从而使中多的计算机可以方便地互相传递信息,共享硬件/软件/数据信息等资源;


计算甲联网可以实现：
1.使用远程资源;
2.共享信息/程序/数据;
3.分布式处理： 多个计算机分多个步骤完成同一个任务;


按照网络规模和范围可以简单划分为：

局域网(LAN, Local Area Network);
都市网(MAN, Metropolis Area Network);
广域网(WAN, Wide Area Network);

根据网络拓扑结构可以分为：
星型网络,总线网络,环线网络,树型网络等;

Internet的形成和发展：

第一阶段 (1969-1983):1969年,美国ARPANET诞生,用于军事研究;1983年,TCP/IP应用到ARPANET中,
使得internet得以迅速发展;以ARPANET为中心,组成了新的互联网(internet);

第二阶段 (1983-1994):用于教育和科研的NSFNET(National Science Foundation Net)形成;

第三阶段 (1994-NOW): Internet的商业化运作;


OSI的分成思想：

OSI：开放系统互连 (Open System Interconnection)：采用分成的结构化技术;
分层的理由：将网络简化/模块化的设计网络;

OSI参考模型 (OSI/RM: Open System Interconnection/Reference Model);
共分为7层：最高层为用户层,最底层为物理层; 

1.应用(用户)
2.表示
3.会话
4.传输
5.网络
6.数据链路
7.物理


TCP/IP  TCP(Transmission Control Protocol /Internet Protocol 传输控制协议/互联网协议):
 是Internet上不同系统之间互联的一组协议,它为分散的不同类型的硬件提供了一个通用的编程接口,
 TCP/IP协议使Internet尽可能称为一个分散/无序的网络;


TCP/IP通常被看成一个4层模型：
应用
传输
网络
数据链路+物理


IP地址的定义：

为了实现Internet上不同计算机之间的通信,每台计算机都必须有一个不与其他计算机重复的地址--IP地址;

IP地址是数字型的,32bit,由4个8为的二进制数组成,每8位之间用圆点隔开,比如：192.168.0.1; 范围：0-255.0-255.0-255.0-255;

IP地址被分成了A/B/C/D/E五类,每个类别的网络标识和主机标识都有自己的规则;


端口(port):

一台计算机上只有一张网卡,只有一个IP地址,但是外界网络传入
信息的时候无法分辨是传递给计算机上哪一个应用程序的,而不同的应用程序
向计算机申请注册不同的端口可以解决这个问题,使得外界网路在分发信息的
时候可以准确的定位到相关的应用程序,所以端口号也可以被视为计算机为了
有序地接收外界网络信息而声名的编号;

是一种抽象的软件结构,包括一些数据结构和I/O缓冲区,端口号范围(0-65535);通常将它分为三类：

公认端口 (Well Known Ports)：从0-1023,它们紧密绑定(Binging)一些服务; 不能被自由使用;

注册端口 (Registered Ports): 从1024-49151,它们松散的绑定一些服务,可以被自由使用;

动态/私有端口 (Dynamic/Private Ports):从49152-65535,不稳定,很可能已被占用,不推荐自由使用;


套接字 (Socket):

 表示一个系统的IP地址和端口号的结合; 是TCP/IP连接的一个端点,用来处理两个流对象;
 可以这么说,两台计算机想要互联只需要2个Socket交互就可以了;
 
C/S模式：客户端-服务器端 client-server;
C/S中,客户端程序要独立开发;Server也要独立开发;

C/S的优势：客户端可以实现更高/复杂的效果;易操作性好;
C/S的劣势：维护难度高;


B/S结构：浏览器-服务器 Browser/Server,(一种特殊的C/S结构);

B/S的优势：客户端统一(就是浏览器,不需要经常更新,更新的内容由所访问网站决定);通信协议统一(http协议);维护难度低;
B/S的劣势：客户端可实现的效果有限;


 创建TCP Socket需要的四个信息：
 
 本地系统的IP/本地应用程序使用的TCP端口号;
 
 远程系统的IP/远程应用程序使用的TCP端口号;
 

java.net.ServerSocket/java.net.Socket两个类用于建立一个双边的通信:

ServerSocket用于打开服务端口,等待客户端连接,运行在服务端;

Socket用于连接指定服务器的指定端口;运行在客户端;


ServerSocket类:

用于侦听一个客户端的Socket连接,如果没有连接,它将一直等待;

构造器：

ServerSocket(int port): 用指定的端口port来创建一个侦听Socket;

ServerSocket(int port,int backlog):加上一个用来改变连接队列长度的参数backlog;

ServerSocket(int port,int backlog,InetAddress localAddr):在机器存在多个IP地址的情况下,允许通过
localAddr这个IP地址类的对象参数来指定侦听的IP地址;

ServerSocket方法：

Socket accept(); //连接上之后返回一个Socket对象;此对象的作用是使服务器和客户端的信息交互;

close();  //ServerSocket/Socket都需要关闭;



Socket类：

构造器：(一旦创建了Socket的实例,就自动根据给定的地址和端口连接服务端了;)

Socket(); //基本不用;

Socket(InetAddress address,int port);  //参数1:服务器IP地址; 参数2：服务器程序申请的端口号;

Socket(InetAddress address,int port,InetAddress localAddr,int localPort);

Socket(String host, int port);  //每一台计算机都有一个本机的IP地址：

1:localhost;
2:127.0.0.1;
3:本机IP:xxx.xxx.xxx.xxx(看自己机器的ip);  //控制台：ipconfig (window下); \sbin\ifconfig (linux下);


Socket方法：

getInputStream();

getOutputStream();

close();



*/


//////////////////////////////////////////////////////
//////////////////////////////////////////////////////



package prototype;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.Socket;
import java.net.UnknownHostException;



public class SongWebClient {  //refer to SongWebServer !

	public static void main(String[] args) throws UnknownHostException, IOException {
		//客户端：
		
		Socket so = new Socket("127.0.0.1",2222);  //这里Socket()的的参数应该为服务器的IP和端口,此处用本机IP作为服务器IP;
		
		DataInputStream dis = new DataInputStream(so.getInputStream());
		DataOutputStream dos = new DataOutputStream(so.getOutputStream());
		
		dos.writeUTF("hello");  //发送给客户端; 注意顺序,服务器端是先读,所以这里是先写,需要交错;
		String st=dis.readUTF();  //接收客户端内容;
		
		System.out.println(st);
		
		dis.close();
		dos.close();
		so.close();

	}

}

