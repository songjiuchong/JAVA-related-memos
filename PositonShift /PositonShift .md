PositonShift

package prototype;

public class SongPositonShift {
	public static void main(String[] args) {
		int a=0x41;
		//00000000 00000000 00000000 0100 0001
		int b=a<<1;
		int c=a>>1;
		System.out.println(Integer.toBinaryString(a));
		System.out.println(Integer.toBinaryString(b));
		System.out.println(Integer.toBinaryString(c));
		int f=0xfffffff0;
		int d=f>>1; //有符号右移，负数高位补1;又称为数学右移，符合数学结果;
		int e=f>>>1;//无符号右移，高位补0;有称为逻辑右移，数学结果无意义;
		System.out.println(Integer.toBinaryString(f));
		System.out.println(Integer.toBinaryString(d));
		System.out.println(Integer.toBinaryString(e));
		
		System.out.println(100<<1);  //是获得2倍乘积的最快方式;
		System.out.println(-100>>1);  //是获得与2整除商的最快方式;(注意是整除，如果是奇数会丢弃余数部分);
		/////////////////////////////////////////////////////////////
		int color=0xB7C9E3;
		//color: 00000000 10110111 11001001 11100011
		int mask=0xff;              //掩码，相当于逻辑与运算：&; 利用1和1与0取余结果都是其本身，而0与1和0取余结果都为0来"过滤掉"所需2进制码以外的内容;
		//mask:  00000000 00000000 00000000 11100011
		int blue= color&mask;
		//blue:  00000000 00000000 00000000 11100011
		int red=(color>>>16)&mask;  //虽然此处用>>或是>>>符号作用相同，但从此运算的意义上来说，仅仅为了移动位数，只需逻辑右移;
		int green=(color>>>8)&mask;
		System.out.println(Integer.toHexString(red));
		System.out.println(Integer.toHexString(green));
		System.out.println(Integer.toHexString(blue));
		
		int mycolor=(red<<16)|(green<<8)|(blue<<0);// |为逻辑加法，用数学加法+在位合并中其实功能一样;
		System.out.println(Integer.toBinaryString(mycolor)); //这里运用取或运算来链接几个分离的2进制码，使其组成新的2进制码;注：取反色: (~color);
		byte bb=(byte)0x80;//这里其实是-128.但是0x80表示十六进制的128，超出了byte的范围，用(byte)限定的话就会取到-128;
		System.out.println(Integer.toBinaryString(bb));//从byte转到int的2进制表示，符号位会被扩展;
		System.out.println(Integer.toBinaryString(bb&0xff));//这里由于用&链接，bb与直接数0xff会先转为int类型在做与运算，结果再转为int类型的2进制数;以此达到去除因符号位扩展而产生的一串1;
		//////////////////////////////////////////////////////////////////////////
		//异或运算：
		int a1=1;        //0001
		int b1=2;        //0010
		int a2=a1^b1;    //0011
		a2 = a2^a1;      //0010
		System.out.println(a2);  //一个数对另一个数异或2次，结果是另一个数不变;
		//利用这一点，可以交换2个变量的值而不需要第三方参数;
		a1=a1^b1;  //a1现在已被赋值为a1和b1的异或结果，它已成为中间变量;
		b1=a1^b1;  //再次对b1异或，得到a1的原始值，并且赋给了b1;交换完成一半;
		a1=a1^b1;  //a1此时为中间变量，而b1现在已经通过交换变为了原来的a1的原始值，所以相当于再次对a1原始值异或，结果为b1原始值，赋给a1，交换结束;
		System.out.println(a1);
		System.out.println(b1);
		//异或运算的另一个功能是可以加密,提高安全性 ;
		
		//补充：
		//当需要将一个数作*2操作时,运用<<1这种方法更加专业，因为它提升了运行速度;但是需要注意的是，当超出范围时，与直接用*2运算一样,编译时不会报错,但是会出现溢出;
		byte aaa= 127; // 补码：01111111
		byte bbb= (byte)(aaa<<1);//在不强转的情况下用符号链接后a<<1被视为int类型,报错;
		System.out.println(bbb); // -2 : 左移后补码：11111110,实际为10000010;
		int ccc = 2147483647; //int 类型最大值;
		ccc=ccc*2;
		System.out.println(ccc);//-2; 对一个整数类型的最大值作乘以2的操作,其结果都是-2;
		//最后需要注意的一点是：在一条包含多个运算连接符号的语句中,程序将执行先乘除后加减最后位移运算的操作;  

	}
}

