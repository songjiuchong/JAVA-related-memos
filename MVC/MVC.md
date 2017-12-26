MVC

package prototype;

public class MVC {

	public static void main(String[] args) {
	}
}


/*

mvc(model,view,controller);

(1)mvc是什么?

mvc是一种软件架构的思想,其核心是,将一个软件划分成三种不同类型的模块,分别是
模型(封装业务逻辑),视图(提供输入和输出)和控制器(用来协调模型和视图,即视图向控制器发送请求,
控制器选择相应的模型来处理;模型处理的结果也要给控制器,由控制器来选择合适的视图来展现模型返回的
处理结果);

使用mvc的一个最主要的原因,是因为mvc可以实现模型的复用;

a,模型不关心处理结果如何展现;
比如:一个模型查询出了一些处理结果,这些结果可能是字符串或者是xml,然后这些处理结果可以交给
不同的视图来展现(如以表格的方式,图形的方式);

b,可以利用不同的视图来访问模型;

(2)如何使用mvc来开发一个web应用程序;

1)使用java类来实现模型,
     使用servlet或者filter来实现控制器,
     使用jsp来实现视图;
2)所有请求要发给控制器(servlet/filter),由分析器分析请求资源路径,然后调用相应的模型(java类)来处理,
模型返回的处理结果要交给控制器,让它来选择合适的视图(jsp),将处理结果展现给用户;


mvc实例:根据用户名和余额来申请贷款;

创建web工程:
在工程下创建一个db.sql的文件;

db.sql:

drop table if exists t_account;

create table t_account(
	id int primary key auto_increment,
	accountNo varchar(16)unique,
	balance double
)type=innodb;

insert into t_account(accountNo,balance) values('6225881003102000',1000);


创建Model：

package entity;

public class Account{
	//与数据库中的保存的账户属性一一对应;
	private int id;
	private String accountNo;
	private double balance;
	setter/getter...
	
	public String toString(){
		return accountNo+""+balance;
	}
}

将DBUtil类,数据库驱动以及配置文件引入;
package dao;

import entity.Account;

public class AccountDAO{
	public Account findByAccountNo(String accountNo)
	throws SQLException{
		Account a = null;
		PreparedStatement prep = null;
		ResultSet rst = null;
		try{
		Connection conn = DBUtil.getConnection();
		prep = conn.prepareStatement("select * from t_account where accountNo=?")
		prep.etString(1,accountNo);
		rst = prep.executeQuery();
		if(rst.next()){
			a = new Account();
			a.setAccountNo(accountNo);
			a.setBalance(rst.getDouble("balance"));
			a.setId(rst.getInt("id"));
		}
		}catch(SQLException){
			e.printStackTrace();
			throw e;
		}finally{
			if(rst!=null){
				rst.close();
			}if(prep!=null){
				prep.close();
			}
			DBUtil.close();
		}
		return a;
	}
}

packet service;

public class AccountService{
	//通过帐号和贷款金额来判断是否申请成功,返回值是序号;
	public String apply(String accountNo,double amount)
	throws SQLException,AccountNotExistException,AccountLimitException{
		String number="";
		//查看帐号是否存在;
		AccountDAO dao = new AccountDAO();
		Account a =dao.findByAccountNo(accountNo);
		if(a==null){
			//应用异常;由于用户错误的操作引起的,输入了一个错误的帐号,
			//需要提示用户采取正确的操作;
			throw new AccountNotExistException();
		}
		//检查余额是否充足;
		if(a.getBalance()*10<amount){
			//余额不足;
			throw new AccountLimitException();
		}
		//生成序号,该序号需要保存到数据库;
		Random r = new Random();
		number = r.nextInt(10000)+"";
		System.out.println("保存序号:"+number+"到数据库");
		return number;
	}
}

package service;
//自定义异常:帐号不存在;
public class AccountNotExistException extends Exception{
	
}

package service;
//自定义异常:余额不足;
public class AccountLimitException extends Exception{

}

package service;
//应用异常类;在异常种类非常多的情况下,可以使用应用异常类,通过将事先设计好的错误代码传入异常参数再
//抛出,可以通过异常代码来识别是哪种异常;错误代码以及描述可以通过键值対的形式保存在数据库或者内存中,方便查询;

public class ApplicationException extends Exception{
	private String errorCode;
	public ApplicationException(String errorCode){
		this.errorCode = errorCode;
	}
	getter/setter...
	
}


视图:

apply_form.jsp;

<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>
<html>
	<head></head>
	<body style="font-size:30px;font-style:italic;">
		<span style="color:red;">${apply_error}</span>
		<form action="apply.do" method="post">
			<fieldset>
				<legend>申请贷款</legend>
				帐号:<input name = "accountNo"/><br/>
				金额:<input name = "amount"/><br/>
				<input type="submit" value="确定"/>
			</fieldset>
	</body>
</html>

view.jsp;

<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>
<html>
	<head></head>
	<body style="font-size:30px;font-style:italic;">
		申请贷款成功,请记住序号${number},去柜台处理;
	</body>
</html>


错误页面:

在webroot下创建一个error.jsp;

<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>
<html>
	<head></head>
	<body style="font-size:30px;font-style:italic;">
		系统繁忙,稍后重试;
	</body>
</html>

在webroot下创建一个error2.jsp;

<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>
<html>
	<head></head>
	<body style="font-size:30px;font-style:italic;">
		访问的页面不存在;
	</body>
</html>


在web.xml中配置;

<error-page>
	<exception-type><javax.servlet.ServletException/exception-type>
	<location>/error.jsp</location>
</error-page>
<error-page>
	<error-code>404</error-code>
	<location>/error2.jsp</location>
</error-page>

index.jsp;

修改webroot下的index.jsp;

<%@page pageEncoding="utf-8" contentType="text/html;charset=utf-8" %>
<html>
	<head></head>
	<body style="font-size:30px;font-style:italic;">
		<a href="to_apply.do">申请贷款</a>
	</body>
</html>



控制器;

ActionServlet.java;

	public class ActionServlet extends HttpServlet{
		public void service(HttpServletRequest request,HttpServletResponse response)
		throws ServletException,IOException{
			request.setCharacterEncodeing("utf-8");
			String uri = request.getRequestURI();
			String action = uri.substring(uri.lastIndexOf("/"),uri.lastIndexOf("."));
			//分析请求资源路径,依据请求调用不同的模型来处理;
			if(action.equals("/to_apply")){
				//转发到apply_form.jsp;此时是服务器内部通知容器去调用此jsp,不是浏览器直接向服务器发请求,所以可以访问;
				request.getRequestDispatcher("/WEB-INF/JSP/apply_form.jsp");
			}
			if(action.equals("/apply")){
				String accountNo = request.getParameter("accountNo");
				Double amount = Double.parseDouble(request.getParmeter("amount"));
				AccountService service = new AccountService();
			try{
				String number = service.apply(accountNo.amount);
				//申请成功;
				//根据模型返回的结果,选择合适的视图来展现这些处理结果;
				request.setAttribute("number",number);
				request.getRequestDispatcher("/WEB-INF/JSP/view.jsp").forward(request,response);
			}catch(SQLException e){
				e.printStackTrace();
				throw new ServletException(e);
			}catch(AccountNotExistException e){
				e.printStackTrace();
				request.setAttribute("apply_error","帐号不存在");
				requset.getRequestDispatcher("/WEB-INF/JSP/apply_form.jsp").forward(request,response);
			}catch(AccountLimitException e){
				e.printStackTrace();
				request.setAttribute("apply_form","余额不足");
				requset.getRequestDispatcher("/WEB-INF/JSP/apply_form.jsp").forward(request,response);
			}
			
			}
			
		}
	}


将视图中的.jsp文件由WebRoot中移植WEB_INF中的新建文件夹JSP中,在WEB_INF下的文件浏览器不能直接访问,
如果访问会报404错误,所以最好对404错误页面来进行修改,详见error2.jsp的配置;


mvc的优缺点;

(1)优点;
a,模型的复用;
b,方便测试;比如,将业务处理逻辑直接写在servlet里面,需要部署之后才能测试,而将
业务逻辑放在java类中可以直接做单元测试;
c,方便代码的维护;模型和视图的修改不会互相影响;


(2)缺点;

使用mvc,会增加设计的难度,也会增加代码量以及开发成本;




*/



