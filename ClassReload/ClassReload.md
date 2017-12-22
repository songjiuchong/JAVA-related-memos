ClassReloadThis

package prototype;

public class SongClassReloadThis {
	public static void main(String[]args){
		SongClassReloadThis s = new SongClassReloadThis(5);
		System.out.println(""+s.a+s.b);
	}
	int a =1;
	int b =2;
	public SongClassReloadThis(){
		
	}
    public SongClassReloadThis(int a,int b){
    	this.a=a;
    	this.b=b;
    }
    public SongClassReloadThis(int a){                //在构造方法中调用另一个构造方法用this();且一定要放在构造里的第一行
    	this(a,a);
    }
}
