## Flutter - Notes

### 环境配置
```bash
git clone -b beta https://github.com/flutter/flutter.git

# Flutter ~/.zshrc 增加
export PATH=$HOME/flutter/bin:$PATH

# Flutter CN ~/.zshrc 配置源为国内
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn

# which
which flutter && flutter doctor
```

### Dart语言下的Flutter

* 基本类型  

    var 可以定义变量, 同时 Dart 属于`动态类型`语言，支持闭包。  

    number 分为 int，double。 Dart 中`没有 float 类型`。  

    Dart 下`只有 bool 型可`以用于 if 等判断。  

    DART中，switch 支持 String 类型。

* 变量

    Dart 不需要给变量设置 setter getter 方法， 这和 kotlin 等类似。Dart 中所有的基础类型、类等都继承 Object ，默认值是 NULL， 自带 getter 和 setter ，而`如果是 final 或者 const 的话`，那么它只有一个 `getter` 方法。  

    Dart 中 final 和 const 表示常量，比如 final name = 'GSY'; const value= 1000000; 同时 static const 组合代表了`静态常量`。其中 const 的值在编译期确定，final 的值要到编译时才确定。  

    Dart 下的数值，在作为`字符串`使用时，是需要`显式指定`的。比如：int i = 0; print("aaaa" + i); 这样并不支持，需要 print("aaaa" + i.toString()); 这样使用。所以**在使用动态类型时，需要注意不要把 number 类型当做 String 使用**。  

    Dart 中`数组等于列表`，所以 var list = []; 和 List list = new List() 可以简单看做一样。

