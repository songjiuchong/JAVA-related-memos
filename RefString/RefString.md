RefString

package prototype;

public class SongRefString {
	public static void exception1(String str2){
		str2 = "useless";
	}
	public static void exception2(String str1){
		str1 = "useless";
	}
	public static void main(String[] args) {
		String str1 = "unchangeable";
		String str2 = "useless";
		exception1(str1);
		System.out.println(str1);
		exception2(str1);
		System.out.println(str1);  //String类的值无法通过引用传递来改变
		//////////////////////////////////////////////////////////////
		str1=str2;                 //此时，str1对象指向了str2的地址，断开先前的指向，所以str1,str2都指向了“useless”
		System.out.println(str1);
	}
}
