---
layout: post
title: 安卓开发学习day3
date: 2020-7-20
categories: blog
tags: [学习,Android]
description: 今天学习了Button和EditText这两个控件
---

## Button

昨天提到，button类是textview的直接子类，所以textview有的属性button也会有，比如
text, textcolor, textsize等等。

它的直接子类有CompoundButton, 间接子类有CheckBox, RadioButton, Switch, ToggleButton

Button可以理解为一种经过修饰的textview：它有其默认的样式。一个button最重要的功能就是用户点击后处理一个这个点击事件。

下面我们来创建一个button，它在被点击之后，手机屏幕会出现一个**短暂的提示消息**，内容是**That's right**。

#### 1

在对应的xml文件里创建一个button
```
    <Button
        android:layout_width="match_parent"
        android:layout_height="40dp"
        android:id="@+id/aBtn_3"
        android:text="按钮3"
        android:textColor="#000000"
        android:textSize="20sp"
        android:background="#FF0000"
        />
```

#### 2

在java文件里找到这个button。先声明这个button`    private Button mBtn3;`

再在onCreate方法里调用findviewbyid方法，通过id找到这个button
`        mBtn3 = findViewById(R.id.aBtn_3);`

#### 3

最后给它设置点击监听器
```
        mBtn3.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(ButtonActivity.this, "That's right", Toast.LENGTH_SHORT).show();
            }
        });
```

Toast.makeText方法会产生一个提示消息，方法里第一个参数指明在哪个activity弹出消息，第二个参数为消息内容，最后一个参数是消息持续的时间。最后不要忘记加个show()。

效果如下：
![Ufxmss.gif](https://s1.ax1x.com/2020/07/20/Ufxmss.gif)

请注意，我们也能给textview设置一个点击监听器，从而处理一些它的点击事件。这并不是button独有的。

### 如何自定义button点击效果

Android 5 自带的按钮点击按压效果不算丑，但是我们想要自己自定义一个点击按压效果，该如何做？
[![UfzN9S.gif](https://s1.ax1x.com/2020/07/20/UfzN9S.gif)](https://imgchr.com/i/UfzN9S)

html5 里是通过hover来实现的，而在Android里，则是通过drawable画出控件的形状再引用drawable来实现的。

