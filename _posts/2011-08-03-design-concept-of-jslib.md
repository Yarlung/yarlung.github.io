---
layout: blog-post
author: mytharcher
title: 设计理念（上）：基础库结构设计
category: introduction

tags: [JavaScript, js, library, 基础库, 结构设计, 规范]

trace: 博客 / 设计理念（上）：基础库结构设计
---

对于一个JavaScript基础库，最重要的无外乎API简单，稳定，有良好的通用性，只解决最基本的问题，同时便于理解学习。其实这么说起来很像百度的Tangram的初衷，但elf+js还是略有不同。

虽然JavaScript不是一门支持面向对象编程的语言，但他具有很多面向对象的特性。如果利用得当，面向对象的编程方法将是使用JS构建大型Web应用的较为容易分清各种模块结构的方式。基础库会涵盖很多基本的功能基于这个思路，如果按jQuery针对DOM的设计是不合适的，所有的扩展都会依附在jQuery对象本身上，很难从一个巨长的插件列表上方便的区分各种功能的类型。所以我转向参考了dojo、YUI、Ext和Tangram等，还是设计了“命名空间.包名.类名”的代码组织形式。同时我坚持使用了Java中命名规范：

* 包名小写，如：`js`，`js.dom`，`js.net`等；
* 类名首字母大写驼峰，如：`js.dom.ClassName`，`js.net.Ajax`等；
* 类成员(变量/方法)首字母小写驼峰，如：`js.dom.ClassName.add()`，`js.net.Ajax.option`等；
* 常量命名全部大写，如：`js.net.Ajax.HTTP_GET`等；
* 类名、包结构与文件名、文件夹一一对应；

这样设计的好处是结构条理清晰，便于理解和统一认识。

规范上除了上面的命名大小写等，还有一点最重要的就是所有的功能设计都需要依附于一个类，就算是很常用很通用的方法。例如给DOM元素添加class的`addClass`方法，在Tangram中这个API是`baidu.dom.addClass`，而在elf+js里是`js.dom.ClassName.add`。看起来长了一级命名空间，的确是很不方便，还有可能不利于理解，但实际上这是一种对认识的统一，因为这样规范了写法以后，不会出现一个包下既有类又有方法的情况。而且现在看起来可能难用一点，但是配合上elf以后就会变的非常简单。针对底层API规范的设计和易用性的设计有时候是互斥的，这个话题以后会谈到。

在文件划分粒度上主要以类为主，每一个类即一个文件，因为这都是包含同一组方法的相关功能，但允许一些相关性不大的静态方法的定义文件独立于主类文件之外。这样做的考虑是在打包的时候可以灵活选取需要的和剪除不要的，以达到最小导出打包的目的，同时方法仍是依附于一个类的，也不违反上面定义的规范。这个特性相关的例子可以在`js.dom.Stage.*`里查到。

综合上面提到的这些方面来看，elf+js的底层API部分是非常灵活的，可以任意的加入更多功能，而且通过这样的命名方式也很容易找到常用的API位置，再配合<a title="elf+js的API文档" href="http://elfjs.com/docs" target="_blank">API文档</a>，学习起来就变的非常简单了。