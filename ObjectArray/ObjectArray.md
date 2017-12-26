ObjectArray

package prototype;

class P{
	private String name;
	private int lvl;
	public P(String name,int lvl){
		this.name=name;
		this.lvl=lvl;
	}
	public String getName(){
		return this.name;
	}
	public int getLvl(){
		return this.lvl;
	}
}


public class SongObjectArray {
	public static void main(String[] args) {
		P p1[] = new P [3];                        //动态创建对象数组;
		for(int x=0;x<p1.length;x++){
			System.out.println(p1[x]);             //此时输出3个null，因为声明后只是开辟了空间，默认值为空;
		}
	    p1[0]=new P("玩家1",100);                  //数组中每一个元素都是一个对象，使用对象时必须先实例化对象;
	    p1[1]=new P("玩家2",101);
	    p1[2]=new P("玩家3",102);
	    for(int x =0;x<p1.length;x++){
	    	System.out.println(p1[x].getName()+'\t'+p1[x].getLvl());
	    }	
	    
	    P p2[]={new P("玩家4",100),new P("玩家5",101),new P("玩家6",102)};    //静态创建对象数组;
	   	for(int x =0;x<p2.length;x++){
		    	System.out.println(p2[x].getName()+'\t'+p2[x].getLvl());
	    }
	}
}
