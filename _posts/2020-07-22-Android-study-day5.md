---
layout: post
title: 安卓开发学习day5
date: 2020-7-22
categories: blog
tags: [学习,Android]
description: 今天学习了ListView这个控件
---

## ListView

listview中文名叫做列表视图。它垂直地显示可滚动的视图集合，其中每个视图（view）都直接位于上一个视图的下方。如图：
![UHKwz6.png](https://s1.ax1x.com/2020/07/22/UHKwz6.png)

listview是AbsListView的子类，它的子类有ExpandableListView。
![UHKhSP.png](https://s1.ax1x.com/2020/07/22/UHKhSP.png)

下面我们来实现一个自定义的列表视图，效果如图：
![UHQlbF.gif](https://s1.ax1x.com/2020/07/22/UHQlbF.gif)

#### 1 创建一个view
listview会垂直显示许多个view，我们先来定义一个基本的view。在layout文件夹里新建一个xml文件。布局方式任选，我采用的是线性布局。左边一个imageview，右边一个linearlayout，里面三个textview，具体代码比较简单，读者任选属性设置即可，最终单个view的效果如图：
![UHG7jI.png](https://s1.ax1x.com/2020/07/22/UHG7jI.png)

#### 2 创建listviewAdapter
listview无法直接设置其中的内容和风格，它需要根据listviewadapter的要求来展示内容。因此我们要创建一个listviewadapter，它继承自BaseAdapter
```
public class MyListViewAdapter extends BaseAdapter {
    @Override
    public int getCount() {
        return 10;
    }
    @Override
    public Object getItem(int i) {
        return null;
    }
    @Override
    public long getItemId(int i) {
        return 0;
    }
    @Override
    public View getView(int i, View view, ViewGroup viewGroup) {
    }
}
```
getCount：要绑定的条目的数量

getItem：根据一个索引获得该位置的对象

getItemId：获得条目的id

getView：获得该条目要显示的界面。i指明第几个条目，view是旧视图，viewGroup是父级视图。

简单来说，adapter会根据getCount循环执行getView将条目一个一个绘制出来。因此重要的是这两个方法，其余两个暂时不用管。

要将我们之前写好的view在java代码里实例化，我们需要用到LayoutInflater。它的inflate方法可以将我们的view绘制出来。同时LayoutInflater实例化要用到context。下面推荐一种高效的写法
```
    private Context mContext;
    private LayoutInflater mLayoutInflater;
    MyListViewAdapter(Context context){
        this.mContext = context;
        mLayoutInflater = LayoutInflater.from(context);
    }//adapter的私有变量和构造函数
```
```
static class ViewHolder{
        public ImageView imageView;
        public TextView tvTitle, tvTime, tvContent;
    }//用来标记控件的类，作用后面解释
```
```
public View getView(int i, View view, ViewGroup viewGroup) {
        ViewHolder holder = null;
        if (view == null){
            view = mLayoutInflater.inflate(R.layout.layout_list_item, null);
            holder = new ViewHolder();
            holder.imageView = view.findViewById(R.id.iv);
            holder.tvTitle = view.findViewById(R.id.tv_title);
            holder.tvTime = view.findViewById(R.id.tv_time);
            holder.tvContent = view.findViewById(R.id.tv_content);
            view.setTag(holder);
        }else {
            holder = (ViewHolder) view.getTag();
        }
        //给控件赋值
        holder.tvTitle.setText("这是标题");
        holder.tvTime.setText(Calendar.getInstance().getTime() +"");
        holder.tvContent.setText("2020年要永远努力");

        return view;
    }
```
这是一种优化了的写法。当我们滑动listview时，如果有的条目被滑上去了或者滑下去了，它会被隐藏并保存到getView的view里。因此我们先判断**第i个条目**是否为空，如果不为空，我们把它取出来并重新绘制即可。怎么取出来？这就要求你在第一次绘制的时候就把它保存好。

在这里我们可以看到如果view为空，那么就通过LayoutInflater的inflate方法绘制一个view，然后把view的控件传给viewholder，然后通过setTag方法保存起来。如果view不为空，那么直接通过getTag方法取出控件即可，节省了很多内存。

最后设置一下view的内容后返回view即可。

**参考**[BaseAdapter使用教程](https://www.jianshu.com/p/24833a2cffd1)

#### 3 创建listview

这里就和常规的控件一样了，创建一个Activity，然后在xml文件里创建listview，在java文件里设置adapter，语法：
`        mLv1.setAdapter(new MyListViewAdapter(ListViewActivity.this));`
我们写的Adapter的构造函数需要一个context参数，而一个activity属于是context的子类，因此直接传进去即可。

要个性化条目的按压效果，先编写一个drawable文件，规定各种状态的外观。我的代码如下：
```
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">

    <item android:state_pressed="true" android:drawable="@color/colorSoftBlue"/>
    <item android:state_selected="false" android:drawable="@color/colorGrey"/>

</selector>
```
然后在你的listview里，通过设置listSelector属性引用该drawable resource
`        android:listSelector="@drawable/list_item"`

#### listview的一些点击事件监听器
**点击后显示点击的条目的位置**
```
        mLv1.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> adapterView, View view, int i, long l) {
                Toast.makeText(ListViewActivity.this, "pos:" + i, Toast.LENGTH_SHORT).show();
            }
        });
```

**长按后显示长按的位置**
```
        mLv1.setOnItemLongClickListener(new AdapterView.OnItemLongClickListener() {
            @Override
            public boolean onItemLongClick(AdapterView<?> adapterView, View view, int i, long l) {
                Toast.makeText(ListViewActivity.this, "长按 pos:" + i, Toast.LENGTH_SHORT).show();
                return true;
            }
        });
```

### 问题：为什么一定要context？
在很多地方我都能看到context的存在，而许多时候往往Activity.this即可作为context使用，这到底有啥用？
