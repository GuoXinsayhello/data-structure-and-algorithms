# data-structure-and-algorithms
《算法导论》与《算法》
==
#第一部分 基础知识
##第一章
如何任何一个NP完全问题存在有效算法，那么所有NP完全问题都存在有效算法
“旅行商问题”是NP完全的，归并排序的算法时间正比于nlogn，而插入排序正比于n*n
##第二章
分治算法
一个简单的例子是两堆已经各自排好序的扑克牌,分别比较两堆顶部的牌,把较小的放进第三个最终数组,被抽走的那堆显露新的牌再进行比较
##第三章
函数的增长
###3.1
若f(n)=Θ(g(n)),则称g(n)是f(n)的一个渐近紧确界,如果当n>n0时,f(n)小于等于cg(n),则记f(n)=O(g(n)),如果f(n)总大于等于cg(n),则记f(n)=Ω(g(n)).
用o记号表示一个非渐近紧确的上界，o(g(n))记为f(n)<cg(n),注意与大O符号的区别。
用ω记号表示一个非渐近紧确的下界，ω与Ω的关系与o与O记号的关系类似<br>
###3.2
向下取整表示小于或等于x的最大整数，向上取整表示大于或者等于x的最小整数
斯特林公式给出了n的阶乘的近似表达式
斐波那契数与黄金分割率fai以及其共轭数有关
##第四章
分治策略
###4.2矩阵乘法的Strassen算法
可以通过这个算法把矩阵乘法由基本的Ω(n^3)变为O（n^lg7),但是63页指出,从实用的角度,Strassen算法通常不是解决矩阵乘法的最优选择.<br>
###4.3用代入算法求解递归式
分治策略就是把一个大问题分解成很多相似的小问题,在得出递归式后可以尝试猜测出递归式的时间复杂度的具体表现形式,,然后代入得出上界或是下界<br>
###4.4
用递归树方法求解递归式的表达式,递归式所有结点的代价之和即为总的算法复杂度<br>
###4.5
用主方法求解递归式有固定的方法
##第五章 概率分析和随机算法
#第二部分 排序和顺序统计量
##选择排序
就是就是找到数组中最小的元素与第一个元素交换位置,然后从剩下元素中找到最小的,然后和第二个元素交换位置,依次类推
```java
private static int[] selectionSort(int[] array){  
        //每次循环找出相对最小值，并交换到头部  
        for(int i=0;i<array.length-1;i++){  
            int lowIndex = i;  
            for(int j=i;j<array.length;j++){  
                if(array[j]<array[lowIndex])  
                    lowIndex = j;  
                loopcount++;  
            }  
            swap(array,lowIndex,i);  
            swapcount++;  
        }  
        return array;  
    }  
    private static void swap(int[] array,int i, int j){  
        int temp = array[i];  
        array[i] = array[j];  
        array[j] = temp;  
    }  
```
##插入排序
就是把一个新的元素不断插入到已经排好序的元素集合中,插入排序对于部分有序的数组十分高效
```java
    private static int[] insertSort(int[] array){  
        //与前边排序好的子序列比较，向前依次比较相邻元素，将元素推到正确位置  
        for(int i=0;i<array.length;i++){  
            for(int j=i;j>0 && array[j]<array[j-1];j--){  
                swap(array,j,j-1);  
                loopcount++;  
                swapcount++;  
            }  
        }  
        return array;  
    }  
  ```
  程序来自http://blog.csdn.net/protommy/article/details/5124490
