---
layout: post
title: 几种排序算法总结
date: 2016-03-01 11:00:13
category: "Algorithm"
---
  算法学习笔记
---------------------------------
## 1. 选择排序 ##

**思想：**for循环每次从i到N-1中寻找最小元素并将其放在i处，循环结束后排序完成；

<table>
<tr>
<td>稳定性</td>
<td>是否原地排序</td>
<td>时间复杂度</td>
<td>空间复杂度</td>
<td>备注</td>
</tr>
<tr>
<td>否</td>
<td>是</td>
<td>N^2</td>
<td>1</td>
<td></td>
</tr>
</table>


**核心代码：**


{% highlight java %}

    public static void sort(Comparable[] a){
		int N = a.length;
		for(int i=0;i<N;i++){
			int min = i;
			for(int j = i;j<N;j++){
				if(less(a[j], a[min])) min = j;
			}
			each(a, i, min);
		}
	}
{% endhighlight %}



## 2. 插入排序 ##

**思想：**外层循环控制要插入的元素下标i，内层循环将该元素插入到合适的位置；将i与i-1做比较，若a[i]<a[i-1]，则将两者交换位置，反之停止内层循环。i左侧的元素为正序；

<table>
<tr>
<td>稳定性</td>
<td>是否原地排序</td>
<td>时间复杂度</td>
<td>空间复杂度</td>
<td>备注</td>
</tr>
<tr>
<td>是</td>
<td>是</td>
<td>N~N^2</td>
<td>1</td>
<td>取决去输入元素的排列情况</td>
</tr>
</table>

**优点：**对于小规模输入有较好的时间复杂度，可用于归并排序的优化算法；当输入基本正序时，时间复杂度接近N；有稳定性；

**缺点：**依赖输入的排列情况

**核心代码：**

{% highlight java %}

     public static void sort(Comparable[] a){
		int N = a.length;
	//普通版

    //		for(int i=1;i<N;i++){
	//			for(int j=i;j>0&&less(a[j],a[j-1]);j--)
	//				each(a, j, j-1);
	//		}

	//优化版
		for(int i=1;i<N;i++){
			int j;
			Comparable u;
			u = a[i];
			for(j=i;j>0&&less(u,a[j-1]);j--)
				a[j] = a[j-1];
			a[j] = u;
		}	
	}
	
	public static void sort(Comparable[] a,int lo,int hi){
		int N = hi - lo + 1;
		Comparable temp;
		int i,j;
		for(i =lo+1;i<=hi;i++){
			temp = a[i];
			for(j=i;j>lo&&less(temp, a[j-1]);j--){
				a[j] = a[j-1];
			}
			a[j] = temp;
		}
	}
{% endhighlight %}

## 3. 希尔排序 ##

**思想：**寻找合适的递增序列，如h = 3*h +1;外层循环控制h递增序列从最大减小到1；内层循环使用插入排序对相应的h有序数组进行排序；当h=1时，完成排序

<table>
<tr>
<td>稳定性</td>
<td>是否原地排序</td>
<td>时间复杂度</td>
<td>空间复杂度</td>
<td>备注</td>
</tr>
<tr>
<td>否</td>
<td>是</td>
<td>NlogN</td>
<td>1</td>
<td></td>
</tr>
</table>


**优点：**较快的排序速度；对中等大小的数组运行时间可以接受；代码量较小，不需要额外的空间；

**缺点：**性能不如快排；

**核心代码：**

{% highlight java %}

     public static void sort(Comparable[] a){
		int N = a.length;
		int h = 1;
		while(h<N/3) h = 3 * h + 1;
		while(h >= 1){
			for(int i=h;i<N;i++){
				for(int j=i;j>=h&&less(a[j],a[j-h]);j-=h)
					each(a, j, j-h);
			}
			h /=3;
		}
	}
{% endhighlight %}

## 4. 归并排序 ##

**思想：**利用递归调用的思想，将数组一分为二，分别对左右两边调用sort方法排序；再利用归并方法对左右两个字数组进行归并的到有序数组；其核心是归并方法将子数组复制到辅助数组，i，j，分别是左右子数组当前元素的下标，比较i,j元素，将较小的元素复制回原数组当前位置k；

<table>
<tr>
<td>稳定性</td>
<td>是否原地排序</td>
<td>时间复杂度</td>
<td>空间复杂度</td>
<td>备注</td>
</tr>
<tr>
<td>是</td>
<td>否</td>
<td>NlogN</td>
<td>N</td>
<td></td>
</tr>
</table>

**优点：**任意长度为N的数组排序时间和NlogN成正比；

**缺点：**所需的额外空间和N成正比；

**核心代码：**

{% highlight java %}

     public static void merge(Comparable[] a,int lo,int mid,int hi){
		int i = lo;
		int j = mid + 1;
		for(int k = lo;k <= hi;k++){
			aux[k] = a[k];
		}
		for(int k = lo;k <= hi;k++){
			if(i>mid) a[k] = aux[j++];
			else if(j>hi) a[k] = aux[i++];
			else if(less(aux[j], aux[i])) a[k] = aux[j++];
			else a[k] = aux[i++];
		}
	}
	
	public static void sort(Comparable[] a){
		aux = new Comparable[a.length];
		sort(a,0,a.length-1);
	}
{% endhighlight %}

