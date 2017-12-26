Shuffle（Map->Set）

package testdemo;
import java.util.*;
public class Shuffle {                   //由于HashSet的限制，用相同的牌组调用beginShuffle只有第一次是乱序输出，之后此顺序固定
	public static void main(String[] args) {
		Cards.addCards();
		BeginShuffle.beginShuffle();
	}
}
class BeginShuffle{
	  public static void beginShuffle(){
		  Set<String> cardbox=Cards.getCard().keySet();
		  for(String s:cardbox){
			  Integer temp=Cards.getCard().get(s);
			  System.out.println("卡牌："+s+"费用"+temp);
		  }
	  }
	
}

class Cards{
	private String name;
	private int cost;
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getCost() {
		return cost;
	}
	public void setCost(int cost) {
		this.cost = cost;
	}
	public static Map<String, Integer> getCard() {
		return cards;
	}
	public void setCard(Map<String, Integer> card) {
		this.cards = card;
	}
	
	private static Map<String,Integer> cards =new HashMap<String,Integer>();
	public static void addCards(){
		cards.put("fire ball", 4);
		cards.put("ice arrow", 2);
		cards.put("blizzerd", 6);
		cards.put("ice barrier", 3);
		cards.put("ice shield", 3);
		cards.put("time stop", 10);
	}
}
