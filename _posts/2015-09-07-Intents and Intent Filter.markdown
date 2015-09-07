---
layout: post
title:  "Intents and Intent Filter"
date:   2015-09-07 19:11:00
categories: android
---

* TOC
{:toc}

Intent是一个可以用来请求应用组件行为的消息对象。通常有三种基础的用例：

* 启动一个activity
* 启动一个service
* 发布一个broadcase

## Intent的类型

* 显式的
* 隐式的

## 构建一个Intent

Intent中主要包括以下信息：

* Component name
* Action
* Data
* Category
* Extra
* Flags

## Intent的解析配对

系统收到隐式Intent之后，基于以下三方面选择activity启动：

* action
* data(both URI and data type)
* category

### action

* 每个filter可以声明多个action，目标Intent至少匹配其中一个。
* 如果filter没有指定任何action，则不会匹配任何目标Intent。
* 如果目标Intent没有action，则只要filter还有任何action，即可匹配。

### category

* 每个filter可以声明多个category，目标Intent中的一个或多个category必须都存在于filter中才能匹配。
* 系统自动给 `startActivity` 的Intent增加 `CATEGORY_DEFAULT` 的category，所以注意filter中需要配置此category。

### data

每个filter可以声明多个data，每个data元素可以指定URI结构和MIME类型。
URI格式：`<scheme>://<host>:<port>/<path>`

date中的每个属性都是可选的，但是每个data元素的URI属性有如下线性依赖：

* 如果scheme没有指定，host被忽略。
* 如果host没有指定，post被忽略。
* 如果scheme和host都没有指定，path被忽略。 

Intent中的URI跟filter中的URI比较时，只比较filter中包含的部分：

* 如果filter中只有scheme，则所有含有该scheme的URI匹配。
* 如果filter中指定了scheme和authority，没有path，所有有该scheme和authority的匹配。
* 如果filter中指定了scheme authority path，只有具有相同scheme authority path的Intent匹配。

匹配比较时，需要同时比较URI和MIME type，规则如下：

* 既没有URI和MIMI的Intent可以跟没有指定任何URI和MIME的filter匹配。
* 有URI没有MIME（不管是显式的还是根据URI推导出来的）的Intent可以跟URI匹配并且没有制定MIME的filter匹配。
* 有MIME没有URI的Intent可以跟有同样MIME并且没有指定URI的filter 匹配。
* 既有URI也有MIME的Intent的情况
	* 对于MIME，只要Intent中的MIME类型在filter中有声明。
	* 对于URI，匹配filter中声明的URI或者intent有 `content:` 和 `file:` 的URI并且filter中没有声明URI.



参考：
[官方文档](http://developer.android.com/guide/components/intents-filters.html)
