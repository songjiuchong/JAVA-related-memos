ClassicSortAlgorithem


package prototype;

import java.util.Arrays;

public class SongClassicSortAlgorithem {
	
//插入排序;
//从第二位开始每次内层循环将其放入中间变量然后和其前面的所有元素比较,所有不满足条件(从小到大或从大到小)的元素将被整体后移,直到找到满足条件的元素(就无需再比较它之前的元素了),将中间变量插入到满足条件和不满足条件的元素中间,这样可以保证每次比较完之后外层下标对应元素之前的所有元素都是按照要求(从小到大或从大到小)排列的;
public static void insertSort(int []ary){   //按从小到大顺序的插入排序：
	int i,j,k;
	for(i=1;i<ary.length;i++){
		k=ary[i];       //取出将要和前一个元素比较的元素放入第三方变量保存，它将会最后被插入到它之前所有比它大的元素的前一个位置;
		for(j=i-1;j>=0&&k<ary[j];j--){//比较被取出数与前一个数，只要满足前一个数还在数组中且被取出数小于前一个元素，则把前一个元素的值赋给后一个，再比较取出数与其前第二个元素...一次类推，也就是把整个数组后移，直到被取出数找到比它小德元素，然后插入其后的一个位置;
			ary[j+1]=ary[j];//使数组后移，为之后被取出数的插入做准备;
		}
		ary[j+1]=k;//插入被取出数;
	}
}
	
///////////////////////////////////////////////////////////////////////////////////////////////////

//冒泡排序;
public static void  bubbleSort(int[]ary){     //从小到大排序的冒泡排序：
	for(int i=0;i<ary.length-1;i++){          //这里的for循环只是控制内层循环重复的次数，为length-1次，原因是：内层循环比较前后两个元素，比较了length-1次后把最大的一个数确定了下来放在了最后，那么内层循环第二次进行时能确定倒数第二大的数并放在数组倒数第二位，以此类推，当循环了length-1次后整个数组的大小排序就确定下来了;
		for(int j=0;j<ary.length-1;j++){      //内层循环，不断比较前后两个元素的大小，并按要求互换它们的值;
			if(ary[j]>ary[j+1]){
				int temp=ary[j];
				ary[j]=ary[j+1];
				ary[j+1]=temp;
			}
		}
	}
}

///////////////////////////////////////////////////////////////////////////////////////////////////

//选择排序;
//遍历一次,记录下最值元素所在位置(先将外层循环下标放入中间变量,每次内层循环将最值下标替换中间变量,再用此下标元素继续比较),遍历结束后,将此最值元素调整到外层循环的位置,这样一次遍历,只需一次交换,便可将最值放置到合适位置;
public static void selectSort(int[]ary)  
{  
	int[] a = ary;
	
	int len = ary.length;
	
	int temp;
	
    int nIndex=0;  
  
    for (int i=0;i<len-1;i++)  
    {  
        nIndex=i;  
        for (int j=i+1;j<len;j++)  
        {  
            if (a[j]<a[nIndex])  
            {  
                nIndex=j;         
            }  
        }  
        //交换  
        if (nIndex!=i)  
        {  
            temp=a[i];  
            a[i]=a[nIndex];  
            a[nIndex]=temp;  
        }  
    }
}  

//注意:
//选择排序存在一定的不稳定性,举个例子:
//序列5 8 5 2 9,我们知道第一遍选择第1个元素5会和2交换,那么原序列中2个5的相对前后顺序就被破坏了,也就是说,虽然两个都是5,但是如果想要保持原来
//两个5的前后顺序(因为逻辑上来讲,它们不需要被重新排序),但是此时它们原来的前后顺序就颠倒了;

///////////////////////////////////////////////////////////////////////////////////////////////////

//快速排序;
//是一个速度非常快的交换排序方式,他的实现思路就是选取一个元素作为分界值,将数组小于分界值的元素排列在左边,大于分界值的元素排列在右边,这里也有一个插入的动作,将分解值插入到分界点(i);然后多次递归后得出完成排序的数组;
public static void quickSort(int[] a, int l,int r){
	if(l < r){
		 //Swap(s[l],s[(l + r)/2]); //有的书上是以中间的数作为基准数的,要实现这个方便非常方便,直接写一个Swap方法,将中间的数和第一个数进行交换就可以了;  
		int i = l, j = r, k = a[i];
		while(i<j){
			while(i<j && a[j]>=k) j--;
            if(i < j)   
            	a[i++] = a[j];
			while(i<j && a[i]<k) i++;
            	a[j--] = a[i];
		}
		a[i] = k;
		quickSort(a,l,i-1);
		quickSort(a,i+1,r);
	}
}

///////////////////////////////////////////////////////////////////////////////////////////////////

//基数排序;
/*

第一步

以LSD为例，假设原来有一串数值如下所示：
73, 22, 93, 43, 55, 14, 28, 65, 39, 81
首先根据个位数的数值，在走访数值时将它们分配至编号0到9的桶子中：
0
1 81
2 22
3 73 93 43
4 14
5 55 65
6
7
8 28
9 39

第二步:

接下来将这些桶子中的数值重新串接起来,成为以下的数列：
81, 22, 73, 93, 43, 14, 55, 65, 28, 39
接着再进行一次分配,这次是根据十位数来分配：
0
1 14
2 22 28
3 39
4 43
5 55
6 65
7 73
8 81
9 93

第三步:

接下来将这些桶子中的数值重新串接起来,成为以下的数列：
14, 22, 28, 39, 43, 55, 65, 73, 81, 93
这时候整个数列已经排序完毕;如果排序的对象有三位数以上,则持续进行以上的动作直至最高位数为止;

LSD的基数排序适用于位数小的数列,如果位数多的话,使用MSD的效率会比较好;MSD的方式与LSD相反,是由高位数为基底开始进行分配,但在分配之后并不马上合并回一个数组中,而是在每个“桶子”中建立“子桶”,将每个桶子中的数值按照下一数位的值分配到“子桶”中;
在进行完最低位数的分配后再合并回单一的数组中;

*/


//////////////////////////////////////////////////////////////////////////////////////


	public static void main(String[] args) {
		int[]a={5,6,4,8,0,9,0};
		
		SongClassicSortAlgorithem.insertSort(a);
		System.out.println(Arrays.toString(a));
		
		SongClassicSortAlgorithem.bubbleSort(a);
		System.out.println(Arrays.toString(a));
		
		SongClassicSortAlgorithem.selectSort(a);
		System.out.println(Arrays.toString(a));
		
		SongClassicSortAlgorithem.quickSort(a,0,a.length-1);
		System.out.println(Arrays.toString(a));
	}
}
