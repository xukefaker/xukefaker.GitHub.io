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

进入src>main>res，我们可以看到这里有许多文件夹，这个res文件夹存放的就是各种资源：图片，布局xml文件，还有一些其他的资源文件。并且每种资源都应该有相对应的分辨率存放于对应的文件夹中。我们现在要自定义按钮效果，进入drawable文件夹，右键new一个drawable resource file。

文件名任取，最好有实际意义。由于我们的目的是自定义**按压**效果，所以element root 应该为selector

selector为根元素，其包含了多个item元素，一个item定义了**在某些状态期间使用的可绘制对象**。也就是说，按压前是一个item，按压后是另一个item，总的合起来是一个selector。

```
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android"
    > //selector的xmlns属性必须是这个链接
    <item android:state_pressed="true">

    </item>
    <item android:state_pressed="false">

    </item>

</selector>
```

上面这段代码创建了两个item，一个是按压时的，另一个则时一般时候的。我们现在想要没按压的时候按钮是**橙色**，按压的时候**颜色变深**。另外按钮的**四个角**设计成有一点**弧度**的角，再添加一个**外边框**，颜色**淡蓝色**。

这里推荐一个颜色选择的网站[在线取色器](https://link.fobshanghai.com/rgbcolor.htm)。

在Android，要绘制一个图形，你需要声明一个shape。shape可画四种图形：rectangle， oval， line 和 ring。没有定义android:shape时，默认为rectangle。
```
    <item android:state_pressed="false">
        <shape>
        </shape>
    </item>
```

然后我们定义这个shape的填充颜色，圆角弧度，外边框的宽度和颜色

```
        <shape>
            <solid android:color="#FF9900"/>
            <corners android:radius="10dp"/>
            <stroke android:width="2dp" android:color="#00ff99"/>
        </shape>
```

这样我们就定义好了一般状态下的按钮外观。按压状态就是依葫芦画瓢了。最后的代码如下

```
    <item android:state_pressed="true">
        <shape>
            <solid android:color="#AA6600"/>
            <corners android:radius="10dp"/>
            <stroke android:width="2dp" android:color="#00ff99"/>
        </shape>
    </item>
    <item android:state_pressed="false">
        <shape>
            <solid android:color="#FF9900"/>
            <corners android:radius="10dp"/>
            <stroke android:width="2dp" android:color="#00ff99"/>
        </shape>
    </item>
```

这样我们就成功创建了一个自定义按钮，它规定了按压和没按压两种状态的外观。接下来我们就将其应用到我们的按钮中去。

根据官方文档，在xml中引用drawable资源，语法是 
`android:drawable = "@[package:]drawable/filename" `

因此，在对应的xml布局文件里的button标签中，我们写添加这一行代码
`        android:background="@drawable/bg_btn4"`。这样这个button就会应用该按压效果。

而在java文件里要引用它，语法是 `R.drawable.filename`。但是注意，和view不同，它没有所谓的findviewbyid方法直接获得drawable对象，要对drawable对象进行操作，你需要：

1 声明一个context对象 `    private Context context;`

2 实例化一个resources对象
```
private Resources resources;
    resources = context.getResources();

```

3 实例化drawable对象
```
private Drawable drawable;
    drawable = resources.getDrawable(R.drawable.filename);
```

其他类型的资源文件，也可以依葫芦画瓢来获取然后进行操作。需要注意的是，你需要在java文件里事先指明目的资源文件的类型，比如你想获得一个字符串，那么第三步里的drawable就应该换成String，其他类型同理。

## EditText

从名字上来看，EditText是可编辑文本的意思。其实就类似于javafx中的textfield，供用户输入一些文字的文本框，例如输入用户名，密码和手机号等等。

EditText类是TextView的直接子类，它的直接子类有AutoCompleteTextView 和 ExtractEditText；间接子类有MultiAutoCompleteTextView。

EditText是一个供用户输入和修饰文本的UI控件。当你声明一个EditText对象后，你必须指明它的inputType属性，它的默认值是text。

下面我们来做一个简单的用户注册界面的两个EditText：用户名和密码输入框。效果如图：
![U4sjJS.gif](https://s1.ax1x.com/2020/07/20/U4sjJS.gif)

#### 1 自定义edittext

创建一个drawable，用来定义edittext的样式。由于我们不需要任何按压变化，所以root element选择shape即可。将默认的矩形四个角改成具有一定弧度的圆角，然后边框宽度和颜色设置一下即可。
```
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <stroke android:width="2dp" android:color="#808080"/>
    <corners android:radius="10dp"/>
</shape>
```

#### 2 xml布局文件

布局采用相对布局较好。用户名引用上面自定义的drawable文件，修改一下文字颜色、大小，设置inputType为personName，灰白色提示文字“用户名”通过`        android:hint="用户名"`实现。最后人头图标，可以通过`        android:drawableLeft="@drawable/user"`显示，user文件放在drawable文件夹下面（最好新建一个drawable的子文件夹，用来专门放图标，这样方便管理）。代码如下：
```
    <EditText
        android:layout_width="match_parent"
        android:layout_height="100dp"
        android:id="@+id/et_1"
        android:textSize="40sp"
        android:textColor="#FFAD33"
        android:hint="用户名"
        android:paddingLeft="10dp"
        android:paddingRight="10dp"
        android:background="@drawable/bg_username"
        android:drawableLeft="@drawable/user"
        android:maxLines="1"
        android:inputType="textPersonName"
        />
```

密码栏和手机号栏同理。按钮参考上面介绍button的内容来设计。

#### 3 java文件里设置edittext的监听器

我们已经将整个页面做好了，现在我们想在用户输入的同时监听他输入的内容，比如将其打印到控制台上，那么这就要在java文件里进行操作了。

首先老规矩，声明控件后找到控件，这里不再赘述这一过程。

对于一个edittext对象，你可以通过addTextChangedListener()来对它设置文本改变监听器。这个方法的参数是一个textwatcher对象，它有三个方法：

beforeTextChanged(CharSequence s, int start, int count, int after)，s是修改之前的文字，start是字符串中即将发生修改的位置，count是即将被修改的文字的长度（新增文字则为0），after是被修改的文字修改之后的长度（如果是删除则为0）

onTextChanged(CharSequence s, int start, int before, int count), s是修改之后的文字，start是有变动的字符串的序号，before是被改变的字符串长度（新增则为0），count是添加的字符串长度（删除则为0）

afterTextChanged(Editable s)，s是修改之后的文字

最常用的方法是onTextChanged()。我们要实现在控制台打印用户输入的文字，只需要在这个方法里写一行这样的代码：
`              Log.d("edittext", charSequence.toString());`

Android Studio 控制台可通过点击左下角Run选项卡显示，如图：
![U4HXB6.png](https://s1.ax1x.com/2020/07/20/U4HXB6.png)

