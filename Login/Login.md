Login

package prototype;

public class SongLogin {
	public static void main(String[] args) {             //主方法是客户端,要尽量的简洁明了,流程明确,作为login客户端,只需
		                                                 //接受用户输入的信息,然后反馈最终结果即可;
		Operation oper=new Operation(args);
		System.out.println(oper.str);
	}

}


class Check{                                          //check类只是一个用于验证用户信息是否合法的模块,他的目的是检验在用户名/密码
	                                                  //是否正确,返回boolean即完成任务将会被Operation实例化;
	public boolean validate(String name,String code){
		if(name.equals("songjch")&&code.equals("doomdoom999")){
			return true;
		}else{
			return false;
		}
	}
	
	public boolean formCheck(String []info ){
		if(info.length!=2){
			return false;
		}else{
			return true;
		}
	}
	
}


class Operation{                                        //Operation用来实现主体操作,将作为接受主方法参数和最终返还给主方法结果
	                                                    //的类,会被主方法实例化;
	private String name;
	private String code;
	private String info[];                              //通过构造方法把主方法的参数导入,获取用户名密码
	String str=null;                                    //str将作为最后主方法的输出内容;
	
	public Operation(String info[]){
		this.info=info;
		if(!new Check().formCheck(info)) {
		str="用户名,密码输入格式错误,请重新输入!";
		}else{
		
		this.name=info[0];
		this.code=info[1];
		
		if(new Check().validate(this.name,this.code)){
			this.str="宋九重先生,欢迎回来!";
		}else{
			this.str="身份验证错误,无法进入!";
		     }
		     }
	}
}


