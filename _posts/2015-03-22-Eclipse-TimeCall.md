---
layout: post
title: 以固定格式获得系统的当前时间
date: 2015-03-22 19:13:11
category: "java"
---

我们在java程序中有时需要调用系统当前的时间，这个功能可以使用java类库中的**SimpleDateFormat**函数实现。
`SimpleDateFormat`函数支持以String作为参数输入时间合适，并用**format**方法将DATA类的时间转化为固定格式。

以下是代码示例：

{% highlight java %}
//输入String时间格式，返回当前系统时间
static private String getFormatTime(String pattern) 
   {
   String formatTime = new SimpleDateFormat(pattern).format(new Date());
   return formatTime;
   }
{% endhighlight %}

在main函数中调用该方法：

{% highlight java %}
public static void main(String args[]) throws IOException
	{
		String fileTime = TestFile.getFormatTime("yyyy-MM-dd");
		String postTime = TestFile.getFormatTime("yyyy-MM-dd HH:mm:ss");
		System.out.println(fileTime);
		System.out.println(postTime);
		
	}
{% endhighlight %}

运行结果如下：

>2015-03-22
>
>2015-03-22 19:22:27

![java](http://jy.shangxueba.com/uploadfile/201212/20121213134741.png)




原创文章转载请注明出处: [以固定格式获得系统的当前时间](http://yxzhangbupt.github.io/java/2015/03/22/Eclipse-TimeCall.html)
