---
layout: post
title: Java中的设计模式
date: 2016-03-06 14:20:00
category: "java"
---
 	

> 来源：stackoverflow
>
>译者：刘家财
>
>链接：http://www.importnew.com/12526.html

---------------------------------



提问：我正在学习GoF的《设计模式》，想了解些它们在实际中的应用的例子。大家能给我举一些使用设计模式的好例子吗？尤其是在Java类库中。


赞同最高的回答：

你可以通过Wikipedia对设计模式有个整体上的理解。Wikipedia上也提高了GoF所涉及到的模式。我这里总结一下，并且尽可能指出在JavaSE与JavaEE的API中是如何运用这些模式的。

## 创建型设计模式 ##


**抽象工厂模式**

特点：创建方法返回一个可以用来创建抽象类或接口的工厂类。



- javax.xml.parsers.DocumentBuilderFactory#newInstance()


- javax.xml.transform.TransformerFactory#newInstance()


- javax.xml.xpath.XPathFactory#newInstance()

**生成器模式**

特点:创建方法返回这个实例本身。



- java.lang.StringBuilder#append()不同步


- java.lang.StringBuffer#append()同步


- java.nio.ByteBuffer#put()（除此之外，还有CharBuffer,ShortBuffer,IntBuffer,LongBuffer,FloatBuffer与DoubleBuffer）


- javax.swing.GroupLayout.Group#addComponent()


- java.lang.Appendable的所有实现

**工厂方法模式**

特点：创建方法返回抽象类或接口的实现。



- java.util.Calendar#getInstance()


- java.util.ResourceBundle#getBundle()


- java.text.NumberFormat#getInstance()


- java.nio.charset.Charset#forName()


- java.net.URLStreamHandlerFactory#createURLStreamHandler(String)对于每个协议（protocol）返回一个单例对象

**原型模式**

特点：创建方法返回一个同类型且具有相同属性的另一个实例。



- java.lang.Object#clone()（这个类必须实现java.lang.Cloneable）

**单例模式**

特点：创建方法返回同一个实例（无论在何时调用）。



- java.lang.Runtime#getRuntime()


- java.awt.Desktop#getDesktop()

## 结构型模式 ##

**适配器模式**

特点：创建方法接受一个与当前类不同的抽象类或接口的实例作为参数，返回一个经过修饰或重写给定参数实例的抽象类或接口的实现。



- java.util.Arrays#asList()


- java.io.InputStreamReader(InputStream)返回一个Reader对象


- java.io.OutputStreamWriter(OutputStream)返回一个Writer对象


- javax.xml.bind.annotation.adapters.XmlAdapter#marshal()与#unmarshal()

**桥接模式**

特点：创建方法接受一个与当前类不同的抽象类或接口的实例作为参数，返回一个经过代理或使用给定参数实例的抽象类或接口的实现。

暂时没有想到，一个可以想到的例子是new LinkedHashMap(LinkedHashSet
, List)，这个方法返回一个不可修改的linkedMap，它就没有拷贝参数中的元素（item），而是直接使用它们。

- java.util.Collections#newSetFromMap()和singletonXXX()方法也与之类似。

**组合模式**

特点：行为方法把相同抽象类或接口的实例转化为一个树结构。



- java.awt.Container#add(Component)（几乎对所有的Swing都适用）


- javax.faces.component.UIComponent#getChildren()（几乎对所有的JSF UI都适用）

**装饰模式**

特点：创建方法以一个抽象类或接口的实例为参数，返回值是增加了额外方法的给参数实例。



- java.io.InputStream、OutputStream、Reader、Writer这些类的所有子类，它们都有一个接受相同类型作为参数的构造函数。


- java.util.Collections中checkedXXX()、synchronizedXXX()、unmodifiableXXX()方法


- javax.servlet.http.HttpServletRequestWrapper与HttpServletResponseWrapper

**外观模式**

特点：行为方法在内部使用完全不同的抽象类或接口的实例做封装。



- javax.faces.context.FacesContext，这个类在内部使用了抽象类或接口LifeCycle、ViewHandler、NavigationHandler以及其他一些用户不需要关心的类（通常这些类都是可通过注入重写的）。


- javax.faces.context.ExternalContext，这个类在内部使用了ServletContext、HttpSession、HttpServletRequest、HttpServletResponse等。

**享元模式**