## 5. 快排 ##

**思想：**在归并排序基础上进行优化，用partition方法把数组切分再递归；partition方法将输入数组第一个元素u作为分割线，将小于u的元素放在其左边，大于u的元素放在右边，返回该元素重排后的下标；

**partition:**i控制当前元素，j控制大于u的子数组的开端-1，若a[i]>u,交换i++，j--；直到i<j;

为了减小概率的影响，排序前首先打乱输入数组；


<table>
<tr>
<td>稳定性</td>
<td>是否原地排序</td>
<td>时间复杂度</td>
<td>空间复杂度</td>
<td>备注</td>
</tr>
<tr>
<td>否</td>
<td>是</td>
<td>NlogN</td>
<td>lgN</td>
<td>效率由概率提供保证</td>
</tr>
</table>

**优点：**最快的通用排序算法；较好的时间、空间复杂度；内循环短小

**缺点：**重复元素较多时性能变差；

**核心代码：**

{% highlight java %}

     public static void sort(Comparable[] a){
		StdRandom.shuffle(a);
		//设置最右侧哨兵
	//		int max = 0;
	//		for(int i=1;i<a.length;i++){
	//			if(less(a[max],a[i]))
	//				max = i;
	//		}
	//		each(a, max, a.length-1);
		
		sort(a,0,a.length-1);
	}
	
	public static void sort(Comparable[] a,int lo,int hi){
		if(hi <= lo + 28) {
			Insertion.sort(a, lo, hi);
			return;
		};
		int j = partition(a,lo,hi);
		sort(a, lo, j);
		sort(a, j+1, hi);
	}
	

	
	public static int partition(Comparable[] a,int lo,int hi){
		int i = lo,j = hi + 1;
		Comparable u = a[lo];
		while(true){
			while(less(a[++i],u)){
				//由于一定有a[hi+1]>u,因此无需边界判断
				if(i == hi)
					break;
			}
			while(less(u,a[--j])){
				//由于一定有a[lo]=u,因此无需边界判断
	//				if(j == lo)
	//					break;
			}
			if(i >= j){
				break;
			}
			each(a, i, j);
		}
		each(a, lo, j);
		return j;
	}

{% endhighlight %}


## 6. 3-ways快排 ##

**思想：**对于有大量重复元素的输入进行优化，将子数组切分成大于、等于、小于三部分；i控制当前元素,it控制小于的子数组的尾端+1；gt控制大于的子数组的开端-1；

**核心代码：**

{% highlight java %}

     	public static void sort(Comparable[] a){
		StdRandom.shuffle(a);	
		sort(a,0,a.length-1);
	}
	
	public static void sort(Comparable[] a,int lo,int hi){
		if(hi <= lo) {
			return;
		};
		int it = lo , i = lo + 1, gt = hi;
		Comparable v = a[lo];
		while(i <= gt){
			int cmp = v.compareTo(a[i]);
			if(cmp > 0){
				each(a, i++, it++);
			}
			if(cmp < 0){
				each(a, i, gt--);
			}
			else i++;
		}
		sort(a, lo, it-1);
		sort(a, gt+1, hi);
	}

{% endhighlight %}

## 7. 堆排序 ##

**思想：**利用优先队列的思想，先将输入数组构建成有序的满二叉树，再逐个将最大或最小元素放到最后，二叉树大小减1，恢复二叉树；当二叉树被消灭时，排序完成；

<table>
<tr>
<td>稳定性</td>
<td>是否原地排序</td>
<td>时间复杂度</td>
<td>空间复杂度</td>
<td>备注</td>
</tr>
<tr>
<td>否</td>
<td>是</td>
<td>NlogN</td>
<td>1</td>
<td></td>
</tr>
</table>

**优点：**唯一能够同时最优的利用空间和时间的方法；

**缺点：**比较在不相邻元素间进行，故无法利用缓存，缓存未命中次数高；

**核心代码：**

{% highlight java %}

     	public static void sort(Comparable[] a){
		int N = a.length;
		//构建maxPQ
		for(int k=N/2;k>=1;k--){
			sink(a, k, N);
		}
		//将max放到最后并减小PQ的大小
		while(N > 1){
			each(a, 1, N--); 
			//恢复maxPQ
			sink(a, 1, N);
		}
	}
	
	private static void sink(Comparable[] a,int k,int N) {
		while(2*k <= N){
			int j = 2*k;
			if(j<N&&less(a,j, j+1))
				j++;
			if(!less(a,k, j)) break;
			each(a,k, j);
			k=j;
		}
	}
{% endhighlight %}









原创文章转载请注明出处: [几种排序算法总结]( http://yxzhangbupt.github.io/java/2016/03/01/algorithm-sort.html)
