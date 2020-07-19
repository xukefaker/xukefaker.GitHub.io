---
layout: post
title: 安卓开发学习day1
date: 2020-7-18
categories: blog
tags: [学习,Android]
description: 今天学习了Textview和Button这两个控件，以及两种布局方式：LinearLayout和RelativeLayout
---

## LinearLayout

顾名思义，线性布局，就是一切控件都按照某个方向来摆放：水平or垂直。

在声明一个LinearLayout后，你必须给它三个属性赋值：layout_width, layout_height, 和orientation（取值都有提示）。不一定要给它取id
```
<LinearLayout ...>

控件1
控件2
控件3
...

</LinearLayout>
```

控件123...都将按照线性布局来排列。你可以通过padding来对所有控件设置边距，也可以通过设置单个控件的margin来指定其与其他控件的边距

## RelativeLayout
顾名思义，相对布局，就是一切控件都相对与**谁**在**哪里**。

相对布局没有方向，所有控件默认在父级元素的正上方。

一些常用的控件布局设置：toRightOf, toStartOf, above, below。

### 一个小问题--选择哪个单位？

在padding的数值选项，我们可以看到有许多种单位可供选择：dp, sp, in, mm, pt, px...

目前我只知道padding和margin最好使用dp为单位，而textsize则推荐使用sp。那么为什么这样推荐？其他几个单位在什么时候使用？

来日进一步学习我才能解决这个问题，现在先mark一下

## TextView

TextView继承自View类，View继承自Object类。

它的直接子类有：Button, CheckedTextView, Chronometer, DigitalClock, EditText, TextClock

关系如下：

![Uf9Frd.png](https://s1.ax1x.com/2020/07/19/Uf9Frd.png)