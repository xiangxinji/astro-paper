---
title: Flutter 布局
author: Andy
pubDatetime: 2023-02-18T03:42:51Z
featured: false
draft: false
tags:
  - Flutter
  - 移动端
description:
  "Flutter  中的布局概念"
---

Flutter 布局的核心机制是 widgets。在 Flutter 中，几乎所有东西都是 widget —— 甚至布局模型都是 widgets。你在 Flutter 应用程序中看到的图像，图标和文本都是 widgets。此外不能直接看到的也是 widgets，例如用来排列、限制和对齐可见 widgets 的行、列和网格。

你可以通过组合 widgets 来构建更复杂的 widgets 来创建布局。比如，下面第一个截图上有 3 个图标，每个图标下面都有一个标签：



![](/assets/images/flutter/5.png)


## 布局 widget

1. 选择一个布局 Widget ，具体可以从 [LayoutWidgets](https://flutter.cn/docs/development/ui/widgets/layout) 进行查找

2. 创建一个可见的 Widget ， 如 Icon , Text ，Image 等等

3. 将 可见的 Widget 添加至布局 Widget

4. 将 布局 Widget 添加至页面

如下图

```dart 
 @override
  Widget build(BuildContext context) {
    return Column(children: [ // 使用了 Column 布局 ， 放置了两个元素
      ElevatedButton(
        onPressed: () {
          increment();
        },
        child: const Text('Increment'),
      ),
      Text(' count $_count')
    ]);
  }
```


## 横向或纵向布局多个 widgets

最常见的布局模式之一是垂直或水平 widgets。你可以使用 Row widget 水平排列 widgets，使用 Column widget 垂直排列 widgets。

Row 和 Column 是水平和垂直布局的基本原始 widgets —— 这些低级 widgets 允许最大程度的自定义。 Flutter 还提供专门的、更高级别的 widgets，可能可以直接满足需求。例如，和 Row 相比你可能更喜欢 ListTile，这是一个易于使用的 widget，有属性可以设置头尾图标，最多可以显示 3 行文本；和 Column 相比你也可能更喜欢 ListView，这是一种类似于列的布局，但如果其内容太长导致可用空间不够容纳时会自动滚动。更多信息可以查看 [通用布局 widgets](https://flutter.cn/docs/development/ui/layout#common-layout-widgets)。



### 对齐 widgets 

你可以使用 mainAxisAlignment 和 crossAxisAlignment 属性控制行或列如何对齐其子项。对于一行来说，主轴水平延伸，交叉轴垂直延伸。对于一列来说，主轴垂直延伸，交叉轴水平延伸。

![](/assets/images/flutter/6.png)

与 CSS 中的flex 一致， 唯一的不同区别只是 Row 的主轴就是横向， 次轴是纵向。 Column 的主轴是纵向 ， 次轴是横向。 然后通过上方的两个方法去更改主次轴的排列顺序

```dart
 @override
  Widget build(BuildContext context) {
    // mainAxisAlignment: MainAxisAlignment.spaceAround
    return Column(mainAxisAlignment: MainAxisAlignment.spaceAround, children: [
      ElevatedButton(
        onPressed: () {
          increment();
        },
        child: const Text('Increment'),
      ),
      Text(' count $_count')
    ]);
  }
```


![](/assets/images/flutter/7.png)


## 调整 widgets 大小

1. Expanded ： 相当于 Web 前端中的 flex :1 ， 标记着这个元素的宽高来进行自动分配

也许你想要一个 widget 占用的空间是兄弟项的两倍。为了达到这个效果，可以使用 Expanded widget 的 flex 属性，这是一个用来确定 widget 的弹性系数的整数。默认的弹性系数为 1，以下代码将中间图像的弹性系数设置为 2：

```dart

Row(
  crossAxisAlignment: CrossAxisAlignment.center,
  children: [
    Expanded(
      child: Image.asset('images/pic1.jpg'),
    ),
    // 中间会为两倍宽
    Expanded(
      flex: 2,
      child: Image.asset('images/pic2.jpg'),
    ),
    Expanded(
      child: Image.asset('images/pic3.jpg'),
    ),
  ],
);
```


### 组合 widgets

默认情况下，行或列沿其主轴会占用尽可能多的空间，但如果要将子项紧密组合在一起，请将其 mainAxisSize 设置为 MainAxisSize.min。以下示例使用此属性将星形图标组合在一起。

```dart
  @override
  Widget build(BuildContext context) {
    return Column(mainAxisSize: MainAxisSize.min, children: [
      ElevatedButton(
        onPressed: () {
          increment();
        },
        child: const Text('Increment'),
      ),
      Text(' count $_count')
    ]);
  }
```


## 通用布局 Widgets 

### 标准 Widgets

1. Container：向 widget 增加 padding、margins、borders、background color 或者其他的“装饰”。

2. GridView：将 widget 展示为一个可滚动的网格。

3. ListView：将 widget 展示为一个可滚动的列表。

4. Stack：将 widget 覆盖在另一个的上面。

### Material widgets
1. Card：将相关信息整理到一个有圆角和阴影的盒子中。

2. ListTile：将最多三行的文本、可选的导语以及后面的图标组织在一行中。


