LoginGUI （用户登录）


package prototype;

import java.awt.Color;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JPasswordField;
import javax.swing.JTextField;

public class SongLoginGUI {   //用户登录图形界面 (refer to SongEvent.java);运用到了按钮事件监听(动作事件);键盘事件监听; 
	
	public static void main(String[] args) {
		new ActionEventDemo2();

	}
}
class LoginCheck{
	private String name;
	private String password;
	public LoginCheck(String name,String password){
		this.name=name;
		this.password=password;
	}
	public boolean validate(){
		if("exorcistata".equals(name)&&"555789".equals(password)){
			return true;
		}else{
			return false;
		}
	}
}
class ActionEventDemo2 extends KeyAdapter{
	
	private JFrame frame =new JFrame("Therapistata!");
	private JButton submit=new JButton("登陆");
	private JButton reset=new JButton("重置");
	private JLabel namelab= new JLabel("用户名：");
	private JLabel passwordlab= new JLabel("密   码：");
	private JLabel infolab= new JLabel("GTN access system");
	private Font fn=new Font("swtor",Font.ITALIC+Font.BOLD,12);
	private JTextField nametext= new JTextField(10);
	private JPasswordField passwordtext= new JPasswordField(); //密码文本输入框;
	//private JPanel pan =new JPanel();	
	public void keyPressed(KeyEvent e){
		String tempe =KeyEvent.getKeyText(e.getKeyCode());
		//System.out.println(tempe);
		if(tempe.equals("Enter")){
			String tname=nametext.getText();
			String tpass=new String(passwordtext.getPassword());//由于.getPassword()方法返回的是一个char[],所以只能通过new String()来接收;
			LoginCheck log =new LoginCheck(tname,tpass);
			if(log.validate()){
				infolab.setText("you can now access to the GTN ");
			}else{
				infolab.setText("Login failed ! ");
			}
		}
		
	
		//e.getSource()方法是获得发生了事件的对象,也可以理解为在哪个载体中有事件发生了,
		//下面的例子就是利用判断是在nametext这个JTextField对象上发生了事件,还是passwordtext这个JTextField对象上;
		if(tempe.equals("Down")&&(e.getSource()==nametext)){    //设置在ID输入窗口如果按"下"键则光标进入密码输入窗口;需要注意的是tab键的功能已经被JAVA默认设置为在窗口的组件中(从左往右从上往下)切换焦点,并且无法获取tab键的按键事件;
			passwordtext.requestFocus();
		}
		
		if(tempe.equals("Up")&&(e.getSource()==passwordtext)){  //设置在密码输入窗口如果按"上"键则光标进入ID输入窗口;
			nametext.requestFocus();
		}
		
	}
	
	public ActionEventDemo2(){
		infolab.setFont(fn);
		
		submit.addActionListener(new ActionListener(){ 
			public void actionPerformed(ActionEvent e) {
				//if(e.getSource instanceof JButton)作用和下一行的相同;
				if(e.getSource()==submit){   
					String tname=nametext.getText();
					String tpass=new String(passwordtext.getPassword());//由于.getPassword()方法返回的是一个char[],所以只能通过new String()来接收;
					LoginCheck log =new LoginCheck(tname,tpass);
					if(log.validate()){
						infolab.setText("you can now access to the GTN ");
					}else{
						infolab.setText("Login failed ! ");
					}
				}
			}

		});
		
		reset.addActionListener(new ActionListener(){
		public void actionPerformed(ActionEvent e){
			if(e.getSource() instanceof JButton){
				nametext.setText("");
				passwordtext.setText("");
				infolab.setText("GTN access system");
			}
		}
		});
		
		nametext.addKeyListener(this);     //使焦点在id输入框时按下Enter键能够进行相当于submit按钮的操作;
		passwordtext.addKeyListener(this); //使焦点在password输入框时按下Enter键能够进行相当于submit按钮的操作;
		
		
		frame.setLayout(null);
		namelab.setBounds(5,5,60,20);   //用绝对定位来代替setLayout方法;
		passwordlab.setBounds(5,30,60,20);
		infolab.setBounds(5,55,220,30);
		nametext.setBounds(65,5,100,20);
		passwordtext.setBounds(65,30,100,20);
		submit.setBounds(165,5,60,20);
		reset.setBounds(165,30,60,20);
		frame.add(namelab);
		frame.add(passwordlab);
		frame.add(infolab);
		frame.add(nametext);
		frame.add(passwordtext);
		frame.add(submit);
		frame.add(reset);
		frame.setSize(280,130);
		frame.setBackground(Color.WHITE);
		frame.setLocation(300,200);
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
		frame.setVisible(true);
		
	}
	
}

