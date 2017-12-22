Event/EventListener 事件及监听


package prototype;

import java.awt.Color;
import java.awt.Font;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.awt.event.MouseMotionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;

public class SongEvent {   //事件及监听: 如果想让一个图形界面变得有意义,那就需要加入事件的处理;
//包,类,接口的关系：	
//java.awt.event包中有WindowListener,KeyListener,MouseListener等等接口;
//java.awt.event.KeyAdapter/WindowAdapter/MouseAdapter则是实现了WindowListener,KeyListener,MouseListener等等接口的适配器,包括java.util包中的EventListener--它们的父接口的类;
//可以用来继承并选择性覆写其中想要使用的监听方法;避免了实现接口必须实现所有抽象方法的累赘操作;

	 

//	java.lang.Object
//	  java.util.EventObject
//	      java.awt.AWTEvent
//	          java.awt.event.ComponentEvent
//	              java.awt.event.WindowEvent
///////////////////////////////////////////////
//	java.lang.Object
//	  java.util.EventObject
//	      java.awt.AWTEvent
//	          java.awt.event.ComponentEvent
//	              java.awt.event.InputEvent
//	                  java.awt.event.KeyEvent
///////////////////////////////////////////////
//	java.lang.Object
//	  java.util.EventObject
//	      java.awt.AWTEvent
//	          java.awt.event.ComponentEvent
//	              java.awt.event.InputEvent
//	                  java.awt.event.MouseEvent


	public static void main(String[] args) {
		//窗体事件监听：
//		JFrame frame = new JFrame("Welcome Exorcistata");
//		//frame.addWindowListener(new WindowEventDemo());//在窗体上安装事件监听;
//		
//		//虽然可以通过适配器如：java.awt.event.WindowAdapter来减少不必要的实现方法,但是如果此时只需要监听窗口关闭呢,用一整个类来达到目的是否也过于繁琐了呢？
//		//所以此种情况下可以引入匿名内部类的概念：
//		
//		frame.addWindowListener(new WindowAdapter(){     //利用匿名内部类来临时继承窗体适配器类并覆写需要的方法,就不用特意为监听窗口事件创建一个类了;
//			public void windowClosing(WindowEvent e) {
//				System.out.println("windowClosing --> 窗口关闭");
//			}
//
//		});
//		frame.setSize(300,150);
//		frame.setBackground(Color.WHITE);
//		frame.setLocation(300,200);
//		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);  //在没有调用这个方法之前窗口的关闭不会使程序结束;
//		frame.setVisible(true);
		//////////////////////////////////////////////////////////////////////////
		//按钮监听：
//		new ActionEventDemo();
		//键盘监听：
		new KeyEventDemo();
		//鼠标监听：
//		new MouseEventDemo();
		
	}

}

class WindowEventDemo implements WindowListener{

	@Override
	public void windowActivated(WindowEvent e) {
		System.out.println("windowActivated --> 窗口被选中");
	}

	@Override
	public void windowClosed(WindowEvent e) {
		System.out.println("windowClosed --> 窗口被关闭");
	}

	@Override
	public void windowClosing(WindowEvent e) {
		System.out.println("windowClosing --> 窗口关闭");
	}

	@Override
	public void windowDeactivated(WindowEvent e) {
		System.out.println("windowDeactivated --> 取消窗口选中");
	}

	@Override
	public void windowDeiconified(WindowEvent e) {
		System.out.println("windowDeiconified --> 窗口从最小化恢复");
	}

	@Override
	public void windowIconified(WindowEvent e) {
		System.out.println("windowIconified --> 窗口最小化");
	}

	@Override
	public void windowOpened(WindowEvent e) {
		System.out.println("windowOpened --> 窗口被打开");
	}
	
	
}

////////////////////////////////////////////////////////////////////////////
//按钮监听：

