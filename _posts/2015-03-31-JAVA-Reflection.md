---
layout: post
title: 学习java反射机制
date: 2015-03-31 14:07:22
category: "java"
---

能够分析类能力的程序称为反射（reflection）。反射机制的功能极其强大，反射能够用来：

* 在运行中分析类的能力

* 在运行中查看对象
 
* 实现通用的数组操作代码	

* 利用Method对象，实现对于方法的访问和调用（类似于C++中的函数指针）

下面用一个Demo来总结反射的使用方法[[Demo下载地址](http://download.csdn.net/detail/u011133213/6420969)]：

{% highlight java %}
package com.yxzhang.demo;

import java.lang.reflect.Constructor;  
import java.lang.reflect.Field;  
import java.lang.reflect.InvocationTargetException;  
import java.lang.reflect.Method;  
import java.lang.reflect.Modifier;  
import java.lang.reflect.TypeVariable;  

public class ReflectionDemo {

	    
	    /** 
	     * 为了看清楚Java反射部分代码，所有异常我都最后抛出来给虚拟机处理！ 
	     * @param args 
	     * @throws ClassNotFoundException 
	     * @throws InstantiationException 
	     * @throws IllegalAccessException 
	     * @throws InvocationTargetException  
	     * @throws IllegalArgumentException  
	     * @throws NoSuchFieldException  
	     * @throws SecurityException  
	     * @throws NoSuchMethodException  
	     */  
	    public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, IllegalArgumentException, InvocationTargetException, SecurityException, NoSuchFieldException, NoSuchMethodException {  
	        // TODO Auto-generated method stub  
	          
	        //Demo1.  通过Java反射机制得到类的包名和类名  
	        Demo1();  
	        System.out.println("===============================================");  
	          
	        //Demo2.  验证所有的类都是Class类的实例对象  
	        Demo2();  
	        System.out.println("===============================================");  
	          
	        //Demo3.  通过Java反射机制，用Class 创建类对象[这也就是反射存在的意义所在]，无参构造  
	        Demo3();  
	        System.out.println("===============================================");  
	          
	        //Demo4:  通过Java反射机制得到一个类的构造函数，并实现构造带参实例对象  
	        Demo4();  
	        System.out.println("===============================================");  
	          
	        //Demo5:  通过Java反射机制操作成员变量, set 和 get  
	        Demo5();  
	        System.out.println("===============================================");  
	          
	        //Demo6: 通过Java反射机制得到类的一些属性： 继承的接口，父类，函数信息，成员信息，类型等  
	        Demo6();  
	        System.out.println("===============================================");  
	          
	        //Demo7: 通过Java反射机制调用类中方法  
	        Demo7();  
	        System.out.println("===============================================");  
	          
	        //Demo8: 通过Java反射机制获得类加载器  
	        Demo8();  
	        System.out.println("===============================================");  
	          
	    }  
	      
	    /** 
	     * Demo1: 通过Java反射机制得到类的包名和类名 
	     */  
	    public static void Demo1()  
	    {  
	        Person person = new Person();  
	        System.out.println("Demo1: 包名: " + person.getClass().getPackage().getName() + "，"   
	                + "完整类名: " + person.getClass().getName());  
	    }  
	      
	    /** 
	     * Demo2: 验证所有的类都是Class类的实例对象 
	     * @throws ClassNotFoundException  
	     */  
	    public static void Demo2() throws ClassNotFoundException  
	    {  
	        //定义两个类型都未知的Class , 设置初值为null, 看看如何给它们赋值成Person类  
	        Class<?> class1 = null;  
	        Class<?> class2 = null;  
	          
	        //写法1, 可能抛出 ClassNotFoundException [多用这个写法]  
	        class1 = Class.forName("cn.lee.demo.Person");  
	        System.out.println("Demo2:(写法1) 包名: " + class1.getPackage().getName() + "，"   
	                + "完整类名: " + class1.getName());  
	          
	        //写法2  
	        class2 = Person.class;  
	        System.out.println("Demo2:(写法2) 包名: " + class2.getPackage().getName() + "，"   
	                + "完整类名: " + class2.getName());  
	    }  
	      
	    /** 
	     * Demo3: 通过Java反射机制，用Class 创建类对象[这也就是反射存在的意义所在] 
	     * @throws ClassNotFoundException  
	     * @throws IllegalAccessException  
	     * @throws InstantiationException  
	     */  
	    public static void Demo3() throws ClassNotFoundException, InstantiationException, IllegalAccessException  
	    {  
	        Class<?> class1 = null;  
	        class1 = Class.forName("cn.lee.demo.Person");  
	        //由于这里不能带参数，所以你要实例化的这个类Person，一定要有无参构造函数哈～  
	        Person person = (Person) class1.newInstance();  
	        person.setAge(20);  
	        person.setName("LeeFeng");  
	        System.out.println("Demo3: " + person.getName() + " : " + person.getAge());  
	    }  
	      
	    /** 
	     * Demo4: 通过Java反射机制得到一个类的构造函数，并实现创建带参实例对象 
	     * @throws ClassNotFoundException  
	     * @throws InvocationTargetException  
	     * @throws IllegalAccessException  
	     * @throws InstantiationException  
	     * @throws IllegalArgumentException  
	     */  
	    public static void Demo4() throws ClassNotFoundException, IllegalArgumentException, InstantiationException, IllegalAccessException, InvocationTargetException  
	    {  
	        Class<?> class1 = null;  
	        Person person1 = null;  
	        Person person2 = null;  
	          
	        class1 = Class.forName("cn.lee.demo.Person");  
	        //得到一系列构造函数集合  
	        Constructor<?>[] constructors = class1.getConstructors();  
	          
	        person1 = (Person) constructors[0].newInstance();  
	        person1.setAge(30);  
	        person1.setName("leeFeng");  
	          
	        person2 = (Person) constructors[1].newInstance(20,"leeFeng");  
	          
	        System.out.println("Demo4: " + person1.getName() + " : " + person1.getAge()  
	                + "  ,   " + person2.getName() + " : " + person2.getAge()  
	                );  
	          
	    }  
	      
	    /** 
	     * Demo5: 通过Java反射机制操作成员变量, set 和 get 
	     *  
	     * @throws IllegalAccessException  
	     * @throws IllegalArgumentException  
	     * @throws NoSuchFieldException  
	     * @throws SecurityException  
	     * @throws InstantiationException  
	     * @throws ClassNotFoundException  
	     */  
	    public static void Demo5() throws IllegalArgumentException, IllegalAccessException, SecurityException, NoSuchFieldException, InstantiationException, ClassNotFoundException  
	    {  
	        Class<?> class1 = null;  
	        class1 = Class.forName("cn.lee.demo.Person");  
	        Object obj = class1.newInstance();  
	          
	        Field personNameField = class1.getDeclaredField("name");  
	        personNameField.setAccessible(true);  
	        personNameField.set(obj, "newName");  
	          
	          
	        System.out.println("Demo5: 修改属性之后得到属性变量的值：" + personNameField.get(obj));  
	          
	    }  
	      
	  
	    /** 
	     * Demo6: 通过Java反射机制得到类的一些属性： 继承的接口，父类，函数信息，成员信息，类型等 
	     * @throws ClassNotFoundException  
	     */  
	    public static void Demo6() throws ClassNotFoundException  
	    {  
	        Class<?> class1 = null;  
	        class1 = Class.forName("cn.lee.demo.SuperMan");  
	          
	        //取得父类名称  
	        Class<?>  superClass = class1.getSuperclass();  
	        System.out.println("Demo6:  SuperMan类的父类名: " + superClass.getName());  
	          
	        System.out.println("===============================================");  
	          
	          
	        Field[] fields = class1.getDeclaredFields();  
	        for (int i = 0; i < fields.length; i++) {  
	            System.out.println("类中的成员: " + fields[i]);  
	        }  
	        System.out.println("===============================================");  
	          
	          
	        //取得类方法  
	        Method[] methods = class1.getDeclaredMethods();  
	        for (int i = 0; i < methods.length; i++) {  
	            System.out.println("Demo6,取得SuperMan类的方法：");  
	            System.out.println("函数名：" + methods[i].getName());  
	            System.out.println("函数返回类型：" + methods[i].getReturnType());  
	            System.out.println("函数访问修饰符：" + Modifier.toString(methods[i].getModifiers()));  
	            System.out.println("函数代码写法： " + methods[i]);  
	        }  
	          
	        System.out.println("===============================================");  
	          
	        //取得类实现的接口,因为接口类也属于Class,所以得到接口中的方法也是一样的方法得到哈  
	        Class<?> interfaces[] = class1.getInterfaces();  
	        for (int i = 0; i < interfaces.length; i++) {  
	            System.out.println("实现的接口类名: " + interfaces[i].getName() );  
	        }  
	          
	    }  
	      
	    /** 
	     * Demo7: 通过Java反射机制调用类方法 
	     * @throws ClassNotFoundException  
	     * @throws NoSuchMethodException  
	     * @throws SecurityException  
	     * @throws InvocationTargetException  
	     * @throws IllegalAccessException  
	     * @throws IllegalArgumentException  
	     * @throws InstantiationException  
	     */  
	    public static void Demo7() throws ClassNotFoundException, SecurityException, NoSuchMethodException, IllegalArgumentException, IllegalAccessException, InvocationTargetException, InstantiationException  
	    {  
	        Class<?> class1 = null;  
	        class1 = Class.forName("cn.lee.demo.SuperMan");  
	          
	        System.out.println("Demo7: \n调用无参方法fly()：");  
	        Method method = class1.getMethod("fly");  
	        method.invoke(class1.newInstance());  
	          
	        System.out.println("调用有参方法walk(int m)：");  
	        method = class1.getMethod("walk",int.class);  
	        method.invoke(class1.newInstance(),100);  
	    }  
	      
	    /** 
	     * Demo8: 通过Java反射机制得到类加载器信息 
	     *  
	     * 在java中有三种类类加载器。[这段资料网上截取] 
	 
	        1）Bootstrap ClassLoader 此加载器采用c++编写，一般开发中很少见。 
	 
	        2）Extension ClassLoader 用来进行扩展类的加载，一般对应的是jre\lib\ext目录中的类 
	 
	        3）AppClassLoader 加载classpath指定的类，是最常用的加载器。同时也是java中默认的加载器。 
	     *  
	     * @throws ClassNotFoundException  
	     */  
	    public static void Demo8() throws ClassNotFoundException  
	    {  
	        Class<?> class1 = null;  
	        class1 = Class.forName("cn.lee.demo.SuperMan");  
	        String nameString = class1.getClassLoader().getClass().getName();  
	          
	        System.out.println("Demo8: 类加载器类名: " + nameString);  
	    }  
	      
	      
	      
	}  
	/** 
	 *  
	 * @author xiaoyaomeng 
	 * 
	 */  
	class  Person{  
	    private int age;  
	    private String name;  
	    public Person(){  
	          
	    }  
	    public Person(int age, String name){  
	        this.age = age;  
	        this.name = name;  
	    }  
	  
	    public int getAge() {  
	        return age;  
	    }  
	    public void setAge(int age) {  
	        this.age = age;  
	    }  
	    public String getName() {  
	        return name;  
	    }  
	    public void setName(String name) {  
	        this.name = name;  
	    }  
	}  
	  
	class SuperMan extends Person implements ActionInterface  
	{  
	    private boolean BlueBriefs;  
	      
	    public void fly()  
	    {  
	        System.out.println("超人会飞耶～～");  
	    }  
	      
	    public boolean isBlueBriefs() {  
	        return BlueBriefs;  
	    }  
	    public void setBlueBriefs(boolean blueBriefs) {  
	        BlueBriefs = blueBriefs;  
	    }  
	  
	    @Override  
	    public void walk(int m) {  
	        // TODO Auto-generated method stub  
	        System.out.println("超人会走耶～～走了" + m + "米就走不动了！");  
	    }  
	}  
	interface ActionInterface{  
	    public void walk(int m);  
	}  
}

{% endhighlight %}

补充:

1.获得class对象的三种方式：

* Object类中的getClass()方法将会返回一个Class类型的对象：

{% highlight java %}
	    Person p;
    ...
    Class c1 = p.getClass();
{% endhighlight %}

* 静态方法forName获得类名对应的Class对象(注意前边要有包名)

{% highlight java %}

    String className = "java.util.Date";
    Class c1 = class.forName(className);

{% endhighlight %}
* 若T是任意的java类型，T.class将返回匹配的类对象

{% highlight java %}

    Class c1 = Date.class;
    class c2 = int.class;
    Class c3 = Double[].class;

{% endhighlight %}
2.虚拟机为每个类型管理一个Class对象。因此，可以利用==运算符实现两个类对象比较的操作。例如：

{% highlight java %}

    if(p.getClass() == Person.class） ...

{% endhighlight %}
3.java.lang.reflection包中的Array类允许动态的创建数组。例如，我们可以利用这个特性创建一个copyOf静态方法实现任意数组扩容。

{% highlight java %}

    Person[] a = new Person[10];
    ...
    //array full
    a = Arrays.copyOf(a,2*a.length);

{% endhighlight %}
然而，如果我们创建新的数组时，用Object类来初始化，则调用方法后，数组中的对象无法被强制转型为原类型（会产生ClassCastException异常），如下所示：

{% highlight java %}

	public static Object[] badCopyOf(Object[] a,int newLength){
		Object[] newArray = new Object[newLength];	
		System.arrayCopy(a,0,newArray,0,Math.min(a.length,newLength));
		return newArray;
	}
{% endhighlight %}

其原因是讲一个Person[]临时转换成Object[]数组，然后再转换回来时可行的，但是一个从一开始就是Object[]的数组却永远无法转换成Person[]。因此我们可以使用class.getcomponentType()方法来获得数组成员类对象，再使用Array.newInstance(componentType,length)来创建新数组。

{% highlight java %}

	public static Object badCopyOf(Object[] a,int newLength){
		class c1 = a.getClass();
		if(!c1.isArray()) return null;
		calss componentType = c1.getcomponentType();
		int length = Array.getLength(a);
		Object newArray = Array.newInstance(componentType,newLength);	
		System.arrayCopy(a,0,newArray,0,Math.min(a.length,newLength));
		return newArray;
	}		
{% endhighlight %}	                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            
	











原创文章转载请注明出处: [学习java反射机制]( http://yxzhangbupt.github.io/java/2015/03/31/JAVA-Reflection.html)
