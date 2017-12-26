PointsCards（点卡生成，查询，充值）

package testdemo;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Scanner;
import java.util.Set;

//此程序的概念是先进的：
//1.利用映射保存随机生成的账号,密码;生成过的卡号将被保存在一个Set集合中,每一次生成新的卡号都会遍历检查此集合,避免重复;
//2.利用亦或操作增强保密性(把日期利用SimpleDateFormat/亦或融入点卡账号密码之中即增强保密性,又便利之后对点卡生成日期的考察);
//3.静态No变量即记录了所有点卡的真实序列,点卡生成总数,又利用亦或被融入了账号,方便根据账号迅速得知此卡序号,又增强了安全性;
//4.用户充值系统,在充值成功后把充值后的点卡的used属性改为true;用户充值时先对比此充值点卡是否在点卡映射中且此卡号对应的点卡对象其used属性为false,如果满足再去验证账号密码;而used属性也方便了管理员查询点卡信息时可以得知此点卡是否已被充值过;
//5.管理员查询系统; 目标卡号对应卡对象的所有信息/点卡总体数量情况的汇总;

//但是此程序没有IO流/数据库的支持,待改进;

//TODO ;


public class PointsCardsDemo {      
	//实现点卡生成,查询,用户充值三项功能,此程序建立在理想化的服务器存档功能的实现上：存储已生成点卡和已使用点卡的映射,
	//验证生成密码是否重复的数组(以天为单位重置),包括记录总生成卡量的No;它们在运行一次程序后能被保存在服务器而不会在下一次运行程序时重置;
	public static void main(String[] args) throws Exception {
		PointsCards.GenerateMap(10);
		PointsCards.check("2014060900");
		PointsCards.check("1153606273");
		
		//模拟客户端运作..
		RechargeClient.recharge();
		///////////////////////////////
		PointsCards.check("1153606273");
	}
}

class PointsCards{
	
	long code; //9位密码，因为数组元素个数的限制(int max),检查重复数组的元素有限,目前只能适应9位密码的验证;
	long id; //卡号,卡号在理论上不会重复,一旦被用户使用过此卡后,通过卡号在Map映射中删除此卡信息;
	private boolean used ; //在点卡对象中保存其是否已被使用过的状态;
	
	static long idSaftyCode = 1020140616; //用于和No异或后生成隐藏了其No的id,也方便之后的查询;注意:由于之后此固定密码会和date进行亦或然后生成期望长度为10位的id所以要保证此固定密码的第一位肯定不能为2;
	private static long No = 1; //点卡实际生成的编号,此数字理论上也不会重复;
	private static long usedNo = 0; //创建一个保存已被使用的点卡数的变量,方便调查;
	private static long nouseNo = 0; //创建一个保存空点卡数的变量,方便调查;
	
	static Set<Long> idBank = new HashSet<Long>(); //用一个静态的Set集合来保存已经生成过的点卡id,用来遍历检查新生成的id是否已经存在,确保id的唯一性;
	static Map<PointsCards,Long> cardsBank=new HashMap<PointsCards,Long>();  //已经生成的点卡的映射，用于之后再次添加新点卡与用户使用点卡后对作废点卡的删除;

	
	//////////////////////////////////////////////////////////
	
	static Date datetemp =new Date();      //获取一个当天日期，作为点卡的一个可被查询的属性;
	static SimpleDateFormat df = new SimpleDateFormat("yyyyMMddHH"); //精确到秒,而且是10位;
	static String datestring = df.format(datetemp);
	static long date =Long.parseLong(datestring);//点卡生成的当天日期时间,将用于和随机生成的密码/点卡id结合,方便查询日期/NO;
	
	////////////////////////////////////////////////////////////
	
	static PointsCards c; //保存最近一次生成的点卡信息以方便把Cards放入Map映射的方法的执行;
	
	///////////////////////////////////////
	
	public static long getNo() {
		return No;
	}
	public static void setNo(long no) {
		No = no;
	}
	
	public boolean getUsed(){
		return this.used;
	}
	
	public void setUsed(boolean used){
		this.used=used;
	}
	
	public static void addUsedNo() {  //使记录已使用的点卡数的数据增加1;
		PointsCards.usedNo++;
	}
	
	public static long getUsedNo() {
		return usedNo;
	}
	
	public static void addNouseNo() {
		PointsCards.nouseNo++;
	}
	public static long getNouseNo() {
		return nouseNo;
	}

///////////////////////////////////////////////////////////////	
	public static PointsCards GeneratePointsCards(){
		c=new PointsCards();
		while(true){
		c.code=(long)(Math.random()*10000000000L);//如果不标明为L型,超出int范围会报错，详见：simplemath2.0;
		if(c.code<=1000000000){
			continue;
		}
		break;
		}
		
		c1:
		while(true){
			
			c.id=(idSaftyCode+No)^date;  //利用固密码数加NO亦或date的方法,在查询时再次与date亦或可以得到No,而date可以从此点卡对象的密码中提取;
			
			for(Long tempId:idBank){		
				if(tempId==c.id){
					PointsCards.addNouseNo();  //空卡数加1;
					No++;                 //如果发生id重复,则把No加1,相当于生成过了一张具有重复id的卡,但是这张卡将不会被赋予任何实质内容,也不会被使用,是一张空卡;
					continue c1;
				}
			}
			PointsCards.idBank.add(c.id); //如果在idBank中未发现重复的id则把新的id加入idBank;
			No++;
			return c;
		}
	}
	
/////////////////////////////////////////////////////////////
	