##希尔排序
希尔排序的思想就是使数组中任意间隔为h的元素都是有序的,h的取法不固定,一般使用h为1,4,13,40…(h=3*h+1),从比较大的h值开始排一遍,然后h递减再排序,一直到h为1
```java
public static void shellSort(int[] data) {  
        // 计算出最大的h值  
        int h = 1;  
        while (h <= data.length / 3) {  
            h = h * 3 + 1;  
        }  
        while (h > 0) {  
            for (int i = h; i < data.length; i += h) {  
                if (data[i] < data[i - h]) {  
                    int tmp = data[i];  
                    int j = i - h;  
                    while (j >= 0 && data[j] > tmp) {  
                        data[j + h] = data[j];  
                        j -= h;  
                    }  
                    data[j + h] = tmp;  
                    print(data);  
                }  
            }  
            // 计算出下一个h值  
            h = (h - 1) / 3;  
        }  
    }  
```
程序来自http://blog.csdn.net/apei830/article/details/6591509<br>
自顶向下的归并排序就是对于一个大数组不断递归调用自身,把大数组不断分小,算法所需要的时间和NlgN成正比.
自底向上的归并排序就是首先把大数组两两排序,然后四个数排序,然后八个数,直到全部
```java
public class MergeSortTest {

	public static void main(String[] args) {
		int[] data = new int[] { 5, 3, 6, 2, 1, 9, 4, 8, 7 };
		print(data);
		mergeSort(data);
		System.out.println("排序后的数组：");
		print(data);
	}

	public static void mergeSort(int[] data) {
		sort(data, 0, data.length - 1);
	}

	public static void sort(int[] data, int left, int right) {
		if (left >= right)
			return;
		// 找出中间索引
		int center = (left + right) / 2;
		// 对左边数组进行递归
		sort(data, left, center);
		// 对右边数组进行递归
		sort(data, center + 1, right);
		// 合并
		merge(data, left, center, right);
		print(data);
	}

	/**
	 * 将两个数组进行归并，归并前面2个数组已有序，归并后依然有序
	 * 
	 * @param data
	 *            数组对象
	 * @param left
	 *            左数组的第一个元素的索引
	 * @param center
	 *            左数组的最后一个元素的索引，center+1是右数组第一个元素的索引
	 * @param right
	 *            右数组最后一个元素的索引
	 */
	public static void merge(int[] data, int left, int center, int right) {
		// 临时数组
		int[] tmpArr = new int[data.length];
		// 右数组第一个元素索引
		int mid = center + 1;
		// third 记录临时数组的索引
		int third = left;
		// 缓存左数组第一个元素的索引
		int tmp = left;
		while (left <= center && mid <= right) {
			// 从两个数组中取出最小的放入临时数组
			if (data[left] <= data[mid]) {
				tmpArr[third++] = data[left++];
			} else {
				tmpArr[third++] = data[mid++];
			}
		}
		// 剩余部分依次放入临时数组（实际上两个while只会执行其中一个）
		while (mid <= right) {
			tmpArr[third++] = data[mid++];
		}
		while (left <= center) {
			tmpArr[third++] = data[left++];
		}
		// 将临时数组中的内容拷贝回原数组中
		// （原left-right范围的内容被复制回原数组）
		while (tmp <= right) {
			data[tmp] = tmpArr[tmp++];
		}
	}

	public static void print(int[] data) {
		for (int i = 0; i < data.length; i++) {
			System.out.print(data[i] + "\t");
		}
		System.out.println();
	}

}
```
各种排序算法的优缺点参见右图![](https://github.com/GuoXinsayhello/data-structure-and-algorithms/tree/master/picture/pro.jpg)
##第六章 堆排序
（二叉）堆可以被近似看成一个完全二叉树，除了最底层外，该树是完全充满的
时间复杂度O(nlgn),最大堆与最小堆的概念,构造一个最大堆,以及对堆进行排序,得到一个有序数组
由sedgewick的《算法》可以知道可以用一个数组来表示堆，最顶点放在第一个位置,并且a[0]不放元素，从a[1]开始放元素，这样可以满足k的父结点为a[k/2]。具体的实现是先构一个堆，满足堆的性质，就是父结点要大于两个子结点，遍历有子结点的父结点，如果不满足堆的性质就与子结点交换，一直到构造出一个有序的堆（也就是父结点大于两个子结点），但此时并不是一个有序的数列，将顶点与尾结点交换，得到最大值，然后是次大值，依次类推，最终得到一个有序的序列。
```java
public class HeapSort {

	public static void main(String[] args) {
		int[] arr = { 50, 10, 90, 30, 70, 40, 80, 60, 20 };
		System.out.println("排序之前：");
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + " ");
		}

		// 堆排序
		heapSort(arr);

		System.out.println();
		System.out.println("排序之后：");
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + " ");
		}
	}

	/**
	 * 堆排序
	 */
	private static void heapSort(int[] arr) { 
		// 将待排序的序列构建成一个大顶堆
		for (int i = arr.length / 2; i >= 0; i--){ 
			heapAdjust(arr, i, arr.length); 
		}
		
		// 逐步将每个最大值的根节点与末尾元素交换，并且再调整二叉树，使其成为大顶堆
		for (int i = arr.length - 1; i > 0; i--) { 
			swap(arr, 0, i); // 将堆顶记录和当前未经排序子序列的最后一个记录交换
			heapAdjust(arr, 0, i); // 交换之后，需要重新检查堆是否符合大顶堆，不符合则要调整
		}
	}

	/**
	 * 构建堆的过程
	 * @param arr 需要排序的数组
	 * @param i 需要构建堆的根节点的序号
	 * @param n 数组的长度
	 */
	private static void heapAdjust(int[] arr, int i, int n) {
		int child;
		int father; 
		for (father = arr[i]; leftChild(i) < n; i = child) {
			child = leftChild(i);
			
			// 如果左子树小于右子树，则需要比较右子树和父节点
			if (child != n - 1 && arr[child] < arr[child + 1]) {
				child++; // 序号增1，指向右子树
			}
			
			// 如果父节点小于孩子结点，则需要交换
			if (father < arr[child]) {
				arr[i] = arr[child];
			} else {
				break; // 大顶堆结构未被破坏，不需要调整
			}
		}
		arr[i] = father;
	}

	// 获取到左孩子结点
	private static int leftChild(int i) {
		return 2 * i + 1;
	}
	
	// 交换元素位置
	private static void swap(int[] arr, int index1, int index2) {
		int tmp = arr[index1];
		arr[index1] = arr[index2];
		arr[index2] = tmp;
	}
}
```

转自http://blog.csdn.net/zdp072/article/details/44227317
##第七章 快速排序
在排序算法中，如果输入数组中仅有常数个元素需要在排序过程中存储在数组之外，则称排序算法是
原址的
快速排序的期望时间复杂度是Θ（nlgn），最坏情况是Θ（n^2)
快速排序由C. A. R. Hoare在1962年提出。它的基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。
只要分割的两部分比例是常数，算法的运行时间总是O（nlgn）
可以用RANDOMIZED-QUICKSORT首先随机选择主元可以得到期望运行时间
```java
 public static void quickSort(int a[], int left, int right) {
        int i, j, temp;
        i = left;
        j = right;
        if (left > right)
            return;
        temp = a[left];
        while (i != j)/* 找到最终位置 */
        {
            while (a[j] >= temp && j > i)
                j--;
            if (j > i)
                a[i++] = a[j];//1号位置代码
            while (a[i] <= temp && j > i)
                i++;
            if (j > i)
                a[j--] = a[i];//2号位置代码

        }
        a[i] = temp;
        quickSort(a, left, i - 1);/* 递归左边 */
        quickSort(a, i + 1, right);/* 递归右边 */
    } 
```
`注意在快速排序中，1号位置代码和2号位置代码位置不能发生互换`<br>
程序来自http://www.cnblogs.com/hubcarl/archive/2011/04/07/2007823.html<br>
`三向切分快速排序`
```java
public class ThreeWayQuickSort {  
  
    public static void sort(int[] a) {  
        sort(a, 0, a.length - 1);  
    }  
  
    //在lt之前的(lo~lt-1)都小于中间值  
    //在gt之前的(gt+1~hi)都大于中间值  
    //在lt~i-1的都等于中间值  
    //在i~gt的都还不确定（最终i会大于gt，即不确定的将不复存在）  
    private static void sort(int[] a, int lo, int hi) {  
        if (lo >= hi) {  
            return;  
        }  
        int v = a[lo], lt = lo, i = lo + 1, gt = hi;  
        while (i <= gt) {  
            if (a[i] < v) {  
                swap(a, i++, lt++);  
            } else if (a[i] > v) {  
                swap(a, i, gt--);  
            } else {  
                i++;  
            }  
        }  
        sort(a, lo, lt - 1);  
        sort(a, gt + 1, hi);  
    }  
      
    private static void swap(int[] a, int i, int j) {  
        int t = a[i];  
        a[i] = a[j];  
        a[j] = t;  
    }  
} 
```
冒泡排序<br>
```java
 public static void bubbleSort(int[] data) {  
        for (int i = 0; i < data.length - 1; i++) {// 控制趟数  
            for (int j = 0; j < data.length - i - 1; j++) {  
                if (data[j] > data[j + 1]) {  
                    int tmp = data[j];  
                    data[j] = data[j + 1];  
                    data[j + 1] = tmp;  
                }  
            }  
        }  
    } 
```
冒泡排序就是依次比较相邻的数字，如果前一个数要大于后一个数就交换两者的位置，对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数<br>

