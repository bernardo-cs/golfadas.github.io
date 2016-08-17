---
layout: post
title:  "Introduction to RxJS"
date:    2016-08-03
categories: js rxjs reactive
---

## Creating our own operator: multiplyBy
 
In order to get the best out of angular 2, I'm always trying to use reactive streams when passing data between components.
The problem is that I just tend to forget everything after spending a month working on backend related issues.

I'll be doing a series of blog posts, ( or notes ) from what I've been learning while discovering the world of RxJS.

This blog post is heavily based on [ Eggheads beyond the basics, operators in depth course ](https://egghead.io/courses/rxjs-beyond-the-basics-operators-in-depth). Here is how we can create hour own RxJS Observable:

RxJS operators receive a source observable and returns an observable, where the source is immutable: 

<a class="jsbin-embed" href="http://jsbin.com/difapu/8/embed?js,console">JS Bin on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?3.38.26"></script>

{% highlight bash %}
// | means completed
// x means error
{% endhighlight %}

Sending a number every second:
{% highlight bash %}
var foo = Rx.Observable.interval(1000);

/*
----1---2---3---4----5----...
*/
{% endhighlight %}

Send all numbers at once:
{% highlight bash %}
var foo = Rx.Observable.of(1,2,3,4); 

/*
(1234)--
*/
{% endhighlight %}

MultiplyBy implemented before, can be diagrammed as:

{% highlight bash %}
/*
foo: ----0---1---2---3---4----5----...

bar: ----0---2---4---6---8----10----...
*/
{% endhighlight %}

Multiply by can be generalized, and instead of receiving a number to multiply, it can receive a function which will turn it into a map, the base RxJS operation.

<a class="jsbin-embed" href="http://jsbin.com/fejeqe/embed?js,console">JS Bin on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?3.38.26"></script>

{% highlight bash %}
/*
foo: ----0---1---2---...

bar: ----0---6---12---...
*/
{% endhighlight %}