	public static void GenerateMap(int q) throws Exception{ //参数控制了生成点卡的数量;
		if(q>1000){
		throw new Exception("q can not above 1000 !"); //如果生成超过1000张点卡,会抛出异常;
		}
		for(int i=0;i<q;i++){
		cardsBank.put(PointsCards.GeneratePointsCards(),PointsCards.c.code^date); //把生成的点卡对象与其验证码（同样运用了亦或来加密保存点卡生成日期,查询时利用其密码亦或即可）放入映射;
		System.out.println(PointsCards.c.id+" has been generated! ");
		}
		}
	
	
	//////////////////////////////////////////////////
	public int hashCode(){    //重写hashCode方法，令id相同的元素被视为相同;
		int temp=this.getClass().hashCode();
		return (int)(temp*43+id);
		}
	
		
	///////////////////////////////////////
	public boolean equals(Object obj){  //重写equals方法;
		if(obj==null){
		return false;
		}else if(obj instanceof PointsCards){
		PointsCards temp=(PointsCards)obj;
		return this.id==temp.id;
		}else{
		return false;
		}
		}
	
		
	///////////////////////////////////////
	
	public String toString(){   //重写toString方法;
		return "id: "+this.id+" code: "+this.code;
		}
	
		
	//////////////////////////////////////
	
	public static void check(String id){
		
	System.out.println("目前生成的总点卡数: "+(PointsCards.getNo()-1)+'\n'+"空卡数: "+PointsCards.getNouseNo()+'\n'+"已被使用的点卡数: "+PointsCards.getUsedNo()); //每次查询首先把点卡数情况总览告知一遍查询者;
	boolean find=false;
	long idtemp=0;
	String usedInfo;
	try{
	idtemp=(long)Integer.parseInt(id); //可能会抛出NumberFormatException();
	}
	catch(NumberFormatException e){
	e.printStackTrace();
	}
	Set<PointsCards> cardsInfo=cardsBank.keySet();
	for(PointsCards temp:cardsInfo){
	if(idtemp==temp.id){
	long value=cardsBank.get(temp);
	long tempDate=value^temp.code;  //通过保存在cardsBank中的点卡对象的code和其对应value重新获得初始化此卡时保存的date;
	if(temp.getUsed()==true){
		usedInfo="This card has already been used ! ";
	}else{
		usedInfo="This is a new card ! ";
	}
	System.out.println("card info:"+'\n'+temp+'\n'+usedInfo+'\n'+"card No: "+((temp.id^tempDate)-idSaftyCode)+";"+'\n'+"the date of releasing: "+tempDate+";");
	find=true;
	break;
	}
	}
	
	if(!find)
	System.out.println("this id doesn't exist ! ");
	}
}

///////////////////////////////////////////////////////////////

class RechargeClient{
	public static void recharge(){     //客户端程序,理想化状态是,此客户端源码保存在公司服务器中,用户通过输入来远程上传程序所需的数据,之后程序把输出结果远程传输在用户的电脑上;
		Scanner console = new Scanner(System.in);
		String clid=null;
		String clcode=null;
		long clid2=0;
		long clcode2=0;
		c2:
		while(true){
			System.out.println("Do you want to recharge or exit? imput R/E to continue!");
			clid=console.nextLine();
			if("E".equals(clid)){
			System.out.println("Have a nice day! cya!");
			//System.exit(0);  为了主方法能够继续执行下面的语句,这里暂时不退出程序,而是跳出这个客户端方法;
			break;
			}
			if(!"R".equals(clid)){
				System.out.println("Error occoured,please choose again!");
				continue;
			}
			System.out.println("please imput the id of the card you want to recharge:");
			clid=console.nextLine();
			System.out.println("please imput the code of the card: ");
			clcode=console.nextLine();
			
			try{
				clid2=(long)Integer.parseInt(clid);
			}
			catch(NumberFormatException e){
				System.out.println("This card doesn't exist or has already been used! please check the id and code and try again!");
				continue;
			}
			try{
				clcode2=Long.parseLong(clcode);
			}
			catch(NumberFormatException e){
				System.out.println("This card doesn't exist or has already been used! please check the id and code and try again!");
				continue;
			}
			Set<PointsCards> cardsInfo=PointsCards.cardsBank.keySet();
			for(PointsCards temp:cardsInfo){
			if(clid2==temp.id&&(!temp.getUsed())){
				if(clcode2==temp.code){
					System.out.println("recharge succeed!");
					temp.setUsed(true);
					PointsCards.addUsedNo();
					continue c2;
				}
			System.out.println("This card doesn't exist or has already been used! please check the id and code and try again!");
			}
			}
			System.out.println("This card doesn't exist or has already been used! please check the id and code and try again!");
			continue;
		}
	}
}