选择排序<br>
```java
    public static void selectSort(int[] data) {  
        if (data == null || data.length == 0) {  
            return;  
        }  
        for (int i = 0; i < data.length - 1; i++) {  
            int min = i;// 将当前下标定为最小值下标  
            for (int j = i + 1; j < data.length; j++) {  
                if (data[j] < data[min]) {  
                    min = j;  
                }  
            }  
            if (i != min) {  
                int tmp = data[i];  
                data[i] = data[min];  
                data[min] = tmp;  
            }  
        }  
    }  
```
归并排序<br>
```java
 public static void mergeSort(int[] data) {  
        sort(data, 0, data.length - 1);  
    }  
    public static void sort(int[] data, int left, int right) {  
        if (left >= right)  
            return;  
        // 找出中间索引  
        int center = (left + right) / 2;  
        // 对左边数组进行递归  
        sort(data, left, center);  
        // 对右边数组进行递归  
        sort(data, center + 1, right);  
        // 合并  
        merge(data, left, center, right);  
        print(data);  
    }  
  
    /** 
     * 将两个数组进行归并，归并前面2个数组已有序，归并后依然有序 
     *  
     * @param data 
     *            数组对象 
     * @param left 
     *            左数组的第一个元素的索引 
     * @param center 
     *            左数组的最后一个元素的索引，center+1是右数组第一个元素的索引 
     * @param right 
     *            右数组最后一个元素的索引 
     */  
    public static void merge(int[] data, int left, int center, int right) {  
        // 临时数组  
        int[] tmpArr = new int[data.length];  
        // 右数组第一个元素索引  
        int mid = center + 1;  
        // third 记录临时数组的索引  
        int third = left;  
        // 缓存左数组第一个元素的索引  
        int tmp = left;  
        while (left <= center && mid <= right) {  
            // 从两个数组中取出最小的放入临时数组  
            if (data[left] <= data[mid]) {  
                tmpArr[third++] = data[left++];  
            } else {  
                tmpArr[third++] = data[mid++];  
            }  
        }  
        // 剩余部分依次放入临时数组（实际上两个while只会执行其中一个）  
        while (mid <= right) {  
            tmpArr[third++] = data[mid++];  
        }  
        while (left <= center) {  
            tmpArr[third++] = data[left++];  
        }  
        // 将临时数组中的内容拷贝回原数组中  
        // （原left-right范围的内容被复制回原数组）  
        while (tmp <= right) {  
            data[tmp] = tmpArr[tmp++];  
        }  
    } 
```
##第八章 线性时间排序
在最坏情况下，任何`比较`排序算法都要做Ω（nlgn）次比较<br>

