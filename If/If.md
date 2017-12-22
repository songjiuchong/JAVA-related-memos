If

package prototype;

public class SongIf {
	public static void main(String args[]){
		int a = 21;
		if(a<10){//if括号中只能是逻辑表达式，结果为true 或者false
			System.out.print("a<10");
		}
		else if(a<20){
			System.out.print("10<=a<20");
		}
		else{
			System.out.print("a>=20");
		}
	}

}