特点：创建方法返回一个缓存的实例，与多例模式有些类似。



- java.lang.Integer#valueOf(int)（除此之外，类似的还有Boolean、Byte、Character、Short、Long）

**代理模式**

特点：创建方法返回一个给定抽象类或接口的实例，这个实例代理或使用了这个给定抽象类或接口的另一个实现。



- java.lang.reflect.Proxy

- java.rmi.*包下的所有类

恕我直言，Wikipedia上的类子不是很好，惰性加载实际上和代理模式一点关系也没有。

## 行为模式 ##

**职责链模式**

特点:行为方法间接调用队列中同一抽象类或接口的另一实例的同名方法。



- java.util.logging.Logger#log()


- javax.servlet.Filter#doFilter()

**命令模式**

特点：一个抽象类或接口中的行为方法调用另一个在创建时经命令方法包装的抽象类或接口实现的另一个方法。



- java.lang.Runnable的所有实现


- javax.swing.Action的所有实现

**解释器模式**

特点：行为方法返回结构上不同的抽象类或接口的实例，需要注意的是解析或格式化过程并不是这个模式的一部分，而这个过程决定了解释器如何将要去应用并实施这个变换。



- java.util.Pattern


- java.text.Normalizer


- java.text.Format类的所有子类


- javax.el.ELResolver类的所有子类

**迭代模式**

特点：行为方法连续地返回队列中的相同对象的不同实例。



- java.util.Iterator类的所有实现（java.util.Scanner也类似）


- java.util.Enumeration类的所有实现

**中介者模式**

特点：行为方法接受一个不同的抽象类或接口的实例（一般使用命令模式）作为参数，而这个参数同时也代理了其它给定抽象类或接口的实例。



- java.util.Timer所有的scheduleXXX()方法


- java.util.concurrent.Executor#execute()


- java.util.concurrent.ExecutorService的invokeXXX()与submit()方法


- java.util.concurrent.ScheduledExecutorService所有scheduleXXX()方法


- java.lang.reflect.Method#invoke()

**备忘模式**

特点：行为方法在内部改变整个实例的状态。



- java.util.Date（setter方法就是典型的例子，Date对象在内容是用一个long值来表示的）


- java.io.Serializable的所有实现


- javax.faces.component.StateHolder的所有实现

**观察者（发布/订阅）模式**

特点：行为方法根据其自身的状态，去调用另一个抽象类或接口实例的方法。



- java.util.Observer/java.util.Observable在实际中用的比较少


- java.util.EventListener的所有实现（几乎对所以Swing对象都适用）


- javax.servlet.http.HttpSessionBindingListener
http://docs.oracle.com/javaee/6/api/javax/servlet/http/HttpSessionAttributeListener.html


- javax.faces.event.PhaseListener

**状态模式**

特点：行为方法根据它能够控制的状态来变自身的行为。



- javax.faces.lifecycle.LifeCycle#execute()由FacesServlet控制，它的行为依赖与当前JSF的生命周期所处的阶段

**策略模式**

特点：在一个抽象类或接口的行为方法会调用作为一种策略实现而通过参数传入的另一个抽象类或接口实例的方法。



- java.util.Comparator#compare()，这个方法被Collections#sort()所调用


- javax.servlet.http.HttpServlet类中的service()与所有的doXXX()方法接受HttpServletRequest与HttpServletResponse这两个参数（并不是通过像成员变量这种方式容纳它们），这样其所有子类都必须去处理它们


- javax.servlet.Filter#doFilter()

**模板方法模式**

特点：具有由抽象类型定义默认行为的行为方法。



- java.io.InputStream、java.io.OutputStream、java.io.Reader与java.io.Writer类的所有非抽象方法


- java.util.AbstractList、java.util.AbstractSet、java.util.AbstractMap的所有非抽象方法


- javax.servlet.http.HttpServlet类的所有doXXX()方法默认发送HTTP 405“方法不允许”这个错误给response，你可以自由地选择是否去实现它们

**访问者模式**

特点：由两个不同的抽象类或接口，它们都有接受对方做参数的方法，被调用的方法会去调用另一个对象的方法，然后根据制定好的策略执行。



- javax.lang.model.element.AnnotationValue与AnnotationValueVisitor


- javax.lang.model.element.Element与ElementVisitor


- javax.lang.model.type.TypeMirror与TypeVisitor