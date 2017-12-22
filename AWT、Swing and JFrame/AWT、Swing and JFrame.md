AWT、Swing and JFrame

package prototype;

import java.awt.Color;
import java.awt.Dimension;
import java.awt.Point;

import javax.swing.JFrame;

public class SongAWTandSWING {   //介绍java.awt包和java.Swing包;虽然在java开发中已经比较少的去编写图形界面了,但是在某些情况下,这些工具是非常有用的;
	
	
	public static void main(String[] args) {
		JFrame f =new JFrame("测试swing窗体");
		
		Dimension d = new Dimension();     //设置窗体大小的封装类;
		d.setSize(530,200);
		f.setSize(d);
		//f.setSize(530,200); //设置窗体大小;
		
		Point p=new Point(300,200); //设置窗体坐标的封装类;
		f.setLocation(p);
		//f.setLocation(300,200); //设置窗口在屏幕上的显示位置;
		
		f.setBackground(Color.WHITE);
		
		//f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);  在没有调用这个方法之前窗口的关闭不会使程序结束;
		
		f.setVisible(true);
		
		
	}

}
/*
AWT (Abstract Window Toolkit) 抽象窗口工具包;

AWT中的所有工具都保存在java.awt包中,此包中的所有操作类可用来建立与平台无关的图形用户界面GUI(Graphical User Interface);
这些类又被称为组件(Components);
可见AWT主要用来创建桌面的应用程序;

AWT包中提供的工具类一共可以分为三类：
1.组件:    Component
2.容器：         Container
3.布局管理器： LayoutManager

                                                                                     java.lang.Object
                                                                                            | (java.awt)
事件类(java.awt.Event)   布局管理器接口(java.awt.LayoutManager)     组件类(java.awt.Component)       颜色类(java.awt.Color)    绘图类(java.awt.Graphics)     图型类(java.awt.Image)     字体类(java.awt.Font)      等等  
                                                                                 |
                                                                                             按钮(Button)         标签类(Label)     容器类(Container)       列表类(List)       滚动条类(Scrollbar)    等等
                                                     (容器本身也是一种组件,但是所有组件都应该放置在容器中,并可以设置其位置和大小)
                                                                                 |
                                              java.awt.Panel          javax.swing.JComponent          java.awt.ScrollPane           java.awt.Window
                                                                                 |                                                        |
                                                JScrollBar    JList      javax.swing.JPanel     JLabel                             Frame      Dialog   
                                                                                                                                     |
                                                                                                                             javax.swing.JFrame
 
 
AWT在开发的时候大量使用了windows函数,所以会造成移植的困难,属于重量级组件;
之后在java2中加入了轻量级组件开发包：javax.swing;   (javax代表扩展包)         
                                                                                                                      
                                                                                                                                    
*/

