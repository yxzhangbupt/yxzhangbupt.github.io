---
layout: post
title: Eclipse中的快捷键分享
date: 2015-03-22 18:15:49
category: "java"
---


### 在使用eeclipse编程的时候，为了达到流畅的编程体验，大家都希望能够双手不用离开键盘就能够完成所有操作，这就需要使用eclipse的快捷键了。下面我分享一些常用的eclipse快捷键，方便大家编程。

* 按住`Ctrl`，把鼠标放到类名上，可以看到类名处有下划线，点击即可跳转到代码处。同样作用的快捷键是`F3`。 
* 刚开始跳转到JDK的类库代码时会需要`Attach Source`一下，比如跳转到String类。点击`Attach Source`，再点击弹出窗口的`External File`。然后选择路径`“C:/Program Files/Java/jdk1.6.0_11/src.zip”`（假设安装在C盘），确定后就能见到代码了。JDK类库的代码是非常棒的，值得钻研。 
* 很实用的快捷键介绍： 

>F4：打开类型层次结构 
>Alt + / ： 补全代码，本来Ctrl + Space也可以的，但和输入法切换冲突了。 
>Ctrl + 1 : 快速修正。 
>Ctrl + Shift + O : 整理你的import部分的内容，会把多余的import项清理掉，也可以自动引入所需要得包。 
>Ctrl + Shift + T : 打开类型，输入类名能够快速转到那个文件，我很喜欢的快捷键。 
>Ctrl +Shift + F ： 代码排版， 为了是你写的程序代码版面更清晰，你可以尝试使用该热键 
>Ctrl + O : 快速大纲：打开当前所选类型的轻量级大纲图。在一个文件中直接打开一个成员变量（如字段、方法），尤其是有许多类似的方法名的时候这个快捷键将变得非常有用。 
>Ctrl + / : 添加或去除注释。 

* 有时候需要查看某个**方法被哪些地方调用了**，我们可以在这个方法上点击右键，选择`“Open Call Hierarchy”`，这时在开发环境的下方，会出现`Call Hierarchy`窗口，显示该方法被调用的情况。 
   
###另外一些常用的快捷键(Thanks to Aaron12) 

>ctrl + D 删除选择行   
>ctrl + F 查找/替换   
>ctrl + <- 返回上一次光标所在，在不同的Java文件来回切换很方便（very 有用的），尤其是debug的时候。   
>ctrl + alt + Z ： 某行代码调用生命抛出异常的方法时，选择该行代码，使用本组合快捷键，很方便的补全try{}catch(){}   
>alt+shift+R： 重构。 选择变量名称或者方法名称后，按该键进行重构。   
>shift+home ： 选择光标所在行行首 至 光标的内容   
>shift+end ： 选择 光标至光标所在行行尾处的内容   

   
###另外f1 - f8 都需要熟悉。

>f5-f8 在debug的时候很方便。   
>F5: Step into 跟踪进入下一个要执行的方法，跟踪进入一个方法会增加一个栈帧。 
>F6: Step over 结束当前行的执行，在下一个可执行的行处挂起。 
>F7: Step Return 从当前方法中跳出。执行将继续，知道执行了当前方法中的return语句为止。 
>F8: Resume 继续执行线程，直到它结束或遇到断点为止。 


>ctrl + page up / page down : 切换java文件 或者 切换同一个文件中的不同视图（比如：一个文件有xml 和 ui 视图）   
>ctrl + p ： 打印   
>ctrl + s 保存   
>ctrl + shift + s 全部保存   
>ctrl + w 关闭当前文件   
>ctrl + shift + w 关闭所有文件   
>ctrl + D 删除选择行 
>ctrl + F 查找/替换 
>ctrl + <- 返回上一次光标所在，在不同的Java文件来回切换很方便（very 有用的），尤其是debug的时候。   
>ctrl + alt + Z ： 某行代码调用生命抛出异常的方法时，选择该行代码，使用本组合快捷键，很方便的补全try{}catch(){} 
>alt+shift+R： 重构。 选择变量名称或者方法名称后，按该键进行重构。 
>shift+home ： 选择光标所在行行首 至 光标的内容 
>shift+end ： 选择 光标至光标所在行行尾处的内容   

   
另外别忘了 ESC 键也是很常用的，当打开的窗口没用需要的内容的时候（比如：使用ctrl+o，ctrl+shift+t查找不到内容时），使用esc键比用鼠标关闭窗口可会快多了  



  







原创文章转载请注明出处: [Eclipse中的快捷键分享](http://yxzhangbupt.github.io/java/2015/03/22/Eclipse-HotKey.html)
