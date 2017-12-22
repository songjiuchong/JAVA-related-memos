ArrayAndClass


package prototype;

import java.util.Arrays;

public class SongArrayAndClass {
	String name;
	String date;
	SongArrayAndClass(String name,String date){
		this.name = name;
		this.date = date;
	}
	public void movieArray(int l){
		String []open= new String [l];   //动态声明数组;
		open[0]= this.name;
		open[1]= this.date;
		
		int []open2=null;                //这种声明方法属于分布动态声明;
		open2 = new int[l];
		open2[0]=1;
		open2[1]=2;
		
		int []open1={1,2,3};             //第二种声明数组的方法，属于静态声明数组;
	}
	

	
	//数组类(java.util.Arrays)的常用静态方法：
	//1.toString(ary);显示数组中的各个元素,返回"[index1,index12,...]"形式的字符串,用于显示数组内容; 需要注意的是如果直接使用ary.toString()所显示的内容是包名.类名@hashcode这样的toString()默认显示;
	//2.equals(ary1,ary2);比较2数组是否相同;返回boolean;
	//3.binarySearch(ary,key);利用二分法查找数组中元素的位置,数组必须是已排序的,否则结果不确定;
	//4.sort(ary);对数组进行排序,从小到大;
	//5.fill(ary,element);使用元素填充数组(会覆盖已声明的元素);
	//6.copyOf();复制数组;
	
	public void iPrint(){
		System.out.println("" + this.name + '\t' + this.date);
		}
	public static void main(String[]args){
		
		//空数组和为null的数组的区别;
		
		//空数组;
		String [] x = new String[0];
		String [] y = { };
		System.out.println(x.length); // 长度为0;
		System.out.println(y.length); // 长度为0;
		
		System.out.println(Arrays.toString(x)); //[];
		System.out.println(Arrays.toString(y)); //[];
		System.out.println(x); //x有自己的内存地址,hashcode;
		System.out.println(y); //y有自己的内存地址,hashcode;
		
		//为null的数组;
		String [] xy = null; //为分配内存地址,不存在hashcode;
		
		System.out.println(Arrays.toString(xy)); //null;
		System.out.println(xy); //null;
		
		//注意:如果一个方法返回一个数组,如果它返回null,则调用方法必须先判断是否返回null,才能对返回数组进一步处理,而如果返回空数组,
		//则无须null引用检查;鉴于此,返回数组的方法在没有结果时我们通常返回空数组,而不是null,这样做对于函数调用者的处理比较方便;
		
		
		
		//数组的赋值：
		
		int[] ary1={4,5,6}; 
		int[] ary2= ary1;   //对内存地址的传递赋值;
		ary1[1]=8;          //两个数组的内容都为{4,8,6};
		
		
		
		//数组的复制一：
		int []ary3=new int[ary1.length];
		for(int i =0;i<ary1.length;i++){
			ary3[i]=ary1[i];
		}
		//数组的复制二：
		int[]ary4=new int[ary1.length];
		System.arraycopy(ary1, 0, ary4, 0, ary1.length); //参数：(被复制的数组,起始位置,目标数组,起始位置,复制长度);
		try{System.arraycopy(ary1, 0, ary4, 0, ary1.length+1);} //这里会报数组越界错误;显然与数组复制三中的copyOf方法不同,这里的方法并无为被复制数组创建副本,而是用类似方复制一中的方法逐一对目标数组元素重新赋值,如果赋值长度与目标数组或是被复制数组不吻合就会报错;
		catch(ArrayIndexOutOfBoundsException e){
			e.printStackTrace();
		}
		//数组的复制三：
		int[]ary5=new int[3];
		ary5=Arrays.copyOf(ary1, 3);  //参数：(被复制数组,复制长度);
		
		//利用copyOf();扩充数组：
		ary5=Arrays.copyOf(ary5,ary5.length+1);//被扩充的数组位置元素为0;注意：2个字符串的链接其实在底层就使用了数组的扩容;
		System.out.println(Arrays.toString(ary2));
		System.out.println(Arrays.toString(ary3));
		System.out.println(Arrays.toString(ary4));
		System.out.println(Arrays.toString(ary5));
		System.out.println("////////////////////////");
		///////////////////////////////////////////////////////
		//test:
		 int [] a=new int[5];
		   a [0]=1;
		   a [1]=2;
		   a [2]=5;
		   a [3]=4;
		   a [4]=3;
		   Arrays.sort(a); //数组的排序方法只是改变参数数组的排序,并没有返回值;
		   System.out.println(Arrays.toString(a));
		//////////////////////////////////////////////////////
		   int [] b=new int[3];
		   b [0]=1;
		   b [1]=2;
		   b [2]=3;
		   
		   int []c=null;
		   
		   a=Arrays.copyOf(a, 4);
		   System.out.println(Arrays.toString(a));//输出后a数组变为4个元素,说明copyOf()方法是建立了一个被赋值数组的副本,将它赋给等式左边的数组变量,也就是说其实等式左边数组变量重新指向了一块新的堆内存(被复制数组的副本内存地址);
		  
		   a=Arrays.copyOf(a, 6);
		   System.out.println(Arrays.toString(a));//输出后a数组变为6个元素,其容量被扩充了,进一步证明了上式的说法,如果不是指向新的地址而是对左边数组变量元素逐一赋值(如同数组的复制一方法那样)的话从逻辑上说会报数组越界错误;
	      
		   a=Arrays.copyOf(b, 5);
	       System.out.println(Arrays.toString(a));
	       System.out.println(Arrays.toString(b));//copyOf()方法中的参数自身不会改变,就算被复制长度与被复制数组不符,也不会报错,只是在创建其副本时做相应改变(多出的长度将会开辟空间并且初始化为0)后赋给左边数组变量而已;
	       
	       try{a=Arrays.copyOf(c,6);  //报错空指向异常;
	       }                           
	       catch(NullPointerException e){
	    	   e.printStackTrace();
	       }
	       System.out.println(Arrays.toString(a));
	}
}



