---
layout: post
title: 安卓开发学习day2
date: 2020-7-19
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

xml文件中的textview等控件在java不直接可用，如果你需要在java文件里对这些控件进行进一步设置（如设置事件监听器等），你需要给它取一个id

如我想对某个textview设置个下划线。先

```
<TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/tv_5"
        android:text="I love Java"
        android:textSize="24sp"
        android:textColor="#000000"
        />
```

以上代码设置了一些常规属性。接着我们去对应的java文件里找到这个textview
`    private TextView mTv5;`
这行代码先声明了mTv5这个textview，接着在onCreate方法里实例化这个textview

`        mTv5 = findViewById(R.id.tv_5);`

这行代码调用了findviewbyid方法找到了activity_textview.xml文件里名为tv_5的这个控件，接着

`        mTv5.getPaint().setFlags(Paint.UNDERLINE_TEXT_FLAG);`

这行代码为这个控件设置了下划线。

上面这行代码也可替换为：

`        mTv6.setText(Html.fromHtml("<u>I love Java</u>"));`

这是直接在java文件里写html代码来实现下划线效果。

#### 小玩意：文字走马灯

文字走马灯就是让一行文字不停地从右向左移动。

先写一个textview，并设置一些常规属性

```
<TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/tv_7"
        android:text="I love Java I love Java I love Java I love Java I love Java "
        android:textSize="24sp"
        android:textColor="#000000"
        />
```

注意文字要尽量多一点，不然单屏可以直接显示所有文字，那么文字也就不会动起来了。

`      android:singleLine="true"`，规定单行显示，不然文字会挤到下一行去

```   
    android:ellipsize="marquee"
    android:marqueeRepeatLimit="marquee_forever"
```

ellipsize属性是当文字长度超过textview的宽度时，文字该如何显示。这里设置成marquee，即横向滚动方式显示，也就是走马灯效果；下面一行代码设置成无限次循环滚动。

但是这样还不行，上述效果还要求textview获得焦点才能实现，为此我们需要添加以下两行代码：

```
        android:focusable="true"
        android:focusableInTouchMode="true"
```

这两行代码让textview可以被聚焦。接着我们需要到java文件里让这个textview一直被选中

老规矩，先声明控件，再找到控件，再设置被选中。

```
    private TextView mTv7;
        mTv7 = findViewById(R.id.tv_7);
        mTv7.setSelected(true);
```

文字走马灯就实现了。效果如下：

![UfFemQ.gif](https://s1.ax1x.com/2020/07/19/UfFemQ.gif)

