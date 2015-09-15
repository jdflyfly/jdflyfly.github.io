---
layout: post
title:  "Effective Go"
date:   2015-09-08 19:11:00
categories: golang
---

* TOC
{:toc}

## Introduction

Go语言是一门新语言，虽然借鉴了很多现有的语言，但是本身还是有很多独特的性质。

## Formatting

利用gofmt格式化go代码。

## Commentary

* Go注释支持C++的行级注释和块级注释。
* 每个包应该有一个包注释，可以定义在包内的任何文件中。
* 程序中每个可导出的声明都应该有一个注释，注释的形式是一个完整的句子，以注释的内容开头（比如函数名），句点作为结尾。
* 利用godoc可以生成注释文档。

例子：

{% highlight C++ %}
// Package strings implements simple functions to manipulate strings.
package strings

import ...

// explode splits s into an array of UTF-8 sequences, one per Unicode character (still strings) up to a maximum of n (n < 0 means no limit).
// Invalid UTF-8 sequences become correct encodings of U+FFF8.
func explode(s string, n int) []string {
	if n == 0 {
		return nil
	}
	l := utf8.RuneCountInString(s)
	if n <= 0 || n > l {
		n = l
	}
	...
}
{% endhighlight %}


## Names

* Go通过首字母是否大写决定该元素在包外的可见性
* 对于多个单词的标识符，Go使用MixedCap和mixedCaps命名规则，而不用underscores规则。


## Semicolons

Go的词法分析器会自动补充分号

## Control structures

### If

普通的if，条件不需要加括号，大括号是必须的
{% highlight C++ %}
if x > 0 {
    return y
}
{% endhighlight %}

if 和 switch支持初始化语句，可以用来定义局部变量
{% highlight C++ %}
if err := file.Chmod(0664); err != nil {
    log.Print(err)
    return err
}
{% endhighlight %}


Go里，如果if语句某种情况下不会执行后面的语句（比如return，break等），不用的else被忽略。
{% highlight C++ %}
f, err := os.Open(name)
if err != nil {
    return err
}
codeUsing(f)
{% endhighlight %}

下面是一系列处理错的情况，注意到并不需要else语句。
并且注意到，err变量"貌似"被重声明了两次，Go有个规则，只要 := 左边有新声明的变量（d），可以同时赋值已有的变量（err，主要err要在同一个scope中）。

{% highlight C++ %}
f, err := os.Open(name)
if err != nil {
    return err
}
d, err := f.Stat()
if err != nil {
    f.Close()
    return err
}
codeUsing(f, d)
{% endhighlight %}


### For

常用的for循环形式
{% highlight C++ %}
// Like a C for
for init; condition; post { }

// Like a C while
for condition { }

// Like a C for(;;)
for { }
{% endhighlight %}


Go没有逗号操作符，如果想该表多个变量值，需要使用平行赋值

{% highlight C++ %}
// Reverse a
for i, j := 0, len(a)-1; i < j; i, j = i+1, j-1 {
    a[i], a[j] = a[j], a[i]
}
{% endhighlight %}


### Switch

Go的switch更加通用
{% highlight C++ %}
func unhex(c byte) byte {
    switch {
    case '0' <= c && c <= '9':
        return c - '0'
    case 'a' <= c && c <= 'f':
        return c - 'a' + 10
    case 'A' <= c && c <= 'F':
        return c - 'A' + 10
    }
    return 0
}
{% endhighlight %}

并且支持逗号多选
{% highlight C++ %}
func shouldEscape(c byte) bool {
    switch c {
    case ' ', '?', '&', '=', '#', '+', '%':
        return true
    }
    return false
}
{% endhighlight %}

switch使用的实例
{% highlight C++ %}
// Compare returns an integer comparing the two byte slices,
// lexicographically.
// The result will be 0 if a == b, -1 if a < b, and +1 if a > b
func Compare(a, b []byte) int {
    for i := 0; i < len(a) && i < len(b); i++ {
        switch {
        case a[i] > b[i]:
            return 1
        case a[i] < b[i]:
            return -1
        }
    }
    switch {
    case len(a) > len(b):
        return 1
    case len(a) < len(b):
        return -1
    }
    return 0
}
{% endhighlight %}



switch也可以用来动态的发现接口变量的类型。
{% highlight C++ %}
var t interface{}
t = functionOfSomeType()
switch t := t.(type) {
default:
    fmt.Printf("unexpected type %T\n", t)     // %T prints whatever type t has
case bool:
    fmt.Printf("boolean %t\n", t)             // t has type bool
case int:
    fmt.Printf("integer %d\n", t)             // t has type int
case *bool:
    fmt.Printf("pointer to boolean %t\n", *t) // t has type *bool
case *int:
    fmt.Printf("pointer to integer %d\n", *t) // t has type *int
}
{% endhighlight %}




{% highlight C++ %}

{% endhighlight %}

[参考](http://golang.org/doc/effective_go.html)

