---
layout: post
title: 安卓开发学习day1
date: 2020-7-18
categories: blog
tags: [标签一,标签二]
description: 今天学习了如何开发一个简单的安卓应用
---

## 前言 

2020年7月，我选择了暑假留校，因为如果我讨厌深圳的生活--深圳白天除了呆在家里，基本上没有地方可以去，因为外面实在是太热了，太阳能把你的头皮烤化了

而呆在家里，我也没法学习。我哥生了两个小孩，一个刚上幼儿园，一个刚过百天，我则一定要承担起照顾小孩的责任

可想而知我是没办法专心发展自己的事业的。幸而我父母也支持我暑假留校，认为我自己的事情更重要一些

言归正传，这次暑假项目我和我的同学们打算一起开发一个安卓应用，大致就是做成一个发布博客文章的应用。

由于我并没有接触过app的开发，所以我要从头开始学Android开发。严格来说我已经学了一点java基础，所以能够比较好地上路。

## IDE的安装

要进行开发活动，IDE肯定是必不可少的。以前的人们大多是用eclipse来开发移动应用，而谷歌2014年发布了Android studio 稳定版，从此程序员们开始逐渐地使用谷歌自己
发布的这款IDE了。

Android studio 是基于JetBrains IntelliJ IDEA的(一款非常好用的Java IDE)。对于我来说，想要彻底搞懂Android studio都是一个不小的挑战。不过好在
它安装起来并不像我初学java安装eclipse那样困难，基本上没什么大问题，和平时用的应用软件一样的傻瓜式安装

[Android studio下载](https://developer.android.com/studio)
[百度网盘下载 提取码gr17](https://pan.baidu.com/s/12jpBLXFpKalTUh5ahbkU2Q)

### 一个最简单的项目：hello world!

安装好了IDE，我们就可以点击左上角的file来new一个project。我是按照b站的教程来的。hello world项目只需要empty activity即可

点击next，名字自己取，包名会随之改变。注意SDK最好选择安卓5以上的版本，因为它占据了绝大部分市场。点击finish即可完成创建

一开始用这个创建一个项目真的是吓我一跳--怎么会有这么多生成的东西？这哪个是干嘛的啊？我要搞一个显示hello world的页面应该写在哪里？

先搞懂系统给我自动生成的东西吧。

先切换一下explorer的显示方式：点击左上角android>切换成project，这样我们就能看到项目的完整文件夹。

进入yourProjectName>app>src>main>java>com.example.yourprojectname，系统自动给我们生成的MainActivity.java就在这里。

这是干啥的？很容易理解，我们使用某个应用，肯定会有某个页面作为我们一开始看到的、交互的页面，这个就是主页面（在这里我们叫做主活动）。

相应的其他页面的java文件也会在这里。

#### MainActivity.java文件

生成的MainActivity继承自AppCompatActivity

实际上Activity类才是最基本的类，之后在v4包中FragmentActivity被引入了，它继承自Activity，AppCompatActivity继承自FragmentActivity

我们的hello world并不是在java文件写输出打印的，而是要在专门的xml布局文件中写。每个activity都有其对应的java文件和xml文件。在这里，
我们可以看到这一行代码
`setContentView(R.layout.activity_main);`
activity_main就是xml布局文件。它位于 res>layout，双击进去看一下

右上角有显示方式，我们选择split更方便进行调试

#### activity_main.xml文件
android开发的布局和html很像，就是一些名字不一样。都能采用嵌套的方式来部置控件

在开发界面，右上角有显示代码和界面的三种方式，我一般选择split，这样方便进行调试

实际上系统一开始就已经帮我们生成了hello world的代码：` android:text="Hello World!"`，这行代码让textview显示了hello world这个字符串

#### AndroidManifest.xml文件

在main文件夹里面我们可以看到还有一个AndroidManifest.xml文件，双击查看代码。

可以看到有一下一段代码

```
<activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```

就是它让MainActivity成为主活动的，如果你想更改主活动，直接更改文件名即可

到这里我们就大致了解了如何创建一个简单的Android开发项目了。Android studio为我们准备了许多种不同分辨率的手机模拟器（需要另外下载文件）。安装好模拟器后，你的app就可以直接在模拟器上运行了。
你也可以选择在自己手机上运行。







