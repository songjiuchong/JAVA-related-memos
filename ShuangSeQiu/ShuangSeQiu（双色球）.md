ShuangSeQiu（双色球）

package testdemo;

import java.util.Arrays;
import java.util.Random;

public class ShuangSeQiu {  //双色球彩票：在1-33个数中随机生成不重复的6个数，按从小到大排列;然后在最后在1-16个数中随机生成一个数;
	public static String[] generate(){
		String[] pool={"01","02","03","04","05","06","07","08","09","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33"};
		boolean [] used=new boolean[pool.length];
		String[] balls=new String[6];
		Random r=new Random();
		int index=0;
		do{ int rn=r.nextInt(pool.length);      //在球池范围内随机生成一个数，以它为下标的球池元素则作为balls数组增加的一个元素;
			if(!used[rn]){                      //利用boolean数组来判断此下标是否已经被使用过;
				used[rn]=true;
			balls[index++]=pool[rn];
			}
		}
		while(index!=balls.length);
		Arrays.sort(balls);                    //按从小打到大的顺序排列数组;
		balls=Arrays.copyOf(balls,balls.length+1); //利用copyOf();来为balls数组扩充一位;
		balls[balls.length-1]=pool[r.nextInt(16)]; //在球池的前1-16位中随机生成一个数作为下标，其元素作为balls数组的最后一个元素;
		return balls;
	}
	
	
	public static void main(String[] args) {
		System.out.println(Arrays.toString(ShuangSeQiu.generate()));
	}
}
