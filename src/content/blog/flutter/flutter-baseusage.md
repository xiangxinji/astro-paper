---
title: flutter 基础使用
author: Andy
pubDatetime: 2023-02-18T03:42:51Z
featured: false
draft: false
tags:
  - Flutter
  - 移动端
description:
  "跨平台 dart 语言框架"
---




## 了解什么是 Widgets 

官方介绍


 >  Flutter 从 React 中吸取灵感，通过现代化框架创建出精美的组件。它的核心思想是用 widget 来构建你的 UI 界面。 Widget 描述了在当前的配置和状态下视图所应该呈现的样子。当 widget 的状态改变时，它会重新构建其描述（展示的 UI），框架则会对比前后变化的不同，以确定底层渲染树从一个状态转换到下一个状态所需的最小更改。


## 创建一个 hello-world 

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    const Center( // 使用 Center 布局 ， child 规定传入节点。
      child: Text( // 文本节点
        'Hello, world!'
      ),
    ),
  );
}
```

展示效果如下： 
![](/assets/images/flutter/1.png)



也就是说在 flutter 中的 widgets 能够帮我们完成对应的视图功能， 类似于 web 前端中的 html 和 css 的结合。  

## 基础 Widgets 

Flutter 自带了一套强大的基础 widgets，下面列出了一些常用的：

1. Text ：渲染文本

2. Row , Column : 这两个 flex widgets 可以让你在水平 (Row) 和垂直(Column) 方向创建灵活的布局。它是基于 web 的 flexbox 布局模型设计的。

3. Stack : Stack widget 不是线性（水平或垂直）定位的，而是按照绘制顺序将 widget 堆叠在一起。你可以用 Positioned widget 作为Stack 的子 widget，以相对于 Stack 的上，右，下，左来定位它们。 Stack 是基于 Web 中的绝对位置布局模型设计的。

4. Container : Container widget 可以用来创建一个可见的矩形元素。 Container 可以使用 BoxDecoration 来进行装饰，如背景，边框，或阴影等。 Container 还可以设置外边距、内边距和尺寸的约束条件等。另外，Container可以使用矩阵在三维空间进行转换。

5. Expanded : 填充剩余空间的容器


基于基础 Widgets 绘制一个 50 * 50 的红色区域， 其中包含文本

```dart
class MyFirstWidget extends StatelessWidget {
  const MyFirstWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return Center( // 采用居中布局
        child: Container( // 使用 Container 绘制一个可见的矩形元素。 并且使用 decoration 进行修饰
      decoration: const BoxDecoration(color: Colors.red),
      width: 50,
      height: 50,
      // child 使用 Column 来进行排列， 也就是纵向排列
      child: Column(children: const [
        Text(
          'zhangsan2',
          textDirection: TextDirection.ltr,
        )
      ]),
    ));
  }
}
```

效果如下 ： 

![](/assets/images/flutter/2.png)




## 使用 Material 组件 

请确认在 pubspec.yaml 文件中 flutter 部分有 uses-material-design: true 这条，它能让你使用预置的 Material icons。

```yaml
name: my_app
flutter:
  uses-material-design: true
```
为了获得(MaterialApp)主题的数据，许多 Material Design 的 widget 需要在 MaterialApp 中才能显现正常。因此，请使用 MaterialApp 运行应用。

> Flutter 提供了许多 widget，可帮助你构建遵循 Material Design 的应用。 Material 应用以 MaterialApp widget 开始，它在你的应用的底层下构建了许多有用的 widget。这其中包括 Navigator，它管理由字符串标识的 widget 栈，也称为“routes”。 Navigator 可以让你在应用的页面中平滑的切换。使用 MaterialApp widget 不是必须的，但这是一个很好的做法。


```dart 
import 'package:flutter/material.dart';

void main() {
  runApp(
    const MaterialApp(
      title: 'Flutter Tutorial',
      home: TutorialHome(),
    ),
  );
}

class TutorialHome extends StatelessWidget {
  const TutorialHome({super.key});

  @override
  Widget build(BuildContext context) {
    // Scaffold is a layout for
    // the major Material Components.
    return Scaffold(
      appBar: AppBar(
        leading: const IconButton(
          icon: Icon(Icons.menu),
          tooltip: 'Navigation menu',
          onPressed: null,
        ),
        title: const Text('Example title'),
        actions: const [
          IconButton(
            icon: Icon(Icons.search),
            tooltip: 'Search',
            onPressed: null,
          ),
        ],
      ),
      // body is the majority of the screen.
      body: const Center(
        child: Text('Hello, world!'),
      ),
      floatingActionButton: const FloatingActionButton(
        tooltip: 'Add', // used by assistive technologies
        onPressed: null,
        child: Icon(Icons.add),
      ),
    );
  }
}
```

![](/assets/images/flutter/3.png)



提示

Material 是 Flutter 中两个自带的设计之一，如果想要以 iOS 为主的设计，可以参考 [Cupertino components](https://flutter.cn/docs/development/ui/widgets/cupertino)，它有自己版本的 CupertinoApp 和 CupertinoNavigationBar。


## 处理手势
大多数应用都需要通过系统来处理一些用户交互。构建交互式应用程序的第一步是检测输入手势，这里通过创建一个简单的按钮来了解其工作原理：

> GestureDetector widget 没有可视化的展现，但它能识别用户的手势。当用户点击 Container 时， GestureDetector 会调用其 onTap() 回调，在这里会向控制台打印一条消息。你可以使用 GestureDetector 检测各种输入的手势，包括点击，拖动和缩放。

许多 widget 使用 GestureDetector 为其他 widget 提供可选的回调。例如，IconButton、ElevatedButton 和 FloatingActionButton widget 都有 onPressed() 回调，当用户点击 widget 时就会触发这些回调。

```dart 
import 'package:flutter/material.dart';

class MyButton extends StatelessWidget {
  const MyButton({super.key});

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        print('MyButton was tapped!');
      },
      child: Container(
        height: 50.0,
        padding: const EdgeInsets.all(8.0),
        margin: const EdgeInsets.symmetric(horizontal: 8.0),
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(5.0),
          color: Colors.lightGreen[500],
        ),
        child: const Center(
          child: Text('Engage'),
        ),
      ),
    );
  }
}

void main() {
  runApp(
    const MaterialApp(
      home: Scaffold(
        body: Center(
          child: MyButton(),
        ),
      ),
    ),
  );
}
```


## 有状态的 Widget

在上方使用的实例全部都是静态 Widget , 也就是没有状态可以操控 （StatelessWidget） ， 在前端中最重要的莫过于视图和数据层的交互， 在 Flutter 中 当视图和数据层交互并且让视图更新， 我们需要使用  （StatefulWidget）



