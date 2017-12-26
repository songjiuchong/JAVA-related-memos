StudentAndScore （Score management system）


package testdemo;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;           

//提供了 1. imput score; 2.score list; 3.index; e.exit 四种可选择的服务：输入分数;打印分数列表;查询分数;退出;
//利用while(true)死循环内加条件语句break跳出循环的操作，来达到类似do while()但是实现了多分支条件判断的功能，使程序在
//正常情况下不断循环运行，特殊情况下退出循环;

public class StudentAndScore {       
	//设置学生姓名集合;
	static List<String> nameList;
    static List<Integer> scoreList;
    static Integer [] score;
    static String[] name;
    
	static{
	nameList = new ArrayList<String>(); //学生名单必须在运行前事先设置;
	nameList.add("s1");
	nameList.add("s2");
	nameList.add("s3");
	nameList.add("s4");
	nameList.add("s5");
	nameList.add("s6");
	scoreList = new ArrayList<Integer>(nameList.size()); //根据学生名单集合设置分数集合的长度;
	
	///////////////////////////////////////////
	
	name=new String[nameList.size()]; //声明一个学生姓名集合相同长度的String数;
	name=nameList.toArray(name);      //利用带参数的toArray方法来返回一个指定类型的数组,如果没有参数则默认返回一个Object类型的数组;
	
	score=new Integer[nameList.size()];//这里要特别注意的是：score数组的长度必须用nameList.size()来定义,因为.size()方法返回的是一个集合中已经存在的元素数,而不是在ArrayList()构造方法中传入的参数(它代表这个集合的容量,也就是一共能装多少元素,但是就算规定了其容量,也能够自动扩容,只是在元素数量达到容量之前提高了运行效率而已);
	score=scoreList.toArray(score);
	
	}
	
	public static void main(String[] args) {
		Scanner console=new Scanner(System.in);
		while(true){                                  //提供服务开始循环
			System.out.println("1. imput score; 2.score list; 3.index; e.exit");
			String cmd = console.nextLine();
			if("1".equals(cmd)){
				for(int i=0;i<name.length;i++){
					System.out.println("imput "+name[i]+" s'score: ");
					String imput=console.nextLine();  //推荐调用.nextLine()来获取输入内容
					if("e".equals(imput)){
						System.out.println("return to menu!");
						break;
					}
					try{
					int imputtemp=Integer.parseInt(imput); //由于使用了.nextLine()所以这里需要利用Integer.parseInt()来获取分数的int型
					if(!(imputtemp<=100&&imputtemp>=0)){
						System.out.println("Wrong score imputed, try again!");
						i--;
						continue;
					}
				score[i]=imputtemp;
				System.out.println("imput complete!");
				continue;}
					catch(NumberFormatException e){
						System.out.println("please imput numbers! try again!");
						i--;
						continue;
					}
					}
			}else if("2".equals(cmd)){
				int sum=0;
				for(int i =0;i<name.length;i++){
					System.out.print(name[i]+":"+'\t');
					System.out.println(score[i]);
					sum+=score[i];
			    }
				System.out.println("lump score :"+sum);
		    }else if("3".equals(cmd)){
		    	for(;;){
				System.out.println("please imput the name of the person u want to check on");
				boolean found=false;                //为了检查循环中是否已找到目标对象
				String index=console.nextLine();
				if("e".equals(index)){
					break;
				}
				for(int i =0;i<name.length;i++){
					if(name[i].equals(index)){      //一旦利用for循环找到目标姓名就通过数组下标输出对应分数
						System.out.println("the score of "+name[i]+" is: "+score[i]);
						found=true;
					}
				}
				if(!found){                        //如果未找到目标对象则报错
						System.out.println("no such a student,please try again!");
				}
		    	}
			}else if("e".equals(cmd)){
				System.out.println("exit!");
				break;
			}else{
				System.out.println("error occurred!");
				continue;
			}
		}
	}
}
