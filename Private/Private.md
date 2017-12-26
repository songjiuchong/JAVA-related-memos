Private

package prototype;

public class SongPrivate {
	private int gold;
	public void setGold(int gold){
		
		if(gold>=0&&gold<=200000){
			this.gold = gold;
			System.out.println(this.gold);
		}else{
			System.out.println("error");
		}
	}
	public int getGold(){
			return gold;
		}
    public static void main(String []args){
	    SongPrivate gold1 = new SongPrivate();
	    gold1.setGold(190000);
	    
	}
}