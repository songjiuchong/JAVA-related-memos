Diamond（菱形生成）

package prototype;

public class SongLingXing {
	static int k1=9;//横向总数
	static int i1=9;//纵向总数
	static int c =1;
	public static void main(String[] args) {
		for(int i=1;i<=i1;i++){
				if(i<=(i1+1)/2){
					for(int k=1;k<=((k1-(2*i-1))/2);k++){
						System.out.print(" ");
			    	}
					for(int k=1;k<=(2*i-1);k++){
						System.out.print("*");
					}
					}else{
					for(int k=1;k<=c;k++){
		     			System.out.print(" ");
					}
					for(int k=1;k<=(k1-c*2);k++){
						System.out.print("*");
					}
					c++;
			    }
				System.out.println();
		}
		
	}

  }