---
layout: post
title: java 异常类型
date: 2016-03-06 12:20:00
category: "java"
---
  编译时异常，运行时异常，已检查异常，未检查异常类型总结。

---------------------------------


## 异常产生的时间和原因  ##

异常分为运行时异常和编译时异常，异常产生的原因有很多，例如：打开不存在的文件，网络连接问题， 加载时类文件缺失等等。

**错误和异常的区别**

错误表明程序出现了很严重的问题或非正常的状况，应用不应该尝试去处理这样的错误。在正常环境下，程序不应该尝试去 捕捉错误，例如内存错误，硬件错误，JVM 错误等等。

异常表明了代码内部的状况，开发者应该处理这样的状况并采取必要的措施，例如：





- DivideByZero exception


- NullPointerException


- ArithmeticException


- ArrayIndexOutOfBoundsException

**异常的类型**

异常有两种类型：



1. 已检查异常


1. 未检查异常

**已检查异常**

除了运行时异常以外的所有异常都被称作已检查异常，编译器会在编译期间去检查程序猿是否处理了它们。如果这些异常没有 被处理，将会出现编译错误。

已检查异常的例子：



- ClassNotFoundException


- IllegalAccessException


- NoSuchFieldException


- EOFException

**未检查异常**

运行时异常也被称作为未检查异常。编译器并不会检查是否有程序员处理这些异常，但程序员有处理这些异常的责任，并程序提供一个 安全的退出

未检查异常的例子：


- ArithmeticException


- ArrayIndexOutOfBoundsException


- NullPointerException


- NegativeArraySizeException


- StackOverFlowException

异常层次结构：

![异常层次结构](/images/Exception.png)