###8.2计数排序
计数排序就是有多少个元素小于它就把它放在第几个位置，计数排序的重要性质就是稳定性，具有相同值的元素在输出中相对次序与输入中的相对次序相同
###8.3基数排序  
基数排序是一种稳定性排序，（稳定性排序就是保留数组中重复元素的相对位置，比如数据已经按照时间顺序排好序，现在要按照地理位置排序，如果排序算法是稳定的，那么处在同一个地点的数据彼此之间还是按照时间先后排序的，如果排序算法不是稳定的，就可能出现同一地点的数据时间先后可能会混乱。插入排序和归并排序是稳定的，选择，希尔，快排，堆排不是稳定的），是从低位到高位进行排序
###8.4桶排序
桶排序 (Bucket sort)或所谓的箱排序，是一个排序算法，工作的原理是将数组分到有限数量的桶子里。每个桶子再个别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排序），其期望运行时间为Θ（n）
9.1同时得到最大值和最小值可以先把输入的一对数据进行比较，然后较小的数据与暂时的最小值比较，较大的数据与暂时的最大值比较
9.2randomized-select方法寻找一个序列中第i小的元素，类似于快速排序，先把数组分为小于以及大于主元的两部分，然后在这两部分中的一个寻找所要求的元素。
9.3作者提出了求中位数的中位数的算法
##第三部分 数据结构
###第十章  基本数据结构
10.1队列是环序的
10.2在链表中可以设置一个哨兵，在代码中可以降低常数因子，使代码简洁
对于没有指针的语言可以用数组来构成链表，每一个对象的关键字，prev以及next都存放在数组中的一个位置上
###第十一章  散列表
将整数散列的最好方法是除留余数法，即k%M，其中M最好是一个素数<br>
对于字符串可以用str.charAt(i)将字符串返回一个char值，即返回一个16位的整数。<br>
如果遇到冲突的话，可以把具有相同散列值的字符用一个链表连接起来，这在《算法》中称为拉链法；还有一种方法叫线性探测法，碰撞发生时检查散列表的下一个位置，从基于线性探测法的散列表中删除一个键不能把该键直接置为null，这样会使得之后的元素无法被找到，因此还需要把被删除键右侧的所有键重新插入散列表。<br>
Java的java.util.TreeMap与java.util.HashMap分别是基于红黑树和拉链法的散列表的符号表实现，HashSet中不会包含重复的元素
###第十二章  二叉搜索树
二叉搜索树的高度为k-1，也就是根结点是第0层
Jensen不等式  将一个凸函数f(x)应用到随机变量X上时，假定期望存在且有限，则由詹森不等式可得E[f(x)]≥f(E[X])
####二叉查找树的delete方法实现
基本思想用的是递归的方法，删除最小结点代码如下：
```java
private Node deleteMin(Node x)
{
 if(x.left==null)
 return x.right;
 x.left=deleteMin(x.left);  //递归调用
 x.N=size(x.left)+size(x.right)+1;//更新计数器
 return x;
}
```
###第十三章 红黑树
####13.1每个结点红色或者黑色的，根节点是黑色的，对于每个结点从该节点到所有后代叶节点(NIL)的简单路径均包含相同数目的黑色结点。
红黑树:平衡二叉树，广泛用在C++的STL中。map和set都是用红黑树实现的，
统计性能比AVL树更高，AVL树是一种高度平衡的二叉搜索树，对于每个结点x，x的左右子树的高度差至多为1。红黑树被Java的TreeMap所应用。
####13.3插入
把一个结点插入红黑树或者从红黑树上删除时，可能会违背红黑树的性质，所以要对红黑树重新染色以及旋转操作来满足红黑树的性质<br>
红黑树的旋转操作用java实现还是挺简单的
具体操作参见![](https://github.com/GuoXinsayhello/data-structure-and-algorithms/tree/master/picture/webwxgetmsgimg.jpg)
###第十四章 数据结构的扩张
顺序统计树是在红黑树上添加存储信息，比如结点以下结点的数目总和。本章举例了扩张红黑树产生区间树
##第四部分  高级设计和分析技术
###第15章 动态规划
动态规划是付出额外的内存空间来节省计算时间,是典型的时空权衡的例子,子问题图G=(V,E)可以确定动态规划算法的运行时间.
动态规划就是对一个大问题的子问题进行求解,然后保存该子问题的结果,然后把问题逐渐扩大,扩大后的问题会用到之前小问题求解后的结果,这样就不用再去求其中的小问题,直接拿来结果用就可以<br>
####15.2 矩阵链乘法问题
多个矩阵相乘,乘的顺序会影响计算复杂度
卡特兰数又称卡塔兰数，英文名Catalan number，是
	组合数学中一个常出现在各种计数问题中出现的
	数列。以
	比利时的数学家欧仁·查理·卡塔兰 (1814–1894)的名字来命名，其前几项为 : 1, 1, 2, 5, 14, 42, 132, 429, 1430, 4862, 16796, 58786, 208012, 742900, 2674440, 9694845, 35357670, 129644790, 477638700, 1767263190, 6564120420, 24466267020, 91482563640, 343059613650, 1289904147324, 4861946401452, …
矩阵代数的中的定义，nontrivial=nonzero,AX=0, 如果行列式|A|=0，那么A不可逆， 则X有非平凡解；否则，A可逆，那么只有解X=0，即是平凡解。
适合用动态规划方法求解问题的两个要素:最优子结构(意思就是一个问题的最优解包含其子问题的最优解）以及子问题重叠。无权最长简单路径问题是NP完全的。简单路径：顶点序列中顶点不重复出现的路径
一个问题适合用动态规划去做要求子问题有无关性和重叠性，无关性即子问题不共享资源，比如求最短路径问题子问题之间没有重复的点，重叠性即问题的递归算法会反复求解相同的子问题。<br>
####15.4最长公共子序列问题（LCS问题，longest-common-subsequence problem）
给定两个序列X，Y，如果Z既是X的子序列也是Y的子序列，则称它是X和Y的公共子序列<br>
####15.5最优二叉搜索树
搜索代价最小的二叉搜索树称为最优二叉搜索树,构造最优二叉搜索树也可以采用动态规划的方法,一棵最优二叉搜索树的子树也是最优的
对于旅行商问题,如果把问题简化为双调巡游(bitonic tours),则存在多项式时间算法
###第16章贪心算法
如果一个问题的最优解包含其子问题的最优解，则称此问题具有最优子结构性质，此性质是能否应用动态规划和贪心算法的关键要素，另外一个关键要素是贪心选择性质。贪心算法可以解决分数背包问题，但是不能解决0-1背包问题。
前缀码（prefix code），没有任何码字是其他码字的前缀，这样做的好处是编码文件的开始码字是无歧义的
用拟阵可以求解任务调度问题,每个任务都有一个截止时间,超过截止时间会有一个惩罚值,可以想到一个server,有多个user请求观看视频,每个用户的容忍时间都不是一样的,也就是截止时间,利用这个可以实现任务的调度
###第17章摊还分析
这个分析的目的就是求数据结构中执行所有操作的平均时间，来评价操作的代价
聚合分析中每个操作赋予相同的代价，就是把n个操作整体进行考虑，
核算法不同操作可能会有不同代价，就是用一部分去支付花费，另外一部分·作为信用用于弥补后面支付不足的情况。
##第五部分  高级数据结构
###第18章 B树
B树的节点可以有多个孩子，一个结点包含x.n个关键字，则该结点有x.n+1个孩子，B树的最小度数t限定了结点包含关键字个数的上界和下界，每个内部结点最多2t-1个关键字，最少t-1个关键字,一棵高度为h的B树,最小度数为t,共有n个关键字,则h<=log(t)(n+1)/2(注释:该式表示以t为底)。由于B树中一个结点有多个孩子，所以在搜索时比普通的二叉树少很多搜索结点.向B树中插入一个新的关键字,当遇到满结点时可能会用到分裂操作.
当从B树中删除一个关键字时为了满足B树的条件,结点的关键字要不少于t-1,所以在删除的时候会分为很多不同的情况讨论.我就纳闷了,为了满足B树的条件,删除的时候太过于繁琐了,是否有一种削足适履的感觉.是否为为了满足B树的定义而增加了算法的复杂度
###第19章 斐波那契堆
理论上斐波那契堆部分操作的摊还时间要比二叉堆要小很多,但是实际当中编程复杂性相比二叉堆不是很适用.斐波那契堆是一系列具有最小堆序的有根树的集合,每棵树均遵循最小堆性质
斐波那契堆extract-min操作在结束之后保证root list的根结点的度数都不一样(度数就是根结点的子孩子数目)
设x是斐波那契堆中的任意结点，设k=x.degree，则有size（x）>=F(k+2)>=fai^k,其中fai=（1+根号5）/2,F(k+2)为第k+2个斐波那契数，size（x)为以x为根的子树中包括x本身在内的结点个数
###第20章 van Emde Boas树
感觉第316页的图好像画错了，在min以及max中应该放置最小或者最大的元素，但是该图好像并不是这样。
vEB树支持在动态集合上运行时间为O（lglgn）的操作，就是把元素分成很多簇，并且在每一簇中都存储了min以及max，这样在查找某个元素的时候就会比较方便一点。
###第21章 用于不相交集合的数据结构
Find-set操作通过指向父结点的指针找到树的根
##第六部分 图算法
###第22章 基本的图算法
《算法》中说可以用邻接表数组来表示一个图，如果表示有连接，就在两个顶点的邻接表中添加对方
稀疏图(边的条数|E|远远小于|V|^2的图
BFS（广度优先搜索）的总运行时间是O（V+E），该算法的思想就是需要在发现所有距离源结点s为k的所有结点之后，才会发现距离源结点s为k+1的其他结点。具体可以使用一个FIFO的队列来保存所有已经被标记过但是邻接表还未被检查过的顶点。
```java
private void bfs(Grapg G,int s)
{
 Queue<Integer> queue=new Queue<Integer>();
 marked[s]=true;
 queue.enqueue(s);
 while(!queue.isEmpty())
 {
  int v=queue.dequeue();
  for(int w:G.adj(v))
   if(!marked[w])
   {
    edgeTo[w]=v;
    marked[w]=true;
    queue.enqueue(w);
   }
 }
}
```
DFS（深度优先搜索）,可以用来寻找结点的发现时间以及完成时间，会形成多个独立的子树，边分为后向边、前向边、树边以及横向边，总运行时间是西塔（V+E）
```java
private void dfs(Graph G,int v)
{
 maked[v]=true;
 count++;
 for(int w:G.adj(v)
  if(!marked)
   dfs(G,w);
}
```
后代区间的嵌套：在有向图或者无向图G的深度优先森林中，结点v是结点u的真后代当且仅当u.d<v.d<v.f<u.f成立，d为发现时间（discover），f为完成时间（finish）
拓扑排序：利用DFS求出每个结点的finish time，结点次序与结点的完成时间恰好相反，可以在西塔（V+E）时间内完成拓扑排序
强联通分量是一个最大结点集合，对于该集合中的任意一对结点可以相互到达，G中连接不同强连通分量的每条边都是从完成时间较晚的分量指向完成时间较早的分量，而转置图反之。强连通分量算法就是对G进行一次DFS，然后再对G的转置进行一次DFS
可以通过深度优先搜索算法找到一幅图中的连通分量
![](https://github.com/GuoXinsayhello/data-structure-and-algorithms/tree/master/picture/graph.jpg)
###第23章 最小生成树
最小生成树问题就是把所有结点连接起来并且具有最小权重。
解决这两种问题有Kruskal算法以及Prim算法，这两种算法都是贪心算法
Kruskal算法就是在图G（V，E）中首先形成V棵树，然后按照边的权重寻找权重最小的边并考虑其是否加入集合，该算法的总运行时间O（ElgV）
Prim算法类似Dijkstra算法，就是从任意一个根结点开始，不断向其中加入轻量级的边，形成一个集合，选择集合外最小的边加入集合，直到覆盖所有结点，该算法的运行时间也是O（ElgV),如果采用斐波那契堆得话，则Prim算法的运行时间改进为O（E+VlgV）
###第24章 单源最短路径
Bellman-ford算法是求含负权图的单源最短路径算法，就是对每条边进行持续的松弛，效率很低，如果有负权重的环路存在，则输出一个bool值，
最短路径算法依赖最短路径的一个重要性质：两个节点之间的一条最短路径包含着其他的最短路径，Dijkstra算法是一个贪心算法，而Floyd-Warshall算法是一个动态规划算法，单源最短路径不会包含环路。
在求单源最短路径时，对一条边的（u，v）的松弛过程为:从节点s到节点u之间的最短路径距离加上节点u与v之间的边权重，并且与当前的s到v的最短路径估计进行比较，如果前者更小，则对v.d  v.pi进行更新。
Dijkstra算法就是重复从结点集V-S中选择最短路径估计最小的结点u，把u加入到集合S中，然后对所有从u发出的边进行松弛
其中Dijkstra算法和用于有向无环图的最短路径算法对每条边仅松弛一次。Bellman-Ford算法对每条边松弛|v|-1次，且该方法需要的运行总时间是O（VE）
Dijkstra算法的运行时间要低于Bellman-Ford算法的运行时间，该算法解决的是带权重的有向图上单源最短路径问题，要求所有边的权重非负，该算法使用的是贪心策略
单纯形算法并不总能在多项式时间内完成线性规划问题
差分约束系统：线性规划矩阵A的每一行包括一个1和一个-1，其他所有项都为0
可以用Bellman-ford算法来求解差分约束系统
###第25 章 所有结点对的最短路径问题
前驱结点矩阵：从结点I到结点j的最短路径结点j的前驱结点
####25.1 最短路径和矩阵乘法
这个算法的思想就是在寻找两个结点之间的最短路径，从之间的中间结点为0个，1个，一直到n-2个
而改进后的重复平方法可以把运行时间改进为西塔（n^3lgn),其思想和上述的差不多，只不过中间点数不是每次以1递增，而是以2的倍数递增
####25.2 Floyd-Warshall算法
Floyd-Warshall算法，其运行时间为西塔（V^3),具体思想如下：
![](https://github.com/GuoXinsayhello/data-structure-and-algorithms/tree/master/picture/graph2.jpg)
图G的传递闭包为图G*=（V，E*），其中E*={（i，j）：如果图G包含一条从结点i到j的路径}，传递闭包算法的运行时间也是西塔（n^3)
####25.3 用于稀疏图的Johnson算法
该算法可以在O（V^2lgV+VE)时间内找到所有结点对的最短路径，
核心技术就是重新赋予权重，如果所有权重非负，就对每个结点运行一次Dijkstra算法，如果有权重为负的环路，就报告，如果有权重为负，但是不是环路，就重新计算出一组非负权重值，然后再用Dijkstra算法
当路径中出现负值的路径时，可以把每条路径都加上一个数（该数为（-最小的权重值）+1），这样就出现了非负的路径了
###第26章 最大流
横跨任何切割的净流量都相同，都等于|f|,即流的值
最大流最小切割定理：
	1、f是G的一个最大流
	2、残存网络不包括任何增广路径
	3、|f|=c（S,T），其中（S,T）是流网络G的某个切割
