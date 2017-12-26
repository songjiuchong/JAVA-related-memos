Thread synchronized


package prototype;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

import prototype.BeanBank.PersonBean;



//实现线程方法一：

public class SongThread extends Thread{
	
	public void run(){
		for(int i=0; i<30;i++){
			System.out.println(getName()+":"+i); //getName()方法是从Thread继承过来的;
		}
		
	}
	
	public static void main(String [] args){
		
		SongThread tt1=new SongThread();
		SongThread tt2=new SongThread();
		
//		tt1.start(); //start()方法会调用run()方法; 
//		tt2.start();
		
//		tt1.run();  //并不会启动新的线程;相当于正常运行run()中的代码;
//		tt2.run();
		
////////////////////////////////////////////////////////////////////////////////////
		
		//实现线程方法二:
		
		Runnable run1 = new TestRunnable1();
		Runnable run2 = new TestRunnable2();
		
		Thread tt3=new Thread(run1);
		Thread tt4=new Thread(run2);
		
//		tt3.start();
//		tt4.start();
		

////////////////////////////////////////////////////////////////////////////////////

		//实现线程方法三：
		
		Thread tt5=new Thread(new Runnable(){

			@Override
			public void run() {
				for(int i=30; i<60;i++){
					System.out.println(Thread.currentThread().getName()+":"+i); 
				}
				
			}
			
		});
		
		Thread tt6=new Thread(new Runnable(){

			@Override
			public void run() {
				for(int i=30; i<60;i++){
					System.out.println(Thread.currentThread().getName()+":"+i); 
				}
				
			}
			
		});
		
//		tt5.start();
//		tt6.start();
		
		
		
////////////////////////////////////////////////////////////////////////////////////
		
//中断睡眠阻塞:
		
		//局部匿名内部类中若想引用该方法的其他局部变量,那么该变量必须是final的;
		final Thread tt7 = new Thread(new Runnable(){ 

			@Override
			public void run() {
				System.out.println(" wrecking ball ! ");
				
				try{
				//当前线程进入睡眠阻塞后,若被其他线程中断,这里就会引发中断异常;
					Thread.sleep(1000000);
				}catch(InterruptedException e){
//					e.printStackTrace();
					System.out.println("I should've let u in .");
				}
			}
		
		});
		
		Thread tt8 = new Thread(new Runnable(){

			@Override
			public void run() {
				for(int i=0;i<5;i++){
					System.out.println("Let me in,Bitch !");
					
					try{
						Thread.sleep(1000);
					}catch(InterruptedException e){
					e.printStackTrace();
				    }
			    }
				System.out.println("YEAH,I'M in,let the fuck begin ! ");
				//中断tt7;
				tt7.interrupt();
		     }	
		});
		
		
//		tt7.start();
//		tt8.start();
		
		
////////////////////////////////////////////////////////////////////////////////////
		
		//线程优先级：
		
		Thread max = new Thread(){
			@Override
			public void run(){
				for(int i=0;i<1000;i++){
					System.out.println("NEED FOR SPEED!");
				}
			}
		};
		
		Thread min = new Thread(){
			@Override
			public void run(){
			for(int i=0;i<1000;i++){
				System.out.println("Slow down , young man !");
			}
		  }
		};
		
		Thread normal = new Thread(){
			@Override
			public void run(){
			for(int i=0;i<1000;i++){
				System.out.println(" Michael! ");
			}
		  }
		};
		
		max.setPriority(Thread.MAX_PRIORITY);
		min.setPriority(Thread.MIN_PRIORITY);
//		normal.setPriority(Thread.NORM_PRIORITY);  //不设置就是默认优先级;
		
//		min.start();
//		normal.start();
//		max.start();
		
		
////////////////////////////////////////////////////////////////////////////////////
		
		//守护线程(后台线程)：
		
		//守护：
		Thread guardian =new Thread(){
			@Override
			public void run(){
				while(true){
					System.out.println(" save yourself !");
					try{
						Thread.sleep(1000);
					}catch(InterruptedException e){
						e.printStackTrace();
					}
				}
			}
		};
		
		//前台线程：
		Thread lord =new Thread(){
			@Override
			public void run(){
				for(int i =0; i<10;i++){
					System.out.println(" guard !");
					try{
						Thread.sleep(1000);
					}catch(InterruptedException e){
						e.printStackTrace();
					}
			    }
				System.out.println(" guardddddddddd !");
				System.out.println(" Ahhhhhh !");
		    }
		 };
		
		guardian.setDaemon(true);  //设置守护线程; 
//		guardian.start();
//		lord.start();  //由于之后没有语句了,所以当把lord线程启动后,main线程就消亡了;
		

////////////////////////////////////////////////////////////////////////////////////
		
//线程安全,及其解决办法：
		
		BeanBank beanbank = new BeanBank();
		PersonBean p1= beanbank.new PersonBean();
		PersonBean p2 = beanbank.new PersonBean();
		//p1线程和p2线程都是由beanbank这个实例创建的,那么p1/p2调用外部类的getBean()方法就
		//相当于调用的是beanbank这个实例的getBean()方法;此时这里就出现了两个线程同时访问
		//beanbank的getBean()方法,对其bean属性进行了操作,线程安全问题便出现了;
		
//		p1.start();
//		p2.start();
		//当getBean()方法还未声明为synchronized时,这里有一定几率会出现bean为负数的死循环,这就是线程异步发生的问题;
		
		
////////////////////////////////////////////////////////////////////////////////////
		
//wait()/notify()方法的应用：
		
		
		//下载图片的线程：
		final Thread download=new Thread(){
			
			public void run(){
				
				//开始下载;
				System.out.println("downloading picture 1...");
				try{
					Thread.sleep(5000);
				}catch(InterruptedException e){
					System.out.println("error! picture 1 downloading process has been interruptted!");
					return;     //一但下载被中断,则让线程消亡;
				}
				WaitNotifyDemo.finish=true;
				System.out.println("download completed !");
				
				synchronized(this){
				this.notify();  //这里不能使用download.notify()是由于download还未初始化,但是只有在匿名内部类代码执行完毕后初始化才完成;
				}
				
				
					System.out.println("此处执行接下去的操作,若再无操作,则notify()方法可以省略.");
				
				}
		};
		
		
		//显示图片的线程：
		final Thread show = new Thread(){
			public void run(){
				
				while(true){
				try {
					synchronized(download){
					download.wait();  //在download对象上等待;不管是图片下载完成或是下载线程被中断后结束,这里的wait()都将重新获得对象锁继续运行;
					break;
					}
				} catch (InterruptedException e) {
					System.out.println("showing process has been interruptted,rebooting..."); 
					if(!WaitNotifyDemo.finish){ //如果show线程在wait()方法等待期间被打断,那会导致直接执行之后的显示图片程序,而此时下载并不一定已经完成,所以必须判断图片的下载状态后重新进入wait();
					if(download.isAlive()){     //判断如果图片下载状态为false且download线程还存在则rebooting,否则说明download线程被中断后已经消亡;
						continue;
					}else{
						System.out.println("picture download failed !");
						return;                 //图片下载失败,无法显示,结束方法;
						}  
					}
					}  
				}
				
				if(!WaitNotifyDemo.finish){
					System.out.println("picture download failed !");
				}else{
				System.out.println("show picture !");  //假设从这条语句开始运行显示已下载图片的程序;
				}
			}
		};
		
		Thread interference1 = new Thread(){
			
			public void run(){
		    try {
				Thread.sleep(2000);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			download.interrupt();
			}
		};
		
        Thread interference2 = new Thread(){
			
			public void run(){
		    try {
				Thread.sleep(2000);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			show.interrupt();
			}
		};
		
	
//		download.start();
//		interference1.start();
//		interference2.start();
//		show.start();
		
		
////////////////////////////////////////////////////////////////////////////////////////////////////		
		//join的用法;
		
		Thread jointest = new Thread(){
			public void run(){
					for(int i =1;i<51;i++){
						System.out.println(i);
					}
			}
		};
		
//		jointest.start();
		
		try {
			jointest.join();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		for(int i =1;i<51;i++){
			System.out.println(i);  //由于jointest线程启动的同时主线程也在执行,所以在还未添加join语句之前,输出一定不是1-50这样的顺序输出;
		}
		
		
	}
}


/////////////////////////////////////////////////////////////////////////////////////////////////////

class WaitNotifyDemo{
	
	public static boolean finish = false;  //是否下载完成;
	
}

/////////////////////////////////////////////////////////////////////////////////////////////////////


class TestRunnable1 implements Runnable{

	@Override
	public void run() {
		for(int i=30; i<60;i++){
			System.out.println(Thread.currentThread().getName()+":"+i); 
		}
		
	}
	
}

class TestRunnable2 implements Runnable{

	@Override
	public void run() {
		for(int i=30; i<60;i++){
			System.out.println(Thread.currentThread().getName()+":"+i); 
		}
		
	}
	
}


/////////////////////////////////////////////////////////////////////////////////////////////////////

class BeanBank{
	int bean = 20;
	
	public synchronized int getBean(){  //如果在此处加上synchronized关键字使方法同步,保证bean这个数据在经过if语句检查之前只能被一个线程修改;
	if(bean==0){	
		throw new RuntimeException("Out of bean !");
	}
	Thread.yield();  //放弃当前running状态重新回到runnable;增加线程异步问题发生的几率;
	return bean--;   //当bean等于1时,两个线程在未达到if(bean==0)判断前,各自在此处对bean进行了--操作,则
	                 //bean的值变为负数,跳过了bean==0的检查进入死循环;
	}
	
	//内部类,一个新的线程：
	class PersonBean extends Thread{
		public void run(){ //run()方法如果出现未捕获异常,那么此线程将会消亡,但不会影响进程中的其他线程;
			while(true){
				System.out.println(getName()+":bean="+getBean());  //当Bean值为0时,getBean方法中会抛出RuntimeException,此处会抛到run()方法外;
				Thread.yield();  //放弃当前running状态重新回到runnable;增加线程异步问题发生的几率;
			}
		}
	}
}


/////////////////////////////////////////////////////////////////////////////////////////////////////




/*
 
//进程/线程：
  
  
现在的操作系统都是支持多任务的,而这个多任务通常是通过多进程实现的,在每一个进程的内部,
又可以通过多线程实现进程内部的多任务;

多进程：在操作系统中能(同时)运行多个任务(程序);

多线程：在同一应用程序中有多个顺序流(同时)执行;

在每一个进程的内部都有一个作为程序主体的线程,我们称为主线程;Java中,每一个JVM其实就是
一个进程,而其主线程就是main();

当我们运行一个java程序时,操作系统会启动一个进程来运行这个程序,而虚拟机启动起来后,
会创建第一个线程去执行main方法;


//Java线程原理和机制：

在大多数操作系统中,线程的原理是通过CPU时间片实现的,CPU时间片就是CPU的一段执行单位时间,
这段时间很短,我们感知不到,但是CPU可以做很多操作;
假如有三个线程并行(同时运行);其实CPU把自己的时间片分给了三个线程,然后拥有时间片的三个
线程随机获得当前时间片所对应时间段的CPU运行权,无论哪个线程获得了此时间片对应时间段的
运行权,运行完成后它将失去此时间段对应的时间片(如果没有消亡的话 )它将进入下一个时间片对应的时间段再进行随机获得运行权;

线程调度机制：线程时间片的分配是随机且非绝对均匀的,如t1,t2两个线程,它们并非一定是交替获得CPU使用权的,
可能是t1,t1,t1,t2,这样不均匀的CPU分配;

对于单核的计算机来说,(多核相当于有多个CPU运作,这里先不讨论),其实不存在真正意义上的"同时运行",只是在人的感知上觉得计算机
在一个单位时间内同时完成了多个线程的操作,其实只是我们感知不到其运行的先后罢了;所以并行是针对
时间段来说的,对于时间点来说不存在并行;不过我们还是承认它是属于并行的;

从宏观上看,所有程序是同时运行的,微观上是走走停停的,这种现象叫做:并发运行;
对于操作系统而言,并发执行的任务叫做进程;
对于程序而言,并发执行的任务叫做线程;


//Thread/Runnable:

虚拟CPU,也就是之前所说的时间片,由java.lang.Thread类封装和实现;
代码和数据由java.lang.Runnable接口提供;它规定了可以运行的任务;

CPU所执行的代码/处理的数据都传递给Tread类对象;

public class Thread extends Object implements Runnable;

每个线程都是通过某个特定Thread对象对应的run()方法来完成其操作的,run()称为线程体;

使用start()方法,线程进入Runnable状态,它将向线程调度器注册这个线程;
调用start()方法并不一定马上会执行这个线程,它只是进入了Runnable而不是Running：

注意：不要直接在程序中调用run()方法,虽然不会报错而且会运行其中的代码但是并没有实现新的线程;


//实现线程方法：

一：extends Thread;

直接继承Thread类并重写run()方法;

Thread.currentThread();  //静态方法取得当前线程;
getName();  //取得线程名;


二：implements Runnable;

    Thread t = new Thread(Runnable target);

通过实现Runnable接口并实现接口中的唯一方法run(),并将此实现类传递给一个创建好的Thread对象,再
利用对象的start()方法开始;

使用Runnable接口就是要将线程与任务解耦;不能让一个线程被指定一种可执行任务,而是让Runnable去指定某项任务,
而线程随时可以选择在何处何时进行何种任务;线程与其并发要执行的任务分开处理可以使得程序更灵活,尤其在线程池中有明显体现;


 三：匿名内部类：

 当多线程任务仅在一处用到,可以使用匿名内部类实现Runnable;
 

 注意：Thread是一个典型的使用了模板模式的类,在其中封装了线程启动的复杂过程,因为所有线程启动的方式
 是一样的,那么不同点就在于线程要并发执行的任务,所以要求子类重写run方法;



 //线程的结束：

(1):线程达到其run()方法的末尾;

(2):线程抛出一个未捕获的Exception/Error;\

(3):调用 Deprecated stop(); 不建议使用,存在问题! 


//后台线程：
  
 后台线程是在后台运行的,它的任务是为其他线程提供服务,被称为"Daemon Thread",或"守护线程";

Java中的垃圾回收就是一个后台线程;后台线程的生命周期依赖于其他的非后台线程,只有当所有其它线程
都消亡了(而不只是主线程),它才会结束;

在一个线程启动前,调用setDaemon(boolean on)方法设置参数为true来设置此线程为守护线程;
  
  
//线程相关方法：
  
sleep(long millis);  //Thread的静态方法;线程睡眠指定时间,单位为毫秒; 并非绝对精准,受系统计时器等因素的影响;
  
  
wait();  //让线程进入等待状态; 是Objecct下的方法; 需要注意的是,如果被等待的线程消亡,并且wait()方法所在的程序块重新获得了对象的锁,wait()方法会自动解除而不需要notify方法的唤醒;
                             还需要特别注意的是,如果在wait()方法开始执行时,所等待的线程已经消亡或从未开始,则此wait()方法将会一直处于等待状态;
                             最后要说明的是,由于wait()是Object类的方法,所以非线程对象也可以调用,但是由于非线程对象不存在消亡(调用了此对象或拥有此对象锁的线程释放了锁或者消亡了也不属于适用范围),
                             那就需要此对象再次调用notify()方法来使得调用wait()方法的线程得以继续运行;

notify()/notifyAll();  //将所有在当前对象上等待的线程全部解除等待阻塞;是Objecct下的方法;

方法概要：假设线程A调用obj的wait()方法,线程B调用了obj.notify()方法;那这两个线程必须必须先获得obj的锁,也就是说
包含obj.wait()/notify()的语句必须写在synchronized(obj){...}代码块内;当程序运行到了obj.wait()方法后,
线程A就释放了obj的锁并进入了等待阻塞,整个线程A停止运行;由于线程A释放了obj的锁,使得线程B有机会获得obj的锁并进入
包含obj.notify()的语句块执行此语句,此时在obj对象上等待的线程A被唤醒,由等待阻塞重新进入runnable状态,但是目前线程B
仍旧拥有obj对象的锁,所以只有当线程B运行到释放obj对象锁之后,线程A才能重新获得obj对象的锁,然后继续执行obj.wait()方法
之后的语句;这也是调用了wait()/notify()方法的对象必须处于同步状态的原因;

需要注意的是：
当一个线程调用了某个对象.wait();方法,另一个线程同样调用了这个对象的.wait()方法,
如果此时在这两个线程外部使用了此方法.notify()方法,JVM将随机选择一个线程解除等待阻塞,
所以想要解除两个线程的等待阻塞就要调用两次notify()方法,或者使用notifyAll()方法;


void interrupt();  //中断线程; 如果线程在调用 Object 类的 wait()/wait(long)或 wait(long, int)方法,
 或者该类的 join()/join(long)/join(long, int)/sleep(long)或 sleep(long, int)方法过程中受阻,则其中断状态将被清除,它还将收到一个 InterruptedException;
 

yield();  //"让位"方法,线程放弃当前的执行,回到runnable状态等待CPU分配时间片;
  
suspend(); resume(); //挂起/唤醒; Deprecated 不推荐使用;

join();  //调用这个方法的当前线程,会让出CPU的控制权,等待加入的子线程结束后再继续运行;
join(long millis);  //等待子线程若干毫秒后无论子线程是否消亡了都会继续主线程之后的语句;


其实join方法在JDK中的源码为:

public final void join() throws InterruptedException {
    join(0);
}
public final synchronized void join(long millis)
throws InterruptedException {
    long base = System.currentTimeMillis();
    long now = 0;
    if (millis < 0) {
        throw new IllegalArgumentException("timeout value is negative");
    }
    if (millis == 0) {
        while (isAlive()) {
            wait(0);
        }
    } else {
        while (isAlive()) {
            long delay = millis - now;
            if (delay <= 0) {
                break;
            }
            wait(delay);
            now = System.currentTimeMillis() - base;
        }
    }
}

所以不难看出,join方法底层就是通过wait()方法和isAlive()判断方法来实现的,所以对join的调用可以理解为等待相应时间或者线程消亡的wait()方法,而不去使用notify()相关方法;
而且需要注意的是join()相关方法是Thread类中的方法,所以必须用线程对象去调用,概念明显比单独使用wait()方法更清晰;

 
final void setPriority(int p);  //设置线程的优先级;优先级越高的线程理论上被分配时间片的机会越多;

int getPriority();  //获得线程优先级等级;

优先级的取值范围(参数)：
常量 MAX_PRIORITY  优先级最高,值为10;
常量 MIN_PRIORITY  优先级最低,值为1;
常量 NORM_PRIORITY 优先级默认,值为5;

线程优先级不能保证一定按照优先级的方式执行,线程并发运行时是靠线程调度机制分配时间片做到的,分配时间片是不可控的;


System.gc();  //通知虚拟机尽快做一次回收工作,但不能保证调用了此方法虚拟机就会马上去回收垃圾;有点类似线程的调用机制;



//线程状态图：

新线程调用start(); 进入Runnable等待线程调度状态,获得CPU运行权后进入Running运行状态,此时可能会遇到
阻塞事件,比如：sleep()/join()/等待用户输入等,进入阻塞状态(Blocked),在解除阻塞状态之后线程会回到
Runnable等待线程调度状态而不是直接回到Running状态;需要注意的是如果一个线程用完了它的时间片,也就是
当它使用完了CPU执行权后会回到Runnable等待状态并领取了新的时间片;同样,当一个线程调用了yield()方法
它也会从Running状态回到Runnable状态;


new -> start() -> Runnable -> Running -> (Blocked) -> Runnable -> Running -> Dead
                                      ->  Runnable -> Running  -> Dead



//对象互斥锁 (synchronized)：
 
   简称对象锁,又称为同步锁;用来保证共享数据操作的完整性;
   
   同步和异步：

   同步：执行有先后顺序;
   异步：执行没有顺序;各执行各的;
   
    
  线程异步操作的安全问题：
  当多线程访问同一段资源时,可能产生线程安全问题,此时,我们需要将异步的访问操作变成同步的;
  换句话说,对于这段资源的引用,多线程要有先后顺序的访问,一个线程访问结束,另一个线程才可以访问;
   
 
   每一个对象都对应一个"互斥锁"的标记,这个标记用来保证在任意时刻只能有一个线程有权限访问该对象;

   关键字synchronized来与对象的互斥锁联系,当某个对象用synchronized修饰时,表明该对象在任意时刻只能被
   一个线程访问;
 
 
 synchronized用法：
 
 
 1.通过在方法声明中加入 synchronized关键字来声明 synchronized方法;  
 
 synchronized 方法控制对类成员变量的访问:每个类实例对应一把锁,每个 synchronized方法都必须获得调用该方法的类实例的锁方能执行,
 否则所属线程阻塞,方法一旦执行,就独占该锁,直到从该方法返回时才将锁释放,此后被阻塞的线程方能获得该锁,重新进入可执行状态;
 这种机制确保了同一时刻对于一个处于同步状态的对象,其所有声明为 synchronized的成员函数中至多只有一个处于可执行状态(因为至多只有一个能够获得该类实例对应的锁),
 从而有效避免了类成员变量的访问冲突(只要所有可能访问类成员变量的方法均被声明为 synchronized); 在 Java中,不光是类实例,每一个类也对应一把锁,
  这样我们也可将类的静态成员函数声明为 synchronized ,以控制其对类的静态成员变量的访问;



 2.通过 synchronized关键字来声明synchronized块; 
 
 synchronized(syncObject) 
 {   
 //允许访问控制的代码 
 } 
  
   synchronized 块是这样一个代码块,其中的代码必须获得对象 syncObject(如前所述,可以是类实例或类)的锁方能执行,
   具体机制同前所述;由于可以针对任意代码块,且可任意指定上锁的对象,故灵活性较高;
   
 
 synchronized用法总结：
  
  用法1时：当一个线程访问到一个synchronized声明的方法时,包括在方法中用synchronized(this)语句限制的方法,此线程获得调用该方法的对象/类的锁,
  其他线程中的此对象/类无法访问任何该对象所在类中被声明为synchronized的方法/任何同样用synchronized(this)语句限制的方法,
  直到拥有锁的线程结束了对synchronized方法/用synchronized(this)语句限制的方法的访问,其他拥有此对象的线程才能重新获得该对象的锁去访问这些方法;
  
  用法2时：用synchronized来指定需要上锁的对象,并声明限制语句块;这种用法更自由,因为它可以限制非上锁对象所在类中的方法,语句块中的内容可以随意
  指定,需要被上锁的对象也可以随意指定,同样,在同一时刻所有被synchronized(syncObject)限制的语句块都只能被当前获得syncObject对象锁的线程执行,直到
  它执行完毕,其他拥有syncObject对象的线程才能去访问这些方法;(参考: LastSong Tetris 中的GameService类中的synchronized语句块);
  
  无论是那种情况,哪种声明锁的方式,一旦一个线程获得该对象的锁,与此对象相关的一切synchronized方法以及语句块都被此线程独占;
  
 
 //注意：
  
(1)如果两个或以上的线程都修改一个对象,那么把执行修改的方法定义为同步方法,如果对象的更新影响到只读方法,
 那么只读方法也要定义为同步的;
 
(2)同一个线程中的不同方法上施加了同一个对象的锁,那么同时调用这些方法不会发生同步,因为同步锁的机制是在不同的线程中施加了同一个对象锁的方法之间保持同步;
 
(3)不要滥用同步,因为它会减低线程的效率,如果在一个对象内的不同方法访问的不是用一个数据,就不要将方法设置为synchronized的;
 
 java.lang.StringBuffer/java.util.Vector就是SUN滥用同步(线程安全)的例子,导致使用这些类的程序效率降低,不过好在之后有StringBuilder/ArrayList来弥补这些问题;
 
 
 线程安全的类/不安全的类：

StringBuilder   StringBuffer

ArrayList       Vector

HashMap         HashTable
 
 
 //释放锁：
   
  如果一个线程一致占用一个对象的锁,则其他的线程将永远无法访问该对象,因此,需要在适当的时候,将对象锁归还;
  
  1.当线程执行到synchronized()块结束时,释放对象锁;
  
  2.当在synchronized()块中遇到break跳出同步语句块,return或抛出未捕获异常时自动释放对象锁;
 
 
 //死锁：
   
  指线程之间无限制的等待对方释放锁;
   
  死锁的预防与避免：
  
 1.顺序上锁; 例如：A等待B的锁,B等待C的锁..
 2.反向解锁; 例如：C的锁先解开,然后B的锁解开,最后A的锁解开;
 3.不要回调; 例如：A等待B的锁,B等待C的锁,C又去等待A的锁;
 
 
 
 //线程安全的集合：
 
 java.util.Collections提供多个静态方法帮助集合实现线程安全;
 
 static <T> List<T>  synchronizedList(List<T> list);  //返回指定列表支持的同步（线程安全的）列表;
          
 static <K,V> Map<K,V>  synchronizedMap(Map<K,V> m);  //返回由指定映射支持的同步（线程安全的）映射;
          
 static <T> Set<T>  synchronizedSet(Set<T> s);   //返回指定 set 支持的同步（线程安全的）set;
      
      
 //注意：
  
 private static List<Task> taskQueue = Collections.synchronizedList(new LinkedList<Task>());

  可以得到本身不是线程安全的容易的线程安全的状态,但是要注意的是线程安全仅仅指的是如果直接使用它提供的函数,
  比如：queue.add(obj); 或者 queue.poll(obj);这样我们自己不需要做任何同步;
  但如果是非原子操作,比如：
   if(!queue.isEmpty()) {  
      queue.poll(obj);  
      }  
      
  我们很难保证,在调用了isEmpty()之后,poll()之前,这个queue没有被其他线程修改;
  所以对于这种情况,我们还是需要自己同步：
    synchronized(queue) {  
        if(!queue.isEmpty()) {  
           queue.poll(obj);  
        }  
    }  
 
 
 
 
//线程池：

当我们需要并发执行任务时,会频繁的创建线程,并在执行任务后销毁线程;
由于创建线程和销毁线程的性能开销比较大,对程序执行的效率影响较高,所以
我们使用另一种方式：首先创建一批空线程,当有任务需要执行时,取出一个去
执行任务,当任务执行完毕后,将线程回收,等待下一次执行任务;这样我们就节省
出创建和销毁线程的资源,从而提高程序的运行效率;

java中的线程池：java.util.concurrent 包中的 ExecutorService继承了Executor接口;  //实现了线程池的功能;

创建线程池的方法需要使用线程池工具类：Executors;

Executors.newCachedThreadPool();  //创建一个根据需要创新线程的线程池;根据任务数量创建线程,任务结束会回收线程,但是如果某些线程长时间处于空置状态它也会去销毁这些线程;扩容性较好;

Executors.newFixedThreadPool(int size);  //创建一个指定大小的线程池; 如果任务多于线程池中线程总数,多出的任务会等待线程的回收,并处于阻塞状态;但是这个线程池不会自主销毁空置线程;

Executors.newScheduledThreadPool(int size);  //创建一个指定大小的线程池,可以指定任务一个延迟时间,在超过这个时间后运行任务;

Executors.newSingleThreadExecutor();  //创建一个只有一个线程的Executor;相当于只指定了一个线程的线程池,Executors.newFixedThreadPool(1);


关于线程池的shutdown()和shutdownNow()方法;
线程的暂停有两个方法 shutdown 与 shutdownNow 两个方法的调用都会阻止新任务的提交;
shutdown会继续执行并且完成所有已经登记而未执行的任务;
shutdownNow会清除所有登记后还未执行的任务并且在已经运行的所有线程上调用interrupt()方法;
在调用了这两个方法后如果还有任务通过execute()方法试图登记到线程池会抛出RejectedExecutionException异常;

需要注意的是shutdownNow会对所有池中正在运行的线程执行interrupt()方法,但是这种方法的作用有限,如果线程中没有类似sleep,wait,Condition,定时锁等阻塞线程的操作,
interrupt()方法是无法中断当前的线程的;所以,ShutdownNow()并不代表线程池就一定立即就能退出,它可能必须要等待所有正在执行的任务都执行完成了才能退出;
 
 
//java中的(双端)缓冲队列：
在存操作同步/取操作同步的基础上,可以保证存和取可以异步操作;比单缓冲效率要高一些;

位于java.util.concurrent包中;

(双)缓冲队列;  //是线程安全的队列,当有多线程要对队列进行存取操作时,最好使用(双)缓冲队列;

接口:
BlockingDeque接口(继承了BlockingQueue接口):支持两个附加操作的 Queue,这两个操作是：获取元素时等待双端队列变为非空;存储元素时等待双端队列中的空间变得可用;
BlockingQueue接口(继承了Queue接口):支持两个附加操作的 Queue,这两个操作是：获取元素时等待队列变为非空,以及存储元素时等待空间变得可用;

实现类:
LinkedBlockingDeque实现类(实现了BlockingDeque接口); //不定长的双缓冲队列,队列最大值可以为int最大值,该队列提供了一个带有int值的构造方法,可以变为定长的队列;
LinkedBlockingQueue实现类(实现了BlockingQueue接口);

ArrayBlockingQueue实现类(实现了BlockingQueue接口); //规定大小的双缓冲队列,其构造方法要求传入一个int值,指定队列长度;存取数据本着先进先出的原则;如果存入数据超出规定长度,则进入等待;

PriorityBlockingQueue;实现类(实现了BlockingQueue接口); //类似于LinkedBlockingQueue,但存取数据是本着自然排序原则或者在构造方法中传入比较器,按照比较规则存取;

SynchronousQueue;实现类(实现了BlockingQueue接口);   //特殊的队列,存取必须满足存一次取一次的顺序;

注意:缓冲队列(阻塞队列)相比其他原始队列的最大区别就在于两个方法;
put(E e); //将指定的元素插入此双端队列表示的队列中,即此(双端)队列的尾部,必要时将一直等待可用空间;
take();   //获取并移除此双端队列表示的队列的头部,即此(双端)队列的尾部,必要时将一直等待可用元素;
 


//////////////////////////////////////////////////////////////////////////////////////////////////////////////
 

终极实验室(推荐复制到另一个测试类中测试)

public class test {
	
	public static void main(String[] args) {
		Thread test1 = new Thread(new test().new Stest1());
		//Thread test2 = new Thread(new test.Stest2());
		Thread test2 = new Thread(new Stest2());
		//注意:如果Stest2类改为非静态的,那么又由于Stest1和Stest2都是test的内部类,那么他们可以通过在需要同步的方法体上添加synchronized(test.this)来保持同步;
		
		//test1.start();
		//test2.start();
		//Stest3.start();
		//Stest4.start();
		//test.Stest5();
		test t1 = new test();
		t1.Stest6();
		t1.Stest5();
		
	}
	
	//通过修改以下6个Stest(内部类或是Thread对象或是方法)中run方法中synchronized()代码块的参数类型
	//可以测试synchronized代码块的运行原理;
	
	//synchronized的监视器只能被看做是一个标识,对这个对象或类中属性的修改和读取是否保持同步主要靠对
	//相应方法施加同步锁来实现的;
	//如果需要保持同步的修改或访问成员变量的方法不存在被不同的线程中的相同实例对象访问,也就是说每一个
	//调用此方法的线程一定是修改或访问各自不同的对象中的数据,此种情况下也不需要对这些方法施加同步锁;
	
	private class Stest1 implements Runnable {

		@Override
		public void run() {
			//将一个类的类对象作为monitor(监视器,也就是锁住此方法的对象锁);
			synchronized(test.class){ 
				for(int i=0;i<=5;i++){
					try {
						Thread.sleep(1000);
						System.out.println("test1:"+i);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				}
			}
		}
	}
	
	private static class Stest2 extends Thread {

		@Override
		public void run() {
			synchronized(Stest4){
				for(int i=0;i<=5;i++){
					try {
						Thread.sleep(1000);
						System.out.println("test2:"+i);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				}
			}
		}
	}
	
	private static Thread Stest3 = new Thread(new Runnable(){

		@Override
		public void run() {
			//注意:此处的this并不是调用此run方法的thread对象test3,而是
			//实现了Runnable接口的一个匿名类的实例,所以尽量不要在实现Runnable接口的run方法中
			//使用synchronized(this);
			synchronized(this){
				for(int i=0;i<=5;i++){
					try {
						Thread.sleep(1000);
						System.out.println("test3:"+i);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				}
			}
		}
	});
		
		private static Thread Stest4 = new Thread(){
			
			@Override
			//在run方法前加上synchronized修饰符相当于将整个run方法至于synchronized(this)中;
			public synchronized void run() {
				//synchronized(this){
					for(int i=0;i<=5;i++){
						try {
							Thread.sleep(1000);
							System.out.println("test4:"+i);
						} catch (InterruptedException e) {
							e.printStackTrace();
						}
					}
				//}
			}
		};
		
		//如果通过test.Stest5()直接用类来调用这个方法,此方法将获得test类对象的锁,与Stest1保持同步;
		public  static synchronized void Stest5(){ 
			//synchronized(this){   //在静态方法中无法使用this变量;
					for(int i=0;i<=5;i++){
						try {
							Thread.sleep(1000);
							System.out.println("test5:"+i);
						} catch (InterruptedException e) {
							e.printStackTrace();
						}
					}
			//}
		}
		
		//当run方法中synchronized(tt)使用对象锁时,用同一个实例对象调用Stest6方法再调用Stest5方法,发现无法同步;
		//而使用synchronized(test.class)类锁时就能同步了,说明无论用实例对象还是直接用类来调用一个static的synchronized的方法,这个方法的监视器一定是
		//类对象;
		public void Stest6(){	
			final test tt = this;
			Thread t1 = new Thread(new Runnable(){

				@Override
				public void run() {
					synchronized(test.class){
						for(int i=0;i<=5;i++){
							try {
								Thread.sleep(1000);
								System.err.println("test6:"+i);  //注意:这里使用System.err.println();输出将是红色字体,方便识别;
							} catch (InterruptedException e) {
								e.printStackTrace();
							}
						}
					}
				}
			});
			t1.start();
		}
}



补充:

关于synchronized和lock;

Lock是JDK1.5以后引进的,它相比synchronized关键词要更灵活,更精确,由于一些扩展的功能使得其可操作性更强;
在Java1.5中，synchronize是性能低效的;因为这是一个重量级操作,需要调用操作接口,导致有可能加锁消耗的系统时间比加锁以外的操作还多;
相比之下使用Java提供的Lock对象,性能更高一些;但是到了Java1.6,发生了变化;synchronize在语义上很清晰,可以进行很多优化;

主要相同点:lock能完成synchronized所实现的所有功能;
主要不同点:lock有比synchronized更精确的线程语义和更好的性能;synchronized会自动释放锁,而Lock一定要求程序员手工释放,并且必须在finally从句中释放;

(1)ReentrantLock拥有Synchronized相同的并发性和内存语义,此外还多了锁投票,定时锁等候和中断锁等候;
     如:线程A和B都要获取对象O的锁定，假设A获取了对象O锁，B将等待A释放对O的锁定;
     如果使用 synchronized,如果A不释放,B将一直等下去,不能被中断;
     如果使用ReentrantLock,如果A不释放,可以使B在等待了足够长的时间以后,中断等待,而干别的事情;
 
    ReentrantLock获取锁定与三种方式：
    a)lock(),如果获取了锁立即返回,如果别的线程持有锁,当前线程则一直处于休眠状态,直到获取锁;
    b)tryLock(),如果获取了锁立即返回true，如果别的线程正持有锁,立即返回false;
    c)tryLock(long timeout,TimeUnit unit),例如:lock.tryLock(100,TimeUnit.NANOSECONDS);
                如果获取了锁定立即返回true，如果别的线程正持有锁,会等待参数给定的时间,在等待的过程中,如果获取了锁定,就返回true,如果等待超时,返回false;
                
    注意:
    tryLock()无论返回true还是false都不会阻塞线程;
    
    d)lockInterruptibly:如果获取了锁定立即返回,如果没有获取锁定,当前线程处于休眠状态,直到获得锁定,或者当前线程被别的线程中断;
           
    注意:
    1.lockInterruptibly()允许在等待时由其他线程的Thread.interrupt()方法来中断等待线程而直接返回,这时是不用获取锁的,而会抛出一个InterruptException;
           所以lock.lockInterruptibly()必须放在try块中或者在调用lockInterruptibly()的方法外声明抛出InterruptedException;
           而ReentrantLock.lock()方法则不允许Thread.interrupt()中断,即使检测到了Thread.interruptted一样会继续尝试获取锁,失败则继续休眠;只是在最后获取锁成功之后在把当前线程置为interrupted状态;
    2.interrupt()是用来设置中断状态的;返回true说明中断状态被设置了而不是被清除了;我们调用sleep、wait等此类可中断(throw InterruptedException)方法时,一旦方法抛出InterruptedException，当前调用该方法的线程的中断状态就会被jvm自动清除了,就是说我们调用该线程的isInterrupted 方法时是返回false;
          如果你想保持中断状态,可以再次调用interrupt方法设置中断状态;这样做的原因是,java的中断并不是真正的中断线程,而只设置标志位(中断位)来通知用户;如果你捕获到中断异常,说明当前线程已经被中断,不需要继续保持中断位;
	interrupted是静态方法,返回的是当前线程的中断状态;例如,如果当前线程被中断(没有抛出中断异常,否则中断状态就会被清除),你调用interrupted方法,第一次会返回true;然后,当前线程的中断状态被方法内部清除了;第二次调用时就会返回false;如果你刚开始一直调用isInterrupted,则会一直返回true,除非中间线程的中断状态被其他操作清除了;
 	3.当一个线程获取了锁之后,是不会被interrupt()方法中断的;因为本身在前面的文章中讲过单独调用interrupt()方法不能中断正在运行过程中的线程,只能中断阻塞过程中的线程;
 	4.当通过lockInterruptibly()方法获取某个锁时,如果不能获取到,只有进行等待的情况下,是可以响应中断的,而用synchronized修饰的话,当一个线程处于等待某个锁的状态,是无法被中断的,只有一直等待下去;
	5.读写锁将对一个资源(比如文件)的访问分成了2个锁,一个读锁和一个写锁;正因为有了读写锁,才使得多个线程之间的读操作不会发生冲突;ReadWriteLock就是读写锁,它是一个接口,ReentrantReadWriteLock实现了这个接口;
	可以通过readLock()获取读锁,通过writeLock()获取写锁;
 	
(2)synchronized是托管给JVM执行的,监控synchronized的锁定,在代码执行时出现异常,JVM会自动释放锁定,但是使用Lock则不行,lock是通过代码实现的,要保证锁定一定会被释放,就必须将unLock()放到finally{}中,防止发生异常后出现死锁;
 
(3)synchronized原始采用的是CPU类似悲观锁机制,即线程获得的是独占锁;独占锁意味着其他线程只能依靠阻塞来等待线程释放锁;而在CPU转换线程阻塞时会引起线程上下文切换,当有很多线程竞争锁的时候,会引起CPU频繁的上下文切换导致效率很低;
而Lock用的是乐观锁方式;所谓乐观锁就是,每次不加锁而是假设没有冲突而去完成某项操作,然后通过添加版本号的机制检测是否有冲突存在,如果因为冲突失败就重试,直到成功为止;
在资源竞争不是很激烈的情况下,Synchronized的性能要优于ReetrantLock,但是在资源竞争很激烈的情况下,Synchronized的性能会下降,但是ReetrantLock的性能能维持常态;


关于Condition类中的await()和signal();

例子:子线程和主线程交替运行一定的次数(子线程先执行6次输出,主线程再执行6次输出,这样交替循环6次);

(1)利用synchronized,wait,notify来实现:

public class test {
	
	public static void main(String[] args) {
		test.Ini g = new test().new Ini();
		g.ini();
	}
	
	private class Ini {
	private boolean switcher = true;
	
	public void ini(){
		Thread son = new Thread(new Runnable(){
			@Override
			public void run() {
				for(int i = 0;i<6;i++){
					if(!switcher){
						synchronized(Ini.this){
						try {
							Ini.this.wait();
						} catch (InterruptedException e) {
							e.printStackTrace();
						}
					}
				}
					
				for(int j=0;j<6;j++){
					try {
						Thread.sleep(1000);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				System.out.println("son:i="+i+"j="+j);
				}
					
				switcher=false;
				synchronized(Ini.this){
				Ini.this.notify();
				}
				}
		   }
		});
		
		son.start();
		
		for(int i = 0;i<6;i++){
			if(switcher){
				synchronized(this){
					try {
						this.wait();
					} catch (InterruptedException e) {
						e.printStackTrace();
					}	
			    }
			}
		for(int j=0;j<6;j++){
				try {
					Thread.sleep(1000);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
		System.err.println("main:i="+i+"j="+j);
		}
			
		switcher=true;
		synchronized(this){
		this.notify();
		}
		}
	}
    }
	
}

(2)利用:
import java.util.concurrent.locks.Condition;  //Condition方便了各种需要互相等待的线程在不同条件下进行细致分类,实现了多队列的等待和唤醒;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
来实现;

public class ThreadTest{
	private static Lock lock = new ReentrantLock(); 
	private static Condition subThreadCondition = lock.newCondition(); 
	private static boolean bBhouldSubThread = false; 
	
	public static void main(String []args){

			ExecutorService threadPool = Executors.newFixedThreadPool(1); 
			threadPool.execute(new Runnable(){ 

				public void run() 
				{ 
					for(int i=0;i<6;i++) 
					{ 
						lock.lock();		
						try 
						{		
							if(bBhouldSubThread) 
								subThreadCondition.await(); 
							for(int j=0;j<6;j++) 
							{ 
								System.out.println("son" + ",j=" + j); 
							} 
							bBhouldSubThread = true; 
							subThreadCondition.signal(); 
						}catch(Exception e) 
						{				 
						} 
						finally 
						{ 
							lock.unlock(); 
						} 
					}
				} 
			}); 
			threadPool.shutdown(); 
			for(int i=0;i<6;i++) 
			{ 
					lock.lock();			
					try 
					{ 
						if(!bBhouldSubThread) 
								subThreadCondition.await();						
						for(int j=0;j<6;j++) 
						{ 
							System.out.println("main" + ",j=" + j); 
						} 
						bBhouldSubThread = false; 
						subThreadCondition.signal();			
					}catch(Exception e) 
					{				
					} 
					finally 
					{ 
						lock.unlock(); 
					}		
			} 
		} 
	} 
    
   
   
补充:
   //阻塞队列的实现;
   class BlockingQueue {
   final Lock lock = new ReentrantLock();
   final Condition notFull  = lock.newCondition(); 
   final Condition notEmpty = lock.newCondition(); 

   final Object[] items = new Object[100];
   int putptr, takeptr, count;

   public void put(Object x) throws InterruptedException {
     lock.lock();
     try {
       while (count == items.length) 
         notFull.await();
       items[putptr] = x; 
       if (++putptr == items.length) putptr = 0;
       ++count;
       notEmpty.signal();
     } finally {
       lock.unlock();
     }
   }

   public Object take() throws InterruptedException {
     lock.lock();
     try {
       while (count == 0) 
         notEmpty.await();
       Object x = items[takeptr]; 
       if (++takeptr == items.length) takeptr = 0;
       --count;
       notFull.signal();
       return x;
     } finally {
       lock.unlock();
     }
   } 
 }

*/




