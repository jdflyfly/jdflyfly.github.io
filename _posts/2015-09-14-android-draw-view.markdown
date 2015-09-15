---
layout: post
title:  "How Android Draws Views"
date:   2015-09-14 19:11:00
categories: android
---

* TOC
{:toc}


每个View的绘制过程都要经历三个主要的过程：measure、layout和draw

## View绘制过程

### measure

计算当前控件所需要的大小。执行完毕后，该View的尺寸已经得到确认，需要使用的话，调用View.getMeasuredWidth()和View.getMeasuredHeight()来获取。

### layout

根据
在onLayout()过程结束后，我们就可以调用getWidth()方法和getHeight()方法来获取视图的宽高了。

### draw

ViewRoot中的代码会继续执行并创建出一个Canvas对象，然后调用View的draw()方法来执行具体的绘制工作。

## 参考

[官方](https://developer.android.com/guide/topics/ui/how-android-draws.html)
[博客1](http://www.cnblogs.com/kross/p/4029570.html)
[博客2](http://www.devtf.cn/?p=582)
[博客3](http://blog.csdn.net/guolin_blog/article/details/12921889)