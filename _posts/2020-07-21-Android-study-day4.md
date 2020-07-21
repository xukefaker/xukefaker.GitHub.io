---
layout: post
title: 安卓开发学习day4
date: 2020-7-21
categories: blog
tags: [学习,Android]
description: 今天学习了RadioButton，CheckBox和ImageView这三个控件
---

## RadioButton

这个控件的中文名叫做单选按钮。根据官方文档的解释，它允许用户从一组选择中选择一个选项。由于单选按钮们应该是互斥的，所以你创建的所有单选按钮都应该放在同一个RadioGroup里面，这样，同一时间只能有一个单选按钮被选中。

RadioButton是CompoundButton的子类
![UISPl8.png](https://s1.ax1x.com/2020/07/21/UISPl8.png)

下面我们来做一个这样的一组单选按钮
![UIp526.gif](https://s1.ax1x.com/2020/07/21/UIp526.gif)

### 1 在xml布局文件里创建RadioGroup

radiogroup没什么特别的属性，它就相当于一个盛放radiobutton的容器，你要规定如何盛放：水平的还是垂直的？与父级元素的边距多少？各个单选按钮之间的间距是多少？代码如下：
```
<RadioGroup
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/rg_2"
        android:layout_below="@id/rg_1"
        android:layout_marginTop="50dp"
        android:orientation="horizontal">
</RadioGroup>
```

### 2 在RadioGroup里面放RadioButton

之前说过，一组单选按钮放在同一个radiogroup里面，那么它们就是互斥的：同一时间只能由一个被选中。

Android 5自带的radiobutton样式是这样的：
![UICghR.gif](https://s1.ax1x.com/2020/07/21/UICghR.gif)

虽然不丑，但是我们很多时候想要自定义自己的RadioButton。其实和自定义Button是一样的：创建一个drawable resource文件，规定各个时期的RadioButton的外观，再引用即可。

上面的图片中RadioButton没有选中的圆圈，要消除这个圆圈，在你的RadioButton中添加这一行代码：`       android:button="@null"`

消除了圆圈，但是文字会变得向左边靠了一点的样子，要实现文字居中，定义RadioButton的gravity属性：`          android:gravity="center"`。代码如下：
```
        <RadioButton
            android:layout_width="80dp"
            android:layout_height="40dp"
            android:id="@+id/rb_3"
            android:text="男"
            android:textSize="20sp"
            android:textColor="#000"
            android:button="@null"
            android:gravity="center"
            />
```

### 3 自定义RadioButton

new一个drawable resource file，由于我们要做的是选中后状态变化的RadioButton，所以root element应该是selector。前面提过，一个selector里面有许多个item，每个item对应一个状态。这里一个状态是**选中**，一个是**未选中**。所以我们写以下代码
```
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_checked="false">
    </item>
    <item android:state_checked="true">
    </item>
</selector>
```

选中前是一个**圆角矩形**，边框为**淡蓝色**；选中后**无边框**，填充颜色变为**橙色**。
```
<item android:state_checked="false">
        <shape> //默认为rectangle，有4种shape
            <stroke android:width="2dp" android:color="#A0E7F8"/>
            <corners android:radius="10dp"/>
        </shape>
    </item>
    <item android:state_checked="true">
        <shape>
            <solid android:color="#A96D13"/>
            <corners android:radius="10dp"/>
        </shape>
    </item>
```
这样我们就完成了自定义的RadioButton，再每个RadioButton里，通过定义它的background属性来引用上面的drawable resource文件即可。
`            android:background="@drawable/selector_orange_radiobutton"`

### RadioGroup的选择事件监听器

通常，我们会想当用户更改选择时，我们要根据他的选择来做相应的事情，比如用户选择了男性，那么展示的商品就应该是男装；女性同理。要这样做，我们应该在对应的java文件里对RadioGroup设置一个选择改变监听器。
```
mRG1.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup radioGroup, int i) {

            }
        });
```
mRG1的类型是RadioGroup，通过findviewbyid方法实例化，这里不再赘述。

可以看到监听器有一个方法onCheckedChanged()，它的参数时radiogroup和i，i就是被选中的radiobutton的ID，通过这个findviewbyid方法你就可以在此方法里对选中的radiobutton进行一些操作。

## CheckBox

CheckBox的中文名叫做复选框。根据官方文档的解释，Checkboxes允许用户从一个集合里选择一个或者多个的选项。一般来说，这些选项应该垂直摆放。由于checkboxes提供复选的选项，所以这些checkboxes并没有像radiobutton绑定在一个group里，而是分别管理的。

CheckBox时CompoundButton的直接子类。
![UIjM7R.png](https://s1.ax1x.com/2020/07/21/UIjM7R.png)

CheckBox和RadioButton非常像，区别就是一个是单选一个是多选。这导致一个需要绑定在一个组里面，而另一个不需要这样做。上面RadioButton已经介绍得很详细了，这里不再做无用功。简单地说一下一个CheckBox的选择更改监听器。

#### CheckBox的选择更改监听器

RadioButton的选择更改监听器要设置在RadioGroup上，而CheckBox则是设置在它们本身上面。声明控件后找到控件，最后设置监听器的方法如下：
```
mCb7.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton compoundButton, boolean b) {

        });
```
onCheckedChanged()方法里有两个参数：compoundButton是这个checkbox本身，b代表了是否check。

## ImageView

ImageView用来显示图片资源。

根据官方文档，它是View的直接子类。它的子类有ImageButton，QuickContactBadge。
![UoSjsO.png](https://s1.ax1x.com/2020/07/21/UoSjsO.png)

它有两个主要的属性：

src，这个属性值填写图片资源的位置

scaleType，这个属性规定了图片的缩放方式，也就是图片要以何种方式来呈现。下面来实际演示以下这个属性的各个值的效果

### scaleType

这个属性能取的值有**CENTER**，**CENTER_CROP**，**CENTER_INSIDE**，**FIT_CENTER**，**FIT_END**，**FIT_START**，**FIT_XY**和**MATRIX**共8个值，默认值是FIT_CENTER，我们来一一测试一下。

我测试用的原始图片是一张随便找的网图
![UoCiKf.jpg](https://s1.ax1x.com/2020/07/21/UoCiKf.jpg)
尺寸是474 × 313 pixels的。现在我创建8个ImageView，每个ImageView的宽是150dp，高是200dp，背景颜色是#22DDDD，天蓝色。

#### 1 fitCenter
它将使得图片缩放到当前视图的大小；如果原图大于视图，则将其缩小；小图则放大；最后居中显示。效果如图：
![UonJ8U.png](https://s1.ax1x.com/2020/07/21/UonJ8U.png)

#### 2 centerCrop
根据官方文档的解释，选择这个缩放方式会均匀缩放图片（保持图像的纵横比）以使得图像的宽度和高度都等于或大于视图的相应尺寸（减去了填充）；图像在视图中是居中的。实际效果：
![UoksXt.png](https://s1.ax1x.com/2020/07/21/UoksXt.png)

可以看到图片并没有变形，但是相当于左右两边各被裁去了一部分，图片的中心部分出现在视图中。就相当于图片的中心部分开始放大，直到把控件区域填满。

#### 3 center
官方文档说这个缩放方式将会使图像在视图中居中，但不缩放。实际效果：
![UoEToT.png](https://s1.ax1x.com/2020/07/21/UoEToT.png)

可以看到图片显示在了视图的中央，没有缩放动作。但是有些地方超出了控件区域，就没有显示。

#### 4 centerInside
这个缩放方式将会统一缩放图片（保持图像的纵横比），以使图像的宽度和高度都等于或者小于视图的相应尺寸（减去填充）；图像在视图中是居中的。实际效果：
![Uoe0y9.png](https://s1.ax1x.com/2020/07/21/Uoe0y9.png)

可以看到图片完整地显示在了视图的中央，但是由于宽高比和控件的不一样，所以上下有些地方多了出来，这是图片尺寸大于控件尺寸的情况；如果小于空间尺寸，那么图片直接就居中显示在控件的中央。

#### 5 fitEnd
它的缩放方式和fitCenter一样，区别就是图像的摆放位置不一样：当图片长度大于宽度时，它居下，宽度大于长度时居右。实际效果：
![UoukL9.png](https://s1.ax1x.com/2020/07/21/UoukL9.png)

#### 6 fitStart
与fitEnd基本相同，就是图片会居上或居左（取决于宽度和高度）。实际效果：
![Uouswn.png](https://s1.ax1x.com/2020/07/21/Uouswn.png)

#### 7 fitXY
它将显示图片的全部内容，但是图片将会被拉伸以覆盖整个控件。实际效果：
![UoK31U.png](https://s1.ax1x.com/2020/07/21/UoK31U.png)

#### 8 matrix
和center类似，不对图像进行缩放。center是直接把图片居中，超过控件的部分不显示；matrix则是把图片的左上角和控件的左上角重合，超过控件的部分不显示。实际效果：
![UolD3V.png](https://s1.ax1x.com/2020/07/21/UolD3V.png)

### 显示网络图片
在GitHub上有一个伟大的项目叫做Glide，它可以让你非常方便地在一个ImageView里显示网络上的图片，要使用这项功能，你需要在你的build.gradle文件里添加：

1：
```
repositories {
    google()
    jcenter()
}
```
2:
```
dependencies {
  implementation 'com.github.bumptech.glide:glide:4.11.0'
  annotationProcessor 'com.github.bumptech.glide:compiler:4.11.0'
}
```

添加完毕后，点击同步。同步完毕后你就可以使用glide的功能了。

要在ImageView里显示网络图片，需要到java文件里进行操作。语法是：
`       Glide.with(this).load("yourPictureLink").into(yourImageView);`


