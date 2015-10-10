---
layout: post
title:  "Android Touch Event"
date:   2015-09-15 19:11:00
categories: android
---

* TOC
{:toc}

主要涉及dispatchTouchEvent(), onTouchEvent(), onInterceptTouchEvent()和OnTouchListener接口等。


## Activity中的触摸事件API

{% highlight java %}
public boolean dispatchTouchEvent(MotionEvent ev);
public boolean onTouchEvent(MotionEvent ev);
{% endhighlight %}


## ViewGroup中的触摸事件API

{% highlight java %}
public boolean dispatchTouchEvent(MotionEvent ev);
public boolean onTouchEvent(MotionEvent ev);
public boolean onInterceptTouchEvent(MotionEvent ev);
{% endhighlight %}

## View中的触摸事件API

{% highlight java %}
public boolean dispatchTouchEvent(MotionEvent ev);
public boolean onTouchEvent(MotionEvent ev);
{% endhighlight %}

## dispatchTouchEvent

dispatchTouchEvent负责整体调度触摸事件。 
返回值：true，表示触摸事件被消费了；false，则表示触摸事件没有被消费。

## onTouchEvent

处理触摸事件的接口。
返回值：返回true，表示它消费了触摸事件。否则，表示它没有消费该触摸事件。

## onInterceptTouchEvent

只有ViewGroup中才有该接口。如果ViewGroup不想将触摸事件传递给它的子View，则可以在onInterceptTouchEvent中进行拦截。
返回值：true表示ViewGroup拦截了该触摸事件，该事件就不会分发给它的子View或者子ViewGroup。false表示ViewGroup没有拦截该事件，该事件就会分发给它的子View和子ViewGroup。

## OnTouchListener接口

onTouch()与onTouchEvent()都是用户处理触摸事件的API。

* onTouch()是View专门提供给用户的接口，目的是为了方便用户自己处理触摸事件。而onTouchEvent()是Android系统自己实现的接口。
* onTouch()的优先级比onTouchEvent()的优先级更高。dispatchTouchEvent()中分发事件的时候，会先将事件分配给onTouch()进行处理，然后才分配给onTouchEvent()进行处理。 如果onTouch()对触摸事件进行了处理，并且返回true；那么，该触摸事件就不会分配在分配给onTouchEvent()进行处理了。只有当onTouch()没有处理，或者处理了但返回false时，才会分配给onTouchEvent()进行处理。



## Activity触摸事件处理

* Activity中的dispatchTouchEvent会将触摸事件传递给Activity所包含的根视图，即ViewGroup的dispatchTouchEvent()。
* Activity中的onTouchEvent是Activity自身对触摸事件的处理。

## View触摸事件处理

* view的dispatchTouchEvent()中分发事件的时候，会先将事件分配给onTouch()进行处理，然后才分配给onTouchEvent()进行处理。 如果onTouch()对触摸事件进行了处理，并且返回true；那么，该触摸事件就不会分配在分配给onTouchEvent()进行处理了。
* view的onTouchEvent()是Android系统提供的，用于处理触摸事件的接口；在onTouchEvent()中会进行一系列的动作，例如获取焦点、设置按下状态、调用onClick、调用onLongClick等。


## ViewGroup触摸事件处理

* ViewGroup中的dispatchTouchEvent()会将触摸事件进行递归遍历传递。ViewGroup会遍历它的所有孩子，对每个孩子都递归的调用dispatchTouchEvent()来分发触摸事件。
* 如果ViewGroup的某个孩子没有接受(消费或者拦截)ACTION_DOWN事件；那么，ACTION_MOVE和ACTION_UP等事件也一定不会分发给这个孩子。
* ViewGroup的onInterceptTouchEvent()默认返回false。



## 参考

[博客1](http://wangkuiwu.github.io/2015/01/01/TouchEvent-Introduce/)
[博客2](http://blog.csdn.net/guolin_blog/article/details/9097463)