求最大流的Ford-Fulkerson算法
就是不断利用残存网络寻找增广路径p，然后不断增加p上的流，这种算法在容量都是整数且最优的流量值较小时，表现比较好，然而当最优的流量值比较大的时候这种算法的运行效果并不是很好
####26.2 Edmonds-Karp算法
该算法是对Ford-Fulkerson算法的改进，是在寻找增广路径中用广度优先搜索来改善算法的效率，其运行时间为O（VE^2)
####26.3图G中一个匹配直接对应G所对应的流网络G‘中的一个流，其中一个最大匹配对应一个最大流
####26.4推送-重贴标签算法
该算法的运行时间为O（V^2E)，而前置重贴标签算法是对推送-重贴标签算法的一种改进，该算法维持一个结点的链表，其运行时间为O（V^3)
##第七部分 算法问题选编
###第27章 多线程算法
贪心调度比较好，多线程算法如果包含竞争就可能产生不正确的结果，可以对矩阵乘法实现多线程的Strassen算法，以及对归并排序实现多线程算法
###第28章 矩阵运算
LUP分解：PA=LU，L是一个单位下三角矩阵，U是一个上三角矩阵，P是一个置换矩阵，满足该式称为矩阵A的LUP分解，设P 是一个 m×n 的 (0,1) 矩阵，如 m≤n且 PP′=E，则称 P为一个 m×n的置换矩阵
如果能对一个矩阵进行LUP分解，用正向替换和反向替换就可以求出Ax=b的解  
A‘-v（w转置）/a11位为矩阵A对于a11的舒尔补，可以把一个矩阵通过这种方式转化成一个上三角矩阵与一个下三角矩阵的乘积
伪逆矩阵  是逆矩阵概念在A不是方阵情况下的自然推广
正规方程
###第29章  线性规划
线性规划 第一个多项式时间算法椭球算法但是运行缓慢
线性规划中标准型所有的约束都是不等式，松弛型中约束都是等式
###第30章 多项式与快速傅里叶变化
FFT可以多项式相乘时间复杂度降低θ(nlgn)
霍纳法则可以提高多项式的计算效率
可以用拉格朗日公式n点进行插值
多项式可以用系数表达也可用点表达
简单地说，递归是重复调用函数自身实现循环。迭代是函数内某段代码实现循环，而迭代与普通循环的区别是：循环代码中参与运算的变量同时是保存结果的变量，当前保存的结果作为下一次循环计算的初始值。
###第31章 数论算法
启发式算法（heuristic algorithm)是相对于最优化算法提出的。一个问题的最优算法求得该问题每个实例的最优解。启发式算法可以这样定义：一个基于直观或经验构造的算法，在可接受的花费（指计算时间和空间）下给出待解决组合优化问题每一个实例的一个可行解，该可行解与最优解的偏离程度一般不能被预计。
给定k个整数输入a1，a2，。。。ak，如果算法可以在关于lg a1，lg a2，。。。lg ak（其实就是把该数用二进制表示的位数）的多项式时间内完成，则称该算法为多项式时间算法
Gcd(a,b)表示a，b的最大公约数
GCD递归定理：任意非负整数a和任意正整数b，gcd（a，b）=gcd（b，a mod b）
欧几里得算法就是根据上述定理得出的求最大公约数的算法
Lame定理证明了欧几里得算法的递归调用次数
###第32章 字符串匹配
![](https://github.com/GuoXinsayhello/data-structure-and-algorithms/tree/master/picture/string.jpg)
假设想寻找字符串A是否包含字符串B
`朴素算法`就是最普通的把字符串B沿着与之比较的字符串A进行滑动，看两者是否相等<br>
`Rabin-Karp算法`就是把字符串B计算出一个数b，再把A中从第i位到i+|B|位计算出一个数ai，I从0一直到|A|-|B|，比较b什么时候和ai相等，Rabin-Karp算法本质上是基于Hash函数的一种启发式算法，该Hash函数实现了从一个字符串到整数之间的映射关系。<br>
`有限自动机`就是首先维持一个状态转移表，然后从字符串A的第一个字符开始读，根据状态转移表得出状态，直到得出的状态匹配B的状态，好处是对于A中的每个字符只需要遍历一次。根据该性质，预处理阶段只需要为所有的可能组合(q,a)都计算出其转移函数值，并用一个表进行保存，然后在匹配阶段，每个输入字符都可以通过查表方式以O(1)计算复杂度完成状态转移<br>
`Knuth-Morris-Pratt算法`
假设当前偏移量s已检验了q个匹配的字符，如果q已经等于m或者检验到第q+1个字符T[s+q]不等于P[q]，那么需要选择下一个偏移量s'继续检验，如果该s'位于[s,s+q)区内，那么s'需要满足条件[s', s+q)区间所形成的子串正好是P的一个前缀。<br>
###第33章 计算几何学
通过计算向量的叉积的符号可以知道两个线段之间是否相交
作者在33.2中通过“扫除线”方法确定一组线段中相交的线段是哪些，就是一条假想的垂直线从左向右进行扫描，看与线段相交的高低次序是否发生变化
凸包（Convex Hull）是一个计算几何（图形学）中的概念。在一个实数向量空间V中，对于给定集合X，所有包含X的凸集的交集S被称为X的凸包。X的凸包可以用X内所有点(X1，...Xn)的线性组合来构造.在二维欧几里得空间中，凸包可想象为一条刚好包著所有点的橡皮圈。用不严谨的话来讲，给定二维平面上的点集，凸包就是将最外层的点连接起来构成的凸多边型，它能包含点集中所有的<br>
寻找凸包的方法有Graham扫描法，运行时间为O（nlgn），另外一种方法为Jarvis步进法，运行时间为O（nh），h为凸包中的顶点数，当然还有其他方法，比如增量法，分治法，剪枝-搜索方法。<br>
二维的最远点对问题：平面上n个点，找出相距最远的两个点。这两个点必然是凸包的顶点
`GRAHAM-SCAN算法`就是首先找到一个极点，（最左最下）的点，然后极射线逆时针旋转，对扫过的点进行编号，然后沿着次序沿每个点走，如果向左转就把该点加入，否则就去除。<br>
Jarvis步进法就是和Graham算法一样找到一个极点，然后找到下一个相对于前一个点的最小极角，直到形成右链，到达最高点，然后开始形成左链
寻找最近点对的问题：可以采用一种分治算法，首先找出一条垂直线，把点集分为左右相等的两部分，然后递归找出左边集合的最近点对以及右边集合的最近点对，然后求出跨区域的最近点对。似乎是不断递归调用。<br>
`欧拉回路`：
若图G中存在这样一条路径，使得它恰通过G中每条边一次,则称该路径为欧拉路径。若该路径是一个圈，则称为欧拉(Euler)回路。<br>
`哈密顿圈`：包含V中每一个顶点的简单回路。<br>
P类问题是在多项式时间内可以解决的问题。NP类问题是指在多项式时间内可以被证明的问题。<br>
顶点覆盖问题是NP完全的，近似算法APPROX-VERTEX-COVER覆盖规模是最优规模的两倍，该算法思想就是从图的边集中任意选出一条边Ei，然后把与Ei的两个端点连接的所有边都从边集中删除，然后再重复上述操作，直到不再剩下边。<br>
旅行商问题的一个近似算法是用prim算法计算出图的最小生成树，然后先序遍历所有顶点，在第一次遇到某个顶点时输出该顶点。这个算法要求满足三角不等式<br>
集合覆盖问题也是NP-hard问题，有一个近似的贪心算法，该算法在每一次选择中都会选择出能覆盖最多尚未被覆盖元素的集合S，一个具体的例子就是有一群人，每个人拥有1个或者多个技能，有一个任务，要求每项技能都至少有一个人<br>
![](https://github.com/GuoXinsayhello/data-structure-and-algorithms/tree/master/picture/tiaohenumber.jpg)
电路可满足性问题就是给定一个由AND，OR以及NOT门组成的布尔组合电路，它是否是可满足电路（即输入一组布尔输入值，输出值为1），这个问题属于NP类，对于这个问题也有个近似算法，就是采用一种随机化的近似算法（就是以概率1/2设置每个变量为1，以1/2的概率设置每个变量为0<br>
子集和问题：对于集合S与正整数t，是否存在S的一个子集，使得其中的数加起来恰好为t，或者加起来最大，但是不能超过t，一个指数时间的准确算法是逐个把一个新的元素加入集合，然后计算由于新的元素的加入而带来的新的和。而一个完全多项式时间的近似模式是首先把集合中相近的元素进行整理（就是相近的元素用一个元素来代替），这样就可以缩小集合的规模，然后不断把元素加进去，计算求和，最终得到的可能不是最优解，而是最优解的一个近似。
##第八部分 《算法》对于《算法导论的补充》
###二分法查找
```java
public static int search(int[] nums, int num) {  
        int low = 0;  
        int high = nums.length - 1;  
  
        while (low <= high) {  
            int mid = (low + high) / 2;  
              
            //与中间值比较确定在左边还是右边区间,以调整区域  
            if (num > nums[mid]) {  
                low = mid + 1;  
            } else if (num < nums[mid]) {  
                high = mid - 1;  
            } else {  
                return mid;  
            }  
        }  
  
        return -1;  
    }  
```
如果搜索不到返回下标的话只需要return （low+high)/2即可
下面这种二分搜索方法不可以，比如{1,3}，搜索3。
```java
private int binsearch(int[] nums,int lo,int hi,int t){
	    	int mid=0;
	    	while(lo<hi)
    		{
    			mid=(lo+hi)/2;
    			if(nums[mid]==t)
    			{
    				return mid;
    			}
    			if(nums[mid]>t)
    				hi=mid;
    			else
    				lo=mid+1;
    		}
	    	return lo;
	    }
```
http://www.jianshu.com/p/7c17cc56e21e 这个网址说了二分法的多种变体

```java
//结果集
public static T ans = new T();
//中间结果集
public static T path = new T();
//问题
public static T problem(){
  ans.clear();//leetcode的一个特殊点，每次要清空结果集，避免重复
  robot(idx ,...);//DFS部分，常用idx作为结果递归的标志
  return ans;
}
//DFS
public static void robot(int idx, ...){
  if(xxx){//边界条件，递归出口条件
    //用当前path内容生成一部分结果集
    //handle path 
    ans.add(tmp);
    return;
  }
  //递归处理
  path[idx] = true;//递归前假设
  robot(++idx, ...);//根据不同情况进行处理
  path[idx] = false;//递归后还原
```
这个是DFS的一般框架
