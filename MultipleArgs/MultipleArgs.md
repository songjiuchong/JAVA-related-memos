MultipleArgs

package prototype;

public class SongMultiArgs {
		public static void movie(int a ,String ... name){  //参数列表里最多只能有一个可变长度的形参，并且在最后，相当于一个数组;
			for(String temp:name){
				System.out.println(temp);
			}
		}
		 public static void main(String []args){
	        movie(3,"pulp fiction","school of rock","9");
	        movie(3,new String[]{"pulp fiction","school of rock","9"});
	        }
	}
       
