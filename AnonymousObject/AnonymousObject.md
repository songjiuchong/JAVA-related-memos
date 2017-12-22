AnonymousObject

package prototype;

public class SongAnonymousObject {
	int a =1;
	public void type(){
		System.out.println(a);
	}
	public static void main(String []args){
	new SongAnonymousObject().type();          //匿名对象，只能使用一次的对象;
	}
}