---
layout: post
title: 通配符和类型参数的区别
date: 2015-04-07 14:15:13
category: "java"
---

#通配符和类型参数的区别
----------------------------

1 泛型参数指具体的某个类型，一般在类体或者方法体中会引用类型变量，返回类型可以指定为参数类型T
	public class NumberTest<T extends Integer> {
		private T num;
	
		public NumberTest(T num) { this.num = num;}
	
		public boolean isOdd(){
			return num.intValue()%2 == 1;
		}
	
		//....
	}

2.通配符通常在方法参数引用一个泛型类型时使用，在泛型类型后边的<>中对该泛型类型进行限定，通常不会在类体或者方法体或返回类型中应用。

3.无界通配符的使用场景：

* **当方法是使用原始的Object类型作为参数时，如：**

		public static void printList(List<Object> list) {
    		for (Object elem : list)
        	System.out.println(elem + " ");
   	 		System.out.println();
		}

可改为以下方式实现：

	public static void printList(List<?> list) {
    for (Object elem: list)
        System.out.print(elem + " ");
    System.out.println();
	}

这样就可以兼容更多的输出，而不单纯是List<Object>,如下：

	List<Integer> li = Arrays.asList(1, 2, 3);
	List<String>  ls = Arrays.asList("one", "two", "three");
	printList(li);
	printList(ls);

* **在定义的方法体的业务逻辑与泛型类型无关**

如List.size,List.cleat。实际上，最常用的就是Class<?>,因为Class<T>并没有依赖于T。

最后提醒一下的就是，List<Object>与List<?>并不等同，List<Object>是List<?>的子类。还有不能往List<?> list里添加任意对象，除了null。








原创文章转载请注明出处: [通配符和类型参数的区别]( http://yxzhangbupt.github.io/java/2015/04/07/java-Generatic-programming.html)
