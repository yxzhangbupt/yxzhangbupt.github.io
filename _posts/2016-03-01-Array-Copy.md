---
layout: post
title: java 数组复制:System.arrayCopy 深入解析
date: 2016-03-05 12:00:13
category: "java"
---
  转载：http://happyjin2010.iteye.com/blog/1073195 

---------------------------------


## 先看ArrayList源码中数组复制的代码:  ##

其实ArrayList 就是一个数组的形式存放数据的. 没有高深的地方.他的性能在于他的索引能力, 正因为他是数组形式,所以索引元素的时候他表现得非常的快速成,试想一下, 只要知道这个元素的索引,E[2] 你看对像就出来了.这就是ArrayList 最突出的地方. 

让我们来看下ArrayList 内部数组是如何自我Copy的.要想深入的了解他就必需要看他的API,add 方法与remove 方式. 
看完后你就会对它有一个深刻的理解了.如下原码: 

{% highlight java linenos %}
public void add(int index, E element) {   
  if (index > size || index < 0)   
       throw new IndexOutOfBoundsException( "Index: "+index+", Size: "+size);   
   ensureCapacity(size+1); // Increments modCount!!   
  System.arraycopy(elementData, index, elementData, index + 1, size - index);   
  elementData[index] = element;   
  size++;   
}  
{% endhighlight %}


remove 方法 


{% highlight java linenos%}
public E remove(int index) {   
  RangeCheck(index);   
  modCount++;   
  E oldValue = elementData[index];   
  int numMoved = size - index - 1;   
  if (numMoved > 0)   
   System.arraycopy(elementData, index+1, elementData, index, numMoved);   
  elementData[--size] = null; // Let gc do its work   
   return oldValue;   
}  
{% endhighlight %}

上述两个方法足以让你认识他们了.他的主要执行过程就在于数组对像的自我复制.System.arrayCopy. 这个方法是 **System类中的一个JNI方式实现类.(JNI , Java Native Interface 故名思意,就是java 语言调其它语言的一个接口) 这个JNI的底层在不同的平台上不一样.打个比方windows 其实java的JNI就是调了dll . Unix 其实就是调了.so 共享库. 做过C++的一定明白.这个暂且放一下,让我们来关注一下arrayCopy 如何复制数组元素的. 如果有人对java 的JNI接口有兴趣朋友,不防去Sun网站下它的源码.嘎嘎. C代码还是有点深度的.SCSL 源码就能看到.  **

  在JAVA里面,可以用复制语句"A=B"给基本类型的数据传递值,但是如果A,B是两个同类型的数组,复制就相当于将一个数组变量的引用传递给另一个数组;如果一个数组发生改变,那么引用同一数组的变量也要发生改变. 

以下是归纳的JAVA中复制数组的方法：

**1.使用FOR循环,将数组的每个元素复制或者复制指定元素,不过效率差一点 
2.使用clone方法,得到数组的值,而不是引用,不能复制指定元素,灵活性差一点 
3.使用System.arraycopy(src, srcPos, dest, destPos, length)方法,推荐使用  **

举例： 

1.使用FOR循环 

int[] src={1,3,5,6,7,8}; 

int[] dest = new int[6]; 

for(int i=0;i<6;i++) dest[i] = src[i]; 

2.使用clone 

int[] src={1,3,5,6,7,8}; 

int[] dest; 

dest=(int[]) src.clone();//使用clone创建 

副本,注意clone要使用强制转换 

3.使用System.arraycopy 

int[] src={1,3,5,6,7,8}; 

int[] dest = new int[6]; 

System.arraycopy(src, 0, dest, 0, 6); 
-------------------------------------------------

 System提供了一个静态方法arraycopy(),我们可以使用它来实现数组之间的复制. 
其函数原型是： 

	public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length) 

src:源数组; srcPos:源数组要复制的起始位置; 

dest:目的数组; destPos:目的数组放置的起始位置; 

length:复制的长度. 

注意：src and dest都必须是同类型或者可以进行转换类型的数组． 

有趣的是这个函数可以实现自己到自己复制, 

比如：int[] fun ={0,1,2,3,4,5,6}; 

System.arraycopy(fun,0,fun,3,3); 

则结果为：{0,1,2,0,1,2,6}; 

这是StringBuffer的toString的实现，里面也包括System.arraycopy 


{% highlight java linenos %}
public synchronized String toString() {  
	turn new String(value, 0, count);  
 }  
  
  
public String(char value[], int offset, int count) {  
     if (offset < 0) {  
         throw new StringIndexOutOfBoundsException(offset);  
     }  
     if (count < 0) {  
         throw new StringIndexOutOfBoundsException(count);  
     }  
     // Note: offset or count might be near -1>>>1.  
     if (offset > value.length - count) {  
         throw new StringIndexOutOfBoundsException(offset + count);  
     }  
     char[] v = new char[count];  
     System.arraycopy(value, offset, v, 0, count);  
     this.offset = 0;  
     this.count = count;  
     this.value = v;  
 }  
{% endhighlight %}