* 方法

    Dart 下 `?? 、??=` 属于`操作符`，如: AA ?? "999" 表示如果 AA 为空，返回99；AA ??= "999" 表示如果 AA 为空，给 AA 设置成 99。  

    Dart 方法可以设置 参数默认值 和 指定名称 。比如： getDetail(Sting userName, reposName, {branch = "master"}){} 方法，这里 branch 不设置的话，默认是 “master” 。参数类型 可以指定或者不指定。调用效果： getRepositoryDetailDao(“aaa", "bbbb", branch: "dev");  

    Dart 没有关键词 public 、private 等修饰符, _下横向直接代表 private ，但是有 @protected 注解。  

    Dart 中多构造函数，可以通过如下代码实现的。默认构造方法只能有一个，而通过Model.empty() 方法可以创建一个空参数的类，其实方法名称随你喜欢。而变量初始化值时，只需要通过 this.name 在构造方法中指定即可：
    ```dart
    class ModelA {
        String name;
        String tag;
        
        //默认构造方法，赋值给name和tag
        ModelA(this.name, this.tag);

        //返回一个空的ModelA
        ModelA.empty();
        
        //返回一个设置了name的ModelA
        ModelA.forName(this.name);
    }
    ```

* Flutter

    Flutter 中支持 async/await 。

    ```dart
    // 模拟等待两秒，返回OK
    request() async {
        await Future.delayed(Duration(seconds: 1));
        return "ok!";
    }

    // 得到"ok!"后，将"ok!"修改为"ok from request"
    doSomeThing() async {
        String data = await request();
        data = "ok from request";
        return data;
    }

    // 打印结果
    renderSome() {
        doSomeThing().then((value) {
        print(value);
        // 输出ok from request
        });
    }
    ```

    Flutter 中 `setState` 很有 React Native 的既视感，Flutter 中也是通过 state 跨帧实现管理数据状态的。

### Flutter Widget

>&#160; &#160;在 Flutter 中，一切的显示都是 Widget 。**Widget 是一切的基础**，作为响应式的渲染，属于 MVVM 的实现机制。我们可以通过修改数据，再用setState 设置数据，Flutter 会**自动通过绑定的数据更新 Widget** 。 **所以你需要做的就是实现 Widget 界面，并且和数据绑定起来**。  

> &#160; &#160;Widget 分为 有状态 和 无状态 两种，在 Flutter 中每个页面都是一帧。无状态就是保持在那一帧。而有状态的 Widget 当数据更新时，其实是绘制了新的 Widget，只是 State 实现了跨帧的数据同步保存。

>&#160; &#160;当代码框里输入 stl 的时候，可以自动弹出创建无状态控件的模板选项，而输入 stf 的时，就会弹出创建有状态 Widget 的模板选项。代码格式化的时候，括号内外的逗号都会影响格式化时换行的位置。  

### 无状态StatelessWidget

> **继承 StatelessWidget，通过 build 方法返回一个布局好的控件。**  

> &#160; &#160; Widget 和 Widget 之间通过 child: 进行嵌套。其中有的 Widget 只能有一个 child，比如下方的 Container ；有的 Widget 可以多个 child ，也就是children:，比如` Colum 布局。

代码便是 Container Widget 嵌套了 Text Widget。

```dart
import 'package:flutter/material.dart';

class DEMOWidget extends StatelessWidget {
  final String text;

  // 数据可以通过构造方法传递进来
  DEMOWidget(this.text);

  @override
  Widget build(BuildContext context) {
    // 这里返回你需要的控件
    // 这里末尾有没有的逗号，对于格式化代码而已是不一样的。
    return Container(
      // 白色背景
      color: Colors.white,
      // Dart语法中，?? 表示如果text为空，就返回尾号后的内容。
      child: Text(text ?? "这就是无状态DMEO"),
    );
  }
}
```

### 有状态StatefulWidget

> &#160;&#160; 你需要创建管理的是主要是 State ， 通过 State 的 build 方法去构建控件。在 State 中，你可以动态改变数据，这类似 MVVM 实现，在 setState 之后，改变的数据会触发 Widget 重新构建刷新。而下方代码中，是通过延两秒之后，让文本显示为 "这就变了数值"。

State 中主要的声明周期有  
* **initState** ：初始化，理论上只有初始化一次，第二篇中会说特殊情况下。  
* **didChangeDependencies** ：在 initState 之后调用，此时可以获取其他 State 。  
* **dispose** : 销毁，只会调用一次。

>&#160;&#160; 关注点只要在：创建你的 StatelessWidget 或者 StatefulWidget 而已。你需要的就是在 build 中堆积你的布局，然后把数据添加到 Widget 中，最后通过 setState 改变数据，从而实现画面变化。

```dart 
import 'dart:async';
import 'package:flutter/material.dart';

class DemoStateWidget extends StatefulWidget {
  final String text;

  // 通过构造方法传值
  DemoStateWidget(this.text);

  // 主要是负责创建state
  @override
  _DemoStateWidgetState createState() => _DemoStateWidgetState(text);
}

class _DemoStateWidgetState extends State<DemoStateWidget> {
  String text;

  _DemoStateWidgetState(this.text);
  
  @override
  void initState() {
    // 初始化，这个函数在生命周期中只调用一次
    super.initState();
    // 定时2秒
    new Future.delayed(const Duration(seconds: 1), () {
      setState(() {
        text = "这就变了数值";
      });
    });
  }

  @override
  void dispose() {
    ///销毁
    super.dispose();
  }

  @override
  void didChangeDependencies() {
    // 在initState之后调 Called when a dependency of this [State] object changes.
    super.didChangeDependencies();
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      child: Text(text ?? "这就是有状态DMEO"),
    );
  }
}
```

### Flutter 布局

> Flutter 中拥有需要将近30种内置的 布局Widget，其中常用有 Container、Padding、Center、Flex、Stack、Row、Colum、ListView 等。

|类型|作用特点|
|:---:|:-----:| 
|Container|只有一个子 Widget。默认充满，包含了padding、margin、color、宽高、decoration 等配置。|
|Padding|只有一个子 Widget。只用于设置Padding，常用于嵌套child，给child设置padding。|
|Center|只有一个子 Widget。只用于居中显示，常用于嵌套child，给child设置居中。|
|Stack|可以有多个子 Widget。 子Widget堆叠在一起。|
|Colum|可以有多个子 Widget。垂直布局。|
|Row|可以有多个子 Widget。水平布局。|
|Expanded|只有一个子 Widget。在 Colum 和 Row 中充满。|
|ListView|可以有多个子 Widget。|

> Container ：最常用的默认布局！只能包含一个child:，支持配置 padding,margin,color,width,height,decoration（一般配置边框和阴影）等配置，在 Flutter 中，不是所有的控件都有 width、height、padding、margin、color 等属性，所以才会有 Padding、Center 等 Widget 的存在。

```dart
new Container(
    // 四周10大小的maring
    margin: EdgeInsets.all(10.0),
    height: 120.0,
    width: 500.0,
    // 透明黑色遮罩
    decoration: new BoxDecoration(
        // 弧度为4.0
        borderRadius: BorderRadius.all(Radius.circular(4.0)),
        // 设置了decoration的color，就不能设置Container的color。
        color: Colors.black,
        // 边框
        border: new Border.all(color: Color(GSYColors.subTextColor), width: 0.3)
    ),
    child:new Text("Container")
);
```

> Colum、Row 绝对是必备布局， 横竖布局也是日常中最常见的场景。它们常用的有这些属性配置：主轴方向是 start 或 center 等；副轴方向方向是 start 或 center 等；mainAxisSize 是充满最大尺寸，或者只根据子 Widget 显示最小尺寸。

```dart
//主轴方向，Colum的竖向、Row我的横向
mainAxisAlignment: MainAxisAlignment.start, 
//默认是最大充满、还是根据child显示最小大小
mainAxisSize: MainAxisSize.max,
//副轴方向，Colum的横向、Row我的竖向
crossAxisAlignment :CrossAxisAlignment.center,
```

> Expanded 在 Colum 和 Row 中代表着平均充满，当有两个存在的时候默认均分充满。同时页可以设置 flex 属性决定比例。

```dart
new Column(
    // 主轴居中,即是竖直向居中
    mainAxisAlignment: MainAxisAlignment.center,
    // 大小按照最小显示
    mainAxisSize : MainAxisSize.min,
    // 横向也居中
    crossAxisAlignment : CrossAxisAlignment.center,
    children: <Widget>[
    // flex默认为1
    new Expanded(child: new Text("1111"), flex: 2,),
    new Expanded(child: new Text("2222")),
    ],
);
```

> 实现一个居中的Icon图标和文本，中间间隔5.0的 padding

```dart
  // 返回一个居中带图标和文本的Item
  _getBottomItem(IconData icon, String text) {
    // 充满 Row 横向的布局
    return new Expanded(
      flex: 1,
      // 居中显示
      child: new Center(
        // 横向布局
        child: new Row(
          // 主轴居中,即是横向居中
          mainAxisAlignment: MainAxisAlignment.center,
          // 大小按照最大充满
          mainAxisSize : MainAxisSize.max,
          // 竖向也居中
          crossAxisAlignment : CrossAxisAlignment.center,
          children: <Widget>[
            // 一个图标，大小16.0，灰色
            new Icon(
              icon,
              size: 16.0,
              color: Colors.grey,
            ),
            // 间隔
            new Padding(padding: new EdgeInsets.only(left:5.0)),
            // 显示文本
            new Text(
              text,
              // 设置字体样式：颜色灰色，字体大小14.0
              style: new TextStyle(color: Colors.grey, fontSize: 14.0),
              // 超过的省略为...显示
              overflow: TextOverflow.ellipsis,
              // 最长一行
              maxLines: 1,
            ),
          ],
        ),
      ),
    );
  }
```

完整代码
* 首先是 Container包含了Card，用于快速简单的实现圆角和阴影。 
* 然后接下来包含了FlatButton实现了点击，通过Padding实现了边距。
* 接着通过Column垂直包含了两个子Widget，一个是Container、一个是Row。
* Row 内使用的就是_getBottomItem方法返回的 Widget 。

```dart
  @override
  Widget build(BuildContext context) {
    return new Container(
      ///卡片包装
      child: new Card(
           ///增加点击效果
          child: new FlatButton(
              onPressed: (){print("点击了哦");},
              child: new Padding(
                padding: new EdgeInsets.only(left: 0.0, top: 10.0, right: 10.0, bottom: 10.0),
                child: new Column(
                  mainAxisSize: MainAxisSize.min,
                  children: <Widget>[
                    ///文本描述
                    new Container(
                        child: new Text(
                          "这是一点描述",
                          style: TextStyle(
                            color: Color(GSYColors.subTextColor),
                            fontSize: 14.0,
                          ),
                          ///最长三行，超过 ... 显示
                          maxLines: 3,
                          overflow: TextOverflow.ellipsis,
                        ),
                        margin: new EdgeInsets.only(top: 6.0, bottom: 2.0),
                        alignment: Alignment.topLeft),
                    new Padding(padding: EdgeInsets.all(10.0)),

                    ///三个平均分配的横向图标文字
                    new Row(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: <Widget>[
                        _getBottomItem(Icons.star, "1000"),
                        _getBottomItem(Icons.link, "1000"),
                        _getBottomItem(Icons.subject, "1000"),
                      ],
                    ),
                  ],
                ),
              ))),
    );
  }

```

### Flutter 页面
>&#160;&#160; Flutter 中除了布局的 Widget，还有交互显示的 Widget 和完整页面呈现的Widget。其中常见的有 MaterialApp、Scaffold、Appbar、Text、Image、FlatButton等。

|类型|作用特点|
|:---:|:-----:| 
|MaterialApp|一般作为APP顶层的主页入口，可配置主题，多语言，路由等|
|Scaffold|一般用户页面的承载Widget，包含appbar、snackbar、drawer等material design的设定。|
|Appbar|一般用于Scaffold的appbar ，内有标题，二级页面返回按键等，当然不止这些，tabbar等也会需要它 。|
|Text|显示文本，几乎都会用到，主要是通过style设置TextStyle来设置字体样式等。|
|RichText|富文本，通过设置TextSpan，可以拼接出富文本场景。|
|TextField|文本输入框 ：new TextField(controller: //文本控制器, obscureText: "hint文本");|
|Image|图片加载: new FadeInImage.assetNetwork( placeholder: "预览图", fit: BoxFit.fitWidth, image: "url");|
|FlatButton|按键点击: new FlatButton(onPressed: () {},child: new Container());|


### Flutter Animation
* 透明度渐变
```dart 
Animation<double> opacity = Tween<double>(
    begin: 0.0,
    end: 1.0,
).animate(
    CurvedAnimation(
        parent: _controller,
        curve: Interval(
        0.0,
        0.1,
        curve: Curves.easeIn,
        ),
    ),
);
```

* 翻转
```dart 
Animation<double> rotate = Tween<double>(
    begin: 0.0,
    end: math.pi * 2,
).animate(
    CurvedAnimation(
    parent: _controller,
    curve: Interval(
        0.0,
        0.2,
        curve: Curves.ease,
        ),
    ),
);
```

* 位移
```dart
Animation<EdgeInsets> movement = EdgeInsetsTween(
    begin: EdgeInsets.only(top: 0.0),
    end: EdgeInsets.only(top: 100.0),
).animate(
    CurvedAnimation(
    parent: _controller,
    curve: Interval(
        0.2,
        0.375,
        curve: Curves.fastOutSlowIn,
        ),
    ),
);
```

* 方形变圆
```dart 
Animation<BorderRadius> radius = BorderRadiusTween(
    begin: BorderRadius.circular(0.0),
    end: BorderRadius.circular(100.0),
).animate(
    CurvedAnimation(
    parent: _controller,
    curve: Interval(
        0.5,
        0.75,
        curve: Curves.ease,
        ),
    ),
);
```

* 颜色渐变
```dart
Animation<Color> color = ColorTween(
    begin: Colors.blue[300],
    end: Colors.blue[900],
).animate(
    CurvedAnimation(
    parent: _controller,
    curve: Interval(
        0.5,
        0.75,
        curve: Curves.linear,
        ),
    ),
);
```

* 高宽渐变
```dart
Animation<double> height = Tween<double>(
    begin: 100.0,
    end: 200.0,
).animate(
    CurvedAnimation(
    parent: _controller,
    curve: Interval(
        0.375,
        0.6,
        curve: Curves.fastOutSlowIn,
        ),
    ),
);

Animation<double> width = Tween<double>(
    begin: 100.0,
    end: 200.0,
).animate(
    CurvedAnimation(
    parent: _controller,
    curve: Interval(
        0.375,
        0.6,
        curve: Curves.fastOutSlowIn,
        ),
    ),
);
```