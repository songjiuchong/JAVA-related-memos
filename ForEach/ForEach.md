ForEach

package prototype;

public class SongForEach {
	public static void main(String[]args){
		String []a = new String [3];
		a[0]= "结";
		a[1]= "束";
		a[2]= "了";
		for(String b: a){
			System.out.println(b);
		}
	}

}
