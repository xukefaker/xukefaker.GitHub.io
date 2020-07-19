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

控件123...都将按照线性布局来排列。你可以通过padding来对所有控件设置边距