//对于按钮相关的事件之所以用ActionEvent和ActionListener,是由于ActionEvent相比KeyEvent和
//MouseEvent(空格键可以"按下按钮"——触发Button的 ActionEvent)是一个更抽象的高级事件,只要按钮被按下,无论是通过鼠标的点击还是空格键,
//都触发了ActionEvent,而ActionEvent看似包含了KeyEvent和MouseEvent等事件,但其实与它们并无从属关系,只是更加的抽象;


class ActionEventDemo{
	
	private JFrame frame =new JFrame("Therapistata!");
	private JButton button=new JButton("显示");
	private JLabel lab= new JLabel();
	private JTextField text= new JTextField(10);
	private JPanel pan =new JPanel();	
	public ActionEventDemo(){
		Font fn=new Font("swtor",Font.ITALIC+Font.BOLD,28); //设置粗斜体28号字样; 参数分别为：字体,风格,字号;
		lab.setFont(fn);//设置标签的显示文字;
		lab.setText("等待用户输入信息");
		button.addActionListener(new ActionListener(){ //利用匿名内部类方法在按钮上安装动作监听,一旦在按钮上监听到动作事件,就开始执行actionPerformed方法;
			public void actionPerformed(ActionEvent e) {
				//if(e.getSource instanceof JButton)作用和下一行的相同;
				if(e.getSource()==button){   //判断如果这个ActionEvent是在按钮组件上触发的;
					lab.setText(text.getText());//把文本框中的内容放入标签中显示;
				}
			}

		});
		frame.setLayout(new GridLayout(2,1,5,5)); //布局; 其中参数代表frame中添加的组件将会按照一定的顺序被摆放为2行1列(panel/label);
		pan.setLayout(new GridLayout(1,2,5,5));   //其中参数代表frame中添加的组件将会按照一定的顺序被摆放为1行2列(text/button);
		/*
		 GridLayout所切割出来的版面就如同表格一般整齐,加入的组件会按顺序由左至右/由上至下摆放,所以无法直接指定要摆放的区域;除此之外,组件放入后会变成方形,
		 所以不适合放入JButton这类组件中，而比较适合加入JPanel。GirdLayout类的信息如下：
	    public class GridLayout extends Object implements LayoutManager,Serializable
	          构造函数：
	    public GridLayout();
	    public GridLayout(int rows,int cols);
	    public GridLayout(int rows,int cols,int hgap,int vgap);
	          函数作用：建立一个表格的版面对象。rows代表有几行，cols代表有几列；hgap是组件之间的水平距离，vgap是组件之间的竖直距离

		 */
		pan.add(text);
		pan.add(button);
		frame.add(pan);
		frame.add(lab);
		frame.pack(); //Frame.pack()这个方法的作用就是根据窗口里面的布局及组件的preferedSize来确定frame的最佳大小;
		frame.setBackground(Color.WHITE);
		frame.setLocation(300,200);
		//frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
		frame.addWindowListener(new WindowAdapter(){   //这段代码实现了和上一天语句一样的功能;
			public void windowClosing(WindowEvent e){
				System.exit(1);
			}
		});
		frame.setVisible(true);
		
	}
	
}
//键盘监听：
class KeyEventDemo extends JFrame implements KeyListener{ //这里直接在需要被监听的类声明中加入implements KeyListener这样实现接口的操作可以不用重新声明一个监听类,也不用再使用匿名内部类来声明监听方法;
	private JTextArea text = new JTextArea();
	public KeyEventDemo(){
		super.setTitle("GTN access system 2.0"); //这里用super/this效果相同,因为此类已经继承了JFrame的各种方法并且并无覆写过;
		JScrollPane scr=new JScrollPane(text); 
		this.setLayout(null);
		scr.setBounds(0,0,300,200);  //setBounds方法属于绝对定位;这里如果要想让setBounds方法生效必须有上一行setLayout(null);这句语句,因为LayoutManager会默认帮助container来布置其中的所有组件,所以在这里利用 setSize / setLocation / setBounds等方法来设置组件的位置当然是无效的;
		this.add(scr);
		text.addKeyListener(this);
		this.addWindowListener(new WindowAdapter(){
		public void windowClosing(WindowEvent e){
			System.exit(1);
		}
		});
		super.setBounds(100,100,315,235);
		super.setVisible(true);
		}
	@Override
	public void keyPressed(KeyEvent e) {                    
		text.append(" 键盘 :"+KeyEvent.getKeyText(e.getKeyCode())+"键被按下"+'\n');//方法摘要：static String getKeyText(int keyCode) 返回 int getKeyCode()方法返回的键盘监听信息的 String表达形式,返回的是按钮的名字,如 "HOME"、"F1" 或 "A";
	}
	@Override
	public void keyReleased(KeyEvent e) {
		text.append(" 键盘 :"+KeyEvent.getKeyText(e.getKeyCode())+"键被松开\n");
		
	}
	@Override
	public void keyTyped(KeyEvent e) {                      //此事件在按下键盘后满足条件就能够被触发,而不是键盘被松开后;
		text.append(" 输入的内容是 :"+e.getKeyChar()+"\n");  //e.getKeyChar(); 返回与此事件中的键关联的字符而不会去得到类似shift/caps lock等非字符按键的值;
		
	}
	
}
//鼠标监听：
class MouseEventDemo extends JFrame implements MouseListener,MouseMotionListener{
	private JTextArea text = new JTextArea();
	public MouseEventDemo(){
		super.setTitle("GTN access system 2.0"); //这里用super/this效果相同,因为此类已经继承了JFrame的各种方法并且并无覆写过;
		JScrollPane scr=new JScrollPane(text);
		this.setLayout(null);
		scr.setBounds(0,0,300,200);
		this.add(scr);
		text.addMouseMotionListener(this);
		text.addMouseListener(this);
		this.addWindowListener(new WindowAdapter(){
		public void windowClosing(WindowEvent e){
			System.exit(1);
		}
		});
		super.setSize(315,235);
		super.setVisible(true);
		}


@Override
public void mouseClicked(MouseEvent e) {
	int c= e.getButton();     //返回的int值就对应了MouseEvent接口常量BUTTON后的编号(1/2/3);需要注意的是这里的getButton();方法是在鼠标按下并松开后获得;
	String mouseInfo=null;
	System.out.println(c);
	if(c==MouseEvent.BUTTON1){ //一旦监听到鼠标的左键动作;
		mouseInfo="左键";
	}
	if(c==MouseEvent.BUTTON3){ //一旦监听到鼠标的右键动作;
		mouseInfo="右键";
	}
	if(c==MouseEvent.BUTTON2){ //一旦监听到鼠标的滚轮动作;
		mouseInfo="滚轴";
	}
	text.append("鼠标单击： "+mouseInfo+"\n");
	
}

@Override
public void mouseEntered(MouseEvent e) {
	text.append("鼠标进入组件\n");
	
}

@Override
public void mouseExited(MouseEvent e) {
	text.append("鼠标离开组件\n");
	
}

@Override
public void mousePressed(MouseEvent e) {
	text.append("鼠标按下\n");
	
}

@Override
public void mouseReleased(MouseEvent e) {
	text.append("鼠标松开\n");
	
}


@Override
public void mouseDragged(MouseEvent e) {
	System.out.println("鼠标拖拽到：X= "+ e.getX()+", Y= "+e.getY());
	
}


@Override
public void mouseMoved(MouseEvent e) {
	System.out.println("鼠标移动到到：X= "+ e.getX()+", Y= "+e.getY());
	
}
	
}
//鼠标拖拽事件使用MouseMotionListener接口;它和MouseListener接口都是EventListener的子接口;

//补充：
//1.EventListener接口中包含了子接口KeyListener, ActionListener,MouseListener,WindowListener等;
//2.JTextField 是一个轻量级组件,它允许编辑单行文本;JTextArea 是一个显示纯文本的多行区域,也是一个轻量级组件;



