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

    在 Flutter 中，一切的显示都是 Widget 。**Widget 是一切的基础**，作为响应式的渲染，属于 MVVM 的实现机制。我们可以通过修改数据，再用setState 设置数据，Flutter 会**自动通过绑定的数据更新 Widget** 。 **所以你需要做的就是实现 Widget 界面，并且和数据绑定起来**。  

    Widget 分为 有状态 和 无状态 两种，在 Flutter 中每个页面都是一帧。无状态就是保持在那一帧。而有状态的 Widget 当数据更新时，其实是绘制了新的 Widget，只是 State 实现了跨帧的数据同步保存。

    >   当代码框里输入 stl 的时候，可以自动弹出创建无状态控件的模板选项，而输入 stf 的时，就会弹出创建有状态 Widget 的模板选项。  

        代码格式化的时候，括号内外的逗号都会影响格式化时换行的位置。  



