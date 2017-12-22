Fibonacci 

package testdemo;

public class Fibonacci {                     //斐波那契数列
	public static long f(int n){
		long n1 = 1;
		long n2 = 1;
		long nx=1;
		if(n<=2){
			return nx;
		}
		for(int i = 3;i<=n;i++){
			nx=n1+n2;
			n1=n2;
			n2=nx;
		}
		return nx;
	}
	public static void main(String[] args) {
		System.out.println(f(100));
	}
}