---
layout: post
title: CSS趣味计数器
description: "CSS竟然还能做计数器，太强大了。来一探究竟吧。"
modified: 2014-11-23 00:23
tags: [翻译, CSS]
image:
  feature: abstract-4.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
type: fanyi
comments: true
---

CSS计数器是“晕，竟不知道CSS可以做这啊”这类非常有趣的众多特性之一。简言之，用CSS使你持续某增加某个量，而无需JavaScript。

## 简单计数器

我们从这个简单的分页示例开始：

<pre class="codepen" data-height="320" data-type="result" data-href="KkudD" data-user="lonekorean" data-safe="true"> <code> </code> <a href="http://http://codepen.io/lonekorean/embed/KkudD">Check out this Pen!</a> </pre>
<script src="http://codepen.io/assets/embed/ei.js"> </script>

你见到的这些数字不是硬编码在HTML中。它们是以下CSS生成的：
{% highlight css %}
body {
    counter-reset: pages; // initialize counter
}
 
a {
    counter-increment: pages; // increment counter
}
 
a::before {
    content: counter(pages); // display counter
}
{% endhighlight%}
计数属性遵循“文档出现顺序”的规则。首先遇到`Body`元素，初始化一个叫*pages*的计数器。然后遇到一个`a`元素就增加并显示*pages*计数器。

## 多个计数器

用不同的名字你就可以有多个计数器。这个例子有两个计数范围重叠的计数器，*sections*和*boxes*：

<pre class="codepen" data-height="540" data-type="result" data-href="ksFnK" data-user="lonekorean" data-safe="true"> <code> </code> <a href="http://codepen.io/lonekorean/embed/ksFnK">Check out this Pen!</a> </pre>
<script src="http://codepen.io/assets/embed/ei.js"> </script>

相关的CSS：
{% highlight css %}
    body {
        counter-reset: sections boxes;
    }
     
    section {
        counter-increment: sections;
    }
     
    section::before {
        content: 'Section ' counter(sections);
    }
     
    .box {
        counter-increment: boxes;
    }
     
    .box::before {
        content: counter(boxes, upper-roman);
    }
{% endhighlight%}
这里你可以看到用于立即初始化多个计数器的语法（第2行）。别致一点，*boxes*计数器显示为`upper-roman（译者注：大写罗马数字）`（第18行）。`display`的所有参数选项和`list-style-type`属性是一样的，这里是[文档](https://developer.mozilla.org/en-US/docs/Web/CSS/list-style-type)。

## 统计用户选择

现在我们做些有趣的事情。计数属性可以被置于像`:checked`的伪类选择器中（译者注：原文为pseudo-selectors,但一般写作pseudo-classes selectors，故照例译为伪类选择器）。这使得计数器可以通过复选框反映用户的选择。下例是统计用户选了多少项。

CSS只是少许修改前面例子中的。唯一的区别是我们在伪类选择器`(input:checked)`中自增并仅在专门的`.total`元素中显示。
{% highlight css %}
    body {
        counter-reset: characters;
    }
     
    input:checked {
        counter-increment: characters;
    }
     
    .total::after {
        content: counter(characters);
    }
{% endhighlight%}
## 控制自增

并非必须以1为量自增。可以按你增加任何的量。它们甚至可以负增长。以前面的例子为基础，这个例子为每个选择器设置了特殊的增量。

<pre class="codepen" data-height="450" data-type="result" data-href="yhtlb" data-user="lonekorean" data-safe="true"> <code> </code> <a href="http://codepen.io/lonekorean/embed/yhtlb">Check out this Pen!</a> </pre>
<script src="http://codepen.io/assets/embed/ei.js"> </script>

语法足够简单。

{% highlight css %}
    body {
        counter-reset: sum;
    }
     
    #a:checked { counter-increment: sum 64; }
    #b:checked { counter-increment: sum 16; }
    #c:checked { counter-increment: sum -32; }
    #d:checked { counter-increment: sum 128; }
    #e:checked { counter-increment: sum 4; }
    #f:checked { counter-increment: sum -8; }
     
    .sum::before {
        content: '= ' counter(sum);
    }
{% endhighlight%}
回到正题，你也可以控制计数器的初始值。
{% highlight css %}
    body {
        counter-reset: kittens 41; // starting out with 41 kittens
    }
{% endhighlight%}
## Potential Gotcha

`display:none`的元素不会触发计数。如果你想隐藏一个元素但仍然触发计数，你必须用另一种方式隐藏。这是一种方式：
{% highlight css %}
    input {
        position: absolute;
        left: -9999px;
    }
{% endhighlight%}
可能你已经注意到了，这正是我在最后两个例子中所做的。为了演示效果，我隐藏了当前的复选框，但仍然需要它们被选时而计数。

## 结束语

太好了，浏览器支持CSS计数。普大喜奔~

CSS计数器神奇，但也别忘了我们的老朋友\<ol>和\<li>。在基本列表中标序它们还是不错的。CSS计数器是取巧的方式，特别是因为它们在任何元素上都起作用，让你在语法和语义上更灵活。

*更新：我应该提及无障碍阅读。CSS计数依赖于伪类元素中生成的内容。某些屏幕的读者会阅读到，某些则不会。因此，那些关键的内容最好不要依赖伪类元素。尽管示例教学的CSS计数器是精心设计的，但生产环境中我不会原封不动地使用它们。*
